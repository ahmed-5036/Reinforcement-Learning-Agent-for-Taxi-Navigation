# Reinforcement Learning Agent for Taxi Navigation

## Overview

This project implements a reinforcement learning agent using Q-learning to navigate a taxi in the Taxi-v3 environment provided by OpenAI's Gym. The goal is to pick up passengers from specific locations and drop them off at designated destinations efficiently.

## Environment Details

- **Environment**: Taxi-v3
- **Action Space**: Six actions where:
  - `0` moves the taxi south
  - `1` moves the taxi north
  - `2` moves the taxi east
  - `3` moves the taxi west
  - `4` picks up a passenger
  - `5` drops off a passenger
- **Observation Space**: 500 discrete states (25 taxi positions * 5 passenger locations * 4 destinations)
- **Rewards**:
  - -1 for each step taken without other rewards
  - +20 for successful passenger delivery
  - -10 for illegal pickup or dropoff actions

## Explanation

### Q-Learning Algorithm

Q-learning is a model-free reinforcement learning algorithm that learns an optimal action-selection policy for a given Markov decision process (MDP). Here’s how the algorithm works in the context of this project:

1. **Initialization**:
   - Initialize a Q-table, Q(s, a) with zeros, where s is a state and a is an action.

2. **Training Process**:
   - **Episode Initialization**: Reset the environment to start a new episode. Select an initial state, \( s \).
   - **Action Selection**: Use an epsilon-greedy policy to select an action, \( a \), for the current state, \( s \). With probability \( \epsilon \), choose a random action (exploration); otherwise, choose the action with the highest Q-value for \( s \) (exploitation).
     - **Epsilon (Exploration Rate)**: Controls the balance between exploration and exploitation. A higher epsilon encourages more exploration, helping the agent discover better policies. In this project, \( \epsilon = 0.9 \) is used initially to favor exploration.
   - **Environment Interaction**: Execute the selected action, observe the next state, \( s' \), and receive a reward, \( r \).
   - **Q-value Update**: Update the Q-value of the current state-action pair using the Q-learning update rule:
     ```
     Q(s, a) <- (1 - α) * Q(s, a) + α * [r + γ * max_{a'} Q(s', a')]
     ```
     where:
     - α (learning rate) controls the weight given to new information.
     - γ (discount factor) determines the importance of future rewards.
     - max_{a'} Q(s', a') is the maximum Q-value for the next state s', representing the estimated future reward.

   - **Termination**: Repeat the above steps until the episode terminates (e.g., the passenger is successfully dropped off or the maximum number of steps is reached).

3. **Testing Process**:
   - After training, evaluate the agent's learned policy by running it in the environment for one episode.
   - Use the learned Q-table to select actions for each state encountered during testing.
   - Calculate the total reward accumulated during the test episode to assess the agent's performance.
