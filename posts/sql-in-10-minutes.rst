   .. title: SQL in 10 Minutes 总结
   .. slug: sql-in-10-minutes
   .. date: 2017-11-09 20:53:29 UTC+08:00
   .. tags: sql,
   .. category: programming
   .. link:
   .. description:
   .. type: text


.. contents::

Introduction
============

This book was born out of necessity.

I had been teaching Web application development for several years, and students were constantly asking for SQL book recommendations. There are lots of SQL books out there. Some are actually very good. But they all have one thing in common: for most users they teach just too much information. Instead of teaching SQL itself, most books teach everything from database design and normalization to relational database theory and administrative concerns. And while those are all important topics, they are not of interest to most of us who just need to learn SQL. And so, not finding a single book that I felt comfortable recommending, I turned that classroom experience into the book you are holding. Sams Teach Yourself SQL in 10 Minutes will teach you SQL you need to know, starting with simple data retrieval and working on to more complex topics including the use of joins, subqueries, stored procedures, cursors, ??triggers, and table constraints. You’ll learn methodically, systematically, and simply—in lessons that will each take 10 minutes or less to complete. Now in its fourth edition, this book has taught SQL to over a quarter million English speaking users, and has been translated into over a dozen other languages too so as to help users the globe over. And now it is your turn. So turn to Lesson 1, and get to work. You’ll be writing world class SQL in no time at all.

DBMSs Covered in This Book
--------------------------

For the most part, the SQL taught in this book will apply to any Database Management System (DBMS). However, as all SQL implementations are not created equal,

the following DBMSs are explicitly covered (and specific instructions or notes are included where needed): • Apache Open Office Base • IBM DB2 • Microsoft Access • Microsoft SQL Server (including Microsoft SQL Server Express) • MariaDB • MySQL • Oracle (including Oracle Express) • PostgreSQL • SQLite Example databases (or SQL scripts to create the example databases) are available for all of these DBMSs on the book webpage at http://forta.com/books/0672336073/ Errors: https://forta.com/books/0672336073/errata/

1 Understanding SQL
===================

总结 database, table, column, row, primary key

Databases
---------

The term database is used in many different ways, but for our purposes (and indeed, from SQL’s perspective) a database is a collection of data stored in some organized fashion. The simplest way to think of it is to imagine a database as a filing cabinet. The filing cabinet is simply a physical location to store data, regardless of what that data is or how it is organized.

   Database

   A container (usually a file or set of files) to store organized data.

   Caution: Misuse Causes Confusion

   People often use the term database to refer to the database software they are running. This is incorrect, and it is a source of much confusion. Database software is actually called the Database Management System (or DBMS). The database is the container created and manipulated via the DBMS, and exactly what the database is and what form it takes varies from one database to the next.

Tables
------

When you store information in your filing cabinet, you don’t just {phase:toss it in a drawer}. Rather, you create files within the filing cabinet, and then you file related data in specific files.

In the database world, that file is called a table. A table is a structured file that can store data of a specific type. A table might contain a list of customers, a product catalog, or any other list of information.

   Table is a file A structured list of data of a specific type.

The key here is that the data stored in the table is one type of data or one list. You would never store a list of customers and a list of orders in the same database table. Doing so would make subsequent retrieval and access difficult. Rather, you’d create two tables, one for each list.

Every table in a database has a name that identifies it. That name is always unique— meaning no other table in that database can have the same name.

   Note: Table Names

   What makes a table name unique is actually a combination of several things including the database name and table name. Some databases also use the name of the database owner as part of the unique name. This means that while you cannot use the same table name twice in the same database, you definitely can reuse table names in different databases.

Tables have characteristics and properties that define how data is stored in them. These include information about what data may be stored, how it is broken up, how individual pieces of information are named, and much more. This set of information that describes a table is known as a schema, and schemas are used to describe specific tables within a database, as well as entire databases (and the relationship between tables in them, if any).

   Schema

   Information about database and table layout and properties.

Columns and Datatypes Field
---------------------------

Tables are made up of columns. A column contains a particular piece of information within a table.

Column A single field in a table. All tables are made up of one or more columns.

The best way to understand this is to envision database tables as grids, somewhat like spreadsheets. Each column in the grid contains a particular piece of information. In a customer table, for example, one column contains the customer number, another contains the customer name, and the address, city, state, and ZIP code are all stored in their own columns.

Tip: Breaking Up Data It is extremely important to break data into multiple columns correctly. For example, city, state, and ZIP code should always be separate columns. By breaking these out, it becomes possible to sort or filter data by specific columns (for example, to find all customers in a particular state or in a particular city). If city and state are combined into one column, it would be extremely difficult to sort or filter by state.

When you break up data, the level of granularity is up to you and your specific requirements. For example, addresses are typically stored with the house number and street name together. This is fine, unless you might one day need to sort data by street name, in which case splitting house number and street name would be preferable.

Each column in a database has an associated datatype. A datatype defines what type of data the column can contain. For example, if the column were to contain a number(perhaps the number of items in an order), the datatype would be a numeric datatype. If the column were to contain dates, text, notes, currency amounts, and so on, the appropriate datatype would be used to specify this.

Datatype

A type of allowed data. Every table column has an associated datatype that restricts (or allows) specific data in that column.

Datatypes restrict the type of data that can be stored in a column (for example, preventing the entry of alphabetical characters into a numeric field). Datatypes also help sort data correctly and play an important role in optimizing disk usage. As such, special attention must be given to picking the right datatype when tables are created.

Caution: Datatype Compatibility

Datatypes and their names are one of the primary sources of SQL incompatibility. While most basic datatypes are supported consistently, many more advanced datatypes are not. And worse, occasionally you’ll find that the same datatype is referred to by different names in different DBMSs. There is not much you can do about this, but it is important to keep in mind when you create table schemas.

