# Report

## Algorithm
Hyperparameters and special algorithmic features are printed in **bold**.

This project uses a Deep Deterministic Policy Gradients (DDPG) algorithm
That means two neural networks are used:
 1. The actor approximates the Q-table and thus predicts the best action given a state.
 2. The critic predicts the maximum possible target that the action predicted by the actor will grant for the given state.
    This target drives the learning for both, the critic as well as the actor.

For details on the neural network architecture see the section below.

The algorithm runs for at most **`n_episodes = 1000`** episodes.
In each episode the environment is reset and then actions are applied until the environment yields that it is done.
In each timestep for each of the 20 agents an action is generated based on the weights of the current actor network.
This means, the current policy is applied.

Some noise is added to the action in order to randomly alter it.
Allows to further explore the action space.
Then, the action is applied in the environment and the corresponding award and new state are observed.
The corresponding `(state, action, reward, next_state, done)` tuple is enqueued into an **Experience Replay** buffer which has a maximum length of **`replay_buffer_size = 100000`**. 
The idea is to have a limited memory of past experiences that we can learn from again.
Since it's limited, the quality of the stored experiences will increase over time.

A random sample of size **`batch_size = 128`** is drawn from the experience replay buffer.
The algorithm uses two identical networks of each, the actor and the critic, to apply the **Fixed Q-Targets** technique.
This technique decuples the input wheights from the wheight adjustment due to learning:
The "next states" (s') of the sample batch are fed into the secondary ("fixed") actor network.
The resulting action, together with the next state, are fed into the secondary ("fixed") critic network.
Together with the reward and an applied discount rate of **`gamma = 0.99`**, this gives the target values.

The primary critic network is fed with the batch of current states and actions.
This result and the previously generated target values are passed to the loss function (**medium quared error** is used in this project) to calculate an error for the critic.
This error is used to calculate the gradients needed to update the weights of the primary critic network.
The learning rate was chosen to be **lr=0.001**.
Then, the primary actor network is fed the the batch of current states and the result is again passed, together with the states, to the primary critic.
The result is used as the primary actor's loss function that is needed for the gradients to update the weights of the primary actor network.
The learning rate here was also chosen to be **lr=0.001**.

Lastly, the wheights of the fixed networks are actually not fixed, but are slightly updated with the new wheights of the primary networks.
The factor of **`tau = 0.001`** denotes the portion of the new weights that are interpolated into the old ones.

### The neural network architectures
The actor network used in this solution has three layers:
  1. A fully connected layer with 250 units
  2. A fully connected layer with 150 units
  3. An output layer with 4 units, which is the size of the action space
  
Between the layers 1 and 2 and between the layers 2 and 3 ReLU activation and dropout with a rate of 0.2 is applied.
The final activation function is `tanh` whose output is always between `-1` and `1`, just as needed for the given action space.

The critic network also three layers:
  1. A fully connected layer with 250 units
  2. A fully connected layer with 150 units
  3. An output layer with 4 units, which is the size of the action space
  
The critic also feeds the state into the first layer and applies relu and dropout of 0.2, 
but then the output of the first layer is concatenated with the action before it is fed into the second layer.
Then, again ReLU and dropout are applied and the result is passed to the last layer.
Here, no activation function is applied, because the critic must generate an arbitrary number.

### Training progress
![graph](scores.png?raw=true "scores")

## Output
Episode 100	Average Score: 10.14
Episode 165	Average Score: 29.97
Environment solved in 166 episodes!	Average Score: 30.24
Episode 200	Average Score: 32.57
Episode 210	Average Score: 32.61 

So, the goal is already reached after only ~160 episodes.

## Possible future Enhancements
In general, very many different combinations of the hyperparameters can be tested and evaluated.
As part of this, also different network architectures can be used.
Also, other actor critic methods could be given a try like:
 - A3C
 - A2C
 - GAE
