# MongoDb

### Summary

- **Shell is js engine!!!**

#### Install

```
brew update
brew install mongodb
mkdir /Users/hliang/mongoDBData //DB location
cd  /usr/local/Cellar/mongodb/3.6.4/bin
mongod --dbpath /Users/hliang/mongoDBData

```

#### Setup system variable

```
vim ~/.bash_profile 
export PATH=$PATH:/usr/local/Cellar/mongodb/3.6.4/bin
```

#### Add shell.sh

```
vim shell.sh
mongod --dbpath /Users/hliang/mongoDBData
chmod 700 shell.sh
```

## Commands

##### Create DB

```
use foobar
```

##### Show dbs 

```
show dbs 
```

##### Show Collections

```
show collections
```

##### Insert

```
db.persons.insert({name:"hua"})
db.persons.insert({name:"liang"})
```

##### Query

```
db.persons.find()
db.persons.findOne()
```

##### Update

```
db.persons.update({name:"hua"},{$set:{name:"huahuahua"}})
db.persons.update({name:"huahuahua"},{{age:30}}) // Update
db.persons.update({name:"huahuahua"},{$set:{age:30}}) // Append
```

##### Remove

```
db.persons.remove({name:"liang"})
```

##### Drop Table

```
db.persons.drop();
```

##### Drop Database

```
db.dropdatabase
```

##### Help

```
db.help()
```

#### API

http://mongodb.github.io/mongo-java-driver/

#### Naming Convention

- Small case
- 64 chars maximum
- Can't be *<u>admin</u>*, *<u>local</u>* or *<u>config</u>*

#### MongoDb js engine

```
function insert(object){
	db.getCollection("persons").insert(object)
};

insert({name:"huahuahua", age: 31});
db.persons.find();
```

#### Gui

MongoDB Compass -https://www.mongodb.com/download-center?jmp=nav#compass

##### Batch Insert

```
for(var i=0;i<10;i++){
	db.persons.insert({name: i})
}
```

##### Save v.s. Insert

Save would execute update function if *_id* has already exists. Insert would throw an error.

##### Remove

Collection itself and index won't be removed.

```
db.persons.remove()
db.persons.remove({"_id":2}) // where clause
```

##### InsertOrUpdate

```
db.persons.update({_id:5},{_id:5, name:"insertorupdate"}, true)
```

##### Batch Update

```
db.persons.update({name:9},{$set:{name:"hiiiiiiiii"}}, false, true)
```

##### Inc

```
// before
// { "_id" : ObjectId("5ada6c769f2f7b3e66a88870"), "name" : 4 }

db.persons.update({name:4},{$inc:{name:10000000000}})

// after
// { "_id" : ObjectId("5ada6c769f2f7b3e66a88870"), "name" : 10000000004 }
```

##### Push

```
db.persons.insert({name: "array", books:[]})
db.persons.update({name:"array"}, {$push:{books:"java1"}})
db.persons.update({name:"array"}, {$push:{books:"java2"}})
```

##### **$pushAll has been deprecated**

```
db.persons.update({type:"array"}, {$pushAll:{books:["java1","java2","java3","java4"]}})

// output
WriteResult({
	"nMatched" : 0,
	"nUpserted" : 0,
	"nModified" : 0,
	"writeError" : {
		"code" : 9,
		"errmsg" : "Unknown modifier: $pushAll"
	}
})

```

Reference a field from the original or intermediary document

```
db.persons.insert({_id:1, books:[{type:"js",name:"javascript"},{type:"java", name:"java fundamental"},{type:".vb", name:"visual basic"}]})

db.persons.update({"books.type":"java"},{$set:{"books.$.author":"Hua Liang"}})
```

##### AddToSet

```
db.persons.update({_id:1},{$addToSet:{books:{$each:[{type:".vb",name:"visual basic"},{type:"objective c", name:"ios stuff"}]}}})
```

##### Search by clause

db.[document_name].find({condition},{key})