Rows record
-----------

Data in a table is stored in rows; each record saved is stored in its own row. Again, envisioning a table as a spreadsheet style grid, the vertical columns in the grid are the table columns, and the horizontal rows are the table rows.

For example, a customers table might store one customer per row. The number of rows in the table is the number of records in it.

Rows A record in a table. Note: Records or Rows?

You may hear users refer to database records when referring to rows. For the most part the two terms are used interchangeably, but row is technically the correct term.

Primary Keys
------------

Every row in a table should have some column (or set of columns) that uniquely identifies it. A table containing customers might use a customer number column for this purpose, whereas a table containing orders might use the order ID. An employee list table might use an employee ID or the employee Social Security number column.

Primary key

A column (or set of columns) whose values uniquely identify every row in a table.

This column (or set of columns) that uniquely identifies each row in a table is called a primary key. The primary key is used to refer to a specific row. Without a primary key, updating or deleting specific rows in a table becomes extremely difficult as there is no guaranteed safe way to refer to just the rows to be affected.

Tip: Always Define Primary Keys

Although primary keys are not actually required, most database designers ensure that every table they create has a primary key so that future data manipulation is possible and manageable.

Any column in a table can be established as the primary key, as long as it meets the following conditions:

#. No two rows can have the same primary key value.
#. Every row must have a primary key value. (Primary key columns may not allow NULL values.)
#. Values in primary key columns should never be modified or updated.
#. Primary key values should never be reused. (If a row is deleted from the table, its primary key may not be assigned to any new rows in the future.)

Primary keys are usually defined on a single column within a table. But this is not required, and multiple columns may be used together as a primary key. When multiple columns are used, the rules listed above must apply to all columns, and the values of all columns together must be unique (individual columns need not have unique values).

There is another very important type of key called a foreign key, but I’ll get to that later on in Lesson 12, “Joining Tables.”

2 Retrieving Data
=================

总结: select, retrieve individual, multiple, all 概念: distinct limit, offset

Retrieving Individual Column
----------------------------

.. code:: sql

   select prod_name from Products;

Retrieving Multiple Columns
---------------------------

.. code:: sql

   select prod_id, prod_name, prod_price from Products;
   select * from Products;

Retrieving All Columns
----------------------

.. code:: sql

   select * from Products;

Retrieving Distinct Column
--------------------------

.. code:: sql

   select vend_id from Products;

.. code:: sql

   select distinct vend_id from Products;

Limiting Results
----------------

.. code:: sql

   select prod_name from Products  limit 5;

.. code:: sql

   select prod_name from Products limit 5 offset 5;

Using Comments
--------------

.. code:: sql

   SELECT prod_name -- this is a comment FROM Products;

3.Sorting Retrieved Data
========================

In this lesson, you will learn how to use the SELECT statement's ORDER BY clause to sort retrieved data as needed.

关键词 ``order by``, 需要放置到最后 individual columm, multiple columns, column postion, sort direction 接在retrieve之后.

Sorting Data
------------

.. code:: sql

   select prod_name from Products order by prod_name;

Sorting by Multiple Columns
---------------------------

.. code:: sql

   select prod_id, prod_price, prod_name
   from Products
   order by prod_price, prod_name;

Sorting by Column Position
--------------------------

.. code:: sql

   select prod_id, prod_price, prod_name from Products order by 2, 3;

Specifying Sort Direction
-------------------------

.. code:: sql

   select prod_id, prod_price, prod_name
   from Products
   order by prod_price desc, prod_name desc;

4.Filtering Data
================

总结, 开始filter filter, where clause, single value; nonmatches, a range(between), null

Using the WHERE Clause
----------------------

select prod\ :sub:`name`, prod\ :sub:`price` from Products where prod\ :sub:`price` > 3.49

#+end\ :sub:`src`

The WHERE Clause Operators
--------------------------

.. figure:: http://heropublic.oss-cn-beijing.aliyuncs.com/140327.png
   :alt: Screen Shot 2018-08-11 at 10.03.13 PM

   Screen Shot 2018-08-11 at 10.03.13 PM

Checking Against a Single Value
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select prod_name, prod_price
   from Products
   where prod_price < 10;

Checking for Nonmatches
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select vend_id, prod_name from Products where vend_id != "DLL01";

Checking for a Range of Values
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select prod_name, prod_price from Products where prod_price between 3.49 and 11.99;

Checking for No Value
~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select prod_name
   from Products
   where prod_price is null;

.. code:: sql

   select cust_name
   from Customers
   where cust_email is null;

5.Advanced Data Filtering
=========================

总结: 从关键词combine出发. and, or, () which indicate order of evaluation. use in(memberp), not

Combining WHERE Clauses
-----------------------

Using the AND Operator
~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select prod_id, prod_price, prod_name
   from Products
   where vend_id = "DLL01" and prod_price <= 4;

Using the OR Operator
~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select vend_id, prod_name, prod_price
   from Products
   where vend_id="DLL01" or vend_id = "BRS01";

Understanding Order of Evaluation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select vend_id, prod_name, prod_price
   from Products
   where vend_id = "DLL01" or vend_Id = "BRS01" and prod_price >= 10;

.. code:: sql

   select vend_id, prod_name, prod_price
   from Products
   where (vend_id = "DLL01" or vend_id = "BRS01") and prod_price >= 10;

Using the IN Operator
---------------------

.. code:: sql

   select prod_name, prod_price from Products where vend_id in ("dll01","brs01")  order by prod_name;

.. code:: sql

   select prod_name, prod_price from Products where vend_id = "DLL01" or
   vend_id = "BRS01" order by prod_name;

