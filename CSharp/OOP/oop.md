### Object-oriented programming

### Identifying classes

#### Layering the Application

Build with a layered structure

Layering is key to a good application structure.

Solution -> application

Project -> layer



User Interface layer

Business logic layer

Data access layer

Common library



Overloading

same name, but different parameters



#### Unit Testing Methods

Define tests for valid and invalid scenarios

Organize the test

- Arrange: Set up the test
- Act: Call the method being tested
- Assert: Determine the result

#### Method Terminology

Signature

Overloading





### Separation of Responsibilities

Minimizes coupling
Maximizes cohesion
Simplifies maintenance
Improves testability

#### Checklists and Summary

##### Evaluate Coupling

* What: Dependence on other classes or external resources
* How: Extract dependencies into their own classes
* Why: Easier to test and maintain
* Example: Move the responsibility for accessing the data store to a repository class

##### Evaluate Cohesion

* **What**: Class members should relate to the class purpose
* **How**: Extract unrelated members into their own classes
* **Why**: Easier to understand, test and maintain

* **Example**: Move the responsibility for managing addresses into a separate class



### Establishing Relationships





### Leveraging Reuse through inheritance





