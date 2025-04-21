# Chapter 2. What is a unit test?

## The definition of "unit test"
A unit test
- verifies a small piece (unit) of code
- quickly
- in an isolated manner

What does isolated mean? The classical approach thinks it means running one unit test should not affect the result of other unit tests; The London school thinks it means the object under test should be isolated by replacing everything else with mocks.


Following the classical approach, to run unit tests in parallel, we should remove shared dependencies.