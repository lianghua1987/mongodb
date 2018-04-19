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

## Knowledge

#### Create DB

```
use foobar
```

#### show dbs 

```
show dbs 
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
```

