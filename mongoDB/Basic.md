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


## SQL vs MongoDB command comparasion

### List available database

>SQL

    show databases
>MongoDB

    show dbs


|SQL|MongoDB|Description|
|----|----|----|
|```show databases```| ```show dbs```|List available database|
|```use {database name}```|```use {database name}```| Select specific database|
|```show database()```|```db```| show current database|
|drop {database name}|```use {database name} ```|Drop database|




