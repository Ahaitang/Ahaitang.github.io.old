+++
title = 'NOSQL期末复习——MongoDB'
date = 2024-12-22T01:06:20+08:00
weight=5
categories = ['AHUCM', 'NOSQL', '期末复习', 'MongoDB']
images = '/static/NOSQLRECOVER/avatar.png'
+++
# MongoDB
## mongoDB简介
### 什么是MongoDB？
MongoDB是一个基于分布式文件存储的数据库。它是一个开源的文档数据库，旨在为web应用提供可扩展的高性能。

### MongoDB的优点有哪些？    
1. 高性能：MongoDB使用了基于磁盘的存储，它的性能非常高。    
2. 易于使用：MongoDB使用了JSON格式，使得数据的存储和查询变得非常简单。    
3. 自动分片：MongoDB可以自动分片，以便在服务器之间分布数据。    
4. 动态查询：MongoDB支持丰富的查询语言，使得数据的查询变得更加灵活。    
5. 丰富的数据类型：MongoDB支持丰富的数据类型，如字符串、数值、日期、数组、对象等。    
6. 高可用性：MongoDB支持副本集，以提供高可用性。    
7. 自动故障转移：MongoDB可以自动故障转移，以便在服务器发生故障时自动切换到另一个服务器。    
8. 丰富的工具：MongoDB提供了丰富的工具，如mongostat、mongoexport、mongoimport等，使得数据库管理变得简单。    
9. 开源：MongoDB是开源的，任何人都可以免费下载和使用。    

### MongoDB的缺点有哪些？    
1. 弱一致性：MongoDB的查询不保证数据的强一致性。    
2. 高学习曲线：MongoDB的学习曲线比较陡峭。    
3. 文档模型：MongoDB的文档模型使得数据之间的关系不明确。    
4. 复杂的查询语言：MongoDB的查询语言比较复杂。    
5. 缺乏事务支持：MongoDB不支持事务。    
6. 缺乏 joins 支持：MongoDB不支持 joins。    
7. 缺乏 ACID 支持：MongoDB不支持 ACID。    
### MongoDB的应用场景有哪些？    
1. 高性能Web应用：MongoDB可以用于高性能Web应用，如社交网络、内容管理系统、博客网站等。    
2. 实时数据分析：MongoDB可以用于实时数据分析，如实时推荐系统、实时监控系统等。    
3. 移动应用：MongoDB可以用于移动应用，如社交网络、地图应用等。    
4. 物联网应用：MongoDB可以用于物联网应用，如智能家居、智能监控等。    
5. 高并发访问：MongoDB可以用于高并发访问，如社交网络、电商网站等。    
6. 实时游戏：MongoDB可以用于实时游戏，如在线游戏、即时战斗等。    
7. 高性能计算：MongoDB可以用于高性能计算，如大数据分析、科学计算等。    
8. 高可靠性系统：MongoDB可以用于高可靠性系统，如银行系统、电信系统等。    
9. 云计算：MongoDB可以用于云计算，如云存储、云计算平台等。    
10. 高性能分析：MongoDB可以用于高性能分析，如日志分析、数据分析等。  

## mongoDB的安装步骤(包含mongosh)
**下载 mongoBD 与 mongosh**  
mongoBD: https://www.mongodb.com/try/download/community  
![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-1.png)   
mongosh: https://www.mongodb.com/try/download/shell  
![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-2.png)


**安装mongoDB**
1. 将monogoDB解压后安装，将其安装到一个不包含中文路径的地方,并将 mongosh 解压至 monogodb文件夹内部  
 ![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-3.png)  
2. 配置mongodb 与 monogosh的环境变量  
![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-4.png)*![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-6.png)*
3. 在mongodb下创建Data文件夹与 Log 文件夹，并在 Log 文件夹下创建 mongo.log 文件  
![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-7.png)![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-8.png)
 
4. 下载 MongoDB 服务，并启动  
 ![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-9.png)![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-10.png)
