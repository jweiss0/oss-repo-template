## Step 1
TutorialConfig.h.in:
```
// the configured options and settings for Tutorial
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
```
tutorial.cxx:
```
// A simple program that computes the square root of a number
#include <cmath>
#include <cstdlib>
#include <iostream>
#include <string>
#include "TutorialConfig.h"

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  const double inputValue = std::stod(argv[1]);

  // calculate square root
  const double outputValue = sqrt(inputValue);
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}
```
CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.10)

# set the project name
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the executable
add_executable(Tutorial tutorial.cxx)
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )
```
Running Tutorial Code:
![Step1](https://user-images.githubusercontent.com/18493608/153774142-35b4eafb-2609-446b-8aee-a9acc039bcba.png)

## Step 2
TutorialConfig.h.in:
```
// the configured options and settings for Tutorial
#define Tutorial_VERSION_MAJOR @Tutorial_VERSION_MAJOR@
#define Tutorial_VERSION_MINOR @Tutorial_VERSION_MINOR@
#cmakedefine USE_MYMATH
```
tutorial.cxx:
```
// A simple program that computes the square root of a number
#include <cmath>
#include <iostream>
#include <string>
#include "TutorialConfig.h"
#ifdef USE_MYMATH
#  include "MathFunctions.h"
#endif

int main(int argc, char* argv[])
{
  if (argc < 2) {
    // report version
    std::cout << argv[0] << " Version " << Tutorial_VERSION_MAJOR << "."
              << Tutorial_VERSION_MINOR << std::endl;
    std::cout << "Usage: " << argv[0] << " number" << std::endl;
    return 1;
  }

  // convert input to double
  const double inputValue = std::stod(argv[1]);

  // calculate square root
  #ifdef USE_MYMATH
    const double outputValue = mysqrt(inputValue);
  #else
    const double outputValue = sqrt(inputValue);
  #endif
  std::cout << "The square root of " << inputValue << " is " << outputValue
            << std::endl;
  return 0;
}
```
CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
  list(APPEND EXTRA_INCLUDES "${PROJECT_SOURCE_DIR}/MathFunctions")
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC MathFunctions)

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           ${EXTRA_INCLUDES}
                           )

```
Running Tutorial Code:
![Step2](https://user-images.githubusercontent.com/18493608/153774156-ff2c181e-3d52-4586-843b-a6473a902af3.png)

## Step 3
CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )
```
MathFunctions/CMakeLists.txt:
```
add_library(MathFunctions mysqrt.cxx)