```
> db.persons.find()
{ "_id" : ObjectId("5adea16d1b18a98af19af61d"), "name" : "jim", "age" : 25, "email" : "75431457@qq.com", "c" : 89, "m" : 96, "e" : 87, "country" : "USA", "books" : [ "JS", "C++", "EXTJS", "MONGODB" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af61e"), "name" : "tom", "age" : 25, "email" : "214557457@qq.com", "c" : 75, "m" : 66, "e" : 97, "country" : "USA", "books" : [ "PHP", "JAVA", "EXTJS", "C++" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af61f"), "name" : "lili", "age" : 26, "email" : "344521457@qq.com", "c" : 75, "m" : 63, "e" : 97, "country" : "USA", "books" : [ "JS", "JAVA", "C#", "MONGODB" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af620"), "name" : "zhangsan", "age" : 27, "email" : "2145567457@qq.com", "c" : 89, "m" : 86, "e" : 67, "country" : "China", "books" : [ "JS", "JAVA", "EXTJS", "MONGODB" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af621"), "name" : "lisi", "age" : 26, "email" : "274521457@qq.com", "c" : 53, "m" : 96, "e" : 83, "country" : "China", "books" : [ "JS", "C#", "PHP", "MONGODB" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af622"), "name" : "wangwu", "age" : 27, "email" : "65621457@qq.com", "c" : 45, "m" : 65, "e" : 99, "country" : "China", "books" : [ "JS", "JAVA", "C++", "MONGODB" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af623"), "name" : "zhaoliu", "age" : 27, "email" : "214521457@qq.com", "c" : 99, "m" : 96, "e" : 97, "country" : "China", "books" : [ "JS", "JAVA", "EXTJS", "PHP" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af624"), "name" : "piaoyingjun", "age" : 26, "email" : "piaoyingjun@uspcat.com", "c" : 39, "m" : 54, "e" : 53, "country" : "Korea", "books" : [ "JS", "C#", "EXTJS", "MONGODB" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af625"), "name" : "lizhenxian", "age" : 27, "email" : "lizhenxian@uspcat.com", "c" : 35, "m" : 56, "e" : 47, "country" : "Korea", "books" : [ "JS", "JAVA", "EXTJS", "MONGODB" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af626"), "name" : "lixiaoli", "age" : 21, "email" : "lixiaoli@uspcat.com", "c" : 36, "m" : 86, "e" : 32, "country" : "Korea", "books" : [ "JS", "JAVA", "PHP", "MONGODB" ] }
{ "_id" : ObjectId("5adea16d1b18a98af19af627"), "name" : "zhangsuying", "age" : 22, "email" : "zhangsuying@uspcat.com", "c" : 45, "m" : 63, "e" : 77, "country" : "Korea", "books" : [ "JS", "JAVA", "C#", "MONGODB" ] }

> db.persons.find({},{_id:0,name:1,country:1}) // search name, country, skip _id

{ "name" : "jim", "country" : "USA" }
{ "name" : "tom", "country" : "USA" }
{ "name" : "lili", "country" : "USA" }
{ "name" : "zhangsan", "country" : "China" }
{ "name" : "lisi", "country" : "China" }
{ "name" : "wangwu", "country" : "China" }
{ "name" : "zhaoliu", "country" : "China" }
{ "name" : "piaoyingjun", "country" : "Korea" }
{ "name" : "lizhenxian", "country" : "Korea" }
{ "name" : "lixiaoli", "country" : "Korea" }
{ "name" : "zhangsuying", "country" : "Korea" }
```

###### Show columns

```
> db.persons.find({country:'USA'},{name:1,country:1})
{ "_id" : ObjectId("5adea16d1b18a98af19af61d"), "name" : "jim", "country" : "USA" }
{ "_id" : ObjectId("5adea16d1b18a98af19af61e"), "name" : "tom", "country" : "USA" }
{ "_id" : ObjectId("5adea16d1b18a98af19af61f"), "name" : "lili", "country" : "USA" }
```

###### Where

```
// age between 25 - 30
> db.persons.find({age:{$gte:25,$lte:27}},{name:1,country:1,age:1})
{ "_id" : ObjectId("5adea16d1b18a98af19af61d"), "name" : "jim", "age" : 25, "country" : "USA" }
{ "_id" : ObjectId("5adea16d1b18a98af19af61e"), "name" : "tom", "age" : 25, "country" : "USA" }
{ "_id" : ObjectId("5adea16d1b18a98af19af61f"), "name" : "lili", "age" : 26, "country" : "USA" }
{ "_id" : ObjectId("5adea16d1b18a98af19af620"), "name" : "zhangsan", "age" : 27, "country" : "China" }
{ "_id" : ObjectId("5adea16d1b18a98af19af621"), "name" : "lisi", "age" : 26, "country" : "China" }
{ "_id" : ObjectId("5adea16d1b18a98af19af622"), "name" : "wangwu", "age" : 27, "country" : "China" }
{ "_id" : ObjectId("5adea16d1b18a98af19af623"), "name" : "zhaoliu", "age" : 27, "country" : "China" }
{ "_id" : ObjectId("5adea16d1b18a98af19af624"), "name" : "piaoyingjun", "age" : 26, "country" : "Korea" }
{ "_id" : ObjectId("5adea16d1b18a98af19af625"), "name" : "lizhenxian", "age" : 27, "country" : "Korea" }
```