5. 在终端验证环境是否配置成功  
 ![](https://cdn.jsdelivr.net/gh/Ahaitang/PicGo@master/Images/NOSQL%E5%A4%8D%E4%B9%A0-11.png)
## MongoDB 有关操作
### SHELL 终端命令
**//1.进入my_test数据库**
```sql
 use my_test;
```

**//2.向数据库的user集合中插入一个文档**  
```sql
db.user.insert({name:'张三',age:18,gender:'男',email:'zhangsan@163.com'});
```

**//3.查询user集合中的文档**
```sql
db.user.find();
```

**//4.向数据库的user集合中插入一个文档**
```sql
db.user.insert({name:'李四',age:18,gender:'女',email:'lisi@163.com'});
```

**//5.查询数据库user集合中的文档**
```sql
db.user.find();
```

**//6.统计数据库user集合中的文档数量**
```sql
db.user.count();
```

**//7.查询数据库user集合中username为zs的文档**
```sql
db.user.find({name:'张三'});
```

**//8.向数据库user集合中的username为zs的文档，添加一个address属性，属性值为azy**
```sql
db.user.update({name:'张三'},{$set:{address:'azy'}});
```

**//9.使用{username:"lisi"} 替换 username 为 wangwu的文档**
```sql
db.user.replaceOne({name:'李四'},{name:'王五'},1);
```

**//10.删除username为zs的文档的address属性**
```sql
db.user.updateOne({name:'张三'},{$unset:{address:'azy'}});
```

**//11.向username为zs的文档中，添加一个hobby:{cities:["beijing","shanghai","shenzhen"] , movies:["sanguo","hero"]}**
**//在MongoDB的文档中的属性值也可以是一个文档，当一个文档的属性值是一个文档时，称这个文件为内嵌文档**
```sql
db.user.update({name:'张三'},{$set:{hobby:{cities:["beijing","shanghai","shenzhen"], movies:["sanguo","hero"]}}});
```

**//12.向username为tangseng的文档中，添加一个hobby:{movies:["A Chinese Odyssey","King of comedy"]}**
```sql
db.user.insert({name:'唐僧',age:18,gender:'男',email:'tangsen@163.com'});

db.user.update({name:'唐僧'},{$set:{hobby:{movies:["A Chinese Odyssey","King of comedy"]}}});

```

**//13.查询喜欢电影hero的文档**
**//MongoDB支持直接通过内嵌文档的属性进行查询，如果查询内嵌文档可以通过.的形式匹配** **//如果要通过内嵌文档对文档进行查询，此时属性名必须使用引号(单引号、双引号都可以)**
```sql
db.user.find({"hobby.movies":'hero'});
```

**//14.向tangseng中添加一个新的电影Interstellar\$push,用于向数组中添加一个新的元素 \$addToSet向数组中添加有一个新的元素,如果数组中已经有了这个元素，则不会再添加了**
```sql
db.user.update({name:'唐僧'},{$push:{"hobby.movies":'Interstellar'}});
db.user.update({name:'唐僧'},{$addToSet:{"hobby.movies":'Interstellar'}});
```

**//15.删除喜欢beijing的用户**
```sql
db.user.remove({"hobby.cities":'beijing'});
```

**//16.删除user集合**
```sql
db.user.drop();
```

**//18.查询numbers中num为500的文档**
```sql
db.numbers.insert(
[
 {num:500},{num:5001},{num:29},{num:45},{num:19997},
 {num:501},{num:5002},{num:28},{num:46},{num:19998},
 {num:502},{num:5003},{num:27},{num:47},{num:19999}
]
);

db.numbers.find({num:500});
```

**//19.查询numbers中num大于5000的文档**
```sql
db.numbers.find({num:{$gt:5000}});
```

**//20.查询numbers中num小于30的文档**
```sql
db.numbers.find({num:{$lt:30}});
```

**//21.查询numbers中num大于40小于50的文档**
```sql
db.numbers.find({num:{$gt:30, $lt:50}});
```

**//22.查询numbers中num大于19996的文档**
```sql
db.numbers.find({num:{$gt:19996}});
```

**//23.查看numbers集合中的前10条数据**
**//在开发中我们绝对不会进行不带条件的查询**
```sql
db.numbers.find().limit(10);
```

**//24.查看numbers集合中的第11条到20条数据** **//skip(), limilt(), sort()三个放在一起执行的时候，执行的顺序是先 sort(), 然后是 skip()，最后是显示的 limit()，和命令编写顺序无关。**
```sql
db.numbers.find().skip(10).limit(10);
```

**//25.查看numbers集合中的第21条到30条数据**
```sql
db.numbers.find().skip(20).limit(10);
```

**//26.将dept和emp集合导入到数据库中**
```sql
db.dept.insert(
[
    { "id": 1, "name": "财务" },
    { "id": 2, "name": "销售" }
]
);

db.emp.insert(
[
    { "id": 101, "name": "张三", "salary": 1800, "dept_id": 1, "mgr": 7698 },
    { "id": 102, "name": "李四", "salary": 2500, "dept_id": 2, "mgr": 7698 },
    { "id": 103, "name": "王五", "salary": 900, "dept_id": 1, "mgr": 7698 },
    { "id": 104, "name": "赵六", "salary": 3000, "dept_id": 2, "mgr": 1234 }
]
);
```

**//27.查询工资小于2000的员工**
```sql
db.emp.find({salary: {$lt: 2000}});
```

**//28.查询工资在1000-2000之间的员工**
```sql
db.emp.find({salary: {$gte: 1000, $lte: 2000}});
```

**//29.查询工资小于1000或大于2500的员工**
```sql

db.emp.find({$or: [{salary: {$lt: 1000}}, {salary: {$gt: 2500}}]});
```

**//30.查询财务部的所有员工**
```sql
db.emp.find({dept_id: 1});
```

**//31.查询销售部的所有员工**
```sql
db.emp.find({dept_id: 2});
```

**//32.查询所有mgr为7698的所有员工**
```sql
db.emp.find({mgr: 7698});
```

**//33.为所有薪资低于1000的员工增加工资400元**
```sql
db.emp.update({salary: {$lt: 1000}}, {$inc: {salary:400}});
```

### JAVA 操作 MongoDB
#### 安装MongoDBJDBC驱动程序
```
<dependency>
	<groupId>org.mongodb</groupId>
	<artifactId>mongo-java-driver</artifactId>
	<version>3.12.7</version>
</dependency>
```

#### 连接数据库
>不通过认证连接数据库（这里的 "localhost" 表示连接的服务器地址，27017 为端口号。）   
>连接到数据库（这里的 "db_stus" 表示数据库名，若指定的数据库不存在，mongoDB将会在你第一次插入文档时创建数据库。）
```
// 连接数据库  
MongoClient mongoClient = new MongoClient("localhost", 27017);  
// 选择或者创建数据库  
MongoDatabase mongoDatabase = mongoClient.getDatabase("students");
// 选择或者创建集合  
collection = mongoDatabase.getCollection("students");
```

#### 对数据库进行操作
##### 插入文档
>插入一个文档，使用 MongoCollection 对象的insertOne()方法，该方法接收一个 Document 对象作为要插入的数据  
```JAVA
// 创建Document对象，用于存储数据  
Document document = new Document();  
// 向Document对象中添加数据  
document.put("name", "张三");  
document.put("age", 18);  
document.put("sex", "男");  
// 将Document对象插入到集合中  
collection.insertOne(document);  
// 输出插入成功信息  
System.out.println("数据插入成功");
```
>插入多个文档，使用 MongoCollection 对象的 insertMany() 方法，该方法接收一个 数据类型为Document 的 List 对象作为要插入的数据
```JAVA
// 创建Document对象，用于存储数据  
Document doc1 = new Document();  
// 向Document对象中添加数据  
doc1.put("name", "李四");  
doc1.put("age", 18);  
doc1.put("sex", "女");  
  
Document doc2 = new Document();  
// 向Document对象中添加数据  
doc2.put("name", "王二");  
doc2.put("age", 18);  
doc2.put("sex", "男");  
  
List<Document> documents = new ArrayList<Document>();  
documents.add(doc1);  
documents.add(doc2);  
  
// 将Document对象插入到集合中  
collection.insertMany(documents);  
// 输出插入成功信息  
System.out.println("数据插入成功");
```

##### 修改文档 
> 修改单个文档，使用 MongoCollection 对象的 updateOne() 方法，该方法接收两个参数，第一个数据类型为 Bson 的过滤器筛选出需要修改的文档，第二个参数数据类型为 Bson 指定如何修改筛选出的文档。然后修改过滤器筛选出的第一个文档。 
```JAVA
// 准备过滤条件  
Bson eq = Filters.eq("name", "张三");  
// 准备更新内容  
UpdateResult updateResult = collection.updateOne(eq, new Document("$set", new Document("sex", "女")));  
System.out.println(updateResult);  
System.out.println("数据更新成功");
```

> 修改多个文档，使用 MongoCollection 对象的 updateMany() 方法，该方法接收两个参数，第一个数据类型为 Bson 的过滤器筛选出需要修改的文档，第二个参数数据类型为 Bson 指定如何修改筛选出的文档。然后修改过滤器筛选出的所有文档。 
```JAVA
// 准备过滤条件  
Bson eq = Filters.eq("age", 18);  
// 准备更新内容  
UpdateResult updateResult = collection.updateMany(eq, new Document("$set", new Document("age", 20)));  
if (updateResult.getMatchedCount() > 0) {  
System.out.println("数据更新成功");  
}
```

> 多条件的修改  
> gte：匹配大于或等于指定值的值，lte：匹配是小于或等于规定值的值
```JAVA
// 准备过滤条件  
Bson eq1 = Filters.eq("age", 18);  
Bson eq2 = Filters.eq("age", 20);  
// 准备更新内容  
UpdateResult updateResult = collection.updateMany(Filters.and(eq1, eq2), new Document("$set", new Document("age", 20)));  
if (updateResult.getMatchedCount() > 0) {  
System.out.println("数据更新成功");  
}
// 准备过滤条件  
Bson eq1 = Filters.eq("age", 18);  
Bson eq2 = Filters.eq("age", 20);  
// 准备更新内容  
UpdateResult updateResult = collection.updateMany(Filters.or(eq1, eq2), new Document("$set", new Document("age", 20)));  
if (updateResult.getMatchedCount() > 0) {  
System.out.println("数据更新成功");  
}
```

##### 删除文档
>删除与筛选器匹配的单个文档，使用 MongoCollection 对象的 deleteOne()  
```
MongoCollection<Document> collection = db.getCollection("stu");
DeleteResult deleteOne = collection.deleteOne(new Document("name","李四"));
System.out.println(deleteOne);
```
>删除与筛选器匹配的所有文档，使用 MongoCollection 对象的 deleteMany()  
```
DeleteResult deleteOne = collection.deleteMany(new Document("name","李四"));
```

##### 查询文档
>使用 MongoCollection 对象的 find() 方法，该方法有多个重载方法，可以使用不带参数的 find() 方法查询集合中的所有文档，也可以通过传递一个 Bson 类型的 过滤器查询符合条件的文档。这几个重载方法均返回一个 FindIterable 类型的对象，可通过该对象遍历出查询到的所有文档。
- 查询集合中的所有文档
```
FindIterable<Document> find = collection.find();
MongoCursor<Document> iterator = find.iterator();
while(iterator.hasNext()) {
	System.out.println(iterator.next());
}
mc.close();
```
- 指定过滤器查询
```
Bson eq = Filters.regex("name", "晴");
FindIterable<Document> find = collection.find(eq);
MongoCursor<Document> iterator = find.iterator();
while(iterator.hasNext()) {
	System.out.println(iterator.next());
}
```
- 通过 first()
```
//查询出集合中的所有文档
FindIterable<Document> find = collection.find();
//取出查询到的第一个文档
Document first =(Document) find.first();
System.out.println(first);
mc.close();
```
## 题目

### 一、选择题

1. MongoDB 中，以下哪个不是基本的数据类型？ **D**
   - A. String
   - B. Array
   - C. Object
   - D. None

2. 在 MongoDB 中，以下哪个命令用于创建一个新的数据库? **B**
   - A. createDatabase
   - B. use
   - C. newDatabase
   - D. initDatabase

3. MongoDB中的文档模型是什么？**B**
   - A. 关系型
   - B. 非关系型
   - C. 树形
   - D. 图形

4. 在MongoDB中，以下哪个操作用于删除一个文档？ **B**
   - A. delete
   - B. remove
   - C. drop
   - D. erase

5. MongoDB的默认端口号是多少？**B**
   - A. 27015
   - B. 27017
   - C. 3306
   - D. 5432

6. MongoDB中的索引是用来做什么的？**C**
   - A. 存储数据
   - B. 排序数据
   - C. 快速检索数据
   - D. 压缩数据

7. 在MongoDB中，聚合框架的哪个阶段用于过滤数据？ **A**
   - A. $match
   - B. $group
   - C. $sort
   - D. $limit

8. MongoDB支持的事务级别有哪些？ **D**
   - A. 读已提交
   - B. 可重复读
   - C. 串行化
   - D. 所有以上

9. MongoDB的副本集是什么？ **A**
   - A. 一组具有相同数据的服务器
   - B. 一个单独的服务器
   - C. 一个数据库
   - D. 一个文档

10. MongoDB的存储引擎有哪些？ **D**
    - A. WiredTiger
    - B. MMAPv1
    - C. RocksDB
    - D. 所有以上

11. MongoDB是什么类型的数据库？ **B**
   - a) 关系型数据库
   - b) 非关系型数据库
   - c) 分布式数据库
   - d) 内存数据库

