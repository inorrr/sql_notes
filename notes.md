## Introduction ##

资料库管理系统（DBMS）database management system: 连接应用和资料库，检索，添加，删除资料

### Database 资料库
1. 关联式资料库 (SQL)
  - MySQL, Oracle, PostgreSQL, SQL Server
  - 关联式资料库管理系统(RDBMS) relational database management system
2. 非关联式资料库(noSQL/not just SQL)
  - MongoDB, Redis, DynamoDB, Elatissearch
  - 非关联式资料库管理系统(NRDBMS), not relational database management system

### What is SQL? ###
Structured Query Language. 
SQL is a language, it can be used to communicate with the management systems(i.e. mySQL)
NoSQL does not has a language that works for all, each management system has its own way of using.

## Basic Structures ##
### Tables and Keys ###

| student_id        | name           |major   |
| :-------------: |:-------------:| :-----:|
| 101      | Amy | History|
| 102      | Bob      |   English |
| 103 | Cathy     |    Biology |

1. Rows and columns: each row is an instance, each column is a variable.
2. Set key: use `student_id` as **primary key(主键)**, it can uniquely represent every row. 
3. When none of the columns can uniquely represent one row, there can be more than one primary key, we use them together to represent the row. 
4. **Foreign key(外键)** can refer to the primary key of another table.  It can also refer to the primary key of the same table.
5. A column can be both primary key and foreign key. 

## Fundamental Usage ##

### Create Database ##
```
CREATE DATABASE sql_tutorial;
SHOW DATABASE;
DROP DATABASE sql_tutorial;
```
Usually we use backquote around the names we give, the purpose is to seperate them from other key words. Keywords can be capitalized or not, it will not cause any error. 

### Data Types
- `INT` 整数
- `DECIMAL(m,n)` 有小数点的数， m is the total number of digits, n is the number of decimal places. 1.22
- `VARCHAR(n)` 字串, pure text, n is the max number of characters
- `BLOB` (binar large object) 图， 影片，档案
- `DATE` yyyy-mm-dd 日期 2023-06-06
- `TIMESTAMP` YYYY-MM-DD HH:MM:SS 详细时间

### Create Table
```
USE `sql_tutorial`;  //tell SQL to use this database
CREATE TABLE `student`(
	`student_id` INT PRIMARY KEY,
	`name` VARCHAR(20),
	`major` VARCHAR(20)  
); //note that this entire create is one command
```
1. Table is created within a database.
2. Inside the create command, we specify the variables and coresponding datatypes, as will as the primary key.
3. Primary key can also be set by written on a seperate line ``PRIMARY KEY(`student_id `)``

```
DESCRIBE `student`;  //show the table
DROP TABLE  `student`; //remove the student table, trying to describe an already deleted table will cause error.

ALTER TABLE `student` ADD  `gpa` DECIMAL(3,2); //add gpa column
ALTER TABLE `student` DROP COLUMN `gpa`;  //remove gpa column
```
1. Show the table, delete the table
2. Add or remove a colunm from a table.

### Add Value
```
INSERT INTO `student` VALUES(1, ‘小白’，’历史’ )；
INSERT INTO `student` VALUES(2, ‘小绿’，’NULL’ )；
```
1. Using a parimary key that has already been taken will cause error.
2. Using NULL will leave that box empty, primary key cannot be NULL.
```
SELECT * FROM `student`;
```
This will show all selected data, the star means select all. 
```
INSERT INTO `student`(`name`, `major`, `student_id`) VALUES(‘小蓝’，’英语’, 3)；
INSERT INTO `student`(`major`, `student_id`) VALUES(’生物’, 4)；//name will be set to NULL
```
1. The first bracket specifies variables to be filled in, the second bracket gives values, in the order of the first bracket.
2. As shown in line 2, variables not listed in the first bracket will be given a value of NULL.

### Constraints 限制
```
CREATE TABLE `student`(
	`student_id` INT AUTO_INCREMENT,    // automatically increment this variable each time, first one is 1
	`name` VARCHAR(20) NOT NULL,        // name cannot be null
	`major` VARCHAR(20)  UNIQUE,        // had to be unique value 
	`year` INT DEFAULT 1,               // set default value
	PRIMARY KEY(`student_id`)
);
```

### Update & Delete
```
SET SQL_SAFE_UPDATES = 0; //close the default update mode
```
Safe Updates: When enabled (default), MySQL Workbench will not execute UPDATE or DELETE statements if a key is not defined in the WHERE clause. 
In other words, MySQL Workbench attempts to prevent big mistakes, such as deleting a large number of (or all) rows. Set from the SQL Editor preferences tab.
We can use ``SHOW VARIABLES LIKE "sql_safe_updates";`` or ``select @@sql_safe_updates;`` to check the current state.
```
UPDATE `student`           // we want to update this table
SET `major` = `英语文学`    // change major to 英语文学
WHERE `major` = `英语`;    // of those that has major is 英语  // this is a condition, can have more than one, 
//WHERE `major` = `生物` OR `major` = `化学`      // note that the above is one big command, if there;’s no condition given, then all of the rows will be changed
DELETE FROM `student`
WHERE `student_id` = 4 and ` name` = ‘小灰’ ; //everything in this table will be deleted if there’s no WHERE
```

### Select 检索资料/搜索资料

```
SELECT `name`, `major` FROM `student` ;  //选择name, major, star means everything
SELECT `name` FROM `student` ORDER BY `score` ASC; 
SELECT `name` FROM `student` ORDER BY `score` DESC; 
SELECT `name` FROM `student` ORDER BY `score`, `student_id`; //order by score, if same then order by student_id
SELECT `name` FROM `student` LIMIT 3; //get the first 3 row
```
`AEC` means ascending order, this is default, which means that the result will be in acending order if ASC is not given. DESC means decending order. 

