Tags: #reference 
Created: 2023-04-04 19:04

# CS50 Introduction to Artificial Intelligence with Python

# Introduction
First lecture in the course.

## Search
Search is a type of problem in which the artificial intelligence is looking to solve a problem (i.e. reach a desired state). For example, ordering a sliding puzzle in a desired order or solving a maze (taking the right moves to reach an end state).

### Notions
**agent** = an entity that perceives its environment and acts upon that environment
**state** = a configuration of the agent and its environment
**initial state** = the state in which the agent begins
**actions** = choices that can be made in a state

**Note:** the actions that we can take will be modelled as functions - `actions(s)` returns the set of actions that can be executed in state `s`.

**transition model** = a description of what state results from performing any applicable action in any state

**Note:** Also modelled as a function - `result(s, a)` returns the state resulting from performing action `a` in state `s`.

**state space** = the set of all states reachable from the initial state by any sequence of actions
**goal test** = way to determine whether a given state is a goal state
**path cost** = numerical cost associated with a given path

Having defined all of those notions, we can know that we are dealing with a **search problem** when we have the followign elements:
- initial state
- actions
- transition model
- goal test
- path cost function

The end goal of solving a search problem is finding a **solution** (*=a sequence of actions that leads from the initial state to a goal state*) AND IDEALLY, we want to find the **optimal solution** (*=a solution that has the lowest cost among all solutions*). 

In order to solve a search problem we will need to model our data. For that we need a data structure that we will use, a **node** - which keeps track of:
- a state
- a parent (node that generated this node)
- an action (action applied to parent to get node)
- a path cost (from initial state to node)

### Approach
The way you can approach this type of problem is by following an algorithm:
- Start with a frontier that contains the initial state
- Repreat:
	- If frontier is empty, then there is no solution
	- Remove a node from the frontier
	- If node contains goal state, return solution
	- Expand node, add resulting nodes to the frontier

**frontier** = all the states that we can reach at one point in the execution of the algorithm
**expand** = get all the nodes that you can reach from the current node

## Search Algorithms
Mainly, there are two search algorithms, or two ways to implement the algorithm described in the *Approach* section:
- Depth-First Search (**DFS**)
- Breadth-First Search (**BFS**)

#### Intuitive comparison
Intuitively, the difference between the two of them is as follows: *DFS* picks a path and follows it until it reaches a dead-end or the goal. In case it reaches a dead-end, then it returns to the most recent decision point and picks a path that was not already picked.

On the other side, *BFS* does not commit to a single path: it takes one step on all the paths available - it first explores the nodes that are *1-away* from the start, then it explores the ones that are *2-away* and so on...

One advantage that *BFS* has over *DFS* is that it's guaranteed to find the shortest path from the start node to the goal.

#### Implementation comparison
From the point of view of the implementation, the only difference is the data structure you use to store the nodes that are in the *frontier*:
- For *DFS* you would use a **stack**
- For *BFS* you would use a **queue**

## Informed Search Algorithms
Up until now, we discussed DFS and BFS, which are **uninformed search algorithms**, which means that they make their decisions based on a pre-determined strategy that *does not take into account any **problem specific information***.

**Informed search algorithms** on the other side take into account some problem specific information when they pick what node to explore out of the frontier. In order to make the decision, they use a **heuristic function** (often called *h(n)*), which takes a state as an input and returns how close **we think** the state is to the goal state.

And example of an informed search algorithm is **Greedy Best-First Search** (GBFS).

### Greedy Best-First Search
This algorithm always picks the node with the *lowest h(n) value* to explore next. The efficiency of the algorithm is therefore given by the quality of the heuristic function (how well we manage to estimate the closeness of a node to the goal).

This algorithm is **not** guaranteed to find the shortest path.

### A* Search
Unlike GBFS, **A* search** uses more information to figure out which node to expand/pick as the next step out of the frontier: *it's a search algorithm that expands the node with the lowest value of g(h) + h(n)*, where **g(n)** is the cost to reach the node and **h(n)** is the estimated cost to goal.

