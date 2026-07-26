# Reproducibility and limitations

## Reproduction path

The repository is structured around a two-stage R Markdown workflow.

1. Start from the project root.
2. Install the R packages imported by the scripts.
3. Render [01_prepare_datasets.Rmd](../scripts/01_prepare_datasets.Rmd) to read `data/raw/` and write `data/processed/`.
4. Inspect the generated principal workbooks.
5. Render [02_analyse_and_visualise.Rmd](../scripts/02_analyse_and_visualise.Rmd) to create the analytical charts and maps.
6. Optionally render [Tablas_TFG.Rmd](../scripts/Tablas_TFG.Rmd) to regenerate the formatted reference tables.
7. Open the Power BI file only after confirming or updating its local source paths in Power BI Desktop.

The scripts use `here::here()` for active file paths. That strategy makes paths root-relative and avoids reliance on the directory from which R Markdown is launched.

## R packages

No R version is pinned in the repository, so an exact version cannot be verified from the project files. The preparation script imports `readxl`, `tidyr`, `dplyr`, `xlsx`, `openxlsx`, `zoo` and `here`. The analysis script additionally imports `ggplot2`, `scales`, `knitr`, `officer`, `flextable`, `cartography`, `mapSpain`, `tidyverse` and `sf`. The table workflow imports the relevant data packages plus `kableExtra`, `officer` and `flextable`.

A suitable local R installation must include the system dependencies required by geospatial packages such as `sf`. Package versions, operating system, map data and rendering engines can affect presentation details even when the underlying calculations remain unchanged.

## Expected Stage 1 outputs

The principal processed outputs are:

- `Poblacion_Provincias.xlsx`
- `Poblacion_Comunidades.xlsx`
- `Poblacion_Edad.xlsx`
- `Agua_España.xlsx`

The retained full preparation workflow also writes `Tabla_Final.xlsx`, an integrated community-level comparison table. The standalone table script writes `Tablas.docx`. See [Data sources and ETL](data-sources-and-etl.md) for the intended grain, periods and fields.

## Stage 2 rendering

Stage 2 reads the province population, community population and water workbooks. It creates national lines, regional heat maps, province and community choropleth maps, and descriptive scatterplots. It does not fit a causal model or publish statistical significance tests for the cross-domain charts. A successful render proves that the paths and available packages are compatible in the local environment; it does not turn visual co-movement into causal evidence.

## Power BI refresh

The committed report is [Dashboard_Multilanguage.pbix](../dashboard/Dashboard_Multilanguage.pbix). Inspectable metadata identify four required processed sources:

- `data/processed/Poblacion_Provincias.xlsx`
- `data/processed/Poblacion_Comunidades.xlsx`
- `data/processed/Poblacion_Edad.xlsx`
- `data/processed/Agua_España.xlsx`

The archive does not expose inspectable local refresh paths. The owner must open the report in Power BI Desktop, inspect each query source, update only the file locations, refresh, and validate all report pages. Detailed steps are in [Power BI source update](power-bi-source-update.md). The dashboard refresh is an owner action, not a completed validation claimed by this repository.

The thesis bibliography lists a dashboard video link: [YouTube dashboard video](https://youtu.be/e4oLa0Sl2zw). Its present availability has not been validated as part of the local repository check.

## Data and interpretation limitations

### Different coverage windows

Population records span 1975-2021, while water-service records span 2000-2020. Analysis involving both domains is therefore restricted to their overlap. Migration source coverage also begins later than the broad population series. [Thesis, PDF pp. 30-32](../report/bachelor-thesis-es.pdf#page=30)

### Interpolation

The water preparation uses linear interpolation for selected missing years, identified by the thesis as 2015, 2017 and 2019. Interpolated values support continuity in charts, but they are estimates rather than directly observed values. [Thesis, PDF p. 37](../report/bachelor-thesis-es.pdf#page=37)

### Aggregation and geography

The workflow uses provinces and autonomous communities, not individual households, municipalities or hydrological basins. A regional average can conceal local variation. Community-level water indicators are also not direct measures of all water use, physical losses, climate conditions or reservoir storage.

### Descriptive, not causal

The repository contains visual analysis and contextual thesis interpretation. It does not include a causal model, controlled design, uncertainty analysis or statistical test establishing that demographic change causes water availability, supply or consumption outcomes. Water, demographic, economic, policy and climate factors may move together or independently.

### Source extracts and currency

The repository preserves source extracts used for the thesis. They are not a live data feed. Data periods end in 2021 for population and 2020 for water, so this project should not be used to describe current conditions without a separate documented update.

### Redistribution and licensing

Source files are attributed to public institutions, but public availability does not remove source-specific reuse conditions. Consult the original INE and Ministry terms before redistribution. No repository license file is currently committed. Any future repository license can apply only where the project author can grant rights.

## Practical validation checklist

Before presenting an updated version:

- confirm every raw input is present under `data/raw/`;
- render Stage 1 from the root and compare output schemas and row counts with the committed processed workbooks;
- render Stage 2 from the root and inspect for path errors;
- inspect charts for unintended historical changes;
- update Power BI source paths in Power BI Desktop only if necessary;
- refresh the dashboard and verify its six documented views; and
- update this documentation if sources, periods or measures change.

This process keeps the work reproducible while clearly separating verified project history from future owner actions.


## Environment preparation

A practical local setup begins with a current R installation, R Markdown rendering support and Power BI Desktop only if dashboard refresh is needed. Install packages in a user library rather than changing repository code. Open the project root as the working project so that `here::here()` resolves paths consistently. Before rendering, compare the raw and processed folder names with the file references in the scripts.

The original workflows write Excel and document artefacts. Rendering should therefore occur in a working copy where overwriting expected processed outputs is acceptable. Preserve a backup or use version control before regeneration. The repository historical outputs are useful comparison points: row counts, columns and selected values should be checked after Stage 1 rather than assumed to match.

## Result validation after rendering

A reproducible path is more than a successful command. After Stage 1, verify the four principal workbooks, the auxiliary integrated table and their year ranges. After Stage 2, inspect the generated output for missing maps, package warnings, empty charts or path errors. Compare the result with the committed thesis and screenshots only to identify unintended change; do not alter data or logic merely to reproduce an appearance.

Power BI is a separate validation boundary. A report can open while still containing stale file paths or requiring credentials. Refresh validation requires checking source steps, relationships, measures, visuals, slicers and all four verified pages in Power BI Desktop. The screenshots are useful portfolio evidence, but they are not proof that a current local refresh completed.

## Extending the project

A future extension should preserve the raw-to-processed separation, version new extracts, record source-download dates, and document any revised geographic or temporal coverage. New years should not be appended without reviewing how interpolation, lagged variations, territorial name changes and dashboard relationships behave. If the research question changes from descriptive comparison to causal analysis, it needs a separate design, explicit outcome and exposure definitions, appropriate controls and uncertainty reporting.