Using the NOT Operator
----------------------

.. code:: sql

   select prod_name, vend_id
   from Products
   where not vend_id = "DLL01"
   order by 1;

6.Using Wildcard Filtering
==========================

总结 从wildcard出发,推导出来regex的必要性. %, \_, brackets 当然最重要的一点是regex, 日后便只用rlike

.. _using-wildcard-filtering-1:

Using Wildcard Filtering
------------------------

The Percent Sign (%) Wildcard
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select prod_id, prod_name from Products where prod_name like  "fish%";

.. code:: sql

   select prod_id, prod_name
   from Products
   where prod_name like "%bean bag%"

.. code:: sql

   select prod_name from Products where prod_name like "f%y";

The Underscore (_) Wildcard
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select prod_name, prod_price
   from Products
   where prod_name like "__ inch teddy bear";

.. code:: sql

   select prod_id, prod_name from Products
   where prod_name like  "%inch teddy bear";

The Brackets ([]) Wildcard
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select cust_contact from Customers where cust_contact rlike
   '[JM].*' order by cust_contact;

.. code:: sql

   select cust_contact from Customers where cust_contact rlike
   "[^JM].*" order by cust_contact;

7.Creating Calculated Fields
============================

最有意思的一点便是用select测试functions and calcuations calculated field, concat, alias, true calcualtion(expanded price) contat便是对字符串预处理. 从calculated fields引出来

Concatenating Fields
--------------------

.. code:: sql

   select concat(vend_name, "(", vend_country, ")")
   from Vendors
   order by vend_name;

Using Aliases
~~~~~~~~~~~~~

.. code:: sql

   select concat(vend_name, " (", vend_country, ")") as vend_title
   from Vendors
   order by vend_name;

Performing Mathematical Calcualtions
------------------------------------

.. code:: sql

   select prod_id,quantity, item_price,
   quantity*item_price as expanded_price
   from OrderItems
   where order_num = 20008;

**Table 7.1. SQL Mathematical Operators**

http://heropublic.oss-cn-beijing.aliyuncs.com/024821

8.Using Data Manipulation Functions
===================================

总结 functions中的4点:

#. text (soundex) ;;最有意思的一点.
#. numeric
#. date and time
#. system infos

Understanding Functions
-----------------------

The Problem with Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. figure:: http://heropublic.oss-cn-beijing.aliyuncs.com/025926.png
   :alt: Screen Shot 2018-08-12 at 10.59.11 AM

   Screen Shot 2018-08-12 at 10.59.11 AM

Using Functions
---------------

Text Manipulation Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select vend_name, upper(vend_name) as vend_name_upcase
   from Vendors
   order by vend_name;

**Table 8.2. Commonly Used Text-Manipulation Functions**

.. figure:: http://heropublic.oss-cn-beijing.aliyuncs.com/034543.png
   :alt: Screen Shot 2018-08-12 at 11.45.20 AM

   Screen Shot 2018-08-12 at 11.45.20 AM

.. code:: sql

   select cust_name, cust_contact from Customers where cust_contact = "Michael Green";

Date and Time Manipulation Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select *
   from Orders
   where year(order_date) = 2012;

Numeric Manipulation Functions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. figure:: http://heropublic.oss-cn-beijing.aliyuncs.com/033344.png
   :alt: Screen Shot 2018-08-12 at 11.33.29 AM

   Screen Shot 2018-08-12 at 11.33.29 AM

9.Summarizing Data
==================

总结:

avg, min, max, sum, count, # 首先讲avg放在前面

与distinct相结合.

Using Aggregate Functions
-------------------------

**Table 9.1. SQL Aggregate Functions**

.. figure:: http://heropublic.oss-cn-beijing.aliyuncs.com/040046.png
   :alt: Screen Shot 2018-08-12 at 12.00.30 PM

   Screen Shot 2018-08-12 at 12.00.30 PM

The AVG() Function
~~~~~~~~~~~~~~~~~~

.. code:: sql

   select avg(prod_price) as avg_price from Products;

.. code:: sql

   sql :engine mysql :dbuser org :database grocer
   select vend_id, avg(prod_price) as avg_price
   from Products
   where vend_id = "DLL01";

The COUNT() Function
~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select count(*) as num_cust
   from Customers;

.. code:: sql

   select count(cust_email) as num_cust
   from Customers;

The MAX() Function
~~~~~~~~~~~~~~~~~~

.. code:: sql

   select max(prod_price) as max_price
   from Products;

The MIN() Function
~~~~~~~~~~~~~~~~~~

.. code:: sql

   select min(prod_price) as min_price
   from Products;

The SUM()
~~~~~~~~~

.. code:: sql

   select sum(quantity) as items_ordered
   from OrderItems
   where order_num = 20005;

.. code:: sql

   select sum(item_price*quantity) as total_price
   from OrderItems
   where order_num = 20005;

Aggregates on Distinct Values
-----------------------------

.. code:: sql

   select avg(distinct prod_price) as avg_price
   from Products
   where vend_id = "DLL01";

Combining Aggregate Functions
-----------------------------

.. code:: sql

   select count(*) as num_items,
   min(prod_price) as min_price,
   max(prod_price) as max_price,
   avg(prod_price) as avg_price
   from Products;

10.Grouping Data
================

总结, group by之后, 可以应用aggregate calculation 针对单一的column, filter, group and sort 棒, 总结当下所学. select, from, where, group by, having, order by

Creating Groups
---------------

.. code:: sql

   select vend_id, count(*) as num_prods
   from Products
   group by vend_id;

Filtering Groups
----------------

.. code:: sql

   select cust_id, count(*) as orders
   from Orders
   group by cust_id
   having count(*) >=2 ;

