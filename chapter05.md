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

### 5.2.2 Leaking implementation details: An example with an operation
In this example, the method NormalizeName shouldn't be exposed to the client.
```c#
public class UserController
{
    public void RenameUser(int userId, string newName)
    {
        User user = GetUserFromDatabase(userId);

        string normalizedName = user.NormalizeName(newName);
        user.Name = normalizedName;

        SaveUserToDatabase(user);
    }
}
```
A rule of thumb: ideally, any individual goal should be achieved with a single operation.

### 5.2.3 Well-designed API and encapsulation
Encapsulation protect the goal against invariant violations. Exposing implementation details often leads to invariant violations.

Martin Fowler's tell-don't-ask principle: rather than asking an object for data and acting on that data, tell an object what to do. 

### 5.2.4 Leaking implementation details: An example with state
In this example, the state of sub-renders shouldn't be exposed to the client.
```c#
public class MessageRenderer : IRenderer
{
    public IReadOnlyList<IRenderer> SubRenderers { get; }

    public MessageRenderer()
    {
        SubRenderers = new List<IRenderer>
        {
            new HeaderRenderer(),
            new BodyRenderer(),
            new FooterRenderer()
        };
    }

    public string Render(Message message)
    {
        return SubRenderers
            .Select(x => x.Render(message))
            .Aggregate("", (str1, str2) => str1 + str2);
    }
}
```
Good unit tests are intrinsically connected to good APIs. By making implementation details private, the tests can only verify observable behaviors.

## 5.3 The relationship between mocks and test fragility

### 5.3.1 Definning hexagonal architecture

The application service layer and the domain layer forms a hexagon, which represents the application. It emphasize three guidelines:
- The domain layer only contains business logic. The application layer translates income requests into operations of the domain layer, persists or return the results.
- The application layer depends on the domain layer, but not the otherwise.
- External applications don't have direct access to the domain layer.

The principles of well designed API has a fractal nature. Therefore the tests are fractal, covering big goals and smaller subgoals.  
The observable behavior of a domain class helps the application service reach a goal; That of an application service helps an external client.

Tests should have a connection to business requirements because those tests tie to observable behavior only.

### 5.3.2 Intra-system vs. inter-system communications
Intra-system communications are implementation details. Inter-system communications are not.
Mock is useful to verify intra-system communications. When it's used for inter-system communications, it gets coupled to implementation details.

### 5.3.3 Intra-system vs. inter-system communications: An example
Imagine the following business use case:
- A customer tries to purchase a product from a store
- An email receipt is sent to the customer
- A confirmation is returned

The call to the SMTP service is a legitimate reason to do mocking.   
Removing the product from the inventory is not directly connected to the user's goal, and is an implementation detail. It shouldn't be mocked.

## 5.4 The classical vs. London schools of unit testing, revisited
The London school uses mocks without differentiating intra-system and inter-system communications. It can lack resistence to refactoring.

### 5.4.1 Not all out-of-process dependencies should be mocked out
If a shared dependency is out-of-process (database, message bus, etc.), testing in parallel become complicated, because it's slow to instantiate a new database/message bus before each test suite. The usual approach is to replace such dependencies with test doubles.

An out-of-process dependency that can't be observed externally acts as part of the application and shouldn't be mocked. For example, a database that's only used by this single application. Mocking out-of-process dependencies that can be fully controlled also leads to brittle tests.

### 5.4.2 Using mocks to verify behavior
Mocks have something to do with behavior only when they verify interactions that cross the application boundary and only when the side effects of those interactions are visible to the external world.