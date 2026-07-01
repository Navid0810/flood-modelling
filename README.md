# flood-modelling
# Scotland River Flood Catastrophe Risk Model

## Overview

This project implements a simplified catastrophe risk model for river flooding across Scotland using publicly available geospatial hazard data and population data. The objective is to estimate the expected financial impact of flooding on a synthetic portfolio of residential properties by combining hazard, exposure, and vulnerability modelling with Monte Carlo simulation.

Unlike a simple flood mapping exercise, this project estimates monetary losses and portfolio risk metrics commonly used within insurance, actuarial science, catastrophe modelling, and quantitative risk management.

The model produces:

* Property-level Expected Annual Loss (EAL)
* Portfolio Average Annual Loss (AAL)
* Value at Risk (VaR)
* Exceedance Probability (EP) curves
* Spatial visualisations of flood risk
* Portfolio-level loss distributions

---

# Motivation

Flooding is one of the largest natural catastrophe risks faced by insurers in the United Kingdom.

Insurers are not only interested in identifying whether a property is exposed to flooding, but also in estimating:

* the probability of flooding,
* the expected financial loss,
* the uncertainty surrounding future losses,
* and the aggregate exposure of an entire insured portfolio.

This project demonstrates the core workflow of catastrophe risk modelling by combining geospatial analysis with stochastic simulation and financial loss estimation.

Although simplified, the overall modelling framework closely resembles the architecture used within commercial catastrophe modelling software.

---

# Data Sources

## Flood Hazard

Scottish Environment Protection Agency (SEPA)

* River flood extent
* Flood depth
* Flow velocity
* Three flood likelihood scenarios:

  * High likelihood (approximately 1-in-10 year)
  * Medium likelihood (approximately 1-in-200 year)
  * Low likelihood (approximately 1-in-1000 year)

## Population

UK Residential Population Raster (2021)

Used to generate a realistic spatial distribution of synthetic residential properties.

## Rivers

Ordnance Survey Open Rivers

Used for visualisation.

## Administrative Boundaries

UK country boundary dataset used to clip analysis to Scotland.

---

# Model Architecture

The project follows a simplified catastrophe modelling pipeline:

Raw geospatial datasets

↓

Synthetic exposure generation

↓

Hazard modelling

↓

Vulnerability modelling

↓

Financial loss estimation

↓

Portfolio aggregation

↓

Monte Carlo simulation

↓

Portfolio risk metrics

---

# Methodology

## 1. Synthetic Property Portfolio

A portfolio of synthetic residential properties is generated.

Rather than placing properties uniformly across Scotland, locations are sampled probabilistically from the 2021 residential population raster, resulting in higher concentrations of properties in urban areas and fewer in sparsely populated regions.

Each property is assigned a synthetic market value sampled from a lognormal distribution representing the right-skewed nature of residential property prices.

---

## 2. Hazard Modelling

For each property, the model evaluates exposure under three SEPA flood scenarios:

* High likelihood
* Medium likelihood
* Low likelihood

Spatial joins determine whether each property lies within flood extent polygons and retrieve corresponding flood depth and flow velocity information.

---

## 3. Vulnerability Modelling

Flood depth is converted into a percentage loss using a simplified stage-damage (depth-damage) curve.

Damage increases non-linearly with flood depth before approaching a maximum damage ratio.

Flow velocity is treated as a multiplicative modifier, reflecting the increased structural damage caused by fast-moving water.

The resulting damage ratio is applied to each property's value to estimate financial loss.

---

## 4. Expected Annual Loss

Each flood scenario is assigned an annual probability based on its return period.

Expected Annual Loss is calculated as:

Expected Annual Loss = Σ (Annual Probability × Scenario Loss)

This provides the long-run expected yearly loss for every property.

Portfolio EAL is calculated by summing property-level values.

---

## 5. Monte Carlo Simulation

To model annual variability, the project simulates 10,000 independent years.

For every simulated year:

* flood occurrence is sampled probabilistically,
* property losses are determined,
* portfolio losses are aggregated.

The resulting empirical loss distribution is used to estimate:

* Average Annual Loss (AAL)
* Value at Risk (95% and 99%)
* Maximum simulated annual loss
* Exceedance probabilities

---

# Outputs

The project produces:

* Property-level Expected Annual Loss
* Property-level damage ratios
* Flood exposure classifications
* Portfolio statistics
* Risk band classifications
* Spatial flood risk maps
* Annual loss histogram
* Exceedance Probability curve
* CSV portfolio export
* CSV containing simulated annual portfolio losses

---

# Technologies

* Python
* GeoPandas
* NumPy
* Pandas
* Rasterio
* Shapely
* Matplotlib
* Pyogrio

---

# Assumptions and Simplifications

This project is intended as an educational demonstration rather than a production catastrophe model.

Several simplifying assumptions are made, including:

* synthetic rather than real insured properties,
* illustrative depth-damage curves,
* simplified velocity adjustment,
* independent annual scenario occurrence,
* synthetic property value distribution,
* no calibration using historical insurance claims,
* no uncertainty analysis or parameter estimation.

These simplifications make the model computationally tractable while preserving the overall catastrophe modelling workflow.

---

# Possible Future Improvements

Potential extensions include:

* calibration of damage curves using published claims data,
* spatially varying property value distributions,
* correlated flood event generation,
* event catalogue simulation,
* climate change scenario analysis,
* sensitivity analysis,
* Expected Shortfall and Tail Value at Risk,
* Bayesian parameter estimation,
* parallel Monte Carlo simulation,
* GPU acceleration,
* machine learning damage models,
* validation against historical flood events.
