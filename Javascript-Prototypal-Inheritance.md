#Prototypal Javascript
## Generating Objects with Object create

### Generate a Prototype with a Object Literal.
This is the "class" or better lets say the **prototype**.
 
```javascript
var Person = {
	name : "",	
	sayName : function(){
		console.log("my name is "+ this.name);
	},	
};
```



### Instantiate a new Object and set its property
```javascript
var p1 = Object.create(Person);
simplePerson.name = "John";
```

### Instantiate a new Object and set its property (another way)
```javascript
// second parameter is called "object descriptors"
var p2 = Object.create(Person, {
	name : { value : "Jane"}
});
```

### Instantiate a new Object and ** add new properties**
```javascript
// adding a new property "age" and a new method "greet"
var p3 =  Object.create(Person, {
		name : { value : "Arnie"},
		age  : { value : 65 },
		greet : {
			value : function (){
				console.log("Hasta la vista, baby!");
			}
		}
});
```



