### SVG mit Javascript


### Attributes
```javascript
    element.getAttribute(attributeName)
    element.setAttribute(name, newValue)
    element.removeAttribute(name)
```

### CSS
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

### Element erzeugen
```javascript
    1. // document er 
    var obj = document.getElementById("mySVG");
    var svgDoc = obj.getSVGDocument();    
    
    2. // erzeugen 
    circle = svgDoc.createElementNS("http://www.w3.org/2000/svg", "circle");
   
    3. // append
    parentElement.appendChild(circle);
       
   
    var svgns = clock.namespaceURI, 
    // Weitere Elemente
    doc.createElementNS(svgns, "circle");
    doc.createElementNS(svgns, "g") );
    doc.createElementNS(svgns, "path")
    
    // nesting
    parentElement.appendChild(doc.createElementNS(svgns, "path"));

```

`var svgns = document.getElementById("mySVG").namespaceURI;`
    


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

    <rect id="eventCatcher" x="0" y="0" width="400" height="300"
      style="fill: none;" pointer-events="visible" /> 
 ```

`fill: none;` macht es unsichtbar
`pointer-events:visible`  um auf sichtbare Elemente zu reagieren, unabhängig von ihrer opacity

In einer init() function fügt man event listener hinzu:
`document.getElementById("eventCatcher").addEventListener("mouseup", endDrag, false);`



### get target from event like a click

```javascript
   function clickButton(evt) {
          var choice = evt.target.parentNode;
   }
```

### Scaling object and keeping its stroke-width
```javascript   
  
  var scaleFactor = [1.25, 1.5, 1.75];
  function transformShirt() {
      var factor = scaleFactor[scaleChoice];
      var obj = document.getElementById("shirt");
      obj.setAttribute("transform", "translate(150, 150) scale(" + factor + ")");
      obj.setAttribute("stroke-width", 1 / factor);
  }
  
```

  
--> D3.js
--> Raphaël
--> Snap.svg
jQuery ist nicht geeignet:
"if you ask JQuery to make you a circle, it will return an HTMLUnknownElement object"