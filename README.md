# N-model processing network control via Deep Reinforcement Learning

The repository implements Proximal policy optimization algorithm to optimize N-model control.

## Setup

Python 3.10 or newer is required (tensorflow 2.21 declares `Requires-Python >=3.10`; on Python 3.9
the interpreter aborts at `import tensorflow`). Install the dependencies into a virtual environment:

```
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

`train.py` runs the policy optimization and `value_iteration_Nmodel.py` runs the value iteration.
Both are run from the repository root, because `check_optimality` loads `action09.npy` relative to
the working directory. `train.py` simulates the episodes on ray actors, so the ray cluster is
started automatically.

## N-model
The N-model processing network was first proposed in [1]. It is a processing network system with two independent Poisson input arrival flows, two servers, exponential service times, and
linear holding costs. We use uniformization to convert the continuous-time control problem to a discrete-time control problem.\
The detailed description of the system and its uniformization can be found in Section 5.3 of [2].\
File `NmodelDynamics.py` contains an object describing the N-model. Its method `next_state_N1` generates the next state of the system given the current state and the action.  

## Proximal policy optimization
We use Proximal policy optimization algorithm [3] for N-model control optimization. The main file is `train.py` which starts with hyperparaments selection.
File `actor_utils.py` contains actor-related objects and functions, file `value_function.py` contains critic-related objects and functions.

## Value iteration
File 'value_iteration_Nmodel.py' can be used to find optimal actions for the N-model via the value iteration method. File 'value_iteration_Nmodel.py' should be run separately. File `action09.npy` contains a numpy array with optimal actions for N-model with load 0.9. 


## References
[1] J Michael Harrison. Heavy traffic analysis of a system with parallel servers: asymptotic optimality
of discrete-review policies. The Annals of Applied Probability, 8(3):822–848, 1998.\
[2] J. G. Dai, Mark Gluzman. Queueing Network Controls via Deep Reinforcement Learning. https://arxiv.org/abs/2008.01644, 2021.\
[3] John Schulman, Filip Wolski, Prafulla Dhariwal, Alec Radford, and Oleg Klimov. Proximal policy
optimization algorithms. http://arxiv.org/abs/1707.06347, 2017