.. code:: sql

   select vend_id, count(vend_id) as num_prods
   from Products
   where prod_price >= 4
   group by vend_id
   having count(*) >= 2;

.. code:: sql

   select vend_id, count(*) as num_prods from Products group by vend_id having count(*) >=2 ;

Grouping and Sorting
--------------------

.. figure:: http://heropublic.oss-cn-beijing.aliyuncs.com/070716.png
   :alt: Screen Shot 2018-08-12 at 3.07.00 PM

   Screen Shot 2018-08-12 at 3.07.00 PM

.. code:: sql

   select order_num, count(*) as items
   from OrderItems
   group by order_num
   having count(*) >= 3
   order by order_num desc;

.. code:: sql

   select order_num, count(*) as items from OrderItems group by order_num having count(*) >= 3 order by items, order_num;

11.Working with Subqueries
==========================

总结归纳 subqueries, filter by subqueries(用in), calculated fields

Filtering by Subquery
---------------------

.. code:: sql

   select cust_id from Orders
   where order_num in (select order_num from OrderItems where prod_id = "rgan01");

.. code:: sql

   SELECT cust_id from Orders WHERE order_num IN (20007,20008)

.. code:: sql

   select cust_name, cust_contact
   from Customers
   where cust_id in ("1000000004", "1000000005");

.. code:: sql

   select cust_name, cust_contact from Customers
   where cust_id in (select cust_id from Orders where order_num in
   (select order_num from OrderItems where prod_id = "rgan01"));

Using Subqueries as Calculated Fields
-------------------------------------

.. code:: sql

   select cust_id, count(*) as orders from Orders group by cust_id;

.. code:: sql


   select cust_name, cust_state,
   (select count(*) from Orders where Orders.cust_id = Customers.cust_id) as orders
   from Customers
   order by cust_name;

.. code:: sql

   select cust_id, cust_state,
   (select count(*) from Orders where Orders.cust_id = Customers.cust_id) as orders
   from Customers
   order by cust_name;

12.Joining Tables
=================

解决subquery的问题而引入join 先join再查询, break down然后再join回去. inner join, where, multiple tables

Creating a Join
---------------

.. code:: sql

   select vend_name, prod_name, prod_price
   from Vendors, Products
   where Vendors.vend_id = Products.vend_id;

The Importance of the WHERE Clause
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select vend_name, prod_name, prod_price
   from Vendors, Products;

Inner Joins
~~~~~~~~~~~

.. code:: sql

   select vend_name, prod_name, prod_price
   from Vendors inner join Products
   on Vendors.vend_id = Products.vend_id;
   # comfortable with this solution

Joining Multiple Tables
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select prod_name, vend_name, prod_price, quantity
   from OrderItems, Products, Vendors
   where Products.vend_id = Vendors.vend_id
   and OrderItems.prod_id = Products.prod_id
   and order_num = 20007;

.. code:: sql

   SELECT cust_name, cust_contact
   FROM Customers
   WHERE cust_id IN (SELECT cust_id
                     FROM Orders
                     WHERE order_num IN (SELECT order_num
                                         FROM OrderItems
                                         WHERE prod_id = 'RGAN01'));
   #这用于思考的过程

.. code:: sql

   select cust_name, cust_contact
   from Customers, Orders, OrderItems
   where customers.cust_id = orders.cust_id
   and orders.order_num = orderitems.order_num
   and prod_id = 'rgan01';

13.Creating Advanced Joins
==========================

总结 table alias outer join, join便是对接的部分. (inner join忽略null) 在join中使用aggregate

Using Table Aliases
-------------------

.. code:: sql

   select concat(vend_name, " (", vend_country, ") ")
   as vend_title
   from Vendors
   order by vend_name;

.. code:: sql

   select cust_name, cust_contact
   from Customers as c, Orders as o, OrderItems as oi
   where c.cust_id=o.cust_id
   and oi.order_num = o.order_num
   and prod_id = "rgan01";

Using Different Join Types
--------------------------

Self Joins
~~~~~~~~~~

.. code:: sql

   select cust_id, cust_name, cust_contact
   from Customers
   where cust_name = (select cust_name from Customers where cust_contact="Jim Jones");
   # 不喜欢self join

.. code:: sql

   select c1.cust_id, c1.cust_name, c1.cust_contact
   from Customers as c1, Customers as c2
   where c1.cust_name = c2.cust_name
   and c2.cust_contact = "Jim Jones";

Natural Joins
~~~~~~~~~~~~~

|image0|

Outer Joins
~~~~~~~~~~~

.. code:: sql

   select Customers.cust_id, Orders.order_num
   from Customers inner join Orders
   on Customers.cust_id = Orders.cust_id;

.. code:: sql

   select Customers.cust_id, Orders.order_num
   from Customers left outer join Orders
   on Customers.cust_id = Orders.cust_id;

.. code:: sql

   select Customers.cust_id, Orders.order_num
   from Customers right outer join Orders
   on Orders.cust_id = Customers.cust_id;

Using Joins with Aggregate Functions
------------------------------------

.. code:: sql

   select Customers.cust_id, count(Orders.order_num) as num_order
   from Customers, Orders
   where Customers.cust_id = Orders.cust_id
   group by Customers.cust_id;

.. code:: sql

   select Customers.cust_id, count(Orders.order_num) as num_order from Customers, Orders where Customers.cust_id = Orders.cust_id group by Customers.cust_id;

14.Combining Queries
====================

总结, 这一章没有实质的内容, union便是or, intersection便是and

Creating Combined Queries
-------------------------

Using UNION
~~~~~~~~~~~

.. code:: sql

   select cust_name, cust_contact, cust_state, cust_email
   from Customers
   where cust_state in ("il", "in", "mi")
   union
   select cust_name, cust_contact, ucust_state,  cust_email
   from Customers
   where cust_name = "Fun4All";

