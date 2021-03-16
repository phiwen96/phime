

file (GLOB_RECURSE HEADER_LIST ${PROJECT_SOURCE_DIR}/include/*.hpp)
file (GLOB SOURCE_LIST ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)


add_library (phime SHARED)

target_sources(phime
  PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/phime.cpp
)

target_include_directories(phime
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>

    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/phime/${LIBRARY_NAME}>
        $<INSTALL_INTERFACE:include/phime/${LIBRARY_NAME}>
)



target_compile_definitions (phime
  PRIVATE   
    $<$<CONFIG:Debug>:Debug>
    $<$<CONFIG:Release>:Release>
)

ph_precompile (TARGET phime PRIVATE PCH ${std_pch})



