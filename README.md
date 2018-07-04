# Fortran Unit Test Library FUT

### Overview
This is a Fortran Unit Test library purely written in Fortran to encourage scientific programmer to write tests.

### Installation
A CMake-Setup is provided.

### Example

demo.F90:
```fortran
program good_test

  use unit_test

  implicit none
  
  type(test_suite_type)  specific_suite
  
  ! example with default suite
  call test_case_init()

  call test_case_create('Test 1')

  ! By sending macros __FILE__ and __LINE__, report will print the file and line number where assertion fails.
  call assert_approximate(1.0, 2.0, __FILE__, __LINE__) ! line 14

  call test_suite_report()
  call test_case_final()
  
  ! example with specific suite
  specific_suite%name = 'my specific test suite'
  call test_case_create('Specific Test 1', specific_suite)
  call assert_approximate(1.0, 2.0, suite = specific_suite)
  
  call test_case_create('Specific Test 2', specific_suite)
  ! suite = SUITE need in this case (cause optional argument eps is missing)
  call assert_equal(1.0, 2.0, __FILE__, __LINE__,  suite = specific_suite)
  
  call test_case_create('Specific Test 3', specific_suite)
  call assert_approximate(1.0, 2.0, __FILE__, __LINE__, 1E-0, specific_suite)
  
  ! report a test_case
  call test_case_report('Specific Test 2', specific_suite)
  
  ! report the complete suite
  call test_suite_report(specific_suite)
  call test_case_final(specific_suite)

end program good_test
```

Output:
```txt
///////////////////// Report of Suite: Default test suite ///////////////////////

 -> Details:
 Test 1: 0 of 1 assertions succeed.
 Assertion #1 failed with reason: x (1.0000) =~ y (2.0000)
 Check line: test_assert.F90:13


 -> Summary:
 Default test suite: 0 of 1 assertions succeed.

////////////////////////////////////////////////////////////////////////////////


//////// Report of Suite: my specific test suite, Case: Specific Test 2 /////////

 Specific Test 2: 0 of 1 assertions succeed.

 Assertion #1 failed with reason: x (1.0000) == y (2.0000)
 Check line: test_assert.F90:25


/////////////////// Report of Suite: my specific test suite /////////////////////

 -> Details:
 Specific Test 1: 0 of 1 assertions succeed.
 Assertion #1 failed with reason: x (1.0000) =~ y (2.0000)
 Check line: Unknown file:-1

 Specific Test 2: 0 of 1 assertions succeed.
 Assertion #1 failed with reason: x (1.0000) == y (2.0000)
 Check line: test_assert.F90:25

 Specific Test 3: 0 of 1 assertions succeed.
 Assertion #1 failed with reason: x (1.0000) =~ y (2.0000)
 Check line: test_assert.F90:28


 -> Summary:
 my specific test suite: 0 of 3 assertions succeed.

////////////////////////////////////////////////////////////////////////////////
```

You can integrate this library into your CMake based project as:

```
...
add_subdirectory (<fortran-unit-test root>)
include_directories (${UNIT_TEST_INCLUDE_DIR})
...
target_link_libraries (<user target> fortran_unit_test ...)
```

### Compiler Support

[![Compiler](https://img.shields.io/badge/GNU-v4.8.5+-brightgreen.svg)]()
[![Compiler](https://img.shields.io/badge/PGI-v18.4+-brightgreen.svg)]()
[![Compiler](https://img.shields.io/badge/Intel-v17.0.2.187+-brightgreen.svg)]()
[![Compiler](https://img.shields.io/badge/IBM%20XL-not%20tested-yellow.svg)]()
[![Compiler](https://img.shields.io/badge/g95-not%20tested-yellow.svg)]()
[![Compiler](https://img.shields.io/badge/NAG-not%20tested-yellow.svg)]()

### License
[![License](https://img.shields.io/badge/license-MIT-brightgreen.svg)]()

Go to [Top](#top)
