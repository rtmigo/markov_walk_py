# [markov walk](https://github.com/rtmigo/markov_walk#readme)
[![Actions Status](https://github.com/rtmigo/markov_walk/workflows/unit%20test/badge.svg?branch=master)](https://github.com/rtmigo/vien/actions)
[![Generic badge](https://img.shields.io/badge/Python-3.8+-blue.svg)](#)

This module solves a particular mathematical problem related to probability theory. 

Let's say a completely drunk passenger, while on a train, is trying to find his car. The cars are connected, so he wanders between them. Some transitions between cars attract him more, and some less.

At the very beginning and at the very end of the train there are ticket collectors: if they meet a drunkard, they will kick him out of the train.

We know which car the drunkard is in now. Questions answered by the module:

- What is the probability that he will be thrown out by the ticket collector standing at the beginning of the train, and not at the end?

- What is the likelihood that he will at least visit his car before being kicked out?

# Stricter problem statement

Suppose we are dealing with a discrete 1D random walk. At each point, we have different probabilities of
making step to the left or to the right.


|Points        | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|--------------|---|---|---|---|---|---|---|---|
|P(move right) |...|0.3|0.5|0.7|0.4|0.8|0.9|...|
|P(move left)  |...|0.7|0.5|0.3|0.6|0.2|0.1|...|


For example, the probability to get from point 3 to point 4 is 0.7, and the probability to get from same
point 3 to 2 is 0.3.

In other words, it is like a Markov chain: states are points; transitions are possible only between
neighboring states; all transition probabilities are known.

Suppose the motion begins at point 3. How can we calculate the probability that we will get to point 7
before we get to point 0?

# The code

The described problem is well modeled by [Absorbing Markov chains](https://en.wikipedia.org/wiki/Absorbing_Markov_chain).
The code performs the necessary matrix calculations and returns the answers as `float` numbers. 

```python3
from markov_walk import MarkovWalk

step_right_probs = [0.3, 0.5, 0.7, 0.4, 0.8, 0.9]
walk = MarkovWalk(step_right_probs)
```

So now

`ever_reach_probs[startPos][endPos]` is the probability, that after
infinite wandering started at `startPos` we will ever reach the point `endPos`.

`walk.right_edge_probs[pos]` is the probability for a starting point `pos`, that after infinite wandering we will leave 
the table on the right, and not on the left.

By positions we mean indexes in `step_right_probs`.

