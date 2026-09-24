# Data Dictionary
## Ikeja Electric Decision Intelligence POC

This data dictionary documents the principal data fields, analytical measures, formulas and business terminology used in the **Ikeja Electric Decision Intelligence POC**.

The project combines two complementary analytical perspectives:

1. **Commercial Performance Intelligence** – energy, billing, collection and indicative ATC&C performance.
2. **Band A Service Intelligence** – feeder-level service events, recurring exceptions, substations and voltage-level analysis.

The Power BI report presents these perspectives through three management views:

- Executive Commercial Performance
- Band A Service Intelligence
- Executive Exception Centre

---

# 1. Commercial Performance

The commercial-performance analysis follows the electricity commercial value chain:

```text
Energy Offtake
      ↓
Energy Billed
      ↓
Energy Accounting Efficiency
      ↓
Customer Billing
      ↓
Revenue Collection
      ↓
Collection Efficiency
      ↓
Indicative ATC&C
```

---

## Energy Offtake GWh

**Type:** Measure  
**Unit:** GWh

Total electrical energy received/offtaken during the selected reporting period, expressed in gigawatt-hours.

### Formula

```text
Energy Offtake GWh =
Total Energy Offtake (MWh) ÷ 1,000
```

### Business Use

Provides the starting energy-volume reference for evaluating how effectively received energy is subsequently converted into billed energy and collected revenue.

---
## Energy Offtake GWh

**Formula**

Energy Offtake GWh = Sum of Energy Offtake (GWh)

## Energy Billed GWh

**Formula**

Energy Billed GWh = Sum of Energy Billed (GWh)

## Total Billing ₦bn

**Formula**

Total Billing ₦bn = Sum of Total Billing (₦bn)

## Revenue Collected ₦bn

**Formula**

Revenue Collected ₦bn = Sum of Revenue Collected (₦bn)
### Business Interpretation

A higher percentage indicates that a greater proportion of energy received has been converted into billed energy.

The metric supports identification of energy-accounting and commercial-loss pressure.

---

## Total Billing ₦bn

**Type:** Measure  
**Unit:** ₦ billion

Total monetary value billed to customers during the selected reporting period.

### Formula

```text
Total Billing ₦bn =
Total Billing ₦ ÷ 1,000,000,000
```

### Business Use

Represents the monetary value expected from customer billing before considering actual collections.

It provides the denominator for evaluating collection efficiency.

---

## Revenue Collected ₦bn

**Type:** Measure  
**Unit:** ₦ billion

Total customer revenue collected during the selected reporting period.

### Formula

```text
Revenue Collected ₦bn =
Revenue Collected ₦ ÷ 1,000,000,000
```

### Business Use

Used to evaluate cash realisation from billed revenue and monitor the commercial conversion of billing into actual collections.

---

## Collection Efficiency

**Type:** Measure  
**Unit:** Percentage

Measures the proportion of billed revenue successfully collected.

### Formula

```text
Collection Efficiency =
Revenue Collected ÷ Total Billing
```

or:

```text
Collection Efficiency (%) =
(Revenue Collected ÷ Total Billing) × 100
```

### Business Interpretation

A higher percentage indicates stronger conversion of billed revenue into actual cash collections.

The measure supports analysis of revenue recovery and commercial performance.

---

## Indicative ATC&C

**Type:** Measure  
**Unit:** Percentage

An analytical estimate of Aggregate Technical, Commercial and Collection loss derived from the available commercial-performance data.

The project deliberately uses the term **Indicative ATC&C** because the measure is intended for management analysis and portfolio demonstration rather than presentation as an independently audited regulatory ATC&C figure.

### Formula

The analytical relationship is:

```text
Indicative ATC&C =
1 - (Energy Accounting Efficiency × Collection Efficiency)
```

When expressed as a percentage:

```text
Indicative ATC&C (%) =
[1 - (Energy Accounting Efficiency × Collection Efficiency)] × 100
```

Where both efficiency inputs are expressed as decimal values in the calculation.

Equivalent commercial-chain representation:

```text
Indicative ATC&C =
1 -
(
    Energy Billed
    ───────────────
    Energy Offtake
        ×
    Revenue Collected
    ─────────────────
    Total Billing
)
```

