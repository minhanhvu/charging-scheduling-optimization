# Charging-Scheduling-Optimization
The model suggests the optimal starting time for electric vehicles to minimize the user's charging cost, considering the Time-of-use tariff scheme

### ⚡Context
**Problem statement**

Amidst the growing climate-conscious population and the government incentives for electric vehicles (EVs), the world is experiencing a transition from internal combustion engines to zero-emission mobility. Large-scale adoption of electric cars in the near future is foreseeable. Electric car registrations have quadrupled within just three years (2019-2021) and its market share hit a new record of 19.2% in the EU as of April 2023. However, this transition poses some potential obstacles to both the EV owners and the system that supports EV penetration.

- _Grid's instability_: such as unacceptable voltage fluctuation, caused by a surge in electricity consumption due to EV charging at peak hours.
- _Inconvenience of charging access:_ is one of the keys that discourage both the adoption and continued use of EVs [1]. The charging infrastructure network shall be designed in a way that avoids charging inconvenience, which requires an understanding of the charging behaviors, for example, where, when, how often, and how long EV owners charge their vehicles.
- _The uncertainty of the charging cost_: charging cost is the primary operating cost of an EV, which is directly linked to the electricity price during charging. EV owners would be happy when they can minimize this cost while still meeting their vehicle's charging needs.


**Solutions:** 

Consider a manager of a residential building that wishes to minimize the cost of energy consumption while still meeting the EV charging demand of the tenants. This algorithm will fulfill that goal by determining the optimal time to initiate charging for each individual EV. 

The charging cost is computed based on a _Time-of-Use tariff scheme_, where prices are predetermined and fixed during a period of time. Typically, the tariffs during high-load phases are generally higher compared to low-load periods. Consequently, delaying charging to low-cost hours helps to shift partial electricity consumption to off-peak periods, which _not only provides economic advantages but also contributes to the stability of grid operations._



### 🔌 Process

**Step 1**: Load the arrival time, departure time, charging duration, and charging power of the EV

**Step 2**: Load the electricity tariff from the energy service supplier

**Step 3**: Find the number of possible starting time block

**Step 4:** Calculate the charging start time and the respective end time

- Start time = arrival + x(i) -1
- End time = arrival + x(i) -1 + charge length

**Step 5:** Retrieve a set of electricity prices that correspond to the charging time blocks between start and end time.

Note: overnight charging start till 24 + 0 till end.

**Step 6:** Calculate the charging cost of each scenario

**Step 7:** Find the charging starting time that incurred the lowest cost

**Step 8:** Assign the optimal charging starting time to the vehicle and report the charging cost

### 🚨 Outcome


### 💡 Key learnings

###  Reference
[[1]](https://alternative-fuels-observatory.ec.europa.eu/system/files/documents/2023-06/2022%20EAFO_CountryReport_DE.pdf) Consumer Monitor 2022, European Alternative Fuels Observatory. Country report: Germany
