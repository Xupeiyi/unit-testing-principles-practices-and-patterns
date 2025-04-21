# Chapter 1. The goal of unit testing
How to get the best returns out of the efforts put into testing?

## The goal of unit testing
Better code design is not the primary goal but just a pleasant side effect.  
The goal is to enable sustainable growth of the software project, to make existing functionality work even after introducing new features - so it's better to start testing from projects under active development?  
Not all tests are created equal. Don't write tests just for the sake of writing tests. Consider their value and the upkeep cost.

## Using coverage metrics to measure test suite quality
Low coverage is bad. High coverage is not necessarily good. Some possible outcomes and code path in external libraries might not be tested. So do not use test coverage as a goal.

## What makes a successful test suite?
- integrated in the development cycle.
- targets only the most important part of the code base.
- provides maximum value with minimum maintenance costs.
