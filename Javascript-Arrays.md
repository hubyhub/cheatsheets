# Arrays in Javascript

### Find Object by Id
```javascript
myArray = [{'id':'13','foo':'bar'},{'id':'55','foo':'bar'}];
myArray.find(x => x.id === '13')
```

### Erstellen
```javascript
var arr = []  			// empfohlen
var arr = [1,2,3,4];
var arr = new Array();
arr[1] 			// gibt "2" zurück
```

### length
```javascript
arr.length(); 				// liefert length zurück
arr.length = 3; 			// beschneidet array, alles nach 3.pos ist weg.
```
 
### push 
```javascript
// does mutate the original array ( see concat for no mutation)
arr.push("myString");		// fügt element an LETZTER Stelle ein und liefert neue length zurück
arr.push("s1", "s2");		// mehrere Elemente können gleichzeitig eingefügt werden
arr.push();			// liefert nur length zurück
```

### pop 
```javascript
arr.pop();			// ENTFERNT das LETZTE Element und liefert es zurück
```

### unshift
```javascript
arr.unshift("vorne");		// fügt Element an ERSTER Stelle ein und liefert neue length zurück
arr.unshift("s1", "s2");	// mehrere Elemente können gleichzeitig eingefügt werden
arr.unshift(); 			// liefert nur length zurück
```

### shift
```javascript
arr.shift(); 			// ENTFERNT ERSTES Element und liefert es zurück
```

### indexOf
[3,"3","s1",2, "s1"]
```javascript
arr.indexOf("s1");			// liefert index des ERSTEN Vorkommnis von "s1"
arr.lastIndexOf("s1");		// liefert index des letzten Vorkommnis von "s1"
arr.indexOf("s1", 3);		// der zweite Parameter bestimmt den ANFANGS-index ab dem die Suche beginnen soll.
arr.indexOf("3");			// ACHTUNG: zwischen 3 und "3" wird unterschieden
```

```javascript
[{a:1},{b:2}].indexOf({b:2})	// ACHTUNG: sind 2 unterschiedliche Objekte, wird hier nicht gefunden 
var myObj = {a:1};
var yourObj = {b:2};
var arr2 = [myObj,yourObj];
arr2.indexOf(yourObj);			// Wird gefunden! (1)
// indexOf ist type und instanz-sensitiv
```


### String.split
```javascript
 "13 13, 12 32, 23".split(", ");        // ["13 13", "12 32", "23 3"]

// konvertiert String in Array

```

### slice - slice(start?, end?)
```javascript
arr.slice();	// erzeugt eine copy von arr.   Array "arr" bleibt unverändert. 
arr.slice(0);	// erzeugt eine copy von arr. 
arr.slice(3);	// erzeugt eine copy von arr ab index 3. Array "arr" bleibt unverändert.
arr.slice(3,4);	// erzeugt eine copy von arr mit den werten zwischen index 3 und 4. Array "arr" bleibt unverändert. 
```

### splice - splice(start, deleteCount,...items) 
```javascript
arr.splice(0);	  // entfernt alle elemente aus dem array. liefert neues array mit entfernten elementen zurück. "arr" hat keine Elemente mehr.
arr.splice(3);	  // entfernt alle elemente ab index 3, liefert neues array mit entfernten elementen zurück
arr.splice(2,3);  // entfernt ab index 2 die nächsten 3 elemente, liefert neues array mit entfernten elementen zurück
arr.splice(2,3, "newEl1", "newEl2");  // entfernt ab index 2 die nächsten 3 elemente, und fügt an der Stelle die neuen Elemente ein.

delete arr[3]	// ändert Element an Index 3 zu "undefined". Length des Arrays bleibt unverändert.
```

### concat  
```javascript
// the concat() method does not mutate the original array
var arr1, arr2, arr3;
arr1 = ["Apfel","Banane"];
arr2 = ["Melone", "Pfirsich"];
arr3 = arr1.concat(arr2); 		// ["Apfel","Banane","Melone", "Pfirsich"]
```

### join 
```javascript
var fruits,myString;
fruits = ["Apfel","Banane","Melone", "Pfirsich"];
myString = fruits.join();		// "Apfel,Banane,Melone,Pfirsich"
```

