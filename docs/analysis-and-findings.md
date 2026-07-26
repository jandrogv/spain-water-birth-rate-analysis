# Analysis and findings

## Reading this page

This project uses descriptive visual analysis. Each block below identifies the question, data level, visual choice, observed pattern, thesis interpretation and an interpretation limit. The thesis provides the academic narrative and figures; [02_analyse_and_visualise.Rmd](../scripts/02_analyse_and_visualise.Rmd) provides the executable visual definitions. Neither source contains a causal model or statistical test for the water-population comparisons.

The population series spans 1975-2021. Water-service measures span 2000-2020. Cross-domain views must therefore be read only over their common period.

## National demographic analysis

### Population through time

**Question.** How has the total Spanish population changed across the available long-run series?

**Data and aggregation.** Province-year population is aggregated to a national total in the analysis workflow.

**Visual.** A national line chart in the `Grafico de la Poblacion Anual de España` section of Stage 2.

**Observed pattern.** The thesis describes sustained growth from the late 1970s, acceleration around the 2000s, a temporary decline around 2013, and renewed growth after the mid-2010s. [Thesis, PDF pp. 20-21](../report/bachelor-thesis-es.pdf#page=20)

**Interpretation.** The thesis contextualises these periods with changing economic conditions, birth patterns and migration.

**Limit.** The line itself does not identify the contribution of any single driver. It is an aggregate descriptive series.

### Births and deaths

**Question.** How do births and deaths evolve in relation to one another?

**Data and aggregation.** Annual birth and death fields joined to province population records and aggregated to Spain.

**Visual.** A dual time-series comparison in `Grafico de la Natalidad vs Mortalidad en España`.

**Observed pattern.** The thesis describes a falling birth trend and rising mortality in later years, with an interval in which deaths exceed births. [Thesis, PDF pp. 53 and 56](../report/bachelor-thesis-es.pdf#page=53)

**Interpretation.** The thesis links this description to population ageing and lower fertility in its demographic narrative.

**Limit.** The project does not estimate age-standardised rates, fertility determinants or mortality causes. Contextual explanations are not formal causal estimates.

### Internal and external migration

**Question.** How do internal and external movement flows contribute to population dynamics?

**Data and aggregation.** Province-level entry and exit fields for internal and external migration, aggregated for national and community views.

**Visual.** A national migration time series and regional heat maps in Stage 2, including `Grafico de la Migracion Anual en España`.

**Observed pattern.** The thesis reports that incoming migration contributes to the later recovery of population growth while low births and rising deaths persist. [Thesis, PDF p. 56](../report/bachelor-thesis-es.pdf#page=56)

**Interpretation.** Migration is treated as a visible demographic component rather than as a standalone explanation for every change.

**Limit.** Migration-source coverage begins later than the population series. Missing or unavailable earlier migration values must not be read as zero movement.

### Population change

**Question.** When does the overall population increase or decrease?

**Data and aggregation.** The processed province workbook supplies population and demographic-flow fields; the workflow creates population variation.

**Visual.** `Grafico del Aumento y disminucion de la Poblacion en España`.

**Observed pattern.** The thesis separates 1975-2008, 2009-2015 and 2015-2020, describing a shift toward greater dependence on incoming migration where births no longer offset deaths. [Thesis, PDF p. 56](../report/bachelor-thesis-es.pdf#page=56)

**Interpretation.** The chart provides a compact view of demographic balance.

**Limit.** The periodisation and contextual discussion come from the thesis. The chart does not test structural breaks or forecast future population.

## Regional demographic analysis

### Autonomous-community heat maps

**Question.** Which communities differ in population level, births, deaths and migration over time?

**Data and aggregation.** `Poblacion_Comunidades.xlsx`, with 19 community-level entries and annual observations from 1975 to 2021.

**Visual.** Heat maps in Stage 2 for population, births, deaths, internal migration and external migration. Rows encode years and columns encode territories, enabling a simultaneous time and region comparison.

**Observed pattern.** The thesis identifies Andalusia, Catalonia, the Community of Madrid and the Valencian Community as high-population and comparatively dynamic regions. It also distinguishes more stable or lower-population groups and highlights growth in the Canary and Balearic Islands. [Thesis, PDF pp. 58-59](../report/bachelor-thesis-es.pdf#page=58)

**Interpretation.** Regional trajectories differ materially; national averages hide that variation.

**Limit.** Heat-map colour encodes magnitude, not significance. Differences may reflect population scale as well as different rates or mechanisms.

### Community population variation

**Question.** Where did population change most over multi-year windows?

**Data and aggregation.** Community population and variation fields derived using multi-year lags.

**Visual.** Community choropleths for selected comparison years in `Mapa Variacion de Poblacion para las Comunidades de España`.

**Observed pattern.** The thesis highlights declines in Castilla y León, Asturias and Extremadura in one comparison, while other regions show more positive or stable paths. [Thesis, PDF p. 63](../report/bachelor-thesis-es.pdf#page=63)

**Interpretation.** The map is useful for locating the territorial pattern of demographic change.

**Limit.** A map does not establish why a territory changed. Economic, social, policy, age-structure and mobility factors are not separately modelled.

### Province maps and density

**Question.** How are population and density distributed within Spain?

**Data and aggregation.** Province-year population joined with territorial area.

**Visual.** Choropleth maps for population and density in 2000, 2010 and 2021, using `mapSpain`, `sf` and `cartography`.

**Observed pattern.** The analysis makes concentration and sparse-territory contrasts visually inspectable at province level.

**Interpretation.** Density adds geographic context that an absolute population map cannot provide.

**Limit.** Province area is large and heterogeneous. Density is a broad territorial ratio, not a measure of settlement pattern within a province.

## National water-service analysis

### Total, registered and unregistered water

**Question.** How did network supply and its components evolve between 2000 and 2020?

**Data and aggregation.** Community water records are aggregated to a national annual view.

**Visual.** Three Stage 2 lines: total annual supply, registered and distributed water, and unregistered water.

**Observed pattern.** The thesis describes total supply rising from 2000 to 2008, then falling and later stabilising. It describes registered and distributed water as stabilising around later values and unregistered water as rising before later reduction. [Thesis, PDF pp. 66-69](../report/bachelor-thesis-es.pdf#page=66)

**Interpretation.** Separating the measures avoids treating all supplied water as final distributed use.

**Limit.** Unregistered water is an accounting and network indicator in this source context. It should not be interpreted automatically as physical leakage, nor should changes be assigned to a single policy or climate cause.

### User-group allocation

**Question.** How is registered-water distribution allocated across households, municipal consumption and economic sectors?

**Data and aggregation.** Community-year allocation fields aggregated to national totals.

**Visual.** Line or area-style comparisons and an allocation view in `Grafico del Suministro de Agua por Grupo de Usuarios`.

**Observed pattern.** The thesis describes changing distribution across these groups and notes a growing relative emphasis on household allocation in its narrative. [Thesis, PDF p. 70](../report/bachelor-thesis-es.pdf#page=70)

**Interpretation.** Allocation provides a more informative view than total supply alone because different user groups have distinct demand profiles.

**Limit.** The project does not model sector production, municipal service needs or household behaviour. Shares do not identify priority decisions or welfare impacts.

### Potable and non-potable availability

**Question.** How did the two available-water categories move through the observed water period?

**Data and aggregation.** Community annual availability values aggregated nationally.

**Visual.** Separate Stage 2 time-series charts for potable and non-potable availability.

**Observed pattern.** The thesis describes early growth followed by a decline after the mid-2000s for both categories. [Thesis, PDF p. 71](../report/bachelor-thesis-es.pdf#page=71)

**Interpretation.** The distinction supports a more precise discussion of service availability.

**Limit.** These measures are neither a reservoir series nor a complete hydrological balance. They cannot independently quantify drought severity or water security.

## Regional water-service analysis

### Water heat maps and choropleths

**Question.** Which communities differ in supply, supply per area, household allocation, per-capita household supply and availability?

**Data and aggregation.** `Agua_España.xlsx`, with 399 rows, 34 columns, 19 community-level entries and 2000-2020 coverage.

**Visual.** Stage 2 provides heat maps for total supply, water per square kilometre, household supply, per-capita household supply, potable availability and non-potable availability. It also maps selected years for total supply, household supply, water per square kilometre, per-capita household supply and household-supply variation.

**Observed pattern.** The thesis notes that larger communities such as Andalusia and Catalonia have high water volumes and describes heterogeneity in regional supply and availability. [Thesis, PDF pp. 72-76](../report/bachelor-thesis-es.pdf#page=72)

**Interpretation.** The regional views reveal that national trends do not apply uniformly to all communities.

**Limit.** Large absolute volumes can reflect population, territorial scale, economic structure or service configuration. They do not identify a single driver.

### Household supply per inhabitant

**Question.** How does household supply relate to community population over time?

**Data and aggregation.** Household supply divided by community population, expressed by the thesis as daily litres per inhabitant.

**Visual.** Community heat map and selected-year choropleth.

**Observed pattern.** The thesis reports a decline from around 170 litres per inhabitant per day toward around 120-130 in later years. [Thesis, PDF p. 77](../report/bachelor-thesis-es.pdf#page=77)

**Interpretation.** This is a normalised comparison that makes different population sizes more comparable.

**Limit.** The metric is based on household supply, not direct metered consumption by each individual. It may reflect both demand and network or allocation changes.

### Household-supply variation

**Question.** Which communities increased or reduced supply to households relative to an earlier period?

**Data and aggregation.** Community household-supply variation using lagged values.

**Visual.** A community map for household-supply variation.

**Observed pattern.** The thesis describes broad reductions and identifies exceptions in its comparison. [Thesis, PDF pp. 78-79](../report/bachelor-thesis-es.pdf#page=78)

**Interpretation.** The view supports targeted comparison of change rather than level.

**Limit.** Lagged percentage change can be volatile when starting values are small. It is not a causal effect of population, policy or climate.

## Cross-domain exploratory views

The final Stage 2 section contains four community-level scatterplots:

1. population and total water supplied;
2. population and water supplied to households;
3. population and water available for potabilisation; and
4. population and non-potable water availability.

**Question.** Do community-level population and selected water variables display visible co-movement in the observed 2000-2020 records?

**Visual choice.** Scatterplots make the joint distribution visible without fitting a model.

**Observed association.** The charts provide an exploratory view of size and variation across communities.

**Interpretation limit.** No correlation coefficient, regression, confidence interval, p-value, control variables or causal design is included. They are not evidence that population causes water outcomes, or that water measures cause migration or demographic change.

## Portfolio-worthy findings

| Finding | Evidence | Guardrail |
| --- | --- | --- |
| Long-run national population growth is not uniform across all years | Thesis Chapter 6.1, especially PDF pp. 52 and 56; Stage 2 national population and change charts | Descriptive time-series observation |
| Later demographic balance is shaped by falling births, rising deaths and migration flows | Thesis PDF pp. 53-56; Stage 2 births, deaths and migration charts | Not a demographic forecast or causal decomposition |
| Large communities show markedly different demographic trajectories from lower-population territories | Thesis PDF pp. 58-64; Stage 2 community heat maps and variation map | Absolute population and rates must be distinguished |
| Water supply has a distinct 2000-2020 trajectory with an initial rise, reduction and later stabilisation in the thesis narrative | Thesis PDF p. 66; Stage 2 annual-supply chart | Not a measure of reservoir levels |
| Registered and distributed water, unregistered water and household allocation answer different analytical questions | Thesis PDF pp. 67-70; Stage 2 supply-component charts | They are not interchangeable labels |
| Potable and non-potable availability both show change over the observed period | Thesis PDF p. 71; Stage 2 availability charts | Availability is not a complete water-security model |
| Household supply per inhabitant declines in the thesis descriptive account | Thesis PDF p. 77; Stage 2 household-consumption map and heat map | Household supply proxy, not individual metered use |
| Cross-domain scatterplots are a useful transparency device for visual comparison | Stage 2 `Analisis de Correlacion` section | No tested correlation or causal claim follows |

## Interpretation standard

The strongest reading of this repository is that it documents temporal and territorial patterns in two connected policy domains. It can show measured differences, changes and visual associations. It cannot isolate causal mechanisms without additional data, a defined identification strategy and statistical modelling. This distinction is deliberate and central to the professional presentation of the project.


## How to use the findings responsibly

A portfolio finding is strongest when it links a visible chart or map to a precise processed field and names the scope of the comparison. This repository supports statements such as: a specified water-service indicator changed between 2000 and 2020; a community appears higher or lower under an active metric; or population trajectories differ between named regions. It does not support claims that a chart proves a policy, climate event, migration flow or demographic characteristic caused that change. The difference between these two statement types is part of the analytical quality of the project.

The thesis includes contextual explanations and recommendations, especially around sustainable water management. Those passages are useful for communicating why the analysis matters, but they are kept separate here from measured observations. The R Markdown artefacts remain the source for actual chart construction, while the thesis remains the source for the original academic interpretation.