12. MongoDB的数据模型是什么？ **B**
   - a) 表格
   - b) 文档
   - c) 键值对
   - d) 图

13. MongoDB的查询语言是什么？ **C**
   - a) SQL
   - b) JSON
   - c) MongoDB查询语言
   - d) NoSQL查询语言

14. MongoDB的主要特点是什么？ **A**
   - a) 高性能
   - b) 可扩展性
   - c) 强一致性
   - d) ACID事务支持

15. MongoDB中的集合类似于关系型数据库中的什么？ **A**
   - a) 表格
   - b) 数据库
   - c) 列
   - d) 主键
### 二、简答题
1. 描述MongoDB中的CRUD操作，并给出每个操作的一个示例命令。

2. 解释MongoDB中的分片是什么，并简述其优点。
   >**分片**是MongoDB的一种水平扩展机制，用于将数据分散存储在多个服务器（或称为分片）上，以提高性能和可扩展性。通过将数据分成多个部分（即“片”），MongoDB能够处理更大的数据集和更高的并发请求。
   
   > 分片的工作原理
   1. **分片键**：在创建分片时，用户需选择一个分片键，以决定文档如何在不同分片中分布。分片键的选择对性能和数据均衡至关重要。  
   2. **数据分布**：数据根据分片键的值被分散到不同的分片。当插入新文档时，MongoDB会根据分片键的值将其存储到特定的分片上。
   3. **路由层**：MongoDB使用一个路由层（mongos）来处理客户端的请求，确定数据存储在哪个分片上，并将请求路由到相应的分片。

   > 优点
   1. **可扩展性**：分片允许数据库横向扩展，可以通过添加更多的分片来处理更大的数据集和更高的并发负载。  
   2. **性能优化**：通过将数据分散到不同的分片上，MongoDB可以并行处理查询和写入操作，从而提高整体性能。  
   3. **高可用性**：分片可以与副本集结合使用，确保在某个分片或节点发生故障时，系统仍然能够正常运行。 
   4. **负载均衡**：MongoDB能够在不同的分片之间均匀分布数据和请求，从而避免某个分片过载。
   5. **灵活性**：用户可以根据需要选择分片键，并在需要时对数据进行动态扩展。

