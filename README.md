# StaffLinkPro

Welcome to **StaffLinkPro**—a cutting-edge R Shiny app revolutionizing temporary healthcare staffing. Built with AI innovation (Grok from xAI and OpenAI) and coded in RStudio, this tool empowers staffing executives to streamline operations while offering developers a robust, extensible platform. Whether you’re optimizing shift management or exploring staff availability on a live map, StaffLinkPro bridges efficiency and insight.

- [**What It Does**](#what-it-does)  
- [**How It Was Built**](#how-it-was-built)  
- [**Getting Started**](#getting-started)  
- [**Deploying to Shinyapps.io**](#deploying-to-shinyappsio)  
- [**Live Demo**](#live-demo)  
- [**Why This Matters**](#why-this-matters)  
- [**Conclusion**](#conclusion)

---

## What It Does

StaffLinkPro tackles the chaos of healthcare staffing with intuitive features:
- **Dashboard**: Real-time metrics—shifts filled, open shifts, clients served—at your fingertips.
- **Shift Management**: Add and track shifts with a few clicks, ensuring no gap goes unfilled.
- **Map View**: Visualize staff locations (Dallas, Fort Worth, Plano) on an interactive map.
- **Document Upload**: Securely upload certifications to keep compliance in check.
- **Staff Credentials**: Monitor team qualifications in one glance.

Executives get actionable insights; developers get a clean, modular codebase to build on.

---

## How It Was Built

This app emerged from a collaboration of human ingenuity and AI power:
- **Grok (xAI)**: Iteratively refined features and debugged code with real-time feedback.
- **OpenAI**: Provided initial structure and brainstorming for user-friendly design.
- **RStudio**: Coded in R with `shiny`, `shinydashboard`, `DT`, `leaflet`, and `shinyWidgets` for a polished UI and backend.

The result? A seamless blend of AI-driven development and R’s data prowess, tailored for healthcare staffing.

---

## Getting Started

For developers and curious stakeholders:
1. **Clone the Repo**: `git clone https://github.com/yourusername/StaffLinkPro.git`
2. **Install R and RStudio**: Download from [R Project](https://www.r-project.org/) and [RStudio](https://rstudio.com/).
3. **Install Dependencies**: Open `stafflink_pro_app.R` in RStudio and run:
   ```R
   install.packages(c("shiny", "shinydashboard", "DT", "leaflet", "shinyWidgets"))
   ```
4. **Run Locally**: In RStudio, click "Run App" or use:
   ```R
   shiny::runApp("path/to/StaffLinkPro")
   ```
   Log in with `admin`/`password` to explore.

---

## Deploying to Shinyapps.io

Host StaffLinkPro online for your team:
1. **Sign Up**: Create an account at [Shinyapps.io](https://www.shinyapps.io/).
2. **Install `rsconnect`**: In RStudio:
   ```R
   install.packages("rsconnect")
   ```
3. **Configure Token**: In RStudio, go to Tools > Global Options > Publishing, and connect your Shinyapps.io account.
4. **Deploy**: Run in the R console (update path as needed):
   ```R
   rsconnect::deployApp("path/to/StaffLinkPro")
   ```
5. **Access**: Once deployed, get your app’s URL (e.g., `https://yourname.shinyapps.io/StaffLinkPro/`).

---

## Live Demo

Check it out live: [StaffLinkPro Demo](https://mmcdonald411.shinyapps.io/StafflinkProApp/)  
*(Replace with your actual URL after deployment.)*

---

## Why This Matters

Healthcare staffing is a high-stakes puzzle—open shifts mean lost revenue, compliance slips risk penalties. StaffLinkPro delivers:
- **Efficiency**: Automate shift tracking, freeing executives for strategy.
- **Transparency**: Map and credential views keep everyone aligned.
- **Scalability**: Developers can extend it—add APIs, analytics, or integrations.

In an industry where every minute counts, this app turns chaos into control, proving AI and data can transform lives and bottom lines.

---

## Conclusion

StaffLinkPro isn’t just an app—it’s a vision for smarter healthcare staffing. Born from AI collaboration (Grok and OpenAI) and crafted in RStudio, it’s ready for executives to optimize and developers to enhance. Clone it, deploy it, and join the revolution.

---

### Latest Code (Embedded Link)
Here’s the [full code](#fixed-code) as of April 7, 2025, ready to roll:

```R
library(shiny)
library(shinydashboard)
library(DT)
library(leaflet)
library(shinyWidgets)

# Sample data for placeholder
temp_staff <- data.frame(
  Name = c("Alice Brown", "John Doe", "Maria Smith"),
  Specialty = c("Dental Hygienist", "Medical Assistant", "Front Desk"),
  Availability = c("Mon-Wed", "Tue-Thu", "Weekends"),
  Status = c("Available", "Booked", "Available"),
  Location = c("Dallas, TX", "Fort Worth, TX", "Plano, TX"),
  stringsAsFactors = FALSE
)

# Simulated shift data
shift_data <- data.frame(
  Name = c("Alice Brown", "John Doe", "Maria Smith", "Alice Brown", "John Doe"),
  Date = c("2025-04-01", "2025-04-02", "2025-04-03", "2025-04-04", "2025-04-05"),
  Time = c("9:00 AM", "10:00 AM", "11:00 AM", "1:00 PM", "2:00 PM"),
  Status = c("Confirmed", "Pending", "Confirmed", "Confirmed", "Pending"),
  stringsAsFactors = FALSE
)

# UI
ui <- dashboardPage(
  dashboardHeader(title = "StaffLink Pro"),
  dashboardSidebar(
    sidebarMenu(
      menuItem("Dashboard", tabName = "dashboard", icon = icon("tachometer-alt")),
      menuItem("Shift Management", tabName = "shifts", icon = icon("calendar-check")),
      menuItem("Map View", tabName = "map", icon = icon("map-marked")),
      menuItem("Upload Documents", tabName = "docs", icon = icon("file-upload")),
      menuItem("Staff Credentials", tabName = "staff_credentials", icon = icon("user-cog")),
      menuItem("Log Off", tabName = "logoff", icon = icon("sign-out-alt"))
    )
  ),
  dashboardBody(
    uiOutput("login_ui"),
    conditionalPanel(
      condition = "output.logged_in == true",
      fluidRow(
        column(3, valueBoxOutput("shifts_filled_box")),
        column(3, valueBoxOutput("open_shifts_box")),
        column(3, valueBoxOutput("clients_served_box"))
      ),
      tabItems(
        tabItem(tabName = "dashboard",
                fluidRow(
                  box(title = "Staff Availability", width = 12,
                      DTOutput("staff_table"))
                )
        ),
        tabItem(tabName = "shifts",
                fluidRow(
                  box(title = "Add New Shift", width = 6,
                      textInput("name", "Staff Name"),
                      textInput("date", "Shift Date (YYYY-MM-DD)"),
                      textInput("time", "Shift Time (e.g., 9:00 AM)"),
                      actionButton("submit_shift", "Submit Shift")),
                  box(title = "Upcoming Shifts", width = 6,
                      DTOutput("shift_table"))
                )
        ),
        tabItem(tabName = "map",
                leafletOutput("staff_map", height = 600)
        ),
        tabItem(tabName = "docs",
                fileInput("cert_upload", "Upload Certification Document"),
                verbatimTextOutput("file_uploaded")
        ),
        tabItem(tabName = "staff_credentials",
                fluidRow(
                  box(title = "Staff Credentials", width = 12,
                      DTOutput("staff_credentials_table"))
                )
        ),
        tabItem(tabName = "logoff",
                fluidRow(
                  box(title = "Log Off", width = 12, status = "danger",
                      actionButton("confirm_logoff", "Confirm Log Off", icon = icon("sign-out-alt"), class = "btn-danger"),
                      br(), br(),
                      textOutput("logoff_message"))
                )
        )
      )
    )
  )
)

# Server
server <- function(input, output, session) {
  
  # Simulate the user login process
  logged_in <- reactiveVal(FALSE)
  user_credentials <- list(username = "admin", password = "password")  # Simulated credentials
  
  # Login UI
  output$login_ui <- renderUI({
    if (!logged_in()) {
      fluidRow(
        column(4, offset = 4,
               wellPanel(
                 textInput("username", "Username"),
                 passwordInput("password", "Password"),
                 actionButton("login_btn", "Login", class = "btn-primary")
               )
        )
      )
    }
  })
  
  # Login logic
  observeEvent(input$login_btn, {
    if (input$username == user_credentials$username && input$password == user_credentials$password) {
      logged_in(TRUE)
      updateTabItems(session, "tabs", "dashboard")  # Redirect to dashboard after login
    } else {
      showModal(modalDialog(
        title = "Login Failed",
        "Incorrect username or password.",
        easyClose = TRUE,
        footer = NULL
      ))
    }
  })
  
  # Expose logged_in status to UI
  output$logged_in <- reactive({
    logged_in()
  })
  outputOptions(output, "logged_in", suspendWhenHidden = FALSE)
  
  # Log off logic
  observeEvent(input$confirm_logoff, {
    logged_in(FALSE)
    output$logoff_message <- renderText("You have been logged off.")
    updateTextInput(session, "username", value = "")
    updateTextInput(session, "password", value = "")
  })
  
  # Dashboard metrics
  output$shifts_filled_box <- renderValueBox({
    valueBox(
      sum(shift_data$Status == "Confirmed"),
      "Shifts Filled",
      icon = icon("check-circle"),
      color = "aqua"
    )
  })
  
  output$open_shifts_box <- renderValueBox({
    valueBox(
      sum(shift_data$Status == "Pending"),
      "Open Shifts",
      icon = icon("clock"),
      color = "green"
    )
  })
  
  output$clients_served_box <- renderValueBox({
    valueBox(
      length(unique(shift_data$Name[shift_data$Status == "Confirmed"])),
      "Clients Served",
      icon = icon("user-check"),
      color = "yellow"
    )
  })
  
  # Staff table
  output$staff_table <- renderDT({
    datatable(temp_staff, options = list(pageLength = 5))
  })
  
  # Shift Management functionality
  shifts <- reactiveVal(data.frame(Name = character(), Date = character(), Time = character(), stringsAsFactors = FALSE))
  
  observeEvent(input$submit_shift, {
    req(input$name, input$date, input$time)
    new_shift <- data.frame(Name = input$name, Date = input$date, Time = input$time, stringsAsFactors = FALSE)
    shifts(rbind(shifts(), new_shift))
  })
  
  output$shift_table <- renderDT({
    datatable(shifts(), options = list(pageLength = 5))
  })
  
  # Staff Map functionality
  output$staff_map <- renderLeaflet({
    leaflet() %>%
      addTiles() %>%
      addMarkers(lng = c(-96.7969, -97.3308, -96.6989),
                 lat = c(32.7767, 32.7555, 33.0198),
                 popup = temp_staff$Name)
  })
  
  # File upload functionality
  output$file_uploaded <- renderPrint({
    if (is.null(input$cert_upload)) {
      "No file uploaded yet."
    } else {
      paste("Uploaded:", input$cert_upload$name)
    }
  })
  
  # Staff credentials table
  output$staff_credentials_table <- renderDT({
    datatable(temp_staff, options = list(pageLength = 5))
  })
}

# Run the app
shinyApp(ui = ui, server = server)
```

---

### Notes
- **Repo Name**: `StaffLinkPro`—short, descriptive, and professional, appealing to both execs and devs.
- **URL Placeholder**: Replace `https://yourname.shinyapps.io/StaffLinkPro/` with your actual deployed URL after following the deployment steps.
- **Clickable Sections**: All headers use GitHub Markdown’s `#` syntax with links (e.g., `[What It Does](#what-it-does)`), making navigation easy.
- **Tone**: Balances exec-friendly benefits (efficiency, insights) with dev-friendly details (libraries, AI tools).
- **Why This Matters**: Positions the app as a solution to real staffing pain points, broadening its appeal.

Save this as `README.md` in your `StaffLinkPro` repo root, deploy the app, update the URL, and you’re set to impress healthcare staffing execs and devs alike! Let me know if you want further polish.
