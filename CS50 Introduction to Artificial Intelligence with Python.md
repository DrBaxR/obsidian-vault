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
...

# Resources
CS50 Introduction to Artificial Intelligence with Python 2020