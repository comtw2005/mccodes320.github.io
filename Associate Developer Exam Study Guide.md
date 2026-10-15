
# Associate Developer Exam Study Guide

### Exam Study Guide

> 版本說明（2026-10-15）：下表保留本教材收錄的考綱與比例（CRUD 51%、Indexes 17%）；本次勘誤未取得完整官方考綱供重新確認比例，不宣稱這是最新版本。實際範圍以報考時的官方 Study Guide 為準。
>
> 本筆記以 Java Driver 為主要複習方向；原考綱中的 XX／XXX 代表所選語言的 driver。mongosh 範例另行標示，不能把 shell 的參數或回傳型別直接套到 Java。

[Exam Study Guide](https://learn.mongodb.com/learn/course/mongodb-associate-developer-exam-study-guide/main/associate-dba-exam-study-guide)

| 編號 | 考核內容 (Learning Objectives) |
| :--- | :--- |
| **Section 1** | **MONGODB OVERVIEW AND THE DOCUMENT MODEL (8%)** |
| 1.1 | Identify the set of value types MongoDB BSON supports. |
| 1.2 | Given three documents that are of different shape, identify which can co-exist in the same collection. |
| **Section 2** | **CRUD (51%)** |
| 2.1 | Given a scenario with a type of structured document that needs to be inserted into a database, identify properly and improperly formed insert commands. |
| 2.2 | Given an update scenario where an entire updated document (no update operators used) is provided, identify the output and how the database changed state. |
| 2.3 | Given an update scenario where $set is used, identify the output and how the database changed state. |
| 2.4 | Given a scenario about updating a document and information about where it should be inserted if it does not exist, identify the upsert command that should be used. |
| 2.5 | Given a scenario where multiple documents need to be updated, identify the correct update expression. |
| 2.6 | Given a findAndModify scenario where another operation is run concurrently, identify the output and how the database changed state. |
| 2.7 | Given a scenario where a document should be deleted from the database, identify the delete expression that should be used. |
| 2.8 | Given a scenario where a single document should be looked up by a simple equality constraint (eg {x: 3}), identify the expression that should be used. |
| 2.9 | Identify documents matched by a query with an equality constraint on an array field. |
| 2.10 | Identify documents matched by an expression with relational operators in it. |
| 2.11 | Identify documents matched by an expression with $in. |
| 2.12 | Identify documents matched by an $elemMatch expression. |
| 2.13 | Identify documents matched by an expression that has several logical operators. |
| 2.14 | Given a query with a sort and limit, identify the correct output. |
| 2.15 | Identify the incorrect projection among a set of expressions. |
| 2.16 | Identify how to get all results from a cursor. |
| 2.17 | Identify the expressions used to count the number of documents matching a query. |
| 2.18 | Given an indexing scenario, identify the correct command for defining a search index. |
| 2.19 | Given a scenario, identify the correct search query. |
| 2.20 | Given an aggregation expression using $match, $group, identify the correct output. |
| 2.21 | Given an aggregation expression using $lookup, identify the correct output. |
| 2.22 | Given an aggregation expression using $out, identify the correct output. |
| **Section 3** | **INDEXES (17%)** |
| 3.1 | Given a query that is performing a collection scan, identify which index would improve the performance of this query. |
| 3.2 | Given a query that is performing a collection scan on an equality match on an array field, identify which index would improve the performance of this query. |
| 3.3 | Given a query with no constraint and a sort of two fields that is doing collection scan, identify which index would improve the performance of this query. |
| 3.4 | Given a collection, identify how many indexes exist for that collection. |
| 3.5 | Identify the trade-offs of using indexes and the ramifications of deleting indexes support queries. |
| 3.6 | Identify the explain plan outputs that signify a potential performance issue, specifically whether an index is present or not for the given query. |
| **Section 4** | **DATA MODELING (4%)** |
| 4.1 | Given a scenario with three collections (a parent and two children) and the user, identify the embedded relationships and which should be linked. |
| 4.2 | Identify data model examples that are considered an anti-pattern. |
| **Section 5** | **TOOLS AND TOOLING (2%)** |
| 5.1 | Given a scenario to load Atlas Sample Dataset and then use Data Explorer to use it to find a given first document in a collection. |
| **Section 6** | **DRIVERS (18%)** |
| 6.1 | Define what the XX driver is? |
| 6.2 | Define how the XX application connects/uses the XXX driver? |
| 6.3 | Define the components of the URI string used by MongoClient to connect the driver to the database. |
| 6.4 | Identify what connection pooling is in terms of the driver and what advantages it offers. |
| 6.5 | Identify the correct syntax for the XX driver to insert one document and to insert many documents. |
| 6.6 | Identify the correct syntax for the XX driver to update one document and to update many documents. |
| 6.7 | Identify the correct syntax for the XX driver to delete one document and to delete many documents. |
| 6.8 | Identify the correct syntax for the XX driver to find many documents and to find one document. |
| 6.9 | Identify the correct syntax for the XX driver to create an aggregation pipeline. |
| 6.10 | Identify the different syntax for the XX driver when using the MongoDB Query Language (MQL) and when using the Aggregation Framework. |





## 教材勘誤對照（2026-10-15）

以下為本儲存庫教材修正摘要，不是新增的官方考核目標或權重。

| 考綱目標 | 同步教材 | 修正後複習重點 |
| --- | --- | --- |
| 2.1、6.5 | CRUD Lesson 1 | insertOne(document)、insertMany([documents], options)；ordered:false 不保證順序或整批原子性。 |
| 2.5、2.6 | Transactions Lesson 2 | 原子性基本單位是文件；同一 collection 的 updateMany() 不保證整批原子性。 |
| 2.9、2.12 | CRUD Lesson 4 與陣列題 | 同一元素的條件須放在同一個 $elemMatch；陣列題 2 改為單選 A，分開的 $elemMatch 不能保證同元素。 |
| 2.10、2.13 | CRUD 比較／邏輯題 | 整數與欄位存在前提需明示；$not 不可無條件當成相反比較運算子；活躍用戶題須加 status 條件。 |
| 2.13 | CRUD 邏輯題 4 | $and 可包含一個以上條件，且可與其他欄位同層；改為複選 B、C。 |
| 2.14–2.16 | CRUD、Indexes | 重複排序值需唯一欄位作次排序；find() 回傳 cursor，findOne() 回傳文件或 null；新 cursor 的 toArray() 取得全部結果，200 筆題答案 B。 |
| 3.1、3.6 | Indexes 考試註記／前綴 | Range 不會自動讓索引失效；跳過中間欄位仍可能利用最左前綴；以 executionStats 比較掃描量。 |
| 3.2 | Indexes Lesson 3 | Multikey 定義題答案 B；一般複合索引的平行陣列限制以每份文件判斷。 |
| 3.3、3.6 | Indexes 排序／覆蓋查詢 | 有索引不保證免 SORT；覆蓋查詢不需 FETCH 完整文件，不能只檢查投影欄位。 |
| 3.5 | Indexes 索引成本／刪除 | 前綴索引是否冗餘，須確認 unique、sparse、partial、collation 等選項與工作負載。 |
| 2.18 vs Section 3 | Search 與 Indexes | Search index 與一般資料庫索引分開複習；一般 createIndex() 不代表建立 Search index。 |

### Transactions 補充教材

現有目標表沒有獨立 Transactions 權重，不自行分配考試百分比。

- 單文件原子性、多文件交易與 session 的差異。
- mongosh 使用 startTransaction()、commitTransaction()、abortTransaction()；不以 shell 顯示輸出判斷方法用途。
- 交易 read concern 可用 local、majority、snapshot；read concern 不能代替 write concern。
- 不以「金融」作為必選 majority 的口訣；依題目要求的快照、可見性與持久性判斷。
- abort 只放棄目前交易的變更；預設約一分鐘生命週期限制與業務類型無關。

各教材末尾附有官方文件連結。CRUD、Indexes、Transactions 的題目與本表需一起維護。

### 模擬試題

https://learn.mongodb.com/courses/mongodb-associate-developer-exam-study-guide


https://learn.mongodb.com/learn/learning-path/mongodb-java-developer-path

MongoDB助理開發人員考試

https://learn.mongodb.com/pages/mongodb-associate-developer-exam



ref
https://github.com/yixin0829/mongodb-dev-cert-prep



## Caspar Chang

[MongoDB Certification Study Guide / MongoDB 證照考試指南 重點整理（上）](https://askstw.medium.com/mongodb-study-guide-mongodb-%E5%82%99%E8%80%83-%E8%A6%81%E9%BB%9E-%E4%B8%8A-6cd3259a6bbf)  
[MongoDB Certification Study Guide / MongoDB 證照考試指南 重點整理（下）](https://askstw.medium.com/mongodb-certification-study-guide-1fc53196eea6)  
[MongoDB Certified Overview / MongoDB 認證 流程](https://askstw.medium.com/mongodb-certification-overview-448718bec20e)  




https://www.youtube.com/watch?v=9vraN5NqpaA

https://www.youtube.com/watch?v=iHG6l6LhtZM

> 分類提醒：原列 C100DBA 題庫屬 DBA 類別，非本 Developer 考綱對照來源，已移除連結。

## 考試預約
https://go.proctoru.com/students/reservations/44603321/edit?bluebird=true





<img width="1917" height="1807" alt="image" src="https://github.com/user-attachments/assets/35208ba3-ccc9-49f1-a0c4-11a794c5f2b9" />


https://learn.mongodb.com/learn/course/mongodb-associate-developer-exam-java/purchase-and-schedule-your-certification/certification?client=customer

<img width="3356" height="1062" alt="image" src="https://github.com/user-attachments/assets/73f5d62e-d92e-48d0-b564-00d059ca6bbd" />


## 參考資料分類

原列 C100DBA 題庫屬 DBA 類別，非本 Developer 考綱對照來源。答案應以官方教材與對應版本文件核對。

## 
https://www.mongodb.com/zh-cn/docs/manual/reference/glossary/

## 
