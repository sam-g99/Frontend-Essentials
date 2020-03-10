# Frontend-Essentials
Some things I think are pretty good to be aware of for frontend development. I put [MDN](https://developer.mozilla.org/en-US/) and [Javascript.info]() links where applicable for more examples and/or in-depth explanation.

## CSS Defaults
Sometimes browsers add the darnest defaults...here's an effort to make things consistent across browsers :D

```css
* {
  box-sizing: border-box;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
    Helvetica, Arial, sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji',
    'Segoe UI Symbol';
  margin: 0;
  padding: 0;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-kerning: auto;
}

:focus {
  outline: none;
}

::-moz-focus-inner {
  border: 0;
}

html {
  height: 100%;
  position: relative;
  -webkit-text-size-adjust: 100%;
}
```

## Making Api Request

### XMLHttpRequest (Supported By Older Browsers)

#### Get Request Example
```js
const xhr = new XMLHttpRequest();
// Make a get request to retrieve some fake JSON for testing
xhr.open('GET', 'https://jsonplaceholder.typicode.com/posts');

// If you want to send/receive credentials like cookies
xhr.withCredentials = true;
xhr.send();
xhr.onload = () => {
  console.log(xhr);
  if(xhr.status === 200){
    console.log(JSON.parse(xhr.response));
  }  else  {
    console.log('It didn\'t work D:', xhr.status, xhr.statusText)
  }
}
```

#### Post Request Example

```js 
const xhr = new XMLHttpRequest;
xhr.open('POST', 'https://example.com/login');
xhr.withCredentials = true;
xhr.setRequestHeader("Content-Type", "application/json;charset=UTF-8");
xhr.send(JSON.stringify({username: "Sam123", password: "secretpa$$word123"}));
xhr.onload = () => console.log(xhr.response);
```

[MDN - XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)

[Javscript.info - XMLHttpRequest](https://javascript.info/xmlhttprequest)

### Fetch

#### Get Request Example

```js
fetch('https://jsonplaceholder.typicode.com/posts')
.then(response => {
  return response.json();
})
.then(data => {
  console.log(data);
})
```
#### Post Request Example
```js
fetch('https://example.com/login', {
  method: 'POST',
  credentials: 'include',
  headers: {
    'Content-Type': 'application/json;charset=UTF-8',
  },
  body: JSON.stringify({username: "Sam123", password: "secretpa$$word123"});
})
.then(response => {
  return response.json();
})
.then(data => {
  console.log(data);
})
```
[MDN - Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

[Javascript.info - Fetch](https://javascript.info/fetch)

#### Sending Form Data
```html
<form onsubmit="return login(event)">
  <label>
    Username:
    <input type="text" name="username">
  </label>
  <label> 
    Password: 
    <input type="text" name="password">
  </label>
</form>

<script>
  const login = async (e) => {
    e.preventDefault();
    const loginForm = document.querySelector('form');
    const formData = new FormData(loginForm);
    const response = await fetch('https://example.com/login', { method: 'POST', credentials: 'include', body: formData });
    const data = await response.json();
    console.log(data);
    return false;
  }
</script>
```
[MDN - FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)

[Javascript.info - FormData](https://javascript.info/formdata)

## Dom Manipulation

### Document Fragment
If you have a lot of stuff you want to add to the dom, I like to construct it in a DocumentFragment instead of appending it in the real DOM one by one.
```html
<div id="root"></div>

<script>
  const root = document.getElementById('root'); 
  const fragment = new DocumentFragment();
  
  Array.from(`don't-forget-to-drink-water`).forEach((v) => {
    const div = document.createElement('div');
    div.innerText = `${v}`
    fragment.appendChild(div);
  })
  
  root.appendChild(fragment);
</script>
```
[MDN - Document Fragment](https://developer.mozilla.org/en-US/docs/Web/API/DocumentFragment)

## Quick Tips

### Using `defer` For External Scripts
If you have an external script that relies on certain dom elements existing you may be tempted to chuck it to the bottom. However this causes the script to wait for the whole dom to be loaded before even parsing the JavaScript. With `defer` the JavaScript is parsed but waits for the document to load before executing.
(This html document is simplified for this example.)
```html
<html>
  <head>
    <script src="main.js" defer></script>
  </head>
  <body>
    <div id="elementJavaScriptNeeds"></div>
  </body>
</html>
```
[MDN - `<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) (ctrl f "defer")

[Javascript.info - defer](https://javascript.info/script-async-defer#defer)


