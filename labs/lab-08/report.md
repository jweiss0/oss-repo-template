## 1: Getting Started
Successful configuration and generation:
![c1-1](https://user-images.githubusercontent.com/18493608/159081954-bba6b8ad-75f3-45ed-b695-9c7a6a40c218.png)

Successful run of `cmake` in build folder:
![c1-2](https://user-images.githubusercontent.com/18493608/159081972-74e05b77-3f2a-4264-a984-513eae299e52.png)

## 2: Executing the Tests
In the *Nightly* and *Experimental* sections, you can see what tests were run for a specific submission by first clicking on the build name. Then, scroll down to the "Current Build" table and click on "Test". Finally, click on "View Tests Summary" to view all tests run for the submission.

One submission with errors is the *Nightly* build for [Linux-Gentoo-Sparc32-gcc4.8](https://open.cdash.org/viewTest.php?onlyfailed&buildid=7802189). There is a failed test for *RunCMake.CompatibleInterface*. By clicking on the test name that failed, we can view the error conditions. We can also view the line of code that raised the error, making it easier to track where the error occurred and ultimately debug the issue.

A *Master* build similar to my system is [cmake-debian10_iwyu](https://open.cdash.org/build/7802190). I would consider the dashboard to be clean, since there are no errors or test failures with the build. There are also no errors with my submission to the dashboard. The output is consistent with the expected result of the similar *Master* build mentioned previously task.

Test submission in Experimental Dashboard:
![c2-1](https://user-images.githubusercontent.com/18493608/159081988-001bf709-a6a1-494c-9ba0-bf2f7cc4ed6a.png)

## 3: Failing/Passing a Test
The failure occurs with test 26 and provides information about the test name, as well as the exact issue. The copyright notice states the years as 2000-2020, but the error mentions that the current year is 2022, therefore this copyright file is outdated.

Test submission in Experimental Dashboard (with error):
![c3-1](https://user-images.githubusercontent.com/18493608/159081999-57b6114f-b31c-41dd-99d5-ca77fef6f541.png)

Error fix:
![c3-2](https://user-images.githubusercontent.com/18493608/159082014-41c24bb2-7bd9-4f0e-846c-cd6dec2acc35.png)

Successful test after error fix:
![c3-3](https://user-images.githubusercontent.com/18493608/159082018-22e05089-623b-4ee1-98a3-6484c53f7b9e.png)

## 4: CI/CD
Link to repository here: [CMake-Step5](https://github.com/jweiss0/CMake-Step5).

Screenshots of successful push and workflow test run:
![image](https://user-images.githubusercontent.com/18493608/159082033-32c7e7c4-2685-431e-a854-336031e84dd6.png)
![image](https://user-images.githubusercontent.com/18493608/159082044-f13b859b-39fe-4684-aecf-99fe0e0b3cf1.png)
![image](https://user-images.githubusercontent.com/18493608/159082053-02bdf6ed-c46f-4efa-a863-2836c2ef69b6.png)
