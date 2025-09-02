# LCA ScenarioResults from CO2InnO-H2-ICE-CHP-Demonstrator

This work has been done as part of the [CO2InnO project](https://co2inno.com/), which was financed by the European Union through [Interreg Upper Rhine 2021–2027, A1.3](https://www.interreg-rhin-sup.eu/projet/co2inno-laboratoire-vivant-pour-une-region-dinnovation-pilote-neutre-en-co2-developpement-de-solutions-energetiques-et-de-mobilite/). 

The associated scientific article will be linked as soon as it is published.

## Context

This repository contains LCI and LCIA datasets used in a LCA (Life-Cycle Assessment) study, derived from the outputs of a TEA (Techno-Economic Assessment) `OpenModelica` model which code is also available in Github under the name [CO2InnO-H2-CHP-Demonstrator](https://github.com/IKKUengine/CO2InnO-H2-CHP-Demonstrator). It is used for the study of decentralized energy systems (DES) based on hydrogen use (H₂) and using H₂ in an internal combustion engine for combined heat & power (ICE-CHP). 

The LCI datasets come with a synthetic documentation explaining the determination of each flow of the Life-Cycle Inventory (LCI) for the different scenarios studied. The full and ready-to-use `Brightway2` project can be transmitted on demand, provided you have a valid `ecoinvent v3.10` license. The LCIA datasets are provided with the `lcia_results` Jupyter Notebook used to process them in order to create relevant visualizations of the Life-Cycle Impact Assessment (LCIA) for the LCA study. An alternate version of this repository is hosted on [Zenodo](https://doi.org/10.5281/zenodo.17020672) without modifications. 

## Objective & Methodology

The simulated energy system developed in `OpenModelica` ([Fritzson et al., 2020](https://doi.org/10.4173/mic.2020.4.1)) using a TEA framework [Beerlage et al., 2024](https://doi.org/10.3384/ecp20780 ) was used to generate several quantitative scenarios with hourly resolution of flows over a year. Based on the results outlined, a comparative cradle-to-grave LCA (compliant with ISO 14040/44) was conducted to evaluate the environmental impacts of the DES configurations identified in the TEA study. Mass and energy balances for each scenario were reconstructed from `OpenModelica` outputs using custom `Python` procedures within `Jupyter Notebooks` – see companion `Processing CO2InnO-H2-CHP-Demonstrator outputs` [Github](https://github.com/Paul-Robineau/Processing-CO2InnO-H2-CHP-Demonstrator-outputs) or [Zenodo](https://doi.org/10.5281/zenodo.16919026) repository. The LCA was implemented using the `Brightway2` open-source framework [Mutel, 2018](https://doi.org/10.21105/joss.00236) via the `Activity Browser` GUI [Steubing et al., 2020](https://doi.org/10.1016/j.simpa.2019.100012). The background system was modeled using the `ecoinvent v3.10` database with the “allocation, cut-off by classification” approach. Life cycle impact assessment (LCIA) was initially performed using the EU-recommended `EF v3.1` method, which includes 16 environmental indicators [Andreasi Bassi et al., 2023](https://data.europa.eu/doi/10.2760/798894). For clarity and relevance, five key indicators were selected for detailed analysis – Table below – but this repository contains the data for every indicator. Uncertainty analysis was conducted through Monte Carlo simulations, followed by global sensitivity analysis (GSA) to identify key contributors to result variability.

| Environmental category        | Impact indicator                                        |
|-------------------------------|---------------------------------------------------------|
| Climate change                | Global Warming Potential (GWP100)                       |
| Ecotoxicity (freshwater)      | USEtox / Ecotoxicity: freshwater (ECOTOX-FW)            |
| Human toxicity (carcinogenic) | USEtox / Human toxicity: carcinogenic (HT-C)            |
| Resource consumption          | Abiotic Depletion Potential: ultimate reserves (ADP-UR) |
| Water use                     | User Deprivation Potential (UDP-WU)                     |

## Functional unit

The functional unit, defined as the "quantified performance of a product system used as a reference unit in life cycle assessment," is set as "one year of operation of the H₂ ICE-CHP-based DES, calibrated to meet heat demand." This definition reflects that the system’s decentralized character is primarily related to heat supply, while electricity exchange with the grid remains readily achievable. The annual heat and electricity demand respectively amounted to 1,410 MJ and 297.3 MWh.

## System model boundaries

The investigated system is centered arround the ICE-CHP in a context of heat-lead functioning where electricity exchanges with the grid are possible. It comprises three interconnected subsystems: electrical, thermal, and hydrogen.

Given the system’s ability to exchange electricity with the grid, multiple system boundaries can be defined for impact assessment, each yielding distinct results and interpretations. To ensure a comprehensive evaluation, three boundary scenarios were considered. In the **Load** (L) boundary, only the flows directly meeting the heat and electricity demands of the Offenburg buildings are included. The **Load + Excess** (LE) boundary also accounts for the environmental burden of the electricity overproduction which is not consumed by the buildings. Lastly, the **Load + Excess - Grid(DE)** (LEG) boundary consequently assumes that the excess electricity is exported to the GRID for other uses. It replaces an equivalent amount of electricity from the German electricity mix, thereby potentially crediting the system with avoided emissions.

## Scenario dimensioning

See the companion `Processing CO2InnO-H2-CHP-Demonstrator outputs` [Github](https://github.com/Paul-Robineau/Processing-CO2InnO-H2-CHP-Demonstrator-outputs) or [Zenodo](https://doi.org/10.5281/zenodo.16919026) repository for description of the scenarios.

## Data origin and uncertainties

See companion `Processing CO2InnO-H2-CHP-Demonstrator outputs` [Github](https://github.com/Paul-Robineau/Processing-CO2InnO-H2-CHP-Demonstrator-outputs) or [Zenodo](https://doi.org/10.5281/zenodo.16919026) repository for description of the TEA-related data origin and uncertainties.

Regarding LCA specifically, uncertainty analysis incorporated the existing uncertainty data from the `ecoinvent v3.10` database for the background system. As most ecoinvent processes required scaling to align with the TEA-modeled system components, additional uncertainties were introduced using triangular distributions and pedigree matrix-based methods for modified flows. Monte Carlo simulations were performed with 1,000 iterations for each scenario. It is important to note that the `Activity Browser` does not include uncertainty data for characterization factors (CFs); thus, uncertainty propagation in the results reflects only the background and adapted foreground systems.

## Structure & How to use

The LCI folder contains:

- The LCI-report docx file documenting the building of the Life-Cycle Inventory.
- The different xlsx files provides the LCI of all the subsystems and full scenarios.

However, the direct reuse of these xlsx files in `Brightway2` / `Activity-Browser` is difficult due to the structure adopted: each subsystem is a dedicated database, and the interdependancy of the databases currently prevents their streamlined importation. This approach was originally adopted because of the use of the ecoinvent database, which is not open source. LCA practitionners with a valid `ecoinvent v3.10` license should contact us for sharing of the full and ready-to-use `Brightway2` project.

The LCIA folder contains:

- The various results folders of `xlsx` files for MonteCarlo simulation (MC), Contribution analysis (CA folder), Global Sensitivity Analysis (GSA).
- The `lcia_results` Jupyter Notebook using Python code to process these files and interactively generate relevant visualizations. This notebook has in-depth internal documentation to help new users.
- The pickles folder containing `pkl` files with the different subdatasets used in the whole pipeline. It allows users to only execute some part of the notebook according to their need.

## Requirements

### Python environment manager

Note: the notebooks were tested on `Python 3.11`.

We suggest you to use `Miniconda`:

> Miniconda is a free, miniature installation of Anaconda Distribution that includes only conda, Python, the packages they both depend on, and a small number of other useful packages.

[Miniconda Installation Guide](https://www.anaconda.com/docs/getting-started/miniconda/main)

### Setting up the environment

Within your Python environment manager console (like miniconda console, or anything that suits you):

Create a new Conda environment with all required libraries (in this example named `da` for "data analysis"):

```
conda create  -n da -c conda-forge os pandas numpy tdqm matplotlib seaborn jupyterlab
```
(Add any required library)

Or, create the Conda environment only:

```
conda create -n da -c conda-forge
```

And install the libraries afterwards, after activating the environment:

```
conda activate da
```

```
conda install os pandas numpy tdqm matplotlib seaborn jupyterlab
```
(Add any required library)

Some libraries may require you to use  the `pip` manager instead of conda. In that case, use the command:

```
pip install name_of_library
```

## Use the Jupyter Notebooks

1.  Activate the environment:

```
conda activate da
```

2. Launch Jupyter Lab:

```
jupyter-lab
```
You can select the Notebook you want to use by navigating in the directory where you downloaded the Notebook.

It is thoroughly documented to allow you to understand what does each bloc of code.
