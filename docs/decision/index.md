# Decision Doc

# unittest.apln

Unit tests file is the main file of the program that runs the tests. It is structured with 3 main functions:
- `RunTests`
- `Assert`
- `GetTests`

### RunTests
Base function, parses all input parameters, fetches and executes tests.

Parses verbose, stop and random link(⎕RL) values.

⎕RL value is set to 0 by default, 0 here generates a random ⎕RL value and logs it, so that non-similar tests run everytime the suite is executed. It logs the random value put into ⎕RL so these tests can be repeated. Any valid value can be put into ⎕RL.

Final result is displayed with number of tests ran, failed, passed and the time taken to complete the testing.

### Assert
Handles execution, termination and visualization of each test based on instructions from verbose and stop.

It is a dyadic function of the format `r←tData Assert r`. `tData` being test ID and test comments for the particular test. `r` being the result of the indivisual test.

### GetTests
Fetches tests for RunTests to execute them. Fetches a namespace containing functions called test_*.

# The tests
The tests are categorised into the type of function/operator they are categorised according to the Language Reference by [help.dyalog.com](https://help.dyalog.com/latest/)
- Primitive functions
    - Non Scalar Selector functions
    - Scalar Monadic Functions

## Test Files

The test files are structed with two functions:
- `Test_Functionname`: General testcases including variations of ⎕IO, ⎕FR, ⎕CT, ⎕DCT(implicit arguments of the function) and datatypes for each testcase.
- `RunVariations`: Each test is run with variations of normal, empty, and differently shaped.