3. 描述MongoDB中的聚合框架，并给出一个使用聚合框架的示例。

### 三、应用题

1. 假设你正在设计一个电子商务网站的数据库，需要存储用户信息、产品信息和订单信息。请设计相应的文档结构，并解释你的设计选择。

2. 编写一个MongoDB脚本，该脚本能够实现以下功能：
   - 连接到MongoDB服务器
   - 选择或创建一个名为“students”的数据库
   - 向“students”集合中插入三个学生文档
   - 查询所有学生的姓名和年龄
   - 更新一个学生的年龄
   - 删除一个学生记录
   - 断开与MongoDB服务器的连接

```java
import java.util.ArrayList;

public class MongoDBWork {
    public static MongoCollection<Document> collection = null;

    public static void main(String[] args) {
        // 连接数据库
        MongoClient mongoClient = new MongoClient("localhost", 27017);
        // 选择或者创建数据库
        MongoDatabase mongoDatabase = mongoClient.getDatabase("students");
        // 选择或者创建集合
        collection = mongoDatabase.getCollection("students");
        MongoDBWork mongoDBWork = new MongoDBWork();
        // 向 students 集合中插入数据
        mongoDBWork.InsertMany();
        // 查询所有学生的年龄和姓名
        mongoDBWork.FindMany();
        // 更新一个学生的年龄
        mongoDBWork.Update();
        // 删除一个学生记录
        mongoDBWork.Delete();
        // 断开与 MongoDB 服务器的连接
        mongoClient.close();
    }

    public void InsertMany() {
        Document document1 = new Document();
        document1.put("name", "张三");
        document1.put("age", 20);
        Document document2 = new Document();
        document2.put("name", "李四");
        document2.put("age", 20);
        Document document3 = new Document();
        document3.put("name", "王二");
        document3.put("age", 20);
        ArrayList<Document> DocumentList = new ArrayList<>();
        DocumentList.add(document1);
        DocumentList.add(document2);
        DocumentList.add(document3);
        collection.insertMany(DocumentList);
    }

    public void FindMany() {
        FindIterable<Document> documents = collection.find();
        for (Document document : documents) {
            System.out.println(document);
        }
    }

    public void Update() {
        UpdateResult updateResult = collection.updateOne(Filters.eq("name", "张三"),
                new Document("$set", new Document("age", "21")));
        System.out.println("更新后的结果: ");
        FindIterable<Document> documents = collection.find(Filters.eq("name", "张三"));
        Document document = documents.iterator().next();
        System.out.println(document);
    }

    public void Delete() {
        DeleteResult deleteResult = collection.deleteOne(Filters.eq("name", "张三"));
        if (deleteResult.getDeletedCount() > 0) {
            System.out.println("删除成功");
        }
    }
}