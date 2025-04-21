# Chapter 5. Mocks and test fragility?

## 5.1 Differentiating mocks from stubs

### 5.1.1 The types of test doubles
There are five kinds of test doubles: 
- Mock: mock, spy. The SUT changes the mock's inner state.
- Stub: stub, dummy, fake. The SUT gets input data from the stub. 

### 5.1.2 Mock (the tool) vs. mock (the test double)
Mock (the tool) can be a class provided by the testing library. Tester can use a Mock (the tool) to create both mocks (the test double) and stubs.

### 5.1.3 Don't assert interactions with stubs
Calling a stub to get data is just an internal implementation detail and should not be specifically checked.

### 5.1.4 Using mocks and stubs together
When a test double is both a mock and a stub, it's still called a mock.

### 5.1.5 How mocks and stubs are relate to commands and queries
Commands produce side effects and don't return any value. Mocks substitute commands.   
Queries are side-effect free and return a value. Stubs substitute queris.

## 5.2. Observable behavior vs. implementation details
Tests must verify the observable hehavior and keep distance with implementation details.

### 5.2.1 Observable behavior is not the same as a public API
The code is either an internal implementation or an observable behavior.
For code to be observable behavior, it must help the client to reach its goals by:
- expose an operation (calculation and or side effect)
- expose a state

