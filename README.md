# Charging-Scheduling-Optimization
The model suggests the optimal starting time for electric vehicles to minimize the user's charging cost, considering the Time-of-use tariff scheme

### ⚡Context
#### Problem & Constraint
A workplace building manager grapples with a rising demand for electric car charging. There are two pressing concerns. If  the cars commence charging immediately and simultaneously at the highest charging speed upon their arrival, it would cause a sudden heightened load and risk the electrical system stability. Moreover, if they start charging during peak hours, the energy cost would be unnecessarily expensive. 
So her goal is to devise a charging strategy that minimizes energy costs and alleviates the electrical system burden, with the main constraint being to meet energy demand before car departure.
#### Settings
* **Electricity prices:**** The electricity prices during high-load phases are generally higher compared to low-load periods. Here, the project addresses the Time-of-Use tariff scheme, where electricity prices vary throughout the day but are predetermined.
* **Charging speed**:The charging system is built-in with 3 charging speeds, namely, slow, medium, and fast charging.
* **Charging behaviors:** The vehicles stay longer than the duration needed for charging, which is a typical charging pattern found in residential or workplace buildings
  
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

<p align="center"> 
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/f3f37427-de6b-499d-b2ff-0f4aa10613a4" width=60% height=60%>
</p>

<p align="center"> The assumed price of electricity that captures the load profile of a workplace
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/bccb6aa6-7e31-4222-925d-26ab64c11e46" width=60% height=60%>
</p>


#### Result Summary

Apart from Car 2 and Car 4 that must start immediately upon arrival to meet their energy demand, the remaining cars are scheduled to start charging 3 to 5 hours after arrival, leveraging favorable electricity prices during shoulder and off-peak periods. Charging costs are depicted by the areas underneath the green and red outlines. We can easily see that the green areas are either smaller or overlap the red ones, which proves the cost-effective aspect of the optimized algorithm.

<p align="center">
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/0439bdf5-21d0-4358-bf6a-aa9e54d69c80" width=60% height=60%>
</p>

#### Evaluation

🎊 The algorithm’s effectiveness is assessed based on two metrics: (1) energy fulfillment rate and (2) the percentage of cost savings compared to charging upon arrival. Briefly, the average fulfillment rate is 94.8%. The entire fleet enjoy a 17.3% decrease in energy cost compared to the unscheduled charging. 

🤓 Unsurprisingly, cars that stay for long hours are the primary beneficiaries of the optimization, as seen in the case of car 1, 3, 5 and 6. Both car 2 and car 4 present no cost savings given that they commence charging upon arrival. However, in contrast to car 2, which does not benefit from the optimization for having potential to delay charging, car 4, in fact, enjoys the lowest energy cost by charging immediately. 

⁉️ Now, you may question why car 2 and car 4 don’t hit a 100% of fulfillment rate. This is due to one of the two reasons: (1) Users request exceptionally high energy demand with respect to their stay, which is the case of car 2 or (2) Continuing the session into the next interval could violate the time constraint as in car 4’s instance. In the first case, the algorithm will notify the driver. The second case can be mitigated by implementing shorter charging interval.

<p align="center">
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/66164332-575e-48df-94de-5b1115a5bfc1" width=60% height=60%>
</p>

### 💡 Key learnings
- This project is a simple form of placement algorithm with an objective function (charging cost minimization), constraint function (charging must be done within parking time), and decision variable (charging starting time). Using Python, I can simulate all feasible solutions and then identify the optimal one. 

- I found this logic quite similar to Monte-Carlo simulation, which is an incredible useful tool in business forecast to understand possible outcomes given there are random factors in the decision model. 


###  Reference
[[1]](https://ieeexplore.ieee.org/document/8821960) S. M. S., I. A. T. P. and D. D., "Optimized Charge Scheduling of Plug-In Electric Vehicles using Modified Placement Algorithm," 2019 International Conference on Computer Communication and Informatics (ICCCI), Coimbatore, India, 2019, pp. 1-5, doi: 10.1109/ICCCI.2019.8821960.