.. code:: sql

   select cust_name, cust_contact, cust_email
   from customers
   where cust_state in ("il", "in", "mi")
   or cust_name = "fun4all";

UNION Rules
~~~~~~~~~~~

As you can see, unions are very easy to use. But there are a few rules governing exactly which can be combined:

#. A UNION must be composed of two or more SELECT statements, each separated by the keyword UNION (so, if combining four SELECT statements there would be three UNION keywords used).
#. Each query in a UNION must contain the same columns, expressions, or aggregate functions (and some DBMSs even require that columns be listed in the same order).
#. Column datatypes must be compatible: They need not be the exact same type, but they must be of a type that the DBMS can implicitly convert (for example, different numeric types or different date types).

Aside from these basic rules and restrictions, unions can be used for any data retrieval tasks.

Including or Eliminating Duplicate Rows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select cust_name, cust_contact, cust_email from Customers
   where cust_state  in ("il", "in", "mi")
   union all
   select cust_name, cust_contact, cust_email
   from Customers
   where cust_name = "fun4all";

Sorting Combined Query Results
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select cust_name, cust_contact, cust_email
   from Customers
   where cust_state  in ("il", "in", "mi")
   union all
   select cust_name, cust_contact, cust_email
   from Customers
   where cust_name = "fun4all" order by cust_name, cust_contact;

18.Using Views
==============

介绍view的三个基本应用

#. simplify complex joins
#. Reformat data
#. Filter unwanted data

Understanding Views
-------------------

.. code:: sql

   SELECT cust_name, cust_contact
   FROM Customers, Orders, OrderItems
   WHERE Customers.cust_id = Orders.cust_id
   AND OrderItems.order_num = Orders.order_num
   AND prod_id = 'RGAN01';

.. code:: sql

   SELECT cust_name, cust_contact
   FROM ProductCustomers
   WHERE prod_id = 'RGAN01';

Creating Views
--------------

Using Views to Simplify Complex Joins
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   create view  ProductCustomers as
   select cust_name,cust_contact, prod_id
   from Customers, Orders, OrderItems
   where Customers.cust_id = Orders.cust_id
   and Orders.order_num = OrderItems.order_num;

.. code:: sql

   select * from ProductCustomers;

.. code:: sql

   select cust_name, cust_contact
   from ProductCustomers
   where prod_id = "rgan01";

Using Views to Reformat Retrieved Data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   select concat(trim(vend_name), " (", trim(vend_country), ") ") as vend_title
   from Vendors order by vend_name;

+-------------------------+
| vend\ :sub:`title`      |
+=========================+
| Bear Emporium (USA)     |
+-------------------------+
| Bears R Us (USA)        |
+-------------------------+
| Doll House Inc. (USA)   |
+-------------------------+
| Fun and Games (England) |
+-------------------------+
| Furball Inc. (USA)      |
+-------------------------+
| Jouets et ours (France) |
+-------------------------+

Using Views to Filter Unwanted Data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   CREATE VIEW CustomerEMailList AS
   SELECT cust_id, cust_name, cust_email
   FROM Customers
   WHERE cust_email IS NOT NULL;

.. code:: sql

   select * from CustomerEMailList;

.. code:: sql

   create view OrderItemsExpanded as
   select order_num, prod_id, quantity, item_price, quantity*item_price as expanded_price from OrderItems where order_num = 20008;

15.Inserting Data
=================

总结 讲到了最关键的一点, 如何增加数据. insert completed rows, partial rows, insert retrieved data, copy

Understanding Data Insertion
----------------------------

Inserting Complete Rows
~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   insert into Customers
   values ("1000000006","toy land", "123 any street", "New York", "NY", "11111", "USA", NULL, NULL);

.. code:: sql

   insert into Customers (cust_id, cust_name, cust_address, cust_city, cust_state, cust_zip, cust_country, cust_contact,  cust_email) values ("1000000012", "boy land", "456 any street", "New York", "NY", "11111", "USA", NULL, NULL);

.. code:: sql

   INSERT INTO Customers(cust_id,
                         cust_contact,
                         cust_email,
                         cust_name,
                         cust_address,
                         cust_city,
                         cust_state,
                         cust_zip)
   VALUES('1000000006',
          NULL,
          NULL,
          'Toy Land',
          '123 Any Street',
          'New York',
          'NY',
          '11111');

Inserting Partial Rows
~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   INSERT INTO Customers(cust_id,
                         cust_name,
                         cust_address,
                         cust_city,
                         cust_state,
                         cust_zip,
                         cust_country)
   VALUES('1000000006',
          'Toy Land',
          '123 Any Street',
          'New York',
          'NY',
          '11111',
          'USA');

Inserting Retrieved Data
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   INSERT INTO Customers(cust_id,
                         cust_contact,
                         cust_email,
                         cust_name,
                         cust_address,
                         cust_city,
                         cust_state,
                         cust_zip,
                         cust_country)
   SELECT cust_id,
          cust_contact,
          cust_email,
          cust_name,
          cust_address,
          cust_city,
          cust_state,
          cust_zip,
          cust_country
          FROM CustNew;

Copying from One Table to Another
---------------------------------

.. code:: sql

   SELECT * INTO CustCopy FROM Customers;

.. code:: sql

   create table custcopy as
   select * from Customers;

.. code:: sql

   select cust_id, cust_name from custcopy;

16.Updating and Deleting Data
=============================

update table set #multiple 没有comma delete from

Updating Data
-------------

.. code:: sql

   update Customers
   set cust_email = "kim@thetoystore.com"
   where cust_id = "1000000005";

``SET cust_email = 'kim@thetoystore.com'``

