---
title: Laravel 开源电商之数据字典
date: 2018-12-19 12:00:06
tags: [laravel,开源电商,数据字典]
categories: [开源电商] 
---

iBrand 开源电商系统数据库的数据字典，可以通过该文档快速了解表及字段的相关意义和说明，有助于快速了解电商系统的数据库设计。



## 商品类相关表

### 商品表（SPU)：ibrand_goods

| 字段             | 类型                  | 空   | 索引 | 默认值 | 注释                                                    |
| ---------------- | --------------------- | ---- | ---- | ------ | ------------------------------------------------------- |
| id               | INT(10)自增长、无符号 | 否   | 主键 |        |                                                         |
| name             | VARCHAR(191)          |      |      |        | 商品名称                                                |
| goods_no         | VARCHAR(191)          |      |      |        | 商品货号                                                |
| brand_id         | INT(10)无符号         |      |      |        | 品牌ID                                                  |
| model_id         | INT(10)无符号         |      |      |        | 模型ID                                                  |
| min_price        | DECIMAL(15,2)         |      |      |        | 销售价格                                                |
| max_price        | DECIMAL(15,2)         |      |      |        | 销售价格                                                |
| sell_price       | DECIMAL(15,2)         |      |      |        | 销售价格                                                |
| market_price     | DECIMAL(15,2)         | 是   |      | NULL   | 市场价                                                  |
| min_market_price | DECIMAL(15,2)         | 是   |      | NULL   | 市场价                                                  |
| cost_price       | DECIMAL(15,2)         | 是   |      | NULL   | 成本价                                                  |
| weight           | DECIMAL(15,2)         | 是   |      | NULL   | 重量                                                    |
| store_nums       | INT(11)               |      |      | 0      | 库存                                                    |
| img              | VARCHAR(191)          | 是   |      | NULL   | 封面图                                                  |
| content          | MEDIUMTEXT            | 是   |      |        | 商品描述(mobile)                                        |
| contentpc        | MEDIUMTEXT            | 是   |      |        | 商品描述(pc)                                            |
| sync             | TINYINT(4)            |      |      | 0      | 内容是否同步 0：不同步 1：同步至PC端  2：PC同步到移动端 |
| comment_count    | INT(11)               |      |      | 0      | 评论次数                                                |
| visit_count      | INT(11)               |      |      | 0      | 浏览次数                                                |
| favorite_count   | INT(11)               |      |      | 0      | 收藏次数                                                |
| sale_count       | INT(11)               |      |      | 0      | 销量                                                    |
| grade            | INT(11)               |      |      | 0      | 评分总数                                                |
| tags             | VARCHAR(191)          | 是   |      | NULL   | 标签                                                    |
| keywords         | VARCHAR(191)          | 是   |      | NULL   | SEO关键词                                               |
| description      | VARCHAR(191)          | 是   |      | NULL   | SEO描述                                                 |
| is_del           | TINYINT(4)            |      |      | 0      | 上架或者下架                                            |
| is_largess       | TINYINT(4)            |      |      | 0      | 是否赠品：0否 1是                                       |
| is_commend       | TINYINT(4)            |      |      | 0      | 是否推荐                                                |
| is_new           | TINYINT(4)            |      |      | 0      | 是否新品                                                |
| extra            | TEXT                  | 是   |      |        |                                                         |
| created_at       | TIMESTAMP             | 是   |      | NULL   |                                                         |
| updated_at       | TIMESTAMP             | 是   |      | NULL   |                                                         |
| deleted_at       | TIMESTAMP             | 是   |      | NULL   |                                                         |

### 属性基础表：ibrand_goods_attribute

| 字段       | 类型                  | 空   | 索引 | 默认值 | 注释              |
| ---------- | --------------------- | ---- | ---- | ------ | ----------------- |
| id         | INT(10)自增长、无符号 | 否   | 主键 |        |                   |
| type       | TINYINT(4)            |      |      | 1      | 下拉列表 2 输入框 |
| name       | VARCHAR(191)          |      |      | NULL   | 属性名称          |
| is_search  | TINYINT(4)            |      |      | 0      |                   |
| is_chart   | TINYINT(4)            |      |      | 0      |                   |
| created_at | TIMESTAMP             | 是   |      | NULL   |                   |
| updated_at | TIMESTAMP             | 是   |      | NULL   |                   |
| deleted_at | TIMESTAMP             | 是   |      | NULL   |                   |

### 商品和属性值关系表：ibrand_goods_attribute_relation

