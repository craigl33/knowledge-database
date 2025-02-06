The following represents the general modelling workflow and a checklist for when you are preparing a new model in PLEXOS.

The PLEXOS model itself will consist of the following components:

[[_TOC_]]

## Generation

Generation data for a project will come from either country-level sources (direct stakeholders) or from WEO projections. As WEO projections are, at best, at a national level, this will require some capacity splitting. For country-level data, this should be requested with regional information to allow the assignment of generators to specific regions.

Capacity splitting for existing generation should be based on data collected on the existing regional split, either from contacts in the country or public domain information.

For the split of generation in the future power system, this will depend on both the retirement of existing generation and the building of new resources. Ideally, you would account for these separately. Though not built into the current tool for obtaining capacity, WEO can provide data on retirements which could be split based on existing generation split (as an example).

For new generation, the regional splitting factor should be based on available resources around, for example:

1. Capacity expansion plans
2. Forecasts for TDPs and other plans (e.g. ERRA or TYNDP in Europe)
3. Project pipelines

Generation data should be provided in a standard format that allows matching to the [**model indices**](https://gitlab.iea.org/iea/ems/rise/plexos-model-setup/-/blob/main/templates%20and%20indices/all%20model%20indices.csv?ref_type=heads) as per the template provided in [plexos-model-setup](https://gitlab.iea.org/iea/ems/rise/plexos-model-setup) package. Generator parameters (as per the Indices tab of the generator_parameters template file found in the [templates and indices](https://gitlab.iea.org/iea/ems/rise/plexos-model-setup/-/tree/main/templates%20and%20indices?ref_type=heads) folder) are used for create generic properties of generators based on technology class. These should be reviewed and updated per project.

For plant-level data (i.e. for country-level generation parameters) these should be provided with the right column name (as per generator_parameters template file) and specified as plant-level parameters in the configuration file (TOML).

## Transmission

When the project commences, a decision must be made on the regional representation of the power system being modelled. Modelling is performed at an aggregated regional level for a number of reasons:

1. Data availability
2. Computation time
3. Trade-off of effort vs. benefit for high-level analysis
4. Required speed of development of model, coupled with needs to validate and explore results

As a result, in general, we model power systems with a very high-level representation of transmission. Regions are chosen based different factors depending on the system, but in general correspond to the following:

1. Regional control or planning zones as per the TSO
2. Identificaiton of key transmission bottlenecks

Lines are modelled as pipelines between these regions. Ideally, there would be information on the existing or planned transfer capacity of different transmission corrdidors. In the absence of detailed information around transmission capacity between regions, this has been assumed based on the physical transmission network, assuming the Surge Impedance Loading (SIL) of different lines based on their voltage level. The topology of the transmission network would either be based on direct information from stakeholders or on open-source information such as OpenStreetMaps (which can be analysed visually or using geospatial analysis).

Note that this part of the model set-up has typically been done manually.

## Demand

Demand is modelled at a regional level. For WEO projects, this is provided at end-use level which has permitted our analysis to include demand response of different end-uses, either for shifting or shedding (interruptible load). The end-uses [can be seen here](https://gitlab.iea.org/iea/ems/rise/knowledge-database/-/wikis/Modelling-overview).

In most cases, WEO's demand team has split the demand regionally, taking into account regional factors for growth of different end-uses. However, we have also sometimes had to find creative ways to split this demand based on what data was avaialble (e.g. [China EFC 2024](https://gitlab.iea.org/iea/ems/rise/knowledge-database/-/wikis/China-model) project).

For country-level work, we have typically received demand at a regional level, however a similar approach has often been necessary to split national demand into different regions (e.g. Ukraine 2024 work). In all these cases, a manual/adhoc step is necessary to prepare the final demand data.

In cases where only existing demand is provided, there will also be an additional step to create future demand profiles based on forecasted annual and peak demand values. PLEXOS itself provides a tool to do so (as used for Thailand and Ukraine work). This, however, has shortcomings for regions with significant electrification in transport, heating, cooling and other end-uses. Therefore creative methods are often needed to add new electrified loads (based on end-use profiles from WEO or the [IEA EV Integration Tool](https://www.iea.org/data-and-statistics/data-tools/electric-vehicle-charging-and-grid-integration-tool) tool to create more realistic profiles. Profiles from other research institutes should also be considered.

**This step is vital, and considerable time should be spent on trying to get a good demand curve!**

## Fuels

Fuels can be modelled at either a regional or national level, depending on the country and the difference in sources of fuels (e.g. pipeline gas vs. LNG) and prices.

These should be based on either IEA forecasts (experts in GCP like Carlos for coal or Gergely for gas should be consulted!) or data directly from countries.

## Reserves

Reserves are modelled at either a regional or national level, depending on how they are held and procured in the country being modelled.

As a minimum spinning reserves are modelled based on direct information from the countries, which is usually the largest contigency in the system. This may or may not be dynamic, in that the sizes of these reserves changes based on the operation of the system. Yhis can be explicitly modelled in PLEXOS.

Regulating reserves have also been modelled in many countries though this requires a representation of forecast uncertainty in demand and VRE generation. For demand and as a status quo, this has been modelled as 3% of demand. For systems with higher shares of VRE, there have been attempts to model forecast uncertainty based on two methodologies from NREL, though it has not succesfully been ported over to the gis-script tool.

For solar forecasts, this was based on methodology from https://www.nrel.gov/docs/fy16osti/64472.pdf

For the wind forecast methodology, this based on persistence model as detailed in https://www.nrel.gov/docs/fy16osti/64472.pd

The unfinished attempt to port this over can be found at the [bottom of a development Jupyter Notebook used for Ukraine](https://gitlab.iea.org/iea/ems/rise/gis-script/-/blob/main/examples/archived/06_UKR_VRE_inputs_for_PLEXOS.ipynb?ref_type=heads). This requires some level of debugging, however, to find the source of errors which lead to very high values that don't correspond to the expected results.