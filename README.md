# Exotic Options — Monte Carlo Pricing App

An interactive [Shiny](https://shiny.posit.co/) app for pricing vanilla and exotic options with Monte Carlo simulation, under both the Black-Scholes and the Heston (stochastic volatility) model.

![App overview](images%20for%20UI/tutorial.png)

## Features

- **10 option types**
  - Vanilla: European, American
  - Path-dependent: Asian, Barrier, Lookback
  - Time-dependent: Forward Start, Chooser
  - Other exotic: Binary, Gap, Compound
- **Two simulation models**: geometric Brownian motion (Black-Scholes) and the Heston stochastic volatility model, with a side-by-side price-path plot to compare the two
- **American options** priced via least-squares Monte Carlo (Longstaff-Schwartz regression)
- **Simulation diagnostics**: price, standard deviation, standard error, and a histogram of the discounted payoffs
- **Market-aware inputs**: current risk-free rate fetched from FRED (3-month T-bill), optional dividend yield, and maturity as either a calendar date (bank holidays excluded) or a plain year fraction
- **Input validation** with contextual warnings whenever a parameter changes after the last simulation run

## Running the app

Requires [R](https://www.r-project.org/) with the following packages:

```r
install.packages(c("shiny", "shinydashboard", "waiter", "quantmod",
                   "gridExtra", "shinyjs", "shinycssloaders", "timeDate",
                   "ggplot2", "matrixStats", "MASS"))
```

Then, from the project directory:

```r
shiny::runApp()
```

(or open `exoticoptions.Rproj` in RStudio and click *Run App*).

## Project structure

| File | Purpose |
| --- | --- |
| `app.R` | Entry point; sources the UI and server and starts the app |
| `ui.R` | Dashboard layout (`shinydashboard`), one tab per option type, formula panels |
| `server.R` | Reactive wiring: input validation, simulation triggers, plots, result boxes |
| `functions.R` | Pricing engine: path generation (Black-Scholes / Heston), payoff logic per option type, Longstaff-Schwartz for American options, helpers |

The core functions in `functions.R`:

- `create.StockPaths` — generates random stock paths under either model
- `simulate.Heston` — Heston path simulation (correlated Wiener processes via `MASS::mvrnorm`)
- `price.Option` — computes the discounted payoff for the selected option type
- `get.riskFreeRate` — pulls the latest 3-month T-bill rate from FRED
- `create.Holidays` — builds the bank-holiday calendar for date-based maturities

## Background

Built as a university group project on the valuation of exotic options.
