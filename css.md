# CSS Tipps
### "Debugging" CSS
```css
*, *:before, *:after {
    outline: 2px solid lime;
    background: rgba(111, 11, 11, 0.5) !important;
}
```

### Dark / Light Favicons for light dark mode
```html
<html>
<head>
<link rel="icon"
      href="/white.png"
      media="(prefers-color-scheme: dark)" 
<link rel="icon"
      href="/dark.png"
      media="(prefers-color-scheme: light)" 
...
```


### Great number input

```html
// normal text
<input type="text" />

// [✓] better keyboard on mobile
// [✓] input allows only numbers (forms)
// [X] annoying arrows
<input type="number" />

// [✓] great numbers keyboard, (the big, only numbers layout)
// [X] still annoying arrows
<input type="number" inputmode="numeric" />

// [✓] great numbers input on keyboard
// [✓] no annoying arrows
// [X] but no validation
<input type="text" inputmode="numeric" />

// [✓] no annoying arrows
// [✓] great numbers input on keyboard
// [✓] validation
<input type="text" inputmode="numeric" pattern="[0-9]+" />
```