target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )
```
Running Tutorial Code:
![Step3](https://user-images.githubusercontent.com/18493608/153774165-daa189d7-e298-4d73-a735-34ac5458dc2f.png)

## Step 4
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)

target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  DESTINATION include
  )

enable_testing()

# does the application run
add_test(NAME Runs COMMAND Tutorial 25)

# does the usage message work?
add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"
  )

# define a function to simplify adding tests
function(do_test target arg result)
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})
  set_tests_properties(Comp${arg}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
    )
endfunction()

# do a bunch of result based tests
do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is (-nan|nan|0)")
do_test(Tutorial 0.0001 "0.0001 is 0.01")
```
MathFunctions/CMakeLists.txt
```
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
```
Running ctest -VV:
![Step4-1](https://user-images.githubusercontent.com/18493608/153774186-3f4d8c6c-6975-41ec-8329-17929df2ea42.png)
![Step4-2](https://user-images.githubusercontent.com/18493608/153774193-8b5e7490-10a0-4e9b-91a2-d72f75d76939.png)
![Step4-3](https://user-images.githubusercontent.com/18493608/153774198-a6c4e3c2-6ecf-4b73-a750-c86b2499e755.png)
![Step4-4](https://user-images.githubusercontent.com/18493608/153774205-5c5eb2c8-70c2-44c7-8c4a-c5bd66f34dd8.png)
![Step4-5](https://user-images.githubusercontent.com/18493608/153774217-b1a3df07-970a-4984-91d8-a5e6f5e1971e.png)

## Step 5
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.10)

# set the project name and version
project(Tutorial VERSION 1.0)

# specify the C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# should we use our own math functions
option(USE_MYMATH "Use tutorial provided math implementation" ON)

# configure a header file to pass some of the CMake settings
# to the source code
configure_file(TutorialConfig.h.in TutorialConfig.h)

# add the MathFunctions library
if(USE_MYMATH)
  add_subdirectory(MathFunctions)
  list(APPEND EXTRA_LIBS MathFunctions)
endif()

# add the executable
add_executable(Tutorial tutorial.cxx)
target_link_libraries(Tutorial PUBLIC ${EXTRA_LIBS})

# add the binary tree to the search path for include files
# so that we will find TutorialConfig.h
target_include_directories(Tutorial PUBLIC
                           "${PROJECT_BINARY_DIR}"
                           )

# add the install targets
install(TARGETS Tutorial DESTINATION bin)
install(FILES "${PROJECT_BINARY_DIR}/TutorialConfig.h"
  DESTINATION include
  )

# enable testing
enable_testing()

# does the application run
add_test(NAME Runs COMMAND Tutorial 25)

# does the usage message work?
add_test(NAME Usage COMMAND Tutorial)
set_tests_properties(Usage
  PROPERTIES PASS_REGULAR_EXPRESSION "Usage:.*number"
  )

# define a function to simplify adding tests
function(do_test target arg result)
  add_test(NAME Comp${arg} COMMAND ${target} ${arg})
  set_tests_properties(Comp${arg}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result}
    )
endfunction()

# do a bunch of result based tests
do_test(Tutorial 4 "4 is 2")
do_test(Tutorial 9 "9 is 3")
do_test(Tutorial 5 "5 is 2.236")
do_test(Tutorial 7 "7 is 2.645")
do_test(Tutorial 25 "25 is 5")
do_test(Tutorial -25 "-25 is (-nan|nan|0)")
do_test(Tutorial 0.0001 "0.0001 is 0.01")
```
MathFunctions/CMakeLists.txt
```
add_library(MathFunctions mysqrt.cxx)

# state that anybody linking to us needs to include the current source dir
# to find MathFunctions.h, while we don't.
target_include_directories(MathFunctions
          INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}
          )

# does this system provide the log and exp functions?
include(CheckSymbolExists)
check_symbol_exists(log "math.h" HAVE_LOG)
check_symbol_exists(exp "math.h" HAVE_EXP)
if(NOT (HAVE_LOG AND HAVE_EXP))
  unset(HAVE_LOG CACHE)
  unset(HAVE_EXP CACHE)
  set(CMAKE_REQUIRED_LIBRARIES "m")
  check_symbol_exists(log "math.h" HAVE_LOG)
  check_symbol_exists(exp "math.h" HAVE_EXP)
  if(HAVE_LOG AND HAVE_EXP)
    target_link_libraries(MathFunctions PRIVATE m)
  endif()
endif()

if(HAVE_LOG AND HAVE_EXP)
  target_compile_definitions(MathFunctions
                             PRIVATE "HAVE_LOG" "HAVE_EXP")
endif()

# install rules
install(TARGETS MathFunctions DESTINATION lib)
install(FILES MathFunctions.h DESTINATION include)
```
Running Tutorial Code:
![Step5](https://user-images.githubusercontent.com/18493608/153774232-2b77ba9d-8e86-4580-884b-a1f6f01bc7ba.png)

## Lab-BuildSystemsExample
Makefile
```
CFLAGS = -c -Wall -Iheaders -Isource
HEADER = headers
SRC = source

all: block static_block dynamic_block

clean:
	rm $(SRC)/block.o libstatic.a libshared.so program.o static_block dynamic_block

block: $(SRC)/block.o libstatic.a libshared.so
	echo "got here 5"

block.o: $(SRC)/block.c $(HEADER)/block.h
	cc $(CFLAGS) $(SRC)/block.c -o $(SRC)/block.o

libstatic.a: $(SRC)/block.o
	ar qc libstatic.a $(SRC)/block.o

libshared.so: $(SRC)/block.o
	cc -shared -o libshared.so $(SRC)/block.o

program.o: program.c

static_block: program.o libstatic.a
	cc program.o libstatic.a -o static_block -Wl,-rpath='$$ORIGIN'

dynamic_block: program.o libshared.so
	cc program.o libshared.so -o dynamic_block -Wl,-rpath='$$ORIGIN'
```
Size of `static_block`: 16856 bytes\
Size of `dynamic_block`: 16696 bytes\
Results:\
`$ ./static_block`
```
d y n a m i c   o r   s t a t i c 
y n a m i c   o r   s t a t i c d
n a m i c   o r   s t a t i c d y
a m i c   o r   s t a t i c d y n
m i c   o r   s t a t i c d y n a
i c   o r   s t a t i c d y n a m
c   o r   s t a t i c d y n a m i
  o r   s t a t i c d y n a m i c
o r   s t a t i c d y n a m i c
r   s t a t i c d y n a m i c   o
  s t a t i c d y n a m i c   o r
s t a t i c d y n a m i c   o r
t a t i c d y n a m i c   o r   s
a t i c d y n a m i c   o r   s t
t i c d y n a m i c   o r   s t a
i c d y n a m i c   o r   s t a t
c d y n a m i c   o r   s t a t i
```
`$ ./dynamic_block`
```
d y n a m i c   o r   s t a t i c 
y n a m i c   o r   s t a t i c d
n a m i c   o r   s t a t i c d y
a m i c   o r   s t a t i c d y n
m i c   o r   s t a t i c d y n a
i c   o r   s t a t i c d y n a m
c   o r   s t a t i c d y n a m i
  o r   s t a t i c d y n a m i c
o r   s t a t i c d y n a m i c
r   s t a t i c d y n a m i c   o
  s t a t i c d y n a m i c   o r
s t a t i c d y n a m i c   o r
t a t i c d y n a m i c   o r   s
a t i c d y n a m i c   o r   s t
t i c d y n a m i c   o r   s t a
i c d y n a m i c   o r   s t a t
c d y n a m i c   o r   s t a t i
```
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.10)

project(Lab5 VERSION 1.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${PROJECT_BINARY_DIR}")

add_library(LibStatic STATIC source/block.c)
add_library(LibShared SHARED source/block.c)

add_executable(static_block program.c)
add_executable(dynamic_block program.c)
target_link_libraries(static_block PUBLIC LibStatic)
target_link_libraries(dynamic_block PUBLIC LibShared)
```
Size of `static_block`: 16856 bytes\
Size of `dynamic_block`: 16696 bytes

Results:\
`$ ./static_block`
```
d y n a m i c   o r   s t a t i c 
y n a m i c   o r   s t a t i c d
n a m i c   o r   s t a t i c d y
a m i c   o r   s t a t i c d y n
m i c   o r   s t a t i c d y n a
i c   o r   s t a t i c d y n a m
c   o r   s t a t i c d y n a m i
  o r   s t a t i c d y n a m i c
o r   s t a t i c d y n a m i c
r   s t a t i c d y n a m i c   o
  s t a t i c d y n a m i c   o r
s t a t i c d y n a m i c   o r
t a t i c d y n a m i c   o r   s
a t i c d y n a m i c   o r   s t
t i c d y n a m i c   o r   s t a
i c d y n a m i c   o r   s t a t
c d y n a m i c   o r   s t a t i
```
`$ ./dynamic_block`
```
d y n a m i c   o r   s t a t i c 
y n a m i c   o r   s t a t i c d
n a m i c   o r   s t a t i c d y
a m i c   o r   s t a t i c d y n
m i c   o r   s t a t i c d y n a
i c   o r   s t a t i c d y n a m
c   o r   s t a t i c d y n a m i
  o r   s t a t i c d y n a m i c
o r   s t a t i c d y n a m i c
r   s t a t i c d y n a m i c   o
  s t a t i c d y n a m i c   o r
s t a t i c d y n a m i c   o r
t a t i c d y n a m i c   o r   s
a t i c d y n a m i c   o r   s t
t i c d y n a m i c   o r   s t a
i c d y n a m i c   o r   s t a t
c d y n a m i c   o r   s t a t i
```
