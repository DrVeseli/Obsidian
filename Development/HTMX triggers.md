## Scroll into view
```
hx-on="htmx:afterSwap: document.getElementById('ID').scrollIntoView({ behavior: 'smooth' })"
```
## Form values 
```
hx-vals='{"chatID": {{.ChatID}}}'
```
## Clear input
```
hx-on="htmx:afterRequest:document.getElementById('messageInput').value = ''"
```
## Event source refreshing on load elements
```
if (typeof EventSource !== "undefined") {

var eventSource = new EventSource("/events");

  

eventSource.onmessage = function (event) {

console.log("New event: ", event.data);

var chatID = event.data;

  

var chatElement = document.getElementById(chatID);

if (chatElement) {

var indicator = chatElement.querySelector("#indicator");

if (indicator) {

indicator.classList.remove("hidden");

}

  

// Re-trigger the hx-get event on the div with hx-get="/getLastMessage"

var lastMessageDiv = chatElement.querySelector(".lastMessage");

if (lastMessageDiv) {

htmx.ajax("GET", "/getLastMessage", {

target: lastMessageDiv,

values: { chatID: chatID },

});

}

}

};

  

eventSource.onerror = function (err) {

console.error("EventSource failed:", err);

};

} else {

console.log("Your browser does not support Server-Sent Events.");

}
```

