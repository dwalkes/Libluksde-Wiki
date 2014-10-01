# Automated tests
The libluksde package comes with automated tests, which are stored in the sub directory: tests.

To run the automated tests:
```
make check
```

The automated tests are currently only intended to run on a system that can run a bash shell.

## Test files
**Note that no test files are provided by the project.**

If you have your own test files the automated tests expect the files to be stored in sub directories of:
```
tests/input/
```

Every sub directory is considered a test set, e.g. the basic test set would be the directory:
```
tests/input/basic
```

## Test profiles
There are also multiple test profiles:
* libluksde
* pyluksde
* luksdeinfo


These test profiles are used by one or more tests.
By default the tests run all the test sets, if applicable.
Each test profile can define which test sets to ignore in the ignore-file, e.g. for the libluksde test profile:
```
tests/input/.libluksde/ignore
```

Every line of the ignore-file specifies (the sub directory name of) the test set that should be ignored, e.g. for the basic test set:
```
basic
```

Some of the tests store data in the test profile to be used for comparison in successive test runs.
At the moment this data needs to be cleaned out manual if it causes the test to fail due to legitimate changes.

Each test profile can also define which files to run in a specific test set in the files-file, e.g. for the basic test set in the libluksde test profile:
```
tests/input/.libluksde/basic/files
```

Every line of the files-file specifies the filename in the test set to include in the test.
Files not defined in the files-file are ignored by the test.

E.g. if the basic test set sub directory contains the files:
```
image1.raw
image2.raw
```

and only the first file needs to be tested, the files-file can then be defined as:
```
image1.raw
```

## Detecting memory leakage
The tests can also be run with valgrind to detect memory leakage.

**Running the automated tests with valgrind will significantly impact the speed at which they are run.**

To run the automated tests with valgrind:
```
export CHECK_WITH_VALGRIND=1; make check; export CHECK_WITH_VALGRIND=;
```

If the automated test process detects memory leaks it will end the specific test and indicate it failed.

