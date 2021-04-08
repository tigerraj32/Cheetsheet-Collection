
Node.js `Mongoose` os a MongoDB object Modeling tool designed to work in an asynchronos enviroent.


## Installation

> npm install mongoose

## Using Mongoose

```javascript

var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/tutorialkart');
 
var db = mongoose.connection;
 
db.on('error', console.error.bind(console, 'connection error:'));
 
db.once('open', function() {
  console.log("Connection Successful!");
});
```

- ` mongoose.connection` provides connection object to mongoDB

We can connect to mongoDB by following method also

```javascript
var mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/tutorialkart')
        .then(()=>{console.log("Connection Successful")})
        .catch((error)=> {console.log("Connection error", error)})
```


## Define Schema 

  Let's assume we are representing book object, so first create book.js

```javascript

var BookSchema = new mongoose.Schema({
  name: String,
  price: Number,
  quantity: Number
});

BookSchema.pre('save', ()=>{
    console.log('[Save Operation] About to save user object')
})

BookSchema.post('save', ()=>{
    console.log('[Save Operation] user object saved')
})

module.exports = mongoose.model('users', UserSchema)

// module.exports = mongoose.model('users', UserSchema, <collection name>)

```

We can additionally pass the collection name,  so that documet transaction is made on specific collection


## CRUD Operation

### Crate Single Document

```javascript

var book = new Book({ name: 'Introduction to Mongoose', price: 10, quantity: 25 });

book1.save(function (err, book) {
    if (err) return console.error(err);
        console.log(book.name + " saved to bookstore collection.");
    }); 

```
If we need to implement callback upon success or failure consider following

```javascript

const saveOperation = async ()=>{
    var book = new Book({ name: 'Introduction to Mongoose', price: 10, quantity: 25 });
    let operation = new Promise(async (resolve, rejects) => {
        try{
            var result = await book.save()
            resolve(result)
        }catch(error){
            reject(error)
        }
    }
}

//Now 

saveOperation()
    .then((result)=> {console.log("saved book: ", book)})
    .catch(error =>{console.log("error saving book: ", error)})
```


### Single Multiple Document

>  Model.collection.insert(docs, options, callback)

- doc : array of document to be inserted
- option: optional configuration obkect
- callback: callback function for success and error


```javascript
 // documents array
    var books = [{ name: 'Mongoose Tutorial', price: 10, quantity: 25 },
                    { name: 'NodeJS tutorial', price: 10, quantity: 5 },
                    { name: 'MongoDB Tutorial', price: 20, quantity: 2 }];
 
    // save multiple documents to the collection referenced by Book Model
    Book.collection.insert(books, function (err, docs) {
      if (err){ 
          return console.error(err);
      } else {
        console.log("Multiple documents inserted to Collection");
      }
    });
```

### Read

[Refrence link](https://mongoosejs.com/docs/queries.html)

There are several ways we can read from mongodb

- find(filter,projection, options, callback)
  - filter: filter criteria
  - projection: optional fields to return, see Query.prototype.select()
  - options: optional see Query.prototype.setOptions()
  
- findById()
- findOne()

```javascript
//To get all the document
    var books = await Book.find().lean()
```
- use .lean(), Mongoose returns plain JSON objects instead of memory and resource heavy documents. Makes queries faster and less expensive on your CPU, too.


```javascript
    

    //Book whose price is 10
    var books = await Book.find({price: 10})
    //Books whose price is 10 and quantity > 5
    var books = await Book.find({price: 10, price: { $gte: 5 }})
    //Books whose name is Mongoose Tutorial and return price and it quantity only
    const books = await Book.find({name: /Mongoose Tutorial/}, 'price quantity')

    //To get book that match the selected ID
    var book = await Book.findById({_id: <someid>})


```

### Update

Mongoose has 4 different ways to update a document. Here's a list:

- Document#save()

```javascript
    var book = await Book.find({name:"Mongoose Tutorial"})
    book.price = 100
    await book.save()
```

- Model.updateOne() and updateMany()

```javascript
    // Update the document using `updateOne()`
    await Book.updateOne({ name: 'Mongoose Tutorial' }, {
        price: 2oo
    });
```
- Document#updateOne()
  
```javascript
  // Load the document
    const book = await Book.findOne({ name: 'Mongoose Tutorial' });
    onst update = { price: 100 };
    await book.updateOne(update);
```

- Model.findOneAndUpdate()

```javascript
const book = await Book.findOneAndUpdate(
  { name: 'Mongose Tutorial' },
  { price: 100}, //upate price to 100
  // If `new` isn't true, `findOneAndUpdate()` will return the
  // document as it was _before_ it was updated.
  { new: true }
);

book.price; 
```




- [Refrence Link1: www.tutorialkart.com](https://www.tutorialkart.com/nodejs/mongoose/)