.. code:: sql

   UPDATE Customers
   SET cust_contact = 'Sam Roberts',
   cust_email = 'sam@toyland.com'
   WHERE cust_id = '1000000006';

.. code:: sql

   UPDATE Customers
   SET cust_email = NULL
   WHERE cust_id = '1000000005';

Deleting Data
-------------

.. code:: sql

   DELETE
   FROM Customers
   WHERE cust_id = '1000000006';

17.Creating and Manipulating Tables
===================================

总结 create table, work with null, default value, alter table drop table

Creating Tables
---------------

Basic Table Creation
~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   CREATE TABLE Products3
   (
       prod_id CHAR(10)  NOT NULL,
       vend_id CHAR(10)  NOT NULL,
       prod_name CHAR(254)  NOT NULL,
       prod_price DECIMAL(8,2) NOT NULL,
       prod_desc VARCHAR(1000)  NULL
   );

Working With Null Values
~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   create table Contracts2
   (contract_num integer not null,
   contract_date datetime not null,
   contract_id char(10) not null);

.. code:: sql

   create table Suppliers (
   supplier_id char(10) not null,
   supplier_name char(50) not null,
   supplier_address char(50),
   supplier_city char(50),
   supplier_state char(50),
   supplier_zip char(10),
   supplier_country char(50));

Specifying Default Values
~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: sql

   create table ContractItems
   (
   contract_num integer not null,
   contract_item integer not null,
   prod_id char(10) not null,
   quantity integer not null default 1,
   item_price decimal(8,2) not null
   );

.. figure:: http://heropublic.oss-cn-beijing.aliyuncs.com/031136.png
   :alt: Screen Shot 2018-08-13 at 11.11.20 AM

   Screen Shot 2018-08-13 at 11.11.20 AM

Updating Tables
---------------

.. code:: sql

   alter table Suppliers add supplier_phone char(20);

.. code:: sql

   select * from Suppliers;

.. code:: sql

   alter table Suppliers
   drop column supplier_phone;

Deleting Tables
---------------

.. code:: sql

   DROP TABLE Suppliers;

Renaming Tables
---------------

Summary
-------

In this lesson, you learned several new SQL statements. CREATE TABLE is used to create new tables, ALTER TABLE is used to change table columns (or other objects like constraints or indexes), and DROP TABLE is used to completely delete a table. These statements should be used with extreme caution, and only after backups have been made. As the exact syntax of each of these statements varies from one DBMS to another, you should consult your own DBMS documentation for more information.

   Foreign-Key首先是数据类型其次才是connecting point

19.Working with Stored Procedures
=================================

Executing Stored Procedures
---------------------------

.. code:: sql

   EXECUTE AddNewProduct('JTS01',
                         'Stuffed Eiffel Tower',
                         6.49,
                         'Plush stuffed toy with the text La Tour Eiffel in red white and blue');

Creating Stored Procedures
--------------------------

.. code:: sql

   CREATE PROCEDURE MailingListCount (
   ListCount OUT INTEGER )
   IS
   v_rows INTEGER;
   BEGIN
       SELECT COUNT(*) INTO v_rows
       FROM Customers
       WHERE NOT cust_email IS NULL;
       ListCount := v_rows;
       END;

.. code:: sql

   var ReturnValue NUMBER
   EXEC MailingListCount(:ReturnValue);
   SELECT ReturnValue;

20.Managing Transaction Processing
==================================

Controlling Transactions
------------------------

Using Rollback
~~~~~~~~~~~~~~

.. code:: python

   DELETE FROM Orders;
   ROLLBACK;

Using Commit
~~~~~~~~~~~~

.. code:: python

   BEGIN TRANSACTION
   DELETE OrderItems WHERE order_num = 12345
   DELETE Orders WHERE order_num = 12345
   COMMIT TRANSACTION

.. code:: python

   SET TRANSACTION
   DELETE OrderItems WHERE order_num = 12345;
   DELETE Orders WHERE order_num = 12345;
   COMMIT;

Using Savepoints
~~~~~~~~~~~~~~~~

.. code:: python

   BEGIN TRANSACTION
   INSERT INTO Customers(cust_id, cust_name)
   VALUES('1000000010', 'Toys Emporium');
   SAVE TRANSACTION StartOrder;
   INSERT INTO Orders(order_num, order_date, cust_id) VALUES(20100,'2001/12/1','1000000010');
   IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
   INSERT INTO OrderItems(order_num, order_item, prod_id, quantity, item_price)
   VALUES(20100, 1, 'BR01', 100, 5.49);
   IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
   INSERT INTO OrderItems(order_num, order_item, prod_id, quantity, item_price) VALUES(20100, 2, 'BR03', 100, 10.99);
   IF @@ERROR <> 0 ROLLBACK TRANSACTION StartOrder;
   COMMIT TRANSACTION

21.Using Cursors
================

Working With Cursors
--------------------

Creating Cursors
~~~~~~~~~~~~~~~~

.. code:: sql

   DECLARE CustCursor CURSOR
   FOR
   SELECT * FROM Customers
   WHERE cust_email IS NULL

.. code:: sql

   DECLARE CURSOR CustCursor
   IS
   SELECT * FROM Customers WHERE cust_email IS NULL

.. _using-cursors-1:

Using Cursors
~~~~~~~~~~~~~

.. code:: sql

   DECLARE TYPE CustCursor IS REF CURSOR
       RETURN Customers%ROWTYPE;
   DECLARE CustRecord Customers%ROWTYPE
   BEGIN
       OPEN CustCursor;
       FETCH CustCursor INTO CustRecord;
       CLOSE CustCursor;
   END;

