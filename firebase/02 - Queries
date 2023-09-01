# Queries
Queries can only be run to retrieve data from 1 collection/subcollection

NoSQL philosophy prioritze reads over writes

### Collection Group Query (new since 2019)
* a query that spans multiple collections, is now supported
* in firebase console you need to set this up. 
* EXAMPLE. RATINGS collection and REVIEW COLLECTION. firebase will index them as one giant collection
* Attention:
  * limited to 200 collection group queries
  * these queries look for collections of the same name regardless where they appear in the database
     * collection "reviews" should be used only once --> "support-team-reviews" "restaurant-reviews"

### Rules
* there are no joins like in sql
  * no joins from one collection to grab data from another collection
  * example no:  "fetch me data from users collection  for users that have written "reviews" from e a specific "restaurant"
* queries that you run should be based on a quality, or a greater or less comparison of 1 or more fields in the document
  * "find me all restaurants from collection where field city == "san francisco"
* the results from a query are "shallow"
 * you would only get documents of 1 collection and not other subcollection
 * you would need to to do multiple requests or not break it up into to multiple collections
* the time it takes to run a query is proportional to the number of results you get back 
 * and NOT the number of documents you are searching through
 * firebase creates an index for every field of a document, EVEN for fields of a MAP! (JSON stored in document)
* not possible in firestrore 
 * "Find name like "%Taqueriea%"
 * OR queries
 * NOT queries != but you can add null values that sovles the you cannot query for fields that dont exist problem

### Compound Fields or actually "Composite Indexes"
* needs to be created manually
* kind of stores fields together automatically on index-level:  zip code and rating: 1040_5.0 and puts them into same field
* then you can find all restaurants of a specific rating in specific zip code
* 2 ways to create them:
  * manually goto firebase console
  * trough a query that throws error because composite index does not exist. shows url to firebase console you follow it and click "create"


  

