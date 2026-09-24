# KPI Definitions & DAX Documentation
## Ikeja Electric Decision Intelligence POC

This document provides the business definitions, calculation logic, exact DAX measures, interpretation rules and analytical usage of the principal KPIs used in the **Ikeja Electric Decision Intelligence POC**.

The project combines two complementary analytical perspectives:

1. **Commercial Performance Intelligence** – energy offtake, energy billing, revenue collection, efficiency and indicative ATC&C performance.
2. **Band A Service Intelligence** – feeder-level service events, recurring exceptions, voltage-level patterns and source-substation concentration.

The Power BI report presents these perspectives through three management views:

1. Executive Commercial Performance
2. Band A Service Intelligence
3. Executive Exception Centre

---

# 1. Commercial Performance KPIs

## Energy Offtake GWh

### Business Definition

Total electrical energy received/offtaken during the selected reporting period.

### Formula

```text
Energy Offtake GWh =
Sum of Energy Offtake (GWh)
```

### Exact DAX

```DAX
Energy Offtake GWh = 
SUM(FactCommercial[Energy Offtake (GWh)])
```

### Unit

GWh

### Interpretation

Provides the starting energy-volume reference for evaluating how effectively received energy is subsequently converted into billed energy and collected revenue.

---

## Energy Billed GWh

### Business Definition

Total electrical energy billed to customers during the selected reporting period.

### Formula

```text
Energy Billed GWh =
Sum of Energy Billed (GWh)
```

### Exact DAX

```DAX
Energy Billed GWh = 
SUM(FactCommercial[Energy Billed (GWh)])
```

### Unit

GWh

### Interpretation

Represents the portion of energy reflected in customer billing and provides the numerator for Energy Accounting Efficiency.

---

## Energy Accounting Efficiency

### Business Definition

Measures the proportion of energy received/offtaken that is converted into billed energy.

### Formula

```text
Energy Accounting Efficiency =
Energy Billed GWh ÷ Energy Offtake GWh
```

### Exact DAX

```DAX
Energy Accounting Efficiency = 
DIVIDE(
    [Energy Billed GWh],
    [Energy Offtake GWh]
)
```

### Unit

Percentage

### Interpretation

A higher percentage indicates that a greater proportion of energy received is represented within customer billing.

A lower percentage indicates greater energy-accounting loss pressure.

---

## Total Billing ₦bn

### Business Definition

Total monetary value billed to customers during the selected reporting period.

### Formula

```text
Total Billing ₦bn =
Sum of Total Billing (₦bn)
```

### Exact DAX

```DAX
Total Billing ₦bn = 
SUM(FactCommercial[Total Billing (₦bn)])
```

### Unit

₦ billion

### Interpretation

Represents the monetary value billed to customers and provides the denominator for Collection Efficiency.

---

## Revenue Collected ₦bn

### Business Definition

Total customer revenue collected during the selected reporting period.

### Formula

```text
Revenue Collected ₦bn =
Sum of Revenue Collected (₦bn)
```

### Exact DAX

```DAX
Revenue Collected ₦bn = 
SUM(FactCommercial[Revenue Collected (₦bn)])
```

### Unit

₦ billion

### Interpretation

Represents the amount of billed value converted into actual revenue collection during the reporting period.

---

## Collection Efficiency

### Business Definition

Measures the proportion of billed monetary value successfully converted into collected revenue.

### Formula

```text
Collection Efficiency =
Revenue Collected ₦bn ÷ Total Billing ₦bn
```

### Exact DAX

```DAX
Collection Efficiency = 
DIVIDE(
    [Revenue Collected ₦bn],
    [Total Billing ₦bn]
)
```

### Unit

Percentage

### Interpretation

A higher percentage indicates stronger conversion of customer billing into actual revenue collection.

A lower percentage indicates greater collection-performance pressure.

---

# 2. Indicative ATC&C

## Indicative ATC&C

### Business Definition

An analytical estimate of Aggregate Technical, Commercial and Collection loss derived from Energy Accounting Efficiency and Collection Efficiency.

The project deliberately labels this measure **Indicative ATC&C** because it is intended as a decision-intelligence indicator based on the available dataset rather than an independently audited regulatory ATC&C figure.

### Formula

```text
Indicative ATC&C =
1 - (Energy Accounting Efficiency × Collection Efficiency)
```

