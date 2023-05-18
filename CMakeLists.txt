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
