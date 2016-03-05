MONGODB
=========

* ``mongod.exe`` ist der Server der im Hintergrund läuft
* ``mongo.exe``  ist der Client für die Queries 

* beim starten von mongod.exe muss man ein *data-directory* angeben. In diesem Ordner wird die Datenbank gespeichert.
    *  Das default-Verzeichnis ist  "_C:\data\db_"
    *  Ein benutzerdefiniertes *data-directory* definiert man mit:     
>     C:\Program Files (x86)\MongoDB\Server\3.0\bin>mongod.exe --dbpath C:\Data\mongodb
	
* die Datenbank horcht default-mäßig auf Port 27017

* Um auf MongoDB mit PHP zugreifen zu können, muß man die [PHP Treiber](http://docs.mongodb.org/ecosystem/drivers/php/) installieren    
(Bei mir hat nur version php_mongo-1.5.1.zip von hier [https://s3.amazonaws.com/drivers.mongodb.org/php/index.html](https://s3.amazonaws.com/drivers.mongodb.org/php/index.html) funktioniert.)  

* Plugin für Netbeans "Tools-->Plugins --> NBMongo" 
  In Netbeans unter dem Tab "Services" taucht dann die MongoDB auf

* Mongo-Shell in Netbeans:   
  [https://github.com/le-yams/nbmongo/wiki/MongoNativeTools](https://github.com/le-yams/nbmongo/wiki/MongoNativeTools)   
  (leider nur 1 zeilige Befehle?)

### Grundlagen
  
* Eine _Datenbank_ besteht aus verschiedenen _collections_
* _collections_ ist eine Gruppe von gleichen Daten. z.B. (Users, Players, Employees,...)
* In _collections_ befinden sich _documents_ 
* _documents_ sind im wesentlichen JSON-Objekte mit _"name-value"_-Paaren  
* MongoDB fügt *automatisch* eine ObjectId  als UNIQUE-Identifier zu jedem _document_  ``"_id" : ObjectId("r23jlasd982as90fa")``   
* Die _id wird aus performance-gründen automatisch indiziert.

### Befehle
`show dbs` listet alle Datenbanken (_'local'_ ist default-db und wird automatisch erzeugt)   
`use names` erzeugt eine neue Datenbank "names", und/oder setzt den scope auf "names"   
`db` listet die aktuelle DB (aktueller scope)   
`show collections` listet alle _collections_ in der aktuellen DB   
`db.mynames.insert({name:'superman'})` erzeugt eine neue _collection_ "mynames" in der aktuellen DB und fügt ein _document_ ein   
`db.mynames.insert([ {},{},{} ])`      in Form eines Arrays können mehrere _documente_ gleichzeitig eingefügt werden.   
`db.mynames.remove()`                  löscht alle _documents_ in der collection "mynames"  
`db.mynames.remove({"_id" : ObjectId("123456abc")})` entfernt das _document_ mit dieser _id von der collection   
`db.mynames.update({"_id" : ObjectId("123456abc")},{name:'batman'} )`  überschreibt das zur "_id" gehörende _document_ mit dem überlieferten _document_   
`db.mynames.drop()` löscht die komplette _Collection_ 
`databasename.dropDatabase()` 	löscht die DB mit dem namen "databasename"       

#### Find und Suchqueries
`db.mynames.find()`                    listet ALLE _documente_ in der _Collection_ "mynames". Suchparameter sind möglich.   
`db.mynames.find().pretty()`           liefert eine schön-formatierte Ausgabe  
`db.mynames.findOne()`                 listet das erste _document_ in der Collection  
`db.mynames.find( {"name": "batman" } )` liefert alle _documente_ bei denen "name" gleich "batman" ist   
`db.mynames.find( {"name": "batman", "job" : "superhero" } )` **UND** - Verknüpfung: Liefert alle _documente_ mit namen "batman" und job "superhero".   
`db.mynames.find( { $or: [ {"name": "batman"}, {"job" : "superhero"} ] )` **OR**-Verknüpfung: wird mit $or:[ ] erzeugt.   
`db.mynames.find( {"age" : {$gt:30} } )`  **größer als**: liefert alle _documente_ bei denen age *größer* als 30 ist.   
`db.mynames.find( {"age" : {$lt:30} } )`  **kleiner als**: liefert alle _documente_ bei denen age *kleiner* als 30 ist.    
`db.mynames.find( {"age" : {$gte:30} } )` **größer-gleich als**: liefert alle _documente_ bei denen age größer als oder gleich 30 ist.   
`db.mynames.find( {"age" : {$lte:30} } )` **kleiner-gleich als**: liefert alle _documente_ bei denen age kleiner als oder gleich 30 ist.    
`db.mynames.find( {"age" : {$ne:30} } )`  **nicht**: liefert alle _documente_ bei denen age *nicht* 30 ist.    
`db.mynames.find( {"name": "batman", "_id": 0, "job": 0 } } )` **Exklusion**: "_id" und "job" werden nicht im Ergebnis angezeigt

[Mongodb.org - Find](http://docs.mongodb.org/manual/reference/method/db.collection.find/)


#### Weiter Queries
`db.mynames.find( {"job": "superhero" } ).limit(3)` **Maximum** 3 Ergebnisse werden angezeigt.   
`db.mynames.find( {"job": "superhero" } ).skip(2)` **SKIP** Die ersten 2 Ergebnisse werden *nicht* angezeigt.   
 


### Indizierung
`db.mynames.getIndexes()` zeigt an, welche Indizes man erstellt hat. ("_id" wird standardmäßig indiziert)   
`db.mynames.ensureIndex( {"age":1} )` **ensureIndex**: erstellt aus dem Ergebnis einen Index
`db.mynames.dropIndex( {"age":1} )` **dropIndex**: löscht den Index   


### Aggregation und Gruppen
Mit der **aggretate()** -function erzeugt man eine **Daten-Gruppe**, und kann daraufhin weitere Berechnungen auf dieser Gruppe machen.   

```javascript
    db.users.aggregate({
       $group : {
           _id : "$eyeColor"
           total: {$sum :1 }                 
       }
    })
```    


*  **id:** Gruppiert die *users*-Ergebnisse; In diesem Fall nach der Augenfarben. Das heißt, man bekommt hier mehrere Gruppen zurück, je nachdem wie viele unterschiedliche
Augenfarben es in den *users* gibt.  *eyeColor* ist ein *key* in dem *document*
Nun kann man weitere Berechnungen auf diesen Gruppen machen:   
*  **{ $sum : 1 }** liefert die Summe zurück. Wievele Einträge gibt es? ("1" bedeutet hier "true")   
*  **{ $avg : "$age" }** liefert den Durchschnitt pro Gruppe zurück.
*  **{ $max : "$age" }** liefert Eintrag mit dem höchsten Betrag zurück
 



### Monitoring
`db.mynames.find( {"name": "batman" } ).explain("executionStats")` Liefert **Metadaten** über das Suchergebnis zurück   
z.B **totalDocsExamined: 36**  sagt aus, dass bei dieser Suche 30 Einträge durchsucht werden müssen.    
Ohne Indizierung müssen alle _documente_ durchsucht werden


### Tutorials:

[mongodb.com - Introduction](https://www.mongodb.com/presentations/building-web-applications-mongodb-introduction)    
 

[MongoDB Tutorial for Beginners - 1 - Installing MongoDB](https://www.youtube.com/watch?v=1uFY60CESlM)    
[MongoDB Tutorial for Beginners - 2 - Set Up for JetBrains IDE's](https://www.youtube.com/watch?v=1NP5lLwH9F8)    
[MongoDB Tutorial for Beginners - 3 - Creating Databases and Inserting Data](https://www.youtube.com/watch?v=ynPAkZyH3R8)    
[MongoDB Tutorial for Beginners - 4 - Updating, Removing, and Collections](https://www.youtube.com/watch?v=ifOtpeh-U_0)    
[MongoDB Tutorial for Beginners - 5 - Find and Search Query](https://www.youtube.com/watch?v=q5EY2HRfw5c)    
[MongoDB Tutorial for Beginners - 6 - Indexing](https://www.youtube.com/watch?v=Gazp4GpbcAM)    
[MongoDB Tutorial for Beginners - 7 - Aggregation and Groups](https://www.youtube.com/watch?v=RPv2r4Fms2g)       
