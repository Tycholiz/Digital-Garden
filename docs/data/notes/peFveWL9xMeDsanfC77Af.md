
A Test Harness is the collection of all the items needed to test software at the unit, module, application or system level, and provides the mechanism to execute the tests
- Every item such as input data, test parameters, test case, test script, expected output data, test tool, and test result report is part of the test harness.

A test harness (a.k.a *automated test framework*) is a combination of:
1. The code to be tested
2. The test data
    - because we need to test different circumstances, there will be multiple sets of test data

Test harnesses allow for the automation of tests
- They can call functions with supplied parameters and print out and compare the results to the desired value.

A test harness should allow specific tests to run (this helps in optimizing), orchestrate a runtime environment, and provide a capability to analyse results.

The typical objectives of a test harness are to:
- Automate the testing process.
- Execute test suites of test cases.
- Generate associated test reports.

Think of a Test Harness as an 'enabler' that actually does all the work of (1)executing tests using a (2)test library and (3)generating reports. It would require that your test scripts are designed to handle different (4)test data and (5)test scenarios. Essentially, when the test harness is in place and prerequisite data is prepared (aka data prep) someone should be able to click a button or run one command to execute all your tests and generate reports.
