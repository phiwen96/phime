cmake_minimum_required (VERSION 3.19.4)

project (phime
	VERSION 1.0
	LANGUAGES CXX C
    DESCRIPTION "Very nice project"
)

add_subdirectory (cmake/phake)

if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
	set (CMAKE_CXX_STANDARD 20)
	set (CMAKE_CXX_STANDARD_REQUIRED ON)
    set (CMAKE_CXX_EXTENSIONS        ON)
	set_property (GLOBAL PROPERTY USE_FOLDERS YES)
    include (CTest)

    include (GNUInstallDirs)
    set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
    set (CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
    set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_BINDIR})
endif ()


add_subdirectory (src)


if (CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    enable_testing ()
    add_subdirectory (tests)
endif ()