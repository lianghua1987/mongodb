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

