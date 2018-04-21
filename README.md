## MongoDb Mac Steup

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

#### Create DB

```
use foobar
```

#### Show dbs 

```
show dbs 
```

#### Show Collections

```
show collections
```

#### Insert

```
db.persons.insert({name:"hua"})
db.persons.insert({name:"liang"})
```

#### Query

```
db.persons.find()
db.persons.findOne()
```

#### Update

```
db.persons.update({name:"hua"},{$set:{name:"huahuahua"}})
db.persons.update({name:"huahuahua"},{{age:30}}) // Update
db.persons.update({name:"huahuahua"},{$set:{age:30}}) // Append
```

#### Remove

```
db.persons.remove({name:"liang"})
```

#### Drop Table

```
db.persons.drop();
```

#### Drop Database

```
db.dropdatabase
```

#### Help

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

#### Batch Insert

```
for(var i=0;i<10;i++){
	db.persons.insert({name: i})
}
```

#### Save v.s. Insert

Save would execute update function if *_id* has already exists. Insert would throw an error.

#### Remove

Collection itself and index won't be removed.

```
db.persons.remove()
db.persons.remove({"_id":2}) // where clause
```

#### InsertOrUpdate

```
db.persons.update({_id:5},{_id:5, name:"insertorupdate"}, true)
```

#### Batch Update

```
db.persons.update({name:9},{$set:{name:"hiiiiiiiii"}}, false, true)
```

#### Inc

```
// before
// { "_id" : ObjectId("5ada6c769f2f7b3e66a88870"), "name" : 4 }

db.persons.update({name:4},{$inc:{name:10000000000}})

// after
// { "_id" : ObjectId("5ada6c769f2f7b3e66a88870"), "name" : 10000000004 }
```

#### Push

```
db.persons.insert({name: "array", books:[]})
db.persons.update({name:"array"}, {$push:{books:"java1"}})
db.persons.update({name:"array"}, {$push:{books:"java2"}})
```

#### **$pushAll has been deprecated**

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

#### **Reference a field from the original or intermediary document**

```
db.persons.insert({_id:1, books:[{type:"js",name:"javascript"},{type:"java", name:"java fundamental"},{type:".vb", name:"visual basic"}]})

db.persons.update({"books.type":"java"},{$set:{"books.$.author":"Hua Liang"}})
```

#### AddToSet

```
db.persons.update({_id:1},{$addToSet:{books:{$each:[{type:".vb",name:"visual basic"},{type:"objective c", name:"ios stuff"}]}}})
```