| 字段               | 类型                  | 空   | 索引 | 默认值 | 注释   |
| ------------------ | --------------------- | ---- | ---- | ------ | ------ |
| id                 | INT(10)自增长、无符号 | 否   | 主键 |        |        |
| goods_id           | INT(10)无符号         |      |      |        | 商品id |
| model_id           | INT(10)无符号         |      |      |        | 模型id |
| attribute_id       | INT(10)无符号         |      |      |        |        |
| attribute_value_id | INT(10)无符号         |      |      |        |        |
| attribute_value    | VARCHAR(191)          | 是   |      | NULL   |        |
| created_at         | TIMESTAMP             | 是   |      | NULL   |        |
| updated_at         | TIMESTAMP             | 是   |      | NULL   |        |
| deleted_at         | TIMESTAMP             | 是   |      | NULL   |        |

### 属性值表：ibrand_goods_attribute_value

| 字段         | 类型                  | 空   | 索引 | 默认值 | 注释 |
| ------------ | --------------------- | ---- | ---- | ------ | ---- |
| id           | INT(10)自增长、无符号 | 否   | 主键 |        |      |
| attribute_id | INT(10)无符号         |      |      |        |      |
| name         | VARCHAR(191)          |      |      |        |      |
| created_at   | TIMESTAMP             | 是   |      | NULL   |      |
| updated_at   | TIMESTAMP             | 是   |      | NULL   |      |
| deleted_at   | TIMESTAMP             | 是   |      | NULL   |      |

### 品牌表：ibrand_goods_brand

| 字段        | 类型                  | 空   | 索引 | 默认值 | 注释     |
| ----------- | --------------------- | ---- | ---- | ------ | -------- |
| id          | INT(10)自增长、无符号 | 否   | 主键 |        |          |
| name        | VARCHAR(191)          |      |      |        | 品牌名称 |
| description | VARCHAR(191)          | 是   |      | NULL   | 描述     |
| is_show     | INT(11)               |      |      | 1      | 是否显示 |
| sort        | INT(11)               |      |      | 99     | 排序     |
| url         | VARCHAR(191)          | 是   |      | NULL   | 链接     |
| logo        | VARCHAR(191)          | 是   |      | NULL   | 图标     |
| created_at  | TIMESTAMP             | 是   |      | NULL   |          |
| updated_at  | TIMESTAMP             | 是   |      | NULL   |          |
| deleted_at  | TIMESTAMP             | 是   |      | NULL   |          |

### 商品分类表：ibrand_goods_category

| 字段        | 类型                  | 空   | 索引 | 默认值 | 注释   |
| ----------- | --------------------- | ---- | ---- | ------ | ------ |
| id          | INT(10)自增长、无符号 | 否   | 主键 |        |        |
| goods_id    | INT(10)无符号         |      |      |        | 商品id |
| category_id | INT(10)无符号         |      |      |        | 分类id |
| created_at  | TIMESTAMP             | 是   |      | NULL   |        |
| updated_at  | TIMESTAMP             | 是   |      | NULL   |        |

### 商品模型表：ibrand_goods_model

| 字段       | 类型                  | 空   | 索引 | 默认值 | 注释 |
| ---------- | --------------------- | ---- | ---- | ------ | ---- |
| id         | INT(10)自增长、无符号 | 否   | 主键 |        |      |
| name       | VARCHAR(191)          |      |      |        |      |
| spec_ids   | VARCHAR(191)          |      |      |        |      |
| created_at | TIMESTAMP             | 是   |      | NULL   |      |
| updated_at | TIMESTAMP             | 是   |      | NULL   |      |
| deleted_at | TIMESTAMP             | 是   |      | NULL   |      |

### 模型和属性关联表：ibrand_goods_model_attribute_relation

| 字段         | 类型                  | 空   | 索引 | 默认值 | 注释 |
| ------------ | --------------------- | ---- | ---- | ------ | ---- |
| id           | INT(10)自增长、无符号 | 否   | 主键 |        |      |
| model_id     | INT(10)无符号         |      |      |        |      |
| attribute_id | INT(10)无符号         |      |      |        |      |
| created_at   | TIMESTAMP             | 是   |      | NULL   |      |
| updated_at   | TIMESTAMP             | 是   |      | NULL   |      |
| deleted_at   | TIMESTAMP             | 是   |      | NULL   |      |

### 商品图片表：ibrand_goods_photo

| 字段       | 类型                  | 空   | 索引 | 默认值 | 注释       |
| ---------- | --------------------- | ---- | ---- | ------ | ---------- |
| id         | INT(10)自增长、无符号 | 否   | 主键 |        |            |
| goods_id   | INT(10)无符号         |      |      |        | 商品id     |
| url        | VARCHAR(191)          |      |      |        | 链接       |
| sort       | INT(11)               |      |      | 0      | 排序       |
| code       | VARCHAR(191)          | 是   |      | NULL   | 编码       |
| is_default | TINYINT(4)            |      |      | 1      | 是否为默认 |
| created_at | TIMESTAMP             | 是   |      | NULL   |            |
| updated_at | TIMESTAMP             | 是   |      | NULL   |            |
| deleted_at | TIMESTAMP             | 是   |      | NULL   |            |

