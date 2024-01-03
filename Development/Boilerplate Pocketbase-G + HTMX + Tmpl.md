
Default main.go looks like this:


```
package main

import (
    "log"
    "os"

    "github.com/pocketbase/pocketbase"
    "github.com/pocketbase/pocketbase/apis"
    "github.com/pocketbase/pocketbase/core"
)

func main() {
    app := pocketbase.New()

    // serves static files from the provided public dir (if exists)
    app.OnBeforeServe().Add(func(e *core.ServeEvent) error {
        e.Router.GET("/*", apis.StaticDirectoryHandler(os.DirFS("./pb_public"), false))
        return nil
    })

    if err := app.Start(); err != nil {
        log.Fatal(err)
    }
}
```

The one you should use as of 1.1.2024 looks like this:
```
package main

  

import (

"fmt"

  

"github.com/a-h/templ"

"github.com/a-h/templ-examples/hello-world/template"

"github.com/labstack/echo/v4"

)

  

func main() {

e := echo.New()

  

component := template.Hello("John", "Vaine")

  

e.GET("/", func(c echo.Context) error {

templ.Handler(template.Whoarewe("GO", "HTMX", "SQLite")).ServeHTTP(c.Response(), c.Request())

templ.Handler(component).ServeHTTP(c.Response(), c.Request())

  

return nil

})

  

fmt.Println("Listening on :3000")

e.Start(":3000")

}

func Hello(s1, s2 string) {

panic("unimplemented")

}
```


This is the referenced template 
```
package template

  

templ Whoarewe(back, front, db string){

<h1>Our backend is written in {back} </h1>

<h1>Our frontend is written in {front}</h1>

<p1>Our data base is {db}</p1>

}

templ Hello(name, last_name string){

<div>Hello {name} {last_name}, how are you?</div>

  

}
```

And the templ file is on this link https://pkg.go.dev/github.com/a-h/templ@v0.2.476