### Business Interpretation

Indicative ATC&C represents the proportion of the commercial energy/revenue chain not successfully converted from energy received into collected revenue.

A **lower** ATC&C percentage indicates stronger overall commercial conversion.

A **higher** ATC&C percentage indicates greater combined loss pressure.

---

## ATC&C Target

**Type:** Measure / Benchmark  
**Unit:** Percentage

Reference ATC&C performance target used to benchmark indicative ATC&C performance.

### Formula

```text
ATC&C Target =
Applicable Target for Selected Reporting Period
```

This is a benchmark value rather than a performance ratio calculated from energy and revenue.

### Business Use

Provides management with a reference against which indicative ATC&C performance can be evaluated.

---

## ATC&C Variance to Target

**Type:** Measure  
**Unit:** Percentage points

Measures the difference between indicative ATC&C performance and the applicable target.

### Formula

```text
ATC&C Variance to Target =
Indicative ATC&C - ATC&C Target
```

### Interpretation

```text
Positive Variance
= Indicative ATC&C is above target

Zero Variance
= Indicative ATC&C is on target

Negative Variance
= Indicative ATC&C is below target
```

Because lower ATC&C represents stronger performance, a positive variance identifies an adverse performance gap requiring management attention.

---

## Revenue Gap ₦bn

**Type:** Measure  
**Unit:** ₦ billion

Represents the monetary difference between billed revenue and revenue actually collected.

### Formula

```text
Revenue Gap ₦ =
Total Billing ₦ - Revenue Collected ₦
```

For dashboard presentation:

```text
Revenue Gap ₦bn =
(Total Billing ₦ - Revenue Collected ₦)
÷ 1,000,000,000
```

### Business Interpretation

A larger revenue gap indicates a greater amount of billed revenue that has not yet been converted into collected cash.

The measure should not automatically be interpreted as permanently unrecoverable revenue.

---

# 2. Date Dimension

## Table: `DimDate`

The date dimension supports chronological analysis and filtering of commercial-performance information.

---

## Date

**Type:** Date  
**Purpose:** Primary calendar date used for chronological analysis and model relationships.

---

## Year

**Type:** Dimension

Calendar year associated with each reporting date.

### Derivation

```text
Year =
YEAR(Date)
```

### Business Use

Supports annual filtering and year-level performance comparison.

---

## Quarter

**Type:** Dimension

Calendar quarter associated with each reporting date.

### Derivation

Conceptually:

```text
Quarter =
Quarter Number derived from Date
```

Displayed as:

```text
Q1
Q2
Q3
Q4
```

### Business Use

Supports quarterly commercial-performance analysis.

---

## Month

**Type:** Dimension

Calendar month associated with each reporting date.

### Derivation

```text
Month =
Month derived from Date
```

### Business Use

Supports monthly trend analysis across energy, billing, revenue and efficiency measures.

---

# 3. Band A Service Intelligence

## Table: `FactBandAService`

The Band A service dataset supports analysis of recorded feeder-level service events and recurring network/service exceptions.

The analysis is designed to identify:

- affected feeders;
- recurring feeder events;
- source substations associated with events;
- voltage-level patterns;
- service-period patterns; and
- potential service hotspots.

The dataset should be interpreted as an **event-based service intelligence dataset**, rather than a complete representation of every operating condition across the entire distribution network.

---

## Feeder/Circuit

**Type:** Dimension

Feeder or circuit identifier associated with a Band A service event.

### Business Use

Supports feeder-level analysis and identification of locations associated with repeated service events.

---

## Standardised Feeder Name

**Type:** Dimension

Standardised feeder identifier used to improve consistency when grouping feeder events.

### Derivation

```text
Raw Feeder/Circuit Name
        ↓
Naming standardisation / cleaning
        ↓
Standardised Feeder Name
```

### Business Use

Helps consolidate feeder records that might otherwise contain naming variations.

This supports more reliable repeat-event analysis.

---

## Feeder Display

**Type:** Display / Analytical Field

Feeder-level display field used within report visuals.

### Business Use

Provides a consistent feeder label for presentation, filtering and detailed service-event analysis.