### forEach
```javascript
[1,2,3,4].forEach( callbackFunction, this )
[10,20,30,40].forEach( function(el, index, arr){
	console.log(el);	//liefert Elemente (10,20,30,40)
	console.log(index); // liefert index (0,1,2,3)
	console.log(arr);	// liefert 4x [10,20,30,40]
});
```

### sort
```javascript
var ascending, descending, fruits;

fruits = ["Banana", "Orange", "Apple", "Mango"];
fruits.sort();		// default ist aufsteigend. (string oder zahl) ["Apple", "Banana", "Mango", "Orange"]
// ACHTUNG: 
[2, 10, 5].sort()   // ergibt [10, 2, 5] sort() sortiert auch Zahlen alphabetisch.
fruits.reverse();	// ["Orange", "Mango", "Banana", "Apple"]

// eigene compareFunction:
ascending = [99,1,3,5,6,8,9,5,3].sort(function(a,b){
	return a-b; 				// [1, 3, 3, 5, 5, 6, 8, 9, 99]
});

descending = [99,1,3,5,6,8,9,5,3].sort(function(a,b){
 return b-a; 				// [99, 9, 8, 6, 5, 5, 3, 3, 1]
});
```

### map
ändert die Werte der Elemente des Arrays
und liefert neues Array zurück
```javascript
var newArr;
newArr = [10,20,30,40].map( function(el, index, arr){
	return el*el;		// liefert [100, 400, 900, 1600]
})

// passing parameters
var arr = [1, 2, 3, 4, 5];

function multiplyBy(scale) {
    return function(num){
        return num * scale;
    }
}
console.log( arr.map( multiplyBy(4) ));

```

### filter
filtert bestimmte Elemente aus dem Array heraus.
Liefert neues Array zurück
```javascript
[10,20,30,40].filter( function(el, index, arr){
	return el>20;		// [30,40] 
})						

["Happy", 0, "New", "", "Year", false].filter(Boolean);
["Happy", "New", "Year"] 			// liefert nur die truthy Werte zurück
```


### some
Liefert true oder false zurück abhängig davon  
ob zumindest 1 Element im Array die Bedingung erfüllt

```javascript
[10,20,30,40].some( function(el, index, arr){
	return el>20;		//true
})

[10,20,30,40].some( function(el, index, arr){
	return el>200;		//false
})
```

### every
Liefert true oder false zurück je nachdem ob
*ALLE* Elemente im Array die Bedingung erfüllen

```javascript
[10,20,30,40].every( function(el, index, arr){
	return el>20;		//false
})

[10,20,30,40].every( function(el, index, arr){
	return el>2;		//true
})
```

### reduce
Akkumuliert die Werte des Arrays zurückgeliefert wird eine Zahl.
The reduce() method applies a function against an accumulator and each value of the array (from left-to-right) to reduce it to a single value.

```javascript
[10,20,30,40].reduce( function(sum, el, index, arr){
	return sum+el;		// 100
});

[10,20,30,40].reduce( function(sum, el, index, arr){
	return sum+el;		// 100+10+20+30+40 = 200
}, 100);


["hello ", , ,,,,false,,"javascript"].reduce(function(len,el){
	return len+1; 	// 3
},0);

```


###  Array überprüfen
ACHTUNG: 
```javascript
Array instanceof Object  // Array ist Instanz von Object 
true

typeof []				// type von Array ist Object 
"object"

typeof {} 				// object ist auch "object"
"object"
			
Darum so überprüfen:
Array.isArray({});		// false		
Array.isArray([]);		// true
```

### Array-like in Array ändern
```javascript
var arr1 = Array.prototype.slice.call(arguments); // array-like object in Array ändern, oder...
var arr2 = Array.from(polygon.points); 
```	
### Argumente von Funktionen
```javascript
function foo(){
 console.log(arguments, 'len', arguments.length, '2nd', arguments[1]); 
}
foo(1,3,4,5); 			//[1, 3, 4, 5] "len" 4 "2nd" 3


function foo(){
 var args = Array.prototype.slice.call(arguments); // array-like object in Array ändern
 // kurzform: [].slice.call(arguments, 1); // ab erstem argument
 args.push(5);
 console.log(args, 'len', args.length, '2nd', args[1]);
}
```			
			
