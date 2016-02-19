
#### Testing Questions:

* What are some advantages/disadvantages to testing your code?
  * Ans:
    * advantages
      * Encourage you to write testable code, usually more clean.
      * Notify you if you broke something accidently
      * When debugging, unit test can help you to find out root cause
    * disadvantages
      * Need to spend more time, especially if you modify your code often
* What tools would you use to test your code's functionality?
  * Ans: I'll use selenium for integration/acceptance tests.
* What is the difference between a unit test and a functional/integration test?
  * Ans:
    * Unit Tests:
      * Tests the smallest unit of functionality, typically a method/function (e.g. given a class with a particular state, calling x method on the class should cause y to happen).
      * Unit tests should be focussed on one particular feature (e.g., calling the pop method when the stack is empty should throw an InvalidOperationException).
      * Everything it touches should be done in memory; this means that the test code and the code under test shouldn't:
        * Call out into (non-trivial) collaborators
        * Access the network
        * etc
      * Any kind of dependency that is slow / hard to understand / initialise / manipulate should be stubbed/mocked/whatevered using the appropriate techniques so you can focus on what the unit of code is doing, not what its dependencies do.
      * In short, unit tests are as simple as possible, easy to debug, reliable (due to reduced external factors), fast to execute and help to prove that the smallest building blocks of your program function as intended before they're put together.
    * Functional Tests
      * Functional tests check a particular feature for correctness by comparing the results for a given input against the specification.
      * Functional tests don't concern themselves with intermediate results or side-effects, just the result (they don't care that after doing x, object y has state z).
      * They are written to test part of the specification such as, "calling function Square(x) with the argument of 2 returns 4".
    * Integration Tests
      * Integration tests combining the units of code and testing that the resulting combination functions correctly. This can be either the innards of one system, or combining multiple systems together to do something useful.
      * Integration tests can and will use threads, access the database or do whatever is required to ensure that all of the code and the different environment changes will work correctly.
        * If you've built some serialization code and unit tested its innards without touching the disk, how do you know that it'll work when you are loading and saving to disk? Maybe you forgot to flush and dispose filestreams. Maybe your file permissions are incorrect and you've tested the innards using in memory streams. The only way to find out for sure is to test it 'for real' using an environment that is closest to production.
      * The main advantage is that they will find bugs that unit tests can't such as wiring bugs (e.g. an instance of class A unexpectedly receives a null instance of B) and environment bugs (it runs fine on my single-CPU machine, but my colleague's 4 core machine can't pass the tests).
      * The main disadvantage is that integration tests touch more code, are less reliable, failures are harder to diagnose and the tests are harder to maintain.
    * Summary
      * Unit Tests include least code, should also as less as possible so it can be as small/fast as possible. Functional Tests include more code to perform some useful feature. Integration Tests mix most stuffs together including lots of units/functions and even different systems.
      * Unit Tests should be environment independent (by using mock/stub). Functional Tests can use any environment. Integration Tests should use the environment similar to production.
      * Unit Tests used to test various method/status of a unit (probably a class or object). Functional Tests used to test various use case (as more as possible) of a feature. Integration Tests used to test the whole product/system works well.
  * Ref: [What's the difference between unit, functional, acceptance, and integration tests?](http://stackoverflow.com/questions/4904096/whats-the-difference-between-unit-functional-acceptance-and-integration-test)
* What is the purpose of a code style linting tool?
  * Ans: Help you to increase code quality and avoid bad practice.
  * See also: [JSLint Help](http://www.jslint.com/help.html)