### Exact DAX

```DAX
Indicative ATC&C = 
1 -
(
    [Energy Accounting Efficiency] *
    [Collection Efficiency]
)
```

### Unit

Percentage

### Interpretation

A lower percentage indicates stronger overall conversion of energy received into collected revenue.

A higher percentage indicates greater combined loss pressure across the energy-accounting and revenue-collection chain.

### KPI Dependency

```text
Energy Offtake
      ↓
Energy Billed
      ↓
Energy Accounting Efficiency
             +
Total Billing
      ↓
Revenue Collected
      ↓
Collection Efficiency
             ↓
      Indicative ATC&C
```

---

# 3. ATC&C Benchmarking

## ATC&C Target

### Business Definition

Average applicable ATC&C target within the selected reporting context.

### Formula

```text
ATC&C Target =
Average of Target ATC&C %
```

### Exact DAX

```DAX
ATC&C Target = 
AVERAGE(FactATC[Target ATC&C %])
```

### Unit

Percentage

### Interpretation

Provides the benchmark against which Indicative ATC&C performance is evaluated.

Because this measure uses `AVERAGE`, the displayed target responds to the applicable filter context.

---

## ATC&C Variance to Target

### Business Definition

Measures the difference between Indicative ATC&C and the ATC&C Target.

### Formula

```text
ATC&C Variance to Target =
(Indicative ATC&C - ATC&C Target) × 100
```

### Exact DAX

```DAX
ATC&C Variance to Target = 
([Indicative ATC&C] - [ATC&C Target]) * 100
```

### Unit

Percentage points

### Interpretation

```text
Positive Variance
→ Indicative ATC&C is above target

Zero Variance
→ Indicative ATC&C is on target

Negative Variance
→ Indicative ATC&C is below target
```

Because lower ATC&C represents stronger performance, a positive variance indicates an adverse performance gap relative to target.

The multiplication by `100` converts the difference between the decimal percentage measures into percentage points for presentation.

---

# 4. Revenue Performance

## Revenue Gap ₦bn

### Business Definition

Measures the difference between total customer billing and actual revenue collected during the selected reporting period.

### Formula

```text
Revenue Gap ₦bn =
Total Billing ₦bn - Revenue Collected ₦bn
```

### Exact DAX

```DAX
Revenue Gap ₦bn = 
[Total Billing ₦bn] -
[Revenue Collected ₦bn]
```

### Unit

₦ billion

### Interpretation

A larger Revenue Gap indicates a greater amount of billed value that has not been converted into collected revenue.

### Important Interpretation

Revenue Gap represents a billing-to-collection difference within the analytical period.

It should not automatically be interpreted as permanently unrecoverable revenue.

---

# 5. Band A Service KPIs

## Band A Feeder Events

### Business Definition

Total number of feeder/service-event records represented within the selected Band A reporting context.

### Formula

```text
Band A Feeder Events =
Count of rows in FactBandAService
```

### Exact DAX

```DAX
Band A Feeder Events = 
COUNTROWS(FactBandAService)
```

### Unit

Events

### Interpretation

Provides the principal event-volume measure for Band A Service Intelligence.

Because the calculation uses `COUNTROWS`, the result represents the number of event records remaining after the applicable report filters have been applied.

---

## Unique Affected Feeders

### Business Definition

Number of distinct standardised feeders represented within the selected service-event population.

### Formula

```text
Unique Affected Feeders =
Distinct Count of Standardised Feeder Name
```

### Exact DAX

```DAX
Unique Affected Feeders = 
DISTINCTCOUNT(
    FactBandAService[Standardised Feeder Name]
)
```

### Unit

Feeders

### Interpretation

Separates the number of affected feeders from the total number of event records.

Multiple service events can therefore relate to the same feeder.

---

## Repeat Feeders

### Business Definition

Number of distinct feeders associated with more than one service event within the current filter context.

### Formula

```text
Repeat Feeders =
Count of Distinct Feeders
where Feeder Event Count > 1
```

### Exact DAX

```DAX
Repeat Feeders = 
COUNTROWS(
    FILTER(
        VALUES(FactBandAService[Standardised Feeder Name]),
        CALCULATE(COUNTROWS(FactBandAService)) > 1
    )
)
```

### Unit

Feeders

### Interpretation

Identifies recurring feeder-level service exceptions rather than treating every event as an isolated occurrence.

