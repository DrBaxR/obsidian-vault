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

21:20

## Resources
https://learning.edx.org/course/course-v1:HarvardX+CS50AI+1T2020/block-v1:HarvardX+CS50AI+1T2020+type@sequential+block@99364b31367c43e6a1c2146ed9a0b154/block-v1:HarvardX+CS50AI+1T2020+type@vertical+block@abe10e60ff224bd5bc0b33e37ab607cd