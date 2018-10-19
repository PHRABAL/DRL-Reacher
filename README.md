<img src="https://s3.amazonaws.com/video.udacity-data.com/topher/2018/June/5b1ea778_reacher/reacher.gif" halign="center" />


# Deep Reinforcement Learning - Reacher Continuous Control

## Overview
Using the Unity agent/environment "Reacher", this deep reinforcement learning task trains an AI agent to keep its double jointed arm on a ball for as long as possible. The task is considered solved when the agent, who receives various rewards for maintaining its position, receives an average reward of 30 over 100 consecutive episodes.

The Unity environment has two options - using a single agent or 20 agents in parallel. In the latter case, the experiences and rewards of all 20 agents running at the same time are combined during evalation and training. This repo uses the 20 agent version.

In either environment, each agent receives feedback in the form of a reward (+0.1 to +0.4) or no reward after taking each action. The environment conveys to the agent the state of the agent in the form of 33 different values related to position, rotation, velocity and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints.

At first the agent randomly takes actions and records the feedback, but eventually begins to take those experiences and learn from them using a Deep Deterministic Policy Gradient (DDPG) algorithm.

The attached code written in Python, using PyTorch and presented in a Jupyter Notebook, demonstrates how the agent learns and eventually achieves an average score of 30 over 100 consecutive episodes.

## Methodology

### Model Overview
As seen in the code, the DDPG algorithm is used to train the agent. DDPG is a model-free, off-policy, policy gradient-based algorithm that uses two separate deep neural networks (one actor, one critic) to both explore the stocastic environment and, separately, learn the best policy to achieve maximum reward. DDPG has been shown to be quite effective at continuous control tasks.

### Simplify
One of the challenges with deep reinforcement learning and, particularly the DDPG algorithm, is the large number of hyperparameters that need to be tuned to achieve good performance. My approach to building an effective model was to first use the default settings found in the DDPG research paper for most of the hyperparameters, then attempt to reduce the size of the actor-critic neural networks to the simplest possible structure. I find that the simplier the model, the easier it is to tune and the more stable the learning. 

The <a href="https://arxiv.org/pdf/1509.02971.pdf" atarget="_blank">DDPG paper</a> used a two layer neural network of 400 and 300 nodes, respectively, for both the actor and critic. I reduced this to nodes of 256-128 and reduced the learning rate for the critic network by a factor of 0.1 compared to the DDPG paper. This modification alone produced outstanding performance. The model was able to achieve the 30 reward goal in the least possible amount of time - just 100 episodes.

### Other hyperparameters


### Exploitation vs. Exploration

Initially the agent knows nothing about the environment it is in. It has to try random actions to determine which are optimal to achieve its goal. However, over time, as it develops a repository of past actions, it has to transition from trying new things to doing what it has already learned are high quality actions. This exploration vs. exploitation balance, over the life of hundreds of episodes, is very impactful on the final result. 

<strong>I found in this environment that, relative to other DRL agents I have trained, the agent does not need to try nearly as many random actions to gather the necessary experience to act efficiently. Therefore, I have weighted exploitation heavily from an early stage while keeping a minimal amount of exploration.</strong>

### Learning From Experience

The agent stores all of its past experiences, which includes 1) what action it took that resulted in 2) what reward/penalty and 3) what next state of the environment did the action place the agent in. The question when training the agent is, when and how to use this experience to learn more optimal actions going forward. Hyperparameters include the number of experiences to keep in a rolling history, how often to pull a new batch of experiences to update the learning model, how much to weight what's learned, etc. 

<strong>In optimizing this agent, I found that the agent learns optimal actions fairly quickly, as described above, and therefore, I had the agent do a soft update of the network after every agent action. This approach, as opposed to only updating the network after every few actions, was far superior, cutting the training time by more than half.</strong>

## Results and Future Work

The above approach produced an average reward of 13+ over 100 consecutive episodes within 71 episodes, as shown in the jupyter notebook. Much of the improvement and variation in results centers around how much, how fast and how often to use the agent's past experiences to train the network and improve the action policy. Future work focused on this area could yield improved results, namely Prioritized Experience Replay or other methods of selecting/optimizing which experiences to utilize for training purposes.

## Setup Instructions

To reproduce this model on a Mac, clone the <a href="https://github.com/udacity/deep-reinforcement-learning">Udacity DRLND repo</a>, then place the banana.app.zip file in the p1_navigation folder and uncompress it. Also place the Juputer notebook there and run it.
