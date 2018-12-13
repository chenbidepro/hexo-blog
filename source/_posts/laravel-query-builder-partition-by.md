---
title: Laravel Query Builder 复杂查询案例：子查询实现分区查询 partition by
date: 2018-11-02 14:08:05
tags: [laravel,query-builder,partition-by]
categories: [Laravel-tips]
---


## 案例

案例：[Laravel 在文章列表中附带上前10条评论?][1]，在获取文章列表时同时把每个文章的前10条评论一同查询出来。

这是典型分区查询案例，需要根据 `comments` 表中的 `post_id` 字段进行分区，同时根据条件进行排序，把符合条件的前 `N` 条是数据取出来。

在其他数据库(`Oracle`, `SQL Server`，`Vertica`) 包含了 `row_number` `partition by` 这样的函数，能够比较容易的实现。

比如在 `SQL Server` 中:
```sql
SELECT * FROM (
SELECT *, row_number() OVER (partition by post_id ORDER BY created_at desc) rank FROM comments where post_id in (1,2,3,4,5) 
) b where rand < 11;
```
在 mysql 中要复杂一些，我们先来看看上面案例中实现需求的几种解决办法。

## 解决办法

### 方法1：

在 blade 中要显示评论数据的地方 `post->comments()->limit(10)`

> 问题：如果取了 20 条 Post 数据，就会有 20 条取 comments 的 sql 语句，会造成执行的 sql 语句过多。

> 不是非常可取，主要问题会造成 SQL 语句过多，对数据库服务器产生压力，不过这里可以使用缓存来改进，但是不在本文章讨论范围里。

### 方法2：

直接通过 with 把 Post 的所有 comments 数据都取出来，在 blade 中 `post->comments->take(10)`

> 问题：Laravel 会预先把文章所有的评论数据查询出来，如果文章的评论数据非常多，可能会造成内存泄漏。

### 方法3：

```
$posts = Post::paginate(15);

$postIds = $posts->pluck('id')->all();

//找出符合条件的 comments ，同时定义 @post, @rank 变量，这里没有用 all,get 等函数，此时并不会执行 SQL 语句。
$sub = Comment::whereIn('post_id',$postIds)->select(DB::raw('*,@post := NULL ,@rank := 0'))->orderBy('post_id');

//把上面构造的 sql 查询作为子表进行查询，根据 post_id 进行分区的同时 @rank 变量不断+1
$sub2 = DB::table( DB::raw("({$sub->toSql()}) as b") )
            ->mergeBindings($sub->getQuery())
            ->select(DB::raw('b.*,IF (
			@post = b.post_id ,@rank :=@rank + 1 ,@rank := 1
		) AS rank,
		@post := b.post_id'));

//取出符合条件的前10条comment
$commentIds = DB::table( DB::raw("({$sub2->toSql()}) as c") )
            ->mergeBindings($sub2)
        ->where('rank','<',11)->select('c.id')->pluck('id')->toArray();

$comments = Comment::whereIn('id',$commentIds)->get();

$posts = $posts->each(function ($item, $key) use ($comments) {
    $item->comments = $comments->where('post_id',$item->id);
});
```
> 会产生三条sql

```
select * from `posts` limit 15 offset 0;

select `c`.`id` from (select b.*,IF (
@post = b.post_id ,@rank :=@rank + 1 ,@rank := 1
) AS rank,
@post := b.post_id from (select *,@post := NULL ,@rank := 0 from `comments` where `post_id` in ('2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12', '13', '14', '15', '16') order by `post_id` asc) as b) as c where `rank` < '11';

select * from `comments` where `id` in ('180', '589', '590', '3736');
```

## 知识点

1. `toSql()` 方法的作用是为了获取不带有 `binding` 参数的 `SQL`, 也就是说带问号的 `SQL`
2. `getQuery()` 方法的作用是为了获取 `binding `参数并代替 `toSql()` 获得SQL的问号，从而得到完整的SQL
3. `raw()` 的作用是直接把 `SQL` 套进 `Laravel` 的查询构造器中。
4. mysql 查询语句中定义变量 `@post := NULL ,@rank := 0`  以及 `IF` 函数的使用
5. 如何构建子查询。

**为什么不直接用原生 SQL 语句来实现？**

这里之所以坚持使用 Laravel Query Builder 来实现，可以有效防止 `SQL 注入`，并且和 `ORM` 的 `Model` 对象关联起来。



> 如果还有更多类似这种复杂的需求，欢迎联系我 : )