The use of `VALUES` creates the distinct feeder population in the current filter context. Each feeder is then evaluated using its corresponding event count.

Only feeders with more than one event are retained.

---

## Repeat Feeder Rate

### Business Definition

Measures the proportion of affected feeders that are associated with repeated service events.

### Formula

```text
Repeat Feeder Rate =
Repeat Feeders ÷ Unique Affected Feeders
```

### Exact DAX

```DAX
Repeat Feeder Rate = 
DIVIDE(
    [Repeat Feeders],
    [Unique Affected Feeders]
)
```

### Unit

Percentage

### Interpretation

A higher Repeat Feeder Rate indicates that a greater proportion of the affected feeder population appears repeatedly within the service-event dataset.

This provides a normalised recurrence indicator rather than relying solely on total event volume.

---

# 6. Voltage-Level Service Measures

## 11kV Events

### Business Definition

Number of Band A feeder/service events associated with the 11kV voltage level.

### Formula

```text
11kV Events =
Band A Feeder Events
filtered where Voltage kV = 11
```

### Exact DAX

```DAX
11kV Events = 
CALCULATE(
    [Band A Feeder Events],
    FactBandAService[Voltage kV] = 11
)
```

### Unit

Events

### Interpretation

Allows service-event activity at the 11kV level to be isolated from the overall Band A event population.

---

## 33kV Events

### Business Definition

Number of Band A feeder/service events associated with the 33kV voltage level.

### Formula

```text
33kV Events =
Band A Feeder Events
filtered where Voltage kV = 33
```

### Exact DAX

```DAX
33kV Events = 
CALCULATE(
    [Band A Feeder Events],
    FactBandAService[Voltage kV] = 33
)
```

### Unit

Events

### Interpretation

Allows management to isolate service-event activity occurring at the 33kV level.

---

## 33kV Events Display

### Business Definition

Text-formatted version of the 33kV event count used for executive dashboard presentation.

### Exact DAX

```DAX
33kV Events Display = 
FORMAT(
    CALCULATE(
        [Band A Feeder Events],
        FactBandAService[Voltage kV] = 33
    ),
    "0"
) & " Events"
```

### Return Type

Text

### Example Output

```text
12 Events
```

### Interpretation

This is a presentation measure rather than a separate analytical KPI.

It converts the underlying 33kV event count into executive-friendly display text.

---

# 7. Source Substation Exception Measures

## Top Exception Substation

### Business Definition

Identifies the source substation associated with the highest number of recorded Band A service events within the current filter context.

### Calculation Logic

```text
1. Identify the Source Substations in the current context
2. Calculate Band A Feeder Events for each substation
3. Rank the substations by event count
4. Select the highest-ranked substation
5. Return its name
```

### Exact DAX

```DAX
Top Exception Substation = 
VAR SubstationTable =
    TOPN(
        1,
        ADDCOLUMNS(
            VALUES(FactBandAService[Source Substation]),
            "Events", [Band A Feeder Events]
        ),
        [Events], DESC
    )
RETURN
    CONCATENATEX(
        SubstationTable,
        FactBandAService[Source Substation],
        ", "
    )
```

### Return Type

Text

### Dashboard Role

**Service Hotspot / Top Exception Substation**

### Interpretation

Highlights the source substation associated with the highest concentration of recorded Band A events.

### Important Interpretation

This is an analytical exception indicator.

It does not independently prove:

- equipment failure;
- inadequate capacity;
- protection-system problems;
- maintenance deficiencies; or
- any other specific engineering root cause.

Further operational investigation would be required.

---

## Top Substation Events

### Business Definition

Returns the number of Band A service events associated with the highest-event source substation.

### Calculation Logic

```text
1. Calculate event count for each Source Substation
2. Rank substations by event count
3. Select the highest-ranked substation
4. Return its event count
```

### Exact DAX

```DAX
Top Substation Events = 
MAXX(
    TOPN(
        1,
        ADDCOLUMNS(
            VALUES(FactBandAService[Source Substation]),
            "Events", [Band A Feeder Events]
        ),
        [Events], DESC
    ),
    [Events]
)
```

### Unit

Events

### Interpretation

Complements `Top Exception Substation` by quantifying the number of events associated with the identified service hotspot.

Together, the two measures answer:

```text
WHERE is the leading exception?
        +
HOW MANY events are associated with it?
```

