# ReliefRoute AI: Hazard-Aware Drone Delivery Planner

An interactive AI simulation that plans relief delivery routes for a drone while avoiding storm and no-fly zones. Three classic search algorithms (Hill Climbing, Simulated Annealing and a Genetic Algorithm) solve the same problem side by side so their behaviour can be compared live.

**Live simulation:** https://shivaykapoor24.github.io/reliefroute-ai/
**Report:** see `ReliefRoute_AI_Report.pdf` in this repository

## Problem Statement

After a flood, cyclone or earthquake, roads are often cut off. A single drone starts at a base station, must visit every village exactly once and return to base. Some areas of the map are hazard zones, and flying through them is allowed but heavily penalised. The task is to find the visiting order with the lowest total cost.

For 25 villages there are about 6 x 10^23 possible routes, so brute force is impossible and heuristic AI search is needed.

## Features

- Three AI solvers running in parallel on the same map and the same evaluation budget (60,000 cost evaluations)
- Live map panels showing the best route found so far by each method
- Convergence chart comparing best cost against evaluations
- Ranked results table with percentage improvement over a nearest-neighbour baseline
- Adjustable number of villages (10 to 40), hazard zones (0 to 7) and simulation speed
- Random map generator and a restart option for repeated experiments
- Works in any modern browser, on desktop and mobile, with no installation

## Problem Formulation

| Element | Definition |
|---|---|
| State | A complete tour, which is a permutation of villages starting and ending at the base |
| Initial state | A random permutation |
| Actions | 2-opt move: reverse the segment between two positions |
| Goal | Minimise total route cost |
| Edge cost | distance x (1 + 4 x fraction of the edge inside hazard zones) |

Each edge is sampled at 16 points. If k of them fall inside a hazard zone, the edge cost is distance x (1 + 4k/16). An edge fully inside a storm zone therefore costs five times its length.

## Algorithms

**Hill Climbing**
Proposes random 2-opt neighbours and accepts only improvements. Fast, but it stops improving once it reaches a local optimum.

**Simulated Annealing**
Uses the same neighbourhood but accepts worse moves with probability exp(-delta / T). The temperature T is cooled geometrically to one thousandth of its starting value, which lets the search escape local optima early and settle down later.

**Genetic Algorithm**
Population of 60 tours with tournament selection (size 3), order crossover (OX), swap or 2-opt mutation, and elitism (the best two tours always survive).

## How to Use

1. Open the live simulation link.
2. Press **Run** to start all three solvers.
3. Watch the route panels and the convergence chart update.
4. Use **Restart solvers** to rerun on the same map from a new random start.
5. Use **New map** or the sliders to test more villages or more hazard zones.
6. Record the final costs from the results table for your experiments.

## Running Locally

No build step or dependencies are needed.

1. Clone or download this repository.
2. Open `index.html` in any web browser.

## Project Structure

| File | Description |
|---|---|
| index.html | Complete simulation (HTML, CSS and JavaScript in a single file) |
| ReliefRoute_AI_Report.pdf | Case study report |
| README.md | Project documentation |

## Technologies Used

- HTML5 and CSS3
- JavaScript (ES6)
- HTML Canvas API for rendering

## Experiments

| Experiment | Villages | Hazard zones | Purpose |
|---|---|---|---|
| E1 | 15 | 0 | Pure distance problem |
| E2 | 25 | 4 | Default balanced case |
| E3 | 25 | 7 | Many obstacles, rugged cost landscape |
| E4 | 40 | 4 | Large search space |

Each experiment should be run at least five times and the final best costs recorded and compared.

## Future Work

- Multi-drone fleet routing
- Moving storm cells with live replanning
- Battery and payload constraints
- Village urgency estimated with Bayesian reasoning
- Comparison against exact solvers on small maps

## References

1. S. Russell and P. Norvig, Artificial Intelligence: A Modern Approach, 4th ed., Pearson, 2020.
2. S. Kirkpatrick, C. D. Gelatt and M. P. Vecchi, "Optimization by Simulated Annealing," Science, vol. 220, 1983.
3. J. H. Holland, Adaptation in Natural and Artificial Systems, University of Michigan Press, 1975.
4. G. A. Croes, "A Method for Solving Traveling-Salesman Problems," Operations Research, vol. 6, 1958.
5. L. Davis, "Applying Adaptive Algorithms to Epistatic Domains," IJCAI, 1985.

## Author

Shivay Kapoor 
