title: knex.js中文文档
author: songxingguo
tags:
  - knexjs
categories:
  - 技术学习
  - 数据库
date: 2018-06-30 08:07:00
---
### knexjs 简介
 &emsp;&emsp;Knex.js是为Postgres，MSSQL，MySQL，MariaDB，SQLite3，Oracle和Amazon Redshift设计的“包含电池”SQL查询构建器，其设计灵活，便于携带并且使用起来非常有趣。它具有传统的节点样式回调以及用于清洁异步流控制的承诺接口，流接口，全功能查询和模式构建器，事务支持（带保存点），连接池 以及不同查询客户和方言之间的标准化响应。[传送门](https://knexjs.org/#Installation-node)
  

---
### 支持

&emsp;&emsp;Knex的主要目标环境是Node.js，您需要安装该knex库，然后安装适当的数据库库：pg适用于PostgreSQL和Amazon Redshift，mysql适用于MySQL或MariaDB，sqlite3适用于SQLite3或mssql适用于MSSQL。

---
### 安装
```
$ npm install knex --save

# Then add one of the following (adding a --save) flag:
$ npm install pg
$ npm install sqlite3
$ npm install mysql
$ npm install mysql2
$ npm install mariasql
$ npm install strong-oracle
$ npm install oracle
$ npm install mssql
```
<!-- more -->

---
### 初始数据库

&emsp;&emsp;该knex模块本身是一个为Knex提供配置对象的函数，它接受一些参数。该client参数是必需的，并确定哪个客户端适配器将与该库一起使用。

mysql数据库初始化

```
var knex = require('knex')({
  client: 'mysql',
  connection: {
    host : '127.0.0.1',
    user : 'your_database_user',
    password : 'your_database_password',
    database : 'myapp_test'
  }
});
```
---
### knex 查询构造器

knex 查询构造器是用于构建和执行标准的SQL查询,例如：select, insert, update, delete.

在API的许多地方，表名或列名等标识符都可以传递给方法。

最常见的只需要简单tableName.columnName，tableName或者columnName，但在许多情况下，还需要传递一个别名，以便稍后在查询中引用该标识符。

 - #### 声明别名
 
   有两种方法可以为标识符声明别名。可以直接 `as aliasName` 为标识符提供前缀，也可以传递对象 `{ aliasName: 'identifierName' }` 。
   
   如果对象具有多个别名 `{ alias1: 'identifier1', alias2: 'identifier2' }` ，则所有别名标识符将扩展为逗号分隔列表。
   
   例：
   ```
   knex({ a: 'table', b: 'table' })
.select({
  aTitle: 'a.title',
  bTitle: 'b.title'
})
.whereRaw('?? = ??', ['a.column_1', 'b.column_2'])
```
  输出：
```
select `a`.`title` as `aTitle`, `b`.`title` as `bTitle` from `table` as `a`, `table` as `b` where `a`.`column_1` = `b`.`column_2`
```
---
 - #### knex 
    -knex(tableName, options={only: boolean}) / knex.[methodName]
 
 查询构建器通过指定要查询的表名或通过直接在knex对象上调用任何方法来启动。这将启动类似jQuery的链，您可以根据需要调用其他查询构建器方法来构造查询，最终调用任何接口方法，将其转换为toString或返回一个promise对象，回调或链式执行查询。

  - ##### timeout
     -.timeout(ms, options={cancel: boolean})
 
    设置查询的超时时间，并且如果超时超时，将会引发TimeoutError。该错误包含有关查询，绑定和设置的超时的信息。对于想要确保执行的复杂查询不会花太长时间。传递选项的可选第二个参数：cancel：if ，如果达到超时则取消查询。*注意：目前只支持MySQL和MariaDB。 
     
     例：
     ```
      knex.select().from('books').timeout(1000)
     ```
     输出：
     ```
      select * from `books`
     ```
     ---
     例：
     ```
    knex.select().from('books').timeout(1000, {cancel: true}) // MySQL and MariaDB only
    ```
    输出：
    ```
    select * from `books`
    ```

   - ##### select 
      -.select([*columns])

    创建一个select查询，为查询提供可选的列数组，如果在构建查询时没有指定，则最终默认为*。select调用的响应将使用从数据库中选择的对象数组来解析。

    例：
    ```
    knex.select('title', 'author', 'year').from('books')
    ```
    输出：
    ```
    select `title`, `author`, `year` from `books`

     ```
     ---
     例：
     ```
    knex.select().table('books')
    ```
    输出：
    ```
    select * from `books`
     ```
  - ##### as 
    -.as(name)

    允许对子查询进行别名，并取出希望命名当前查询的字符串。如果查询不是子查询，它将被忽略。

     例:
     ```
    knex.avg('sum_column1').from(function() {
      this.sum('column1 as sum_column1').from('t1').groupBy('column1').as('t1')
    }).as('ignored_alias')

     ```
     输出：

     ```
    select avg(`sum_column1`) from (select sum(`column1`) as `sum_column1` from `t1` group by `column1`) as `t1`
     ```
  - ##### column 
    — .column(columns)

    专门设置要在选择查询中选择的列，并获取数组，列表或列名称。传递对象将使用给定的键自动对列进行别名。
    
    例：
    ```
    knex.column('title', 'author', 'year').select().from('books')
    ```
    输出：
    ```
    select `title`, `author`, `year` from `books`
    ```
    ---
    例：
    ```
    knex.column(['title', 'author', 'year']).select().from('books')
    ```
    输出：
    ```
    select `title`, `author`, `year` from `books`

    ```
    ---
    例：
    ```
    knex.column('title', {by: 'author'}, 'year').select().from('books')
    ```
    输出：
    ```
    select `title`, `author` as `by`, `year` from `books`
    ```

  - ##### from 
    — .from([tableName], options={only: boolean})

    指定当前查询中使用的表格，如果已经指定了当前表格名称，则替换当前表格名称。这通常用于高级where或union方法中执行的子查询。传递选项的可选第二个参数：only：if ，在放弃继承表数据之前使用ONLY关键字。*注意：目前只支持PostgreSQL。 truetableName

    例：
    ```
    knex.select('*').from('users')
    ```
    输出：
    ```
    select * from `users`
    ```

  - ##### with 
    — .with(alias, function|raw)

    为查询添加“with”子句。“With”子句由PostgreSQL，Oracle，SQLite3和MSSQL支持。

    例:
    ```
    knex.with('with_alias', knex.raw('select * from "books" where "author" = ?', 'Test')).select('*').from('with_alias')
    ```
    输出：
    ```
    with `with_alias` as (select * from "books" where "author" = 'Test') select * from `with_alias`
    ```
    ---
    例:

    ```
    knex.with('with_alias', (qb) => {
      qb.select('*').from('books').where('author', 'Test')
    }).select('*').from('with_alias')
    ```
    输出：
    ```
    with `with_alias` as (select * from `books` where `author` = 'Test') select * from `with_alias`
    ```

  - ##### withSchema 
    — .withSchema([schemaName])

    指定要用作表名前缀的模式。多表查询。
    
    例：
    ```
    knex.withSchema('public').select('*').from('users')
    ```
    输出：
    ```
    select * from `public`.`users`
    ```
---
- #### Where 语句

    有几种方法可以帮助动态地从句。在很多地方，函数可以用来代替值，构造子查询。在大多数地方，现有的knex查询可能用于组成子查询等。请查看每种使用说明方法的几个示例：

    重要提示：undefined为任何where函数提供值的knex将导致knex在SQL编译期间抛出错误。这是为了你和我们的缘故。Knex不知道如何处理where子句中的未定义值，并且通常在提供一个开头时会出现编程错误。该错误将引发包含查询类型和已编译的查询字符串的消息。

  例：

  ```
  knex('accounts')
  .where('login', undefined)
  .select()
  .toSQL()
  ```
  错误：

  ```
  Undefined binding(s) detected when compiling SELECT query: select * from `accounts` where `login` = ?
  ```
 - ##### where 
   — .where(~mixed~)

     - 对象语法

      例：
      ```
      knex('users').where({
        first_name: 'Test',
        last_name:  'User'
      }).select('id')
      ```
      输出：
       ```
       select `id` from `users` where `first_name` = 'Test' and `last_name` = 'User'
       ```
     - 键值对

       例：
       ```
       knex('users').where('id', 1)
       ```
       输出：
       ```
       select * from `users` where `id` = 1
       ```

    - 函数

      例：
      ```
      knex('users')
      .where((builder) =>
        builder.whereIn('id', [1, 11, 15]).whereNotIn('id', [17, 19])
      )
      .andWhere(function() {
        this.where('id', '>', 10)
      })
      ```
      输出：
      ````
      select * from `users` where (`id` in (1, 11, 15) and `id` not in (17, 19)) and (`id` > 10)
      ```
    - 链式调用

      例:
      ```
      knex('users').where(function() {
        this.where('id', 1).orWhere('id', '>', 10)
      }).orWhere({name: 'Tester'})
      ```
      输出：

      ```
      select * from `users` where (`id` = 1 or `id` > 10) or (`name` = 'Tester')
      ```
    - 分隔符

      例：

      ```
      knex('users').where('columnName', 'like', '%rowlikeme%')
      ```
      输出：

      ```
      select * from `users` where `columnName` like '%rowlikeme%'
      ```
   以上查询演示了返回指定列中出现特定模式的所有用户的常见用例。

    例：
    ```
    knex('users').where('votes', '>', 100)
    ```
    输出：
    ```
    select * from `users` where `votes` > 100
    ```
    ---
    例：
    ```
    var subquery = knex('users').where('votes', '>', 100).andWhere('status', 'active').orWhere('name', 'John').select('id');

    knex('accounts').where('id', 'in', subquery)
    ```
    输出：

    ```
    select * from `accounts` where `id` in (select `id` from `users` where `votes` > 100 and `status` = 'active' or `name` = 'John')
    ```
    或者与对象一起自动包装声明并创建一个or (and - and - and)子句

    例：
    ```
    knex('users').where('id', 1).orWhere({votes: 100, user: 'knex'})
    ```
    输出：
    ```
    select * from `users` where `id` = 1 or (`votes` = 100 and `user` = 'knex')
    ```
  - ##### whereNot 

    — .whereNot(~mixed~)

      - 对象

      例：
      ```
      knex('users').whereNot({
        first_name: 'Test',
        last_name:  'User'
      }).select('id')
      ```
      输出：
      ```
      select `id` from `users` where not `first_name` = 'Test' and not `last_name` = 'User'
      ```

      - 键值对

      例：
      ```
      knex('users').whereNot('id', 1)
      ```
      输出：
      ```
      select * from `users` where not `id` = 1
      ```

      - 链式调用

      例：
      ```
      knex('users').whereNot(function() {
        this.where('id', 1).orWhereNot('id', '>', 10)
      }).orWhereNot({name: 'Tester'})
      ```
      输出：
      ```
      select * from `users` where not (`id` = 1 or not `id` > 10) or not `name` = 'Tester'
      ```

      - 分隔符

      例：
      ```
      knex('users').whereNot('votes', '>', 100)
      ```
      输出：
      ```
      select * from `users` where not `votes` > 100
      ```

    CAVEAT：WhereNot不适用于“in”和“between”类型的子查询。您应该使用“not in”和“not betwee”之间。

    例：
    ```
    var subquery = knex('users')
      .whereNot('votes', '>', 100)
      .andWhere('status', 'active')
      .orWhere('name', 'John')
      .select('id');

    knex('accounts').where('id', 'not in', subquery)
    ```
    输出：
    ```
    select * from `accounts` where `id` not in (select `id` from `users` where not `votes` > 100 and `status` = 'active' or `name` = 'John')
    ```
 - ##### whereIn 
   — .whereIn(column|columns, array|callback|builder) / .orWhereIn   
   
   .where（'id'，'in'，obj）的简写，.whereIn和.orWhereIn方法在查询中添加“where in”子句。
   
    例：
    ```
    knex.select('name').from('users')
    .whereIn('id', [1, 2, 3])
    .orWhereIn('id', [4, 5, 6])
    ```
    输出：
    ```
    select `name` from `users` where `id` in (1, 2, 3) or `id` in (4, 5, 6)
    ```
    ---
    例：
    ```
    knex.select('name').from('users')
    .whereIn('account_id', function() {
      this.select('id').from('accounts');
    })
    ```
    输出：
    ```
    select `name` from `users` where `account_id` in (select `id` from `accounts`)
    ```
    ---
    例：
    ```
    var subquery = knex.select('id').from('accounts');

    knex.select('name').from('users')
      .whereIn('account_id', subquery)
    ```
    输出：
    ```
    select `name` from `users` where `account_id` in (select `id` from `accounts`)
    ```
    ---
    例：
    ```
    knex.select('name').from('users')
    .whereIn(['account_id', 'email'], [[3, 'test3@example.com'], [4, 'test4@example.com']])
    ```
    输出：
    ```
    select `name` from `users` where (`account_id`, `email`) in ((3, 'test3@example.com'), (4, 'test4@example.com'))
    ```
    ---
    例：
    ```
    knex.select('name').from('users')
    .whereIn(['account_id', 'email'], knex.select('id', 'email').from('accounts'))
    ```
    输出：
    ```
    select `name` from `users` where (`account_id`, `email`) in (select `id`, `email` from `accounts`)
    ```
  - ##### whereNotIn 
     — .whereNotIn(column, array|callback|builder) / .orWhereNotIn
     
     例：
     ```
      knex('users').whereNotIn('id', [1, 2, 3])
     ```
     输出：
     ```
      select * from `users` where `id` not in (1, 2, 3)
     ```
    ---     
    例：
    ```
    knex('users').where('name', 'like', '%Test%').orWhereNotIn('id', [1, 2, 3])
    ```
    输出：
    ```
    select * from `users` where `name` like '%Test%' or `id` not in (1, 2, 3)
    ```
  - ##### whereNull 
    -.whereNull(column) / .orWhereNull
    
    例：
    ```
     knex('users').whereNull('updated_at')
    ```
    输出：
    ```
    select * from `users` where `updated_at` is null
    ```
  - ##### whereNotNull 
     -.whereNotNull(column) / .orWhereNotNull
     
      例：
      ```
      knex('users').whereNotNull('created_at')
      ```
      输出：
      ```
      select * from `users` where `created_at` is not null
      ```
  - ##### WhereExists 
     -.whereExists(builder | callback) / .orWhereExists
     
    例：
    ```
    knex('users').whereExists(function() {
      this.select('*').from('accounts').whereRaw('users.account_id = accounts.id');
    })
    ```
    输出：
    ```
    select * from `users` where exists (select * from `accounts` where users.account_id = accounts.id)
    ```
    ---
    例：
    ```
  knex('users').whereExists(knex.select('*').from('accounts').whereRaw('users.account_id = accounts.id'))
    ```
    输出：
    ```
  select * from `users` where exists (select * from `accounts` where users.account_id = accounts.id)
    ```
  - ##### whereNotExists 
     — .whereNotExists(builder | callback) / .orWhereNotExists

      例：
      ```
      knex('users').whereNotExists(function() {
        this.select('*').from('accounts').whereRaw('users.account_id = accounts.id');
      })
      ```
      输出：
      ```
      select * from `users` where not exists (select * from `accounts` where users.account_id = accounts.id)
      ```
      ---
      例：
      ```
      knex('users').whereNotExists(knex.select('*').from('accounts').whereRaw('users.account_id = accounts.id'))
      ```
      输出：
      ```
      select * from `users` where not exists (select * from `accounts` where users.account_id = accounts.id)
      ```
  - ##### whereBetween 
    — .whereBetween(column, range) / .orWhereBetween

      例：
      ```
      knex('users').whereBetween('votes', [1, 100])
      ```
      输出：
      ```
      select * from `users` where `votes` between 1 and 100
      ```

  - ##### whereNotBetween 
    — .whereNotBetween(column, range) / .orWhereNotBetween

      例：
      ```
      knex('users').whereNotBetween('votes', [1, 100])
      ```
      输出：
      ```
      select * from `users` where `votes` not between 1 and 100
      ```

  - ##### whereRaw 
     — .whereRaw(query, [bindings])

       .where(knex.raw(query))的简写.

      例：
      ```
      knex('users').whereRaw('id = ?', [1])
      ```
      输出：
      ```
      select * from `users` where id = 1
      Join Me
      ```
---      
- #### Join 语句

 提供了几种帮助构建连接的方法。
 
 - ##### join 
    — .join(table, first, [operator], second)
    
    连接构建器可用于指定表之间的连接，第一个参数是连接表，后三个参数分别是第一个连接列，连接操作符和第二个连接列。

    例：
    ```
    knex('users')
    .join('contacts', 'users.id', '=', 'contacts.user_id')
    .select('users.id', 'contacts.phone')
    ```
    输出：
    ```
    select `users`.`id`, `contacts`.`phone` from `users` inner join `contacts` on `users`.`id` = `contacts`.`user_id`
    ```
    ---
    例：
    ```
    knex('users')
    .join('contacts', 'users.id', 'contacts.user_id')
    .select('users.id', 'contacts.phone')
    ```
    输出：
    ```
    select `users`.`id`, `contacts`.`phone` from `users` inner join `contacts` on `users`.`id` = `contacts`.`user_id` 
    ```
    对于分组连接，指定一个函数作为连接查询的第二个参数，并使用on具有orOn或andOn创建联接所用括号分组。
  
    例：
    ```
    knex.select('*').from('users').join('accounts', function() {
      this.on('accounts.id', '=', 'users.account_id').orOn('accounts.owner_id', '=', 'users.id')
    })
    ```
    输出：
    ```
    select * from `users` inner join `accounts` on `accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`
    ```
    对于加入语句嵌套，指定一个函数作为第一个参数on，orOn或andOn
    
    例：
    ```
    knex.select('*').from('users').join('accounts', function() {
      this.on(function() {
        this.on('accounts.id', '=', 'users.account_id')
        this.orOn('accounts.owner_id', '=', 'users.id')
      })
    })
    ```
    输出：
    ```
    select * from `users` inner join `accounts` on (`accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`)
    ```
    也可以使用对象来表示连接语法。
   
    例：
    ```
    knex.select('*').from('users').join('accounts', {'accounts.id': 'users.account_id'})
    ```
    输出：
    ```
    select * from `users` inner join `accounts` on `accounts`.`id` = `users`.`account_id`
    ```
    如果需要在连接而不是列中使用文字值（字符串，数字或布尔值），请使用knex.raw。
    
    例：
    ```
    knex.select('*').from('users').join('accounts', 'accounts.type', knex.raw('?', ['admin']))
    ```
    输出：
    ```
    select * from `users` inner join `accounts` on `accounts`.`type` = 'admin'
    ```
 - ##### innerJoin 
    -.innerJoin(table, ~mixed~)
    
    例：
    ```
    knex.from('users').innerJoin('accounts', 'users.id', 'accounts.user_id')
    ```
    输出：
    ```
    select * from `users` inner join `accounts` on `users`.`id` = `accounts`.`user_id`
    ```
    ---
    例：
    ```
    knex.table('users').innerJoin('accounts', 'users.id', '=', 'accounts.user_id')
    ```
    输出：
    ```
    select * from `users` inner join `accounts` on `users`.`id` = `accounts`.`user_id`
    ```
    ---
    例：
    ```
    knex('users').innerJoin('accounts', function() {
      this.on('accounts.id', '=', 'users.account_id').orOn('accounts.owner_id', '=', 'users.id')
    })
    ```
    输出：
    ```
    select * from `users` inner join `accounts` on `accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`
    ```
 - ##### leftJoin 
    -.leftJoin(table, ~mixed~)
    
    例：  
    ```
    knex.select('*').from('users').leftJoin('accounts', 'users.id', 'accounts.user_id')
    ```
    输出：
    ```
    select * from `users` left join `accounts` on `users`.`id` = `accounts`.`user_id`
    ```
    ---
    例：
    ```
    knex.select('*').from('users').leftJoin('accounts', function() {
      this.on('accounts.id', '=', 'users.account_id').orOn('accounts.owner_id', '=', 'users.id')
    })
    ```
    输出：
    ```
    select * from `users` left join `accounts` on `accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`
    ```
 - ##### leftOuterJoin 
    -.leftOuterJoin(table, ~mixed~)
    
    例：
    ```    
    knex.select('*').from('users').leftOuterJoin('accounts', 'users.id', 'accounts.user_id')
    ```
    输出：
    ```
    select * from `users` left outer join `accounts` on `users`.`id` = `accounts`.`user_id`
    ```
    ---
    例：
    ```
    knex.select('*').from('users').leftOuterJoin('accounts', function() {
      this.on('accounts.id', '=', 'users.account_id').orOn('accounts.owner_id', '=', 'users.id')
    })
    ```
    输出：
    ```
    select * from `users` left outer join `accounts` on `accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`
    ```
 - ##### rightJoin 
    — .rightJoin(table, ~mixed~)
    
    例：
    ```   
    knex.select('*').from('users').rightJoin('accounts', 'users.id', 'accounts.user_id')
    ```
    输出：
    ```
    select * from `users` right join `accounts` on `users`.`id` = `accounts`.`user_id`
    ```
    ---
    例：
    ```
    knex.select('*').from('users').rightJoin('accounts', function() {
      this.on('accounts.id', '=', 'users.account_id').orOn('accounts.owner_id', '=', 'users.id')
    })
    ```
    输出：
    ```
    select * from `users` right join `accounts` on `accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`
    ```
 - ##### rightOuterJoin 
    — .rightOuterJoin(table, ~mixed~) 
    
    例：
    ```
    knex.select('*').from('users').rightOuterJoin('accounts', 'users.id', 'accounts.user_id')
    ```
    输出：
    ```
    select * from `users` right outer join `accounts` on `users`.`id` = `accounts`.`user_id`
    ```
    ---
    例：
    ```
    knex.select('*').from('users').rightOuterJoin('accounts', function() {
      this.on('accounts.id', '=', 'users.account_id').orOn('accounts.owner_id', '=', 'users.id')
    })
    ```
    输出：
    ```
    select * from `users` right outer join `accounts` on `accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`
    ```
 - ##### fullOuterJoin 
    -.fullOuterJoin(table, ~mixed~)
    
    例：
    ```
    knex.select('*').from('users').fullOuterJoin('accounts', 'users.id', 'accounts.user_id')
    ```
    输出：
    ```
    select * from `users` full outer join `accounts` on `users`.`id` = `accounts`.`user_id`
    ```
    ---
    例：
    ```
    knex.select('*').from('users').fullOuterJoin('accounts', function() {
      this.on('accounts.id', '=', 'users.account_id').orOn('accounts.owner_id', '=', 'users.id')
    })
    ```
    输出：
    ```
    select * from `users` full outer join `accounts` on `accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`
    ```
 - ##### crossJoin 
    -.crossJoin(table, ~mixed~)
    
    交叉连接条件仅在MySQL和SQLite3中受支持。对于连接条件而不是使用innerJoin。

    例：
    ```
    knex.select('*').from('users').crossJoin('accounts')
    ```
    输出：
    ```
    select * from `users` cross join `accounts`
    ```
    ---
    例：
    ```
    knex.select('*').from('users').crossJoin('accounts', 'users.id', 'accounts.user_id')
    ```
    输出：
    ```
    select * from `users` cross join `accounts` on `users`.`id` = `accounts`.`user_id`
    ```
    ---
    例：
    ```
    knex.select('*').from('users').crossJoin('accounts', function() {
      this.on('accounts.id', '=', 'users.account_id').orOn('accounts.owner_id', '=', 'users.id')
    })
    ```
    输出：
    ```
    select * from `users` cross join `accounts` on `accounts`.`id` = `users`.`account_id` or `accounts`.`owner_id` = `users`.`id`
    ```
 - ##### joinRaw 
    -.joinRaw(sql, [bindings])   
    
    例：
    ```
    knex.select('*').from('accounts').joinRaw('natural full join table1').where('id', 1)
    ```
    输出：
    ```
    select * from `accounts` natural full join table1 where `id` = 1
    ```
    ---
    例：
    ```
    knex.select('*').from('accounts').join(knex.raw('natural full join table1')).where('id', 1)
    ```
    输出：
    ```
    select * from `accounts` inner join natural full join table1 where `id` = 1   
    ```
---    
- #### Having 语句
  
  - ##### having 
     -.having(column, operator, value)
     向查询添加having子句。
     
      例：
      ```
      knex('users')
      .groupBy('count')
      .orderBy('name', 'desc')
      .having('count', '>', 100)
      ```
      输出：
      ```
      select * from `users` group by `count` having `count` > 100 order by `name` desc
      ```
  - #### havingIn 
     -.havingIn(column, values)
     向查询添加havingIn子句。
     
      例：
      ```
      knex.select('*').from('users').havingIn('id', [5, 3, 10, 17])
      ```
      输出：
      ```
      select * from `users` having `id` in (5, 3, 10, 17)
      ```
  - ##### havingNotIn
     -.havingNotIn(column, values)
     向查询添加havingNotIn子句。
     
      例：
      ```
      knex.select('*').from('users').havingNotIn('id', [5, 3, 10, 17])
      ```
      输出：
      ```
      select * from `users` having `id` not in (5, 3, 10, 17)
      ```
  - ##### havingNull 
     -.havingNull(column)
     向查询添加havingNull子句。
     
      例：
      ```
      knex.select('*').from('users').havingNull('email')
      ```
      输出：
      ```
      select * from `users` having `email` is null
      ```
  - ##### havingNotNull 
     -.havingNotNull(column)
     向查询添加havingNotNull子句。
      
      例：
      ```
      knex.select('*').from('users').havingNotNull('email')
      ```
      输出：
      ```
      select * from `users` having `email` is not null
      ```
  - ##### havingExists 
     -.havingExists(builder | callback)
     向查询添加一个havingExists子句。
      
      例：
      ```
      knex.select('*').from('users').havingExists(function() {
        this.select('*').from('accounts').whereRaw('users.account_id = accounts.id');
      })
      ```
      输出：
      ```
      select * from `users` having exists (select * from `accounts` where users.account_id = accounts.id)
      ```
  - ##### havingNotExists 
     -.havingNotExists(builder | callback)
     向查询添加havingNotExists子句。
     
      例：
      ```
      knex.select('*').from('users').havingNotExists(function() {
        this.select('*').from('accounts').whereRaw('users.account_id = accounts.id');
      })
      ```
      输出：
      ```
      select * from `users` having not exists (select * from `accounts` where users.account_id = accounts.id)
      ```
  - ##### havingBetween 
     -.havingBetween(column, range)
     向查询添加一个havingBetween子句。
     
      例：
      ```
      knex.select('*').from('users').havingBetween('id', [5, 10])
      ```
      输出：
      ```
      select * from `users` having `id` between 5 and 10
      ```
  - ##### havingNotBetween 
     -.havingNotBetween(column, range)
     向查询添加havingNotBetween子句。
     
      例：
      ```
      knex.select('*').from('users').havingNotBetween('id', [5, 10])
      ```
      输出：
      ```
      select * from `users` having `id` not between 5 and 10
      ```
  - ##### havingRaw 
     -.havingRaw(column, operator, value)
     向查询添加havingRaw子句。
     
      例：
      ```
      knex('users')
      .groupBy('count')
      .orderBy('name', 'desc')
      .havingRaw('count > ?', [100])
      ```
      输出：
      ```
      select * from `users` group by `count` having count > 100 order by `name` desc
      ```
---      
- #### On 语句

  - ##### onIn 
     -.onIn(column, values)
     向查询添加onIn子句。
     
      例：
      ```
      knex.select('*').from('users').join('contacts', function() {
        this.on('users.id', '=', 'contacts.id').onIn('contacts.id', [7, 15, 23, 41])
      })
      ```
      输出：
      ```
      select * from `users` inner join `contacts` on `users`.`id` = `contacts`.`id` and `contacts`.`id` in (7, 15, 23, 41)    
      ```
  - ##### onNotIn 
     -.onNotIn(column, values)
     向查询添加一个onNotIn子句。
     
      例：
      ```
      knex.select('*').from('users').join('contacts', function() {
        this.on('users.id', '=', 'contacts.id').onNotIn('contacts.id', [7, 15, 23, 41])
      })
      ```
      输出：
      ```
      select * from `users` inner join `contacts` on `users`.`id` = `contacts`.`id` and `contacts`.`id` not in (7, 15, 23, 41)
      ```
  - ##### onNull 
      -.onNull(column)     
      向查询添加一个onNull子句。
      
      例：      
      ```
      knex.select('*').from('users').join('contacts', function() {
        this.on('users.id', '=', 'contacts.id').onNull('contacts.email')
      })
      ```
      输出：
      ```
      select * from `users` inner join `contacts` on `users`.`id` = `contacts`.`id` and `contacts`.`email` is null  
      ```
  - ##### onNotNull 
     -.onNotNull(column)
     向查询添加一个onNotNull子句。
      
      例：
      ```
      knex.select('*').from('users').join('contacts', function() {
        this.on('users.id', '=', 'contacts.id').onNotNull('contacts.email')
      })
      ```
      输出：
      ```
      select * from `users` inner join `contacts` on `users`.`id` = `contacts`.`id` and `contacts`.`email` is not null
      ```
  - ##### onExists 
     -.onExists(builder | callback)
     向查询添加一个onExists子句。
      
      例：
      ```     
      knex.select('*').from('users').join('contacts', function() {
        this.on('users.id', '=', 'contacts.id').onExists(function() {
          this.select('*').from('accounts').whereRaw('users.account_id = accounts.id');
        })
      })
      ```
      输出：
      ```
      select * from `users` inner join `contacts` on `users`.`id` = `contacts`.`id` and exists (select * from `accounts` where users.account_id = accounts.id)
      ```
 - ##### onNotExists 
    -.onNotExists(builder | callback)
    向查询添加一个onNotExists子句。
    
    例：
    ```
    knex.select('*').from('users').join('contacts', function() {
      this.on('users.id', '=', 'contacts.id').onNotExists(function() {
        this.select('*').from('accounts').whereRaw('users.account_id = accounts.id');
      })
    })
    ```
    输出：
    ```
    select * from `users` inner join `contacts` on `users`.`id` = `contacts`.`id` and not exists (select * from `accounts` where users.account_id = accounts.id)
    ```
 - ##### onBetween 
    -.onBetween(column, range)
    向查询添加一个onBetween子句。
    
     例：
     ```
     knex.select('*').from('users').join('contacts', function() {
      this.on('users.id', '=', 'contacts.id').onBetween('contacts.id', [5, 30])
    })
    ```
    输出：
    ```
    select * from `users` inner join `contacts` on `users`.`id` = `contacts`.`id` and `contacts`.`id` between 5 and 30
    ```
 - ##### onNotBetween 
    -.onNotBetween(column, range)
    向查询添加一个onNotBetween子句。
    
      例：
      ```
      knex.select('*').from('users').join('contacts', function() {
        this.on('users.id', '=', 'contacts.id').onNotBetween('contacts.id', [5, 30])
      })
      ```
      输出：
      ```
      select * from `users` inner join `contacts` on `users`.`id` = `contacts`.`id` and `contacts`.`id` not between 5 and 30
      ```
---
- #### Clear 语句

  - ##### clearSelect 
    -.clearSelect()
    清除查询中的所有select子句，不包括子查询。

      例：
      ```     
      knex.select('email', 'name').from('users').clearSelect()
      ```
      输出：
      ```
      select * from `users`
      ```
  - #####  clearWhere 
     -.clearWhere()
     清除查询中的所有where子句，不包括子查询。
     
      例：
      ```
      knex.select('email', 'name').from('users').where('id', 1).clearWhere()
      ```
      输出：
      ```
      select `email`, `name` from `users`
      ```
  - ##### clearOrder 
     -.clearOrder()
     清除查询中的所有订单子句，不包括子查询。
      
      例：
      ```
      knex.select().from('users').orderBy('name', 'desc').clearOrder()
      ```
      输出：
      ```
      select * from `users`
      ```
  - ##### distinct 
     -.distinct()
     去除重复的记录。
     
      例：
      ```
      // select distinct 'first_name' from customers
      knex('customers')
        .distinct('first_name', 'last_name')
        .select()
      ```
      输出：
      ```
      select distinct `first_name`, `last_name` from `customers`
      ```
  - ##### groupBy 
     -.groupBy(*names)
     向查询添加一个group by子句。
        例：
        ```
        knex('users').groupBy('count')
        ```
        输出：
        ```
        select * from `users` group by `count`
        ```
  - ##### groupByRaw 
     -.groupByRaw(sql)
     向查询添加一个原始的group by子句。
     
      例：    
      ```
      knex.select('year', knex.raw('SUM(profit)')).from('sales').groupByRaw('year WITH ROLLUP')
      ```
      输出：
      ```
      select `year`, SUM(profit) from `sales` group by year WITH ROLLUP
      ```
  - ##### orderBy 
     -.orderBy(column, [direction])
     向查询添加一个order by子句。
     
      例：
      ```
      knex('users').orderBy('name', 'desc')
      ```
      输出：
      ```
      select * from `users` order by `name` desc
      ```
  - ##### orderByRaw 
     -.orderByRaw(sql)
    通过raw子句向查询添加一个订单。
    
    例：
    ```
    knex.select('*').from('table').orderByRaw('col DESC NULLS LAST')
    ```
    输出：
    ```
    select * from `table` order by col DESC NULLS LAST
    ```