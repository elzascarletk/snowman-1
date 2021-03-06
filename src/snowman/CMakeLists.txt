set(SOURCES
    main.cpp
)

# This sets up the exe icon for Windows under MinGW.
set(RES_FILES "")
if(MINGW OR MSVC)
  SET(RES_FILES "snowman.rc")
endif()

if(MINGW)
  SET(CMAKE_RC_COMPILER_INIT "windres")
  ENABLE_LANGUAGE(RC)
  SET(CMAKE_RC_COMPILE_OBJECT
    "<CMAKE_RC_COMPILER> -O coff -i <SOURCE> -o <OBJECT>")
endif(MINGW)

add_executable(snowman WIN32 ${RES_FILES} ${SOURCES})
target_link_libraries(snowman nc-gui nc-core ${Boost_LIBRARIES} ${QT_LIBRARIES} ${CMAKE_DL_LIBS})
if(MSVC)
    set_target_properties(snowman PROPERTIES LINK_FLAGS /ENTRY:mainCRTStartup)
endif()

if (NOT ${IDA_PLUGIN_ENABLED})
    install(TARGETS snowman RUNTIME DESTINATION bin)
    if(WIN32 AND NOT ${NC_QT5})
        install_qt4_executable("bin/snowman.exe")
    endif()
    if(WIN32 AND ${NC_QT5})
        install_qt5_executable("bin/snowman.exe")
    endif()
endif()

# vim:set et sts=4 sw=4 nospell:
