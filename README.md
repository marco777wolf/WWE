library(base)
library(methods)
library(utils)
library(grDevices)
library(stats)
library(dygraphs)
library(ggplot2)
library(shiny)
# Define UI for application that draws a histogram
ui <- fluidPage(

    # Application title
    titlePanel("WWE Analysis"),
    tags$h3("Data"),
    tags$p("I've downloaded data that corresponds to many different wreslters. The data spans over match statistics.
           Some of these statistics measure the amount of wins, losses and draws as well as their respective percentages."),
    tags$hr(),
    tags$h3("Analysis"),
    tags$p("For my analysis, I decided to use a scatter plot with a regression"),
    plotOutput(outputId = "scatterplot"),
    selectInput(inputId = "x", choices = c("Seth Rollins", "AJ Styles", "Big E"), label = "x-axis", selected = "Big E"),
    selectInput(inputId = "y", choices = c("Cesaro", "Baron Corbin", "Luke Gallows"), label = "y-axis", selected = "Luke Gallows"),
    tags$h3("Interpretation"),
    tags$br(),
    tags$p("So for my default I set them to model the fighters of Big E and AJ Styles. We can see that these two wrestlers experience similar
          trends with respect to when the win and lose their matches. This can potentially be a signal of these two individuals sharing similar fighting style
          and nutrition plans. (nutrition can be a sign of fatigue -- which in turn can be a reason for the volatile behavior of the graph)")
)

library(ggplot2)
server <- function(input, output) {
    output$scatterplot <- renderPlot({
        ggplot(WWE_data, aes(!!as.name(input$x), !!as.name((input$y)))) + geom_point() + geom_smooth(method = "auto", se =T,
                                                                                                     fullrange=FALSE, level = 0.95)
    })
}

# Run the application 
shinyApp(ui = ui, server = server)
