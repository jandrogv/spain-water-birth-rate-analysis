# Power BI source update

## Status

`dashboard/Dashboard_Multilanguage.pbix` is a valid Power BI archive. Its inspectable metadata references these processed datasets:

- `Poblacion_Provincias.xlsx`
- `Poblacion_Comunidades.xlsx`
- `Poblacion_Edad.xlsx`
- `Agua_España.xlsx`

The archive does not contain a `DataMashup` file or inspectable local file paths. Its connection manifest identifies a remote Power BI artifact, so the current refresh-source paths cannot be safely determined or changed without Power BI Desktop.

## Required owner action in Power BI Desktop

1. Make a backup copy of `dashboard/Dashboard_Multilanguage.pbix`.
2. Open it in Power BI Desktop.
3. Select **Transform data**, then inspect every query and its **Applied Steps**.
4. For each file-based source, update the `Source` step or use **File > Options and settings > Data source settings > Change Source** to these repository-relative targets:

   - `data/processed/Poblacion_Provincias.xlsx`
   - `data/processed/Poblacion_Comunidades.xlsx`
   - `data/processed/Poblacion_Edad.xlsx`
   - `data/processed/Agua_España.xlsx`

5. Preserve table names, transformations, relationships, measures, and visuals; only update file locations.
6. Select **Close & Apply**, refresh the report, and verify all four pages and the dashboard screenshots.
7. Save the updated PBIX and commit it only after successful refresh validation.

Do not claim that Power BI paths have been updated until this owner validation is complete.