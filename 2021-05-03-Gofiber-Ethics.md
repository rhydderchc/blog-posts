# Web Server With GoFiber

**Golang**, is an open source modular programming language. It is somewhat equal to the C syntax, but has its own features that make it better.

Today we'll learn how to setup/initialize a web server using the golang programming languge.
Golang does offer you a core package for creating web servers using the language called  Gofiber.
Gofiber offers you a large amount of customizability and ease while doing web development. You can check out its github repository here and documentation here.

```go
package main
import(
"fmt"
"github.com/gofiber/fiber"

)
func main(){
app := fiber.New()
app.Get("/", func(ctx *fiber.Ctx){
return ctx.SendString("Hello World")
}
  app.Listen(":3000")
}
```

With the help of the following code, you can easily create a web server in golang!
Ctx : Ctx is the fiber context which is literally res, req together if you've ever used express.
Some essential & Helpful Functions of GoFiber

GoFiber has some very helpful functions, that help us to create a minimal web server or a rest api.

## Grouping Routers

Routers, on the web are treated as url prefixes that allow you to navigate from one web page to another on a website. GoFiber allows you to group your routes in a very efficient and easy way.

```go
package main
import(
"fmt"
"github.com/gofiber/fiber"

)
func main(){
app := fiber.New()
app.Get("/", func(ctx *fiber.Ctx){
return ctx.SendString("Hello World")
}
  app.Listen(":3000")

/*
GROUPING ROUTES
*/
api := app.Group("/api")
v1 := api.Group("/v1")
v1.Get("/", func(ctx *fiber.Ctx){
return ctx.SendString("Api Version 1")
}
}
```
Using the app.Group function here we are creating a new router group, which is straight forward as you can see in the code. You can use curl to test out your router in the terminal.


```
curl http://localhost:3000/api/v1/
```
This should successfully give 


Api Version 1
In the terminal

## Sending JSON Data

Since we are talking about rest apis, we all know that rest apis response with json data, and gofiber easily lets you response with json data on a particular request. Let's addon to the previous example a json response.


```go

package main
import(
"fmt"
"github.com/gofiber/fiber"

)
func main(){
app := fiber.New()
app.Get("/", func(ctx *fiber.Ctx){
return ctx.SendString("Hello World")
}
  app.Listen(":3000")

/*
GROUPING ROUTES
*/
api := app.Group("/api")
v1 := api.Group("/v1")
v1.Get("/", func(ctx *fiber.Ctx){
return ctx.SendString("Api Version 1")
}
/*
SENDING A JSON RESPONSE
This sample, requests at http://localhost:3000/api/v1/example
and responses with
{
"message":"This is an example",
"author": "Rhydderchc"
}
*/
v1.Get("/example", func(ctx *fiber.Ctx){
const author string = "Rhydderchc"
return ctx.JSON(fiber.Map{
"message":"This is an example",
"author": author,
})

}
/*
Additionally Gofiber lets you send JSON response with JSONP callback, 
here's an example. Basically what that means is the response type 
has a callback like,
// => callback({"message":"This is an example", "author":"rhydderchc"})
Yes, of course you can also add your own custom callback name
*/
type SampleStruture struct{
message string
author string
}
v1.Get("/example/JSONP", func(ctx *fiber.Ctx){
mess := &SampleStructure{
message:"This is a JsonP callback message",
author:"Rhydderchc"
return ctx.JSONP(mess, "CustomCallback")
// request at http://localhost:3000/api/v1/example/JSONP
}
}
```
These are two basic examples of JSON responsing with fiber!

## Querying

Now there are two ways of getting a parameter from the request and querying a parameter.

```go
v1.Get("/:id", func(ctx *fiber.Ctx){
id := ctx.Params("id")
return ctx.SendString("Your Id was"+id)
}
/* 
http://localhost:3000/api/v1/id?number=Abcd
Sends response Your id number was Abcd
*/
v1.Get("/id", func(ctx *fiber.Ctx){
id := ctx.Query("number")
return ctx.SendString("Your id number was"+id)
}
```
Thus with the help of these few snippets you can see how easy it is to query with Fiber!

**Well, that was it for todays lesson, thanks for sharing your valuable time reading this.Hope you use GoFiber in your next project 👋 !**
