# Model Guide and Documentation

## Table of Contents

1. Introduction
2. Getting Started
3. Model Setup and Procedures
4. Technical Model Components
5. Model Analysis and Processing

## 1. Introduction

This documentation provides comprehensive information for modellers and team members working on projects with modelling components. It covers setup procedures, resource locations, model architecture, and technical details of model components.

## 2. Getting Started

### 2.1 Important Contacts

* **GIT Access**: RISE gitlab administrator - Isaac Portugal
* **IEA IT Support**: it.support@iea.org
* **PLEXOS Support**: support@energyexemplar.com
* **WEO Contacts**:
  * Head of PSU for capacity/generation: Brent Wanner
  * In copy for capacity/generation: Davide D'Ambrosio
  * Head of demand: Stephanie Bouckaert
  * In copy for demand: Anthony Vautrin

### 2.2 VPN Access

When working remotely, connect using BI-IP Edge Client:

* Username format: LASTNAME_F (first initial)
* Windows password: Standard IEA login
* RSA Passcode: From RSA app (not PIN)

### 2.3 Virtual Machine Setup

* Request VM from IT.Support@iea.org
* Recommended specs: 128GB RAM for python operations
* Access via VM Horizon Client
* Default installation includes 16GB RAM; request more if needed

### 2.4 PLEXOS Access

Current status:

* 4 PLEXOS licenses available
* 4 named users allowed simultaneously
* License changes require IT coordination
* Training available monthly (3-day program)
* Previous training materials stored on G: drive

Setup process:

1. Download from Software Centre
2. Request license from IT
3. Connect to VPN if remote
4. Server: vplxcon01.ad.iea.org
5. Port: 8888
6. Use Windows authentication

### 2.5 Drive Mapping

Network drives used for modelling data:

**S: Drive**

* Mapping: \\7920-9ynvwm2-10\\data$
* Contents: China, India, Korea, corporate procurement models

**X: Drive**

* Mapping: \\vfiler2\\group3\\EMS\\SOURCEDATA\\Geospatial
* Contents: Weather data (ERA5), GIS data, Vaisala analysis

**Y: Drive**

* Mapping: \\vfilermc1\\EMS\\RED
* Contents: ASEAN, Indonesia, Thailand, Ukraine models
* Important subfolders:
  * General_figures_and_maps/Maps
  * Python scripts
  * Modelling folders

### 2.6 Python Setup

* Install Anaconda Navigator (minimum version 2023.09, Python 3.11.5)
* Request access from IT if newer version needed
* Primary language for RISE code
* Required for PLEXOS model setup and analysis

## 3. Model Setup and Procedures

### 3.1 General Setup Steps

1. Create new folder in country directory (Y:\\Modelling or S:)
2. Copy base model file to 03_Modelling folder
3. Create 01_InputData folder inside 03_Modelling
4. Collect main inputs (coordinate with WEO team if applicable)

### 3.2 Generation Setup

1. **Capacity Data Collection**:
   * WEO models: Use model setup code with DW data
   * Non-WEO models: Create/update plant list using template
2. **Generator Parameters**:
   * Copy base model parameters sheet
   * Update plant lists in Overview and Capacity tabs
   * Review individual parameter tabs
   * Verify property values
3. **PLEXOS Configuration**:
   * Modify plant objects to match plant list
   * Use XML export/import for bulk changes
   * Ensure name consistency across files

### 3.3 Load Configuration

1. Copy combined load inputs Excel from base model
2. Update three key sheets:
   * Hourly demand by end use
   * Demand response index
   * Regional splitting factors
3. Use model setup project for demand inputs

### 3.4 Transmission Setup

* Collect effective transmission capacity between regions
* Prepare standardized line list
* Configure capacities and resistance in PLEXOS
* Focus on transfer capacities and resistance

### 3.5 VRE Generation Inputs

* Consider WEO curves for consistency
* Update for:
  * New projects
  * Suboptimal analysis upgrades
  * Multi-year weather data addition
* Use gitlab project for:
  * Geospatial analysis
  * Site selection
  * Weather data analysis
  * Profile aggregation

## 4. Technical Model Components

### 4.1 Generation

* **Data Sources**:
  * Country-level sources
  * WEO projections
  * Regional information for generator assignment
* **Capacity Splitting**:
  * Base on existing regional split
  * Account for retirements
  * Consider new resource distribution
* **Parameters**:
  * Use standard format matching model indices
  * Review technology class properties
  * Update project-specific parameters

### 4.2 Transmission

* **Regional Representation**:
  * Based on control/planning zones
  * Key transmission bottlenecks
  * Pipeline modeling between regions
* **Capacity Calculation**:
  * Use stakeholder information when available
  * Calculate Surge Impedance Loading
  * Consider voltage levels

### 4.3 Demand

* **Regional Modeling**:
  * End-use level detail
  * Demand response capabilities
  * Regional growth factors
* **Profile Creation**:
  * Consider electrification impacts
  * Include transport and heating profiles
  * Account for peak demand forecasts

### 4.4 Fuels

* Regional or national level modeling
* Based on IEA forecasts or country data
* Consider source differences (pipeline vs. LNG)

### 4.5 Reserves

* **Spinning Reserves**:
  * Based on largest contingency
  * Dynamic sizing options
* **Regulating Reserves**:
  * 3% of demand baseline
  * VRE forecast uncertainty
  * NREL methodology integration

## 5. Model Analysis and Processing

### 5.1 Solution File Processing

1. Pull solution files processing project from gitlab
2. Save files in 03_SolutionFiles
3. Use run script for 06_DataProcessing outputs
4. Consider Julia workarounds if needed

### 5.2 Output Analysis

* Follow readme instructions
* Debug processing steps
* Validate results against expectations
* Document processing choices

### 5.3 Data Visualization

* Use established templates
* Consider end-use requirements
* Maintain consistency with previous analyses