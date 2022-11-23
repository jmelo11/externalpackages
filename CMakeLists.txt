cmake_minimum_required(VERSION 3.0)

project(
  externalpackages
  VERSION 1.0.0
  LANGUAGES CXX)

add_subdirectory(Engine/QuantLib)
add_subdirectory(Engine/QuantExt)
add_subdirectory(json)
add_subdirectory(json-schema-validator)
add_subdirectory(pybind11)
add_subdirectory(pybind11_json)

add_subdirectory(nanobench)
