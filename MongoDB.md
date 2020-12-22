# MongoDB Cheat Sheet

### Connecting to MangoDB

    mongo # connects to mongodb://127.0.0.1:27017 by default
    mongo --host <host> --port <port> -u <user> -p <pwd> # omit the password if you want a prompt
    mongo "mongodb://192.168.1.1:27017"
    mongo "mongodb+srv://cluster-name.abcde.mongodb.net/<dbname>" --username <username> # MongoDB Atlas



### Show all datanase

    show dbs
    
 
### Show current database 

    db

### Create or switch database

    use <database name>

### Drop database

     use <database name> --  use chat
     db.dropDatase() -- this will delete selected database <database name>
     
     
### Create collection

    db.createCollection(<collection name>) -- db.createCollection('user')
    

## CRUD Operation

### Create

**Insert one record**

Since mongodb is schema less we can create following records with no problem. 
Upon creation of record `_id` is automatically created with each record

    db.user.insertOne({name:"Rajesh", age: 33})
    db.user.insertOne({name:"Rajan", age: 32, address: "Kathmandu Nepal"}) 
    
**Insert multiple record**

    db.user.insert([{name:"gita"}, {name:'rama"}]) -- ordered bulk insert
    db.user.insert([{name:"11"}, {name:"444"}, {name: "435"}], {ordered: false})


###  Read

    db.user.find() -- get all records from user collection
    db.user.find().pretty() -- get all records from user collection in json format
    db.user.findOne() -- get one record only
    
   
   
 [More about the mangoDB](https://developer.mongodb.com/quickstart/cheat-sheet)
