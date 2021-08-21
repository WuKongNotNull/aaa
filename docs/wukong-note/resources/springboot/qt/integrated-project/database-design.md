# 数据库设计

## **数据库概述**



库名 wukong_blog ，包含 6 张表<br>



**文章详情表  b_article** <br>

| 字段名        | 类型     | 长度 | 备注               |
| ------------- | -------- | ---- | ------------------ |
| id            | int      | 20   | 文章 id            |
| title         | varchar  | 100  | 文章标题           |
| content       | longtext |      | 文章内容           |
| categories    | varchar  | 200  | 文章分类           |
| tags          | varchar  | 200  | 文章标签           |
| allow_comment | tinyint  | 1    | 是否允许评论       |
| thumbnail     | varchar  | 200  | 文章缩略图         |
| create_by     | int      | 20   | 创建人（用户 id）  |
| create_date   | datetime |      | 创建时间           |
| modify_by     | int      | 20   | 修改人（用户 id ） |
| modify_date   | datetime |      | 修改时间           |



<br>

**评论表  b_comment** <br>

| 字段名      | 类型     | 长度 | 备注                 |
| ----------- | -------- | ---- | -------------------- |
| id          | int      | 20   | 评论 id              |
| article_id  | int      | 20   | 外键 文章i d         |
| content     | text     |      | 评论内容             |
| status      | varchar  | 200  | 评论状态（默认允许） |
| author      | Varchar  | 100  | 评论作者             |
| create_by   | int      | 20   | 创建人（用户 id）    |
| create_date | datetime |      | 创建时间             |
| modify_by   | int      | 20   | 修改人（用户 id ）   |
| modify_date | datetime |      | 修改时间             |



<br>

**统计表 b_statistic** <br>

| 字段名      | 类型     | 长度 | 备注               |
| ----------- | -------- | ---- | ------------------ |
| id          | int      | 20   | 文章统计 id        |
| article_id  | int      | 20   | 文章 id            |
| hit         | int      | 20   | 文章点击量         |
| comment_num | int      | 20   | 文章评论量         |
| create_by   | int      | 20   | 创建人（用户 id）  |
| create_date | datetime |      | 创建时间           |
| modify_by   | int      | 20   | 修改人（用户 id ） |
| modify_date | datetime |      | 修改时间           |



<br>

**用户表 b_user** <br>

| 字段名      | 类型     | 长度 | 备注                                    |
| ----------- | -------- | ---- | --------------------------------------- |
| id          | int      | 20   | 用户  id                                |
| username    | varchar  | 100  | 用户名                                  |
| password    | varchar  | 100  | 密码                                    |
| email       | varchar  | 100  | 密码                                    |
| valid       | tinyint  | 1    | 用户是否有效（默认有效）1：有效 0：无效 |
| create_by   | int      | 20   | 创建人（用户 id）                       |
| create_date | datetime |      | 创建时间                                |
| modify_by   | int      | 20   | 修改人（用户 id ）                      |
| modify_date | datetime |      | 修改时间                                |



<br>

**用户权限表 b_authority** <br>

| 字段名      | 类型     | 长度 | 备注                   |
| ----------- | -------- | ---- | ---------------------- |
| id          | int      | 20   | 权限  id               |
| authority   | varchar  | 100  | 权限类型：以ROLE_ 开头 |
| create_by   | int      | 20   | 创建人（用户 id）      |
| create_date | datetime |      | 创建时间               |
| modify_by   | int      | 20   | 修改人（用户 id ）     |
| modify_date | datetime |      | 修改时间               |



<br>



**用户权限-文章关联表 b_authority_article** <br>

| 字段名       | 类型     | 长度 | 备注               |
| ------------ | -------- | ---- | ------------------ |
| id           | int      | 20   | id                 |
| article_id   | int      | 20   | 文章 id            |
| authority_id | int      | 20   | 权限 id            |
| create_by    | int      | 20   | 创建人（用户 id）  |
| create_date  | datetime |      | 创建时间           |
| modify_by    | int      | 20   | 修改人（用户 id ） |
| modify_date  | datetime |      | 修改时间           |

<br>



## **sql 脚本**

```sql
CREATE DATABASE wukong_blog;

USE wukong_blog;

CREATE TABLE `b_article` (
  `id` INT(20) NOT NULL AUTO_INCREMENT COMMENT '文章id',
  `title` VARCHAR(200) DEFAULT NULL COMMENT '文章标题',
  `content` LONGTEXT COMMENT '文章内容',
  PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO `b_article` VALUES ('1', 'Spring Boot 青铜篇', '从入门到放弃讲解...');
INSERT INTO `b_article` VALUES ('2', 'Spring Cloud 王者篇', '从入门到入土讲解...');

  CREATE TABLE `b_comment` (
  `id` INT(20) NOT NULL AUTO_INCREMENT COMMENT '评论id',
  `content` LONGTEXT COMMENT '评论内容',
  `author` VARCHAR(200) DEFAULT NULL COMMENT '评论作者',
  `article_id` INT(20) DEFAULT NULL COMMENT '外键：文章id',
  PRIMARY KEY (`id`)
) ENGINE=INNODB  DEFAULT CHARSET=utf8;

INSERT INTO `b_comment` VALUES ('1', '赞1', 'wukong', '1');
INSERT INTO `b_comment` VALUES ('2', '赞2', 'zhubajie', '1');
INSERT INTO `b_comment` VALUES ('3', '赞3', 'tangseng', '1');
INSERT INTO `b_comment` VALUES ('4', '赞4', 'bailongma', '2');
INSERT INTO `b_comment` VALUES ('5', '赞5', 'baigujing', '2');


```



