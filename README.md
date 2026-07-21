# Air Combat Multi-Agent Reinforcement Learning

## Function Approximation and Neural Value-Based Learning for Autonomous Decision Making in Adversarial Environments


<p align="center">

A Research-Oriented Reinforcement Learning Framework for Autonomous Aircraft Control Under Multi-Agent Adversarial Conditions

</p>


---

# Abstract

This repository presents a research-oriented implementation of a multi-agent air combat simulation framework designed to investigate autonomous decision-making under adversarial conditions.

The proposed environment models a tactical battlefield where a single autonomous aircraft agent must complete a mission while facing multiple coordinated suicide drones.

The aircraft must simultaneously optimize:

- Mission completion
- Threat avoidance
- Resource management
- Enemy elimination
- Long-term survival


This project studies the transition from manually engineered decision systems toward data-driven reinforcement learning by comparing:

1. **Linear Function Approximation with Temporal Difference Learning**
2. **Neural Value Function Approximation with Approximate Policy Iteration**

Experimental results demonstrate that neural value-based learning significantly improves tactical decision-making and scalability compared with handcrafted feature-based policies.


---

# Research Question

Can autonomous agents learn effective tactical strategies in adversarial environments without explicitly programmed combat rules?


This project investigates how different value representation methods influence decision quality in a dynamic multi-agent environment.


---

# Project Overview


The system consists of:

- One autonomous aircraft agent (learning agent)
- Multiple cooperative enemy drones
- A stochastic grid-world battlefield
- Limited missile resources
- Static obstacles
- Mission objectives


The learning pipeline evolves through two stages:


```

        Stage 1
Hand-crafted Feature Engineering
+
Linear Value Approximation

          ↓

        Stage 2
Neural Value Approximation
+
Temporal Difference Learning
+
Experience Replay
+
Curriculum Learning

```


---

# Environment Description


## Battlefield Simulation


| Component | Specification |
|---|---|
| Environment | Discrete Grid World |
| Grid Size | 15 × 10 |
| Aircraft Agents | 1 |
| Enemy Drones | 1 – 4 |
| Obstacles | 10 Random Mountains |
| Episode Length | 30 Steps |
| Objective | Destroy all drones and reach goal |


Each episode contains a randomly generated battlefield configuration:

- Aircraft initial position
- Drone positions
- Mountain locations
- Reload zone position
- Goal location


This stochastic design requires adaptive decision-making rather than fixed policies.


---

# Agent Architecture


# Aircraft Agent


The aircraft is the learning agent responsible for mission execution.


## Action Space


### Movement Actions

- Up
- Down
- Left
- Right


### Combat Actions

- Fire missile
- Engage nearby drones
- Maintain line-of-sight constraints


---

## Resource Management


The aircraft operates with limited ammunition.

A reload zone is randomly positioned in the environment, requiring strategic decisions between:

- Attacking enemies
- Avoiding threats
- Recovering resources


---

## Objective Function


The aircraft succeeds when:

1. All drones are eliminated
2. The aircraft reaches the mission goal


Reward design:


| Event | Reward |
|---|---:|
| Destroy drone | +250 / +350 |
| Reach goal | +1000 |
| Crash into mountain | -500 |
| Captured by drones | -1000 |
| Time penalty | -1 per step |


---

# Adversarial Drone Agents


Enemy drones represent cooperative adversarial agents.

Their objective is to neutralize the aircraft through:


- Pursuit behavior
- Spatial blocking
- Surround formation
- Swarm attacks


Drone decision-making considers:

- Distance to aircraft
- Relative position
- Formation coordination
- Inter-agent proximity


---

# Methodology


## Part 1 — Linear Function Approximation


The first experiment investigates whether manually designed tactical features can represent complex combat situations.


The aircraft value function is defined as:


\[
V(s)=w^T\phi(s)
\]


where:

- \( \phi(s) \) is the engineered state feature vector
- \( w \) represents learnable parameters


---

## Feature Representation


The aircraft state representation contains:


### Navigation Features

- Distance to goal
- Distance to reload zone
- Obstacle proximity


### Combat Features

- Distance to each drone
- Number of nearby enemies
- Shootable targets
- Threat estimation


### Resource Features

- Remaining missiles
- Reload availability


Drone features include:

- Distance to aircraft
- Relative direction
- Surrounding positions
- Blocking strategy
- Cooperative formation


---

# Part 2 — Neural Value-Based Reinforcement Learning


The second stage replaces linear approximation with a neural value estimator.


## Neural Network Architecture


```

Input State Representation

      |
      v

Fully Connected Layer
128 neurons

      |

    ReLU

      |
      v

Fully Connected Layer
64 neurons

      |

    ReLU

      |
      v

Estimated State Value V(s)

```


The neural model learns nonlinear relationships between:

- Enemy proximity
- Remaining ammunition
- Survival probability
- Mission progress


---

# Reinforcement Learning Framework


The learning system incorporates:


## Temporal Difference Learning


The value function is updated using future reward estimation.


## Experience Replay


A replay buffer stores previous experiences:

\[
(s,a,r,s')
\]


to improve learning stability and reduce correlation between samples.


## Target Network


Periodic target synchronization stabilizes value updates.


## ε-Greedy Exploration


The agent balances:

- Exploration of new strategies
- Exploitation of learned behavior


## Curriculum Learning


Training difficulty increases gradually:


```

1 Drone

↓

2 Drones

↓

3 Drones

↓

4 Drones

```


Knowledge learned from simpler scenarios is transferred to more challenging environments.


---

# Experimental Setup


Evaluation is performed using:


- 400 independent test episodes
- Different numbers of enemy drones
- Quantitative comparison between methods


Metrics:


## Win Rate

Percentage of episodes where:

- All drones are destroyed
- Aircraft reaches the goal


## Average Return

Mean cumulative reward achieved during evaluation.


---

# Results


## Performance Comparison


| Enemy Drones | Linear Approximation | Neural Value Learning |
|---|---:|---:|
| 1 | 77.2% | **100%** |
| 2 | 74.5% | **100%** |
| 3 | 78.0% | **100%** |
| 4 | 0.0% | **14.5%** |


---

# Discussion


## Neural Value Learning Improves Tactical Reasoning


The neural value estimator captures nonlinear interactions between:

- Missile availability
- Enemy distance
- Threat level
- Mission objectives


These relationships are difficult to model using handcrafted linear functions.


---

## Curriculum Learning Improves Scalability


Progressive training enables knowledge transfer from simple combat scenarios to increasingly complex environments.


The learned policy achieves perfect performance against up to three drones.


---

## Multi-Agent Coordination Remains Challenging


The four-drone scenario remains difficult due to:


- Expanded state space
- Coordinated enemy behavior
- Limited ammunition
- Increased tactical complexity


This highlights the challenges of scalable multi-agent reinforcement learning.


---

# Learning Stability Analysis


Training behavior was analyzed using:


## Aircraft Value Function Training Error


![Value Function Error](results/Aircraft%20Value%20Function%20Training%20Error.png)



## Temporal Difference Error Convergence


![TD Error](results/TD%20Error%20Convergence%20with%20Moving%20Average.png)



The decreasing error patterns indicate improved value estimation stability during training.


---

# Repository Structure


```

air-combat-multi-agent-reinforcement-learning/

│
├── docs/
│   └── Technical_Report.pdf
│
├── notebooks/
│   ├── part1_function_approximation_air_combat.ipynb
│   └── part2_multi_agent_drone_defense_rl.ipynb
│
├── results/
│   ├── Aircraft Value Function Training Error.png
│   └── TD Error Convergence with Moving Average.png
│
├── README.md
├── requirements.txt
├── LICENSE
└── CITATION.cff

````


---

# Technologies


- Python
- PyTorch
- NumPy
- Matplotlib
- Reinforcement Learning
- Temporal Difference Learning
- Neural Networks
- Multi-Agent Systems
- Function Approximation


---

# Reproducibility


All experiments are implemented using Jupyter notebooks.

The provided notebooks include:

1. Environment implementation
2. Feature extraction
3. Value function training
4. Reinforcement learning experiments
5. Evaluation and visualization


---

# Future Research Directions


Possible extensions include:


- Deep Q Networks (DQN)
- Double DQN
- Dueling Network Architectures
- CNN-based spatial state representation
- Graph Neural Networks for drone coordination
- Multi-Agent Actor-Critic algorithms
- Transformer-based autonomous decision models


---

# Research Contribution


This project provides an experimental study of autonomous decision-making in adversarial environments by comparing:


- Feature-engineered value approximation
- Neural value-based reinforcement learning
- Curriculum-based policy improvement


The results demonstrate that learned value representations provide stronger tactical decision-making capabilities compared with manually designed policies.


---

# Author


## Hannah Fathi


M.Sc. Student in Artificial Intelligence and Robotics  


Spring 2025


## Research Interests

- Reinforcement Learning
- Multi-Agent Systems
- Autonomous Agents
- Computer Vision
- Medical AI
- Remote Sensing
- Explainable AI


---

# Citation


If you use this repository in academic work, please cite:


```bibtex
@software{fathi2025aircombat,
author = {Hannah Fathi},
title = {Air Combat Multi-Agent Reinforcement Learning:
Function Approximation and Neural Value-Based Learning for Autonomous Decision Making},
year = {2025},
url = {https://github.com/hannah-fathi/air-combat-multi-agent-reinforcement-learning}
}