---

# 8. KPI Dependency Framework

## Commercial Performance

```text
Energy Offtake GWh
        ↓
Energy Billed GWh
        ↓
Energy Accounting Efficiency
        ↓
Indicative ATC&C
        ↑
Collection Efficiency
        ↑
Revenue Collected ₦bn
        ↑
Total Billing ₦bn
```

The resulting Indicative ATC&C is then evaluated against:

```text
ATC&C Target
        ↓
ATC&C Variance to Target
```

Revenue performance is additionally evaluated through:

```text
Total Billing ₦bn
        -
Revenue Collected ₦bn
        =
Revenue Gap ₦bn
```

---

## Band A Service Intelligence

```text
FactBandAService
        ↓
Band A Feeder Events
        ↓
Unique Affected Feeders
        ↓
Repeat Feeders
        ↓
Repeat Feeder Rate
```

The same event population is analysed by:

```text
Voltage Level
      ↓
11kV Events / 33kV Events

Source Substation
      ↓
Top Exception Substation
      ↓
Top Substation Events
```

---

# 9. KPI Interpretation Summary

| KPI | Unit | Preferred Direction | Management Interpretation |
|---|---|---|---|
| Energy Offtake GWh | GWh | Context dependent | Scale of energy received |
| Energy Billed GWh | GWh | Context dependent | Energy converted into customer billing |
| Energy Accounting Efficiency | % | Higher | Stronger energy-to-billing conversion |
| Total Billing ₦bn | ₦bn | Context dependent | Monetary value billed |
| Revenue Collected ₦bn | ₦bn | Higher | Greater revenue collection |
| Collection Efficiency | % | Higher | Stronger billing-to-cash conversion |
| Indicative ATC&C | % | Lower | Lower combined loss pressure |
| ATC&C Target | % | Benchmark | Reference loss-performance target |
| ATC&C Variance to Target | Percentage points | Lower | Smaller/adverse performance gap |
| Revenue Gap ₦bn | ₦bn | Lower | Smaller billing-to-collection gap |
| Band A Feeder Events | Events | Lower | Fewer recorded service exceptions |
| Unique Affected Feeders | Feeders | Lower | Fewer feeders represented in exception data |
| Repeat Feeders | Feeders | Lower | Fewer recurring feeder exceptions |
| Repeat Feeder Rate | % | Lower | Lower recurrence concentration |
| 11kV Events | Events | Lower | Fewer recorded 11kV service exceptions |
| 33kV Events | Events | Lower | Fewer recorded 33kV service exceptions |
| Top Substation Events | Events | Lower | Lower event concentration at leading hotspot |

---

# 10. Validation Rules

The following relationships provide quality-assurance checks for the model.

## Energy Accounting Reconciliation

```text
Energy Accounting Efficiency =
Energy Billed GWh ÷ Energy Offtake GWh
```

The displayed efficiency should reconcile with the corresponding energy totals within the same filter context.

---

## Collection Reconciliation

```text
Collection Efficiency =
Revenue Collected ₦bn ÷ Total Billing ₦bn
```

The displayed collection efficiency should reconcile with the underlying billing and collection values.

---

## Indicative ATC&C Reconciliation

```text
Indicative ATC&C =
1 -
(
    Energy Accounting Efficiency
    ×
    Collection Efficiency
)
```

Both efficiency measures must be evaluated within the same reporting context.

---

## Revenue Gap Reconciliation

```text
Revenue Gap ₦bn =
Total Billing ₦bn - Revenue Collected ₦bn
```

---

## ATC&C Target Variance Reconciliation

```text
ATC&C Variance to Target =
(Indicative ATC&C - ATC&C Target) × 100
```

The result is expressed in percentage points.

---

## Band A Event Population

```text
Unique Affected Feeders
≤
Band A Feeder Events
```

One feeder can be represented by multiple event records.

---

## Repeat Feeder Population

```text
Repeat Feeders
≤
Unique Affected Feeders
```

---

## Repeat Feeder Rate Reconciliation

```text
Repeat Feeder Rate =
Repeat Feeders ÷ Unique Affected Feeders
```

---

## Voltage Event Reconciliation

Within an equivalent filter context:

```text
11kV Events
=
Band A Feeder Events filtered to 11kV
```

and:

```text
33kV Events
=
Band A Feeder Events filtered to 33kV
```

