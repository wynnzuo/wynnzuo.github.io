---
title: "mysql行列转换"
subtitle: "「mysql」- 行列转换"
layout: post
author: "WynnZuo"
header-style: text
tags:
  - mysql
---


### 适用场景
在一行数据里，同一个列中存储了多个属性值，需要将此行的多属性列依据分隔符分隔，
得到多行的结果，每行的被分隔列只存储单一属性。
#### 建表语句
```
DROP TABLE IF EXISTS tbl_name;
	
CREATE TABLE tbl_name 
( 
 id INT ( 11 ) NOT NULL auto_increment, 
 userName VARCHAR ( 100 ) NOT NULL, 
 PRIMARY KEY ( id ) 
) 
ENGINE = INNODB AUTO_INCREMENT = 2 DEFAULT CHARSET = utf8;

INSERT INTO tbl_name VALUES ( 1, 'a,aa,aaa' ),( 2, 'b,bb' ),( 3, 'c,cc' );
```
![img](/img/in-post/post-wynn/line-to-list1.png)
#### 查询语句
```
SELECT
	a.id,
	SUBSTRING_INDEX( SUBSTRING_INDEX( a.userName, ',', b.help_topic_id + 1 ), ',',- 1 ) AS NAME 
FROM
	tbl_name a
	LEFT JOIN mysql.help_topic b ON b.help_topic_id < ( LENGTH( a.userName ) - LENGTH( REPLACE ( a.userName, ',', '' ) ) + 1 ) 
ORDER BY
	a.id;
```
**查询结果**  
![img](/img/in-post/post-wynn/line-to-list2.png)
#### 分析
```
SELECT
	a.id,
	b.help_topic_id + 1 num,
	SUBSTRING_INDEX( a.userName, ',', b.help_topic_id + 1 ) AS NAME_tmp,
	SUBSTRING_INDEX( SUBSTRING_INDEX( a.userName, ',', b.help_topic_id + 1 ), ',',- 1 ) AS NAME 
FROM
	tbl_name a
	LEFT JOIN mysql.help_topic b ON b.help_topic_id < ( LENGTH( a.userName ) - LENGTH( REPLACE ( a.userName, ',', '' ) ) + 1 ) 
ORDER BY
	a.id;
```
**分析结果**  
![img](/img/in-post/post-wynn/line-to-list3.png)  
**拆分数量**
```
LENGTH( a.userName ) - LENGTH( REPLACE ( a.userName, ',', '' ) ) + 1
```
#### 其他
需要一个拥有连续数列的独立表，并且连续数列的最大值一定要大于符合分割的值的个数。当然，mysql内部也有现成的连续数列表可用。
如mysql.help_topic 其中 help_topic_id 共有504个数值，从0开始自增的，一般能满足于大部分需求了，也可用其他表的自增字段辅助。
#### 加深理解
**样例SQL**
```
-- '7654,7698,7782,7788'  按","分割变成四行数据
SELECT
	help_topic_id id,
	SUBSTRING_INDEX( '7654,7698,7782,7788', ',', help_topic_id + 1 ) temp,
	SUBSTRING_INDEX( SUBSTRING_INDEX( '7654,7698,7782,7788', ',', help_topic_id + 1 ), ',',- 1 ) AS num 
FROM
	mysql.help_topic 
WHERE
	help_topic_id < LENGTH( '7654,7698,7782,7788' ) - LENGTH( REPLACE ( '7654,7698,7782,7788', ',', '' ) ) + 1
```
**查询结果**  
![img](/img/in-post/post-wynn/line-to-list4.png)  
**知识点**  
1. 字符串拆分:SUBSTRING_INDEX（str, delim, count）
 
    **参数**  |  **解释**
    ---- | ----- 
    str  | 需要拆分的字符串 
    delim  | 分隔符
    count  | 正数取第n个分隔符之前的所有字符 , 负数取倒数第n个分隔符之后的所有字符
    
    * 获取第2个以“，”逗号为分隔符之前的所有字符。
    ```
    SUBSTRING_INDEX('7654,7698,7782,7788',',',2)
    ```
    * 获取倒数第2个以“，”逗号分隔符之后的所有字符   
     ```
     SUBSTRING_INDEX('7654,7698,7782,7788',',',-2)
     ```
2. 替换函数：REPLACE( str, from_str, to_str)
3. 获取字符串长度：LENGTH( str )

**SQL解析**  
1. 获取最后需被拆分成多少个字符串，利用 help_topic_id 来模拟遍历 第n个字符串
```
help_topic_id < LENGTH('7654,7698,7782,7788')-LENGTH(REPLACE('7654,7698,7782,7788',',',''))+1
```
2. 根据“，”逗号来拆分字符串，此处利用 SUBSTRING_INDEX（str, delim, count） 函数，最后把结果赋值给 num 字段。
```
SUBSTRING_INDEX(SUBSTRING_INDEX('7654,7698,7782,7788',',',help_topic_id+1),',',-1) AS num 
```