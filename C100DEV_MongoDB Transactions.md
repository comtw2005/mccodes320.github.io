
Lesson 1: Introduction to ACID Transactions  
Lesson 2: ACID Transactions in MongoDB  



# 考試註記

本檔程式碼為 mongosh JavaScript；Java Driver 的參數、回傳型別與 session 傳遞方式須另查對應文件。

1. Read concern 與 write concern 解決不同問題，不要只因題目出現「金融」就選 majority。
   - local：讀取目前節點資料，不保證已多數提交，資料可能回滾。
   - available：不保證多數提交；分片環境不做相同的 shard ownership 檢查，容忍分區，但可能讀到孤兒文件。
   - majority：讀取多數提交的資料，不等於最新資料或跨分片同一時間點快照。
   - linearizable：針對符合條件的單文件讀取提供即時順序保證；與 majority 不同，也不適用多文件交易。
   - snapshot：提供同一時間點的快照；交易跨分片時可提供同步快照。
2. 多文件交易可使用 local、majority、snapshot read concern；available、linearizable 不適用。
3. 交易內 majority／snapshot 的多數提交資料保證，需搭配以 write concern {w:"majority"} 提交；以 w:1 提交仍可能在故障轉移時回滾。
4. 不使用固定的安全性或速度排名作答；依題目所需的原子性、可見性、快照與寫入確認保證選擇。
5. 多文件交易適用於 replica set 或 sharded cluster，不支援 standalone。Session 本身不代表交易已開始。

Lesson 1: Introduction to ACID Transactions
---
https://learn.mongodb.com/learn/course/mongodb-transactions/lesson-1-introduction-to-acid-transactions/learn

ACID Transcation

An ACID transcation is a group of database operations that must happen together or not at all, ensuring database safety and consittency

使用場景在將記錄傳喚到另一則記錄中, 如金融交易與商品賣賣

Atomicity 原子性, 全部的操作只有同時成功或同時失敗

Guarantees that every transaction is "all or nothing" when committing data to a database. For example, we don't want money to be taken from an account but not successfully added to another.

Consistency 一致性

一致性要求操作維持已定義的約束。MongoDB 不會自動知道餘額不能為負等業務規則；必須以更新條件、驗證規則或應用程式檢查實作，並在必要時中止交易。

Isolation 隔離

隔離性描述並行操作之間的隔離保證。MongoDB 交易不可直接推論為所有並行結果都等同循序執行；讀取保證取決於 read concern，snapshot isolation 也不等於完整 serializable isolation。應明確設計業務約束並處理寫入衝突。

Durability 耐久性

Durability（持久性）描述已提交變更的保存保證；MongoDB 的實際故障保證取決於 write concern、journaling 與部署設定。以 w:1 提交的交易仍可能在故障轉移時回滾，不能概括為任何故障下資料永不遺失。


QA


Which of the following is the best definition of an ACID transaction? (Select one.)

a.
A group of database operations that cannot fail.
b.
Database operations that involve transferring money between two parties.
c.
A group of database operations that must happen all together or not at all.
d.
Database operations that are durable and will persist even if the server is down.


==> C


a:A database operation involved in an ACID transaction can fail. But if an operation does fail, the rest will fail as well.

B:ACID transactions can be used to transfer funds or goods between parties, but they are not limited to this use case.

C: An ACID transaction involves a group of database operations that must happen together successfully or not at all.

D:  Durability is only one of the ACID properties that transactions have.





Which of the following scenarios require the use of an ACID transaction? (Select all that apply.)

a.
Updating a bank database to reflect the transfer of money from Customer A's bank account into Customer B's bank account.
b.
Updating inventory and shopping cart records when a customer adds an item to their online shopping cart in an ecommerce app.
c.
Updating records in a user database to bulk add user profile pictures.
d.
Adding multiple documents that contain information about movies to a database.


==> AB




A:In the case of a bank account transfer, the database operations MUST happen atomically. This scenario requires the use of ACID properties to guarantee that all operations happen successfully and securely, or that they don't happen at all.


B:In most scenarios involving the transfer of value, inventory, or ownership of goods, database operations MUST happen atomically. This scenario requires the use of ACID properties to guarantee that both the inventory and shopping cart records are updated together or not at all.


C:In this scenario, adding user profile pictures to a database probably does not need to be an ACID compliant transaction. ACID transactions are needed when database operations MUST happen atomically, such as most scenarios involving the transfer of value, inventory, or ownership of goods.