---

## Top Substation Reconciliation

`Top Exception Substation` and `Top Substation Events` are generated from the same ranking logic.

Therefore:

```text
Top Exception Substation
=
Name of highest-event substation
```

while:

```text
Top Substation Events
=
Event count associated with the highest-event substation
```

---

# 11. Filter Context

The measures are designed to respond dynamically to the report's applicable filter context.

## Commercial Performance

Commercial measures respond to relevant reporting-period selections, including the Year and Quarter controls used within the report.

This allows management to compare:

- energy performance;
- billing;
- collections;
- efficiencies;
- indicative ATC&C; and
- revenue gaps

across different reporting periods.

---

## Band A Service Intelligence

Band A measures respond to applicable selections including:

- Source Substation
- Voltage Level
- Service Period
- Service Date
- Feeder

This allows analysis to move from the overall service-event population to individual network/service segments.

---

# 12. Analytical Boundaries

## Indicative ATC&C

The project deliberately uses the term **Indicative ATC&C**.

The measure is calculated from the available energy-accounting and collection-efficiency data and is intended for analytical decision support.

It should not be represented as an independently audited or official regulatory ATC&C result.

---

## Band A Service Events

`Band A Feeder Events` counts records within the research dataset.

It should not automatically be interpreted as a complete census of every interruption or network condition across the entire Ikeja Electric distribution system.

---

## Repeat Feeders

A repeat feeder identifies recurrence within the available service-event data.

It does not independently establish the technical root cause of that recurrence.

---

## Service Hotspot

`Top Exception Substation` identifies the highest event concentration in the applicable dataset/filter context.

It is a prioritisation indicator rather than an engineering diagnosis.

---

## Revenue Gap

Revenue Gap represents the difference between billing and collections within the analytical period.

It should not automatically be interpreted as permanently unrecoverable revenue.

---

# 13. Dashboard Application

## Executive Commercial Performance

Primary KPIs:

- Energy Offtake GWh
- Energy Billed GWh
- Energy Accounting Efficiency
- Total Billing ₦bn
- Revenue Collected ₦bn
- Collection Efficiency
- Indicative ATC&C
- ATC&C Target
- ATC&C Variance to Target
- Revenue Gap ₦bn

### Management Question

**How effectively is energy received being converted into billed energy and ultimately collected revenue, and how does indicative ATC&C compare with target?**

---

## Band A Service Intelligence

Primary KPIs:

- Band A Feeder Events
- Unique Affected Feeders
- Repeat Feeders
- Repeat Feeder Rate
- 11kV Events
- 33kV Events

Supporting dimensions include:

- Feeder
- Source Substation
- Voltage Level
- Service Date
- Service Period

### Management Question

**Where are Band A service events occurring, and which feeders, substations or voltage levels show recurring exception patterns?**

---

## Executive Exception Centre

Principal indicators include:

- ATC&C Target
- Indicative ATC&C
- ATC&C Variance to Target
- Revenue Gap ₦bn
- Top Exception Substation
- Top Substation Events
- 33kV Events Display

### Management Question

**Which commercial-performance gaps and service exceptions warrant management attention?**

---

# 14. Measure Classification

## Commercial Measures

- Energy Offtake GWh
- Energy Billed GWh
- Energy Accounting Efficiency
- Total Billing ₦bn
- Revenue Collected ₦bn
- Collection Efficiency
- Indicative ATC&C
- Revenue Gap ₦bn

## Benchmark Measures

- ATC&C Target
- ATC&C Variance to Target

## Band A Service Measures

- Band A Feeder Events
- Unique Affected Feeders
- Repeat Feeders
- Repeat Feeder Rate
- 11kV Events
- 33kV Events

## Executive Exception / Display Measures

- 33kV Events Display
- Top Exception Substation
- Top Substation Events

---

# 15. Documentation Status

The DAX expressions documented above were taken from the final Power BI project measures rather than reconstructed from conceptual KPI definitions.

The accompanying **Data Dictionary** documents:

- business terminology;
- field meanings;
- units;
- analytical context;
- calculation principles; and
- interpretation boundaries.

Together:

```text
Data Dictionary
        +
KPI Definitions & Exact DAX
        =
Business Meaning + Technical Implementation
```

These documents provide the supporting analytical reference for the **Ikeja Electric Decision Intelligence POC**.
