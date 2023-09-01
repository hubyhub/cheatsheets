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

Example subcollection

dickens_books collectaion
```json
{
 "title": "Greate Expectation"
}

```

characters (sub-collection)
```json
 {
 "name": "Pips",
 "occupation": "Apprentice"
}
```

```json
 {
"name": "Estella",
 "occupation": "Socialite"
}
```

Example with Map Field (json)
dickens_books collectaion
```json
{
 "title": "Greate Expectation",
"chararctars": {
 "name": "Pips",
 "occupation": "Apprentice"
}
}


