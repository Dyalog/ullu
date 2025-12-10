# How to add tests

### Make a namespace

Make a namespace titled the primitive being tested

Eg: uniquemask

```APL
:Namespace uniquemask
    ⍝ ...
:EndNamespace
```

### Main function

Start with making the main function titled `test_functionname` like `test_uniquemask`. Here `test_` is important because the [`./unittest.apln`](../unittest.apln) recognises the main function of the test suite of the primitive with the `test_` keyword.

### Initialise variables

Primitives depend on ⎕CT/⎕DCT, ⎕FR, ⎕DIV and ⎕IO, so all default values of these can be initialised:

```APL
ct_default←#.utils.ct_default
dct_default←#.utils.dct_default
fr_dbl←#.utils.fr_dbl
fr_decf←#.utils.fr_decf
io_default←#.utils.io_default
io_0←#.utils.io_0
div_0←#.utils.div_0
div_1←#.utils.div_1
```

Then we need to get some specific data that we can manipulate to give us expected results to some testcases to logically/mathematically check the correct output. This is meant as a very basic fallback for testing with model functions fail.

This can look something like this:

#### Example 1: [old way] Using manually created data

This is an example from [unique mask](../tests/uniquemask.apln) (≠). These values can be changed according to what you want the fundamental tests to do but general layout should remain the same covering all the data types possible.

```APL
⍝ All data generated is unique
bool←0 1                                      ⍝ 11: 1 bit Boolean type arrays
i1←¯60+⍳120                                   ⍝ 83: 8 bits signed integer
char1←⎕UCS (100+⍳100)                         ⍝ 80: 8 bits character
char2←⎕UCS (1000+⍳100)                        ⍝ 160: 16 bits character
i2←{⍵,-⍵}10000+⍳100                           ⍝ 163: 16 bits signed integer
char3←⎕UCS (100000+⍳100)                      ⍝ 320: 32 bits character
i3←{⍵,-⍵}100000+⍳100                          ⍝ 323: 32 bits signed  integer
ptr←(13↑⎕a) (13↓⎕a)                           ⍝ 326: Pointer (32-bit or 64-bit as appropriate)
dbl←{⍵,-⍵}i3+0.1                              ⍝ 645: 64 bits Floating
cmplx←{⍵,-⍵}(0J1×⍳100)+⌽⍳100                  ⍝ 1289: 128 bits Complex
Hcmplx←{⍵,-⍵}(1E14J1E14×⍳20)                  ⍝ 1289 but larger numbers to test for CT value
⍝ Hdbl is 645 but larger numbers to test for CT value
⍝ intervals of 2 are chosen because CT for these numbers +1 and -1
⍝ come under the region of tolerant equality
Hdbl←{⍵,-⍵}1E14+(2×⍳50)

⍝ This is needed for a case that can be hit if we have a lot of small numbers
⍝ which produce a hash collision
⍝ Occurrence: same.c.html#L1153
Sdbl←{⍵,-⍵}(⍳500)÷1000

⍝ Hfl is 1287 but larger numbers to test for CT value
⍝ far intervals are chosen for non overlap
⍝ with region of tolerant equality
⎕FR←fr_decf
fl←{⍵,-⍵}i3+0.01                              ⍝ 1287: 128 bits Decimal
Hfl←{⍵,-⍵}2E29+(1E16×⍳10)
⎕FR←fr_dbl
```

#### Example 2: Using utility functions

This is an example from [union_and_intersection.apln](../tests/union_and_intersection.apln) (∪ and ∩). Here we use utility functions from [random.apln](../random.apln) like `Ints` to generate integer data and `Chars` for character data. We also dynamically create variables for different lengths.

