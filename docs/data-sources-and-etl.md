# Data sources and ETL

## Purpose and evidence boundary

The preparation workflow turns a set of public Spanish demographic, migration, territorial and water-service extracts into analysis-ready Excel workbooks. The executable source is [01_prepare_datasets.Rmd](../scripts/01_prepare_datasets.Rmd). This page describes what that source does; it does not add transformations that are absent from the repository.

The thesis attributes the demographic and water-statistics extracts to the Spanish National Statistics Institute, known as INE. Territorial-area data are attributed to the Ministry for Ecological Transition and Demographic Challenge. [Thesis, PDF pp. 29-32](../report/bachelor-thesis-es.pdf#page=29) [Thesis bibliography, PDF p. 85](../report/bachelor-thesis-es.pdf#page=85) Source files are committed as local extracts for reproducibility, but the original source terms govern their reuse and redistribution.

## Source matrix

| Institution | Source dataset in project terms | Filename | Geographic level | Time range used | Format | Project use | Processed destination |
| --- | --- | --- | --- | --- | --- | --- | --- |
| INE | Resident population by province, sex and age | `Poblacion_España.xls` | Province and national total | 1975-2021 | XLS | Population backbone, sex totals and annual records | `Poblacion_Provincias.xlsx` |
| INE | Population by autonomous community | `Poblacion_Comunidad.xls` | Autonomous community | 1975-2021 | XLS | Community population and water join | `Poblacion_Comunidades.xlsx`, `Agua_España.xlsx` |
| INE | Population by age and sex | `Poblacion_Edad.xls` | Province | 1975-2021 | XLS | Age bands for population-pyramid analysis | `Poblacion_Edad.xlsx` |
| INE | Births by residence | `Natalidad.xls` | Province | 1975-2021 | XLS | Birth counts and birth rate | Province and community outputs |
| INE | Deaths by residence | `Mortalidad.xls` | Province | 1975-2021 | XLS | Death counts and death rate | Province and community outputs |
| INE | Internal migration | `Migracion_interna.xls` | Province | Source coverage begins later than population series | XLS | Internal entry and exit fields | Province and community outputs |
| INE | Immigration from abroad | `Migracion_externa_entrada.xls` | Province | Source coverage begins later than population series | XLS | External-entry field | Province and community outputs |
| INE | Emigration abroad | `Migracion_externa_salida.xls` | Province | Source coverage begins later than population series | XLS | External-exit field | Province and community outputs |
| INE | Province to community correspondence | `Provincias_Comunidad.xlsx` | Province to autonomous community | Reference table | XLSX | Territorial joins and standardised labels | All geographic outputs |
| Ministry for Ecological Transition and Demographic Challenge | Provincial territorial area | `Extension territorial Provincias.xlsx` | Province and derived community total | Reference table | XLSX | Area, density and water-per-area indicators | Province, community and water outputs |
| INE | Potable and non-potable water availability | `Agua_disponible.xls` | Autonomous community | 2000-2020 after preparation | XLS | Availability indicators | `Agua_España.xlsx` |
| INE | Water supplied to network | `Volumen de agua.xls` | Autonomous community | 2000-2020 after preparation | XLS | Total, registered and unregistered supply | `Agua_España.xlsx` |
| INE | Registered-water distribution by user group | `Distribución de agua.xls` | Autonomous community | 2000-2020 after preparation | XLS | Economic sectors, households and municipal allocation | `Agua_España.xlsx` |

The thesis reports population data from 1975 to 2021, migration coverage from 2008 to 2021, and water data from 2000 to 2020. The processed workbooks retain the reported long population range and a 2000-2020 water range. [Thesis, PDF pp. 30-32](../report/bachelor-thesis-es.pdf#page=30)

## Stage 1 workflow

### Ingestion and structural reshaping

The script imports Excel extracts with `readxl`, then uses `tidyr`, `dplyr`, `xlsx`, `openxlsx`, `zoo` and `here`. `here::here()` resolves every active raw input from the project root, so execution is not dependent on the current working directory.

Many inputs are arranged as broad tables with one column per year or contain non-data header rows. Stage 1 selects the relevant rows and fields, then pivots year columns into a single `Año` field. That long shape makes it possible to join population, vital events, migration and water measures by territory and year. The thesis describes this change from columns to rows, removal of unnecessary leading rows and simplification of date labels to annual values. [Thesis, PDF pp. 33-35](../report/bachelor-thesis-es.pdf#page=33)

For population by age, the workflow extracts province and sex information, reshapes year fields, and aggregates age into ten-year groups. It retains total population, men and women, allowing Power BI to draw a population pyramid. [Thesis, PDF p. 36](../report/bachelor-thesis-es.pdf#page=36)

### Territorial cleaning and harmonisation

Territorial labels are cleaned before joins. Province strings are split where needed, names are standardised for known differences and the province-to-community lookup is applied. The script also builds a national-total area row by summing provincial area. At the community level, the preparation described in the thesis combines Ceuta and Melilla for consistent calculations and adds a national territorial-area total. [Thesis, PDF pp. 35-38](../report/bachelor-thesis-es.pdf#page=35)

This work is essential rather than cosmetic. A join on inconsistent territory names can silently drop records or create unmatched regions, which would affect every rate, map and dashboard filter downstream.

### Demographic joins and derived fields

The province workflow starts from population and joins births, deaths, internal migration in and out, and external migration in and out. It calculates:

| Indicator | Definition in workflow or thesis | Analytical role |
| --- | --- | --- |
| `Poblacion_Km2` | Population divided by territorial area | Provincial density comparison |
| `Tasa_natalidad` | Births divided by total population | Birth intensity relative to population |
| `Tasa_mortalidad` | Deaths divided by total population | Death intensity relative to population |
| `Tasa_Migracion` | Net internal and external migration relative to population, scaled per thousand in the thesis definition | Migration balance comparison |
| `Variacion` | Population change relative to earlier period | Population movement over time |

The community workflow aggregates province-level demographic values, joins community population and area, then creates demographic variation fields over 2, 4 and 10 years. The thesis documents density, birth, death and migration rates as comparative indicators rather than direct causal measures. [Thesis, PDF p. 36](../report/bachelor-thesis-es.pdf#page=36)

### Water preparation, joining and interpolation

The water stage reshapes availability, network-supply and user-group distribution tables into annual community records. It separates availability types and supply types into dedicated variables, then joins them with community population and area. The resulting water workflow therefore distinguishes:

- total water supplied to the network;
- water registered and distributed;
- unregistered water;
- allocation to economic sectors, households and municipal consumption;
- water available for potabilisation; and
- non-potabilised availability.

The script applies `zoo::na.approx` within each community to fill identified missing water observations for 2015, 2017 and 2019. This is interpolation, not an observed measurement. It keeps the annual series continuous for visualisation, but it is a limitation when interpreting those points. [Thesis, PDF p. 37](../report/bachelor-thesis-es.pdf#page=37)

### Water indicators

| Indicator | Calculation | Interpretation guardrail |
| --- | --- | --- |
| `Eficiencia_Agua` | Registered and distributed water divided by supplied water, multiplied by 100 | Distribution-efficiency indicator, not a direct measure of physical leakage alone |
| `Agua_Km2` | Supplied water divided by area, with the script conversion used for litres per square kilometre | Normalises supply by territory, not by demand or hydrology |
| `Consumo_Habitante` | Household supply and population, converted to daily litres per inhabitant in the thesis formula | Household supply proxy; it should not be labelled as total water use |
| Supply, registered, household and availability variations | Percentage changes using 2, 4 and 10 year lags | Describes change relative to earlier observations |

The thesis presents the water-efficiency and water-per-area formulas, explains the population link used for household per-capita supply, and distinguishes potable from non-potable availability. [Thesis, PDF pp. 38 and 77](../report/bachelor-thesis-es.pdf#page=38)

## Raw-to-processed mapping

| Raw inputs | Transformation outcome | Processed workbook |
| --- | --- | --- |
| Population, births, deaths, migration, province-community lookup and territorial area | Clean annual province table with demographic fields, rates, density and variation | `Poblacion_Provincias.xlsx` |
| Province output plus community population and area | Community annual population and demographic table with variations | `Poblacion_Comunidades.xlsx` |
| Population by age source | Province-year-ten-year-age-band table split by total, men and women | `Poblacion_Edad.xlsx` |
| Water availability, supply, distribution, community population and territorial area | Community-year water-service table with allocation, availability, efficiency and variations | `Agua_España.xlsx` |
| Community population output plus water output | Integrated 2000-2020 comparison table with five and ten year variations | `Tabla_Final.xlsx` |

## Processed outputs

### Poblacion_Provincias.xlsx

This workbook contains 2,491 rows and 18 columns at community, province and year grain. Its 1975-2021 records include total population, men, women, births, deaths, four migration fields, area, population density, birth rate, death rate and migration rate. It is the most detailed demographic source for provincial maps and national aggregation.

### Poblacion_Comunidades.xlsx

This workbook contains 893 rows and 29 columns for 19 community-level entries across 1975-2021. It holds community population, density, births, deaths, migration fields, area, rates, net migration and 2, 4 and 10 year variations. It is the core regional demographic input for the dashboard and heat maps.

### Poblacion_Edad.xlsx

This workbook contains 251,591 rows and six columns at province, year and age-band grain. It covers 1975-2021, 53 province-level entries and ten age groups, with total, male and female populations. Its role is age and sex composition, especially the Power BI population-pyramid view.

### Agua_España.xlsx

This workbook contains 399 rows and 34 columns across 19 community-level entries for 2000-2020. It is the central water-service dataset, including supply, registered and distributed water, unregistered water, user-group allocation, availability, population, area, efficiency, water per square kilometre, household per-capita supply and multi-year variations.

## Quality and lineage notes

Stage 1 writes the principal analysis workbooks under `data/processed/`. The generated `Tabla_Final.xlsx` is an auxiliary integrated comparison table, and `Tablas.docx` is a formatted reference output generated by the separate table workflow. The four principal workbooks are the data layer inspected by Stage 2 and referenced by the Power BI model metadata.

No transformation should be interpreted as an update to the historical source. The repository keeps the original extracts in `data/raw/`, the derived artefacts in `data/processed/`, and executable transformation logic in Stage 1. This separation provides a clear audit trail from source extract to portfolio view.


## Implementation detail by transformation family

### Wide-to-long time handling

The source tables contain historical observations arranged across year columns. The preparation stage pivots these fields into `Año`, which creates one observation per territory-year and permits reusable joins. This is the backbone of the data model: demographic events can be matched to the relevant population record, and water measures can be compared across years without maintaining a separate column for each period.

### Join discipline

The workflow uses territorial cleaning before combining data. Population is not joined to water at province grain because the water data are community-level. Instead, the community population workflow provides the compatible population field for the water table. This avoids an implicit and undocumented provincial-to-community aggregation inside the analytical layer. The auxiliary `Tabla_Final.xlsx` then exposes an explicit community-year bridge between population and supplied water for the common 2000-2020 period.

### Aggregation and national totals

National charts are not separate raw source tables. They are generated from processed records through aggregation in the analysis workflow. The preparation stage also creates an area total where a national denominator is needed. This design keeps territory-level data available for maps while allowing a reproducible national view.

### Derived percentages and lagged changes

Rates and variations use existing project fields as denominators or earlier observations. They are therefore sensitive to the quality and completeness of those fields. A percentage variation is useful for comparing movement across communities of different size, but a large percentage can arise from a small base. Documentation and dashboards should always pair a change measure with a level view where interpretation requires it.

### Controlled output layer

All active preparation paths are resolved through `here::here()` and outputs are written to `data/processed/`. The folder separation is an operational control: it prevents an analysis script from silently treating a derived workbook as an original input, and it makes the hand-off to Power BI clearer. No source data are overwritten by the workflow.

## Data quality considerations

The project performs name standardisation, removes non-data rows and reshapes source layout, but it is not a claim that the underlying public tables are error-free. It also does not manufacture unavailable migration history. The script records source coverage through available fields and produces the historical outputs used by the thesis. Users extending the project should re-check schemas, labels, territorial codes and period coverage before appending new observations.

The interpolation choice is especially important. `na.approx` generates a straight-line estimate between neighbouring non-missing water observations. It is appropriate for maintaining continuity in the documented visual workflow, but it cannot capture unobserved shocks. An extended version of the project should flag interpolated points visibly and retain a raw observed-value marker.


## Extension rule

Any extension should retain the same lineage tables, add source metadata for new extracts, and compare old and new outputs before publishing. A new field belongs in the processed layer only after its definition, unit, period, territorial grain and treatment of missing values are documented.