```
//WHERE can also be used here
SELECT * FROM `student` WHERE `major` = `英语` or `score` > 20;
```
`>`,`<`, `>=`, `<=` are all valid, `<>` means not equal to

```
SELECT * FROM `student` WHERE `major` IN('历史', '英语', '生物');
SELECT * FROM `student` WHERE `major` = '历史' OR `major` =  '英语' OR `major` =  '生物');
```
The above two lines are equal. 

## Example: Company Database

<img width="479" alt="Screen Shot 2023-06-11 at 9 57 07 AM" src="https://github.com/inorrr/sql_notes/assets/94703030/2c7a04ee-e74a-493e-9472-072090ba3a70">

### Creating Tables ###
#### Employee Table ####

```
CREATE TABLE `employee`(
    `emp_id` INT PRIMARY KEY,
    `name` VARCHAR(20), 
    `birth_date` DATE, 
    `sex` VARCHAR(1), 
    `salary` INT, 
    `branch_id` INT, 
    `sup_id` INT
);
```
Note that as this point this is the only table exists, so we cannot set any foreign key.
#### Branch Table ####
```
CREATE TABLE `branch`(
    `branch_id` INT PRIMARY KEY,
    `branch_name` VARCHAR(20), 
    `manager_id` INT,
    FOREIGN KEY(`manager_id`) REFERENCES `employee`(`emp_id`) ON DELETE SET NULL
);
```
The last line is setting foreign key. It means that `manager_id` in this table refers to `emp_id` in the employee table.

Now we have the branch table, we can set the doreign keys in the employee table.
```
ALTER TABLE `employee`
ADD FOREIGN KEY(`branch_id`)
REFERENCES `branch`(`branch_id`)
ON DELETE SET NULL;

ALTER TABLE `employee`
ADD FOREIGN KEY(`sup_id`)
REFERENCES `employee`(`emp_id`)
ON DELETE SET NULL;
```

#### Client Table ####
```
CREATE TABLE `client`(
    `client_id` INT PRIMARY KEY,
    `client_name` VARCHAR(20),
    `phone` VARCHAR(20)
);
```

#### Work_with Table ####
```
CREATE TABLE 'works_with'(
    `emp_id` INT,
    `client_id` INT,
    `totle_sales` int,
    PRIMARY KEY(`emp_id`, `client_id`),
    FOREIGN KEY(`emp_id`) REFERENCES `employee`(`emp_id`) ON DELETE CASCADE,
    FOREIGN KEY('client_id') REFERENCES `client`(`client_id`) ON DELETE CASCASE
);
```
### Adding Data ###
#### Employee ####
```
INSERT INTO `employee` VALUES(206, `小黄`, `1998-10-08`, `F`, 50000, 1, NULL);
```
Note that the above code will error, becuase branch is a foreign key that refers to the primary key of the branch table, but at this point we don't have any data in the branch table with the value 1.

In order to solve this, we can add data to the branch table first, leave the `manager_id` column empty. 
Later we change `manager_id` to the correct value.

```
INSERT INTO `branch` VALUES(1, `研发`, NULL);
INSERT INTO `branch` VALUES(2, `行政`, NULL);
INSERT INTO `branch` VALUES(3, `咨询`, NULL);
```

Next we add data to the employee table
```
INSERT INTO `employee` VALUES(206, `小黄`, `1998-10-08`, `F`, 50000, 1, NULL);
INSERT INTO `employee` VALUES(207, `小绿`, `1985-09-06`, `	M`, 29000, 2, 206);
INSERT INTO `employee` VALUES(208, `小黑`, `2000-12-19`, `M`, 35000, 3, 206);
INSERT INTO `employee` VALUES(209, `小白`, `1997-01-22`, `F`, 39000, 3, 207);
INSERT INTO `employee` VALUES(210, `小蓝`, `1925-11-10`, `F`, 84000, 1, 207);
```

Now we change manager_id to the correct value 
```
UPDATE `branch`  SET `manager_id` = 206 WHERE `branch_id` = 1;
UPDATE `branch`  SET `manager_id` = 207 WHERE `branch_id` = 2;
UPDATE `branch`  SET `manager_id` = 208 WHERE `branch_id` = 3;
```
Add client data

```
INSERT INTO `client` VALUES(400, `阿狗`, 254354335);
INSERT INTO `client` VALUES(401, `阿猫`, 9182749398);
INSERT INTO `client` VALUES(402, `旺财`, 7278364753);
INSERT INTO `client` VALUES(403, `露西`, 5894892345);
INSERT INTO `client` VALUES(404, `派克`, 2837565732);
```

Add work_with data
```
INSERT INTO `works_with` VALUES(206, 400, `70000`);
INSERT INTO `works_with` VALUES(207, 401, `24000`);
INSERT INTO `works_with` VALUES(208, 402, `9800`);
INSERT INTO `works_with` VALUES(209, 403, `24000`);
INSERT INTO `works_with` VALUES(210, 404, `87940`);

```

Exercise
1. get all employee data ``SELECT * FROM `employee`;``
2. get all client data ``SELECT * FROM `client`;``
3. get all employee data, order of acending salary ``SELECT * FROM `employee` ORDER BY `salary` ASC; ``
4. top 3 highest salary employee ``SELECT * FROM `employee` ORDER BY `salary` DESC LIMIT 3;``
5. all employee names ``SELECT `name` FROM `employee`; ``