---

## Source Substation

**Type:** Dimension

Identifies the source substation associated with the feeder/service event.

### Business Use

Allows feeder events to be aggregated at source-substation level and supports identification of concentrated service exceptions.

---

## Transformer

**Type:** Dimension

Transformer associated with the feeder/service record where available in the research dataset.

### Business Use

Provides an additional network hierarchy level for detailed investigation.

---

## Voltage kV

**Type:** Dimension  
**Unit:** kV

Voltage level associated with the feeder/service event.

### Business Use

Supports segmentation of service events by network voltage level.

The Executive Exception Centre also uses this information to isolate **33kV service exceptions**.

---

## Service Date From

**Type:** Date

Start date associated with the recorded Band A service event or service period.

### Business Use

Supports chronological analysis of service events and allows event patterns to be examined across reporting periods.

---

## Service Period

**Type:** Dimension

Reporting/service period associated with the Band A event record.

### Business Use

Allows users to filter service-event analysis across selected reporting windows.

---

# 4. Band A Analytical Measures

The Power BI model contains a dedicated analytical measure group for Band A service intelligence.

---

## Band A Feeder Events

**Type:** Measure  
**Unit:** Events

Counts the feeder/service event records represented within the selected reporting context.

### Formula

Conceptually:

```text
Band A Feeder Events =
Count of Band A Service Event Records
```

### Business Use

Provides the fundamental event-volume measure for the Band A Service Intelligence page.

It supports analysis by:

- feeder;
- source substation;
- voltage level; and
- service period.

---

## Unique Affected Feeders

**Type:** Measure  
**Unit:** Feeders

Counts the distinct feeders represented within the selected Band A event population.

### Formula

```text
Unique Affected Feeders =
Distinct Count of Standardised Feeder Name
```

### Business Interpretation

Separates the number of affected feeders from the number of events.

For example, several event records may relate to the same feeder, meaning:

```text
Band A Feeder Events > Unique Affected Feeders
```

---

## Repeat Feeders

**Type:** Measure  
**Unit:** Feeders

Counts feeders represented by more than one event within the applicable analytical context.

### Formula

Conceptually:

```text
Repeat Feeders =
Count of Distinct Feeders
where
Feeder Event Count > 1
```

### Business Use

Identifies recurring feeder-level service exceptions rather than treating every event as an isolated occurrence.

Repeated feeder appearances can therefore be prioritised for deeper operational investigation.

---

## Repeat Feeder Rate

**Type:** Measure  
**Unit:** Percentage

Measures the proportion of affected feeders that experience repeated events.

### Formula

```text
Repeat Feeder Rate =
Repeat Feeders ÷ Unique Affected Feeders
```

or:

```text
Repeat Feeder Rate (%) =
(Repeat Feeders ÷ Unique Affected Feeders) × 100
```

### Business Interpretation

A higher repeat rate indicates that a larger proportion of affected feeders appears repeatedly in the service-event dataset.

This provides management with a normalised measure of recurrence rather than relying solely on total event volume.

---

# 5. Executive Exception Measures

The Executive Exception Centre combines commercial-performance and Band A service intelligence to highlight issues requiring management attention.

---

## Top Exception Substation

**Type:** Measure / Dynamic Text

Identifies the source substation associated with the highest concentration of recorded Band A service events within the selected reporting context.

### Dashboard Label

**Service Hotspot**

### Formula

Conceptually:

```text
1. Group Band A events by Source Substation
2. Calculate event count for each substation
3. Rank substations by event count
4. Return the highest-ranked Source Substation
```

Equivalent analytical expression:

```text
Top Exception Substation =
Source Substation with MAX(Band A Feeder Events)
```

### Business Use

Provides management with an immediate indication of where recorded service-event concentration is highest and where further investigation may be warranted.

### Important Interpretation

This identifies an **analytical hotspot** within the available event dataset.

It does not, by itself, prove that the substation is technically defective or establish the root cause of the events.

---

## 33kV Events Display

**Type:** Measure / Dynamic Display

Displays the Band A service-event population associated with 33kV records within the selected reporting context.

### Dashboard Label

**33kV Service Exceptions**

### Formula

