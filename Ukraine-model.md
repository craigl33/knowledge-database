**Contents**

[TOC]


# **General China model background**

- **Regions**: A WEO-style model with originally 8 regions but which was
  updated/reported in 6 regions in recent updates (the underlying
  structure is still 8 but we aggregate). In the previous report
  feedback it was noted that the central region is now separated so this
  should ideally be updated in future and we need to identify clearly
  that our modelling region aggregates these two regions.
- **Scenarios**: the main scenario analysis revolves mainly around
  sensitivities in the dispatching (full load hour allocations, or in
  the past, 'fair dispatch') and in trade between regions (based largely
  on flow constraints developed in the original model).
- **Generation**: Capacities come from WEO data and regional capacity
  splits were originally based on analysis by a consultant (Marco
  Boroni) taking into account existing capacity and regional adequacy
  considerations. These were subsequently updated (Net Zero Roadmap
  analysis in 2021) with input from Dan Wetzel and taking into account
  regional adequacy. These were not updated in the 2023 project but
  should be updated at least to account for the year change (2035
  to 2030) for the 2024 project.
- **Demand**: The demand profile comes from the WEO, containing the
  bottom up end use disaggregation. The regional demand disaggregation
  was originally set up by a RISE \[SIR\] team member ([Yugo
  Tanaka](file:///G:\DOCS\03%20PERSONAL\STAFF%20HAND%20OVER,%20etc\190730_Yugo))
  in 2018 and for subsequent models the analysis was extrapolated or
  interpolated to obtain the relevant year. Ideally this could be
  updated with more recent base data but would be somewhat time
  consuming.
- **Transmission**: capacities were originally based on information
  provided by a state grid secondee in WEO in 2018. For years further
  into the future we either undertook some transmission expansion in
  PLEXOS (original project for 2035) or excluded transmission
  constraints and did not analyse regional results (2021 Net Zero
  Roadmap analysis for 2060). The historical values including individual
  transmission lines was based on a GIS analysis using a grid map.
- **VRE profiles**: so far based on a site selection limited to 2000
  sites and paid generation data prepared by Vaisala. Ideally this could
  be updated to use the same profiles as WEO or failing that to generate
  them with our new gis setup.
- **Market design**: limitations in regional trading are represented
  through constraints on the annual use of transmission corridors
  between regions. Dispatching practices with administrative allocation
  of full load hours are represented with minimum capacity factor
  constraints on relevant generators.

# **2024 EFC project** 
(TODO: Ed to add)

# **2024 EFC project handover summer 2024**

- Demand:
  - All WEO inputs are saved in [the Load
    folder](file:///S:\China\2024_China_EFC\01_Data\06_Load) along with
    [combined demand
    inputs](file:///S:\China\2024_China_EFC\01_Data\06_Load\China_EFC_2024_04_09_combined_demand_inputs.xlsx),
    which contains everything that goes into the model. The
    'RegionalFactors' sheets for 2022 and 2030 are the ones that need to
    be updated with the demand splits for the new regions.

  - Status:
    - The hourly end use profiles from WEO contain negative values so we
      need to ask them about it
    - Changed the model to 7 nodes
    - Added new end uses to match the WEO demand
      - Code was created in
        [plexos-model-setup](https://gitlab.iea.org/iea/ems/rise/plexos-model-setup)
        (LoadSetup class) to create the PLEXOS inputs from the new
        formats saved in the combined demand inputs sheet
      - Needs to be tested to make sure it runs properly
      - EVs in the model are now called Shift12h (to match the index)
        but they are still represented with daily limits as are 10h and
        24h end uses
      - Some extra headers were added to the demand response index
        (Shed4h/Shift8h) for end uses with no shift or shed potential to
        avoid file errors for the old purchasers (it just creates 0
        columns with those names), but the purchasers could be deleted
        in PLEXOS and then these could be removed
    - Demand/generation totals from the PLEXOS run still need to be
      checked against the [WEO extended
      dataset](file:///G:\DOCS\09%20LITERATURE\IEA\WEO\WEO2023%20extended%20data\WEO2023_Extended_Data.xlsx)
  - Regarding the new regional splits, in principle [the main data is
    tracked
    here](file:///R:\RISE\DOCS\03%20PERSONAL\STAFF%20HAND%20OVER,%20etc\190730_Yugo\CHINA%20PSO%20modelling%20data%20inventory%202019_07_26_Rev.xlsx).
    - Looking at it, I'm not convinced that we can find anywhere the
      projections themselves by province as I think they were aggregated
      before projection or in some cases only ever developed at the
      regional level. In this case, rather than redoing the projection,
      as a simplification I would suggest:
      - Try to obtain the electricity demand / electricity sales by
        province for 2022
      - Use this to effectively subtract and add provinces from the
        regions in the [split I did
        here](file:///S:\China\2024_China_EFC\01_Data\06_Load\demand%20splitting%20progression%20based%20on%20PSO%20EFC%202024.xlsx)
        by assuming they take an even proportion of all end uses so that
        the new regions are the right size in demand even if the end use
        split is not perfect
      - Do the same for the future year, or adjust the proportions up or
        down slightly for end uses if you think the separated provinces
        have stronger or weaker growth prospects. Again it should mean
        the regions have about the right size even if the end use split
        is not perfect, which in the end is the most important. For
        residential you could use the projections in the population tab
        in [this excel
        sheet](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\CHINA\_ChinaPSO\06_Data\06_Load\WEO_Demand_Model_NPS_final\Assumption\20181120_Residential_provincial_projections.xlsx)
        for guidance to adjust the future split. For
        [services](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\CHINA\_ChinaPSO\06_Data\06_Load\WEO_Demand_Model_NPS_final\Assumption\20181205_regional%20parameter_service%20and%20agriculture.xlsx)
        and
        [industrial](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\CHINA\_ChinaPSO\06_Data\06_Load\WEO_Demand_Model_NPS_final\Assumption\20181113_regional%20parameter_industry.xlsx),
        these were only ever estimated on the regional level and it
        seems to be based on expert opinion, so I think we can justify
        adjusting the proportions slightly manually if it seems
        appropriate based on the information or expectations we have.
- Generation capacity and properties:
  - The [generator parameters
    sheet](file:///S:\China\2024_China_EFC\01_Data\02_PowerPlants\2024_03_2_generator_parameters_China_0.1_updating_plant_list.xlsx)
    contains all the model inputs and is saved in the [power plants
    folder](file:///S:\China\2024_China_EFC\01_Data\02_PowerPlants) with
    the various files used to create it.
    - The PLEXOS input files are [created by this R
      script](file:///S:\China\2024_China_EFC\07_Scripts\R\make_parameters_csvs_China_EFC_2024.R).
      It should only need to be pointed to the latest version of the
      generator parameters sheet and can be used to remake the input
      files any time parameters is updated.
    - The RegionSplit sheet in generator parameters is the one that
      needs to be updated to change the regional split of generation
      technologies. If a more granular split is desired (e.g. by
      individual plant type), the 'Category' column can be swapped for
      any of the index columns in 'all model indices' found in the
      gitlab folder and code will automatically match on the column
      name.
      - We can use the current data we have on generation by province to
        adjust the split for today (ideally mid or end 2022) and then
        the future splits could be guided by the previous project but
        suggest for Jiepeng to look into it and again we aim to maintain
        regional generation adequacy while starting from the current
        status and adjusting capacity from there to match the future
        numbers from WEO.
  - [Plant list with
    capacities](file:///S:\China\2024_China_EFC\01_Data\02_PowerPlants\capacity_list_for_pasting.csv)
    is created by the CapacitySetup class in the
    [plexos-model-setup](https://gitlab.iea.org/iea/ems/rise/plexos-model-setup)
    in gitlab.
    - This is then pasted into the generator parameters sheet in the
      'ScaledCapacities' tab as the RawCapacity
    - Currently, a manually created table of Maximum Capacities (derived
      from the China PSO 2019 project) is pasted in the same tab to
      represent the maximum unit capacity.
    - The class now also creates a [table of
      efficiencies](file:///S:\China\2024_China_EFC\01_Data\02_PowerPlants\efficiencies_table.csv)
      for pasting into generator parameters (in the LHVs tab). This was
      set up last minute so it needs some checking but it uses the first
      product/flow combination from all model indices (found in the
      templates and indices in the model setup gitlab) to match the
      plant technologies to the efficiency values in the DW (some plants
      are constructed from multiple product/flows so not 100% sure if
      this is correct in every instance).
  - The calculations in the Overview tabs have been checked to some
    degree but there could be errors and in general everything should be
    checked in the model runs to make sure the value ranges make sense
    - The lookups for the fair dispatch assumptions were fixed to deal
      with the new plant technologies
      - Should check that the mapping for the new technologies (Indices
        tab, FDtech column) makes sense
    - The heat rate had to be fixed (using the efficiency table above)
    - CHP setup should be checked that it is working/makes sense
  - A couple of files had been created ad hoc in the previous project
    for the parameters sheet and PLEXOS inputs so this was redone for
    the new technologies. The code is in the gitlab folder in /project
    scripts/China/2024_China_EFC/China 2024 EFC ad hoc code.py. Files
    created were:
    - [Max capacity by tech type for
      pasting](file:///S:\China\2024_China_EFC\01_Data\02_PowerPlants\maxcaps_from_old_for_pasting.csv)
      into the generator parameters sheet. This was based on regional
      maxes from the previous model with the [technologies mapped based
      on the
      index](file:///S:\China\2024_China_EFC\01_Data\02_PowerPlants\max%20capacities%20based%20on%20previous%20model%20update%20plant%20techs.xlsx)
      so it could be good to check it and potentially update if there
      are any issues.
    - PLEXOS input file
      [CHPminCFhour](file:///S:\China\2024_China_EFC\03_Modelling\01_InputData\01_GeneratorParameters\CHPminCFhour.csv)
      which is used in the new CHP setup which was used to fix the
      issues with CHP not working correctly in the older model
  - Capacity totals from the PLEXOS model still need to be checked
    against [WEO extended
    data](file:///G:\DOCS\09%20LITERATURE\IEA\WEO\WEO2023%20extended%20data\WEO2023_Extended_Data.xlsx)
    (note that due to the capacity derating approach ours should be
    lower. There are tabs in the parameters sheet (Installed Summary for
    checks and InstalledCheck for pasting the PLEXOS solution file data
    direct from PLEXOS) that could be updated to check the derated and
    non-derated capacities to check that non-derated matches WEO
    extended data and the derated adjustment is correct and aligned with
    the PLEXOS model.

- Transmission
  - The active setup being used in the previous project was only using
    simplified lines representing the entire corridor between regions
    with one line. To update we would therefore only need the aggregate
    capacities in each direction
  - So far this has only been updated by removing the lines involving
    NSR, so the capacities and potentially the linkages between regions
    would need to be adjusted to reflect the new 7-node setup

- VRE profiles
  - It might be worth discussing with WEO what they have now (they
    recently redid the generation data analysis and I think they have
    something new for China) and if it could be better for us to take
    data from them to be more aligned.
  - Current profiles have simply been inherited from the previous
    project. Ideally at least the old data, [saved in this
    folder](file:///X:\Weather_data_overflow\01_Vaisala_Raw_Data), would
    be remapped to the new regional setup. The original indexing of the
    2000 sites to the regions can be found [here for
    solar](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\CHINA\_ChinaPSO\06_Data\03_VREdata\20181127-china_sites_solar_VAISALA_FINAL.csv)
    and [here for
    wind](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\CHINA\_ChinaPSO\06_Data\03_VREdata\20181127-china_sites_wind_VAISALA_FINAL.csv),
    and [this script used for reprocessing the
    data](file:///S:\China\2021a_China_adequacy\07_Scripts\Python\China_Vaisala_multiyear_profiles.py)
    for the adequacy project that accessed these and the data could
    provide a starting point.

- Prices
  - There is data for coal, oil, gas provided by WEO in the DW. Basic
    fuel prices were set up in the model [using a manual
    sheet](file:///S:\China\2024_China_EFC\01_Data\02_PowerPlants\fuel%20prices%20manual%20adjustments.xlsx)
    to create the input files based on the DW data. For coal past
    regional variation was applied with the (flat) average equal to the
    new WEO value. Some numbers stolen from Ukraine for
    0000000000000000000000000000000000000000000000Low C fuels - ideally
    might be improved specifically for China

- Reserves
  - These were copied over from the previous project and need to be
    updated for the new demand, generation, regional split, etc.

 

 

# **2022/3 project summary**

- [2022/23 project](file:///S:\China\2023_China%20proj) Funded by Energy
  Foundation China. New scenarios were developed but the underlying
  model inputs were not updated. The main deliverable was a report,
  [Building a Unified Power System in
  China](https://www.iea.org/reports/building-a-unified-national-power-market-system-in-china),
  published in 2023.

- Changes to the scenarios were based on
  - Applying full load hour allocations to some regions only to reflect
    changes in the way these markets are being operated. While these
    variations are at the province level and could not be correctly
    captured in the regional model, instead regions were selected to
    cover approximately the same amount of conventional capacity as the
    relevant provinces. This requires some caution in presenting
    regional results or specific flows.
  - In addition, the control of the flows was adjusted to capture the
    move away from full load hour allocations. Since these apply to
    dispatch within the relevant provinces not to trading, the flows
    were determined using a run with full load hour allocations applied
    to all regions. The flows were then constrained for the run with
    economic dispatch in some regions.

- Useful files and locations
  - [Report](https://www.iea.org/reports/building-a-unified-national-power-market-system-in-china):
    scenario settings description p47-48, model results p49-55,
    modelling annex p69-74.
  - [Presentations](https://ieaorg.sharepoint.com/:f:/r/sites/EMS-RISE/Shared%20Documents/China/3.%20EFC%20power%20markets/3.%20Presentations?csf=1&web=1&e=MgEvvI)
  - [Generator parameters
    sheet](file:///S:\China\2023_China%20proj\01_Data\02_PowerPlants\2023_02_03_generator_parameters_China_ETP_v1.5_separate_biomass_operational_class.xlsx),
    load was not updated. Minor updates to transmission to reduce the
    capacity to the South based on current/planning status input /
    advice from Zhiyu.

 

# **Pre-2022 projects summary**

- Originally developed in 2018 and published early 2019, supported by an
  EPPEI secondee (LI Xiang).
  - Report: [China Power System
    Transformation](https://www.iea.org/reports/china-power-system-transformation).
    Scenarios description from p134, model description p143, results
    from 144, detailed modelling annex p181-187.
  - Locations and sheets: modelling folder, [data
    folder](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\CHINA\_ChinaPSO\06_Data\),
    generator parameters sheet, load data (old system), [data
    inventory](file:///G:\DOCS\04%20PROJECTS\COUNTRIES\CHINA\_ChinaPSO\06_Data\CHINA%20PSO%20modelling%20data%20inventory%202019_07_26_Rev.xlsx)

  - Presentations:
 

- The most recent capacity update before the current model was done in
  2021 in the China net zero roadmap. We modelled 2035 and 2060 but did
  not fully update the base year:
  - Report: [An energy sector roadmap to carbon neutrality in
    China](https://www.iea.org/reports/an-energy-sector-roadmap-to-carbon-neutrality-in-china),
    model results p85-86, we also helped give input to the capacity
    splitting shown on p83 and the regional peak demand on p87.
  - Locations and sheets: [modelling
    folder](file:///S:\China\2021c_China%20ETP%20NZE), [data
    folder](file:///S:\China\2021c_China%20ETP%20NZE\01_Data), demand
    sheet, generation sheet
  - Presentations (?)



**Useful resources**

-  
