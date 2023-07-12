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

$\quad \text{Where } d_{j} > a_{j}:$
<p align="center">$st_{j}^{i} = a_{j} + i - 1$</p>
<p align="center">$et_{j}^{i} = a_{j} + i + l_{j} - 1$</p>
<p align="center">$$cost_{j}^{i} = \sum_{h=st_{j}^{i}+1}^{et_{j}^{i}} r_{j} \times C^{h}$$</p>


$\quad \text{Where } d_{j} < a_{j}:$
<p align="center">$st_{j}^{i} = a_{j} + i - 1$</p>
<p align="center">$et_{j}^{i} = a_{j} + i + l_{j} - 1 - 24$</p>
<p align="center">$$cost_{j}^{i} = \sum_{h=st_{j}^{i}+1}^{24} r_{j} \times C^{h} + \sum_{h=1}^{et_{j}^{i}} r_{j} \times C^{h}$$</p>

**Step 5:** Find the scenario that incurs the minimum charging cost
<p align="center">$\quad \text{min } cost_{j}^{i}$</p>

**Step 6:** Assign the optimal charging starting time, ending time and report its charging cost
<p align="center">$opt_{j}^{i} (st_{j}^{i}, et_{j}^{i},\text{min } cost_{j}^{i})$</p>

### 🚨 Outcome


### 💡 Key learnings

###  Reference
[[1]](https://alternative-fuels-observatory.ec.europa.eu/system/files/documents/2023-06/2022%20EAFO_CountryReport_DE.pdf) Consumer Monitor 2022, European Alternative Fuels Observatory. Country report: Germany
