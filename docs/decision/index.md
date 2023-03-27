# Decision Doc

# unittest.apln

Unit tests file is the main file of the program that runs the tests. It is structured with 3 main functions:
- `RunTests`
- `Assert`
- `GetTests`

### RunTests
Base function, parses all input parameters, fetches and executes tests.

Parses verbose, stop and random link(⎕RL) values.

⎕RL value is set to 0 by default, so that similar tests run everytime the suite is executed. It is also easier to reproduce failed tests because the ⎕RL value is reset after each test. `?` can be passed to generate a possible random value for ⎕RL.

Final result is displayed with number of tests ran, failed, passed and the time taken to complete the testing.

### Assert
Handles execution, termination and visualization of each test based on instructions from verbose and stop.

It is a dyadic function of the format `r←tData Assert r`. `tData` being test ID and test comments for the particular test. `r` being the result of the indivisual test.

### GetTests
Fetches tests for RunTests to execute them. Fetches a namespace containing functions called test_*.

# The tests
The tests are categorised into the type of function/operator they are.
- Primitive functions
    - Non Scalar Selector functions
    - Scalar Monadic Functions
- Primitive operators

## Test Files

The test files are structed with two functions:
- `Test_Functionname`: General testcases including variations of ⎕IO. ⎕FR, ⎕CT, ⎕DCT and datatypes for each testcase.
- `RunVariations`: Each test is run with variations of normal, empty, and differently shaped.
