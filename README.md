POC-ASSIGNMENT

1.	sample C/C++ project that uses CMake as the build system and enables compiler warnings using the warnings-ng plugin:
2.	First, let's assume you have the following directory structure for our project:
project/
|-- CMakeLists.txt
|-- src/
|    |-- main.cpp
|-- include/
|-- myclass.h

1.	Create a CMakeLists.txt file in the root directory (project/) with the following content:
cmake_minimum_required(VERSION 3.0)
project(MyProject)

# Add source files
set(SOURCES
    src/main.cpp
    src/myclass.cpp
)

# Add header files
set(HEADERS
    include/myclass.h
)

# Create an executable target
add_executable(MyProject ${SOURCES} ${HEADERS})

# Add include directories
target_include_directories(MyProject PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

# Enable compiler warnings
include(CheckCXXCompilerFlag)

# Check if warnings-ng plugin is available
find_package(WarningsNG QUIET)
if (WarningsNG_FOUND)
    set(WARNING_FLAGS "${WARNING_FLAGS} -fanalyzer")
endif()

# Compiler-specific warning flags
if (CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    # Check and set individual warning flags
    check_cxx_compiler_flag(-Wall COMPILER_SUPPORTS_WALL)
    check_cxx_compiler_flag(-Wextra COMPILER_SUPPORTS_WEXTRA)
    check_cxx_compiler_flag(-Wpedantic COMPILER_SUPPORTS_WPEDANTIC)

    if (COMPILER_SUPPORTS_WALL)
        target_compile_options(MyProject PRIVATE -Wall)
    endif()

    if (COMPILER_SUPPORTS_WEXTRA)
        target_compile_options(MyProject PRIVATE -Wextra)
    endif()

    if (COMPILER_SUPPORTS_WPEDANTIC)
        target_compile_options(MyProject PRIVATE -Wpedantic)
    endif()
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(MyProject PRIVATE /W4)
endif()

2.	Inside the src directory, create a main.cpp file with a sample code:
#include "myclass.h"

int main() {
    MyClass obj;
    obj.sayHello();
    return 0;

src/myclass.cpp:
#include "myclass.h"
#include <iostream>

void MyClass::sayHello() {
    std::cout << "Hello, World!" << std::endl;
}
3.	Ensure that these files are present and properly included in your project. Also, double-check that you have added the correct paths in the CMakeLists.txt file for the header and source files.
4.	Now, you can generate the build files using CMake. Open a terminal, navigate to the project directory, and run the following commands:
cmake -B build
 
cmake --build build
 

To perform static code analysis using SonarQube with GATE conditions for your C/C++ sample application, you need to follow these steps:
1.	Install and configure SonarQube:
•	Download SonarQube from the official website: https://www.sonarqube.org/downloads/
•	Extract the downloaded archive to a directory of your choice.
•	Open the conf/sonar.properties file in the SonarQube directory and configure it according to your environment.
•	Start SonarQube by executing the bin/[OS]/sonar.sh start command.
2.	Install and configure SonarScanner:
•	Download SonarScanner from the official website: https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/
•	Extract the downloaded archive to a directory of your choice.
•	Add the SonarScanner directory to your system's PATH environment variable.
3.	Configure your project for analysis:
•	Create a sonar-project.properties file in the root directory of your project.
•	Add the following properties to the file:
sonar.projectKey=Demo
sonar.projectName=Demo
sonar.projectVersion=1.0
sonar.sources=src
sonar.language=c++
sonar.sourceEncoding=UTF-8
sonar.cxx.cppcheck.reportPath=cppcheck.xml
sonar.host.url=http://localhost:9000
sonar.login=admin
sonar.password=welcome!

1.	
•	Customize the properties based on your project's details and structure.
2.	Run the analysis:
•	Open a terminal or command prompt and navigate to the root directory of your project.
•	Run the following command to start the analysis:
sonar-scanner
 
 
 
1.	
•	The SonarScanner will analyze your project's source code and send the results to SonarQube for processing.
2.	View the analysis results:
•	Open your web browser and navigate to http://localhost:9000 (assuming SonarQube is running on your local machine).
•	Log in to SonarQube with the default credentials (admin/admin).
•	Create a new project using the same project key specified in the sonar-project.properties file.
•	Once the project is created, you will be able to view the analysis results, including code quality metrics, issues, and GATE conditions violations.
3.	CONCLUSION: As I found that as per SonarQube Docs I have got to know that in free edition we cannot analyze C++ files were detected in this project during the last analysis. C++ cannot be analyzed with my current SonarQube edition. Please consider upgrading to Developer Edition to find Bugs, Code Smells, Vulnerabilities and Security Hotspots in this file.

