# deep_rl_continuous_control

DRL Agent to controll a double-jointed arm that can move to target locations


# Project: Control 20 double-jointed arms

### Intro

[//]: # (Image References)

[image1]: https://s3.amazonaws.com/video.udacity-data.com/topher/2018/June/5b1ea778_reacher/reacher.gif "Trained Agent"


This projects goal is to utilize Deep Reinforcement Learning (DRL) to train an agent to control 20 double-jointed arms so that they continue reaching a moving target location.

Trained agents  can be seen in below image: 

![Trained Agent][image1]

(source: https://s3.amazonaws.com/video.udacity-data.com/topher/2018/June/5b1ea778_reacher/reacher.gif)

## Environment
### Action space
At each timestep the agent has to choose take a 4 dimensional action, all values between `-1` and `1`:

### State space

The state space is a continuous 33 dimensional vector that contains e.g. the position, rotation, velocity, and angular velocities of the arm.

A reward of `+0.1` is provided for each step that the agent's hand is in the goal location.

The task is episodic. The problem is considered solved once the average score of the last 100 episodes is greater than 30.

## How to use

### Installing dependencies

- Download ML-Agents Toolkit beta 0.4.0a.
   - https://github.com/Unity-Technologies/ml-agents/releases/tag/0.4.0a
- install it by running following command, with activated conda environment in the directory of ml-agents, that contains the setup.py.
   - pip install -e . .
- install PyTorch.
    - conda install pytorch-cpu torchvision-cpu -c pytorch
- Get Unity Environment designed for this project.
   - Linux: [click here](https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Linux_NoVis.zip).
    - Place the file in the DRLND GitHub repository, in the `deep_rl_continuous_control/` folder, and unzip (or decompress) the file. 

### Instructions

Follow the instructions in `Continuous_Control.ipynb`. It also provides links to the necessary installation precedure.
Then, execute the cells to train the agent.

### Expected Result
After approximately 200 episodes the agents reach the goal of a score of 30 on average.
But it runs even further and reaches better results.
