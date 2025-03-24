![ullu Banner](assets/ullu-cover.png)

[![GitHub Licence](https://img.shields.io/github/license/sloorush/ullu)](LICENSE)

# ullu

> A test suite to test APL Primitives.

## 🤔 What is ullu?

Ullu is a QA for DyalogAPL (can be used to test any APL ideally) which tests specifically the functionality of primitives one by one. This test suite's main focus is finding bugs, irregularities, edge cases and code coverage.

## 🎿 Coverage

### 💪  Available Tests
- add (dyadic +)
- divide (dyadic ÷)
- floor (monadic ⌊)
- index of (dyadic ⍳) (Not tested for coverage)
- magnitude (monadic |)
- membership (dyadic ∊) (Not tested for coverage)
- multiply (dyadic ×)
- residue (dyadic |)
- subtract (dyadic -)
- unique (monadic ∪)
- unique mask (monadic ≠)

### 🧱 In progress and Next up

All details about upcoming tests can be found in the [project board](https://github.com/orgs/Dyalog/projects/4/views/1)

## ✍ The name

Pronounced as `/ˈulːluː/`, The name comes from the Hindi word for owl. The owl looks over Dyalog APL when everyone else is asleep.

Just as the owl represents both wisdom and foolishness the QA also has a dual nature of being wise and dumb at the same time.

## ⬇ Usage

You can use ullu in a dyalog session on any supported operating system.

### Quick Run

Run tests using dyalogscript:
```
dyalogscript run.apls
```

After this, you will be prompted with options to choose from

### Detailed Run

Using Dyalog Interpreter (prefered):

- Load the namespaces:

```
]LINK.Create # <path to repository>
```

- Run the test cases

```
unittest.RunTests tests.[test_namespace] [prod=1|0] [verbose= 1|0] [stop=1|0] [⎕RL=any seed value(''?'' for random)] (0 default)
```

Options:

prod: Changes the result of the test suite to be completely non-verbose and just return 1 if everything passes to not clutter the Jenkins jobs

verbose: if set to 0, only output failing tests and a single summary line and version information.

stop: if set to 1, any test which fails causes the framework to stop and allows the developer to inspect the failing test.

⎕RL: Seed value to for the random link variable that generates random numbers through the tests (it gets reset after each test)

Example:
```
unittest.RunTests tests.membership 0 1 0 1232
```
or
```
unittest.RunTests tests.membership
```

### 🔗 More documentation

- [contributing.md](contributing.md): Guide on how to contribute to the codebase
- [Decision docs](docs/decision): Explains the decisions taken with each step of the codebase and also documents anomalies for future users.


### 🔗 Suggestions/Questions

Feel free to open GitHub issues for any questions, suggestions or feature requests

If you want to reach out, please email `aarush[at]dyalog.com`

<!-- ### 🔗 References -->

## ⚖ Licence

Copyright 2023 Dyalog

Licensed under MIT License: https://opensource.org/licenses/MIT

<!-- <p align="center">Made with ❤ at Dyalog</p> -->
