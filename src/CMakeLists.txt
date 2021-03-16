

file (GLOB_RECURSE HEADER_LIST ${PROJECT_SOURCE_DIR}/include/*.hpp)
file (GLOB SOURCE_LIST ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)


add_library (phime-shared SHARED "")

target_sources(phime-shared
  PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/phime.cpp
)

target_include_directories(phime-shared
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>

    PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/phime>
        $<INSTALL_INTERFACE:include/phime>
)



target_compile_definitions (phime-shared
  PRIVATE   
    $<$<CONFIG:Debug>:Debug>
    $<$<CONFIG:Release>:Release>
)

ph_precompile (TARGET phime-shared PRIVATE PCH ${std_pch})


set_target_properties(phime-shared
  PROPERTIES
    POSITION_INDEPENDENT_CODE 1
    SOVERSION ${PROJECT_VERSION_MAJOR}
    OUTPUT_NAME "phime"
    DEBUG_POSTFIX "_d"
    PUBLIC_HEADER "${PROJECT_SOURCE_DIR}/include/phime/phime.hpp"
    MACOSX_RPATH ON
    WINDOWS_EXPORT_ALL_SYMBOLS ON
  )


  # Prepare RPATH
file (RELATIVE_PATH _rel ${CMAKE_INSTALL_PREFIX}/${INSTALL_BINDIR} ${CMAKE_INSTALL_PREFIX})

if (APPLE)
    set (_rpath "@loader_path/${_rel}")
else ()
    set (_rpath "\$ORIGIN/${_rel}")
endif ()

file (TO_NATIVE_PATH "${_rpath}/${INSTALL_LIBDIR}" phime_RPATH)