.. code:: sql

       DECLARE TYPE CustCursor IS REF CURSOR
           RETURN Customers%ROWTYPE;
       DECLARE CustRecord Customers%ROWTYPE
       BEGIN
           OPEN CustCursor;
           LOOP
           FETCH CustCursor INTO CustRecord;
           EXIT WHEN CustCursor%NOTFOUND;
           ...
   .        END LOOP;
           CLOSE CustCursor;

Closing Cursors
~~~~~~~~~~~~~~~

.. code:: sql

   CLOSE CustCursor

Here's the Microsoft SQL Server version:

.. code:: sql

   CLOSE CustCursor
   DEALLOCATE CURSOR CustCursor

The CLOSE statement is used to close cursors; once a cursor is closed, it cannot be reused without being opened again. However, a cursor does not need to be declared again to be used; an OPEN is sufficient.

22.Understanding Advanced SQL Features
======================================

Understanding Constraints
-------------------------

.. _primary-keys-1:

Primary Keys
~~~~~~~~~~~~

.. figure:: http://heropublic.oss-cn-beijing.aliyuncs.com/142840.png
   :alt: Screen Shot 2018-08-13 at 10.28.19 PM

   Screen Shot 2018-08-13 at 10.28.19 PM

.. code:: python

   ALTER TABLE Vendors
   ADD CONSTRAINT PRIMARY KEY (vend_id);

Foreign Keys
~~~~~~~~~~~~

.. code:: python

   MySQL [distributor]> create table oorders ( order_num integer not null primary key, order_date datetime not null, cust_id char(10) not null references customers(cust_id) );
   Query OK, 0 rows affected (0.157 sec)

.. code:: sql

   ALTER TABLE Orders
   ADD CONSTRAINT
   FOREIGN KEY (cust_id) REFERENCES Customers (cust_id)

#+BEGIN\ :sub:`QUOTE`

Unique Constraints
~~~~~~~~~~~~~~~~~~

.. code:: sql

   CREATE TABLE OrderItems
   (
       order_num INTEGER
       order_item INTEGER
       prod_id CHAR(10) quantity INTEGER 0),
      item_price MONEY );

Understanding Indexes
---------------------

The following statement creates a simple index on the Products table's product name column:

.. code:: sql

   CREATE INDEX prod_name_ind
   ON PRODUCTS (prod_name);

Every index must be uniquely named. Here the name prod\ :sub:`nameind` is defined after the keywords CREATE INDEX. ON is used to specify the table being indexed, and the columns to include in the index (just one in this example) are specified in parentheses after the table name.

   **Tip: Revisiting Indexes**

   Index effectiveness changes as table data is added or changed. Many database administrators find that what once was an ideal set of indexes might not be so ideal after several months of data manipulation. It is always a good idea to revisit indexes on a regular basis to fine-tune them as needed.

Understanding Triggers
----------------------

Triggers are special stored procedures that are executed automatically when specific database activity occurs. Triggers might be associated with **INSERT, UPDATE, and DELETE operations** (or any combination thereof) on specific tables.

Unlike stored procedures (which are simply stored SQL statements), triggers are tied to individual tables. A trigger associated with INSERT operations on the Orders table will be executed only when a row is inserted into the Orders table. Similarly, a trigger on INSERT and UPDATE operations on the Customers table will be executed only when those specific operations occur on that table.

Within triggers, your code has access to the following:

#. All new data in INSERT operations
#. All new data and old data in UPDATE operations
#. Deleted data in DELETE operations

Depending on the DBMS being used, triggers can be executed before or after a specified operation is performed.

The following are some common uses for Triggers:

#. Ensuring data consistency—For example, converting all state names to uppercase during an INSERT or UPDATE operation
#. Performing actions on other tables based on changes to a table—For example, writing an audit trail record to a log table each time a row is updated or deleted
#. Performing additional validation and rolling back data if needed—For example, making sure a customer's available credit has not been exceeded and blocking the insertion if it has
#. Calculating computed column values or updating timestamps As you probably expect by now, trigger creation syntax varies dramatically from one DBMS to another. Check your documentation for more details.

As you probably expect by now, trigger creation syntax varies dramatically from one DBMS to another. Check your documentation for more details.

The following example creates a trigger that converts the cust\ :sub:`state` column in the Customers table to uppercase on all INSERT and UPDATE operations.

This is the SQL Server version:

.. code:: sql

   CREATE TRIGGER customer_state
   ON Customers
   FOR INSERT, UPDATE
   AS
   UPDATE Customers
   SET cust_state = Upper(cust_state)
   WHERE Customers.cust_id = inserted.cust_id;

This is the Oracle and PostgreSQL version:

.. code:: sql

   CREATE TRIGGER customer_state
   AFTER INSERT OR UPDATE
   FOR EACH ROW
   BEGIN
   UPDATE Customers
   SET cust_state = Upper(cust_state)
   WHERE Customers.cust_id = :OLD.cust_id
   END;

..

   **Tip: Constraints Are Faster Than Triggers**

   As a rule, constraints are processed more quickly than triggers, so whenever possible, use constraints instead.

Database Security
-----------------

There is nothing more valuable to an organization than its data, and data should always be protected from would-be thieves or casual browsers. Of course, at the same time data must be accessible to users who need access to it, and so most DBMSs provide administrators with mechanisms by which to grant or restrict access to data.

The foundation of any security system is user authorization and authentication. This is the process by which a user is validated to ensure he is who he says he is and that he is allowed to perform the operation he is trying to perform. Some DBMSs integrate with operating system security for this, others maintain their own user and password lists, and still others integrate with external directory services servers.

Some operations that are often secured