D: In this scenario, adding new documents that contain information about movies probably does not need to be an ACID-compliant transaction. ACID transactions are needed when database operations MUST happen atomically, such as most scenarios involving the transfer of value, inventory, or ownership of goods.




Lesson 2: ACID Transactions in MongoDB
---

A group of db operations that will be completed as a unit or not at all. 

### Single-document operations are already atomic in MondoDB
MongoDB 的單一文件寫入具原子性。即使 updateMany() 只操作同一個 collection，也僅保證每份文件的修改各自原子，不保證整批修改全部成功或全部失敗；需要整批原子性時才使用多文件交易。

類似updateOne，將修改後的結果同時展示給客戶端
<img width="1161" height="795" alt="image" src="https://github.com/user-attachments/assets/83c34e30-3982-47de-907e-f98624581f8c" />

### Mulit-document Operations
相較Single 多個文檔操作本質上就不是原子的，需要額外的步驟

1. Are not inherently atimic
2. Require extra steps to have ACID properties
3. 
<img width="646" height="795" alt="image" src="https://github.com/user-attachments/assets/b580f9dc-1966-448b-a603-0164f4dcb9c8" />

<img width="1199" height="806" alt="image" src="https://github.com/user-attachments/assets/7618a963-2b28-4988-bfc5-b96b39d7cb78" />

### Mulit-document ACID Transcation

1. MongoDB "locks" resources involved in a transcation
2. Incurs performance cost and affects latency
3. Use multi-document transcations as a precise(精確) tool。在絕對確定要使用ACID屬性完成多的文檔操作時才被使用


### Learn

All single-document operations are atomic。  
  
單一文件寫入不需多文件交易即可取得原子性；讀取一致性與故障持久性仍須依 read/write concern 與部署設定判斷。  

<img width="482" height="456" alt="image" src="https://github.com/user-attachments/assets/95ad000e-71c6-43cc-8ded-141b9c215b40" />





QA

Which of the following statements are true about multi-document transactions in MongoDB? (Select all that apply.)

a.
Database operations that affect more than one document, like .updateMany(), are not inherently atomic in MongoDB and must be completed by using a multi-document transaction in order to have ACID properties.
b.
Multi-document transactions should be treated as a precise tool that is used only in certain scenarios.
c.
Using a multi-document transaction with a MongoDB database ensures that the database will be in a consistent state after running a set of operations on multiple documents.
d.
When using multi-document transactions in MongoDB, each individual read and write operation must be wrapped in its own transaction.


==> ABC


A:A multi-document must be used to ensure that database operations that affect more than one document in a MongoDB database have ACID properties.


B:Multi-document transactions can incur a performance cost. It's important to follow best practices and use multi-document transactions only when ACID properties are essential.


C:Using a multi-document transaction guarantees that operations have ACID properties and, therefore, that the database will be in a consistent state once the transaction is complete.


D:All database operations involved in a multi-document transaction should be wrapped in the same transaction so that all the operations succeed or fail together.















Nadia needs to update customer account balances across multiple collections in MongoDB. It's important that the database operations used in this transaction adhere to ACID properties. Should Nadia use a transaction in this scenario? (Select one.)

a.
Nadia does NOT need to use a transaction in this scenario because multi-document operations are inherently atomic in MongoDB.
b.
Nadia does need to use a transaction in this scenario because multi-document operations are NOT inherently atomic in MongoDB.

==> B



A:Nadia should use a multi-document transaction in this scenario. Unlike single-document operations, multi-document operations are not inherently atomic in MongoDB. Because the group of database operations will be performed on multiple documents and require ACID properties, they should use a multi-document transaction.


B:Nadia should use a multi-document transaction in this scenario. Unlike single-document operations, multi-document operations are not inherently atomic in MongoDB. Because the group of database operations will be performed on multiple documents and require ACID properties, they should use a multi-document transaction.


C:
D:


Lesson 3: Using Transactions in MongoDB
---

示範多文件交易與中斷多文件交易，帳戶之間的資金轉移。 

交易預設生命週期限制約一分鐘，由 transactionLifetimeLimitSeconds 控制；這是一般交易限制，與是否為資金交易無關。  

兩個帳戶舉例如下
<img width="1136" height="757" alt="image" src="https://github.com/user-attachments/assets/34dfe597-981a-4be5-87b0-023ad8425f9f" />

### Session

Used to group database operations that are related to each other and should be run together




Using the mongosh tab, run the following command to create a session variable for the multi-document transaction:
  
```javascript
const session = db.getMongo().startSession()    // 進入mongodb Session
```

