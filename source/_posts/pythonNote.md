---
title: pythonNote
tags:
  - python
categories:
  - python
date: 2026-06-27 01:12:06
---

+ 什么是类型推到式
快速生成某种类型的式子： 列表，字典，集合，元组

+ 什么是依赖注入
```python
async def get_categories(
    db: AsyncSession = Depends(get_database),  # 这就是依赖注入！
    skip: int = 0,
    limit: int = 100
):
    # ...


在这行代码背后，FastAPI 帮你做了 3 件事：

声明依赖：db 参数说“我需要一个数据库会话”。

执行依赖：当请求进来时，FastAPI 会自动调用 get_database() 函数，执行里面的 async with AsyncSessionLocal() as session，生成一个可用的数据库会话对象。

注入结果：把这个会话对象赋值给 db 参数，让你在函数体里直接使用（执行 SQL 查询）。

整个过程你不需要在函数体内手动写 session = AsyncSessionLocal()，也不需要手动 try...finally 关闭它（因为 get_database 里面已经用 async with 处理好了）。
````
+ 把Pedantic类型转化为字典 然后 解包**
```python
# 在updata的时候 values(字段=值，字段=值) values参数是逗号隔开的 需要将字典解包
updata(User).where(User.username == username).values(**user_data.model_dump(
  exclude_unset=True, # 没有设置值的不更新
  exclede_none=True # none 的不更新

))
````
+ fastpai中的唯一约束
唯一约束的作用是什么

+ pip install xxx
+ pip uninstall xxx
+ pip show xxx
+ pip index versions xxx 查看所有版本

+ redis 缓存雪崩
我们要将redis不同的数据类型 的过期时间设置成不同的类型，为了避免数据同时过期 造成缓存雪崩
设置数据过期时间原则就是  数据越稳定，缓存越持久
分类、配置 7200；列表： 600； 详情： 1800；验证码：120 -- 数据越稳定，缓存越持久
