# How to add tests

### Make a namespace

Make a namespace titled the test name
Eg: uniquemask

```APL
:Namespace uniquemask
    ⍝...
:EndNamespace
```

### Main function

Start with making the main function titled `test_functionname` like `test_uniquemask`. Here `test_` is important because the `./unittest.apln` recognises the main function of the test suite of the primitive with the `test_` keyword.

### Initialise variables

Primitives depend on ⎕CT/⎕DCT, ⎕FR and ⎕IO, so all default values of these can be initialised:

```APL
ct_default←1E¯14
dct_default←1E¯28
fr_dbl←645
fr_decf←1287
io_default←1
io_0←0
```

Then we need to get some specific data that we can manipulate to give us expected results to some testcases to logically/mathematically check the correct output. This is meant as a very basic fallback for testing with model functions fail.

This can look something like this:

This is an example from [unique mask](tests\uniquemask.apln) (≠)

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

### Initialise test description

Test description gives information about the `testID`, datatypes being tested on, the [test variaiton](todo: add link to variation section), and the different setting values.

```APL
testDesc←{'for ',case,{0∊⍴case2:'',⍵⋄' , ', case2,⍵},' & ⎕CT ⎕DCT:',⎕CT,⎕DCT, '& ⎕FR:', ⎕FR, '& ⎕IO:', ⎕IO}
```

### Testing functions

#### `Assert`

Assert is a function described in `./unittest.apln` that takes in a test expression that gives a boolean result and evaluates the output of the result and gives the instructions to pretty print the result based on the user settings of the test suite.

#### `RunVariations`

RunVariations is a function described in each test file which takes the expressions to be evaluated and does the following:
- tests using the standard form it comes in
- tests a scalar element from the data it gets
- tests an empty array derived from the input
- applies a different shape to the input and evaluates
- creates a different shape that has a 0 in the shape of the input

#### Model function

A model function replicates the behavior of an existing function by employing alternative primitives or computational steps. Model functions are used to test outputs of tests that can give not very intuitively computable results. Model functions here try to use primitives that are least related to the primitive being tested(this is mainly related so that it can be easily pin pointed which primitive is failing because shared code can be difficult to deal with). Model functions look like:

```APL
    modelMagnitude←{⍵×(¯1@(∊∘0)(⍵>0))}
```

```APL
    modelUnique←{0=≢⍵:⍵ ⋄ ↑,⊃{⍺,(∧/⍺≢¨⍵)/⍵}⍨/⌽⊂¨⊂⍤¯1⊢⍵}
```

### The tests

All tests should run with all types of ⎕CT/⎕DCT, ⎕FR and ⎕IO values depending on which settings are implicit arguments of the primitive, ie. all of the settings that they depend on.
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

#### Types of tests

The general structure followed with all tests is as follows:

##### General tests

General tests are tests that test information other than if the primitive gives the correct output. Some examples of uniquemask:

- uniquemask cannot return a result that exceeds the number of elements of the input
    ```APL
    r,← 'TGen1' desc Assert (≢data)≥≢≠data
    ```

- datatype of the result will always be boolean in nature
    ```APL
    r,← 'TGen2' desc Assert 11≡⎕dr ≠data intertwine data ⍝ intertwine is a util function that intertwines the data like (1 1 1 1) intertwine (0 0 0 0) gives 1 0 1 0 1 0 1 0
    ```

##### Logical/mathematical tests

These are tests that evaluate the result of the primitive with a very logical straightforward approach and try to depend on as few primitives as possible to reduce the number of false failures if the dependent primitives fail. Some examples of unique mask:

- all elements of data are unique so the result would be all 1s
    ```APL
    r,← 'T1' desc RunVariations (1⍨¨data) data
    ```

- all elements are perfectly intertwined so the result would be 1 0 1 0 1 0...
    ```APL
    r,← 'T3' desc RunVariations ((1⍨¨data) intertwine (0⍨¨data)) (data intertwine data)
    ```

##### Cross datatype tests

Cross data type tests deal with the primitive handling 2 datatypes at a time in the same input. Each datatype must be tested with every other datatype for a more accurate result.

##### comparison tolerance tests

Comparison tolerance tests deal with the primitive getting inputs which are believed to be in the tolerance range of numbers. The inputs are generally numbers slightly bigger and smaller than the original number that is treated to be equal at default ⎕CT and ⎕DCT values and should be treated differently when ⎕CT and ⎕DCT are zero.

More information about comparison tolerance here: https://help.dyalog.com/latest/Content/Language/System%20Functions/ct.htm

and here: https://www.dyalog.com/uploads/documents/Papers/tolerant_comparison/tolerant_comparison.htm

##### Independent tests

Independent tests are tests for special cases that either have optimisations in the sources or have a special need that cannot be covered in general data types and only work on certain specific values. For example:
- The special case can be hit if we have two 8 bit int numbers in the input: a & b, and a is b-⎕CT. That means, that when we get to element b in the loop, we will find element a and hit the case.
Occurrence: same.c.html#L1152
    ```APL
                d←i1[?≢i1]
                r,←'TCTI1' desc Assert (1 0)≡(≠ (d-({fr-1:⎕dct⋄⎕ct}⍬)) d)
    ```