### 商品SKU：ibrand_goods_product

| 字段         | 类型                  | 空   | 索引 | 默认值 | 注释     |
| ------------ | --------------------- | ---- | ---- | ------ | -------- |
| id           | INT(10)自增长、无符号 | 否   | 主键 |        |          |
| goods_id     | INT(10)无符号         |      |      |        |          |
| store_nums   | INT(11)               |      |      | 0      | 库存     |
| sku          | VARCHAR(191)          | 是   |      | NULL   |          |
| sell_price   | DECIMAL(15,2)         |      |      |        | 销售价   |
| market_price | DECIMAL(15,2)         | 是   |      | NULL   | 市场价   |
| cost_price   | DECIMAL(15,2)         | 是   |      | NULL   | 成本价   |
| weight       | DECIMAL(15,2)         | 是   |      | NULL   | 权重     |
| is_show      | TINYINT(4)            |      |      | 1      | 是否显示 |
| spec_ids     | TEXT                  | 是   |      |        |          |
| created_at   | TIMESTAMP             | 是   |      | NULL   |          |
| updated_at   | TIMESTAMP             | 是   |      | NULL   |          |
| deleted_at   | TIMESTAMP             | 是   |      | NULL   |          |

### 商品规格：ibrand_goods_spec

| 字段         | 类型                  | 空   | 索引 | 默认值 | 注释     |
| ------------ | --------------------- | ---- | ---- | ------ | -------- |
| id           | INT(10)自增长、无符号 | 否   | 主键 |        |          |
| name         | VARCHAR(191)          |      |      |        | 属性名称 |
| display_name | VARCHAR(191)          |      |      |        | 展示名称 |
| type         | TINYINT(4)            |      |      |        | 类型     |
| created_at   | TIMESTAMP             | 是   |      | NULL   |          |
| updated_at   | TIMESTAMP             | 是   |      | NULL   |          |
| deleted_at   | TIMESTAMP             | 是   |      | NULL   |          |

### 商品和规格关联表：ibrand_goods_spec_relation

| 字段          | 类型                  | 空   | 索引 | 默认值 | 注释   |
| ------------- | --------------------- | ---- | ---- | ------ | ------ |
| id            | INT(10)自增长、无符号 | 否   | 主键 |        |        |
| goods_id      | INT(10)无符号         |      |      |        | 商品id |
| spec_id       | INT(10)无符号         |      |      |        |        |
| spec_value_id | INT(10)无符号         |      |      |        |        |
| alias         | VARCHAR(191)          | 是   |      | NULL   |        |
| img           | VARCHAR(191)          | 是   |      | NULL   |        |
| created_at    | TIMESTAMP             | 是   |      | NULL   |        |
| updated_at    | TIMESTAMP             | 是   |      | NULL   |        |
| deleted_at    | TIMESTAMP             | 是   |      | NULL   |        |

### 规格值：ibrand_goods_spec_value

| 字段       | 类型                  | 空   | 索引 | 默认值 | 注释 |
| ---------- | --------------------- | ---- | ---- | ------ | ---- |
| id         | INT(10)自增长、无符号 | 否   | 主键 |        |      |
| spec_id    | INT(10)无符号         |      |      |        |      |
| name       | VARCHAR(191)          |      |      |        |      |
| created_at | TIMESTAMP             | 是   |      | NULL   |      |
| updated_at | TIMESTAMP             | 是   |      | NULL   |      |
| deleted_at | TIMESTAMP             | 是   |      | NULL   |      |
| rgb        | VARCHAR(255)          | 是   |      | NULL   |      |

------

## 用户相关表

### 用户表：ibrand_user

| 字段           | 类型                  | 空   | 索引 | 默认值 | 注释                 |
| -------------- | --------------------- | ---- | ---- | ------ | -------------------- |
| id             | INT(10)自增长、无符号 | 否   | 主键 |        |                      |
| name           | VARCHAR(191)          | 是   | 唯一 | NULL   | 姓名                 |
| nick_name      | VARCHAR(191)          | 是   |      | NULL   | 昵称                 |
| email          | VARCHAR(191)          | 是   | 唯一 | NULL   | 邮箱                 |
| mobile         | VARCHAR(191)          | 是   | 唯一 | NULL   | 手机号               |
| password       | VARCHAR(191)          | 是   |      | NULL   | 密码                 |
| status         | TINYINT(4)            |      |      | 1      | 状态 1：启用 0：禁用 |
| sex            | VARCHAR(191)          | 是   |      | NULL   | 性别                 |
| avatar         | VARCHAR(191)          | 是   |      | NULL   | 头像                 |
| city           | VARCHAR(191)          | 是   |      | NULL   | 城市                 |
| birthday       | VARCHAR(191)          | 是   |      | NULL   | 生日                 |
| remember_token | VARCHAR(100)          | 是   |      | NULL   |                      |
| created_at     | TIMESTAMP             | 是   |      | NULL   |                      |
| updated_at     | TIMESTAMP             | 是   |      | NULL   |                      |

### 用户第三方平台信息表：ibrand_user_bind 

| 字段       | 类型                  | 空   | 索引  | 默认值 | 注释               |
| ---------- | --------------------- | ---- | ----- | ------ | ------------------ |
| id         | INT(10)自增长、无符号 | 否   | 主键  |        |                    |
| user_id    | INT(10)无符号         |      | index | NULL   | 用户id             |
| type       | VARCHAR(191)          |      |       | wechat | 类型               |
| app_id     | VARCHAR(191)          |      |       |        | 绑定公众号id       |
| open_id    | VARCHAR(191)          |      | 唯一  |        | 用户公众号唯一标识 |
| nick_name  | VARCHAR(191)          | 是   |       | NULL   | 昵称               |
| sex        | VARCHAR(191)          | 是   |       | NULL   | 性别               |
| email      | VARCHAR(191)          | 是   |       | NULL   | 邮箱               |
| avatar     | VARCHAR(191)          | 是   |       | NULL   | 头像               |
| city       | VARCHAR(191)          | 是   |       | NULL   | 城市               |
| province   | VARCHAR(191)          | 是   |       | NULL   | 省份               |
| country    | VARCHAR(191)          | 是   |       | NULL   | 国家               |
| language   | VARCHAR(191)          | 是   |       | NULL   | 语言               |
| created_at | TIMESTAMP             | 是   |       | NULL   |                    |
| updated_at | TIMESTAMP             | 是   |       | NULL   |                    |

------

###  收货地址：ibrand_addresses 

| 字段         | 类型                   | 空   | 索引 | 默认值 | 注释                       |
| ------------ | ---------------------- | ---- | ---- | ------ | -------------------------- |
| id           | INT(10) 自增长、无符号 | 否   | 主键 |        |                            |
| user_id      | INT(11)                | 否   |      |        | 用户id                     |
| accept_name  | VARCHAR(255)           | 否   |      |        |                            |
| mobile       | VARCHAR(255)           | 否   |      |        | 手机号                     |
| province     | INT(11)                | 否   |      |        | 省份id                     |
| city         | INT(11)                | 否   |      |        | 城市id                     |
| area         | INT(11)                | 否   |      |        | 地区id                     |
| address_name | VARCHAR(255)           | 否   |      |        | 详细地址                   |
| address      | VARCHAR(255)           | 否   |      |        | 地址                       |
| is_default   | TINYINT(4)             | 否   |      | 0      | 默认地址 1：默认 0：非默认 |
| created_at   | TIMESTAMP              | 是   |      | NULL   |                            |
| updated_at   | TIMESTAMP              | 是   |      | NULL   |                            |
| deleted_at   | TIMESTAMP              | 是   |      | NULL   |                            |

### 收藏：ibrand_favorite 

| 字段              | 类型                  | 空   | 索引 | 默认值 | 注释                        |
| ----------------- | --------------------- | ---- | ---- | ------ | --------------------------- |
| id                | INT(10)自增长、无符号 | 否   | 主键 |        |                             |
| user_id           | INT(10)无符号         |      |      |        | 用户id                      |
| favoriteable_id   | INT(10)无符号         |      |      |        | 收藏的id                    |
| favoriteable_type | VARCHAR(191)          |      |      |        | 收藏的类型(如：商品 ，故事) |
| created_at        | TIMESTAMP             | 是   |      | NULL   |                             |
| updated_at        | TIMESTAMP             | 是   |      | NULL   |                             |

### 积分表：ibrand_point 

| 字段       | 类型                  | 空   | 索引     | 默认值     | 注释                                |
| ---------- | --------------------- | ---- | -------- | ---------- | ----------------------------------- |
| id         | INT(10)自增长、无符号 | 否   | 主键     |            |                                     |
| user_id    | INT(10)无符号         |      |          |            | 用户id                              |
| action     | VARCHAR(191)          |      |          | order_item | action:产生积分变化的动作，用于查询 |
| note       | VARCHAR(191)          | 是   |          |            | 积分变化的提示信息                  |
| value      | DECIMAL(10, 2)        | 是   |          | 0          | 积分变化数值，可为负数              |
| valid_time | TIMESTAMP             | 是   |          | NULL       | 积分有效期 为空时永久有效           |
| status     | INT(10)无符号         | 是   |          | 1          |                                     |
| item_type  | VARCHAR(191)          | 是   | 组合索引 |            |                                     |
| item_id    | INT(10)无符号         | 是   | 组合索引 |            | 积分变化动作对应表的id              |
| created_at | TIMESTAMP             | 是   |          | NULL       |                                     |
| updated_at | TIMESTAMP             | 是   |          | NULL       |                                     |
| deleted_at | TIMESTAMP             | 是   |          | NULL       |                                     |

