cmake_minimum_required (VERSION 3.1.0)
project (loguru)
set(CMAKE_VERBOSE_MAKEFILE ON)


if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release)
endif()


message(STATUS "pThreads found:                 ${Threads_FOUND}")
message(STATUS "dl found:                       ${DL_FOUND}")
message(STATUS "CMAKE_SYSROOT:                  ${CMAKE_SYSROOT}")
message(STATUS "CMAKE_FIND_ROOT_PATH            ${CMAKE_FIND_ROOT_PATH}")
message(STATUS "includes:                       ${INCLUDE_DIRECTORIES}")


# Define the CXX sources
set ( CXX_SRCS
	loguru.cpp
)


#compile flags
set(LOGURU_WITH_STREAMS 1)
set(NDEBUG 0)




set(LOGURU_FLAGS "-DLOGURU_WITH_STREAMS=${LOGURU_WITH_STREAMS} -DNDEBUG=${NDEBUG}")

set_source_files_properties(
	${CXX_SRCS} PROPERTIES COMPILE_FLAGS
"-g -O3 -std=c++11 -Wall -Wno-psabi -w ${LOGURU_FLAGS}")


add_library(${PROJECT_NAME}_static STATIC ${CXX_SRCS})
add_library(${PROJECT_NAME} SHARED ${CXX_SRCS})


set_target_properties(${PROJECT_NAME}_static PROPERTIES PUBLIC_HEADER "loguru.hpp")
set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER "loguru.hpp")

 

install(TARGETS ${PROJECT_NAME}_static ${PROJECT_NAME}
    
    LIBRARY DESTINATION "/usr/lib"
    ARCHIVE DESTINATION "/usr/lib"
    PUBLIC_HEADER DESTINATION "/usr/include"
)

if(BUILD_DEB)
	SET(CPACK_GENERATOR "DEB")
	SET(CPACK_DEBIAN_PACKAGE_MAINTAINER "Nope")
	#set(CPACK_DEBIAN_PACKAGE_DEPENDS "")
	INCLUDE(CPack)
endif()
