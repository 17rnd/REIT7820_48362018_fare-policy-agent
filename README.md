# MCP-Based AI Agent for Spatial Fare Equity and Policy Scenario Analysis

This project develops an MCP-based AI decision-support agent for public transport fare-policy analysis, using Queensland's 50-cent flat fare as the initial case study.

The agent is designed for transport policy analysts who need to evaluate how fare-policy benefits are distributed across geographic areas and socioeconomic groups, and to compare alternative fare scenarios.

Rather than using an LLM to generate numerical results directly, the system uses deterministic analytical tools exposed through MCP. The AI agent interprets natural-language policy questions, selects and combines the appropriate tools, and synthesises the resulting evidence into an interpretable policy response.

## Background

Queensland introduced a universal 50-cent flat fare across the Translink network, replacing the previous multi-zone fare structure.

Although the new fare substantially reduces travel costs, the benefits of the policy may not be distributed evenly across different travel distances, geographic areas, transport modes, and socioeconomic groups.

Long-distance passengers generally receive larger savings per trip, but this does not necessarily mean that the areas receiving the largest total policy benefits are also the most disadvantaged areas.

Evaluating this question requires combining multiple datasets, including origin-destination travel data, historical fare structures, spatial information, and socioeconomic indicators.

## Research Gap

Existing public transport fare-policy studies often rely on aggregate indicators such as total patronage, average fare changes, or average fare elasticities.

However, these aggregate measures can hide important differences across:

- geographic areas
- trip distances
- transport modes
- socioeconomic groups

The literature also shows that the equity effects of flat fares are highly context-dependent. Therefore, the impact of Queensland's 50-cent fare should be evaluated using local travel-demand and spatial socioeconomic data rather than assuming that the policy is uniformly beneficial.

## Project Idea

The proposed system is an MCP-based AI agent that allows transport policy analysts to ask natural-language questions about fare-policy impacts.

The agent will interpret the user's question, determine which analytical tools and datasets are required, execute the relevant analysis through MCP tools, and then explain the results and limitations.

Queensland's 50-cent fare will be used as the first case study.

Example user questions include:

- Which geographic areas receive the largest realised benefits from the 50-cent fare?
- Which travel-distance groups benefit most from the policy?
- Are larger fare benefits concentrated in socioeconomically disadvantaged areas?
- How do fare benefits differ across transport modes?
- How would the distribution of benefits change under a $1 flat fare?
- Would a targeted concession policy produce a more equitable distribution of benefits?

## Planned Capabilities

### 1. Fare Benefit Analysis

Calculate the direct fare saving for observed origin-destination trips by comparing the historical fare with a selected policy fare.

The main calculation is:

`fare saving per trip = historical fare - scenario fare`

and:

`realised fare benefit = fare saving per trip × observed trip quantity`

### 2. Spatial Fare Equity Analysis

Analyse how realised fare benefits are distributed across:

- geographic areas
- fare zones
- travel-distance groups
- transport modes

### 3. Socioeconomic Equity Analysis

Combine fare-benefit results with ABS socioeconomic indicators such as SEIFA, income, population, and car ownership.

This capability will assess whether larger realised fare benefits are concentrated in more disadvantaged areas.

### 4. Policy Scenario Comparison

Compare alternative fare-policy scenarios, such as:

- $0.50 flat fare
- $1 flat fare
- $2 flat fare
- targeted concession policies

Initial scenario comparisons will hold observed travel demand constant rather than assuming a specific demand response.

## Current Data

The project currently includes or has identified the following datasets:

- Translink Origin-Destination Trips
- Translink Origin-Destination Stops
- Reconstructed South East Queensland historical fare-zone polygons
- Historical South East Queensland fare structure
- Planned: ABS SA2 boundaries
- Planned: ABS SEIFA socioeconomic indicators
- Planned: ABS population, income, and car-ownership data

## Current Progress

The current project scope has been refined from a simple fare-elasticity simulation approach to a multi-tool AI policy-analysis agent.

The following progress has been completed:

- reviewed literature on fare elasticity, fare equity, flat fares, and fare-free public transport
- identified spatial and distributional fare equity as the main research direction
- obtained Translink origin-destination trip data
- obtained Translink stop-location data
- validated the connection between OD records and stop locations
- identified the historical eight-zone fare structure
- obtained reconstructed spatial polygons for the historical fare zones
- confirmed that the fare-zone polygons can be used to map the large majority of Translink stops to historical fare zones

The next steps are:

- validate the final pre-50-cent historical fare table
- confirm historical fare rules for boundary zones such as `1/2` and `2/3`
- obtain ABS SA2 boundaries
- obtain ABS SEIFA data
- build the first deterministic MCP analysis tools
- implement the AI agent layer for tool selection and policy-question interpretation

## Primary User

The primary user is a transport policy analyst working for a government agency, public transport authority, or transport-planning organisation.

The agent is intended to reduce the need for analysts to manually combine multiple datasets and write separate analysis scripts for every new policy question.
