# Project overview

## Proposition

This project is a Bachelor thesis in data science that combines Spanish demographic and water-service data into a reproducible analytical portfolio. It begins with heterogeneous public statistical tables, turns them into a small set of analysis-ready workbooks in R, studies time and territorial patterns through visual analysis, and presents the results in a multilingual Power BI dashboard.

The project asks a practical analytical question: how can population growth, births, deaths and migration be examined alongside the supply, allocation and availability of water services across Spanish territories? It does not answer that question with a causal model. The contribution is a structured descriptive workflow that makes the two domains comparable while preserving their different time coverage, measurement units and geographic detail.

The original thesis is available in Spanish at [bachelor-thesis-es.pdf](../report/bachelor-thesis-es.pdf). Its introduction positions the work in a context of demographic change, migration, climate-related pressure and the need for sustainable resource management. [Thesis, Chapter 1](../report/bachelor-thesis-es.pdf#page=19)

## Context and motivation

Spanish population dynamics are not uniform across the territory. The source material supports long-run views of population at province and autonomous-community level, together with births, deaths and migration flows. This makes it possible to move beyond a single national total and inspect population density, regional growth, ageing-related structure and territorial redistribution.

Water is handled in a similarly granular but distinct way. The project examines water supplied to the network, water registered and distributed, unregistered water, allocation to households, municipal uses and economic sectors, plus potable and non-potable availability. These are service and management indicators. They should not be conflated with natural reservoir storage or used as a direct measure of drought.

The thesis motivation is therefore twofold. First, it uses public data and data-science tools to make complex demographic and water information easier to explore. Second, it brings the domains into a shared analytical frame, so that territorial and temporal comparisons can inform questions about demand, allocation and sustainable management. [Thesis, PDF pp. 24-25](../report/bachelor-thesis-es.pdf#page=24)

## Research and analytical objectives

The academic objectives were to evaluate migration-related demographic dynamics in Spanish autonomous communities and to explore their relationship with water-resource management. The portfolio implements those objectives as transparent analytical tasks:

1. collect and prepare public tables that vary in layout, period and geographic grain;
2. create consistent province and autonomous-community identifiers and year fields;
3. calculate demographic and water-service indicators that support comparison;
4. inspect national trends and regional variation with appropriate charts and maps;
5. communicate results through a dashboard that supports filtering and geographic exploration; and
6. document the assumptions, provenance and limits so that observed association is not overstated as causation.

The thesis discusses migration as a relevant demographic mechanism, but this repository does not contain a causal design, causal identification strategy, regression model or hypothesis test that would establish migration or population as the cause of water outcomes. The cross-domain material is exploratory and contextual.

## Scope

### Time

Population, births, deaths and the principal population workbooks span 1975-2021. The thesis states that migration-source coverage starts later, from 2008, even though the processed population records retain the long annual series. Water-service records span 2000-2020. The different windows matter: a water and population comparison can use their overlap, but it cannot support a 1975 water conclusion. [Thesis, PDF pp. 30-32](../report/bachelor-thesis-es.pdf#page=30)

### Geography

The processed data provide national, autonomous-community and province views. The community-level workbooks contain 19 entries, reflecting the handling used by the original workflow, including a combined Ceuta and Melilla entry for the relevant community-level preparation. The province workbook contains 53 levels. National totals are created in Stage 1 where a country-level view is needed.

### Topics included

The project covers population totals, sex and age structure, births, deaths, internal migration, external migration, density, demographic rates and variations. Water topics include supply, registered and distributed volumes, unregistered volumes, household supply, municipal and economic allocation, potable and non-potable availability, efficiency, supply per square kilometre and household per-capita supply.

### Exclusions

The repository does not publish a causal model, forecasts, climate model, reservoir-storage data, water-quality analysis, individual-level microdata or policy impact evaluation. It does not claim that every observed population and water pattern shares a single mechanism. Figures in the thesis often offer contextual explanations; these are discussed as interpretation, not measured causal effects.

## Project stages

### 1. Data preparation and ETL

[Stage 1](../scripts/01_prepare_datasets.Rmd) imports raw Excel extracts and prepares four principal analysis-ready workbooks. It reshapes time columns, cleans labels, standardises territory names, combines tables, calculates indicators and interpolates selected missing water values. The transformation logic is described in [Data sources and ETL](data-sources-and-etl.md).

### 2. Visual and exploratory analysis

[Stage 2](../scripts/02_analyse_and_visualise.Rmd) reads the processed workbooks. It uses national line charts for temporal movement, heat maps for year-by-region comparison, and choropleth maps for geographic distribution at selected years. It also includes descriptive community-level scatterplots connecting population to four water variables. [Thesis, Chapter 5](../report/bachelor-thesis-es.pdf#page=39)

### 3. Interactive presentation

The [Power BI dashboard](power-bi-dashboard.md) turns the processed data into six documented views: population graphs, maps and a selectable table; then equivalent water-resources graphs, maps and a selectable table. It is designed to complement the R workflow with interactive filtering, map exploration and presentation-oriented KPI views.

### 4. Academic communication

The original thesis supplies the research framing, methods discussion, interpretations and conclusions. This documentation translates the repository evidence into recruiter-facing English without rewriting the original academic work or changing its historical outputs.

## Final deliverables

| Deliverable | Role |
| --- | --- |
| Raw source extracts | Traceable starting material for the R workflow |
| Four main processed workbooks | Stable input layer for analysis and Power BI |
| Integrated comparison table | Community-level population and water comparison for 2000-2020 |
| R Markdown stages | Executable preparation and visual-analysis logic |
| Power BI report | Interactive multilingual portfolio artefact |
| Six screenshots | Immediate visual access without opening Power BI |
| Bachelor thesis | Original academic narrative, references and figures |
| Public documentation | English guide to scope, methods, findings and limitations |

## Professional competencies demonstrated

This repository demonstrates the complete path from messy public data to a professional analytical artefact. It shows source assessment, reproducible path management, R data wrangling, exploratory visual analysis, geographic comparison, dashboard design and documentation discipline.

The technical work is paired with analytical judgement. Different time ranges are made explicit; water supply, availability and consumption are kept distinct; and territorial comparisons are not framed as causal evidence. For a recruiter, that combination is as important as the charts: it shows the ability to build an end-to-end workflow while communicating what the evidence does and does not support.

## Where to go next

- For raw sources, transformations and workbook lineage, read [Data sources and ETL](data-sources-and-etl.md).
- For the analytical questions, chart choices and evidence-backed observations, read [Analysis and findings](analysis-and-findings.md).
- For interactive pages and refresh constraints, read [Power BI dashboard](power-bi-dashboard.md).
- For execution order and limitations, read [Reproducibility and limitations](reproducibility-and-limitations.md).


## Research design in portfolio terms

The design is intentionally layered. The raw layer preserves source extracts. The processed layer establishes a stable analytical contract through four main workbooks. The R analysis layer makes transformation, aggregation and visual choice inspectable in code. The Power BI layer offers a compact interface for a non-technical reviewer. The thesis layer records the original academic framing and interpretation. Each layer has a different responsibility, which prevents a dashboard card or a single chart from being treated as the entire evidence base.

The project also makes an important methodological trade-off visible. It chooses breadth of territorial and temporal descriptive coverage over a narrow causal experiment. That choice enables comparison of population, migration, water supply, allocation and availability in one repository, but it also demands caution. Population and water records have different periods; community values are aggregates; and the workflow does not include controls for economic activity, weather, policy, household composition or infrastructure condition. The appropriate portfolio claim is therefore an ability to build and communicate a traceable descriptive analytical product.

For a recruiter, the value lies in the sequence of decisions: selecting public sources, making incompatible structures joinable, defining denominators for comparison, choosing views for time and geography, and writing down limitations before presenting results. The project demonstrates both production skills and evidence discipline.

## Deliverable relationship

A reader can begin with the README and screenshots, then follow a specific question into the detailed pages. For example, a question about water efficiency can be traced from the dashboard table to `Agua_España.xlsx`, then to the Stage 1 formula and the source extracts. A question about a provincial map can be traced from the dashboard or R map to `Poblacion_Provincias.xlsx`, the territorial lookup and the population source. This path is deliberate: every public presentation layer has a route back to code and data.


## Review and maintenance perspective

The portfolio is intentionally preserved as an academic historical snapshot. A maintenance pass would begin by refreshing the public extracts, documenting the exact download dates and comparing new schemas with the current processed contract. It would then re-run Stage 1, inspect changes in row counts and indicators, refresh Stage 2, and validate the Power BI sources and screenshots. This sequence prevents a dashboard update from becoming detached from its data lineage or thesis-era definitions.
