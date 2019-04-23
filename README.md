# shiny.users - demo

### User authentication for R Shiny applications.

 We are in early access, join our [waitlist](https://mailchi.mp/appsilondatascience.com/shiny-user-management/ "Shiny Users early access registration")

To install shiny.users package look for the **installation** section in your shiny.users early access **invitation email**.

### Example

```r
library(shiny)
library(shiny.users)

SHINY.USERS.APP_KEY <- "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" # Get app key from Developer Panel

ui <- shinyUI(fluidPage(
  div(class = "container",
    style = "padding: 4em",
    login_screen_ui("login_screen"),
    uiOutput("authorized_content")
  )
))

server <- shinyServer(function(input, output) {
  users <- initialize_users(SHINY.USERS.APP_KEY)

  callModule(login_screen, "login_screen", users)

  output$authorized_content <- renderUI({
    if (!is.null(users$user())) {
      tagList(
        shiny::numericInput("secret_input", "Input secret number", 0),
        uiOutput("secret_output")
      )
    }
  })
  output$secret_output <- renderUI({
    span(as.numeric(input$secret_input) * 2)
  })
})

shinyApp(ui, server)
```

More examples are in package inside the `examples` folder.
