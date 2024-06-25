To implement a real-time notification system in your Go chat application, you can use Server-Sent Events (SSE) or WebSockets. Both technologies allow the server to push updates to the client without the client having to request them. Here, I'll guide you through setting up SSE, which is simpler for unidirectional communication from server to client.

### Step 1: Set Up SSE in Go

First, you need to create a handler that will keep a connection open and send events to the client.

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"time"
)

// Event represents a server-sent event
type Event struct {
	EventType string
	Data      string
}

// EventHandler handles SSE connections
func EventHandler(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/event-stream")
	w.Header().Set("Cache-Control", "no-cache")
	w.Header().Set("Connection", "keep-alive")

	flusher, ok := w.(http.Flusher)
	if !ok {
		http.Error(w, "Streaming unsupported!", http.StatusInternalServerError)
		return
	}

	events := make(chan Event)
	defer close(events)

	// Register the client's event channel
	go func() {
		for {
			select {
			case event := <-events:
				fmt.Fprintf(w, "event: %s\n", event.EventType)
				fmt.Fprintf(w, "data: %s\n\n", event.Data)
				flusher.Flush()
			case <-r.Context().Done():
				return
			}
		}
	}()

	// Keep the connection alive
	for {
		time.Sleep(15 * time.Second)
		events <- Event{EventType: "ping", Data: "{}"}
	}
}

func main() {
	http.HandleFunc("/events", EventHandler)
	http.HandleFunc("/", indexHandler)
	// Other handlers...

	log.Fatal(http.ListenAndServe(":8090", nil))
}
```

### Step 2: Send Events from Your Handlers

Modify your existing handlers to send events to the clients. You can create a global channel to send events.

```go
var eventChannel = make(chan Event)

func handleCreateMessage(w http.ResponseWriter, r *http.Request) {
	// Your existing logic to create a message
	// ...

	// Send an event to notify clients
	eventChannel <- Event{EventType: "newMessage", Data: "New message in chat"}
}
```

### Step 3: Modify the EventHandler to Use the Global Channel

Update the `EventHandler` to listen to the global event channel.

```go
func EventHandler(w http.ResponseWriter, r *http.Request) {
	w.Header().Set("Content-Type", "text/event-stream")
	w.Header().Set("Cache-Control", "no-cache")
	w.Header().Set("Connection", "keep-alive")

	flusher, ok := w.(http.Flusher)
	if !ok {
		http.Error(w, "Streaming unsupported!", http.StatusInternalServerError)
		return
	}

	for {
		select {
		case event := <-eventChannel:
			fmt.Fprintf(w, "event: %s\n", event.EventType)
			fmt.Fprintf(w, "data: %s\n\n", event.Data)
			flusher.Flush()
		case <-r.Context().Done():
			return
		}
	}
}
```

### Step 4: Client-Side JavaScript to Handle SSE

On the client side, you need to set up an EventSource to listen for events from the server.

```html
<script>
if (typeof(EventSource) !== "undefined") {
  var eventSource = new EventSource("/events");

  eventSource.onmessage = function(event) {
    console.log("New event: ", event);
    if (event.data) {
      var data = JSON.parse(event.data);
      if (data.eventType === "newMessage") {
        // Refresh messages or notify the user
      }
    }
  };

  eventSource.onerror = function(err) {
    console.error("EventSource failed:", err);
  };
} else {
  console.log("Your browser does not support Server-Sent Events.");
}
</script>
```

This setup will allow your server to push notifications to the client whenever a new message is created, prompting the client to refresh the messages or notify the user accordingly.