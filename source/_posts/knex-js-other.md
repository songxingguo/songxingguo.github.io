title: knex.js中文文档-其他
author: songxingguo
tags:
  - knexjs
categories:
  - 技术学习
  - 数据库
date: 2018-07-01 16:22:00
---
### Raw

有时您可能需要在查询中使用原始表达式。原始查询对象可以在任何地方注入，并且使用正确的绑定可以确保正确地转义您的值，从而防止SQL注入攻击。

- #### Raw Parameter Binding

    人们可以参数化SQL给予knex.raw(sql, bindings)。参数可以是位置命名的。也可以选择参数是否应该被视为值或作为sql标识符，例如在'TableName.ColumnName'引用的情况下。

    例：
    ```
    knex('users')
    .select(knex.raw('count(*) as user_count, status'))
    .where(knex.raw(1))
    .orWhere(knex.raw('status <> ?', [1]))
    .groupBy('status')
    ```
    输出：
    ```
    select count(*) as user_count, status from `users` where 1 or status <> 1 group by `status`
    ```
    位置绑定?被解释为值并被??解释为标识符。
    
    例：
    ```
    knex('users').where(knex.raw('?? = ?', ['user.name', 1]))
    ```
    输出：
    ```
    select * from `users` where `user`.`name` = 1
    ```
    命名的绑定例如:name被解释为值并被:name:解释为标识符。只要值不是任何值，就会处理命名绑定undefined。

    例：
    ```
    knex('users')
    .where(knex.raw(':name: = :thisGuy or :name: = :otherGuy or :name: = :undefinedBinding', {
      name: 'users.name',
      thisGuy: 'Bob',
      otherGuy: 'Jay',
      undefinedBinding: undefined
    }))
    ```
    错误：
    ```
    Undefined binding(s) detected when compiling RAW query: `users`.`name` = ? or `users`.`name` = ? or `users`.`name` = :undefinedBinding
    ```
    对于只有单个绑定的简单查询，.raw可以接受所述绑定作为其第二个参数。

    例：
    ```
    knex('users')
    .where(
      knex.raw('LOWER("login") = ?', 'knex')
    )
    .orWhere(
      knex.raw('accesslevel = ?', 1)
    )
    .orWhere(
      knex.raw('updtime = ?', new Date.UTC('01-01-2016'))
    )
    ```
    错误：
    ```
    Date.UTC is not a constructor
    ```
    请注意，由于含糊不清，数组必须作为参数传递到包含数组中。

    例：
    ```
    knex.raw('select * from users where id in (?)', [1, 2, 3]);
    // Error: Expected 3 bindings, saw 1

    knex.raw('select * from users where id in (?)', [[1, 2, 3]])
    ```
    输出：
    ```
    select * from users where id in (1, 2, 3)
    ```
    为了防止更换?一个可以使用转义序列\\?。

    例：
    ```
    knex.select('*').from('users').where('id', '=', 1).whereRaw('?? \\? ?', ['jsonColumn', 'jsonKey'])
    ```
    输出：
    ```
    select * from `users` where `id` = 1 and `jsonColumn` ? 'jsonKey'
    ```
    为了防止替换命名绑定，可以使用转义序列\\:。

    例：
    ```
    knex.select('*').from('users').whereRaw(":property: = '\\:value' OR \\:property: = :value", {
      property: 'name',
      value: 'Bob'
    })
    ```
    输出：
    ```
    select * from `users` where `name` = ':value' OR :property: = 'Bob'
    ```
- #### Raw Expressions

    原始表达式通过使用knex.raw(sql, [bindings])并传递它作为查询链中任何值的值来创建。

    例：
    ```
    knex('users')
    .select(knex.raw('count(*) as user_count, status'))
    .where(knex.raw(1))
    .orWhere(knex.raw('status <> ?', [1]))
    .groupBy('status')
    ```
    输出：
    ```
    select count(*) as user_count, status from `users` where 1 or status <> 1 group by `status`
    ```
- #### Raw Queries

    该knex.raw也可用于构建一个完整的查询并执行它，作为一个标准的查询生成器的查询将被执行。这样做的好处是它使用连接池并为不同的客户端库提供标准接口。

    ```
    knex.raw('select * from users where id = ?', [1]).then(function(resp) { ... });
    ```
    请注意，响应将是任何底层sql库通常会在普通查询中返回的内容，因此您可能需要查看执行查询的基本库的文档，以确定如何处理响应。

- #### Wrapped Queries

  原始查询构建器还附带了一个wrap方法，该方法允许将查询包装在一个值中：

  例：
  ```
  var subcolumn = knex.raw('select avg(salary) from employee where dept_no = e.dept_no')
  .wrap('(', ') avg_sal_dept');

  knex.select('e.lastname', 'e.salary', subcolumn)
  .from('employee as e')
  .whereRaw('dept_no = e.dept_no')
  ```
  输出：
  ```
  select `e`.`lastname`, `e`.`salary`, (select avg(salary) from employee where dept_no = e.dept_no) avg_sal_dept from `employee` as `e` where dept_no = e.dept_no
  ```
  请注意，使用as方法可以更轻松地实现上述示例。

  例：
  ```
  var subcolumn = knex.avg('salary')
  .from('employee')
  .whereRaw('dept_no = e.dept_no')
  .as('avg_sal_dept');

  knex.select('e.lastname', 'e.salary', subcolumn)
  .from('employee as e')
  .whereRaw('dept_no = e.dept_no')
  ```
  输出：
  ```
  select `e`.`lastname`, `e`.`salary`, (select avg(`salary`) from `employee` where dept_no = e.dept_no) as `avg_sal_dept` from `employee` as `e` where dept_no = e.dept_no
  ```