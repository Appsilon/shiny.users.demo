# shiny.users - demo

User authentication for R Shiny applications.

[![CircleCI](https://circleci.com/gh/Appsilon/shiny.users.svg?style=svg&circle-token=ab2add6ebfebe15d93520d4b4efced8f8aafe7fb)](https://circleci.com/gh/Appsilon/shiny.users)

Login to https://shinyusers.appsilon.com to get your APP_KEY from Developer Panel

## Installation
To install package run:
```
install.packages("shiny.users", repos = "go.appsilon.com/ARAN")
```

For rsconnect deployment also add Appsilon R Application Network to your repos:

```
r <-getOption("repos")
r["ARAN"] <- "https://go.appsilon.com/ARAN"
options(repos = r)
```

If you want to use semantic form_style for shiny.users you need to install latest shiny.semantic package:
```
devtools::install_github("git@github.com:Appsilon/shiny.semantic.git", ref = "develop")
```

## Example

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
