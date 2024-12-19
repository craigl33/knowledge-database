# **Model guide**

This section aims to provide introductory information mainly for modellers to get started, but can also be useful for all team members working on projects with modelling to understand the setup and where to find things. The section includes:
- Getting started page with how to get connected to all the modelling resources including code in GitLab, mapping information for all the drives we use for modelling and data storage, and a list of locations that might be needed/useful.
- Model overview page with overall information on the modelling approach, types of models we have and the drive structure of the modelling folders
- Procedure summaries describing the typical steps for modellers setting up new models, updating existing models and analysing solution files.
- Individual pages for each model providing basic background, where to find published material related to the model, presentations relating to that model and locations of and notes on past models with locations of some of the main data sheets and folders, including most importantly a description of the most recent update (and any prior updates, particularly if they are the source of current settings in the model).

Note that we have recently updated the architecture of a lot of the scripts related to model processing and setup but in some cases this is not complete, so there is still a combination of legacy system and the new system based on gitlab. As much as possible new work should move to gitlab.
	

## **Getting started**
**Important contacts summary - to update regularly**
- GIT access - RISE gitlab administrator: Isaac Portugal
- IEA IT queries - it.support@iea.org
- PLEXOS queries - support@energyexemplar.com
- WEO
  - Head of PSU for all capacity / generation data: Brent Wanner
  - In copy for capacity / generation: Davide D'Ambrosio
  - Head of demand: Stephanie Bouckaert
  - In copy for demand: Anthony Vautrin


**Installing python**

Most RISE code exists in python, with a few scripts in R and some python
scripts utilising other platforms such as julia and SQL. Not all team
members need to use python, but it can facilitate large queries to the
data warehouse, be used to make maps for reports, and is necessary for
any team members setting up or updating the PLEXOS models, or analysing
solution files from PLEXOS.
To get started you will need to install Anaconda Navigator from the
software centre. If there is not a recent version (at least 2023.09,
Python 3.11.5), you may need to request access to it from IT.
 

**Connecting to gitlab**

The current RISE gitlab administrator who can provide permissions etc.
is: Isaac Portugal

RISE has its own [gitlab page
here](https://gitlab.iea.org/iea/ems/rise). You should connect using the
IEA SSO Registering and Login. The first time you connect you will need
to setup a password that may be needed when you access gitlab through
your scripts such as pull requests.

The gitlab page contains all the code we use for [model
setup](https://gitlab.iea.org/iea/ems/rise/plexos-model-setup) and
[solution file
processing](https://gitlab.iea.org/iea/ems/rise/solution-file-processing)
as well as a [knowledge
database](https://gitlab.iea.org/iea/ems/rise/knowledge-database) with
initial [setup
instructions](https://gitlab.iea.org/iea/ems/rise/knowledge-database/-/wikis/setup/Setup)
to get you started using python and git. The RISE gitlab administrator
will need to give you permissions in order to make changes to the code
on gitlab. Each of the project folders contains a readme that will guide
you through getting that specific project working, which may require
specific installations outside python such as julia.

 

**Using PLEXOS**

RISE currently has 4 PLEXOS licences and is allowed have 4 named PLEXOS
users. This means that only 4 team members can access the PLEXOS GUI and
run models at any one time.
- To change the named users we need to ask IT. The terms of our contract
  also require us to inform Energy Exemplar when we change the named
  users.
- See [IT access VPN, VM,
  PLEXOS](onenote:#IT%20access%20VPN,%20VM,%20PLEXOS&section-id={3A400C3C-8527-48C9-BAE1-A2C9F8CC2AD2}&page-id={57697526-7D8C-4233-B501-0D250D9EBD7E}&end&base-path=https://ieaorg.sharepoint.com/sites/EMS-RISE/SiteAssets/EMS-RISE%20Notebook/Onboarding%20and%20WIKI.one)
  for instructions on accessing PLEXOS once installed, incl through a
  VM.
- We are able to access beginner PLEXOS training run by Energy Exemplar
  (now every month), which runs for 3 days. The modeller who is the main
  point of contact (currently Craig ) should email the case manager
  (currently Charles Manners) to get the training schedule and let them
  know who wants to join. Joining the training allows staff members /
  interns / secondees etc. to gain access to a PLEXOS licence during the
  training.
- We also have materials from past training [saved in the G:
  drive](file:///G:\DOCS\07%20DATA%20AND%20TOOLS\02_PLEXOS\01_basic_introductory_material).
  The material is quite extensive including recordings of the sessions,
  so the main limitation is the lack of a temporary PLEXOS licence and
  the inability to ask questions live.



**Drive mapping**

The modelling team uses a number of additional drives to store data
related to the modelling. The drives are not ideally organised but there
are space limitations to fixing this (it could be improved within these
limitations though). In part the existing structure relates to the fact
that before migrating to PLEXOS Connect we ran models on local machines
and it was more efficient to save the modelling data on the local drive.
Because some of the data is big we cannot currently fit it all in the
same place. It is recommended to map these drives to the designated
letters to facilitate communication and sharing script paths within the
team (go to This PC, Map network drive). Note that some mappings are to
subfolders in the same server drive. This is because some folders were
moved over time but we kept the original mappings to minimise changes to
scripts.


If unable to map any of these drives you may need permission provided by
IT. You can request directly (it.support@iea.org) or if needed a staff
member from the modelling team should be sufficient to confirm you
should be given access.

Overview of the existing drive mappings and their contents.

**S:** Mapping: [\\7920-9ynvwm2-10\data\$](file:///\\7920-9ynvwm2-10\data$)

Contents: modelling folders for China, India, Korea and corporate
procurement

Should not need permission for staff; secondees or interns may need
permission.

**X:** Mapping:
[\\vfiler2\group3\EMS\SOURCEDATA\Geospatial](file:///\\vfiler2\group3\EMS\SOURCEDATA\Geospatial)

Contents: various timeseries weather data (ERA5 data for ASEAN, China,
Indonesia, Thailand, Ukraine), [GIS data](file:///X:\GIS2) for India and
Korea and some other countries for minor analyses, [raw data from the
China analysis](file:///X:\Weather_data_overflow\01_Vaisala_Raw_Data)
done by Vaisala, [wind power curves](file:///X:\wind_power_curves) from
Vaisala

Likely needs permission.

**Y:** Mapping: [\\vfilermc1\EMS\RED](file:///\\vfilermc1\EMS\RED)

Contents: modelling folders for ASEAN, Indonesia, Thailand, Ukraine
(inside \Modelling), GIS data (China, Europe, global, Indonesia, Japan,
Thailand, Ukraine).

Likely needs permission.

**Z:** LEGAGY mapping - should not be needed as it's equivalent to
Y:\Modelling\\ but you may encounter it in some scripts

Mapping:
[\\vfilermc1\EMS\RED\Modelling](file:///\\vfilermc1\EMS\RED\Modelling)

Contents: as above, modelling folders for some countries

Likely needs permission.


**Specific locations to be aware of**

**Y:\General_figures_and_maps\Maps**

Contents: centralised location for code (until there's a gitlab project)
and files related to creating maps for various reports \\ presentations
etc.

**Y:\Python**

Contents: should be being phased out but legacy scripts may be found
here from before the gitlab setup.



 
