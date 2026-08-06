# Capability Slot Proposal

## Project Context

This document proposes several possible capability slots for the REIT7820 Transport Policy & Fares domain.

The purpose of the capability slot is to define the specific role that the proposed agent will perform within the Smart Mobility Agent Collective.

The preferred direction is to build an MCP-compatible agent that evaluates public transport fare-policy scenarios using observed patronage data, fare assumptions, and elasticity-based modelling.

These capability-slot proposals are preliminary and should be reviewed during the Week 2 workshop and finalised during Week 3 scoping.

---

# Option 1: Fare Policy Scenario Evaluation

## Capability Slot Name

**Fare Policy Scenario Evaluation**

## Short Description

An MCP-compatible agent capability that estimates how public transport fare changes may affect patronage and fare revenue under explicit fare-elasticity assumptions.

## Main Purpose

The purpose of this capability is to support reproducible comparison of alternative public transport fare policies.

The agent receives baseline fare and patronage information, applies a proposed fare change, and estimates:

- the new fare;
- the expected patronage response;
- the estimated fare revenue;
- percentage changes in patronage and revenue;
- the assumptions and limitations behind the result.

## Example Questions

The capability should support questions such as:

- What may happen to patronage if fares are reduced by 10%?
- What may happen to fare revenue if fares increase by 20%?
- Which scenario produces the highest estimated patronage?
- Which scenario produces the highest estimated revenue?
- How sensitive are the results to different fare-elasticity assumptions?

## Proposed Tools

- `simulate_fare_policy`
- `compare_policy_scenarios`
- `run_elasticity_sensitivity_analysis`

## Required Inputs

Typical inputs include:

- baseline fare;
- baseline patronage;
- fare-change percentage;
- fare-elasticity assumption;
- scenario name;
- optional transport mode;
- optional analysis period.

## Expected Outputs

Typical outputs include:

- new fare;
- estimated patronage;
- estimated revenue;
- patronage change percentage;
- revenue change percentage;
- scenario ranking;
- assumptions;
- limitations.

## Data Requirements

This capability may use:

- Translink South East Queensland monthly patronage data;
- official fare-policy information;
- fare-elasticity estimates from academic literature;
- optional complaint or customer-experience data as an extension.

## Strengths

- Clearly aligned with the Transport Policy & Fares domain.
- Scope is manageable within one semester.
- Produces structured outputs suitable for MCP interoperability.
- Supports quantitative analysis.
- Can be evaluated using deterministic calculations and sensitivity analysis.
- Has useful connections to pricing, forecasting, scenario analysis, and future banking or quantitative-analysis work.

## Limitations

- Results depend on the selected fare-elasticity assumptions.
- Scenario estimates do not prove causal policy effects.
- Aggregated patronage data may not capture passenger-level behaviour.
- Revenue estimates may omit concessions, transfer rules, caps, and operating costs.

## Difficulty

**Estimated difficulty: Medium**

The main technical requirements are:

- Python;
- pandas;
- basic statistics;
- fare-elasticity modelling;
- structured tool design;
- MCP integration;
- testing and validation.

## Recommendation

This is the recommended primary capability slot because it is focused, technically meaningful, and feasible.

---

# Option 2: Patronage Trend and Policy-Period Analysis

## Capability Slot Name

**Patronage Trend and Policy-Period Analysis**

## Short Description

An MCP-compatible agent capability that summarises observed public transport patronage and compares patronage across selected time periods, transport modes, or policy periods.

## Main Purpose

The purpose of this capability is to provide evidence from real Translink data before any policy simulation is performed.

The agent can retrieve patronage statistics, identify trends, and compare periods such as before and after the introduction of a fare-policy change.

## Example Questions

The capability should support questions such as:

- What was the average monthly bus patronage in 2025?
- Which transport mode had the highest patronage growth?
- How did patronage change before and after a selected policy date?
- What were the minimum and maximum monthly patronage values?
- Did patronage increase after the introduction of a fare-policy change?

## Proposed Tools

- `get_patronage_summary`
- `compare_patronage_periods`
- `analyse_mode_trends`

## Required Inputs

Typical inputs include:

- start date;
- end date;
- transport mode;
- comparison period;
- aggregation level.

## Expected Outputs

Typical outputs include:

- total patronage;
- average patronage;
- minimum and maximum patronage;
- percentage change between periods;
- transport-mode comparison;
- data period;
- data source;
- limitations.

## Data Requirements

This capability mainly uses:

- Translink South East Queensland monthly patronage data;
- optional weekly patronage and complaints data.

## Strengths

- Uses real observed data.
- Easy to validate.
- Provides an evidence-based baseline for later policy modelling.
- Useful to other agents that require patronage information.
- Lower modelling risk than direct forecasting.

## Limitations

- Descriptive analysis cannot establish causality.
- Policy-period comparisons may be influenced by seasonality, service changes, weather, population growth, or external events.
- Aggregated data limits passenger-group analysis.
- This capability alone may be less research-intensive than policy simulation.

## Difficulty

**Estimated difficulty: Low to Medium**

The main technical requirements are:

- Python;
- pandas;
- date filtering;
- data aggregation;
- descriptive statistics;
- structured MCP output.

## Recommendation

This is a strong supporting capability and can be combined with Option 1.

It is probably better as a secondary capability than as the only capability slot.

---

# Option 3: Patronage and Complaints Impact Analysis

## Capability Slot Name

**Patronage and Complaints Impact Analysis**

## Short Description

An MCP-compatible agent capability that analyses whether changes in patronage are associated with changes in complaint volumes or complaint rates.

## Main Purpose

The purpose of this capability is to extend fare-policy evaluation beyond patronage and revenue by including a simple service-quality or customer-impact indicator.

The agent may compare:

- patronage trends;
- complaint counts;
- complaints per 10,000 trips;
- periods before and after a selected policy change.

## Example Questions

The capability should support questions such as:

- Did complaint rates increase when patronage increased?
- Which period had the highest complaints per 10,000 trips?
- How did complaints change after a selected fare-policy period?
- Was higher patronage associated with higher complaint rates?

## Proposed Tools

- `get_complaint_summary`
- `compare_patronage_and_complaints`
- `analyse_complaint_rate`

## Required Inputs

Typical inputs include:

- start date;
- end date;
- complaint category;
- comparison period;
- optional transport mode, if supported.

## Expected Outputs

Typical outputs include:

- total complaints;
- complaint rate;
- total patronage;
- change in complaint rate;
- correlation or descriptive relationship;
- assumptions;
- limitations.

## Data Requirements

This capability may use:

- Translink patronage and complaints data;
- monthly patronage data;
- optional customer-experience indicators.

## Strengths

- Adds a service-quality perspective.
- Makes policy evaluation broader than patronage and revenue alone.
- Can produce useful evidence for decision-makers.
- Provides an optional extension if the primary capability is completed early.

## Limitations

- Complaints are an imperfect measure of service quality.
- Reporting behaviour may change over time.
- A correlation between patronage and complaints does not imply causation.
- Complaint categories may not be directly related to fare policy.
- Data compatibility and time granularity need to be checked.

## Difficulty

**Estimated difficulty: Medium**

The main technical requirements are:

- Python;
- pandas;
- time-series alignment;
- rate calculations;
- descriptive statistics;
- optional correlation analysis;
- MCP tool integration.

## Recommendation

This should be treated as an optional extension rather than the primary capability slot.

---

# Preferred Capability-Slot Design

## Recommended Primary Capability

**Fare Policy Scenario Evaluation**

## Recommended Supporting Capability

**Patronage Trend and Policy-Period Analysis**

## Optional Extension

**Patronage and Complaints Impact Analysis**

## Recommended Combined Scope

The final project can be defined as:

> An MCP-compatible public transport fare-policy analysis agent that retrieves observed patronage summaries, evaluates alternative fare-policy scenarios, and compares their estimated patronage and revenue outcomes under explicit fare-elasticity assumptions.

This combined scope keeps the project focused while still using real data.

---

# Recommended Capability Flow

```text
Observed Translink Patronage Data
               |
               v
get_patronage_summary
               |
               | provides baseline patronage
               v
simulate_fare_policy
               |
               | produces one scenario result
               v
compare_policy_scenarios
               |
               | compares multiple alternatives
               v
run_elasticity_sensitivity_analysis
               |
               v
Orchestrator / Other Smart-Mobility Agents
```

An optional extension may add:

```text
Patronage and Complaints Data
               |
               v
compare_patronage_and_complaints
```

---

# Preliminary Research Question

A suitable preliminary research question is:

> How can an MCP-based agent support reproducible evaluation of public transport fare-policy scenarios using observed patronage data, fare-elasticity assumptions, and revenue indicators?

Possible supporting questions are:

1. How can observed Translink patronage data be used to define reliable baseline inputs?
2. How do alternative fare changes affect estimated patronage and fare revenue?
3. How sensitive are policy results to different fare-elasticity assumptions?
4. What are the main trade-offs between maximising patronage and maximising fare revenue?

---

# Interoperability Value

Within the Smart Mobility Agent Collective, this capability may support other agents by providing:

- patronage baselines for a selected period;
- estimated fare-policy impacts;
- revenue and patronage trade-off information;
- structured scenario comparisons;
- assumptions and limitations that can be incorporated into a larger response.

For example:

- a journey-planning agent may provide trip information;
- a policy agent may evaluate how an alternative fare affects demand;
- an orchestrator may combine routing, charging, public transport, and fare-policy information into a composite response.

---

# Evaluation Plan

The capability can be evaluated using:

## Functional Correctness

- correctness of patronage summaries;
- correctness of fare and revenue calculations;
- unit-test pass rate;
- validation of boundary inputs.

## Scenario Consistency

- identical inputs produce identical outputs;
- a 0% fare change returns the baseline result;
- lower fares with negative elasticity increase estimated patronage;
- higher fares with negative elasticity reduce estimated patronage.

## Sensitivity Analysis

Test multiple elasticity values, such as:

```text
-0.2
-0.3
-0.4
-0.5
-0.6
```

Compare how the estimated patronage, revenue, and scenario ranking change.

## MCP Interoperability

- server starts successfully;
- tools are discoverable;
- input schemas are valid;
- outputs are machine-readable;
- invalid inputs return informative errors;
- an external client can successfully call the tools.

## Performance

- average response time;
- tool-call success rate;
- failure rate under invalid inputs;
- behaviour when the local dataset is unavailable.

---

# Current Risks and Open Questions

The following issues should be confirmed with the teaching team:

1. Is fare-policy scenario evaluation an acceptable primary capability slot?
2. Are literature-based fare-elasticity values acceptable if historical fare variation is limited?
3. Is the Translink monthly patronage dataset sufficient as the main observed dataset?
4. Is comparison of the 50-cent fare policy with hypothetical alternatives acceptable?
5. Should patronage retrieval and policy simulation be treated as one capability slot or two?
6. Are there required MCP naming or schema conventions?
7. Is complaint analysis appropriate as an optional extension?
8. What level of evaluation is expected for Week 3 scoping and Week 7 Sprint Review?

---

# Final Preliminary Recommendation

The proposed project should proceed with:

## Primary Capability Slot

**Fare Policy Scenario Evaluation**

## Supporting Capability

**Patronage Trend and Policy-Period Analysis**

## Optional Extension

**Patronage and Complaints Impact Analysis**

This structure provides a clear semester scope:

- real data retrieval;
- quantitative policy simulation;
- scenario comparison;
- sensitivity analysis;
- MCP interoperability;
- optional service-quality analysis.

The scope should remain limited to aggregated policy analysis and should not initially include:

- individual passenger prediction;
- full causal inference;
- deep-learning models;
- real-time journey planning;
- national fare integration;
- complex spatial-equity analysis.
