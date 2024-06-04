# MDLZ Pallet Simulation - General Assumptions

## Simulation Assumptions/Definitions

#### Locations
* Most locations are grouped into buckets for simplicity. Each bucket represents multiple Locations in terms of outbound demand etc. The buckets are as follows:
  * Salinas - All Salinas, MX locations
  * EMs - All External Manufacturer locations
  * Repack - All repack locations
  * RDC - All RDC locations
  * DSD - All 46 DSD branch locations
  * Customers - All Non-DSD customer locations
  * Richmond Bakery - Individual location
  * Chicago Bakery - Individual location
  * Portland Bakery - Individual location
  * Naperville Bakery - Individual location
 
#### Pallet Demand
* Pallet demand and throughput is calculated on a weekly basis.
  * This is calculated by taking the yearly forecasted demand for each location and dividing it evenly by 52 operational weeks.

#### New Pallet Purchases
* Each location in the simulation buys new pallets to cover their weekly demand, unless some/all of their weekly demand is covered by the pallet return program.
* Excluding Salinas locations, ALL locations buy stringer pallets to cover any unmet demand at the start of each week.

#### Transportation Lanes
* Mondelez White Wood Block pallets are ONLY bought in Salinas, MX locations.
* Mondelez White Wood Block pallets are ONLY returned from DSD locations.
* Mondelez White Wood Block pallets can ONLY be returned to the following locations:
  * Richmond
  * Portland
  * Chicago
  * Naperville

#### Turns
* One "turn" for a pallet occurs when the pallet reaches the DSD bucket and becomes available to return. The next turn starts when it is returned upstream through the pallet return program.
* Once a pallet has reached the end of its last turn defined by the turn parameter, it is treated as 100% loss.

#### Pallet Loss to Customers
* Any pallets sent directly to customers or repack are treated as a 100% loss. There is no expectation of return from these locations.

#### Plant Lead Times & Holding Times
* Plant lead times and holding times are uniformly random for each batch of sent pallets between the intervals described below. 

#### Truck Capacity
* One truck can hold up to a maximum of 500 pallets to return.

#### CHEP
* Chep pallet demand is not considered in this simulation in any capacity.

## Data Assumptions
The below parameters are all inputs that can be modified before each simulation run. The default values are as follows:

#### Yearly Outbound Pallet Volume (Demand)
* Salinas - 753K - Send only MDLZ WW Block pallets
* EMs/HFS - 384K/165K - Send only stringer pallets
* Repack - 63K - Send only stringer pallets
* Richmond - 188K - Send stringer pallets until return program , then send MDLZ WW Block pallets
* Chicago - 250K - Send stringer pallets until return program, then send MDLZ WW Block pallets
* Portland - 117K - Send stringer pallets until return program, then send MDLZ WW Block pallets
* Naperville - 182K - Send stringer pallets until return program kicks in, then send MDLZ WW Block pallets

#### New Pallet Purchase Cost
* Salinas - $19
* EMs/HFS - $14
* Repack - $14
* Richmond - $14
* Chicago - $14
* Portland - $14
* Naperville - $14

#### Plant Lead Times
* Salinas -> RDC - 10-14 days
* Salinas -> DSD - 10-14 days
* Salinas -> Customers - 10-14 days

* Bakeries -> RDC - 5-7 days
* Bakeries -> DSD - 5-7 days

* RDC -> DSD - 4-5 days
* RDC -> Customers - 2-4 days

* DSD -> Customers - 2 days

* Repack -> DSD - 5-7 days
* Repack -> Customers - 5-7 days

#### Plant Holding Times
* Salinas - 7-10 days
* Bakeries - 7-8 days
* Repack/HFS - 5-7 days
* RDCs - 14-28 days
* DSD - 7-10 days

### RDC Distribution Percentages
* RDC -> DSD - 54%
* RDC -> Customers - 38%
* RDC -> Repack - 8%

#### Pallet Return Lead Times
* DSD -> Salinas - 14-21 days
* DSD -> Bakeries - 10-15 days
* DSD -> Repack - 7-10 days
* DSD -> EMs/HFS - 10-14 days

#### Plant Return Priority
* Richmond -> Portland -> Chicago -> Naperville

#### Pallet Return Cost
*One full truck = 500 pallets*
* Bakeries - $2100/truck
* HFS - $2100/truck
* Salinas - $6000/truck

#### Pallet Recyclability
* 3-5 turns

#### Pallet Loss Rate
* 6%/Turn

#### Pallet Damage Rates
* Turn 1 - 4%
* Turn 2 - 8%
* Turn 3 - 16%
* Turn 4 - 32%
* Turn 5 - 64%




