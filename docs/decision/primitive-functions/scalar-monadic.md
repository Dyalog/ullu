# Scalar Monadic Functions

## Magnitude (`R←|Y`)([docs](https://help.dyalog.com/latest/#Language/Primitive%20Functions/Magnitude.htm))

The tests include:
- Datatype tests: tests for positive and negativefor all the available numeric datatypes
- Tests based on Floating point representation(`⎕FR`): All the tests run with values of `⎕FR` as 645 and 1287.
- Separate tests for boolean values: Booleans need special tests because construction of boolean arrays is simple and different than others.

Variations include:
- Normal: general test case with parameter and expected result.
- Empty: an empty array generated from the parameter of the testcase is used for the test.
- Different shapes: shapes are randomly generated.
- Different shapes with 0 in shape: A 0 is randomly inserted into the shape to generate this case.