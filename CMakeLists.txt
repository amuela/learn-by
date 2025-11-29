cmake_minimum_required(VERSION 3.21)
project(BootstrapCpp LANGUAGES CXX)

# Use ccache if available and not already set
if(NOT CMAKE_CXX_COMPILER_LAUNCHER)
    find_program(CCACHE_PROGRAM ccache)
    if(CCACHE_PROGRAM)
        set(CMAKE_CXX_COMPILER_LAUNCHER "${CCACHE_PROGRAM}")
        message(STATUS "Using ccache: ${CCACHE_PROGRAM}")
    endif()
endif()

# Find all Chapter* directories
file(GLOB CHAPTER_DIRS RELATIVE ${CMAKE_SOURCE_DIR} "Chapter*")

foreach(CHAPTER_DIR ${CHAPTER_DIRS})
    if(IS_DIRECTORY "${CMAKE_SOURCE_DIR}/${CHAPTER_DIR}")
        # Collect all .cpp files in the chapter directory
        file(GLOB CHAPTER_SOURCES "${CMAKE_SOURCE_DIR}/${CHAPTER_DIR}/*.cpp")

        if(CHAPTER_SOURCES)
            # Create executable with the folder name
            add_executable(${CHAPTER_DIR} ${CHAPTER_SOURCES})

            # Add include directory for header files
            target_include_directories(${CHAPTER_DIR} PRIVATE
                "${CMAKE_SOURCE_DIR}/${CHAPTER_DIR}"
            )
        endif()
    endif()
endforeach()
