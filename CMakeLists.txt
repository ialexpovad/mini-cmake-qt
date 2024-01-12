cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

# Add folder where are supportive functions
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Include Qt basic functions
include(QtCommon)

project(Example LANGUAGES CXX C VERSION 0.0.0.0)

# Options: Things you can set via commandline options to cmake (e.g. -DENABLE_LTO=[ON|OFF])
option(ENABLE_WARNINGS_SETTINGS "Allow target_set_warnings to add flags and defines.
                                 Set this to OFF if you want to provide your own warning parameters." ON)

option(ENABLE_DOCTESTS "Include tests in the library. Setting this to OFF will remove all doctest related code.
                                 Tests in tests/*.cpp will still be enabled." ON)

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
include(Misc)
include(Warnings)
include(Doctest)

# Set additional project information
set(COMPANY "Example")
set(COPYRIGHT "Copyright (c) 2024 Alex Povad. All rights reserved.")
set(IDENTIFIER "com.example.Example")

set(SOURCES
    source/main.cpp
    source/mainwindow.cpp
)

add_project_meta(META_FILES_TO_INCLUDE)

set(RESOURCE_FILES example.qrc)

set(CMAKE_PREFIX_PATH "C:/Qt/5.15.2/Static/MSVC2019-32bit-extented")
find_package(Qt5 COMPONENTS Widgets REQUIRED)

# source_group("UI Files" FILES ${UI_FILES})

add_executable(${PROJECT_NAME} ${OS_BUNDLE} # Expands to WIN32 or MACOS_BUNDLE depending on OS
    ${SOURCES} ${META_FILES_TO_INCLUDE} ${RESOURCE_FILES}
)

target_link_libraries(${PROJECT_NAME} Qt5::Widgets doctest)

# Set the compile options you want (change as needed).
target_set_warnings(${PROJECT_NAME} ENABLE ALL AS_ERROR ALL DISABLE Annoying)


# Set the properties you require, e.g. what C++ standard to use. Here applied to library and main (change as needed).
set_target_properties(
    ${PROJECT_NAME} 
      PROPERTIES
        CXX_STANDARD 17
        CXX_STANDARD_REQUIRED YES
        CXX_EXTENSIONS NO
)

# Set the properties for your target (library or executable)
set_target_properties(
    ${PROJECT_NAME}
    PROPERTIES
        C_STANDARD 11  # Set your desired C standard (e.g., 11, 99)
        C_STANDARD_REQUIRED YES
        C_EXTENSIONS NO
)

# Set up tests (see tests/CMakeLists.txt).
add_subdirectory(tests)

# Add targets related to doxygen documention generation
add_subdirectory(docs)