# Charging-Scheduling-Optimization
The model suggests the optimal starting time for electric vehicles to minimize the user's charging cost, considering the Time-of-use tariff scheme

### ⚡Context
Consider a manager of a residential building that wishes to minimize the cost of energy consumption while still meeting the EV charging demand of the tenants. This algorithm will fulfill that goal by determining the optimal time to initiate charging for each individual EV. 

The charging cost is computed based on a _Time-of-Use tariff scheme_, where prices are predetermined and fixed during a period of time. Typically, the tariffs during high-load phases are generally higher compared to low-load periods. Consequently, delaying charging to low-cost hours helps to shift partial electricity consumption to off-peak periods, which _**not only provides economic advantages but also contributes to the stability of grid operations.**_

### 🔌 Process

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
<p align="center"> Table 1: Summary of the simulated EVs’ mobility and charging data.
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/7b29e237-373f-42f1-8c88-ce73815913b4" width=60% height=60%>
</p>

<p align="center"> Figure 1: The assumed price of electricity that captures the load profile of a workplace
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/c2fdae5c-5b4d-448f-b28b-4f1415d85af8" width=60% height=60%>
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