Conceptually:

```text
33kV Service Exceptions =
Band A Feeder Events
filtered where Voltage kV = 33
```

### Business Use

Provides immediate management visibility of service exceptions occurring at the 33kV level without requiring manual filtering of the detailed Band A dataset.

---

# 6. Dashboard Pages

## Executive Commercial Performance

### Purpose

Provides management with an integrated view of energy, billing, revenue collection and indicative ATC&C performance.

### Principal Metrics

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

### Analytical Views

- Revenue Collected vs Total Billing
- Energy Accounting Efficiency Trend
- Energy Offtake vs Energy Billed
- Year and Quarter filtering

---

## Band A Service Intelligence

### Purpose

Provides feeder-level intelligence on Band A service events and recurring network/service exceptions.

### Principal Metrics

- Band A Feeder Events
- Unique Affected Feeders
- Repeat Feeders
- Repeat Feeder Rate

### Analytical Views

- Events by Feeder
- Events by Source Substation
- Events by Voltage Level
- Detailed Feeder Event Analysis
- Events across Service Dates
- Voltage filtering
- Substation filtering
- Service-period filtering

---

## Executive Exception Centre

### Purpose

Converts detailed commercial and service-performance analysis into a concise management exception view.

### Principal Indicators

- ATC&C Target
- Indicative ATC&C
- ATC&C Variance to Target
- Revenue Gap
- Top Exception Substation
- 33kV Service Exceptions

The page is designed to focus management attention on **material commercial-performance gaps and recurring service exceptions**, rather than reproduce the detailed analysis available on the other dashboard pages.

---

# 7. Key Business Terminology

## Energy Offtake

Electrical energy received/offtaken for distribution during the applicable reporting period.

---

## Energy Billed

The portion of energy received that is subsequently represented within customer billing.

---

## Energy Accounting Efficiency

The proportion of energy received that is converted into billed energy.

### Formula

```text
Energy Accounting Efficiency =
Energy Billed ÷ Energy Offtake
```

---

## Collection Efficiency

The proportion of billed monetary value converted into collected revenue.

### Formula

```text
Collection Efficiency =
Revenue Collected ÷ Total Billing
```

---

## ATC&C

Aggregate Technical, Commercial and Collection losses.

ATC&C provides a consolidated indication of the loss between energy entering the distribution system and the corresponding revenue ultimately collected.

Within this project, the calculated measure is explicitly labelled **Indicative ATC&C**.

### Analytical Formula Used

```text
Indicative ATC&C =
1 - (Energy Accounting Efficiency × Collection Efficiency)
```

---

## Revenue Gap

Difference between billed revenue and revenue collected.

### Formula

```text
Revenue Gap =
Total Billing - Revenue Collected
```

It provides a monetary representation of billed revenue not yet converted into collected cash.

---

## Band A

A Nigerian electricity-service classification associated with customers/feeders subject to defined service expectations.

Within this project, Band A analysis is based on the service-event records assembled for the proof of concept.

---

## Feeder Event

A recorded service event associated with a feeder/circuit in the Band A service dataset.

One feeder may appear in multiple event records.

---

## Repeat Feeder

A feeder represented by multiple service events within the relevant analytical context.

### Rule

```text
Repeat Feeder =
Feeder with Event Count > 1
```

The measure is intended to identify recurrence and support prioritisation for further investigation.

---

## Service Hotspot

The source substation with the highest concentration of recorded service events within the applicable reporting context.

### Rule

```text
Service Hotspot =
Source Substation with Highest Band A Event Count
```

This is an exception indicator, not by itself a diagnosis of the technical cause of the events.

---

# 8. KPI Formula Summary

