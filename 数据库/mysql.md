# 创建资料库

```mysql
CREATE DATABASE `sql_name`;
SHOW DATABASES;
DROP DATABASE `sql_name`;
```

1. 大写的是关键字，可以不大写，数据库名称也可以不加反引号，但是推荐这样做
2. 创建数据库CREATE
3. 展示存在的数据库 SHOW DATABASES有“s”；
4. 删除数据库 DROP
5. 每个命令后面别忘了分号（c++友好）



# 创建表格

| 类型         | 解释                                                     |
| ------------ | -------------------------------------------------------- |
| INT          | 整数                                                     |
| DECIMAL(3,2) | 带小数点的，（3，2）表示一共三位，其中两位小数，例如1.23 |
| VARCHAR(10)  | 字符串，括号设置最大长度                                 |
| BLOB         | 二进制资料，图片、视频、                                 |
| DARA         | 日期 “YYYY-MM-DD”                                        |
| TIMESTAMP    | 时间“ yyyy-mm-dd hh:mm:ss”                               |

```mysql
-- 创建表格
CREATE TABLE `stu_subjects`(
    `student_id` INT PRIMARY KEY,
    `student_name` VARCHAR(20),
    `subject` VARCHAR(20)
);
-- 展示表格的定义
DESCRIBE `stu_subjects`;
-- 删除表格元素
ALTER TABLE `student` DROP `parent_tele_number`;
-- 展示表格数据
SELECT * FROM `stu_subjects`;
-- 插入数据的不同方式
INSERT INTO `stu_subjects` VALUES(2, "jerry", "servive");
INSERT INTO `stu_subjects`(`student_name`,`subject`,`student_id`) VALUES('pig', 'eat', 3);
INSERT INTO `stu_subjects`(`student_id`, `subject`) VALUES(4, 'math');
-- 相当于 (4, null, 'math');
```



| 限制约束       | 含义         |
| -------------- | ------------ |
| DEFAULT        | 后面跟默认值 |
| NULL           | 空值         |
| NOT NULL       | 不能为空     |
| AUTO_INCREMENT | 自增1        |

# 问题：update 不更新