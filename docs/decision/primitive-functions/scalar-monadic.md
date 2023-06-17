# Scalar Monadic Functions

## Magnitude (`R←|Y`)([docs](https://help.dyalog.com/latest/#Language/Primitive%20Functions/Magnitude.htm))

The tests include:
- Datatype tests: tests for positive and negative for all the available numeric datatypes
- Tests based on Floating point representation(`⎕FR`): All the tests run with values of `⎕FR` as 645 and 1287.
- Separate tests for boolean values: Booleans need special tests because construction of boolean arrays is simple and different than others.
- Edge Cases:
    - I4 (32-bit int) argument and D8 (IEEE float) result. ¯2147483648 is the largest magnitude negative 32-bit int but 2147483647 is the largest positive. Therefore, the absolute value of ¯2147483648 has to be stored in a float. Note that the argument must not be a scalar for this code to be hit. Magnitude on a non-complex number is abs(olute). Also, The elements in the argument to | will all have the same type, and similarly the elements in the result will do as well (The other side of the turnary operator for the template).
    - Same as the above example, except that DF is DECF.
    - Z (complex) argument and DF (DECF float) result. Magnitude, when applied to complex, gives a non-complex result.
    - When a singleton scalar boolean is passed as rarg to magnitude, this `if block` hits, but there is no way to make a singleton scalar boolean as it is rarely used due to performance concerns. They way to make singleton booleans ∧/1 0 1 0. Whether this will continue is hard to say, but it is still the case in v19.0. This case basically returns the rarg as the result as booleans are 0 and 1 values which need not be changed by magnitude.

Variations include:
- Normal: general test case with parameter and expected result.
- Empty: an empty array generated from the parameter of the testcase is used for the test.
- Different shapes: shapes are randomly generated.
- Different shapes with 0 in shape: A 0 is randomly inserted into the shape to generate this case.

Code Coverage report: Magnitude is 100% covered by these tests. The portions from - `allos/src/optimise_expr.cpp` and `allos/src/optimise_parse.cpp` are not covered because they are obsolete.