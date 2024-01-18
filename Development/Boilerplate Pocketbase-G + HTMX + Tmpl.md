
Default main.go looks like this:

("bragi/template" is "go module/directory")

```
package main


import (
"bragi/template"
"log"

  

"github.com/a-h/templ"
"github.com/labstack/echo/v5"
"github.com/pocketbase/pocketbase"
"github.com/pocketbase/pocketbase/core"
)

  

func main() {
app := pocketbase.New()

// serves the templ.Handler instead of the static files directory

app.OnBeforeServe().Add(func(e *core.ServeEvent) error {

e.Router.GET("/", func(c echo.Context) error {

templ.Handler(template.Hello("GO", "HTMX", "Pocketbase")).ServeHTTP(c.Response(), c.Request())

return nil
})
return nil
})

  

if err := app.Start(); err != nil {
log.Fatal(err)
}}
```


This is the referenced template, located in ./template/
```
package template

  

templ Hello(back, front, db string){

<h1>Welcome to the simple stack</h1>

<p>You are extending {db} with {back} to run the server</p>

<p>And the server is returning HTML thanks to {front}</p>

}
```

And the templ file is on this link https://pkg.go.dev/github.com/a-h/templ@v0.2.476

The command to generate .go files from .templ is:
```
./templ generate (on MacOS)
```

It requires the executable.

The Hello template is served on localhost:8090, Admin UI on localhost:8090/_/