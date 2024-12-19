**Contents**

[TOC]




## **General background**

**Modelling approach**

\[to complete: economic dispatch modelling, limited capacity expansion
for some models\]

**Model types**

The team broadly has two types of model setup, depending if the model is
based on WEO inputs and typically more IEA-driven or is a country
collaboration project where we are working closely with partner
institutions in a specific country.

- WEO-style

  - Produced for IEA-driven projects where we want regional modelling to
    support the analysis. E.g. we have often used these models to
    support the WEO including the regional outlooks or special reports
    such as the China net zero roadmap.

  - Based on WEO capacity and demand inputs. No individual
    representation of real plants, uses generic values for plant
    capacity to create generalised plants. As of only 2024, WEO now
    shares capacity through the data warehouse, and there is code in
    gitlab in the [model setup
    project](https://gitlab.iea.org/iea/ems/rise/plexos-model-setup)
    that can read it in and convert it to a plant list.

  - Full demand response representation based on WEO’s different
    flexibility assumptions by end use, allowing part of the demand in
    different end uses to shift by a certain number of hours in either
    direction. The assumptions are provided by WEO for each model and
    another script from the model setup project converts these to PLEXOS
    inputs. However, this (and the model setups in PLEXOS in China,
    India and Indonesia) needs to be updated to accommodate the new WEO
    setup that has more shifting timeframes and slightly different end
    uses.

  - Regional splitting for demand and capacity, and the transmission
    setup, is typically all determined by RISE research or with the help
    of country connections. The WEO team may be including more regional
    analysis in the future so in this case it may be possible to get
    some input into these from WEO as well.

  - This includes the China, India and Indonesia (both types) models.

- Detailed country representation

  - Produced for collaboration projects where we typically work with an
    institution in the country such as the utility (Thailand, Indonesia)
    or a government research institute (Korea). Other models of this
    type include Ukraine and Gujarat.

  - Data comes from public sources and the utility. In most cases we
    complete as much as we can from public sources and receive some
    specific things from the utility that are not publicly available.

  - In this case the individual plants present in the system are
    represented although we often still use some generic assumptions for
    their properties etc., unless more specific data is provided by the
    utility.

  - Demand response representation is trickier in these models because
    we don't have bottom up demand profiles by end-use, so it is often
    limited to EVs or some specific end uses with whatever input
    profiles we can obtain.

 

## **Modelling project folder structures**

We recently (April 2024) harmonised the modelling project folder
structures. Note that this means if you are handling legacy scripts some
paths may need updating. Not all folders need to be present for every
project, but when setting up a project please use these names for the
folders that you do need. If the substructure is specified please also
use the same names for the relevant folders but anything outside of this
can be named freely:

 

**01_Data**

Substructure: examples but this is not standardised - '01_Transmission',
'02_PowerPlants', '03_VRE', '04_DSM', '06_Load', '07_Fuels'

Contents: the underlying data sources that we use to set up the model,
any worksheets we use to process or structure that data, and the
worksheets that are read by scripts to create PLEXOS inputs.

Note: for some models this folder is located on the G: drive to
facilitate access by non-modellers. In these cases, a shortcut to it
named 01_Data should be included in the modelling project folder.

**02_MiscDocsFiguresFiles**

Contents: this is a folder for any extra files you might need to create
including any manually created results analysis, project scope or
organisation information for projects that don't have a separate project
folder in the team drive (G:) and additional analysis

**03_Modelling**

Substructure: '01_InputData'

Contents: the PLEXOS model itself (current and archive versions) as well
as the direct PLEXOS input files

**04_SolutionFiles**

Contents: batches of PLEXOS solution files to be processed together
saved in a subfolder together.

**05_SolutionFilesCache**

Contents: automatically created folder and contents that contains the
cached parquet files containing the data tables used in the [solution
file processing
code](https://gitlab.iea.org/iea/ems/rise/solution-file-processing).

**06_DataProcessing**

Contents: processed results created by the solution file processing
code. These files are automatically created for making charts and
creating other analysis and report inputs. You should not save any
manually created or edited files here - please use
02_MiscDocsFiguresFiles.

**07_Scripts**

Substructure: 'python' and 'R' folders

Contents: May be phased out. Model-specific scripts for creating the
PLEXOS input files from the model (R-based currently) and for solution
file processing (for new models this may be in the gitlab folder
instead).

**09_ModellingSupportFiles**

Contents: May be phased out. Mainly contains the ParametersIndex which
works with the R script for making .csv inputs to PLEXOS and determines
which input files will be created and their names. This is typically
inherited from a similar model and then small adjustments made to add
files where needed.

 

## **Key files for model inputs and setup**

While overall there are many files that vary by model depending on the
input data, we have some particular files that are more or less always
needed. If you are looking for one of these sheets for a past/current
project that is based on a previous project and it is not present, it
may indicate that that particular input is not being updated for the new
project and you may need to look in previous project folders to find the
relevant sheet. For various calculations and data collection you might
need to do for different models, there is no problem to create files
outside of these, they should just at a minimum be stored in the
appropriate folder within 01_Data and ideally be noted with a brief
explanation in the data inventory Excel for that model. Standard sheets
that should be present and consistent with a template (available in the
model setup gitlab folder) are:

- Model configuration TOML files for model setup and solution
  processing. This is part of the new system so does not exist for all
  models but should be included for all models from now. It can be found
  in the git repository folder for the solution file processing / model
  setup after pulling them from gitlab.

- Plants list. For non-WEO models there should be a list of all plants
  by name and their classification into the PLEXOS technologies (this
  has just been updated, you can find the technology index in the model
  setup gitlab folder). It can be found in: \[modelling project
  folder\]\01_Data\02_PowerPlants

- Generator parameters sheet. This is read (currently by an R script,
  hopefully to be updated) to create the PLEXOS input files. Contains
  all the operating properties etc. of the generators in separate tabs
  and overview sheets that calculate the plant specific values which are
  then read, formatted and written to csv by the script. It also
  contains regional splitting factors and sub-technology splitting
  factors (e.g. to split WEO aggregate coal or hydro into more detailed
  technologies). The parameters sheet could be simplified if the R
  script is moved to python/gitlab and the calculations transferred to
  the code for more transparency. It can be found in: \[modelling
  project folder\]\01_Data\02_PowerPlants

- Combined demand inputs sheet. This is read by a python script to
  create the PLEXOS demand inputs including the regional timeseries
  profiles and the suite of files needed to make the PLEXOS demand
  response constraints work. It contains the complete load profile as a
  timeseries (by end use in the case of a WEO model), regional splitting
  factors, and the demand response index which contains the WEO
  assumptions for that model. In non-WEO models, the same format can be
  used and values entered only into one end use with the demand response
  index values set to 0. If there is limited demand response, the
  flexible demand should be placed in separate column(s) and the demand
  response index used to control the duration and depth of flexibility.
  It can be found in: \[modelling project folder\]\01_Data\06_load

 

## **Analysing and presenting modelling results**

**Things to watch out for when presenting and publishing results related
to WEO**

We need to be particularly careful when presenting results from
WEO-based models:

- **The inputs WEO shares with us include many details that WEO does not
  publish themselves**. From this data, we can typically only publish
  something that has already been published by WEO in the free materials
  (not extended materials behind a paywall) and for anything beyond that
  we need to get permission from the WEO team to do it (which often may
  not be possible).

- **We cannot present any result for the WEO scenarios that conflicts
  with the numbers WEO publishes**. Typically, for example, our
  generation totals would not completely align with those of WEO, even
  though we use the same capacity and fuel price inputs. We should
  carefully align the inputs as much as possible (in terms of capacity,
  fuel prices, demand response setup, VRE generation capacity factors
  but not everything as WEO does not have the operational detail that we
  do) but we do not usually try to fully align the outputs to the WEO
  model as this somewhat defeats the purpose of running a more detailed
  dispatch model. Instead, for values that may conflict such as the
  generation totals, we need to present the WEO data and identify in a
  figure or table note that it is derived from the WEO. We can then
  indicate for our own results (such as detailed operational profiles,
  system ramps, etc.) that they come from our regional model (give it a
  name that distinguishes it from the WEO model).

- **The WEO team and particularly the head of PSU (Brent Wanner/Davide)
  and the head of the demand unit (Stéphanie Bouckaert) should have the
  opportunity to review everything we publish based on their inputs**.
  They ultimately are responsible to ensure that the IEA presentation of
  their scenarios and messaging around them is consistent and correct,
  and that only data that is cleared for publication is presented. The
  team are always very busy so in addition to including them in the peer
  review it is best to send them an email clearly identifying the main
  components they would need to check with relevant sections attached
  (if it's not too much text, I would even include the specific
  statements for them to review in the body of the email). Include in
  copy any more junior staff that have been involved in sharing inputs.
  Give a clear date by which they should let you know if they have any
  concerns (ideally give at least a week, it doesn't have to be a super
  final version so around the peer review or at the beginning of the
  edit should be ok). In many cases I have only heard back when there is
  a concern but it's important to still do it every time. Addressing the
  first two bullets in this list should minimise any issues.

## **Solution Files Processing script outputs**

While all PLEXOS results can be directly queried in the PLEXOS GUI, to
facilitate faster analysis we have automated result processing that
creates a standard set of files that can be used to understand and
troubleshoot the model as well as create outputs for reports. This also
facilitates team members without access to PLEXOS to access the
results.  
An important thing to note is that while anyone in RISE is encouraged to
dig into the results where relevant, if planning to use a model result
that has not been previously published it is best to discuss first with
a modeller (ideally the project modeller but otherwise the modelling
lead if there is one or another member of the modelling team) as there
may be results that are not ready for publication or need particular
care or consideration when presenting them. Note that for older models
the outputs will be from the legacy R script and differ somewhat from
the new format but should include in some format the same outputs (and
additional ones since not all functionalities have been transferred to
the new code yet).

 

These are the result folders produced by the processing script, all
found within \[modelling project folder\]\05_DataProcessing\\result set
folder\]:

**plots**: contains regional generation by technology, and hourly
generation stacks for a week surrounding various 'days of interest'
identified by the code (e.g. inertia minimum, minimum and maximum net
load, load minimum and maximum, etc.)

**summary_out**: contains annual results including regional load,
unserved energy, generation by technology and plant, renewables and
variable renewables by region, unit starts, vre capacity and
curtailment, renewable curtailment, line capacities and imports/exports,
installed capacity, capacity factors and emissions.

**timeseries_out**: contains timeseries data (hourly) including load,
unserved energy, generation, net load and duration curves, ramps by
technologies, 3h ramps, outages

 

**General resources**
\[to complete\]

- Data warehouse - see [Data &
  Git](onenote:#Data%20%20Git&section-id={3A400C3C-8527-48C9-BAE1-A2C9F8CC2AD2}&page-id={E964F2CB-953D-45A0-8A20-C242465BC2DD}&end&base-path=https://ieaorg.sharepoint.com/sites/EMS-RISE/SiteAssets/EMS-RISE%20Notebook/Onboarding%20and%20WIKI.one)
- PLATTS database
- WEO extended datasets
- IEA repository for Copernicus data
- Copernicus climate data online

 

## **List of WEO end-use demand sectors**

| **End use**|    **End-use name**                                                      |
|------------|----------------------------------------------------------|
| IND.IS     | Iron and steel                                           |
| IND.CH     | Chemicals                                                |
| IND.NM     | Non-metallic minerals                                    |
| IND.NF     | Non-ferrous metals                                       |
| IND.PP     | Paper, pulp and print                                    |
| IND.NS     | Other industry                                           |
| NEU.OT     | Non energy use                                           |
| OES.ON     | Hydrogen production inputs: onsite refining and industry |
| OES.OF     | Hydrogen production inputs: offsite                      |
| OES.OT     | Other energy sector                                      |
| TRA.DA     | Domestic aviation                                        |
| TRA.WL     | Road 2/3 wheelers                                        |
| TRA.BS     | Road bus                                                 |
| TRA.HT     | Road heavy freight truck                                 |
| TRA.CV     | Road light-commercial vehicles                           |
| TRA.MT     | Road medium freight truck                                |
| TRA.PV     | Road passenger light duty vehicle                        |
| TRA.RA     | Rail                                                     |
| TRA.OT     | Transport                                                |
| TRA.BU     | International bunkers                                    |
| RES.SH     | Residential space heating other                          |
| RES.HP     | Residential space heating heat pumps                     |
| RES.WH     | Residential water heating other                          |
| RES.WP     | Residential water heating heat pumps                     |
| RES.CK     | Residential cooking                                      |
| RES.LI     | Residential lighting                                     |
| RES.RE     | Residential refrigeration                                |
| RES.CA     | Residential cleaning appliances                          |
| RES.BW     | Residential brown appliances                             |
| RES.OT     | Residential other appliances                             |
| RES.CL     | Residential space cooling                                |
| SER.SH     | Services space heating other                             |
| SER.HP     | Services space heating heat pumps                        |
| SER.WH     | Services water heating other                             |
| SER.WP     | Services water heating heat pumps                        |
| SER.CK     | Services cooking                                         |
| SER.LI     | Services lighting                                        |
| SER.AP     | Services total appliances                                |
| SER.CL     | Services space cooling                                   |
| SER.DS     | Services desalination                                    |
| AGR.AG     | Agriculture                                              |