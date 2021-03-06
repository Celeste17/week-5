# week-5

### 要求二：建立資料庫和資料表
---
* 建立一個新的資料庫，取名字為website。<br>
  ``` mysql
  CREATE DATABASE `website`;
  ```
* 在資料庫中，建立會員資料表，取名字為member。<br>
  ``` mysql
  CREATE TABLE `member` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `name` varchar(255) NOT NULL,
  `username` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `follower_count` int NOT NULL DEFAULT '0',
  `time` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
  );
  ```
*檔案為 website_1.sql*


### 要求三：SQL CRUD
---

* 使用 INSERT 指令新增一筆資料到 member 資料表中，這筆資料的 username 和 password 欄位必須是 test。接著繼續新增至少 4 筆隨意的資料。<br>
  ``` mysql
  INSERT INTO `member`
  (name, username, password)
  VALUES
  ("test", "test", "test"),
  ("Celeste, "test1", "test1"),
  ("Rainie, "test2", "test2"),
  ("Nicole, "test3", "test3"),
  ("Wilson, "test4", "test4");
  ```
  ![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/3-1.png) <br>
  
* 使用 SELECT 指令取得所有在 member 資料表中的會員資料。<br>
  ``` mysql
  SELECT * FROM `member`;
  ```
  ![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/3-2.png)<br>
  
* 使用 SELECT 指令取得所有在 member 資料表中的會員資料，並按照 time 欄位，由近到遠排序。<br>
  ``` mysql
  SELECT * FROM `member`
  ORDER BY time DESC;
  ```
  ![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/3-3.png)<br>
  
* 使用 SELECT 指令取得 member 資料表中第 2 ~ 4 共三筆資料，並按照 time 欄位，由近到遠排序。( 並非編號 2、3、4 的資料，而是排序後的第 2 ~ 4 筆資料 )<br>
  ``` mysql
  SELECT * FROM `member`
  ORDER BY time DESC
  LIMIT 1,3;
  ```
  ![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/3-4.png)<br>
  
* 使用 SELECT 指令取得欄位 username 是 test 的會員資料。<br>
  ``` mysql
  SELECT * FROM `member`
  WHERE username = "test";
  ```
  ![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/3-5.png)<br>
  
* 使用 SELECT 指令取得欄位 username 是 test、且欄位 password 也是 test 的資料。<br>
  ``` mysql
  SELECT * FROM `member`
  WHERE username = "test"
  AND password = "test"; 
  ```
  ![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/3-6.png)<br>
  
* 使用 UPDATE 指令更新欄位 username 是 test 的會員資料，將資料中的 name 欄位改成 test2。<br>
  ``` mysql
  UPDATE `member`
  SET name = "test2"
  WHERE username = "test";
  ```
  ![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/3-7.png)<br>
  


### 要求四：SQL Aggregate Functions
---
#### 利用要求二建立的資料庫和資料表，寫出能夠滿足以下要求的 SQL 指令：<br>
##### 目前 follower_count 欄位值
  -![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/4-0.png)<br>
  
* 取得 member 資料表中，總共有幾筆資料 ( 幾位會員 )。<br>
  ``` mysql
  SELECT COUNT(*) FROM `member`;
  ```
  -![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/4-1.png)<br>
  
* 取得 member 資料表中，所有會員 follower_count 欄位的總和。<br>
  ``` mysql
  SELECT SUM(follower_count)
  FROM `member`;
  ```
  -![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/4-2.png)<br>
  
* 取得 member 資料表中，所有會員 follower_count 欄位的平均數。<br>
  ``` mysql
  SELECT AVG(follower_count)
  FROM `member`;
  ```
  -![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/4-3.png)<br>
*檔案為 website_2.sql*

### 要求五：SQL JOIN (Optional)
---
* 在資料庫中，建立新資料表，取名字為message。<br>
  ``` mysql
  CREATE TABLE `message` (
  `id` bigint NOT NULL AUTO_INCREMENT,
  `member_id` bigint NOT NULL,
  `content` varchar(255) NOT NULL,
  `time` datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `msg_mber_id_fk` (`member_id`),
  CONSTRAINT `msg_mber_id_fk` FOREIGN KEY (`member_id`) REFERENCES `member` (`id`)
  );
  ```
  -![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/5-1.png) <br>
##### 目前 member 、 message 資料表內容
  -![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/5-2.0.png)
* 使用 SELECT 搭配 JOIN 語法，取得所有留言，結果須包含留言者會員的姓名。<br>
  ``` mysql
  SELECT g.id, g.member_id, b.name, g.content, g.time
  FROM message g JOIN member b
  ON g.member_id = b.id;
  ```
  -![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/5-2.png)
* 使用 SELECT 搭配 JOIN 語法，取得 member 資料表中欄位 username 是 test 的所有留言，資料中須包含留言者會員的姓名。<br>
  ``` mysql
  SELECT g.id, g.member_id, b.name, g.content, g.time
  FROM message g JOIN member b
  ON g.member_id = b.id;
  WHERE username = "test";
  ```
  -![](https://github.com/Celeste17/week-5/blob/main/week5%20mysql%E6%88%AA%E5%9C%96/5-3.png)
  
*完整檔案為 data.sql*