| KPI | Formula |
|---|---|
| Energy Offtake GWh | Energy Offtake MWh ÷ 1,000 |
| Energy Billed GWh | Energy Billed MWh ÷ 1,000 |
| Energy Accounting Efficiency | Energy Billed ÷ Energy Offtake |
| Total Billing ₦bn | Total Billing ₦ ÷ 1,000,000,000 |
| Revenue Collected ₦bn | Revenue Collected ₦ ÷ 1,000,000,000 |
| Collection Efficiency | Revenue Collected ÷ Total Billing |
| Indicative ATC&C | 1 − (Energy Accounting Efficiency × Collection Efficiency) |
| ATC&C Variance to Target | Indicative ATC&C − ATC&C Target |
| Revenue Gap | Total Billing − Revenue Collected |
| Band A Feeder Events | Count of Band A Event Records |
| Unique Affected Feeders | Distinct Count of Standardised Feeders |
| Repeat Feeders | Distinct Feeders where Event Count > 1 |
| Repeat Feeder Rate | Repeat Feeders ÷ Unique Affected Feeders |
| Top Exception Substation | Substation with Highest Band A Event Count |
| 33kV Service Exceptions | Band A Events where Voltage kV = 33 |

---

# 9. Interpretation and Analytical Boundaries

This project is a **decision-intelligence proof of concept** developed from the available research dataset.

Several interpretation rules are therefore important.

## Indicative ATC&C

The dashboard deliberately labels the calculated ATC&C measure **Indicative ATC&C**.

It should not be interpreted as an independently audited or official regulatory ATC&C result.

Its purpose is analytical benchmarking and management decision support using the available commercial-performance data.

---

## Band A Event Analysis

Band A service measures describe the event records contained within the research dataset.

They should not automatically be interpreted as a complete census of every interruption or network condition across Ikeja Electric's entire distribution system.

---

## Repeat Events

Repeated feeder appearances identify recurrence within the available event data.

They provide a basis for prioritising further investigation but do not independently establish the technical root cause of a service issue.

---

## Substation Exceptions

A high event concentration at a source substation identifies an analytical hotspot.

Further operational or engineering investigation would be required before attributing the pattern to equipment condition, network configuration, capacity, protection operation or another technical cause.

---

## Revenue Gap

Revenue Gap represents the difference between billed and collected revenue within the analytical model.

It should not automatically be interpreted as permanently unrecoverable revenue.

---

## Target Variance

ATC&C variance measures performance relative to the applicable target.

Because **lower ATC&C is preferable**, a positive variance indicates that indicative ATC&C exceeds the target and therefore represents an adverse performance gap.

---

# 10. Analytical Framework

The commercial-performance analytical sequence is:

```text
ENERGY OFFTAKE
      ↓
ENERGY BILLED
      ↓
ENERGY ACCOUNTING EFFICIENCY
      ↓
TOTAL BILLING
      ↓
REVENUE COLLECTED
      ↓
COLLECTION EFFICIENCY
      ↓
INDICATIVE ATC&C
      ↓
TARGET VARIANCE / REVENUE GAP
      ↓
MANAGEMENT EXCEPTION
```

The Band A service-intelligence sequence is:

```text
BAND A SERVICE EVENTS
      ↓
AFFECTED FEEDERS
      ↓
REPEAT FEEDERS
      ↓
SUBSTATION / VOLTAGE ANALYSIS
      ↓
SERVICE HOTSPOTS
      ↓
MANAGEMENT EXCEPTION
```

Together, the two analytical streams allow the report to connect:

**Commercial Performance Intelligence + Network/Service Exception Intelligence → Management Decision Support**

---

# 11. Project Scope

**Project:** Ikeja Electric Decision Intelligence POC  
**Platform:** Microsoft Power BI  
**Project Type:** Decision Intelligence / Business Intelligence Proof of Concept  
**Primary Analytical Areas:** Commercial Performance, Revenue Intelligence, Indicative ATC&C, Band A Service Performance, Feeder Recurrence, Network Exceptions and Management Decision Support  
**Report Pages:** 3

### Report Pages

1. Executive Commercial Performance
2. Band A Service Intelligence
3. Executive Exception Centre

---

# 12. Documentation Note

This Data Dictionary documents the **business meaning, calculation principles and analytical use** of the project's principal fields and KPIs.

The accompanying **KPI Definitions & DAX Documentation** provides the more detailed technical layer, including:

- KPI calculation logic;
- DAX measures;
- filter-context behaviour;
- benchmark logic;
- validation rules; and
- management interpretation.

Together, the two documents provide both the **business definition** and the **technical implementation** of the Ikeja Electric Decision Intelligence POC.