A* search will find the optimal solution if the heuristing function respects the rules:
- It's admissible (never overestimates the true cost)
- It's consistent (for every node *n*, and successor *n'* with step cost *c*, *h(n) <= h(n') + c*)

## Adversarial Search
Soemtimes, we can find ourselves in situations where I am an agent that is trying to find an optimal solution, but at the same time, there is another agent that is fighting against me and trying to find an optimal solution for himself. This type of problem is called an **adversarial search** algorithm.

We will use a game of tic-tac-toe as an example.

### Minimax
The first thing that we need to do is to find a way to numerically encode a winning/losing/draw state of the game: *-1 means that O won; 0 means that it's a draw; and 1 means that X won*.

In this algorithm, there are two agents:
- **MAX** (or X), who aims to maximize the score
- **MIN** (or Y), who aims to minimize the score

This algorithm will use the following notations to represent certain aspects of the game:
- *S0*: initial state - *PAYLER(s)*: returns which player to move in state *s*
- *ACTIONS(s)*: returns all legal moves in state *s*
- *RESULT(s, a)*: returns state after action *a* is taken in state *s*
- *TERMINAL(s)*: checks if state *s* is a terminal state
- *UTILITY(s)*: final numerical value for terminal state *s*

Having decided upon these notions, the algorithm can be represented as such - given a state *s*:
- **MAX** picks action *a* in *ACTIONS(s)* that produces highest value of *MIN-VALUE(RESULT(s, a))*
- **MIN** picks action *a* in *ACTIONS(s)* that produces  smallest value of *MAX-VALUE(RESULT(s, a))*,
Where the two functions are:

```
function MAX-VALUE(state):
	if TERMINAL(state):
		return UTILITY(state)
	  
	v = -inf
	for action in ACTIONS(state):
		v = MAX(v, MIN-VALUE(RESULT(state, action)))
	return v
```

```
function MIN-VALUE(state):
	if TERMINAL(state):
		return UTILITY(state)
	
	 v = inf
	 for action in ACTIONS(state):
		 v = MIN(v, MAX-VALUE(RESULT(state, action)))
	 return v
```

### Optimizations
As games get more complex, this algorithm can start to take up a lot more memory and a lot more time to compute a *next move*. Therefore, it's a good idea to think of optimizations to this algorithm.

One such optimization is called **alpha-beta pruning**, which tries to reduce the number of computations we do by keeping track of the best and worst possible outcomes at a certain move.

Another optimization would be to instead of simulate the entire game each time, to just simulate a couple moves (an arbitraty number). The issue that arises then is the fact that we no longer have a simple way to compute the utility of the states of the game (in case we reach a non-terminal state), which means that we will need a way to *estimate the utility of any given state of the game*.

The **evaluation function** is exactly that: *a function that estimates the expected utility of the game from a given state*.

This final optimization modifies the algorithm in what is called **depth-limited minimax**.

# Knowledge
People know stuff about the word and based on that information, they *draw conclusions* about the world.

This is the exact thing that we will try to do using computers - we will build

**knowledge-based agents** = agents that reason by operating on internal representations of knowledge

## Notation
First we will need to discuss some symbols and notions:
- *Proposition symbols*: they can be anything, generally letters (`P`, `Q`, `R`) and they represent a statement about the world, or **sentence** (an assertion about the world in a knowledge representation language)
- *Logical connectives*: they are the things that connect sentences. The most common of them are `not`, `and`, `or`, `implication` and `biconditional`; all of which have a truth table that represents the way they connect sentences

Other notions:
- **model**: assignment of a truth value to every propositional symbol
- **knowledge base**: set of sentences known by a *knowledge-based agent*
- **entailment**: `a |= b` is read as ***a** entails **b*** and is defined as *"in every model in which sentence **a** is true. sentence **b** is also true"*
- **inference**: the process of deriving new sentences from old ones

Our goal when solving a knowledge problem is to answer the following question: *Does `KB |= a`?*, where `KB` is our knowledge base and `a` is a sentence that we want to find out if it's true.

## Model Checking
This is an algorithm that can be used to solve inference problems. Here's what the steps are:
- To determine if KB |= a:
	- Enumerate all possible models
	- If in every model where *KB* is true *a* is true, then *KB entails a*
	- Otherwise, KB does not entail a

## Knowledge Engineering
This is the process of trying to take a problem and representing it in logical statements which get thrown into a knowledge base (so that a computer can undesrtand them), meaning that they can then be used in inference algorithms to solve problems.

## Inference Rules
There are certain rules that we can use to obtain new information out of the statements that we already have.

Using those rules, we can use the same algorithms that we used in search problmes to solve knowledge problems. Here's an analogy netween search and knowledge problems (where we apply a technique called **theorem proving**):
- initial state: starting knowledge base
- actions: inference rules
- transition model: new knowledge base after inference
- goal test: check statement we are trying to prove
- path cost function: number of steps in proof

**clause** = a disjuction of literals (a bunch of symbols with an *or* between them)
**conjunctive normal form** = logical sentence that is a conjunction of clauses (an *and* of *or*s)

### Conversion to CNF
There's a certain algorithm that uses the inference rules to convert any statement into a CNF:
- eliminate biconditionals
- eliminate implications
- move negations inwards using **De Morgan's Laws**
- use distributive law to distribute *or* wherever possible

## Inference by Resolution
Another algorithm that we can use to determine if KB |= a is called *resolution*:
- to determine if KB |= a
	- convert *(KB & ~a)* to CNF
	- keep checking to see if we can use resolution to produce a new clause
		- if ever we produce the empty clause (equivalent to false), we have a contradiction, and KB |= a
		- otherwise, if we can't add new clauses, no entailment

## First-Order Logic
Propositional logic (the one we've been using up until now) has certain limitations and can get hard to use to represent certain statements. This is a problem that first-order logic solves.

In first-order logic we have two types of symbils: *constant* and *predicate*. It also introduces some more concepts to which we can apply the concepts from propositional logic:
- universal quantification
- existential quantification

# Uncertainty
Sometimes we won't be only working with events that are either true or false. In times like these, we don't know exactly whether something happens or it does not happed, therefore we need to introduce a way to model uncertainty.

The simplest way to do this is by using probabilities. Most of the times, probability won't be enough, so we'll need to also introduce **conditional probability**, for example *the probability that a disease manifests given some test results* - P(disease/test results).

## Random Variables
A variable in probability theory with a domain of possible values it can take on. For example, a random variable can be *Flight* and the values it can take are *{on time, delayed, cancelled}*.

Having a random variable, describing the values of each value it takes is called a **probability distribution**. Using the same *flight* example:

```
P(Flight = on time) = 0.6
P(Flight = delayed) = 0.3
P(Flight = cancelled) = 0.1
```

**Independence** is the knowledge that one event occurs does not affect the probability of the other event.

## Bayes' Rule
This is a theorem that is really common in statistics and anywhere probabilities are used, and it is really handy for conditional probabilities.

Here's an example of what we can do with it: *Knowing **P(medical test result | disease)**, we can calculate **P(disease | medical test result)***.

## Join Probability
This is a probability distribution, only for two random variables at one time. These can be represented in a table.

Here's an example: *Given we have two random variables **rain** and **cloud**, a joint probability would be having the probabilities of the following events happening: **cloud & rain**, **cloud & ~rain**, **~cloud & rain** and **~cloud & ~rain***.

## Probability Rules
There are many rules that serve as the foundation that we will use in our AI algorithms. The names of the ones presented in the lecture:
- Negation
- Inclusion-Exclusion
- Marginalization
- Conditioning

These rules and some others that were presented before, such as *Bayes' rule* will be used internally by the following algorithms.

## Bayesian Networks
Long story short, a Bayesian network is descrobed as such:
- directed graph
- each node represents a random varaible
- arrow from *X* to *Y* means that *X* is a parent of *Y*
- each node *X* has a probability distribution **P(X | Parents(X))**

### Example
The random variables are as such: *Rain {none, light, heavy}*, *Maintenance {yes, no}*, *Train {on time, delayed}*, *Appointment {attend, miss}*.

The dependencies are as such: *Rain -> Maintenance*, *Rain -> Train*, *Maintenance -> Train*, *Train -> Appointment*.

Also, for each of the random variables we have a conditional probability distribution (conditional on its parents). For example, for *Rain* we can think of a table, its header being *Rain Value; Maintenance Value; on time probability, delayed probability*.

### Inference
Here's what we can do once we have the data described above:
- Query *X*: variable for wh ich to  compute distribution
- Evidence variables E: observed variables for event *e*
- Hidden variables *Y*: non-evidence, non-query variable
- Goal: calculate **P(X | e)**.

Applied to the example problem above, the goal can sound like this: *given that it ls light raining and there is no maintenance, what's the probability distribution of **Appointment***. - **P(Appointment | light, no)**

## Sampling
Since the process of computing a solution with the inference algorithm can be quite expensive, an optimization would be to run a bunch of simulations and then take the collective result as the solution.

There are multiple sampling algorithms, such as **rejection sampling** and **likelihood weighting**

## Markov Models
Sometimes we have to deal with uncertainty over a period of time. For example, we want to predict the weather for the next 10 days.

For such cases we will make the **Markov assumption**: *the assumption that the current state depends on only a finite fixed number of previous states.*

We will use what is called a **transition model**, which is best described with an example: *a table that contains the following probabilities: today rains and yerderday rained; today rains and yesterday was sunny; today is sunny and yesterday rained; today is sunny and yesterday was sunny*.

Using this transition model we can generate what is called a **Markov chain**.

## Hidden Markov Models
We can't observe the state of the world (in our example whether it it raining or not), but we can observe some *hidden state* (such as whether people are carrying unbrellas ur not).

We need to make an assumption (**sensor Markov assumption**): *the assumption that the evidence variable (the thing that we observe) depends only on the corresponding state*.

We will also need something else besides the **transition model**: the **sensor model** - *what's the probability that today is raining if people are carrying umbrellas and if they are not; same for probability of it being sunny*.

In this model, there are certain tasks that are defided and can be solved (by a library for example):
- **filtering** - given observations from start until now, calculate distribution for current state
- **prediction** - given observations from start until not, calculate distribution for a future state
- **smoothing** - given observations from start until now, calculate distribution for past state
- **most likely exaplanation** - given observations from start until now, calculate most likely sequence of states

# Optimization
This is another type of problem that you might encounter in the field of AI. They show some similarities to *search problems*, in that there are multiple states and you switch from one to another via actions you take, however the end goal **is not knwon**, you have to find it.

An example of a problem is: *you have a city with houses placed all over the place, you need to place two hospitals so that the accumulated distance from the houses to the hospotals is minimal.*

## Local Search
In this type of algorithm you start in a state, only keep track of the current state you are in and pick an action to make so that you move to the next state.

An example is **hill climbing**:
```
function HILL-CLIMB(problem):
	current = initial state of problem
	repreat:
		neighbor = highest valued neighbor of current
		if neighbor not better than current:
			return current
```

The iissue with this type of algorithm is that you are not guaranteed to find the maximum. You may get stuck in a **local maximum**.

Another algorithm for solving a local search problem that can lead to better resolts than *hill climbing* is called **simulated annealing**. This one can lead to solving the issue created by local minimums and, in principle, works as follows:
- Early on, higher *temperature*: more likely to accept neighbors that are worse than current state
- Later on, lower *temperature*: less likely to accept neighbors that are worse than current state

## Linear Programming
This is another type of problem and has the following requirements:
- Minimize a cost function *c1\*x1 + c2\*x2 + ... + cn\*xn*
- With constraints of form *c1\*x1 + c2\*x2 + ... + cn\*xn <= b* (or with equality)
- With bounds for each varaible *li <= xi <= ui*

#### Example
Two machines X1 and X2. X1 costs $50/hour to run, X2 costs $80/hour to run. Goal is to minimize cost.

X1 requires 5 units of labor per hour. X2 requires 2 units of labor per hour. Total of 20 units of labor to spend.

X1 produces 10 units of output per hour. X2 produces 12 units of output per hour. Company needs 90 units of output.

## Constraint Satisfaction
Yet another type of problem. Here's the requirements to categorize a problem as **constraint satisfaction**:
- Set of varaibles {X1, X2, ..., Xn}
- Set of domains for each variable {D1, D2, ..., Dn}
- Set of constraints C

#### Example
Sudoku:
- {(0, 0), (0, 1), ...} (each square)
- {1, 2, 3, 4, 5, 6, 7, 8, 9} for each variable
- {(0, 0) != (0, 1) != (0, 2), ...}

#### Solving
CSPs can be considered **search problems**. Here's how:
- *initial state*: empty assignment (no variables)
- *actions*: add a *{variable = value}* to assignment
- *transition model*: shows how adding an assignment changes the assignment
- *goal test*: check if all variables are assigned and constraints are all satisfied
- *path cost function*: all paths have same cost (we don't care)

Since they CSPs can be considered search problems, all the algorithms that can be applied to solve search problems can also be applied for CSPs (such as *BFS*, and *DFS*). However, they are not optimal and since CSPs are more specialized search problems we can use more information for a better algorithm: **backtracking search**.

# Learning
We don't tell the computer exactly what to do, but we give it access to **information**, from which it has to deduct information, find patterns and ultimately **learn** from it.

There are multiple types of machine learning, some of which being:
- **Supervised learning**
- **Reinforcement learning**
- **Unsupervised learning**

## Supervised Learning
Given a data set of input-output pairs, learn a function to map inputs to outputs.

### Classification
Supervides learning task of learning a function mapping an input point to a discrete category.

#### Nearest-Neighbor Classification
Algorithm that, given an input, chooses the class of the nearest data point to that input.

A variation of this algorithm is the **k-Nearest-Neighbor Classification** algorithm, where given an input, it chooses the most common class out of the *k* nearest data points to that input.

#### Perceptron
This is yet another algorithm that can be used to classify data. It has a certain function to update each weight of a function given a data point *(x, y)*.

#### Support Vector Machines
This algorithm uses a **maximum margin separator** to split the data in multiple categories. It represents a boundary that maximizes the distance between any of the data points.

### Regression
It is a supervised learning task of learning a function mapping an input point to a consinuous value.

You basically generate a hypothesis and evaluate how good it is based on a **loss function**: a function tha expresses how poor hypothesis performs.

*Supervised learning* problems have a problem that may arise that is called **overfitting**: a model that fits too closely to a particular data set and therefore may fail to generalize to future data.

For this reason, the concept of **regularization**: penalizing hypotheses that are more complex to favor simpler, more general hypotheses.

A practice that makes it less likely that you *overfit* your model is **holdout cross-validation**: splitting data into a training set and a test set, such that learning happens on the training set and is evaluated on the test set.

Another type of *cross-validation* is **k-fold cross-validation**: splitting data into *k* sets, and experimenting *k* times, using each set as a test set once, and using remaining data as training set.

### Reinforcement Learning
Given a set of rewards of punishments, learn what actions to take in the future.

The general flow of such an algorithm is:
- There is an environment that has multiple states, of which the agent (AI) is put into an initial one
- Agent can take an action, which wil lresult into the change of the state it is in
- Based on the action it took and the state it got into, the agent receives a reward.

#### Markov Decision Proces
This is a way to model the *world* of reinforcement learning:
- Set of states **S**
- Set of actions **ACTIONS(s)**
- Transition model **P(s' | s, a)**
- Reward function **R(s, a, s')**

#### Q-Learning
This is a method for learning a function *Q(s, a)*, esimate of the value of performing action *a* in state *s*.

### Unsupervised Learning
Given input data without any additional feedback, learn patterns.

#### k-Means Clustering
An algorithm for clustering data based on repretedly assigning point sto clusters and updating those clusters' centers.

**Clustering** is exactly what the definition of *unsupervised learning* described: finding patterns.

# Neural Networks
...

# Resources
CS50 Introduction to Artificial Intelligence with Python 2020