

file (GLOB_RECURSE HEADER_LIST ${PROJECT_SOURCE_DIR}/include/*.hpp)
file (GLOB SOURCE_LIST ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)


add_library (phime SHARED ${SOURCE_LIST} ${HEADER_LIST})

target_include_directories(phime
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>

    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/phime/${LIBRARY_NAME}>
        $<INSTALL_INTERFACE:include/phime/${LIBRARY_NAME}>
)