###### in & nin

```
> db.persons.find({country:{$nin:['USA','China']}},{country:1,name:1,_id:0})
{ "name" : "piaoyingjun", "country" : "Korea" }
{ "name" : "lizhenxian", "country" : "Korea" }
{ "name" : "lixiaoli", "country" : "Korea" }
{ "name" : "zhangsuying", "country" : "Korea" }
> db.persons.find({country:{$in:['USA','China']}},{country:1,name:1,_id:0})
{ "name" : "jim", "country" : "USA" }
{ "name" : "tom", "country" : "USA" }
{ "name" : "lili", "country" : "USA" }
{ "name" : "zhangsan", "country" : "China" }
{ "name" : "lisi", "country" : "China" }
{ "name" : "wangwu", "country" : "China" }
{ "name" : "zhaoliu", "country" : "China" }
```

###### or

```
> db.persons.find({$or:[{c:{$gt:85}},{e:{$gt:90}}]},{_id:0, name:1, e:1, c:1})
{ "name" : "jim", "c" : 89, "e" : 87 }
{ "name" : "tom", "c" : 75, "e" : 97 }
{ "name" : "lili", "c" : 75, "e" : 97 }
{ "name" : "zhangsan", "c" : 89, "e" : 67 }
{ "name" : "wangwu", "c" : 45, "e" : 99 }
{ "name" : "zhaoliu", "c" : 99, "e" : 97 }
```

###### Update

```
> db.persons.update({country:'USA'},{$set:{country:"North Korea"}},false, true)
WriteResult({ "nMatched" : 3, "nUpserted" : 0, "nModified" : 3 })
> db.persons.find({},{_id:0, name:1, country:1});
{ "name" : "jim", "country" : "North Korea" }
{ "name" : "tom", "country" : "North Korea" }
{ "name" : "lili", "country" : "North Korea" }
{ "name" : "zhangsan", "country" : "China" }
{ "name" : "lisi", "country" : "China" }
{ "name" : "wangwu", "country" : "China" }
{ "name" : "zhaoliu", "country" : "China" }
{ "name" : "piaoyingjun", "country" : "Korea" }
{ "name" : "lizhenxian", "country" : "Korea" }
{ "name" : "lixiaoli", "country" : "Korea" }
{ "name" : "zhangsuying", "country" : "Korea" }

> db.persons.update({country:'China'},{$set:{gender:'M'}}, false, true)
WriteResult({ "nMatched" : 4, "nUpserted" : 0, "nModified" : 4 })
> db.persons.find({},{_id:0, name:1, country:1, gender: 1});
{ "name" : "jim", "country" : "North Korea" }
{ "name" : "tom", "country" : "North Korea" }
{ "name" : "lili", "country" : "North Korea" }
{ "name" : "zhangsan", "country" : "China", "gender" : "M" }
{ "name" : "lisi", "country" : "China", "gender" : "M" }
{ "name" : "wangwu", "country" : "China", "gender" : "M" }
{ "name" : "zhaoliu", "country" : "China", "gender" : "M" }
{ "name" : "piaoyingjun", "country" : "Korea" }
{ "name" : "lizhenxian", "country" : "Korea" }
{ "name" : "lixiaoli", "country" : "Korea" }
{ "name" : "zhangsuying", "country" : "Korea" }
```

###### null