```APL
data_single_bool_0←∧/1 0 1 0                      ⍝ singleton boolean
data_single_bool_1←∧/1 1 1 1                      ⍝ singleton boolean
data_bool←1 0
data_i1←100 #.random.Ints 8                       ⍝ 100 random 8-bit integers
data_i2←100 #.random.Ints 16                      ⍝ 100 random 16-bit integers
data_i4←100 #.random.Ints 32                      ⍝ 100 random 32-bit integers

⍝ Dynamically create variables for different lengths
:For len :In 8 16 32 64 128
    ⍎'data_i1_',⍕len'←len #.random.Ints 8'
    ⍎'data_i2_',⍕len'←len #.random.Ints 16'
    ⍎'data_i4_',⍕len'←len #.random.Ints 32'
:EndFor

data_char0←⎕AV                                    ⍝ 82: DyalogAPL classic char set
:If ~#.utils.isClassic
    data_char1←100 #.random.Chars 8               ⍝ 80: 8 bits character
    data_char2←100 #.random.Chars 16              ⍝ 160: 16 bits character
    data_char3←100 #.random.Chars 32              ⍝ 320: 32 bits character
    data_char_ptr←data_char1 data_char2 data_char3⍝ 326: Pointer
:EndIf
data_ptr←data_i1 data_i2 data_i4                  ⍝ 326: Pointer
data_dbl←{⍵,-⍵}data_i4+0.1                        ⍝ 645: 64 bits Floating
data_cmplx←{⍵,-⍵}(0J1×⍳100)+⌽⍳100                 ⍝ 1289: 128 bits Complex
data_Hcmplx←{⍵,-⍵}(1E14J1E14×⍝20)                 ⍝ 1289 but larger numbers to test for CT value
data_Hdbl←{⍵,-⍵}1E14+(2×⍳50)                      ⍝ 645 but larger numbers to test for CT value
data_Sdbl←{⍵,-⍵}(⍳500)÷1000                       ⍝ Small doubles for hash collision testing

⎕FR←#.utils.fr_decf
data_fl←{⍵,-⍵}data_i4+0.01                        ⍝ 1287: 128 bits Decimal
data_Hfl←{⍵,-⍵}2E29+(1E16×⍳10)
⎕FR←#.utils.fr_dbl
```

Note: The `Ints` and `Chars` functions are defined in [random.apln](../random.apln) and generate random integers and characters respectively of the specified bit size.

### Initialise test description

Test description gives information about the `testID`, datatypes being tested on, the [test variation](todo: add link to variation section), and the different setting values.

Example from [union_and_intersection.apln](../tests/union_and_intersection.apln):

```APL
∇ r←testDesc
  r←'for ',case,{0∊⍴case2:'',⍵ ⋄ ' , ',case2,⍵},' & ⎕CT ⎕DCT ⎕FR:',⍕⎕CT ⎕DCT ⎕FR
∇
```

Or as a dfn (alternative example):

```APL
testDesc←{'for ',case,{0∊⍴case2:'',⍵⋄' , ', case2,⍵},' & ⎕CT ⎕DCT:',⎕CT,⎕DCT, '& ⎕FR:', ⎕FR, '& ⎕IO:', ⎕IO}
```

### Testing functions

#### `Assert`

Assert is a function described in [`./unittest.apln`](../unittest.apln) that takes in a test expression that gives a boolean result and evaluates the output of the result and gives the instructions to pretty print the result based on the user settings of the test suite.

#### `RunVariations`

RunVariations is a function described in [testfns.apln](../testfns.apln) which takes the expressions to be evaluated and does the following:
- tests using the standard form it comes in
- tests a scalar element from the data it gets
- tests an empty array derived from the input
- applies a different shape to the input and evaluates
- creates a different shape that has a 0 in the shape of the input
- tests the input with the model function to double check the result
- RandModelTest takes the datatype and the boundary values of the expressions and generates a random array of the same datatype to increase the amount of data we have.

#### Model function

A model function replicates the behavior of an existing function by employing alternative primitives or computational steps. Model functions are used to test outputs of tests that can give not very intuitively computable results. Model functions here try to use primitives that are least related to the primitive being tested(this is mainly related so that it can be easily pin pointed which primitive is failing because shared code can be difficult to deal with).

Examples from [union_and_intersection.apln](../tests/union_and_intersection.apln):

```APL
    modelUnion←{⍺,⍵~⍺}
    modelIntersection←{(⍺∊⍵)/⍺}
```

Other examples:

```APL
    modelMagnitude←{⍵×(¯1@(∊∘0)(⍵>0))}
    modelUnique←{0=≢⍵:⍵ ⋄ ↑,⊃{⍺,(∧/⍺≢¨⍵)/⍵}⍨/⌽⊂¨⊂⍤¯1⊢⍵}
```

### The tests

All tests should run with all types of ⎕CT/⎕DCT, ⎕FR, ⎕DIV and ⎕IO values depending on which settings are implicit arguments of the primitive, ie. all of the settings that they depend on.

#### Example 1: Basic loop structure

```APL
:For io :In io_default io_0
    ⎕IO←io

    :For ct :In 1 0
        (⎕CT ⎕DCT)←ct × ct_default dct_default ⍝ set comparison tolerance

        :For fr :In fr_dbl fr_decf
            ⍝ ...
        :EndFor
    :EndFor
:EndFor
```

#### Example 2: Testing multiple operators with varied CT values

From [union_and_intersection.apln](../tests/union_and_intersection.apln) - shows testing multiple operators (∪ and ∩) with more varied comparison tolerance values:

