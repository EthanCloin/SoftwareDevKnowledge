Tackling Complexity at the Heart of Software - Eric Evans
# Part II - The Building Blocks of a Model-Driven Design
Devs need to understand the fundamentals of Model-Driven Design. 
Isolate the domain design from other concerns in the software system. This will clarify the connection between the domain and model elements.
# Chapter 4: Isolating the Domain
When domain-related code is mixed in with other concerns as part of the UI or Database, it becomes difficult to reason about. 
Complex tasks require [[separation of concerns]].
Principle of [[Layered Architectures]] is that each element depends only on other elements in the same 'layer' or those beneath it. Many designs include the following 4 layers:
1. Presentation/UI Layer - Displays information
2. Application Layer - Defines jobs the software ought do. Thin and lacking business logic. Role is to delegate and coordinate Domain objects.
3. Domain/Model Layer - Business logic containing state that reflects the business situation.
4. Infrastructure Layer - Technical capabilities like message sending + persistence.

Domain models should not be burdened with the responsibility of displaying themselves, storing themselves, or deciding application tasks. 
## Relating the Layers
Layers ought to be loosely coupled and have one-way dependencies.
[[Model-View-Controller]] is a popular pattern for connecting the UI and application.
In the example of an email service, the Application layer should know **when** to send a message, and Infrastructure layer should know **how**.
## Architectural Frameworks
Selectively apply only the most helpful parts of frameworks which are helpful for the team. 
Be wary that an elaborate framework can hamper a developer's ability to understand the application.
## The Domain Layer Is Where the Model Lives
Domain layer is the design and implementation of business logic.
## The Smart UI "Anti-Pattern"
SMART UI is when the business logic is engineered in the UI. This is considered an anti-pattern if the goal of the application is to deploy Domain-Driven Design. But it has its uses. Allows for fast prototyping, quick results, and allows less experienced devs to contribute meaningfully without training.


