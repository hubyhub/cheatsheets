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
  example match /users/{restOfPath=**} {} matches every document in 'users'-collection,and also every document in every subcollection 
  the value changes and is the rest of the document path