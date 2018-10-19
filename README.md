<img align="center" src="https://s3.amazonaws.com/video.udacity-data.com/topher/2018/June/5b1ea778_reacher/reacher.gif">


# Deep Reinforcement Learning - Reacher Continuous Control

## Overview
Using the Unity agent/environment "Reacher", this deep reinforcement learning task trains an AI agent to keep its double jointed arm on a ball for as long as possible. The task is considered solved when the agent, who receives various rewards for maintaining its position, receives an average reward of 30 over 100 consecutive episodes.

The Unity environment has two options - using a single agent or 20 agents in parallel. In the latter case, the experiences and rewards of all 20 agents running at the same time are combined during evalation and training. This repo uses the 20 agent version.

In either environment, each agent receives feedback in the form of a reward (+0.1) or no reward after taking each action. The environment conveys to the agent the state of the agent in the form of 33 different values related to position, rotation, velocity and angular velocities of the arm. Each action is a vector with four numbers, corresponding to torque applicable to two joints.

At first the agent randomly takes actions and records the feedback, but eventually begins to take those experiences and learn from them using a Deep Deterministic Policy Gradient (DDPG) algorithm.

The attached code written in Python, using PyTorch and presented in a Jupyter Notebook, demonstrates how the agent learns and eventually achieves an average score of 30 over 100 consecutive episodes.

## Methodology

### Model Overview
As seen in the code, the DDPG algorithm is used to train the agent. DDPG is a model-free, off-policy, policy gradient-based algorithm that uses two separate deep neural networks (one actor, one critic) to both explore the stocastic environment and, separately, learn the best policy to achieve maximum reward. DDPG has been shown to be quite effective at continuous control tasks.

### Simplier The Better
One of the challenges with deep reinforcement learning and, particularly actor-critic algorithms such as DDPG, is the large number of hyperparameters that need to be tuned to achieve good performance. My approach to building an effective model was to first use the default settings found in the <a href="https://arxiv.org/pdf/1509.02971.pdf">DDPG research paper</a> for most of the hyperparameters, then attempt to reduce the size of the actor-critic neural networks to the simplest possible structure. I find that the simplier the model, the easier it is to tune and the more stable the learning. 

The DDPG paper used a two layer neural network of 400 and 300 nodes, respectively, for both the actor and critic. I reduced this to nodes of 256-128 and reduced the learning rate for the critic network by a factor of 0.1 compared to the DDPG paper. I also increased the batch size from 64 to 128.

## Results and Future Work

The above modifications produced outstanding performance. The model was able to achieve the 30 reward goal in the least possible amount of time - just 100 episodes.

I did experiment with tuning other hyperparameters, but they did not have the same positive impact as hidden layer node reduction and learning rate changes described above. Having said that, I believe that adjustments to the Ornstein-Uhlenbeck noise level, which controls the amount of exploration the agent does, could yield improved results in some environments. Decaying the noise level (exploration) over time is regularly done in other deep reinforcement learning algorithms. However, since my model had already achieve maximum performance for this task, there was no need to explore it further.

## Setup Instructions

To reproduce this model on a Mac, clone the <a href="https://github.com/udacity/deep-reinforcement-learning">Udacity DRLND repo</a>, then place the reacher.app file in the p2_continuous-control folder. Also place the Juputer notebook there and run it.
