# Basics
Firestore is a "horizontally scaling" nosql document database in the cloud
[What is a NoSQL Database? How is Cloud Firestore structured? | Get to know Cloud Firestore #1](https://www.youtube.com/watch?v=v_hR4K4auoQ&list=PLl-K7zZEsYLluG5MCVEzXAQ7ACZBCuZgZ&ab_channel=Firebase)

### Traditional relational SQL
* every table has its on schema, that means every row is very strictly defined
* you have a specific set of columns taht you can add per row
* and every column has very strict rules about what data-type goes in there
* because of this scheme you typically store 1 type of objet per table
* relations between table are created with another column a "foreign key"

### noSQL 
* no tables
* usuaylly they are based on key value, collection of json, or tree,...
* all usually are schema-less. 
  * thaat means there are no database restrictions about what data you can put in
* no sql: 
  * if you need object from 3 parts of the database you would need to do 3 different database requests
  * but that should not be the way of thinking, you should put the objects in places where you need them
  * that could mean you put duplicate data in duplicate places
  * example separate collection for Restaurant and Reviews means 2 queries and users..
  * and add a user collection:.. now tis not easy to to get the users of the reviews. its ok to duplicate content. and keeping it in sync when a "write" happens

### Firestore
 * a bit more organized, not just JSON
 * consists of "documents" and "collections"
 * documents are similar to JSON objects or dictionies.
   * they consist of key value pairs, called "fields"
   * the values of fields can be any type of data: number, string, boolean, binary data, to smaller json looking data called "MAPS"
 * collections are collections of documents

###  Rules for Collections and Documents
 * collections can only contain documents.. nothing else
 * documents size can be max 1MB
 * a document cannot contain another documents
 * documents can point to sub-collections, but not to other documents directly
 * Example documents in a USERS-collection can point to subcollection "Workouts"
 * the root of a firestore tree can only contain collections

```javascript
firestore.collection(...).document(...).collections(...).document(...)
```



