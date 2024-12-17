# Scalar Dyadic Arithmetic Functions

## [Residue](../../../tests/residue.apln) (`R←X|Y`)([docs](http://help.dyalog.com/latest/Content/Language/Primitive%20Functions/Residue.htm))

The tests include:
- Datatype tests: tests for positive and negative for all the available numeric datatypes
- Tests based on Floating point representation(`⎕FR`): All the tests run with values of `⎕FR` as 645 and 1287.
- Tests based on Comparison tolerance(`⎕CT` and `⎕DCT`): All the tests run with default and zero values.
- Edge Cases:
    - Separate cases had to be added for 0, ¯1, 0J0, 0.0
    - Separate cases had to be added to residue propogated using a scan to target certain sections of sources.

Variations include:
- Normal: general test case with parameter and expected result.
- Empty: an empty array generated from the parameter of the testcase is used for the test.
- Different shapes: shapes are randomly generated.
- Different shapes with 0 in shape: A 0 is randomly inserted into the shape to generate this case.

Code Coverage report: Residue is 100% covered by these tests.
Significant covered files include: `allos/src/arith_su.c`, `allos/src/z.c`, and `allos/src/scalarscalar.cpp`
