# BirdsEye

### Localizing Radio Frequency Targets Using Reinforcement Learning
The BirdsEye project demonstrates the simulated
tracking of radio frequency (RF) signals via
reinforcement learning (RL) techniques implemented
on low-fidelity sensors. 
This permits the in-situ observation-training RL paradigm without the need
for significant compute hardware such as graphical
processing units (GPU).
Instead, these methods can
be run on low-cost, commercial, off-the-shelf
technology, providing capabilities to applications in
which covert or concealed sensors are paramount in
deployment, or where more sensitive sensors do not
function or cannot be installed due to the nature of
the environment.

### Methods
BirdsEye has implemented two statistical methods which drive how the sensor adaptively
tracks an observed target signal: Monte Carlo Tree Search (MCTS) and Deep Q-Learning
(DQN). While each method has advantages over the other, neither requires heavy
compute resources such as a GPU. The MCTS method performs a stochastic search and
selection of the actions available to the sensor, identifying the decision which maximizes
the return on localization rewards. The DQN method is a reinforcement learning algorithm
which can adapt to large decision spaces using neural networks, with major public
successes such as [DeepMind’s AlphaGo](https://deepmind.com/research/case-studies/alphago-the-story-so-far).


### Visualization Example
![particles](data/example.gif)


## Usage

### Installation 

```
pip install -r requirements.txt
```


## Usage
### To run on command line
```
$ python run_birdseye.py -h 
usage: run_birdseye.py [-h] -c FILE [-b]

optional arguments:
  -h, --help            show this help message and exit
  -c FILE, --config FILE
                        Specify a configuration file
  -b, --batch           Perform batch run
```

### To run using a Docker container
First install Docker with GPU support. [Instructions here.](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)

A Docker file has also been provided for ease of use. To run with Docker, execute the following commands:
```
> docker build -t birds_eye .
> docker run -it --gpus all birds_eye -c {config.yaml}
```
In order to streamline this process a `Makefile` has been provided as a shorthand. 
```
> make run_mcts
> make run_dqn
> make run_batch
```
Accepted make values are: `run_mcts, run_dqn, run_batch, build`


### Configurations 
See [Configurations Documentation](CONFIGS.md) for more information. 


## Examples
### Run with Monte Carlo Tree Search policy
```
$ python run_birdseye.py -c configs/mcts.yaml 
```
### Run with Deep Q-Network policy 
```
$ python run_birdseye.py -c configs/dqn.yaml 
```

___


![DQN](data/dqn_arch.png)
> Deep Q-Network architecture


## Description
All code for training and evaluation in simulation is contained in the [birdseye](birdseye) directory.  
The [birdseye](birdseye) directory contains some important base classes which can be extended to offer customizability to a specific use case. We provide a few subclasses to get started. 

### [Sensor](birdseye/sensor.py)
The Sensor class defines the observation model for the simulated sensor. Users must define methods for sampling an observation given a state and for determining the likelihood of an observation given a state. We have provided example subclasses for an omni-directional signal strength setup and a bearing based directional setup. 

### [Actions](birdseye/actions.py)
The Actions class defines the action space for the sensor. For computational simplicity, actions are discretized. 

### [State](birdseye/state.py)
The State class includes methods for updating the state variables of the environment. This includes states for both the sensor and target. Motion dynamics and reward functions are defined within this class. We have included example reward functions based on an entropy/collision tradeoff as well as a range based reward. 

### [RFEnv](birdseye/env.py)
The RFEnv class is a Gym-like class for controlling the entire pipeline of the simulation. 

