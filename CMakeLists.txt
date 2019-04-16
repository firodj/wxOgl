cmake_minimum_required (VERSION 3.9)

#set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)

project (wxOgl)

find_package(wxWidgets REQUIRED COMPONENTS core base adv html xml xrc)
include(${wxWidgets_USE_FILE})

set(wxOgl_SOURCE_FILES
  Source/composit.cpp
  Source/drawn.cpp
  Source/basic.cpp
  Source/ogldiag.cpp
  Source/bmpshape.cpp
  Source/mfutils.cpp
  Source/divided.cpp
  Source/basic2.cpp
  Source/oglmisc.cpp
  Source/canvas.cpp
  Source/lines.cpp
  Source/constrnt.cpp

  Include/wx/ogl/ogl.h
  Include/wx/ogl/composit.h
  Include/wx/ogl/drawn.h
  Include/wx/ogl/drawnp.h
  Include/wx/ogl/basic.h
  Include/wx/ogl/ogldiag.h
  Include/wx/ogl/bmpshape.h
  Include/wx/ogl/mfutils.h
  Include/wx/ogl/misc.h
  Include/wx/ogl/divided.h
  Include/wx/ogl/basicp.h
  Include/wx/ogl/canvas.h
  Include/wx/ogl/lines.h
  Include/wx/ogl/linesp.h
  Include/wx/ogl/constrnt.h
)
message(STATUS "wxWidgets LIB: ${wxWidgets_LIBRARIES}")
add_library(wxOgl ${wxOgl_SOURCE_FILES})
target_include_directories(wxOgl PRIVATE Include)
target_include_directories(wxOgl INTERFACE Include)
# set_property(TARGET wxOgl PROPERTY CXX_STANDARD 11)

###############################################################################

set(OglEdit_SOURCE_FILES
  Sample/OglEdit/ogledit.cpp
  Sample/OglEdit/doc.cpp
  Sample/OglEdit/view.cpp
  Sample/OglEdit/palette.cpp

  Sample/OglEdit/ogledit.h
  Sample/OglEdit/doc.h
  Sample/OglEdit/view.h
  Sample/OglEdit/palette.h
)

if (WIN32)
  list(APPEND OglEdit_SOURCE_FILES Sample/OglEdit/ogledit.rc)
endif (WIN32)

add_executable(ogledit WIN32 ${OglEdit_SOURCE_FILES})

# set_property(TARGET ogledit PROPERTY CXX_STANDARD 11)
target_link_libraries(ogledit PRIVATE ${wxWidgets_LIBRARIES}
  wxOgl)

###############################################################################

set(Studio_SOURCE_FILES
  Sample/Studio/studio.cpp
  Sample/Studio/cspalette.cpp
  Sample/Studio/csprint.cpp
  Sample/Studio/dialogs.cpp
  Sample/Studio/doc.cpp
  Sample/Studio/mainfrm.cpp
  Sample/Studio/project.cpp
  Sample/Studio/shapes.cpp
  Sample/Studio/symbols.cpp
  Sample/Studio/view.cpp

  Sample/Studio/studio.h
  Sample/Studio/cspalette.h
  Sample/Studio/dialogs.h
  Sample/Studio/doc.h
  Sample/Studio/mainfrm.h
  Sample/Studio/project.h
  Sample/Studio/shapes.h
  Sample/Studio/studio_resources.h
  Sample/Studio/symbols.h
  Sample/Studio/view.h
)

if (WIN32)
  list(APPEND Studio_SOURCE_FILES Sample/Studio/studio.rc)
endif (WIN32)

add_executable(studio WIN32 ${Studio_SOURCE_FILES})

# set_property(TARGET studio PROPERTY CXX_STANDARD 11)
target_link_libraries(studio PRIVATE ${wxWidgets_LIBRARIES}
  wxOgl)

  add_custom_command(
    TARGET studio POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy
            ${CMAKE_SOURCE_DIR}/Sample/Studio/studio_resources.xrc
            $<TARGET_FILE_DIR:${CMAKE_PROJECT_NAME}>/studio_resources.xrc)

###############################################################################

if (WIN32)
  target_compile_options(wxOgl PRIVATE
    /WX
    )
  target_compile_options(ogledit PRIVATE
    /WX
    )
  target_compile_options(studio PRIVATE
    /WX
    )

else ()
  target_compile_options(wxOgl PRIVATE
    $<$<COMPILE_LANGUAGE:CXX>:-mmacosx-version-min=10.9 -std=gnu++11 -stdlib=libc++>
    -Werror
    )
  target_compile_options(ogledit PRIVATE
    $<$<COMPILE_LANGUAGE:CXX>:-mmacosx-version-min=10.9 -std=gnu++11 -stdlib=libc++>
    -Werror
    )
  target_compile_options(studio PRIVATE
    $<$<COMPILE_LANGUAGE:CXX>:-mmacosx-version-min=10.9 -std=gnu++11 -stdlib=libc++>
    -Werror
    )

  # set_target_properties(wxOgl PROPERTIES 
  #   COMPILE_FLAGS "-mmacosx-version-min=10.9 -std=gnu++11 -stdlib=libc++"
  #   LINK_FLAGS "-mmacosx-version-min=10.9 -std=gnu++11 -stdlib=libc++"
  # target_link_options(wxOgl PRIVATE
  #   -mmacosx-version-min=10.9 -std=gnu++11 -stdlib=libc++)
  # set_target_properties(gui PROPERTIES COMPILE_FLAGS "-m32" LINK_FLAGS "-m32")
endif ()
