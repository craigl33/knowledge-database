**Contents**

[TOC]

## **Intro**
This page aims to provide procedures for the main activities in relation
to the models including processing solution files and setting up and
updating PLEXOS models. When we set up both new models and new versions
that are updates of previous models, we create new folders but use files
from previous models as a starting point. This section refers in many
places to the 'base model': when updating an existing model, usually the
most recent files from the same model are used, but when setting up a
new model it is best to discuss in the modelling team and decide which
is the most up to date and general model to use as a template. Some
model updates may be a 'branch' that will not be continued into future
models if it contains very different elements that are not relevant to
future work and introduces unwanted complexity into the base model (e.g.
corporate procurement).

 


## **General procedures for updating and setting up models**

Note: some of these steps and procedures could be improved with
continued development of the gitlab architecture but this reflects the
current state in April 2024 and should be modified in future whenever
updates are completed.
- Create a new folder in the relevant country folder in Y:\Modelling or
  S:\\ (or a new one) then create inside it the folder structure
  according to the [modelling folder
  structure](onenote:#Modelling%20overview&section-id={3A400C3C-8527-48C9-BAE1-A2C9F8CC2AD2}&page-id={F550C275-BD42-4B6D-B343-CC6E9DB488B5}&object-id={D4531B9D-57BF-0F64-0114-E7B75118A135}&83&base-path=https://ieaorg.sharepoint.com/sites/EMS-RISE/SiteAssets/EMS-RISE%20Notebook/Onboarding%20and%20WIKI.one)
  section.
- Copy the base model file to use as a basis into the 03_Modelling
  folder and give it a new name
- Create the '01_InputData' folder inside the 03_Modelling folder to
  eventually contain the PLEXOS input files
- Start collecting the main inputs. If WEO-based these need to be
  requested from WEO (e.g. Brent + Davide in copy currently) well in
  advance. Note that we should make sure that the unit head or at a
  minimum the project lead has already informed WEO about the project
  (and they didn't raise objections) prior to making data requests
  (suggest that the unit head informs Laura Cozzi and Brent well in
  advance of actually working on the model so that they can raise any
  concerns such as potential overlaps or clashes with WEO projects
  before timelines and deliverables are fixed). Once you have input
  data:

  - Generation capacity
    - If a WEO model, use the model setup code to create the plant list
      with capacities from the DW. If a non-WEO model:
      - New: create the list yourself from the data you have using the
        template in the model setup gitlab project. You will need to
        start setting up the TOML file for the project, which also has
        templates in the gitlab project folder, and a run script.
      - Update: locate the pre-existing list, make a new copy and update
        capacities/plant names as needed. If new plants, you will need
        to create them in PLEXOS as well.
    - Copy the generator parameters sheet from the base model
      - New: replace the plant lists in the Overview and Capacity tabs
        with the plants from the new model, including their
        classifications
      - Update: add any new plants to the Overview and Capacity tabs.
    - Paste the table of plants with capacities into the Capacity input
      sheet in generator parameters.
    - Check the individual parameters tabs and replace any values for
      which you have region specific data or where you think it's
      appropriate to change the assumptions for your analysis (may stay
      the same for some model updates).
    - Check in the Overview/Capacity tabs that values are present and
      look reasonable for each property for all technologies that should
      have that property.
    - (hopefully to be updated) Copy the R script for creating
      parameters from another project to the 07_Scripts/R folder, update
      the paths and run to create the PLEXOS input files.
    - Go into PLEXOS and modify the plant objects in the model to match
      your plant list. A number of tricks can help with this
      - For a WEO model where you just need to change the number of
        regions and their names, you can trim the model down to a single
        node by deleting all generators, batteries, purchasers from all
        but one node. Then you can export the relevant objects from the
        model to an xml (you need to avoid exporting nodal objects where
        you didn't delete the other node versions or you will get errors
        on import due to duplicate IDs) and make a copy for each new
        node you need to create then use find replace in Notepad++ to
        change all references to the node name. You can then import all
        the nodes back into the model.

      - For cases where you need to create totally new plant names
        - You can copy an existing plant inside PLEXOS with the right
          technology type and rename it
        - You can also export a plant with the right technology type to
          XML and make copies / re-import in the .XML then re-import
        - Make sure that the names inside PLEXOS are exactly matched
          with the names in the Capacity and Overview sheets in
          generator parameters and the solution files index (auto
          generation of the solution file index in the model setup
          gitlab code can take care of this step).
      - If you need to add properties to a lot of plants, you can create
        the lists of properties in Excel and just paste them in bulk.
        The pasting can be a little fiddly so save first but can save
        time and Craig created some scripts that can facilitate it
        \[Craig to add link\].

  - Load
    - Make a copy of the combined load inputs Excel from the base model.
      These contain 3 sheets per scenario which are read by the script:
      - Hourly demand tab by end use \[{scenario_year}\]. For WEO models
        all the end uses will be present and you can copy in the new
        inputs keeping the format consistent (column and row placement).
        For non-WEO models you can paste the load to a single end use to
        keep the format and 0 everything else, or if you have some end
        use that is flexible, e.g. electric vehicles, separate it into
        the most appropriate column.
      - Demand response index \[DSM_index\_{scenario_year}\]. For WEO
        models this stores the flexibility assumptions and is read by
        the script to create the constraint files. It needs some extra
        columns not present in the WEO sheet which will be present in
        past / template versions. Note that the end uses and demand
        response assumptions were recently updated (2024) and this needs
        to be updated in new models.
      - Regional splitting factors \[RegionalFactors{year}\] (usually
        does not differ by scenario). This defines the split among the
        model regions by end use. Depending on the data available it
        could be the same for all end uses or could include different
        splits by sector or end use.
    - Once the combined inputs Excel is set up, the demand inputs for
      the model can be created using the [model setup
      project](https://gitlab.iea.org/iea/ems/rise/plexos-model-setup)
      in gitlab
      - Pull the project if you didn’t already and create an environment
        so you can use it
      - Create the TOML file if you didn't already and ensure the paths
        and settings related to demand are up to date. Run the demand
        setup component in the run script (create if you didn’t
        already).

  - Transmission
    - For minor model updates not focused on transmission we would often
      not change this
    - For new models or more extensive updates:
      - it is necessary to collect effective transmission capacity
        between the model regions. We effectively use only transfer
        capacities and resistance as including impedance/AC modelling
        has led to a lot of issues with flows that are not necessarily
        meaningful since we lack a detailed representation of all the
        transmission levels.
      - In most models these are represented as individual lines in the
        model, for which you should prepare a list in a standardised
        format. Any new lines need to be added in PLEXOS, which can be
        done by copying old lines and ensuring the from and to nodes are
        correct
      - The capacities and resistance are designated in PLEXOS through a
        data file in this format which is currently being prepared
        manually.

 

- VRE generation inputs
  - Note that WEO is creating more detailed VRE generation curves than
    they did in the past so for future WEO-based projects we may be able
    to use the WEO curves for greater consistency.
  - In cases of minor model updates we would typically inherit these
    from the previous project, we typically only create this for:
    - New projects
    - Cases where the old analysis was suboptimal and we want to upgrade
      to a new setup
    - Cases where we want to add multi-year weather data to a project
      that has so far only used a single year
  - If you do need to update the weather data, there is a [gitlab
    project \[still in
    development\]](https://gitlab.iea.org/iea/ems/rise/gis-script) which
    includes:
    - Geospatial analysis and site selection to determine VRE siting
    - Weather data analysis to produce generation profiles by site
    - Aggregation of profiles to region and technology and writing to
      the model input format


## **Processing PLEXOS solution files**

You should start by pulling the solution files [processing project
folder](https://gitlab.iea.org/iea/ems/rise/solution-file-processing)
from gitlab and following the readme instructions to get it working
\[note: at least one team member had big issues and could not get the
julia component working. If the
[troubleshooting](https://gitlab.iea.org/iea/ems/rise/solution-file-processing/-/blob/main/docs/Documentation.md#troubleshooting)
in the documentation doesn't fix all issues it may be necessary to
involve IT, or as a workaround have another team member with working
code do the initial processing step\].

You will need to have the solution files for analysis saved in a folder
inside 03_SolutionFIles and then you can use the run script to create
outputs inside 06_DataProcessing.

 