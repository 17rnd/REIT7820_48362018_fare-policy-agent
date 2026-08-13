# Project Idea

## Project Title

**MCP-Based AI Agent for Spatial Fare Equity and Policy Scenario Analysis**

Case Study: Queensland's 50-Cent Flat Fare

---

## Overview

This project proposes an MCP-based AI decision-support agent for public transport fare-policy analysis.

The system is designed to help transport policy analysts investigate how fare-policy benefits are distributed across different geographic areas, travel patterns, transport modes, and socioeconomic groups.

Queensland's 50-cent flat fare will be used as the initial case study.

Instead of relying on the language model to directly generate numerical results, the project will use deterministic analytical tools exposed through the Model Context Protocol (MCP).

The AI agent will interpret natural-language policy questions, determine which analytical tools are required, call those tools in an appropriate sequence, and then synthesise the results into an interpretable policy response.

---

## Background

Before the introduction of Queensland's 50-cent fare, public transport fares in South East Queensland were based on a multi-zone fare structure.

Under the new policy, most Translink public transport trips are charged a flat fare of $0.50.

Although the policy reduces travel costs substantially, the size and distribution of these benefits may vary across different journeys and locations.

For example, a long-distance passenger may receive a much larger fare saving per trip than a short-distance passenger.

However, a large saving per trip does not necessarily mean that an area receives a large overall policy benefit.

The total realised benefit also depends on how many trips are actually made.

Therefore, fare-policy analysis should consider both:

- fare saving per trip
- observed travel demand

The project will use origin-destination travel data, historical fare structures, spatial data, and socioeconomic indicators to analyse these differences.

---

## Problem

Public transport fare-policy evaluation is a multi-step analytical task.

A policy analyst may need to combine:

- origin-destination trip data
- public transport stop locations
- historical fare zones
- historical fare tables
- geographic boundaries
- socioeconomic indicators
- alternative fare-policy assumptions

These datasets are often stored separately and require different analytical methods.

For each new policy question, analysts may need to manually filter datasets, perform spatial joins, calculate fares, aggregate results, and write new analysis scripts.

This makes policy exploration slow and difficult, particularly when the question requires several analytical steps.

---

## Proposed Solution

The proposed solution is an AI agent that acts as a natural-language decision-support interface over a set of deterministic MCP analytical tools.

A policy analyst will be able to ask questions such as:

> Which geographic areas received the largest realised benefits from the 50-cent fare?

or:

> Were the largest fare benefits concentrated in socioeconomically disadvantaged areas?

The agent will interpret the question and determine which analytical operations are required.

For example, the agent may:

1. retrieve relevant origin-destination trips;
2. identify the origin and destination fare zones;
3. reconstruct the historical fare;
4. calculate the fare saving for each trip;
5. multiply the saving by observed trip quantity;
6. aggregate benefits by geographic area;
7. retrieve socioeconomic indicators;
8. compare fare benefits with socioeconomic disadvantage;
9. explain the results and analytical limitations.

This allows the same system to answer different policy questions without requiring a separate manually written workflow for every question.

---

## Main Research Question

The main analytical question for the Queensland case study is:

**How does Queensland's 50-cent flat fare redistribute realised fare benefits across the public transport network, and to what extent are these benefits aligned with socioeconomic disadvantage?**

The project will focus primarily on spatial and distributional differences rather than only aggregate patronage changes.

---

## Target User

The primary user is a transport policy analyst working in:

- a government transport department
- a public transport authority
- a transport planning organisation
- a policy research organisation

The user may have transport-policy knowledge but should not need to manually combine every dataset or write a new analysis script for each policy question.

---

## Example User Scenario

A transport policy analyst is reviewing the impact of Queensland's 50-cent fare.

The analyst asks:

> Which outer-suburban areas benefited most from the 50-cent fare, and were those benefits concentrated in disadvantaged communities?

The AI agent identifies that the question requires both spatial and socioeconomic analysis.

It then selects and combines the appropriate MCP tools to:

- retrieve observed OD trips
- reconstruct historical fares
- calculate realised fare benefits
- aggregate benefits by area
- obtain socioeconomic indicators
- compare benefit distribution with disadvantage

The agent then provides a structured explanation of the results and limitations.

---

## Core Analysis

The main fare-benefit calculation will be:

`fare_saving_per_trip = historical_fare - scenario_fare`

The realised fare benefit will then be calculated as:

`realised_fare_benefit = fare_saving_per_trip × observed_trip_quantity`

This distinction is important.

A journey may have a very large theoretical saving per trip, but if very few passengers make that journey, its total realised policy benefit may still be relatively small.

---

## Planned Policy Questions

The system may support questions such as:

- Which areas receive the largest total realised fare benefits?
- Which areas receive the largest average saving per trip?
- How do benefits differ between inner-city and outer-suburban areas?
- How do benefits differ across fare zones?
- How do benefits differ across transport modes?
- Are higher benefits associated with greater socioeconomic disadvantage?
- Which disadvantaged areas receive relatively low benefits?
- How would benefit distribution change under a $1 flat fare?
- How would benefit distribution change under a $2 flat fare?
- Would a targeted concession policy distribute benefits differently from a universal flat fare?

---

## Policy Scenario Analysis

The system will also support comparison between alternative fare-policy scenarios.

Possible scenarios include:

- $0.50 flat fare
- $1 flat fare
- $2 flat fare
- targeted concession fares
- alternative zone-based structures

In the initial version of the project, scenario comparisons will hold observed travel demand constant.

The project will therefore analyse how the monetary distribution of benefits changes under different fare rules without assuming a particular passenger-demand response.

Demand modelling or elasticity-based forecasting may be considered as a future extension.

---

## Role of the AI Agent

The AI agent is not intended to replace deterministic numerical analysis.

The language model will mainly be responsible for:

- understanding the user's policy question
- identifying the analytical task
- selecting appropriate MCP tools
- deciding the order in which tools should be called
- combining outputs from multiple tools
- explaining the results
- communicating limitations and assumptions

Numerical calculations, fare reconstruction, spatial aggregation, and statistical operations will be performed by deterministic tools.

This separation reduces the risk of the language model generating unsupported numerical results.

---

## Data Sources

The current project uses or plans to use:

- Translink origin-destination trip data
- Translink origin-destination stop data
- reconstructed South East Queensland historical fare-zone polygons
- historical Translink fare tables
- ABS SA2 geographic boundaries
- ABS SEIFA socioeconomic indicators
- ABS population, income, and car-ownership data

Additional public transport service data may be added later if required.

---

## Expected Contribution

The main contribution of the project is not simply to determine whether Queensland's 50-cent fare is beneficial.

Instead, the project demonstrates how an MCP-based AI agent can support complex public transport policy analysis by dynamically combining multiple analytical tools and datasets.

The Queensland 50-cent fare provides a practical case study for demonstrating this architecture.

The same agent design could later be adapted to analyse other fare-policy scenarios or public transport networks.
