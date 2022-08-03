# Copyright 2022 Google LLC
# SPDX-License-Identifier: Apache-2.0

cmake_minimum_required(VERSION 3.20.0)
project(fff)

# Add the gtest library which will be used below
add_subdirectory(gtest)

# Enable ctest
enable_testing()

# Generate fff.h if fakegen.rb changed
add_custom_command(
  OUTPUT
    ${CMAKE_CURRENT_LIST_DIR}/fff.h
  COMMAND
    cat ${CMAKE_CURRENT_LIST_DIR}/LICENSE > ${CMAKE_CURRENT_LIST_DIR}/fff.h
  COMMAND
    echo >> ${CMAKE_CURRENT_LIST_DIR}/fff.h
  COMMAND
    ruby ${CMAKE_CURRENT_LIST_DIR}/fakegen.rb >> ${CMAKE_CURRENT_LIST_DIR}/fff.h
  DEPENDS
    ${CMAKE_CURRENT_LIST_DIR}/fakegen.rb
    ${CMAKE_CURRENT_LIST_DIR}/LICENSE
)

# Add an interface library for fff.h
add_library(fff INTERFACE)
target_sources(fff PUBLIC ${CMAKE_CURRENT_LIST_DIR}/fff.h)
target_include_directories(fff INTERFACE ${CMAKE_CURRENT_LIST_DIR})

# Add tests and samples
add_subdirectory(test)
add_subdirectory(examples)
