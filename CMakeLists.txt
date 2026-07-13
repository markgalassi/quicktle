cmake_minimum_required(VERSION 3.26)

project(quicktle VERSION 2.0.4)
set(PACKAGE_NAME "quicktle")
#set(PACKAGE_VERSION "2.0.4")

configure_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/quicktle.spec.in"
  "quicktle.spec"
)


set(CPACK_PROJECT_CONFIG_FILE CPackSourceConfig.cmake)
if (CPACK_GENERATOR MATCHES "TGZ")

    set(CPACK_ARCHIVE_COMPONENT_INSTALL ON)

    set(CPACK_COMPONENT_HDR_GROUP "src")
    set(CPACK_COMPONENT_SRC_GROUP "src")
endif ()
set(CPACK_SOURCE_GENERATOR "TGZ")
set(CPACK_PACKAGE_VERSION_MAJOR 2)
set(CPACK_PACKAGE_VERSION_MINOR 0)
set(CPACK_PACKAGE_VERSION_PATCH 4)
set(CPACK_PACKAGE_NAME "quicktle")
set(CPACK_SOURCE_IGNORE_FILES
    "/build/"
    "/_cbuild/"
    "/\\\\.git/"
)
include(CPack)


if (UNIX)
set(CMAKE_INSTALL_PREFIX "/usr")
endif(UNIX)

set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/build)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR})

set(QUICKTLE_SRC_DIR ${CMAKE_SOURCE_DIR}/src)
set(QUICKTLE_INC_DIR ${CMAKE_SOURCE_DIR}/include)
set(QUICKTLE_SAMPLES_DIR ${CMAKE_SOURCE_DIR}/samples)
# set(QUICKTLE_TESTS_DIR ${CMAKE_SOURCE_DIR}/test)

set(QUICKTLE_SOURCES
${QUICKTLE_SRC_DIR}/func.cpp
${QUICKTLE_SRC_DIR}/node.cpp
${QUICKTLE_SRC_DIR}/stream.cpp
${QUICKTLE_SRC_DIR}/dataset.cpp
)
set(QUICKTLE_HEADERS
${QUICKTLE_INC_DIR}/quicktle/func.h
${QUICKTLE_INC_DIR}/quicktle/node.h
${QUICKTLE_INC_DIR}/quicktle/stream.h
${QUICKTLE_INC_DIR}/quicktle/dataset.h
)


include_directories(${QUICKTLE_INC_DIR})

option(BUILD_SAMPLES "Build samples" ON)
if (BUILD_SAMPLES)
	add_subdirectory(${QUICKTLE_SAMPLES_DIR}/sample1)
	add_subdirectory(${QUICKTLE_SAMPLES_DIR}/sample2)
	add_subdirectory(${QUICKTLE_SAMPLES_DIR}/sample3)
	add_subdirectory(${QUICKTLE_SAMPLES_DIR}/sample4)
endif (BUILD_SAMPLES)

# option(BUILD_TESTS "Build tests" ON)
# if (BUILD_TESTS)
# 	add_subdirectory(${QUICKTLE_TESTS_DIR})
# endif(BUILD_TESTS)

add_library(${PROJECT_NAME} SHARED ${QUICKTLE_SOURCES})

install(TARGETS ${PROJECT_NAME} LIBRARY DESTINATION lib COMPONENT bin)
install(FILES ${QUICKTLE_HEADERS} DESTINATION include/quicktle COMPONENT hdr)
#install(FILES ${QUICKTLE_SOURCES} DESTINATION src/quicktle COMPONENT src)
