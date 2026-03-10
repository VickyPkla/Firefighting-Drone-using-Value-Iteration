# Drone Fighter: Grid World Navigation Using Value Iteration

## Overview

**Drone Fighter** is a reinforcement learning project that demonstrates the Value Iteration algorithm applied to a practical grid world navigation problem. A drone must navigate a 10×10 grid world filled with obstacles, navigate to an objective marked by fire to extinguish it, and return successfully while avoiding hazards.

## What This Project Does

This project implements a complete reinforcement learning pipeline:

- **Environment Simulation**: A realistic grid world with dynamic obstacles including boulders, smoke zones, and landmark locations
- **Value Iteration Algorithm**: Computes optimal state values and policies for the drone to maximize rewards
- **Policy Learning**: Generates an optimal navigation policy that guides the drone's decision-making
- **Visualization**: Displays value functions, learned policies with directional indicators, and animated drone trajectories

The drone must:
1. Navigate to the lake to collect water (enters state with water = 1)
2. Travel to the fire location to extinguish it (only awards reward when carrying water)
3. Optimize the path to maximize total cumulative reward

## Key Features

- **Grid World Environment**: 10×10 grid with multiple obstacle types
  - **Boulders**: Impassable obstacles that block movement
  - **Smoke Zones**: Hazardous areas that reduce movement precision
  - **Lake**: Water source at (1,1) for water collection
  - **Fire**: Target location at (8,8) requiring water to extinguish
  
- **Stochastic Movement**: Realistic motion uncertainty
  - In open areas: 70% intended action, 10% sideways drift, 10% staying still, 10% holding
  - In smoke zones: 40% intended action, 40% staying still, 20% drift (divided equally sideways)

- **Reward System**:
  - **+1000**: Extinguishing fire (reaching fire location with water)
  - **-500**: Entering smoke zones (hazard penalty)
  - **-1000**: Hitting boulders (collision penalty)
  - **-10**: Each step (movement cost)

- **Convergence**: Value Iteration converges using threshold θ = 1e-8 with discount factor γ = 0.95

## Project Structure

```
├── drone_fighter.ipynb          # Main Jupyter notebook with complete implementation
├── Drone Simulation.gif         # Animated demo of drone navigation
├── Policy Plot for Empty State.png    # Optimal policy visualization (w=0)
├── Policy Plot for Filled State.png   # Optimal policy visualization (w=1)
├── Value Function for Empty State.png # State values visualization (w=0)  
├── Value Function for Filled State.png # State values visualization (w=1)
└── README.md                    # This file
```

## Getting Started

### Prerequisites

- Python 3.7+
- Jupyter Notebook or JupyterLab
- Required packages: `numpy`, `matplotlib`, `imageio`

### Installation

1. **Clone or download the project** and navigate to the project directory:
   ```bash
   cd "Grid World Navigation using Value Iteration"
   ```

2. **Create a virtual environment** (recommended):
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
   
   Or manually install required packages:
   ```bash
   pip install numpy matplotlib imageio
   ```

### Usage

1. **Open the Jupyter notebook**:
   ```bash
   jupyter notebook drone_fighter.ipynb
   ```

2. **Run all cells** in sequence. The notebook will:
   - Define the grid world environment with obstacles
   - Run Value Iteration to compute optimal values and policies
   - Display visualizations of the learned value functions and policies
   - Simulate drone navigation and generate an animated GIF

3. **View outputs**:
   - **Value Function Plots**: Show the computed value for each position in both water states (w=0 and w=1)
   - **Policy Plots**: Display optimal action at each position (↑↓←→ for directions, • for holding water)
   - **Drone Simulation GIF**: Animated trajectory showing the drone following the learned policy

### Example Output

After running the notebook, you'll see:
- Convergence confirmation: "Value Iteration Converged"
- Four visualization plots demonstrating the learned policy and value function
- An animated GIF (`drone_simulation.gif`) showing the drone solving the grid world

## Understanding the Algorithm

### Value Iteration

The notebook implements the standard Value Iteration algorithm:

```
Initialize V(s) = 0 for all states
While convergence not achieved:
    For each state s:
        V(s) = max_a Σ P(s'|s,a)[R(s,a,s') + γV(s')]
    If max change in V < θ: break
```

### State Representation

Each state is represented as a tuple `(row, column, water)` where:
- `row`: Y-coordinate (0-9)
- `column`: X-coordinate (0-9)
- `water`: Binary flag (0 = no water, 1 = carrying water from lake)

This gives a state space of 10 × 10 × 2 = 200 states.

### Environment Dynamics

Movement is stochastic - the intended action may not occur due to environmental factors (wind, terrain). The transition probabilities vary based on location:
- **Open areas**: Higher probability of intended action (70%)
- **Smoke zones**: Lower probability of intended action (40%), higher chance of staying still (40%)

## Customization

You can modify the notebook to experiment with different configurations:

- **Grid Size**: Change `grid_size` variable to create larger/smaller environments
- **Reward Values**: Adjust reward function returns for different objectives
- **Discount Factor**: Modify `gamma` (0.95) to change long-term planning preference
- **Convergence Threshold**: Adjust `theta` (1e-8) for faster/more precise convergence
- **Obstacle Placement**: Modify `boulders`, `smoke`, `lake`, and `fire` sets
- **Step Limit**: Adjust `max_steps` in the `simulate()` function

## Interpretation Guide

### Policy Visualization
- **↑ ↓ ← →**: Directional movement in each state (North, South, West, East)
- **•**: Hold action (wait; used to collect water at lake)
- **Colored cells**:
  - Blue: Lake (water source)
  - Red: Fire (objective)
  - Gray: Smoke (hazardous areas)
  - Dark gray: Boulders (obstacles)
  - White: Open space

### Value Function Visualization
- Numbers represent the expected cumulative reward starting from that position following the optimal policy
- Higher values indicate more favorable starting positions
- Terminal states (boulders and fire after extinguishing) show no value

## How to Get Help

- **Documentation**: Review the comments and markdown cells in the Jupyter notebook
- **Algorithm Questions**: Refer to standard RL textbooks (e.g., Sutton & Barto's "Reinforcement Learning: An Introduction")
- **Grid World Problems**: Common benchmark problems in RL literature with well-established solutions

## Contributing

This is a demonstration project for educational purposes. If you'd like to:
- Improve the code or documentation
- Add new features (different grid types, multiple agents, different algorithms)
- Fix bugs or enhance visualizations

Feel free to create an improved version for your learning purposes.

## License

This project is provided as-is for educational and learning purposes.

---

**Happy Learning!** This project demonstrates key concepts in reinforcement learning including state representation, value iteration, policy optimization, and environmental simulation. Feel free to experiment with different parameters to deepen your understanding of how algorithms learn optimal behaviors.
