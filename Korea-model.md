**Contents**

[TOC]

# **General model background**

- **Regions**: [5-node
  model](file:///S:\Korea\2021_market%20design\02_MiscDocsFiguresFiles\korea_model_structure_diagram.pptx)
  based on the control regions of the Korean power system.

- **Scenarios**: Core sensitivities were comparing the Korean
  development plan (BPLE) scenario with the APS, inclusion of
  flexibility options including EVs and impact of CO2 pricing.

- **Generation**: Base scenario based primarily on publicly available
  capacity data from KPX and refined based on input from the project
  partner, KEEI. For the WEO scenario, WEO shared with us high level
  capacity and generation data and the BPLE based individual plant
  capacities were manually adjusted (e.g. adding or removing units) to
  match the high level target with adjustments reasonably balanced
  across the regions. Must-run constraints were based on data provided
  through KEEI.

- **Demand**: Historical hourly demand from KPX was provided by KEEI,
  but this is now available online (and in the IEA DW).

- **Transmission**: Tx lines were set up in the model based on [publicly
  available information](https://www.kpx.or.kr/eng/contents.do?key=333)
  but eventually Tx constraints were not used as the capacity was
  insufficient and it seemed that lower level lines between regions were
  probably needed but we could not obtain them from the project partner
  (they discussed with KPX but in the end the information was somewhat
  sensitive and it did not come about).

- **VRE profiles**: Derived using our own GIS analysis and ERA5 weather
  data.

- **Market design**: Analysed based on CO2 price and different
  approaches to calculating plant revenues. Important to note that for
  the revenue calculations, runs with must-run constraints had the
  hourly prices distorted (marginal price reported as that of the higher
  cost must-run generation). To address this, after some exchanges with
  Energy Exemplar, we set up dedicated calculations that use two
  separate model runs to calculate the revenues: one where constraints
  that distort the prices are excluded and one with everything included.
  Hourly prices are taken from the unconstrained model while hourly
  generation is taken from the other to calculate plant revenues. The
  calculations were originally done in R and an initial translation to
  python is included in gitlab but has not yet been integrated and
  tested. If used for future models and the model does not have price
  distortions, the script can just be pointed to the same file for both
  the price curve and the generation results.

 

# **Latest project summary**

- Seasonal flexibility analysis: this was an update used for the WEO-led
  seasonal flexibility work (originally for Phase I but eventually
  included only in Phase II) where the generation capacities for the APS
  were adjusted to match new WEO numbers and the VRE generation analysis
  was extended to add multiple weather years (this component was
  eventually not used\[?\]). The generation numbers particularly for VRE
  were not well aligned with the WEO assumptions so some adjustments to
  the regional split were made to bring it closer although it could not
  be fully matched. Ideally this would be fixed in future with more
  analysis on the input assumptions including info from WEO on the basis
  of their assumptions and perhaps preparing VRE profiles with different
  technology choices etc.

- Useful files and locations

  - Report: forthcoming

  - Presen

  - Locations and files: data folder, [capacity
    sheet](file:///S:\Korea\2024_seasonal\01_Data\02_PowerPlants\2024_01_17_generator_capacities_full_2035_korea%20seasonal_adjust%20wind%20proportions%20for%20CF%20alignment.xlsx)
    containing capacity adjustments (plexos_list_add_data tab),
    [generator parameters
    sheet](file:///S:\Korea\2024_seasonal\01_Data\02_PowerPlants\2024_01_12_generator_parameters_for_korea_seasonal.xlsx)
    (updated capacity pasted manually from the capacity sheet),
    [combined load
    sheet](file:///S:\Korea\2024_seasonal\01_Data\06_Load\Korea_seasonal_load_2024_01_16.xlsx)
    (APS_adj tab)

 

# **Previous projects summary**

- Seasonal flex analysis phase 1 - this was developed but eventually not
  used for the first report.

- Seasonal flex analysis phase 2 - this was developed and contributed to
  the one section of the report (drafted by Keith)

  - Report: [Managing the Seasonal Variability of Electricity Demand and
    Supply](https://www.iea.org/reports/managing-the-seasonal-variability-of-electricity-demand-and-supply),
    from P71. The model is based on the WEO 2022 APS assumptions which
    reflects 10<sup>th</sup> BPLE. Figure 44 and 46 are calculated by
    the WEO team based on our results. (The result contains the
    stochastic analysis but was not used)

  - Locations and files: [modelling project
    folder](http://V:/Korea/2024_seasonal), [key figures or
    documents](https://ieaorg.sharepoint.com/:f:/r/sites/EMS-RISE/Shared%20Documents/Seasonal%20Variability/Phase%202%20RISE?csf=1&web=1&e=KLSK0i)

- Korea market design analysis - this was the original analysis where
  the model was set up, funded by KEEI for the Korean Ministry.

  - Report: [Reforming Korea's Electricity Market for Net
    Zero](https://www.iea.org/reports/reforming-koreas-electricity-market-for-net-zero),
    model introduction and results from p33, market design analysis
    including profitability and CO2 impact from p55, flexibility
    analysis from p79, model annex p95

  - Locations and files:
    [presentations](https://ieaorg.sharepoint.com/:f:/r/sites/EMS-RISE/Shared%20Documents/Korea%20-%20Market%20Design/Presentations?csf=1&web=1&e=O7mwkg),
    [modelling project folder](file:///S:\Korea\2021_market%20design),
    [data
    folder](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\KOREA\Market%20Design%20for%20net%20zero\Modelling\06_Data),
    [capacity
    sheet](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\KOREA\Market%20Design%20for%20net%20zero\Modelling\06_Data\02_PowerPlants\2021_10_24_generator_capacities_full_2035_geospatial%20distribution%20for%20vre.xlsx),
    [generator parameters
    sheet](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\KOREA\Market%20Design%20for%20net%20zero\Modelling\06_Data\02_PowerPlants\2022_07_22_generator_parameters_for_stochastic_2035_APS_cost_stochastic.xlsx),
    [combined load
    sheet](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\KOREA\Market%20Design%20for%20net%20zero\Modelling\06_Data\06_Load\Korea_market_design_load_2021_09_27.xlsx),
    [handover information
    presentation](file:///S:\Korea\2021_market%20design\02_MiscDocsFiguresFiles\IEA%20Korea%20Regional%20Model%20handover%20slides.pptx)
