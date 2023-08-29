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

#### Advanced Examples based on
* Request data coming in 
* documents you want to read or write
* some other data located in some other part in you database

Request data = "requestObject" which contains some properties like  the **auth-object** and the  **resource-object**, 

```javascript
  // is signed-in user
  request.auth != null
  // get user-id
  request.auth.uid
  // get users email-adress
  request.auth.token.email
  // is email verified
  request.auth.token.email_verified
```
**ADVANCED: You can also create your own custom auth claims.**

#### Auth
```javascript
service cloud.firestore {  
  match /databases/{database}/documents {  
    match /myCollection/{docId} {
      // users can read this data, but only if they are signed in
      allow read: if request.auth != null; 
    }
  }
}

service cloud.firestore {  
  match /databases/{database}/documents {  
    match /myCollection/{docId} {
      // users with a verified google email adress can read this document
      allow read: if request.auth.token.email.matches('.*google[.]com$') &&
request.auth.token.email_verified;
    }
  }
}

service cloud.firestore {  
  match /databases/{database}/documents {  
    match /myCollection/{docId} {
      // only albert can read document
      allow read: if request.auth.id == 'albert_2345';
    }
  }
}
```

#### request.resource, resource and resource.data
Firestore is schema-less. users can put any kind of data anywhere.
With this security rules you can make firebase a bit more ~"schema-full", by ensuring that certain fields contain certain values, by looking at this object

Following data be used to evaluate the request:

1) "request.resource"-object is the data that you are trying to write
2) "resource" object represents the data that is already in database
3) "resource.data" the data property can be used to access any of the fields within this document  
    * typically you are going to use
      * the `exists()` function, to check whether the documents exist, or
      * the `get()` function, which will return an object which looks very similar to the resource obect, with a .data property
    * **Use Case:** Store a list of users which have access to a specific document within the app
      * editor, admin, superadmin,... while this would work, there is another approach
      * other approach: Auth CLAIMS

```javascript

service cloud.firestore {  
  match /databases/{database}/documents {  
    match /restaurants/{restaurantId} {
      match /restaurants/{restaurantId} {
       // creating a review
       allow create: if request.resource.data.score is number &&
        request.resource.data.score >= 1 &&
        request.resource.data.score <= 5 &&
        request.resource.data.headline is string &&
        request.resource.data.headline.size() >2 &&
        request.resource.data.headline.size() < 200 &&
         // user can only submit a review, if the provided reviewerID is the users actual user ID. 
        request.resource.data.reviewerID == request.auth.uid;

      // updating an existing review:      
      allow update: if request.resource.data.score is number &&
        request.resource.data.score >= 1 &&
        request.resource.data.score <= 5 &&
        request.resource.data.headline is string &&
        request.resource.data.headline.size() >2 &&
        request.resource.data.headline.size() < 200 &&
        // The score cannot be changed
        request.resource.data.score == resource.data.score &&
         // you can only update an existing document, if userId is equal to reviewerID in DB
        resource.data.reviewerID == request.auth.uid;

      // make sure that other users can only see published reviews
      // BUT!!!: security rules cannot be used to filter out published reviews
      // getAllReviews() request is just denied, even if all reviews happen to be public
      // getAllPublicReviews() request which filter state == public, is allowed by security rules
      // this applies to query type of data, where you get a bunch of data. could work if it is just one
      allow read: if resource.data.state == "published" ||
          resource.data.reviewerID = request.auth.uid;
      } 
    }
  }
}


```



  
