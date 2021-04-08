## SQL vs MongoDB Terminology comparasoin

The following table presents the various SQL terminology and concepts and the corresponding MongoDB terminology and concepts.


|SQL Terms | MongoDB Terms|
|-----|-----|
|Database| Database|
|Table|Collection|
|Row Record|Document|
|Column| Field|
|Index|Index|
|Table Join| $lookup, embedded document|
|Select into new table| $out|
|Merge into table| $merge|
|Union All| $unionWith|


# SQL vs MongoDB Query

>List available database
```js
/*SQL*/
show database

/*MongoDB*/
show dbs
```

>Select database
```js
/*SQL*/
use <database name>

/*MongoDB*/
use <database name>
```


>Show current database
```js
/*SQL*/
show database()

/*MongoDB*/
db
```

>Drop database
```js
/*SQL*/
drop <database name>

/*MongoDB*/
use <database name>
db.dropDatabase()
```


>Create tabe or document
```js
/*SQL*/
create table <tabe name> {
    column1 datatype,
    column2 datatpe,
    column3 datatype
}

/*MongoDB*/
db.createCollection(<collection name>)
```

## SQL VS MongoDB CRUP Operation comparasion

Before making comparasion let consider following data as source of refrence data. Here we have a array of user name and his favourite games names.

Since mongodb is schema less we can create following records with no problem. Upon creation of record _id is automatically created with each record.

Where as in case of SQL we will need to create two seperate table. One for `user` and another for `favourite` games and link  `user` table with `favourite` table via foreign key (create relatoin).

let's assume collection name is `user`
```json
[
  {
    "_id": "606587b365cc824c45cccafd",
    "favourite": [
      {
        "_id": "606587ee65cc824c45cccafe",
        "sportName": "football"
      },
      {
        "_id": "6065885b65cc824c45cccaff",
        "sportName": "table tenis"
      }
    ],
    "name": "rajan"
  },
  {
    "_id": "6065898265cc824c45cccb01",
    "favourite": [
      {
        "_id": "606589a365cc824c45cccb02",
        "sportName": "basket ball"
      },
      {
        "_id": "606589a365cc824c45cccb03",
        "sportName": "cricket"
      }
    ],
    "name": "aarav"
  }
]
```

### Create

>Insert single record
```js
/*SQL*/
insert into user (name) values ('rajan')

/*MongoDB*/
db.user.insertOne({name:'rajan'})
```

>Insert multiple records
```js
/*SQL*/
insert into user (name) values('rajan'), ('aarab')

/*MongoDB*/
db.user.insert([ {name: 'rajan'}, {name:'aarab'}])

```


>Insert multiple records
```js
/*SQL*/


/*MongoDB*/

```


>Insert multiple records
```js
/*SQL*/


/*MongoDB*/

```
    

>Insert multiple records
```js
/*SQL*/


/*MongoDB*/

```
    

>Insert multiple records
```js
/*SQL*/


/*MongoDB*/

```
    

>Insert multiple records
```js
/*SQL*/


/*MongoDB*/

```
    