------

## 促销活动和优惠券相关表

### 优惠基础表：ibrand_discount

| 字段            | 类型                  | 空   | 索引 | 默认值 | 注释                               |
| --------------- | --------------------- | ---- | ---- | ------ | ---------------------------------- |
| id              | INT(10)自增长、无符号 | 否   | 主键 |        |                                    |
| title           | VARCHAR(191)          |      |      |        | 标题                               |
| code            | VARCHAR(191)          | 是   |      | NULL   | 兑换码                             |
| label           | VARCHAR(191)          |      |      |        | 说明                               |
| intro           | VARCHAR(191)          | 是   |      | NULL   | 简介                               |
| exclusive       | TINYINT(4)            |      |      | 0      |                                    |
| usage_limit     | INT(11)               | 是   |      | NULL   | 发放总量                           |
| per_usage_limit | INT(11)               | 是   |      | 0      | 每人领取                           |
| used            | INT(11)               |      |      | 0      | 已使用                             |
| coupon_based    | TINYINT(4)            |      |      | 0      | 是否为优惠券 1：优惠券 0：促销活动 |
| status          | TINYINT(4)            |      |      | 1      | 状态 1：发布 0：下架               |
| starts_at       | TIMESTAMP             | 是   |      | NULL   | 开始时间                           |
| ends_at         | TIMESTAMP             | 是   |      | NULL   | 结束时间                           |
| usestart_at     | TIMESTAMP             | 是   |      | NULL   | 使用开始时间                       |
| useend_at       | TIMESTAMP             | 是   |      | NULL   | 使用结束时间                       |
| tags            | VARCHAR(191)          | 是   |      | NULL   | 标签                               |
| created_at      | TIMESTAMP             | 是   |      | NULL   |                                    |
| updated_at      | TIMESTAMP             | 是   |      | NULL   |                                    |
| deleted_at      | TIMESTAMP             | 是   |      | NULL   |                                    |

### 优惠动作：ibrand_discount_action

| 字段          | 类型                  | 空   | 索引 | 默认值 | 注释                    |
| ------------- | --------------------- | ---- | ---- | ------ | ----------------------- |
| id            | INT(10)自增长、无符号 | 否   | 主键 |        |                         |
| discount_id   | INT(10)无符号         |      |      |        | 关联ibrand_discount表id |
| type          | VARCHAR(191)          |      |      |        | 动作类型                |
| configuration | TEXT                  | 是   |      |        | 配置                    |

### 优惠卷：ibrand_discount_coupon

| 字段        | 类型                  | 空   | 索引 | 默认值 | 注释                    |
| ----------- | --------------------- | ---- | ---- | ------ | ----------------------- |
| id          | INT(10)自增长、无符号 | 否   | 主键 |        |                         |
| discount_id | INT(10)无符号         |      |      |        | 关联ibrand_discount表id |
| user_id     | INT(10)无符号         |      |      |        | 用户id                  |
| code        | VARCHAR(191)          | 是   |      | NULL   | 核销码                  |
| used_at     | TIMESTAMP             | 是   |      | NULL   | 使用时间                |
| expires_at  | TIMESTAMP             | 是   |      | NULL   | 过期时间                |
| created_at  | TIMESTAMP             | 是   |      | NULL   |                         |
| updated_at  | TIMESTAMP             | 是   |      | NULL   |                         |
| deleted_at  | TIMESTAMP             | 是   |      | NULL   |                         |

### 优惠规则：ibrand_discount_rule 

| 字段          | 类型                  | 空   | 索引  | 默认值 | 注释                    |
| ------------- | --------------------- | ---- | ----- | ------ | ----------------------- |
| id            | INT(10)自增长、无符号 | 否   | 主键  |        |                         |
| discount_id   | INT(10)无符号         | 否   | index |        | 关联ibrand_discount表id |
| type          | VARCHAR(191)          | 否   |       |        | 规则类型                |
| configuration | MEDIUMTEXT            | 是   |       |        | 规则信息                |

------



## 订单相关表

### 订单：ibrand_order 

