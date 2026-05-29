# INTERVENTIONIST: Procedural Content Generation via Reinforcement Learning (C-PCGRL)

A Reinforcement Learning framework for **Controllable Procedural Content Generation (PCGRL)** that transforms an RL agent from a game player into an autonomous level designer.

This project explores **Constrained Procedural Content Generation via Reinforcement Learning (C-PCGRL)**, where a PPO-based agent learns to generate playable game levels while satisfying human-defined constraints and guaranteeing functional validity through an integrated A* pathfinding oracle.

---

## Project Motivation

Traditional Procedural Content Generation techniques such as Cellular Automata, Random Walks, and Perlin Noise can generate large quantities of content but often fail to ensure playability, controllability, and design intent.

This project addresses these limitations by introducing an **Interventionist PCGRL framework**, allowing human designers to provide partial sketches while the RL agent completes the environment intelligently.

The goal is to bridge the gap between:

* Human creativity
* Autonomous generation
* Formal playability constraints

---

## Key Contributions

### Constrained Policy Optimization

The agent is trained under strict constraints to ensure generated levels remain valid and traversable.

### Human-AI Collaborative Design

Supports partial designer sketches that the RL agent respects and completes intelligently.

### Reward Oracle Integration

An A* pathfinding oracle is embedded directly into the reward function to verify solvability during training.

### Multi-Objective Reward Engineering

Balances multiple competing objectives:

* Connectivity
* Path Complexity
* Diversity
* Constraint Satisfaction

### Explainable Generation

Generated content can be validated mathematically rather than relying purely on aesthetic heuristics.

---

## System Architecture

```text
                   Human Sketch
                         │
                         ▼
              Environment Initialization
                         │
                         ▼
                RL Architect Agent
                  (PPO + CNN)
                         │
                         ▼
                Level Modification
                         │
                         ▼
               Reward Evaluation
                         │
       ┌─────────────────┼─────────────────┐
       │                 │                 │
       ▼                 ▼                 ▼
 Connectivity      Path Complexity      Diversity
   Reward             Reward             Reward
       │                 │                 │
       └─────────────────┼─────────────────┘
                         │
                         ▼
                A* Search Oracle
                         │
                         ▼
                 PPO Optimization
```

---

## Reinforcement Learning Formulation

The problem is modeled as a Markov Decision Process (MDP):

### State Space

The environment consists of an N × M grid representing the level layout.

Tile types include:

* Empty
* Wall
* Player
* Enemy
* Goal

The state is encoded as a tensor:

```text
(N, M, C)
```

where C represents the number of tile categories.

### Action Space

The project uses the Turtle Representation.

Movement Actions:

```text
Up
Down
Left
Right
```

Modification Actions:

```text
Place Wall
Place Floor
Place Enemy
Place Goal
```

### Transition Function

Deterministic environment transitions update the grid and cursor position.

---

## Reward Function

The reward function combines multiple objectives:

```text
R = W1·Rconn + W2·Rpath + W3·Rent − W4·Rcons
```

Where:

### Connectivity Reward

Rewards valid paths between Start and Goal.

### Path Complexity Reward

Encourages winding and interesting routes instead of trivial solutions.

### Entropy Reward

Promotes diversity in tile placement and level structure.

### Constraint Penalty

Penalizes invalid designs and rule violations.

---

## A* Search Oracle

A deterministic A* pathfinding module acts as a reward oracle.

Responsibilities:

* Verify level solvability
* Measure path complexity
* Detect disconnected layouts
* Guide reward shaping

This ensures generated levels remain functionally playable.

---

## Technology Stack

### Reinforcement Learning

* Proximal Policy Optimization (PPO)

### Deep Learning

* TensorFlow 2.10
* Convolutional Neural Networks (CNN)

### Environment

* OpenAI Gym 0.21
* gym-pcgrl

### Scientific Computing

* NumPy

### Development Environment

* Conda (pcgrl_env)

---

## Experimental Results

### Agent Training

The Architect Agent was trained for 300 episodes and successfully learned to modify grid environments toward improved connectivity and playability.

### Solvability Performance

| Mode                       | Solvability |
| -------------------------- | ----------- |
| Autonomous Generation      | ~40%        |
| Interventionist Generation | 100%        |

The interventionist approach significantly improved reliability by allowing human sketches to provide foundational structural constraints.

### Key Observation

The introduction of Hard Locking Constraints successfully preserved designer intent even when the model was undertrained.

---

## Advantages

* Infinite level generation capability
* Guaranteed playability through A* validation
* Human-AI collaborative workflow
* Adjustable difficulty targets
* Explainable generation process
* Constraint-aware content creation

---

## Current Limitations

* High computational training cost
* Reward engineering complexity
* Limited interpretability of latent policies
* Environment-specific retraining requirements
* Restricted training duration due to hardware limitations

---

## Future Work

### Larger Tile Sets

Support for:

* Keys
* Doors
* Enemies
* Zelda-style mechanics

### Large-Scale Training

Cloud GPU training with 1M+ episodes.

### Interactive Design Tool

Real-time GUI allowing designers to:

* Paint constraints
* Lock regions
* Generate completions instantly

### Advanced Constraints

* Difficulty targeting
* Narrative-driven generation
* Multi-objective optimization
* Explainable PCGRL policies

---

## References

* Khalifa et al. — Procedural Content Generation via Reinforcement Learning
* Schulman et al. — Proximal Policy Optimization Algorithms
* Earle et al. — Controllable PCGRL
* OpenAI Gym
* gym-pcgrl Framework

---

## Author

**Parun Sethupathy**

B.Tech Computer Science and Engineering (AI & ML)

Project completed as part of Reinforcement Learning coursework exploring Human-AI collaborative procedural content generation through constrained reinforcement learning.
