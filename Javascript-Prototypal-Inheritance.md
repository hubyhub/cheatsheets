#Prototypal Javascript

## GENERATING OBJECTS 

This is the "class" or better lets say the **prototype**.
```javascript
var Person = {
	name : "",	
	sayName : function(){
		console.log("my name is "+ this.name);
	},	
};
```



### Make new instance of Person and set its property
```javascript
var p1 = Object.create(Person);
p1.name = "John";
```

### Another instance and set its property (another way)
```javascript
// second parameter is called "object descriptors"
var p2 = Object.create(Person, {
	name : { value : "Jane"}
});
```

### Yet another instance with new properties
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

## INHERITANCE 
(By CHAINING PROTOTYPES)
```javascript

var Person = {
	name : "",	
	sayName : function(){
		console.log("my name is "+ this.name);
	},	
};

var Teacher = Object.create(Person, {
 subject : {value :""},
 teach : {
	value : function (){
		console.log("Hey! I am teaching "+ this.subject);
	}
 }
});

var teacher1 = Object.create(Teacher,  {name : {value : "John Kimble"}, subject : {value : "Math"} } );
var teacher2 = Object.create(Teacher,  {name : {value : "John Keating"}, subject : {value : "English"}}  );

```