This command creates a new session, but it hasn't started a transaction yet. In the next step, you'll start the transaction using this session


Next, start a transaction using the session variable you created:
 
```javascript
session.startTransaction()                      // 開始交易
```

Now use the following command to create an account variable which is a reference to the accounts collection using the session you created:

```javascript
const account = session.getDatabase('bank').getCollection('accounts')                         // 帳戶合集的操作
```
```javascript
account.updateOne({ account_id: "MDB740836066"} , {$inc: {balance: -30}})
```
```javascript

account.updateOne({ account_id: "MDB963134500"} , {$inc: {balance: 30}})

```

Finally, complete the transaction on the session using the following command:

```javascript
session.commitTransaction()
```



### Multi-Document Transactions
ACID transactions in MongoDB are typically used only by applications where values are exchanged between different parties, such as banking or business applications. If you find yourself in a scenario where a multi-document transaction is required, it's very likely that you will complete a transaction with one of MongoDB's drivers. For now, let's focus on completing and canceling multi-document transactions in the shell to become familiar with the steps.


### Using a Transaction: Video Code
Here is a recap of the code that's used to complete a multi-document transaction:

```javascript
const session = db.getMongo().startSession()    // 進入mongodb Session

session.startTransaction()                      // 開始交易

const account = session.getDatabase('< add database name here>').getCollection('<add collection name here>')                         // 帳戶合集的操作

//Add database operations like .updateOne() here

session.commitTransaction()
```

### Aborting a Transaction
If you find yourself in a scenario that requires you to roll back database operations before a transaction is completed, you can abort the transaction. This discards only the changes made in the current uncommitted transaction; it does not restore the entire database or undo other committed operations.


### Aborting a Transaction: Video Code
Here is a recap of the code that's used to cancel a transaction before it completes:
```javascript
const session = db.getMongo().startSession()

session.startTransaction()

const account = session.getDatabase('< add database name here>').getCollection('<add collection name here>')

//Add database operations like .updateOne() here

session.abortTransaction()
```


QA


You are creating a transaction that does the following:

* Inserts a new savings account into the accounts collection for an existing customer

* Funds the new savings account with $200 from their checking account

The transaction looks like this: 

```javascript
const session = db.getMongo().startSession()

session.startTransaction()

const account = session.getDatabase('bank').getCollection('accounts')

account.insertOne({
  account_id: "MDB361428849",
  account_holder: "Donna Wood",
  account_type: "savings",
  balance: 200.0,
  transfers_complete: [],
  last_updated: new Date()
})
account.updateOne( { account_id: "MDB919841472" }, {$inc: { balance: -200.00 }})
```


What method should you use to complete this transaction? (Select one.)

a.
endTransaction()
b.
commitTransaction()
c.
endSessions()
d.
completeTransaction()


==> B


A:.endTransaction() is not a valid method in MongoDB.


B:We use .commitTransaction() to commit a transaction.session.commitTransaction() should be placed at the end of the transaction.

C: endSessions 是資料庫命令名稱，不是此處的 session helper；session.endSession() 用於結束 session，不能代替 commitTransaction()。

D:completeTransaction() is not a valid method in MongoDB.





選擇一個答案。注意：這裡考的是方法用途，不是 shell 顯示輸出；mongosh commitTransaction() 與 abortTransaction() 不回傳值。

Which mongosh method commits the current transaction? (Select one.)

a.
.startTransaction()
b.
.commitTransaction()
c.
.abortTransaction()

==> B








## Conclusion


### MongoDB Transactions
In this unit, you learned that ACID transactions ensure that database operations, such as transferring funds from one account to another, happen together or not at all. You also explored how ACID transactions work with the document model in MongoDB. Finally, you learned how to create and use multi-document transactions by using the startTransaction() and commitTransaction() commands, and how to cancel multi-document transactions by using the abortTransaction() command.


### Resources
Use the following resources to learn more about ACID transactions in MongoDB:

What are ACID transactions?
How do ACID transactions work in MongoDB?

## 官方勘誤依據（2026-10-15）

- [Atomicity and transactions](https://www.mongodb.com/docs/manual/core/write-operations-atomicity/)
- [Transactions / read and write concern](https://www.mongodb.com/docs/manual/core/transactions/)
- [Transaction lifetime](https://www.mongodb.com/docs/manual/core/transactions-production-consideration/)
- [commitTransaction](https://www.mongodb.com/docs/manual/reference/method/Session.commitTransaction/)
- [abortTransaction](https://www.mongodb.com/docs/manual/reference/method/Session.abortTransaction/)