| 字段                | 类型                  | 空   | 索引 | 默认值 | 注释                                                         |
| ------------------- | --------------------- | ---- | ---- | ------ | ------------------------------------------------------------ |
| id                  | INT(10)自增长、无符号 | 否   | 主键 |        |                                                              |
| user_id             | INT(10)无符号         |      |      |        | 用户id                                                       |
| order_no            | VARCHAR(191)          |      |      |        | 订单编号                                                     |
| status              | INT(10)无符号         |      |      | 0      | 订单状态：1生成订单,2支付订单,3取消订单,4作废订单,5完成订单,6退款 |
| pay_status          | TINYINT(3)无符号      |      |      | 0      | 支付状态：0未支付,1已支付                                    |
| distribution_status | INT(11)无符号         |      |      | 0      | 发货状态：0未发货，1已发货                                   |
| count               | INT(11)无符号         |      |      | 0      | 商品总数                                                     |
| items_total         | INT(11)               |      |      |        | 商品总金额                                                   |
| adjustments_total   | INT(11)               |      |      | 0      | 优惠金额，负数，包含了促销和优惠券以及其他优惠的总金额,默认为零因为可能没有优惠活动 |
| payable_freight     | INT(11)               |      |      | 0      | 应付运费金额                                                 |
| real_freight        | INT(11)               |      |      | 0      | 实付运费金额                                                 |
| total               | INT(11)               |      |      |        | 订单总金额:  items_total+adjustments_total+real_freight      |
| accept_name         | VARCHAR(191)          |      |      | NULL   | 收货人姓名                                                   |
| mobile              | VARCHAR(191)          |      |      | NULL   | 电话号码                                                     |
| address             | VARCHAR(191)          |      |      | NULL   |                                                              |
| address_name        | VARCHAR(191)          |      |      | NULL   | 备用:收货地//详细地址址省市区名称                            |
| submit_time         | TIMESTAMP             |      |      | NULL   | 付款时间                                                     |
| pay_time            | TIMESTAMP             |      |      | NULL   | 付款时间                                                     |
| send_time           | TIMESTAMP             |      |      | NULL   | 发货时间                                                     |
| completion_time     | TIMESTAMP             |      |      | NULL   | 订单完成时间                                                 |
| accept_time         | TIMESTAMP             |      |      | NULL   | 客户收货时间                                                 |
| message             | VARCHAR(191)          |      |      | NULL   | 用户留言                                                     |
| type                | INT(11)               |      |      | 0      | //各种订单类型，拼团订单，秒杀订单，折扣订单等               |
| channel             | VARCHAR(191)          |      |      | ec     | 来源 ec：商城订单；shop：门店订单                            |
| channel_id          | INT(10)无符号         |      |      | 0      | 来源id                                                       |
| cancel_reason       | VARCHAR(191)          | 是   |      | NULL   | 取消原因                                                     |
| note                | VARCHAR(191)          | 是   |      | NULL   | 备注                                                         |
| created_at          | TIMESTAMP             | 是   |      | NULL   |                                                              |
| updated_at          | TIMESTAMP             | 是   |      | NULL   |                                                              |
| deleted_at          | TIMESTAMP             | 是   |      | NULL   |                                                              |
### 订单明细：ibrand_order_item 

| 字段              | 类型                  | 空   | 索引 | 默认值 | 注释                                    |
| ----------------- | --------------------- | ---- | ---- | ------ | --------------------------------------- |
| id                | INT(10)自增长、无符号 | 否   | 主键 |        |                                         |
| order_id          | INT(10)无符号         |      |      |        | 订单id                                  |
| item_id           | INT(10)无符号         |      |      |        | 商品id                                  |
| item_name         | VARCHAR(191)          |      |      |        | 商品名称                                |
| type              | VARCHAR(191)          |      |      |        | 类型                                    |
| item_meta         | TEXT                  | 是   |      |        | 商品数据                                |
| quantity          | INT(10)无符号         |      |      |        | 商品数量                                |
| unit_price        | INT(11)               |      |      |        | 商品单价                                |
| units_total       | INT(10)无符号         |      |      |        | 商品总价                                |
| adjustments_total | INT(11)               | 是   |      | 0      | 总的优惠金额,负数                       |
| total             | INT(11)               |      |      |        | unitPrice * quantity + adjustmentsTotal |
| shipping_id       | INT(11)               |      |      | 0      | 发货ID                                  |
| is_send           | TINYINT(4)            |      |      | 0      | 是否已发货                              |
| created_at        | TIMESTAMP             | 是   |      | NULL   |                                         |
| updated_at        | TIMESTAMP             | 是   |      | NULL   |                                         |
| deleted_at        | TIMESTAMP             | 是   |      | NULL   |                                         |


### 订单优惠明细：ibrand_order_adjustment 

