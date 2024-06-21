Here's how you can achieve this in your chat application:

1. **Capture Cookies and Sessions**: When a client establishes an SSE connection, you can access the cookies and session information from the HTTP request. This allows you to verify if the user is a participant in the chat.
    
2. **Attach Recipients to Messages**: You can include a list of recipients in each message. This list will help you filter out messages that should not be sent to the current user.

```
var eventChannels = make(map[string]chan Message)

// EventHandler handles SSE connections
func EventHandler(w http.ResponseWriter, r *http.Request) {
  w.Header().Set("Content-Type", "text/event-stream")
  w.Header().Set("Cache-Control", "no-cache")
  w.Header().Set("Connection", "keep-alive")

  // Capture cookies and session information
  session, err := store.Get(r, "session-name")
  if err != nil {
    http.Error(w, err.Error(), http.StatusInternalServerError)
    return
  }

  userId, ok := session.Values["userId"].(string)
  if !ok {
    http.Error(w, "User ID is required", http.StatusBadRequest)
    return
  }

  flusher, ok := w.(http.Flusher)
  if !ok {
    http.Error(w, "Streaming unsupported!", http.StatusInternalServerError)
    return
  }

  eventChannel := make(chan Message)
  eventChannels[userId] = eventChannel

  defer func() {
    close(eventChannel)
    delete(eventChannels, userId)
  }()

  for {
    select {
    case message := <-eventChannel:
      // Check if the user is a recipient of the message
      if isRecipient(userId, message.Recipients) {
        fmt.Fprintf(w, "data: %s,%d\n\n", message.Data, message.Target)
        flusher.Flush()
      }
    case <-r.Context().Done():
      return
    }
  }
}

// isRecipient checks if the user is in the list of recipients
func isRecipient(userId string, recipients []string) bool {
  for _, recipient := range recipients {
    if recipient == userId {
      return true
    }
  }
  return false
}

// SendMessage sends a message to a specific user
func SendMessage(userId string, message Message) {
  if eventChannel, exists := eventChannels[userId]; exists {
    eventChannel <- message
  }
}
```
```
type Message struct {
  Data       string
  Target     int
  Recipients []string
}
```
When sending a message, include the list of recipients:

```
SendMessage("user123", Message{Data: "Hello", Target: 456, Recipients: []string{"user123", "user456"}})
```

## Client side vanilla JS

```
if (typeof EventSource !== "undefined") {
  var eventSource = new EventSource("/events");

  eventSource.onmessage = function (event) {
    console.log("New event: ", event.data);
    // Handle the event data as needed
  };

  eventSource.onerror = function (err) {
    console.error("EventSource failed:", err);
  };
} else {
  console.log("Your browser does not support Server-Sent Events.");
}
```
By capturing cookies and sessions in the SSE handler and including a list of recipients in each message, you can ensure that only authorized users receive the messages intended for them.