title: knex.js 学习笔记
author: songxingguo
tags:
  - knexjs
categories:
  - 技术学习
  - 数据库
date: 2018-07-01 15:38:00
---
- #### 多表查询
  - 内连接
    — .join(table, first, [operator], second)

    连接构建器可用于指定表之间的连接，第一个参数是连接表，后三个参数分别是第一个连接列，连接操作符和第二个连接列。(等同于自然连接）
    
      例：
      ```
      knex('book as b')
        .join('book_borrow as bb', 'b.book_id', 'bb.book_id')
        .select('book_name as bookName', 'borrow_end_date as endDate', knex.raw('DATEDIFF( borrow_end_date,NOW()) as count'))
        .where({user_id: userId})
        .orderBy('borrow_end_date').first()
      ```
      输出：
      ```
      SELECT
          `book_name` AS `bookName`,
          `borrow_end_date` AS `endDate`,
          DATEDIFF(borrow_end_date, NOW()) AS `count`
      FROM
          `book` AS `b`
      INNER JOIN `book_borrow` AS `bb` ON `b`.`book_id` = `bb`.`book_id`
      WHERE
          `user_id` = '1'
      ORDER BY
          `borrow_end_date` ASC
      LIMIT 1
      ```
      <!-- more -->
      
      等同于下面的 **自然连接**
      ```
      SELECT
          book_name bookName,
          borrow_end_date endDate,
          DATEDIFF(borrow_end_date, NOW())
      FROM
          book b,
          book_borrow bb
      WHERE
          b.book_id = bb.book_id
      AND user_id = 1
      ORDER BY
          borrow_end_date
      LIMIT 1
      ```
     
  - 外连接
  - 子查询

---  
- #### 使用自定义函数 
   -.row()
   查询中使用原始表达式
  
  例：
  ```
  knex('book as b')
    .join('book_borrow as bb', 'b.book_id', 'bb.book_id')
    .select('book_name as bookName', 'borrow_end_date as endDate', knex.raw('DATEDIFF( borrow_end_date,NOW()) as count'))
    .where({user_id: userId})
    .orderBy('borrow_end_date').first()
  ```
  输出：
  ```
SELECT
	`book_name` AS `bookName`,
	`borrow_end_date` AS `endDate`,
	DATEDIFF(borrow_end_date, NOW()) AS `count`
FROM
	`book` AS `b`
INNER JOIN `book_borrow` AS `bb` ON `b`.`book_id` = `bb`.`book_id`
WHERE
	`user_id` = '1'
ORDER BY
	`borrow_end_date` ASC
LIMIT 1
  ```