| 字段               | 类型                  | 空   | 索引 | 默认值 | 注释                                       |
| ------------------ | --------------------- | ---- | ---- | ------ | ------------------------------------------ |
| id                 | INT(10)自增长、无符号 | 否   | 主键 |        |                                            |
| order_id           | INT(10)无符号         | 是   |      | NULL   | 订单id                                     |
| order_item_id      | INT(10)无符号         | 是   |      | NULL   | 订单item id                                |
| order_item_unit_id | INT(10)无符号         | 是   |      | NULL   |                                            |
| type               | VARCHAR(191)          |      |      |        | 优惠类型                                   |
| label              | VARCHAR(191)          | 是   |      | NULL   | 说明                                       |
| amount             | INT(11)               |      |      | 0      | 优惠金额                                   |
| origin_type        | VARCHAR(191)          | 是   |      | NULL   | 优惠类型  discount, coupon ,membership,vip |
| origin_id          | INT(11)               |      |      | 0      | 优惠券ID或者discount ID,或者用户组group id |
| created_at         | TIMESTAMP             | 是   |      | NULL   |                                            |
| updated_at         | TIMESTAMP             | 是   |      | NULL   |                                            |
| deleted_at         | TIMESTAMP             | 是   |      | NULL   |                                            |

### 订单评论：ibrand_order_comment 

| 字段          | 类型                  | 空   | 索引 | 默认值 | 注释                         |
| ------------- | --------------------- | ---- | ---- | ------ | ---------------------------- |
| id            | INT(10)自增长、无符号 | 否   | 主键 |        |                              |
| order_id      | INT(10)无符号         |      |      | 0      | 订单id                       |
| order_item_id | INT(10)无符号         |      |      | 0      | 订单item id                  |
| item_id       | INT(10)无符号         |      |      | 0      | 评价的商品，应该使用goods_id |
| item_meta     | TEXT                  | 是   |      |        | 商品数据                     |
| user_id       | INT(10)无符号         |      |      |        | 用户id                       |
| user_meta     | TEXT                  | 是   |      |        | 用户数据                     |
| contents      | TEXT                  | 是   |      |        | 内容                         |
| point         | INT(11)               | 是   |      | 0      | 评价分数                     |
| status        | VARCHAR(191)          | 是   |      | NULL   | 评价状态                     |
| pic_list      | TEXT                  | 是   |      | NULL   | 评价的图片                   |
| recommend     | TINYINT(4)            |      |      | 0      | 是否推荐 1:推荐 0：不推荐    |
| recommend_at  | TIMESTAMP             | 是   |      | NULL   | 推荐时间                     |
| created_at    | TIMESTAMP             | 是   |      | NULL   |                              |
| updated_at    | TIMESTAMP             | 是   |      | NULL   |                              |
| deleted_at    | TIMESTAMP             | 是   |      | NULL   |                              |

------

### 订单支付明细：ibrand_payment 

| 字段       | 类型                  | 空   | 索引 | 默认值 | 注释               |
| ---------- | --------------------- | ---- | ---- | ------ | ------------------ |
| id         | INT(10)自增长、无符号 | 否   | 主键 |        |                    |
| order_id   | INT(10)无符号         |      |      |        | 关联的订单id       |
| channel    | VARCHAR(191)          |      |      |        | 支付渠道           |
| channel_no | VARCHAR(191)          | 是   |      | NULL   | 取单的支付单号     |
| amount     | INT(11)               |      |      |        | 支付金额：单位分   |
| status     | VARCHAR(191)          |      |      |        | 状态               |
| details    | TEXT                  | 是   |      |        | 存储json meta 数据 |
| paid_at    | TIMESTAMP             | 是   |      | NULL   | 支付时间           |
| created_at | TIMESTAMP             | 是   |      | NULL   |                    |
| updated_at | TIMESTAMP             | 是   |      | NULL   |                    |
| deleted_at | TIMESTAMP             | 是   |      | NULL   |                    |


### 发货信息：ibrand_shipping 

| 字段          | 类型                  | 空   | 索引 | 默认值 | 注释         |
| ------------- | --------------------- | ---- | ---- | ------ | ------------ |
| id            | INT(10)自增长、无符号 | 否   | 主键 |        |              |
| method_id     | INT(11)               |      |      |        | 物流id       |
| order_id      | INT(11)               |      |      |        | 订单id       |
| tracking      | VARCHAR(191)          |      |      |        | 跟踪物流信息 |
| delivery_time | TIMESTAMP             | 是   |      | NULL   | 发货时间     |
| created_at    | TIMESTAMP             | 是   |      | NULL   |              |
| updated_at    | TIMESTAMP             | 是   |      | NULL   |              |
| deleted_at    | TIMESTAMP             | 是   |      | NULL   |              |

### 物流信息表：ibrand_shipping_method 

