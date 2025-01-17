# **Model guide**

[[_TOC_]]

This section aims to provide introductory information mainly for modellers to get started, but can also be useful for all team members working on projects with modelling to understand the setup and where to find things. The section includes:

- Getting started page with how to get connected to all the modelling resources including code in GitLab, mapping information for all the drives we use for modelling and data storage, and a list of locations that might be needed/useful.
- Model overview page with overall information on the modelling approach, types of models we have and the drive structure of the modelling folders
- Procedure summaries describing the typical steps for modellers setting up new models, updating existing models and analysing solution files.
- Individual pages for each model providing basic background, where to find published material related to the model, presentations relating to that model and locations of and notes on past models with locations of some of the main data sheets and folders, including most importantly a description of the most recent update (and any prior updates, particularly if they are the source of current settings in the model).

Note that we have recently updated the architecture of a lot of the scripts related to model processing and setup but in some cases this is not complete, so there is still a combination of legacy system and the new system based on gitlab. As much as possible new work should move to gitlab.

## **Getting started**

##### **Important contacts summary - to update regularly**

- GIT access - RISE gitlab administrator: Isaac Portugal
- IEA IT queries - it.support@iea.org
- PLEXOS queries - support@energyexemplar.com
- WEO
  - Head of PSU for all capacity / generation data: Brent Wanner
  - In copy for capacity / generation: Davide D'Ambrosio
  - Head of demand: Stephanie Bouckaert
  - In copy for demand: Anthony Vautrin

##### **Virtual Private Network (VPN) access**

When not in the office, connect to the VPN to access virtual machines, network drives and internal OECD sites (e.g. Kiosk). BI-IP Edge Client should come installed on IEA laptops. Open it, click Connect and on the window below enter:

1. Username: follows the pattern LASTNAME_F where F is the initial of the user's first name. Not case sensitive
2. Windows password: normal Windows password used to log onto IEA computers
3. RSA Passcode: the passcode shown in the RSA app (ie not your RSA pin). IT Helpdesk knowledge articles for [requesting an RSA token](https://helpdesk.iea.org/helpdesk/KB/View/2326-rsa-securid--request-a-token) and [info on how the RSA app works](https://helpdesk.iea.org/helpdesk/KB/View/679-rsa-securid--modern-token)\
    

##### **Virtual machine (VM)**

A virtual machine needs to be set up/assigned to you by IT. Request from IT either by email (IT.Support@iea.org) or by [IT Helpdesk ticket](https://helpdesk.iea.org/helpdesk/). For people running python scripts to process model results, it is worth asking for 128GB of RAM as the default 16GB is not enough/slow for some operations.

Once assigned, access using the VM Horizon Client software that comes installed on IEA laptops by default. Use the following guide to access the VM: https://helpdesk.iea.org/helpdesk/KB/View/307-remote-access--virtual-machine

 

##### **Installing python**

Most RISE code exists in python, with a few scripts in R and some python scripts utilising other platforms such as julia and SQL. Not all team members need to use python, but it can facilitate large queries to the data warehouse, be used to make maps for reports, and is necessary for any team members setting up or updating the PLEXOS models, or analysing solution files from PLEXOS. To get started you will need to install Anaconda Navigator from the software centre. If there is not a recent version (at least 2023.09, Python 3.11.5), you may need to request access to it from IT.

##### **Using PLEXOS**

RISE currently has 4 PLEXOS licences and is allowed have 4 named PLEXOS users. This means that only 4 team members can access the PLEXOS GUI and run models at any one time.

- To change the named users we need to ask IT. The terms of our contract also require us to inform Energy Exemplar when we change the named users.
- We are able to access beginner PLEXOS training run by Energy Exemplar (now every month), which runs for 3 days. The modeller who is the main point of contact (currently Craig ) should email the case manager (currently Charles Manners) to get the training schedule and let them know who wants to join. Joining the training allows staff members / interns / secondees etc. to gain access to a PLEXOS licence during the training.
- We also have materials from past training saved in the G: drive. The material is quite extensive including recordings of the sessions, so the main limitation is the lack of a temporary PLEXOS licence and the inability to ask questions live.

To access PLEXOS:

1. Download PLEXOS from the Software Centre
2. Request a licence from IT either by email (IT.Support@iea.org) or by [IT Helpdesk ticket](https://helpdesk.iea.org/helpdesk/)
3. Connect to the VPN if not in the office
4. Open PLEXOS and attempt to log in using the authentication as below. You will not be able to log in here but IT apparently needs you to do this before they can see your account in the back end and assign a licence to you
5. Once IT has confirmed the licence has been assigned, you will be able to log in using the server and port below with Windows authentication

| Server: | vplxcon01.ad.iea.org |
|---------|----------------------|
| Port: | 8888 |

##### **Drive mapping**

The modelling team uses a number of additional drives to store data related to the modelling. The drives are not ideally organised but there are space limitations to fixing this (it could be improved within these limitations though). In part the existing structure relates to the fact that before migrating to PLEXOS Connect we ran models on local machines and it was more efficient to save the modelling data on the local drive. Because some of the data is big we cannot currently fit it all in the same place. It is recommended to map these drives to the designated letters to facilitate communication and sharing script paths within the team (go to This PC, Map network drive). Note that some mappings are to subfolders in the same server drive. This is because some folders were moved over time but we kept the original mappings to minimise changes to scripts.

If unable to map any of these drives you may need permission provided by IT. You can request directly (it.support@iea.org) or if needed a staff member from the modelling team should be sufficient to confirm you should be given access.

Overview of the existing drive mappings and their contents.

**S:** Mapping: \\7920-9ynvwm2-10\\data$

Contents: modelling folders for China, India, Korea and corporate procurement

Should not need permission for staff; secondees or interns may need permission.

**X:** Mapping: \\vfiler2\\group3\\EMS\\SOURCEDATA\\Geospatial

Contents: various timeseries weather data (ERA5 data for ASEAN, China, Indonesia, Thailand, Ukraine), GIS data for India and Korea and some other countries for minor analyses, raw data from the China analysis done by Vaisala, wind power curves from Vaisala

Likely needs permission.

**Y:** Mapping: \\vfilermc1\\EMS\\RED

Contents: modelling folders for ASEAN, Indonesia, Thailand, Ukraine (inside \\Modelling), GIS data (China, Europe, global, Indonesia, Japan, Thailand, Ukraine).

Likely needs permission.

**Z:** LEGAGY mapping - should not be needed as it's equivalent to Y:\\Modelling\\ but you may encounter it in some scripts

Mapping: \\vfilermc1\\EMS\\RED\\Modelling

Contents: as above, modelling folders for some countries

Likely needs permission.

##### **Specific locations to be aware of**

**Y:\\General_figures_and_maps\\Maps**

Contents: centralised location for code (until there's a gitlab project) and files related to creating maps for various reports \\ presentations etc.

**Y:\\Python**

Contents: should be being phased out but legacy scripts may be found here from before the gitlab setup.

 