```APL
:For op :In '∪' '∩'
    :If op≡'∪'
        RunVariations←modelUnion #.testfns._RunVariationsWithModel_(⍎op)
    :Else
        RunVariations←modelIntersection #.testfns._RunVariationsWithModel_(⍎op)
    :EndIf

    :For ct :In 0 1 10 0.1  ⍝ Test with 0, default, 10× default, and 0.1× default
        (⎕CT ⎕DCT)←ct×#.utils.(ct_default dct_default)

        :For fr :In 1 2
            ⎕FR←fr⊃#.utils.(fr_dbl fr_decf)
            ⎕IO←1

            quadparams←⎕CT ⎕DCT ⎕FR ⎕IO ⎕DIV

            ⍝ ... tests here ...
        :EndFor
    :EndFor
:EndFor
```

#### Types of tests

The general structure followed with all tests is as follows:

##### General tests

General tests are tests that test information other than if the primitive gives the correct output.

Examples from [uniquemask](../tests/uniquemask.apln):

- uniquemask cannot return a result that exceeds the number of elements of the input
    ```APL
    r,← 'TGen1' desc Assert (≢data)≥≢≠data
    ```

- datatype of the result will always be boolean in nature
    ```APL
    r,← 'TGen2' desc Assert 11≡⎕dr ≠data intertwine data ⍝ intertwine is a util function that intertwines the data like (1 1 1 1) intertwine (0 0 0 0) gives 1 0 1 0 1 0 1 0
    ```

Examples from [union_and_intersection.apln](../tests/union_and_intersection.apln):

- union returns a result that has at least the number of elements of the input
    ```APL
    r,← 'Union (∪) Gen1' desc Assert (≢data)≤≢data∪data
    ```

- intersection returns a result that does not exceed the number of elements of the input
    ```APL
    r,← 'Intersection (∩) Gen1' desc Assert (≢data)≥≢data∩data
    ```

- datatype of the data will not change under union or intersection
    ```APL
    r,← 'Union (∪) Gen2' desc Assert (⎕DR data)≡⎕DR data∪data
    r,← 'Intersection (∩) Gen2' desc Assert (⎕DR data)≡⎕DR data∩data
    ```

##### Namespace tests

When primitives can work with namespaces, test them explicitly:

```APL
⍝ Example from union_and_intersection.apln
desc←testDesc
r,← 'Union (∪) ns' desc Assert ((#)modelUnion(# ⎕SE))≡(#)∪(# ⎕SE)
```

##### Logical/mathematical tests

These are tests that evaluate the result of the primitive with a very logical straightforward approach and try to depend on as few primitives as possible to reduce the number of false failures if the dependent primitives fail. Some examples of subtract:

- substraction with the same number gives 0
    ```APL
    r,← 'T1' desc quadparams RunVariations ((0⍨¨data) data data)
    ```

##### Cross datatype tests

Cross data type tests deal with the primitive handling 2 datatypes at a time in the same input. Each datatype must be tested with every other datatype for a more accurate result.

##### comparison tolerance tests

Comparison tolerance tests deal with the primitive getting inputs which are believed to be in the tolerance range of numbers. The inputs are generally numbers slightly bigger and smaller than the original number that is treated to be equal at default ⎕CT and ⎕DCT values and should be treated differently when ⎕CT and ⎕DCT are zero.

More information about comparison tolerance here: https://help.dyalog.com/latest/Content/Language/System%20Functions/ct.htm

and here: https://www.dyalog.com/uploads/documents/Papers/tolerant_comparison/tolerant_comparison.htm

##### Independent tests

Independent tests are tests for special cases that either have optimisations in the sources or have a special need that cannot be covered in general data types and only work on certain specific values.

Examples:

- The special case can be hit if we have two 8 bit int numbers in the input: a & b, and a is b-⎕CT. That means, that when we get to element b in the loop, we will find element a and hit the case.
Occurrence: same.c.html#L1152
    ```APL
        d←i1[?≢i1]
        r,←'TCTI1' desc Assert (1 0)≡(≠ (d-({fr-1:⎕dct⋄⎕ct}⍬)) d)
    ```

- Testing hash collisions with specially prepared data and `1500⌶` (from [union_and_intersection.apln](../tests/union_and_intersection.apln)):
    ```APL
    ⍝ Test with hashed arrays to check hash collision handling
    r,← 'Union (∪) Hash1' desc quadparams RunVariations data (#.utils.hashArray data)
    ```

## Misc useful information

Interesting things:
- dyalogVersion ← DyalogAPL version from `]version`
- isDyalog32 ← 0 or 1 for if the interpreter 64-bit ot 32-bit?
- isDyalogClassic ← 0 or 1 for if the interpreter classic or unicode
- [utils.apln](../utils.apln) ← This file has some widely used manipulation functions
- [random.apln](../random.apln) ← Contains `Ints` and `Chars` functions for generating random test data
