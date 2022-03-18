## 1: Getting Started
Successful configuration and generation:


Successful run of `cmake` in build folder:


## 2: Executing the Tests
In the *Nightly* and *Experimental* sections, you can see what tests were run for a specific submission by first clicking on the build name. Then, scroll down to the "Current Build" table and click on "Test". Finally, click on "View Tests Summary" to view all tests run for the submission.

One submission with errors is the *Nightly* build for [Linux-Gentoo-Sparc32-gcc4.8](https://open.cdash.org/viewTest.php?onlyfailed&buildid=7802189). There is a failed test for *RunCMake.CompatibleInterface*. By clicking on the test name that failed, we can view the error conditions. We can also view the line of code that raised the error, making it easier to track where the error occurred and ultimately debug the issue.

A *Master* build similar to my system is [cmake-debian10_iwyu](https://open.cdash.org/build/7802190). I would consider the dashboard to be clean, since there are no errors or test failures with the build. There are also no errors with my submission to the dashboard. The output is consistent with the expected result of the similar *Master* build mentioned previously task.

Test submission in Experimental Dashboard:

## 3: Failing/Passing a Test
The failure occurs with test 26 and provides information about the test name, as well as the exact issue. The copyright notice states the years as 2000-2020, but the error mentions that the current year is 2022, therefore this copyright file is outdated.

Test submission in Experimental Dashboard (with error):

Error fix:

Successful test after error fix:


## 4: CI/CD
Link to repository here: [CMake-Step5](https://github.com/jweiss0/CMake-Step5).

Screenshots of successful push and workflow test run:
