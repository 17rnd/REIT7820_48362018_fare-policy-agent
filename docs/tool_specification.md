# Initial Tool Specifications

## Project Context

This document defines the initial tool specifications for the REIT7820 Transport Policy & Fares agent.

The proposed agent will expose MCP-compatible tools that allow an orchestrator or other agents to:

1. query public transport patronage data;
2. simulate the effects of fare-policy changes;
3. compare multiple fare-policy scenarios.

These specifications are preliminary and may be revised after supervisor feedback and Week 3 scoping.

---

## Tool 1: `get_patronage_summary`

### Purpose

Retrieve and summarise public transport patronage for a selected time period and transport mode.

This tool uses the Translink patronage dataset to provide a reliable baseline for later fare-policy analysis.

### Intended Use

The tool can be used when another agent or an orchestrator needs to know:

- total patronage during a selected period;
- average monthly or weekly patronage;
- minimum and maximum patronage;
- patronage for a specific transport mode;
- the exact data period used in the calculation.

### Inputs

| Parameter | Type | Required | Description |
|---|---|---:|---|
| `start_date` | string | Yes | Start of the analysis period, preferably in ISO format such as `2025-01-01`. |
| `end_date` | string | Yes | End of the analysis period, preferably in ISO format such as `2025-12-31`. |
| `mode` | string | No | Public transport mode, such as `Bus`, `Train`, `Ferry`, `Light Rail`, or `All`. |

### Outputs

| Field | Type | Description |
|---|---|---|
| `total_patronage` | number | Total passenger trips in the selected period. |
| `average_patronage` | number | Average patronage per reporting period. |
| `minimum_patronage` | number | Lowest patronage value in the selected period. |
| `maximum_patronage` | number | Highest patronage value in the selected period. |
| `data_period` | object/string | Actual start and end dates used in the calculation. |
| `mode` | string | Transport mode included in the result. |
| `record_count` | integer | Number of data records used. |
| `data_source` | string | Name of the dataset used. |
| `limitations` | list[string] | Important limitations of the dataset or result. |

### Example Input

```json
{
  "start_date": "2025-01-01",
  "end_date": "2025-12-31",
  "mode": "Train"
}
```

### Validation Rules

- `start_date` must be earlier than or equal to `end_date`.
- Dates must be in a recognised format.
- `mode` must match a supported value in the dataset.
- The requested period must overlap with the available data.
- Patronage values must be numeric and non-negative.

### Error Cases

- Missing start or end date.
- Invalid date format.
- Start date later than end date.
- Unsupported transport mode.
- No records found for the requested period.
- Missing or corrupted local dataset.
- Required dataset columns are unavailable.

---

## Tool 2: `simulate_fare_policy`

### Purpose

Estimate how a percentage fare adjustment may affect public transport patronage and fare revenue under an explicit fare-elasticity assumption.

This is a scenario-analysis tool. It does not claim that the predicted result is a confirmed causal outcome.

### Intended Use

The tool can be used to evaluate questions such as:

- What may happen to patronage if fares decrease by 10%?
- What may happen to revenue if fares increase by 20%?
- How does the result change under different fare-elasticity assumptions?
- What are the trade-offs between patronage growth and fare revenue?

### Inputs

| Parameter | Type | Required | Description |
|---|---|---:|---|
| `baseline_fare` | number | Yes | Fare before the policy change. Must be greater than zero. |
| `baseline_patronage` | number | Yes | Patronage before the policy change. Must be non-negative. |
| `fare_change_percentage` | number | Yes | Percentage fare change. Negative values represent fare reductions. |
| `fare_elasticity` | number | Yes | Assumed percentage patronage response to a 1% fare change. |
| `scenario_name` | string | No | Human-readable name for the scenario. |
| `transport_mode` | string | No | Mode associated with the baseline patronage. |
| `currency` | string | No | Currency code, such as `AUD`. Default may be `AUD`. |

### Calculation Logic

```text
fare_change_rate = fare_change_percentage / 100
new_fare = baseline_fare × (1 + fare_change_rate)
patronage_change_rate = fare_elasticity × fare_change_rate
estimated_patronage = baseline_patronage × (1 + patronage_change_rate)
old_revenue = baseline_fare × baseline_patronage
estimated_revenue = new_fare × estimated_patronage
```

### Outputs

| Field | Type | Description |
|---|---|---|
| `scenario_name` | string | Name of the scenario. |
| `new_fare` | number | Fare after the policy change. |
| `estimated_patronage` | number | Estimated patronage after the fare change. |
| `old_revenue` | number | Baseline fare revenue. |
| `estimated_revenue` | number | Estimated fare revenue after the change. |
| `patronage_change_percentage` | number | Estimated percentage change in patronage. |
| `revenue_change_percentage` | number | Estimated percentage change in revenue. |
| `input_parameters` | object | Parameters used in the simulation. |
| `assumptions` | list[string] | Modelling assumptions. |
| `limitations` | list[string] | Important limitations of the result. |

### Example Input

```json
{
  "scenario_name": "Ten percent fare reduction",
  "baseline_fare": 0.50,
  "baseline_patronage": 100000,
  "fare_change_percentage": -10,
  "fare_elasticity": -0.30,
  "transport_mode": "All",
  "currency": "AUD"
}
```

### Example Output