| 字段       | 类型                  | 空   | 索引 | 默认值 | 注释                     |
| ---------- | --------------------- | ---- | ---- | ------ | ------------------------ |
| id         | INT(10)自增长、无符号 | 否   | 主键 |        |                          |
| code       | VARCHAR(191)          |      |      |        | 编码                     |
| name       | VARCHAR(191)          |      |      |        | 物流名称                 |
| url        | VARCHAR(191)          | 是   |      | NULL   | 链接                     |
| is_enabled | TINYINT(1)            |      |      | 1      | 是否启用 1：启用 0：停用 |
| created_at | TIMESTAMP             | 是   |      | NULL   |                          |
| updated_at | TIMESTAMP             | 是   |      | NULL   |                          |
|            |                       |      |      |        |                          |

------

## 其他

### 相册表：ibrand_images 

| 字段        | 类型                  | 空   | 索引 | 默认值 | 注释     |
| ----------- | --------------------- | ---- | ---- | ------ | -------- |
| id          | INT(10)自增长、无符号 | 否   | 主键 |        |          |
| name        | VARCHAR(191)          |      |      |        | 名称     |
| url         | VARCHAR(191)          |      |      |        | 链接     |
| remote_url  | VARCHAR(191)          | 是   |      | NULL   | 远程连接 |
| category_id | INT(11)               |      |      |        | 分类id   |
| created_at  | TIMESTAMP             | 是   |      | NULL   |          |
| updated_at  | TIMESTAMP             | 是   |      | NULL   |          |
| deleted_at  | TIMESTAMP             | 是   |      | NULL   |          |

### 相册分类表：ibrand_images_category

| 字段        | 类型                  | 空   | 索引 | 默认值 | 注释     |
| ----------- | --------------------- | ---- | ---- | ------ | -------- |
| id          | INT(10)自增长、无符号 | 否   | 主键 |        |          |
| parent_id   | INT(11)               |      |      |        | 分类父id |
| name        | VARCHAR(191)          |      |      |        | 分类名称 |
| description | TEXT                  | 是   |      | NULL   | 描述     |
| sort        | INT(11)               |      |      | 99     | 排序     |
| created_at  | TIMESTAMP             | 是   |      | NULL   |          |
| updated_at  | TIMESTAMP             | 是   |      | NULL   |          |
| deleted_at  | TIMESTAMP             | 是   |      | NULL   |          |

### 广告：ibrand_advert 

| 字段       | 类型                   | 空   | 索引 | 默认值 | 注释 |
| ---------- | :--------------------- | ---- | ---- | ------ | ---- |
| id         | INT(10) 自增长、无符号 | 否   | 主键 |        |      |
| code       | VARCHAR(191)           | 否   | 唯一 |        | 编码 |
| name       | VARCHAR(191)           | 否   |      |        | 名称 |
| created_at | TIMESTAMP              | 是   |      | NULL   |      |
| updated_at | TIMESTAMP              | 是   |      | NULL   |      |

### 广告详细：ibrand_advert_item

| 字段       | 类型                  | 空   | 索引 | 默认值 | 注释   |
| ---------- | --------------------- | ---- | ---- | ------ | ------ |
| id         | INT(10)自增长、无符号 | 否   | 主键 |        |        |
| advert_id  | INT(10)无符号         | 否   |      |        |        |
| name       | VARCHAR(191)          | 否   |      |        | 名称   |
| image      | VARCHAR(191)          | 否   |      |        | 广告图 |
| link       | VARCHAR(191)          | 否   |      |        | 链接   |
| sort       | INT(11)               | 否   |      | 99     | 排序   |
| meta       | TEXT                  | 是   |      |        |        |
| created_at | TIMESTAMP             | 是   |      | NULL   |        |
| updated_at | TIMESTAMP             | 是   |      | NULL   |        |


### 省份：province 

| 字段       | 类型                  | 空   | 索引 | 默认值 | 注释     |
| ---------- | --------------------- | ---- | ---- | ------ | -------- |
| id         | INT(10)自增长、无符号 | 否   | 主键 |        |          |
| provinceID | INT(11)               |      |      |        | 省份编号 |
| province   | VARCHAR(20)           |      |      |        | 省份名称 |

### 城市：city 

| 字段     | 类型                  | 空   | 索引 | 默认值 | 注释         |
| -------- | --------------------- | ---- | ---- | ------ | ------------ |
| id       | INT(10)自增长、无符号 | 否   | 主键 |        |              |
| cityID   | INT(11)               |      |      |        | 城市编号     |
| city     | VARCHAR(20)           |      |      |        | 城市名称     |
| fatherID | INT(11)               |      |      |        | 所属省份编号 |

### 区市：area

| 字段     | 类型                  | 空   | 索引 | 默认值 | 注释         |
| -------- | --------------------- | ---- | ---- | ------ | ------------ |
| id       | INT(10)自增长、无符号 | 否   | 主键 |        |              |
| areaID   | INT(11)               |      |      |        | 地区编号     |
| area     | VARCHAR(20)           |      |      |        | 地区名称     |
| fatherID | INT(11)               |      |      |        | 所属城市编号 |