```
> db.persons.find({gender: {$in:[null]}},{_id:0, name:1, country:1, gender: 1})
{ "name" : "jim", "country" : "North Korea" }
{ "name" : "tom", "country" : "North Korea" }
{ "name" : "lili", "country" : "North Korea" }
{ "name" : "piaoyingjun", "country" : "Korea" }
{ "name" : "lizhenxian", "country" : "Korea" }
{ "name" : "lixiaoli", "country" : "Korea" }
{ "name" : "zhangsuying", "country" : "Korea" }

> db.persons.find({gender: null},{_id:0, name:1, country:1, gender: 1}) // same
{ "name" : "jim", "country" : "North Korea" }
{ "name" : "tom", "country" : "North Korea" }
{ "name" : "lili", "country" : "North Korea" }
{ "name" : "piaoyingjun", "country" : "Korea" }
{ "name" : "lizhenxian", "country" : "Korea" }
{ "name" : "lixiaoli", "country" : "Korea" }
{ "name" : "zhangsuying", "country" : "Korea" }
```

###### Regular Expression

```
> db.persons.find({name:/Li/},{_id:0, name:1, country:1})
> db.persons.find({name:/Li/i},{_id:0, name:1, country:1}) // i: ignore case
{ "name" : "lili", "country" : "North Korea" }
{ "name" : "lisi", "country" : "China" }
{ "name" : "zhaoliu", "country" : "China" }
{ "name" : "lizhenxian", "country" : "Korea" }
{ "name" : "lixiaoli", "country" : "Korea" }
```

###### Array

```
> db.persons.find({books:{$all:['JS','MONGODB']}},{_id:0, name:1, country:1, books:1})
{ "name" : "jim", "country" : "North Korea", "books" : [ "JS", "C++", "EXTJS", "MONGODB" ] }
{ "name" : "lili", "country" : "North Korea", "books" : [ "JS", "JAVA", "C#", "MONGODB" ] }
{ "name" : "zhangsan", "country" : "China", "books" : [ "JS", "JAVA", "EXTJS", "MONGODB" ] }
{ "name" : "lisi", "country" : "China", "books" : [ "JS", "C#", "PHP", "MONGODB" ] }
{ "name" : "wangwu", "country" : "China", "books" : [ "JS", "JAVA", "C++", "MONGODB" ] }
{ "name" : "piaoyingjun", "country" : "Korea", "books" : [ "JS", "C#", "EXTJS", "MONGODB" ] }
{ "name" : "lizhenxian", "country" : "Korea", "books" : [ "JS", "JAVA", "EXTJS", "MONGODB" ] }
{ "name" : "lixiaoli", "country" : "Korea", "books" : [ "JS", "JAVA", "PHP", "MONGODB" ] }
{ "name" : "zhangsuying", "country" : "Korea", "books" : [ "JS", "JAVA", "C#", "MONGODB" ] }
```

###### size

```
> db.persons.find({books:{$size:4}},{_id:0, name:1, country:1, books:1})
{ "name" : "jim", "country" : "North Korea", "books" : [ "JS", "C++", "EXTJS", "MONGODB" ] }
{ "name" : "tom", "country" : "North Korea", "books" : [ "PHP", "JAVA", "EXTJS", "C++" ] }
{ "name" : "lili", "country" : "North Korea", "books" : [ "JS", "JAVA", "C#", "MONGODB" ] }
{ "name" : "zhangsan", "country" : "China", "books" : [ "JS", "JAVA", "EXTJS", "MONGODB" ] }
{ "name" : "lisi", "country" : "China", "books" : [ "JS", "C#", "PHP", "MONGODB" ] }
{ "name" : "wangwu", "country" : "China", "books" : [ "JS", "JAVA", "C++", "MONGODB" ] }
{ "name" : "zhaoliu", "country" : "China", "books" : [ "JS", "JAVA", "EXTJS", "PHP" ] }
{ "name" : "piaoyingjun", "country" : "Korea", "books" : [ "JS", "C#", "EXTJS", "MONGODB" ] }
{ "name" : "lizhenxian", "country" : "Korea", "books" : [ "JS", "JAVA", "EXTJS", "MONGODB" ] }
{ "name" : "lixiaoli", "country" : "Korea", "books" : [ "JS", "JAVA", "PHP", "MONGODB" ] }
{ "name" : "zhangsuying", "country" : "Korea", "books" : [ "JS", "JAVA", "C#", "MONGODB" ] }
```

$size

$size can't work with other operators, in this case, we add an extra column called "size"

```
> db.persons.update({},{$set:{size:4}},false, true)
WriteResult({ "nMatched" : 11, "nUpserted" : 0, "nModified" : 10 })
> db.persons.update({name:'jim'},{$push:{books:'REDIS'},$inc:{size:1}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```

