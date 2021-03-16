
add_executable (phime-app app.cpp)

target_link_libraries (phime-app PUBLIC phime-shared)


set_target_properties(phime-app
  PROPERTIES
    MACOSX_RPATH ON
    SKIP_BUILD_RPATH OFF
    BUILD_WITH_INSTALL_RPATH OFF
    INSTALL_RPATH "${phime_RPATH}"
    INSTALL_RPATH_USE_LINK_PATH ON
)

