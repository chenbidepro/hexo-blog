---
title: Laravel 单元测试-模拟认证的用户
date: 2018-10-24 14:27:29
tags: [laravel,phpunit]
---

在 Laravel 编写单元测试时经常会遇到需要模拟认证用户的时候，比如新建文章、创建订单等，那么在 Laravel unit test 中如何来实现呢？

## 官方解决方法

Laravel 的官方文档中的测试章节中有提到:

> Of course, one common use of the session is for maintaining state for the authenticated user. The actingAs helper method provides a simple way to authenticate a given user as the current user. For example, we may use a model factory to generate and authenticate a user:

```php
<?php

use App\User;

class ExampleTest extends TestCase
{
    public function testApplication()
    {
        $user = factory(User::class)->create();

        $response = $this->actingAs($user)
                         ->withSession(['foo' => 'bar'])
                         ->get('/');
    }
}
```

其实就是使用 Laravel Testing `Illuminate\Foundation\Testing\Concerns\ImpersonatesUsers` Trait 中的 `actingAs` 和 `be` 方法。

设置以后在后续的测试代码中，我们可以通过 `auth()->user()` 等方法来获取当前认证的用户。

## 伪造认证用户

在官方的示例中有利用 factory 来创建一个真实的用户，但是更多的时候，我们只想用一个伪造的用户来作为认证用户即可，而不是通过 factory 来创建一个真实的用户。

在 tests 目录下新建一个 `User` calss:
```php
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    protected $fillable = [
        'id', 'name', 'email', 'password',
    ];
}
```
必须在 `$fillable` 中添加 `id` attribute . 否则会抛出异常： `Illuminate\Database\Eloquent\MassAssignmentException: id`

接下来伪造一个用户认证用户：
```php
$user = new User([
    'id' => 1,
    'name' => 'ibrand'
]);

 $this->be($user,'api');
```

> 后续会继续写一些单元测试小细节的文章，欢迎关注 : )

## 讨论交流

![iBrand联系我们](https://iyoyo.oss-cn-hangzhou.aliyuncs.com/post/%E4%BA%8C%E7%BB%B4%E7%A0%81.jpg)