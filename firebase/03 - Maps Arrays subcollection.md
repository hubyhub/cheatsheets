# Maps, Arrays and Subcollection

### Rules
* size of Documents <1MB
* not more than 20.000 fields
* you cannot get partial documents
  * if you want to keep parts of the document secret you have to put them into a separate document
 * you are billed by the number of reads and writes per document
 * queries find documents from a single collection
 * queries are shallow, the sub-collection is not returned
  * a separate query is needed
 * Arrays are weird

*#### Depending on the Datastructure, firestore lets you run very different kind of queries:

Example with subcollection
```
// dickens_books collectaion
title: "Greate Expectations"

// characters (stored in sub-collection)
 {
 name: "Pips",
 occupation: "Apprentice"
 }
{
 "name": "Pips",
 "occupation": "Apprentice"
}
{
"name": "Estella",
 "occupation": "Socialite"
}

 Possible queries might look like

```
// show me every character in the book "Oliver twist" that is a "doctor" or
// show me every character in the book "great expectations" that starts with a P
collection("dickens_books/great_exp//charactars")
.where(name > "P"I
.where(name < "Q)"


// Collection Group
// for a query like "show me all charactars in all books named "Oliver" we could use collectionGroup query
// You need to setup a CollectionGroup Index for the name field in Firestore console (max 200 groups)
collectionGroup("characters")
 .where("name", "==", "Oliver")

```

#### Example with Map Field (json)
```json
// dickens_books collectaion
{
 "title": "Greate Expectation",
 "chararctars": {
  "Pip: "Aprentice",
  "Estella ": "Socialite",
  "Miss havisham": "Hairess"
 }
}
```

Possible queries might look like
```
// a bit weird
collection("dickens_books/great_exp//charactars")
.where("characters.Pip", ">=" "")
```

Third option is to put the characters in  a top level collection 

* if you are searching  for individual records from a group of data you should put them into a collection



