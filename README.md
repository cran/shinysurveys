# shinysurveys

#### Easily Create and Deploy Surveys in Shiny

<!-- badges: start -->

[![R-CMD-check](https://github.com/jdtrat/shinysurveys/workflows/R-CMD-check/badge.svg)](https://github.com/jdtrat/shinysurveys/actions) [![CRAN status](https://www.r-pkg.org/badges/version/shinysurveys)](https://CRAN.R-project.org/package=shinysurveys)

<!-- badges: end -->

------------------------------------------------------------------------

<img src="https://jdtrat.com/project/shinysurveys/featured-hex.png" width="328" height="378" align="right"/>

{shinysurveys} provides easy-to-use, minimalistic code for creating and deploying surveys in Shiny. Originally inspired by Dean Attali's [shinyforms](https://github.com/daattali/shinyforms), our package provides a framework for robust surveys, similar to Google Forms, in R with [Shiny](https://github.com/rstudio/shiny/).

## Table of contents

-   [Installation](#installation)
-   [Demos](#demos)
-   [Getting Started](#getting-started)
-   [Advanced Surveys](#advanced-surveys)
-   [Further Reading](#further-reading)
-   [Feedback](#feedback)
-   [Code of Conduct](#code-of-conduct)

------------------------------------------------------------------------

## Installation

You can install {shinysurveys} via CRAN or GitHub and load it as follows:

``` {.r}
# Install released version from CRAN
install.packages("shinysurveys")

# Or, install the development version from GitHub
remotes::install_github("jdtrat/shinysurveys")

# Load package
library(shinysurveys)
```

## Demos

A survey made with our package might look like this:

![](https://www.jdtrat.com/project/shinysurveys/shinysurveys-final-demo.gif)

You can run a demo survey with the function `shinysurveys::demo_survey()`.

## Getting Started

Aside from `demo_survey()`, {shinysurveys} exports two functions: `surveyOutput()` and `renderSurvey()`. The former goes in the UI portion of a Shiny app, and the latter goes in the server portion. To create a survey, you can build a data frame with your questions. The following columns are required.

-   *question*: The question to be asked.
-   *option*: A possible response to the question. In multiple choice questions, for example, this would be the possible answers. For questions without discrete answers, such as a numeric input, this would be the default option shown on the input. For text inputs, it is the placeholder value.
-   *input_type*: What type of response is expected? Currently supported types include `numeric`, `mc` for multiple choice, `text`, `select`, and `y/n` for yes/no questions.
-   *input_id*: The id for Shiny inputs.
-   *dependence*: Does this question (row) depend on another? That is, should it only appear if a different question has a specific value? This column contains the input_id of whatever question this one depends upon.
-   *dependence_value*: This column contains the specific value that the dependence question must take for this question (row) to be shown.
-   *required*: logical TRUE/FALSE signifying if a question is required. Surveys can only be submitted when all required questions are answered.

Our demo survey can be created as follows:

``` {.r}
library(shiny)
library(shinysurveys)

df <- data.frame(question = "What is your favorite food?",
                 option = "Your Answer",
                 input_type = "text",
                 input_id = "favorite_food",
                 dependence = NA,
                 dependence_value = NA,
                 required = F)

ui <- fluidPage(
  surveyOutput(df = df,
               survey_title = "Hello, World!",
               survey_description = "Welcome! This is a demo survey showing off the {shinysurveys} package.")
)

server <- function(input, output, session) {
  renderSurvey()
  
  observeEvent(input$submit, {
    showModal(modalDialog(
      title = "Congrats, you completed your first shinysurvey!",
      "You can customize what actions happen when a user finishes a survey using input$submit."
    ))
  })
}

shinyApp(ui, server)
```

## Advanced Surveys

-   **Dependencies** can be added so a specific question will appear based on a participant's answer to preceding questions. The `dependence` column takes an input_id of a preceding question and, if the participant answers with the value in `dependence_value`, the new question will be shown.

-   **Required questions** can be specified by adding the value `TRUE` to the `required` column. If a required question is not answered, the user will not be able to submit their responses.

-   **URL-based user tracking** functionality lets you keep track of participants easily and systematically. If you deploy your survey on [shinyapps.io](https://www.shinyapps.io/), or run it locally in a browser, you can add a URL parameter after the backslash as follows: `?user_id=UNIQUE_ID`. A live demo can be found here: <a>https://jdtrat-apps.shinyapps.io/shinysurveys_user_tracking/?user_id=hadley</a>

- **Multi-paged surveys** can be used to better organize questions and make it easier for people to complete. As of v0.2.0., users can add an additional column page to the data frame of questions. The column can either have numeric (e.g. `c(1, 1, 2, 3`) or character (`c("intro", "intro", "middle", "final")`) values. For more documentation, see my [blog post](https://www.jdtrat.com/blog/multi-paged-shinysurvey/).

- **Automatic response aggregation** to easily gather participant's data upon submission with `getSurveyData()`. See the [official vignette](https://shinysurveys.jdtrat.com/articles/get-survey-data.html) for more details.

## Further Reading

For a more in-depth explanation of {shinysurveys}, please see the vignette [*A survey of {shinysurveys}*](https://shinysurveys.jdtrat.com/articles/surveying-shinysurveys.html).

## Feedback

If you want to see a feature, or report a bug, please [file an issue](https://github.com/jdtrat/shinysurveys/issues) or open a [pull-request](https://github.com/jdtrat/shinysurveys/pulls)! As this package is just getting off the ground, we welcome all feedback and contributions. See our [contribution guidelines](https://github.com/jdtrat/shinysurveys/blob/main/.github/CONTRIBUTING.md) for more details on getting involved!

## Code of Conduct

Please note that the shinysurveys project is released with a [Contributor Code of Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html). By contributing to this project, you agree to abide by its terms.
