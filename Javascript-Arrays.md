# Arrays in Javascript

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

### slice
```javascript
arr.slice();	// liefert gesamtes Array zurück. Array "arr" bleibt unverändert. 
arr.slice(0);	// liefert gesamtes Array zurück. Array "arr" bleibt unverändert. 
arr.slice(3);	// liefert array ab index 3 zurück. Array "arr" bleibt unverändert.
arr.slice(3,4);	// liefert array zwischen index 3 und 4 zurück. Array "arr" bleibt unverändert. 
```

### splice
```javascript
arr.splice(0);	  // trennt das Array wirklich ab, ab index 0! Array hat dann keine Elemente mehr.
arr.splice(3);	  // löscht Elemente vom array ab index 3 und liefert Array von gelöschten Elementen
arr.splice(2,3);  // löscht ab index 2, die nächsten 3 elemente, und liefert Array von gelöschten Elementen zurück.
arr.splice(2,3, "newEl1", "newEl2");  // löscht Elemente, und fügt an der Stelle die neuen Elemente ein.

delete arr[3]	// ändert Element an Index 3 zu "undefined". Length des Arrays bleibt unverändert.
```


### forEach
```javascript```
[1,2,3,4].forEach( callbackFunction, this )
[10,20,30,40].forEach( function(el, index, arr){
	console.log(el);	//liefert Elemente (10,20,30,40)
	console.log(index); // liefert index (0,1,2,3)
	console.log(arr);	// liefert 4x [10,20,30,40]
});
```

### map
ändert die Werte der Elemente des Arrays
und liefert neues Array zurück
```javascript
[10,20,30,40].map( function(el, index, arr){
	return el*el;		// liefert [100, 400, 900, 1600]
})
```

### filter
filtert bestimmte Elemente aus dem Array heraus.
Liefert neues Array zurück
```javascript
[10,20,30,40].map( function(el, index, arr){
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

```javascript
[10,20,30,40].reduce( function(sum, el, index, arr){
	return sum+el;		// 15
});

[10,20,30,40].reduce( function(sum, el, index, arr){
	return sum+el;		// 100+15, liefert 115 zurück
}, 100);

["hello ", , ,,,,false,,"javascript"].reduce(function(len,el){
	return len+1;
},0);
// 3
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

### Argumente von Funktionen
```javascript
function foo(){
 console.log(arguments, 'len', arguments.length, '2nd', arguments[1]); 
}
foo(1,3,4,5); 			//[1, 3, 4, 5] "len" 4 "2nd" 3


function foo(){
 var args = Array.prototype.slice.call(arguments); // array-like object in Array ändern
 args.push(5);
 console.log(args, 'len', args.length, '2nd', args[1]);
}
```			
			
