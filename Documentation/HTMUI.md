Base idea came from loosely reading twitter comments on shetzienUI or whatever. I don't use React so shetz is of no use to me but the idea of making my own component library in one place and then just copying them into code speaks to my HTMX consumed mind. Simply put i will take tailwind classes and let users manipulate them with sliders and other input fields like photoshop, then copy finished elements. State will be managed trough URL parameters so everything can run completely on the client-side for that nice nice free Netlify hosting and ease of sharing. After I'm happy with the product, integrating auth and db with pocketbase should be super easy because you are storing essentially just URL stings. The down-the-line development idea is letting people write complex components right in the browser, given the fact that storage is still just strings I don't see much complexity in the feature. 

Everything works client-side, I will integrate Pocketbase at the end.

STATE MANAGEMENT

Two core functions are writeParam and readParam, they do all of state trough URL parameters.

```
//Generic function for writing parameters to the URL

function writeParam(paramName, paramValue) {

const currentUrl = new URL(window.location.href);

currentUrl.searchParams.set(paramName, paramValue);

const newUrl = currentUrl.toString();

history.pushState({}, "", newUrl);

}
```
read takes the name of the parameter and returns the vale from the url, it is kept really simple because it is used on multiple occasions

```
// Function to read a parameter from the URL

function readParam(paramName) {

const urlParams = new URLSearchParams(window.location.search);

return urlParams.get(paramName);

}
```

The third function in the JS/state.js file called magic, the intent is for it to take the parameter name and call a function that uses it to manipulate the UI
