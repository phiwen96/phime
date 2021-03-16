

file (GLOB_RECURSE HEADER_LIST ${PROJECT_SOURCE_DIR}/include/*.hpp)
file (GLOB SOURCE_LIST ${CMAKE_CURRENT_SOURCE_DIR}/*.cpp)

### SHARED ###
add_library (phime-shared SHARED "")

target_sources(phime-shared
  PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/phime.cpp
)

# target_include_directories(phime-shared
#     PUBLIC
#         $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
#         $<INSTALL_INTERFACE:include>

#     PRIVATE
#         $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/phime>
#         $<INSTALL_INTERFACE:include/phime>
# )



target_compile_definitions (phime-shared
  PRIVATE   
    $<$<CONFIG:Debug>:Debug>
    $<$<CONFIG:Release>:Release>
)

ph_precompile (TARGET phime-shared PRIVATE PCH ${std_pch})


# Prepare RPATH
file (RELATIVE_PATH _rel ${CMAKE_INSTALL_PREFIX}/${INSTALL_BINDIR} ${CMAKE_INSTALL_PREFIX})

if (APPLE)
    set (_rpath "@loader_path/${_rel}")
else ()
    set (_rpath "\$ORIGIN/${_rel}")
endif ()

file (TO_NATIVE_PATH "${_rpath}/${INSTALL_LIBDIR}" phime_RPATH)










set_target_properties(phime-shared
  PROPERTIES
    POSITION_INDEPENDENT_CODE 1
    CXX_VISIBILITY_PRESET hidden
    VISIBILITY_INLINES_HIDDEN 1
    SOVERSION ${PROJECT_VERSION_MAJOR}
    OUTPUT_NAME "phime"
    DEBUG_POSTFIX "_d"
    PUBLIC_HEADER "${PROJECT_SOURCE_DIR}/include/phime/phime.hpp;${CMAKE_BINARY_DIR}/${INSTALL_INCLUDEDIR}/phimeExport.h"
    MACOSX_RPATH ON
    WINDOWS_EXPORT_ALL_SYMBOLS ON
)



include (GenerateExportHeader)

generate_export_header (phime-shared
  BASE_NAME "phime"
  EXPORT_MACRO_NAME "phime_EXPORT"
  EXPORT_FILE_NAME "${CMAKE_BINARY_DIR}/${INSTALL_INCLUDEDIR}/phimeExport.h"
  DEPRECATED_MACRO_NAME "phime_DEPRECATED"
  NO_EXPORT_MACRO_NAME "phime_NO_EXPORT"
  STATIC_DEFINE "phime_STATIC_DEFINE"
  NO_DEPRECATED_MACRO_NAME "phime_NO_DEPRECATED"
  DEFINE_NO_DEPRECATED
)


target_include_directories (phime-shared
  PUBLIC
    ${CMAKE_BINARY_DIR}/${INSTALL_INCLUDEDIR}
)


### STATIC ###
add_library (phime-static STATIC "")


target_sources (phime-static
  PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/phime.cpp
)

target_compile_definitions (phime-static
  PUBLIC
    phime_STATIC_DEFINE
)

target_include_directories (phime-static
  PUBLIC
    ${CMAKE_BINARY_DIR}/${INSTALL_INCLUDEDIR}
)

target_link_libraries (phime-static
  PUBLIC
)

set_target_properties (phime-static
  PROPERTIES
    POSITION_INDEPENDENT_CODE 1
    ARCHIVE_OUTPUT_NAME "message"
    DEBUG_POSTFIX "_sd"
    RELEASE_POSTFIX "_s"
    PUBLIC_HEADER "${PROJECT_SOURCE_DIR}/include/phime/phime.hpp;${CMAKE_BINARY_DIR}/${INSTALL_INCLUDEDIR}/phimeExport.h"
)

