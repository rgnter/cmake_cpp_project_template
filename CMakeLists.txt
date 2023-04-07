cmake_minimum_required(VERSION 3.24)
project(cmake_cpp_project_template
        VERSION      1.0.0
        LANGUAGES    CXX
        DESCRIPTION  "My CMake Project"
        HOMEPAGE_URL "https://example.com")

include(3rd-party/CMakeLists.txt)

add_library(project_properties INTERFACE)
# List of all available compile features
# can be found in documentation for
# the CMAKE_CXX_KNOWN_FEATURES variable
target_compile_features(project_properties
        INTERFACE cxx_std_20)
target_compile_definitions(project_properties
        INTERFACE)

# Library
add_library(libapp STATIC
        src/public/libapp/libapp.hpp
        src/private/libapp/libapp.cpp)
target_include_directories(libapp
        PUBLIC src/public)
target_link_libraries(libapp
        PUBLIC project_properties spdlog
        PRIVATE openssl)

# Executable
add_executable(app
        src/private/app/app.cpp
        src/private/app/main.cpp)
target_link_libraries(app
        PUBLIC project_properties
        PRIVATE libapp)
