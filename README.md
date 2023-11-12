# Charging-Scheduling-Optimization
The model suggests the optimal starting time for electric vehicles to minimize the user's charging cost, considering the Time-of-use tariff scheme

### ⚡Context
#### Problem & Constraint
A workplace building manager grapples with a rising demand for electric car charging. There are two pressing concerns. First, if the cars commence charging immediately and simultaneously at the highest charging speed upon their arrival, it would cause a sudden heightened load and risk the electrical system stability. Second, if they start charging during peak hours, the energy cost would be unnecessarily expensive. 
So her goal is to devise a charging strategy that minimizes energy costs and alleviates the electrical system burden, with the main constraint being to meet energy demand before car departure.
#### Settings
 **- **Electricity prices:**** The electricity prices during high-load phases are generally higher compared to low-load periods. Here, the project addresses the Time-of-Use tariff scheme, where electricity prices vary throughout the day but are predetermined.
 
**- **Charging speed**:** The charging system is built-in with 3 charging speeds, namely, slow, medium, and fast charging. 

**- Charging behaviors:** The vehicles stay longer than the duration needed for charging, which is a typical charging pattern found in residential or workplace buildings
#### Strategy
The strategy is straightforwards: delay charging to low-cost hours whenever the cars stay longer than the required charging duration 

To achieve that, the algorithm is designed to follow two key principles regarding the selection of charging speed and charging time. (1) Prioritize slower charge rate whenever feasible to reduce unnecessary pressure on the power grid and then (2) start the charging session at the first interval of the scenario that incurs the minimum cost. If the charging request cannot be fully satisfied within the time limit despite applying the highest charging speed, the algorithm will notify the vehicle's user.

This strategy_**not only provides economic advantages but also contributes to the stability of grid operations.**_

### 🔌 Process
<p align="center"> 
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/70ac913c-a883-4774-8119-c6a5cce5dc31" width=40% height=40%>
</p>


**Step 1**: The charging control process starts by loading EVs’ data which includes the arrival $(a_{j})$ and departure time $(d_{j})$, the charging length $(l_{j})$, and the charging power $(r_{j})$  into its system

<p align="center">$EV_{j}(a_{j},d_{j},l_{j},r_{j})$</p>

**Step 2**: After that, the algorithm loads the ToU tariff from the energy service supplier. The tariff should include 24 prices which corresponds to 24 time blocks of a day. For example:
<p align="center">$tariff = [10, 10, 10, 10, 10, 10, 21, 21, 14, 14, 14, 14, 14, 14, 14, 14, 21, 21, 21, 14, 14, 10, 10, 10]$
</p>


**Step 3**: Find the number of possible charging scenarios $(scen_{j})$. If $scen_{j}=1$, the charging process must initiate immediately. If $scen_{j}=2$, the EV can start charging as soon as it arrives or delays by 1 hour.
<p align="center">$scen_{j} = (d_{j} - a_{j} - l_{j} + 1) \quad \text{where } d_{j} > a_{j}$</p>

<p align="center">$scen_{j} = (24 + d_{j} - a_{j} - l_{j} + 1) \quad \text{where } d_{j} < a_{j}$</p>

**Step 4:** Calculate the charging cost for each scenario.
Determine the starting $(st_{j}^{i})$ and respective ending charging time $(et_{j}^{i})$ to retrieve the corresponding tariff set for the charging cost calculation 

$\quad \text{For i to } scen_{j} :$
<p align="center">$st_{j}^{i} = a_{j} + i - 1$</p>

<p align="center">$et_{j}^{i} = a_{j} + i - 1 + l_{j} \text{ where } a_{j} + i - 1 + l_{j} <= 24 $</p>
<p align="center">$et_{j}^{i} = a_{j} + i - 1 + l_{j} - 24 \text{ where } a_{j} + i - 1 + l_{j} >= 24 $</p>
  
Calculate the charging cost for each scenario

$\quad \text{For i to } scen_{j} :$

$\quad \text{Where } et_{j}^{i} > st_{j}^{i}:$

<p align="center">$$cost_{j}^{i} = \sum_{h=st_{j}^{i}+1}^{et_{j}^{i}} r_{j} \times C^{h}$$</p>

$\quad \text{Where } et_{j}^{i} < st_{j}^{i}:$
<p align="center">$$cost_{j}^{i} = \sum_{h=st_{j}^{i}+1}^{24} r_{j} \times C^{h} + \sum_{h=1}^{et_{j}^{i}} r_{j} \times C^{h}$$</p>

**Step 5:** Find the scenario that incurs the minimum charging cost
<p align="center">$\quad \text{min } cost_{j}^{i}$</p>

**Step 6:** Assign the optimal charging starting time, ending time and report its charging cost
<p align="center">$opt_{j}^{i} (st_{j}^{i}, et_{j}^{i},\text{min } cost_{j}^{i})$</p>

### 🚨 Outcome
#### Simulation setup
We evaluate the performance of the algorithm using a set of 6 simulated charging requirements (table below). System build-in charging rate include slow (3kW), medium (7kW), and fast charging (11kW). The charging process is assumed to be uninterrupted at a constant charge rate. 

<p align="center"> Table 1: Summary of the simulated EVs’ mobility and charging data.
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/893f3cdc-5cc0-4349-9748-ceef609381c0" width=60% height=60%>
</p>

<p align="center"> Figure 1: The assumed price of electricity that captures the load profile of a workplace
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/bccb6aa6-7e31-4222-925d-26ab64c11e46" width=60% height=60%>
</p>


#### Result Summary
<p align="center"> Table 2: Cost comparison between controlled and uncontrolled charging
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/d3d33985-96bf-4cec-9964-90696722e8a5" width=60% height=60%>
</p>

_Same day charging_

In uncoordinated charging, the first vehicle starts charging immediately after it is connected to the charging portal and subsequently occupies the first four time blocks during peak-hours, which are the costliest period in the day. However, since EV1 remains connected to the charging station for a duration longer than necessary for charging, the algorithm schedules the charging to shoulder and off-peak hours. The EV1 begins charging from 17:00 to 21:00 at the charging rate of 3kW to optimize the charging cost at 15EUR, reducing charging expense by roughly 30% compared to that in uncontrolled scenario.

_Overnight charging_

Likewise, EV4  arrives at the charging station at 7:00 and departs at 6:00 on the next day. The vehicle stays plug-in for 23 hours and requires 10 hours of charging at slow charge mode. The algorithm leverages the flexibility by shifting the charging process to off-peak hours between 19:00 and 05:00 the next morning to reduce the charging cost from 52.2EUR to 30EUR 

### 💡 Key learnings
- Handling an end-to-end optimization process, including problem formulation, algorithm development and simulation to test 
the algorithm’s accuracy and efficiency
- Programing code: Python

###  Reference
[[1]](https://ieeexplore.ieee.org/document/8821960) S. M. S., I. A. T. P. and D. D., "Optimized Charge Scheduling of Plug-In Electric Vehicles using Modified Placement Algorithm," 2019 International Conference on Computer Communication and Informatics (ICCCI), Coimbatore, India, 2019, pp. 1-5, doi: 10.1109/ICCCI.2019.8821960.
