# MDLZ-Pallet-Simulation
Comprehensive simulation of pallet flow through the US CS&amp;L network, including a pallet return program. This initiative was sponsored by Michael Hall in order to simulate and detail the Mondelez network with the use of new and improved pallets that will roll out early 2024.

## Installation
This project was built entirely in the Python programming language. Below are the required dependendices the model needs in order to run properly:

**Editor Used:** VS Code  
**Python Version:** 3.11.4

### Python Packages Used

**Simulation:**
* Simpy 4.0.2 - docs: https://simpy.readthedocs.io/en/4.0.2/

**Data Wrangling/Output:**
* pandas 1.5.3 - docs: https://pandas.pydata.org/pandas-docs/version/1.5.3/getting_started/index.html
* numpy 1.24.3 - docs: https://numpy.org/doc/1.24/

**Calculations:**
* math 3.12.1 - docs: https://docs.python.org/3/library/math.html

**Reproducibility:**
* random 3.12.1 - docs: https://docs.python.org/3/library/random.html

## Simulation Overview
The pallet simulation model uses the SimPy discrete event simulation framework to simulate pallet flow through the NA MSC network. The network is defined as follows:

**Nodes:**
The nodes in this simulation are split into two categories: Upstream and Downstream.

*Upstream*
Upstream facilities represent sites that will be the source of both pallet injection and pallet return. These sites include bakeries, external manufacturers & repack facilities.

* US Plants - US bakeries
  * Richmond, VA
  * Portland, WA
  * Chicago, IL
  * Naperville, IL

* Mexico Hub
  * Salinas, MX facilities

* External Manufacturers

* Repack Facilites

*Downstream*
Downstream facilities represent sites that will accept pallets from upstream facilities and/or distrubte pallets throughout the network. These sites include RDCs, DSD branch locations & customer locations. 

* US DSD Branches - *one node for all branches*
* US RDCs - *one node for all RDCs*
* DSD customers - *one node for all customers*
* Club customers - *one node for all customers*

## Data
All input data for the production model was sourced from the project sponsor. Historical data was used to provide estimates for 2024 throughput. This data can be found in the file below:

Pallet Simulation Data.xlsx
* Directional yearly pallet throughput for each node in the simulation
* Lead times between nodes in the network
* Holding times due to processing, customs, etc. at each node
* Costs associated with each leg of the pallet return process
* Pallet damage/loss rates by turn
* Truck fill capacity
