# Non Scalar Selector functions

## [Index of](../../../tests/indexof.apln) (`R←X⍳Y`)([docs](https://help.dyalog.com/latest/Content/Language/Primitive%20Functions/Index%20Of.htm))

The tests include:
- Datatype tests: tests for found and indexed/not-found variations for all the available datatypes
- Cross-datatype tests: tests for found and indexed/not-found across datatypes, concatenating expressions and results to find any errors.
- Tests based on comparison tolerance(`⎕CT` & `⎕DCT`): tests to check if `d=d+1` on larger values of double, floating and complex numbers based on comparison tolerance values(default or 0).
- Tests based on Floating point representation(`⎕FR`): All the tests run with values of `⎕FR` as 645 and 1287.
- Separate tests for boolean values: Booleans need special tests because they only have 2 elements and since `i1` and `bool` have overlapping values.

Variations include:
- Normal: general test case with left, right and expected result.
- Empty left: an empty array generated from the left argument of the testcase is used for the test.
- Empty right: Right argument cannot be empty so it is not included.
- Different shapes: TBD.
- Different shapes with 0 in shape: TBD.

Code Coverage report: NA

## [Membership](../../../tests/membership.apln) (`R←X∊Y`) ([docs](https://help.dyalog.com/latest/Content/Language/Primitive%20Functions/Membership.htm))

The tests include:
- Datatype tests: tests for found/not-found variations for all the available datatypes.
- Cross-datatype tests: tests for found/not-found across datatypes, concatenating expressions and results to find any errors.
- Tests based on comparison tolerance(`⎕CT` & `⎕DCT`): tests to check if `d=d+1` on larger values of double, floating and complex numbers based on comparison tolerance values(default or 0).
- Tests based on Floating point representation(`⎕FR`): All the tests run with values of `⎕FR` as 645 and 1287.
- Separate tests for boolean values: Booleans need special tests because they only have 2 elements and since `i1` and `bool` have overlapping values.

Variations include:
- Normal: general test case with left, right and expected result.
- Empty left: an empty array generated from the left argument of the testcase is used for the test.
- Empty right: an empty array generated from the right argument of the testcase is used for the test.
- Different shapes: shapes are randomly generated with a certain rule. That is, for ANY array lshape, and for any rightshape where `(≢right)≤×/rshape` the condition guarantees that `rshape⍴right` will create an array which contains ALL the elements of right, possibly more than once.
- Different shapes with 0 in shape: A 0 is randomly inserted into the shape of the left array to generate this case.

Code Coverage report: NA
