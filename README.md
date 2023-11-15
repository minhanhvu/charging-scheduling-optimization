### 📝 NOTE:
Here you'll find the summary of my projects. For in-depth analysis/discussion, please visit my [Gitbook](https://minh-anh-vu.gitbook.io/anh-vus-datacracy-hub/~/changes/K52oJexU6PiKPB9bRTo5/statistical-projects/process-simulation-charging-scheduling-optimization)


# Charging-Scheduling-Optimization
The model suggests the optimal charging starting time and charging speed for e-cars to minimize the user's charging cost, considering the predetermined electricity pricing scheme

### ⚡Context
#### Problem & Constraint
A building manager grapples with a rising demand for electric car charging from her tenants. There are two pressing concerns. If the cars commence charging immediately and simultaneously at the highest charging speed upon their arrival, it would cause a sudden heightened load and risk the electrical system stability. Moreover, if they start charging during peak hours, the energy cost would be unnecessarily expensive.

So her goal is to devise a charging strategy that minimizes energy costs and alleviates the electrical system burden, with the main constraint being to meet energy demand before car departure
#### Settings
* **Electricity prices:** The electricity prices during high-load phases are higher compared to low-load periods. These prices are predetermined.
* **Charging speed**:The charging system is built-in with 3 charging speeds, namely, slow, medium, and fast charging.
* **Charging behaviors:** The customers arrive at the charging station, request the energy needed, and provide the departure time. The cars can stay longer than the duration needed for charging, which is a typical charging pattern found in residential or workplace buildings
  
#### Strategy
The strategy is straightforwards: delay charging to low-cost hours whenever the cars stay longer than the required charging duration 

To achieve that, the algorithm is designed to follow two key principles regarding the selection of **charging speed** and **charging time**. (1) Prioritize slower charge rate whenever feasible to reduce unnecessary pressure on the power grid and then (2) start the charging session at the first interval of the scenario that incurs the minimum cost. If the charging request cannot be fully satisfied within the time limit despite applying the highest charging speed, the algorithm will notify the vehicle's user.

### 💡 Outcome

#### Simulated inputs and algorithm's outputs
<p align="center"> 
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/9d004f54-4543-4fd4-a335-018f8f030e72" width=60% height=60%>
</p>

#### Evaluations

<p align="center"> 
<img src="https://github.com/minhanhvu/charging-scheduling-optimization/assets/87383756/d11a89a0-b6a0-4fcf-bc5d-80a2284b2cc4" width=60% height=60%>
</p>

🎊 The algorithm’s effectiveness is assessed based on two metrics: (1) energy fulfillment rate and (2) the percentage of cost savings compared to charging upon arrival. Briefly, the average fulfillment rate is 94.8%. The entire fleet enjoy a 17.3% decrease in energy cost compared to the unscheduled charging. 

### 💡 Key learnings
- This project is a simple form of placement algorithm with an objective function (charging cost minimization), constraint function (charging must be done within parking time), and decision variable (charging starting time). Using Python, I can simulate all feasible solutions and then identify the optimal one. 

- I found this logic quite similar to Monte-Carlo simulation, which is an incredible useful tool in business forecast to understand possible outcomes given there are random factors in the decision model. 


###  Reference
[[1]](https://ieeexplore.ieee.org/document/8821960) S. M. S., I. A. T. P. and D. D., "Optimized Charge Scheduling of Plug-In Electric Vehicles using Modified Placement Algorithm," 2019 International Conference on Computer Communication and Informatics (ICCCI), Coimbatore, India, 2019, pp. 1-5, doi: 10.1109/ICCCI.2019.8821960.