#. Access to database administration features (creating tables, altering or dropping existing tables, and so on)
#. Access to specific databases or tables
#. The type of access (read-only, access to specific columns, and so on)
#. Access to tables via views or stored procedures only
#. Creation of multiple levels of security, thus allowing varying degrees of access and control based on login
#. Restricting the ability to manage user accounts

Security is managed via the SQL GRANT and REVOKE statements, although most DBMSs provide interactive administration utilities that use the GRANT and REVOKE statements internally.

.. _summary-1:

Summary
-------

In this lesson, you learned how to use some advanced SQL features. Constraints are an important part of enforcing referential integrity; indexes can improve data retrieval performance; triggers can be used to perform pre- or post-execution processing; and security options can be used to manage data access. Your own DBMS probably offers some form of these features. Refer to your DBMS documentation for more details.

Appendix C. SQL Statement Syntax
================================

To help you find the syntax you need when you need it, this appendix lists the syntax for the most frequently used SQL operations. Each statement starts with a brief description and then displays the appropriate syntax. For added convenience, you'll also find cross references to the lessons where specific statements are taught.

When reading statement syntax, remember the following:

• The ``|`` symbol is used to indicate one of several options, so ``NULL|NOT NULL`` means specify either ``NULL`` or ``NOT NULL``.

• Keywords or clauses contained within square parentheses ``[like this]`` are optional.

• The syntax listed below will work with almost all DBMSs. You are advised to consult your own DBMS documentation for details of implementing specific syntactical changes.

ALTER TABLE
-----------

``ALTER TABLE`` is used to update the schema of an existing table. To create a new table, use ``CREATE TABLE``. See `Lesson 17 <part0024.html#ch17>`__, “\ `Creating and Manipulating Tables <part0024.html#ch17>`__,” for more information.

Input

--------------

ALTER TABLE tablename (   ADD|DROP  column  datatype  [NULL|NOT NULL]  [CONSTRAINTS],   ADD|DROP  column  datatype  [NULL|NOT NULL]  [CONSTRAINTS],     … );

--------------

COMMIT
------

``COMMIT`` is used to write a transaction to the database. See `Lesson 20 <part0027.html#ch20>`__, “[[file:part0027.html#ch20][Managing Transaction Processin],” for more information.

Input

--------------

COMMIT [TRANSACTION];

--------------

CREATE INDEX
------------

``CREATE INDEX`` is used to create an index on one or more columns. See `Lesson 22 <part0029.html#ch22>`__, “\ `Understanding Advanced SQL Features <part0029.html#ch22>`__,” for more information.

Input

--------------

CREATE INDEX indexname ON tablename (column, …);

--------------

CREATE PROCEDURE
----------------

``CREATE PROCEDURE`` is used to create a stored procedure. See `Lesson 19 <part0026.html#ch19>`__, “\ `Working with Stored Procedures <part0026.html#ch19>`__,” for more information. Oracle uses a different syntax as described in that lesson.

Input

--------------

[[file:part0057_split_001.html#p249pro01][Click here to view code imag]

CREATE PROCEDURE procedurename [parameters] [options] AS SQL statement;

--------------

CREATE TABLE
------------

``CREATE TABLE`` is used to create new database tables. To update the schema of an existing table, use ``ALTER TABLE``. See `Lesson 17 <part0024.html#ch17>`__ for more information.

Input

--------------

CREATE TABLE tablename (     column    datatype    [NULL|NOT NULL]    [CONSTRAINTS],     column    datatype    [NULL|NOT NULL]    [CONSTRAINTS],        … );

--------------

CREATE VIEW
-----------

``CREATE VIEW`` is used to create a new view of one or more tables. See `Lesson 18 <part0025.html#ch18>`__, “\ `Using Views <part0025.html#ch18>`__,” for more information.

Input

--------------

CREATE VIEW viewname AS SELECT columns, … FROM tables, … [WHERE .. [GROUP BY .. [HAVING ..;

--------------

DELETE
------

``DELETE`` deletes one or more rows from a table. See `Lesson 16 <part0023.html#ch16>`__,

Input

--------------

DELETE FROM tablename [WHERE ..;

--------------

DROP
----

``DROP`` permanently removes database objects (tables, views, indexes, and so forth). See `Lessons 17 <part0024.html#ch17>`__ and `18 <part0025.html#ch18>`__ for more information.

Input

--------------

DROP INDEX|PROCEDURE|TABLE|VIEW indexname|procedurename|tablename|viewname;

--------------

INSERT
------

``INSERT`` adds a single row to a table. See `Lesson 15 <part0022.html#ch15>`__,

--------------

INSERT INTO tablename [(columns, …)] VALUES(values, …);

--------------

INSERT SELECT
-------------

``INSERT SELECT`` inserts the results of a ``SELECT`` into a table. See `Lesson 15 <part0022.html#ch15>`__ for more information.

--------------

INSERT INTO tablename [(columns, …)] SELECT columns, … FROM tablename, … [WHERE ..;

--------------

ROLLBACK
--------

``ROLLBACK`` is used to undo a transaction block. See `Lesson 20 <part0027.html#ch20>`__ for more information.

Input

--------------

ROLLBACK [ TO savepointnam;

--------------

or

Input

--------------

ROLLBACK TRANSACTION;

--------------

SELECT
------

``SELECT`` is used to retrieve data from one or more tables (or views).

--------------

SELECT columnname, … FROM tablename, … [WHERE .. [UNION .. [GROUP BY .. [HAVING .. [ORDER BY ..;

--------------

UPDATE
------

``UPDATE`` updates one or more rows in a table. See `Lesson 16 <part0023.html#ch16>`__ for more information.

Input

--------------

UPDATE tablename SET columname = value, … [WHERE ..;

--------------

Publish
=======

.. |image0| image:: http://heropublic.oss-cn-beijing.aliyuncs.com/143411.png

