# SVG 

### Elemente
```xml

<line x1="0" y1="0" x2="100" y2="0" style="stroke: black;"/>

<rect x="70" y="30" width="20" height="20" style="fill: gray;"/>

<circle cx="50" cy="50" r="3" style="fill: black;"/>

<ellipse cx="30" cy="80" rx="10" ry="20" style="stroke: black; fill: none;"/>

<polygon points="40 40, 100 40, 70 70, 40 70" style="fill: gray; stroke: black;"/>

<polyline points="100 0, 0 0, 0 100" style="stroke: black; fill: none;"/>



<g id="arrow"">
    <line x1="60" y1="50" x2="90" y2="50"/>
    <polygon points="90 50, 85 45, 85 55"/>
</g>
<use xlink:href="#arrow" transform="rotate(60, 50, 50)"/>

```

### Auswahl einiger Attribute

```
stroke  Die stroke-Farbe. default none

stroke-width

stroke-opacity  0.0 - 1.0;

stroke-dasharray="15, 10, 5, 10"    Strichmuster: "5, 5" 

stroke-linecap  Form der Linien-enden "butt" (default), "round", or "square".

stroke-linejoin         Form der Ecken eines Polygons "miter", "round", or "bevel" (flat).
stroke-miterlimit       Default Wert is 4.
```




### Attribute setzen, löschen und ändern
```javascript
    element.getAttribute(attributeName)
    element.setAttribute(name, newValue)
    element.removeAttribute(name)
```

### CSS setzen 
```javascript
    element.setAttribute("style", newStyleValue)    // overwrites all styles on the element
    element.style.getPropertyValue(propertyName)
    element.style.setProperty(propertyName, newValue, priority)     // priority is null or “important”
    element.style.removeProperty(propertyName)
    element.style.cssText                       // is a string in "property-name: value" format, to set all styles at once   
    
    obj.style.setProperty("fill", colorStr, null); // example       
```

`element.style.propertyName` oder `element.style["property-name"]` ist nicht x-browser kompatibel.


### Text & Content
```javascript
    element.textContent   // gets and write text. overwrites child-nodes   
```

### Elemente erzeugen
```javascript
    1. // document erhalten 
    var obj = document.getElementById("mySVG");
    var svgDoc = obj.getSVGDocument();    
    
    2. // erzeugen 
    circle = svgDoc.createElementNS("http://www.w3.org/2000/svg", "circle");
   
    3. // append
    parentElement.appendChild(circle);
       
   
    var svgns = obj.namespaceURI, 
    // Weitere Elemente
    doc.createElementNS(svgns, "circle");
    doc.createElementNS(svgns, "g") );
    doc.createElementNS(svgns, "path")
    
    // nesting
    parentElement.appendChild(doc.createElementNS(svgns, "path"));

```

`var svgns = document.getElementById("mySVG").namespaceURI;`
### Transformationen
```html

    <g id="square">
        <rect x="0" y="0" width="20" height="20" />
    </g>
    <use xlink:href="#square" transform="translate(50,50)"/>
    <use xlink:href="#square" transform="scale(2)"/>
    
    <rect x="10" y="10" height="15" width="20" transform="translate(30, 20) scale(2)" /> // combination
    
```



## Basic Interactivity

### Event listener
```javascript
    function grow(evt) {
          var obj = evt.target;
          obj.setAttribute("width", "130");
      }
   
      function shrink(evt) {
          this.setAttribute("width", "40");
      }
   
      function reStroke(evt) {
          var w = evt.target.style.getPropertyValue("stroke-width");
          w = 4 - parseFloat(w); /* toggle between 1 and 3 */
          evt.target.style.setProperty("stroke-width", w, null);
      }
   
      var c = document.getElementById("myRect");
      c.addEventListener("click", reStroke);
      c.addEventListener("mouseover", grow);
      c.addEventListener("mouseout", shrink);      
      
      el.addEventListener("mousemove", mouseMove);
      el.addEventListener("mousedown", mouseDown);
      el.addEventListener("mouseup", mouseUp);
      el.addEventListener("focusIn", focusIn);
      el.addEventListener("focusOut", focusOut);
      
      // SVGZoom, SVGScroll, SVGResize, SVGLoad
      
      
```



**Achtung:** 
`onmouseup` reagiert nur wenn es auf dem Element erfolgt auf dem 'onmousedown' erfolgt ist.
Um das zu lösen macht man ein unsichtbares **rect** das den kompletten viewport covert und auf das "**mouseup** event hört.
 Es muß das erste und daher das unterste object in der Graphic und zusätzlich noch:
 
```html 

<rect id="eventCatcher" x="0" y="0" width="400" height="300"  style="fill: none;" pointer-events="visible" />

```

`fill: none;` macht es unsichtbar
`pointer-events:visible`  um auf sichtbare Elemente zu reagieren, unabhängig von ihrer opacity

In einer init() function fügt man event listener hinzu:
`document.getElementById("eventCatcher").addEventListener("mouseup", endDrag, false);`



### Get Target und Eltern NOde

```javascript
   function clickBtn(evt) {
          var el = evt.target;
          var parentEl = evt.target.parentNode;
   }
```

### Objekt aufskalieren, aber linien-dicke beibehalten:
```javascript   
  
  var scaleFactor = [1.25, 1.5, 1.75];
  function transformShirt() {
      var factor = scaleFactor[scaleChoice];
      var obj = document.getElementById("shirt");
      obj.setAttribute("transform", "translate(150, 150) scale(" + factor + ")");
      obj.setAttribute("stroke-width", 1 / factor);
  }
  
```

  
### Libraries  
* D3.js<br>
* Raphaël
* Snap.svg

jQuery ist nicht geeignet:<br>
"if you ask JQuery to make you a circle, it will return an HTMLUnknownElement object"