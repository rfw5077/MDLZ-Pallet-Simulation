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
The pallet simulation model uses the SimPy discrete event simulation framework to simulate pallet flow through the NA MSC network. In our current state, we use a combination of generic whitewood pallets and CHEP pallets to supply our NA MSC network. The logistics operations team are investing new, more stable white wood pallets to replace the current flow of generic white wood pallets throughout the network. These new pallets have the ability to be recycled through the network between 3-5 times, giving Mondelez the opportunity to return them throughout the network in lieu of continuously sourcing new pallets from suppliers. This simulation serves as the guide for injecting these new pallets into the network and following them through the pallet return program over time.

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
* Customers (non-DSD) - *one node for all customers*
* Club customers - *one node for all customers*


*Depiction of directional pallet flow in pallet simulation*
<img src = "MSC Network Future State.PNG">

**Process:** <br>
Below is the general flow of how the simulation works:

Initially, each of the upstream nodes are injected with pallets. Since the simulation runs on a weekly basis, this means each upstream node holds a weeks worth of outbound pallet demand when the simulation begins.

Every week, the simulation sends out a weeks worth of pallet demand out of upstream nodes and transfers them to downstream nodes (i.e from Mexico to RDCs). Downstream nodes then send pallets to other downstream nodes (i.e from RDCs to DSD branches). All of these pallet movements happen concurrently as the simulation continues to run, week by week.

As the simulation continues to run, pallet surplus starts to build in the DSD branch locations. At weekly intervals, the simulation checks to see if there is any pallet surplus that can be returned to upstream nodes (US Plants). This surplus is then distributed to upstream nodes and returned via the pallet return program. Upstream nodes ONLY buy new pallets from suppliers every week if the pallet return program can't cover their weekly outbound pallet demand.

The simulation runs out 3 years, and tracks daily information for each upstream node including pallet counts, pallet needs and pallet returns.


## Simulation Technical Details
The SimPy framework has many nuances not detailed in this section. Please reference the docs above for clarification.

The pallet simulation uses six main features of the SimPy framework: Environmnets, Classes, Processes, Resources, Containers * Timeouts.

*Environmnets*
* A SimPy environment is a latent control mechanism in which the entire simulation runs. The environment tracks everything related to the simulation including processes, resources and containers. It also controls how processes are triggered and executed over time.

*Classes*
* Classes are used to define entities within the simulation. In this case, our defined classes represent upstream nodes. These are true python classes. As such, they contain attributes and functions that help the situation run.

*Processes*
* Processes are python functions that are executed throughout the simulation. These functions are the executables that move pallets from node to node. This includes all movements related to outbound pallets and inbound return pallets.

*Resources*
* Resources are variables defined within our classes that allow the simulation to interact with the class' attributes. Once a resource is requested, processes run to give or take pallets out of a respective node's class based on the processes that are defined.

*Containers*
* Containers are an attribute of the upstream nodes. These are latent buckets that hold a specific amount of pallets. These are the buckets that pallets are taken out of and put into as the simulation runs and processes interact with the environment.

*Timeouts*
* Timeouts are python generator functions that delay the simulation for a specified period of time. In the pallet simulation model, we use timeouts to simulate pallet in transit times, holding times etc. Timeouts DO NOT stop the entire simulation from running, but they hault the process in which they are defined for the specified period of time.

**Process:**
The SimPy framework uses two methods to move pallets throughout the network via interacting with the upstream node's container objects. These methods are *put* and *get*.

A *get* method takes a specified number of pallets out of a container, while a *put* method puts a specified amount of pallets into a container.

To start, all upstream nodes are initialized with a week's worth of pallet demand. When the simulation starts, all upstream nodes start to send pallets out to downstream nodes through the following functions:

**US Plants**
*richmond_to_dsd* - Process that sends pallets directly from Richmond bakery to dsd locations
*richmond_to_rdc* - Process that sends pallets from Richmond bakery to RDC locations

*portland_to_dsd* - Process that sends pallets directly from Portland bakery to dsd locations
*portland_to_rdc* - Process that sends pallets from Portland bakery to RDC locations

*chicago_to_dsd* - Process that sends pallets directly from Chicago bakery to dsd locations
*chicago_to_rdc* - Process that sends pallets from Chicago bakery to RDC locations

*naperville_to_rdc* - Process that sends pallets from Naperville bakery to RDC locations

**Mexico**
*mexico_to_dsd* - Process that sends pallets directly from Mexico to DSD locations
*mexico_to_rdc* - Process that sends pallets from Mexico to RDC locations
*mexico_to_customers* - Process that sends pallets directly from Mexico to non DSD customer locations

**External Manufacturers (EMs)**
*em_to_rdc* - Process that sends pallets from EMs to RDC locations

**Repack**
*repack_to_dsd* - Process that sends pallets directly from Repack to DSD locations
*repack_to_customers* - Process that sends pallets directly from Repack to non DSD customer locations

All of these processes run concurrently every 7 days, moving pallets in and out of nodes in the network as they flow from upstream nodes to downstream nodes. Throughout this process, pallets are tracked based on which recycled turn they are on, accounting for damage and pallet loss parameters that are set in the simulation.

Within these processes is logic that tracks pallet need at each location and determines whether or not weekly pallet demand can be covered by the pallet return process or if the location must source pallets from a supplier to cover their demand. 

These return processes are run every 7 days, offset from outbound processes by 4 days (i.e if the outbound processes run on day 21, return processes are run on day 25). The return process check the pallets avaialable to return buckets to see how much of each location's pallet need can be covered through the pallet return process.
* **IMPORTANT NOTE:** This specific model has been tuned with the following parameters
  * Mexico will ALWAYS source their weekly demand from suppliers. These pallets will eventually become part of the pallet return program for US Plant's use.
  * Pallets that are sent from EMs & Repack are NOT eligible for the pallet return program
  * US Plants are the only upstream nodes participating in the pallet exchange program. Any and all pallets that are eligible to return will be sent to these upstream nodes only.

There is one return process for each US Plant's upstream node:
*dsd_return_richmond* - Process that returns excess pallets in DSD locations back to Richmond Bakery
*dsd_return_portland* - Process that returns excess pallets in DSD locations back to Portland Bakery
*dsd_return_chicago* - Process that returns excess pallets in DSD locations back to Chicago Bakery
*dsd_return_naperville* - Process that returns excess pallets in DSD locations back to Naperville Bakery

## Data
All input data for the production model was sourced from the project sponsor. Historical data was used to provide estimates for 2024 throughput. This data can be found in the file below:

Pallet Simulation Data.xlsx
* Directional yearly pallet throughput for each node in the simulation
* Lead times between nodes in the network
* Holding times due to processing, customs, etc. at each node
* Costs associated with each leg of the pallet return process
* Pallet damage/loss rates by turn
* Truck fill capacity