```json
{
  "scenario_name": "Ten percent fare reduction",
  "new_fare": 0.45,
  "estimated_patronage": 103000,
  "old_revenue": 50000,
  "estimated_revenue": 46350,
  "patronage_change_percentage": 3.0,
  "revenue_change_percentage": -7.3,
  "assumptions": [
    "Fare elasticity remains constant within the tested range.",
    "Service frequency and quality remain unchanged.",
    "There is no external demand shock."
  ],
  "limitations": [
    "This is a scenario estimate rather than a causal forecast.",
    "Results depend strongly on the selected elasticity assumption."
  ]
}
```

### Validation Rules

- `baseline_fare` must be greater than zero.
- `baseline_patronage` must be zero or greater.
- `fare_change_percentage` must be greater than `-100`.
- `fare_elasticity` must be numeric.
- The resulting fare must remain greater than zero.
- The resulting patronage must not be negative.
- All required parameters must be provided.

### Error Cases

- Missing baseline fare, patronage, fare change, or elasticity.
- Baseline fare is zero or negative.
- Baseline patronage is negative.
- Fare reduction is 100% or more.
- Non-numeric elasticity.
- Estimated patronage becomes negative.

---

## Tool 3: `compare_policy_scenarios`

### Purpose

Compare the estimated patronage and revenue outcomes of multiple fare-policy scenarios using a common baseline.

This tool helps identify trade-offs rather than declaring one policy universally best.

### Intended Use

The tool can be used to compare:

- fare reductions and fare increases;
- different fare-elasticity assumptions;
- alternative policy designs;
- scenarios prioritising patronage growth;
- scenarios prioritising fare revenue.

### Inputs

| Parameter | Type | Required | Description |
|---|---|---:|---|
| `baseline_fare` | number | Yes | Common baseline fare for all scenarios. |
| `baseline_patronage` | number | Yes | Common baseline patronage for all scenarios. |
| `scenarios` | list[object] | Yes | List of two or more fare-policy scenarios. |
| `comparison_objective` | string | No | Optional objective such as `highest_patronage`, `highest_revenue`, or `balanced`. |
| `currency` | string | No | Currency code, such as `AUD`. |

Each scenario should contain:

| Scenario Field | Type | Required | Description |
|---|---|---:|---|
| `name` | string | Yes | Unique scenario name. |
| `fare_change_percentage` | number | Yes | Percentage fare change. |
| `fare_elasticity` | number | Yes | Elasticity assumption for that scenario. |
| `description` | string | No | Additional explanation of the policy. |

### Outputs

| Field | Type | Description |
|---|---|---|
| `scenario_results` | list[object] | Full result for each scenario. |
| `highest_patronage_scenario` | string/object | Scenario with the highest estimated patronage. |
| `highest_revenue_scenario` | string/object | Scenario with the highest estimated revenue. |
| `lowest_fare_scenario` | string/object | Scenario with the lowest estimated fare. |
| `trade_off_summary` | object/string | Summary of the patronage–revenue trade-off. |
| `ranking_basis` | string | Criterion used for any ranking. |
| `assumptions` | list[string] | Shared assumptions across scenarios. |
| `limitations` | list[string] | Important limitations of the comparison. |

### Example Input

```json
{
  "baseline_fare": 0.50,
  "baseline_patronage": 100000,
  "currency": "AUD",
  "comparison_objective": "balanced",
  "scenarios": [
    {
      "name": "Ten percent fare reduction",
      "fare_change_percentage": -10,
      "fare_elasticity": -0.30
    },
    {
      "name": "Ten percent fare increase",
      "fare_change_percentage": 10,
      "fare_elasticity": -0.30
    },
    {
      "name": "No fare change",
      "fare_change_percentage": 0,
      "fare_elasticity": -0.30
    }
  ]
}
```

### Validation Rules

- At least two scenarios must be provided.
- Every scenario must have a unique non-empty name.
- Every scenario must include a valid fare-change percentage.
- Every scenario must include a numeric elasticity value.
- The common baseline fare must be greater than zero.
- The common baseline patronage must be non-negative.
- Invalid scenarios should be reported clearly rather than silently ignored.

### Error Cases

- Fewer than two scenarios.
- Duplicate scenario names.
- Missing scenario parameters.
- Invalid baseline values.
- Fare reduction of 100% or more.
- Non-numeric elasticity.
- Unsupported comparison objective.
- One or more scenarios produce invalid results.

---

## Shared Design Principles

All three tools should follow these principles:

1. **Structured output** — return machine-readable results for an orchestrator or another agent.
2. **Explicit assumptions** — state all assumptions behind calculations.
3. **Clear limitations** — do not present scenario estimates as confirmed causal effects.
4. **Input validation** — reject invalid values with informative errors.
5. **Reproducibility** — return input parameters, data period, and data source.
6. **Interoperability** — use stable field names and consistent data types.
7. **Separation of responsibilities** — observed-data retrieval, single-scenario simulation, and multi-scenario comparison remain separate.

## Relationship Between Tools

```text
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
        | compares multiple results
        v
Orchestrator / Other Smart-Mobility Agents
```

## Current Status

These specifications are an initial Week 2 draft. They should be revised during Week 3 scoping after confirming:

- final dataset fields;
- approved capability slot;
- acceptable fare-elasticity sources;
- MCP starter-kit conventions;
- required input and output schemas;
- course evaluation requirements.
