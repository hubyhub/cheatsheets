# Security Rules

https://www.youtube.com/watch?v=eW5MdE3ZcAw&ab_channel=Firebase
https://firebase.google.com/docs/firestore/security/get-started

#### rules_version = '2';
As of May 2019, version 2 of the Cloud Firestore security rules is now available.
Version 2 of the rules changes the behavior of recursive wildcards {name=**}.
You must use version 2 if you plan to use "collection group queries". 
You must opt-in to version 2 by making `rules_version = '2';`
the first line in your security rules

```javascript
rules_version = '2';
service cloud.firestore {
  // basically all documents fall into following path  
  match /databases/{database}/documents {  
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

####  match /databases/{database}/documents
* path can be nested within /documents folder
* the rules do not cascade, to the children
* best practice add rule-path with wildcards 'match/restaurants/{restaurantID}'

#### Wildcards
there are 2 types of Wildcards:
* {database} = matches all databases and stores it in variable called "database"
* {wildcard=**} = "rest-of-the-document-wildcard", match the rest of the path and store it in variable 
  * example match /users/{restOfPath=**} {} matches every document in 'users'-collection,and also every document in every subcollection 
  * the value changes and is the rest of the document path

#### Usecases for recursive wildcards:
* make database temporarily completely open
  ```
  match /databases/{database}/documents {    
      match /{document=**} {
        allow read, write;
      }
    }
  ```
* when you want to cover a document and all its subcollections by the same rules
  ```
  match /databases/{database}/documents {    
      match /users/{restOfPath=**} {
        allow read;
      }
  }
  ```

### How to allow or block request
You need to set a boolean value for each action

* get: when a user requests a specific document
* list: when a user runs a query that might return this document
* create: when a user creates a new document
* delete: delete a document
* update: updating an existing document

* get & list can be comined into a single action: **read**
* create delete and update  can be comined into a single action: **write**

#### Example
```javascript
// everyone can read documents in publicDoc
service cloud.firestore {  
  match /databases/{database}/documents {  
    match /publicDocs/{docId} {
      // or allow read: if true;
      allow read; 
    }
  }
}
```

```javascript
// readonly documents, no-one should be able to modify
service cloud.firestore {  
  match /databases/{database}/documents {  
    match /publicDocs/{docId} {     
      allow write: if false; 
    }
  }
}
```


   


  
