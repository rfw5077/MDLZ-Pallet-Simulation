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
The pallet simulation model uses the SimPy discrete event simulation framework to simulate pallet flow through the NA MSC network. In our current state, we use a combination of generic whitewood pallets and CHEP pallets to supply our NA MSC network. The logistics operations team are investing new, more stable white wood pallets to replace the current flow of generic white wood pallets throughout the network. These new pallets have the ability to be recycled through the network between 3-5 times, giving Mondelez the opportunity to return them throughout the network in lieu of continuously sourcing new pallets from suppliers. This simulation serves as the guide for injecting these new pallets into the network and following them through the pallet return program.

The insights gleaned from this simulation include:
* Where, when and how many pallets will need to be sourced from suppliers in order to meet pallet demand.
* Costs associated with the pallet return program including calculation of number of trucks needed.
* Network pallet health at any location at any point in time.
* Flagging pallet surplus throughout the network at any point in time.
* Tracking pallet attrition via damage and loss to customers.

The network is defined as follows:

**Nodes:**
The nodes in this simulation are split into two categories: Upstream and Downstream.

*Upstream* <br>
Upstream facilities represent sites that will be the source of both pallet injection and pallet return. These sites include bakeries, external manufacturers & repack facilities.

* US Plants - US bakeries
  * Richmond, VA
  * Portland, WA
  * Chicago, IL
  * Naperville, IL

* Mexico Hub
  * Salinas, MX facilities - *one node for all Salinas facilities*

* External Manufacturers - *one node for all external manufacturers*

* Repack Facilites - *one node for all repack facilities*

*Downstream* <br>
Downstream facilities represent sites that will accept pallets from upstream facilities and/or distrubte pallets throughout the network. These sites include RDCs, DSD branch locations & customer locations. 

* US DSD Branches - *one node for all branches*
* US RDCs - *one node for all RDCs*
* DSD customers - *one node for all customers*
* Club customers - *one node for all customers*


<img src = "MSC Network Future State.PNG">

**Process:**
The technical details of the discrete event simulation will be detailed in another section. The process of the simulation is as follows:


**Technical Details:**
Though this model is flexible, the following parameters have been set in the production model:
* New pallets can turn through the network a maximum of 3 times before being destroyed
* The only sites that will receive pallets in the return program are the US Plants - Richmond, Portland, Chicago & Naperville. All other upstream faciilites will continuously source pallet need directly from suppliers.
* Lead times & Holding times are stochastic between specific boundaries - detailed below.
* Pallet Return costs are fixed - detailed below.
* Pallet loss rate is fixed at 6%
* Pallet damage rate is exponentially distributed by turn, starting at 4%
* Pallet injection costs are fixed - detailed below.

## Data
All input data for the production model was sourced from the project sponsor. Historical data was used to provide estimates for 2024 throughput. This data can be found in the file below:

Pallet Simulation Data.xlsx
* Directional yearly pallet throughput for each node in the simulation
* Lead times between nodes in the network
* Holding times due to processing, customs, etc. at each node
* Costs associated with each leg of the pallet return process
* Pallet damage/loss rates by turn
* Truck fill capacity
