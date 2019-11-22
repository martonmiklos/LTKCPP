cmake_minimum_required(VERSION 3.0)

project(LTKCPP VERSION 1.0.0.10 LANGUAGES CXX)

find_package(LibXml2 REQUIRED)
find_package(LibXslt REQUIRED)

set(CMAKE_CONFIGURATION_TYPES Debug Release CACHE TYPE INTERNAL FORCE)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(BUILD_SHARED_LIBS ON)

option(LTKCPP_BUILD_LIBRARY "Build the library." ON)
mark_as_advanced(LTKCPP_BUILD_LIBRARY)

option(LTKCPP_BUILD_EXAMPLES "Build the examples." OFF)
mark_as_advanced(LTKCPP_BUILD_EXAMPLES)

option(LTKCPP_BUILD_TESTS "Build the tests." OFF)
mark_as_advanced(LTKCPP_BUILD_TESTS)

option(LTKCPP_BUILD_DOCS "Build the documentation." ON)
mark_as_advanced(LTKCPP_BUILD_DOCS)

option(LTKCPP_BUILD_IMPINJ "Build Impinj extensions." ON)
mark_as_advanced(LTKCPP_BUILD_IMPINJ)

option(LTKCPP_BUILD_LLRPORG "Build llrp.org extensions." OFF)
mark_as_advanced(LTKCPP_BUILD_LLRPORG)

if (NOT DEFINED CMAKE_INSTALL_BINDIR)
  set(CMAKE_INSTALL_BINDIR "${CMAKE_INSTALL_PREFIX}/bin")
endif()
if (NOT DEFINED CMAKE_INSTALL_LIBDIR)
  set(CMAKE_INSTALL_LIBDIR "${CMAKE_INSTALL_PREFIX}/lib")
endif()
if (NOT DEFINED CMAKE_INSTALL_INCLUDEDIR)
  set(CMAKE_INSTALL_INCLUDEDIR "${CMAKE_INSTALL_PREFIX}/include/ltkcpp")
else()
  set(CMAKE_INSTALL_INCLUDEDIR "${CMAKE_INSTALL_INCLUDEDIR}/ltkcpp")
endif()
set(DEFINITIONS_PATH "${CMAKE_CURRENT_SOURCE_DIR}/Definitions")

if(LTKCPP_BUILD_LIBRARY)
  add_subdirectory(Library)
endif()
if(LTKCPP_BUILD_EXAMPLES)
  add_subdirectory(Examples)
endif()
if(LTKCPP_BUILD_TESTS)
  add_subdirectory(Tests)
endif()
if(LTKCPP_BUILD_DOCS)
  add_subdirectory(Release)
endif()