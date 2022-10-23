cmake_minimum_required(VERSION 3.0)
project(stb-cmake-wrapper)

function(add_stb_library library_name)
  set(target_name stb-${library_name})
  string(TOUPPER ${library_name} uppercase_library_name)

  add_library(
    ${target_name}
    main.cpp
  )
  target_compile_definitions(
    ${target_name}
    PRIVATE
      STB_${uppercase_library_name}_IMPLEMENTATION
      STB_CMAKE_WRAPPER_LIBRARY_INCLUDE="stb_${library_name}.h"
  )
  target_include_directories(
    ${target_name}
    PUBLIC
      stb
  )
  add_library(stb::${library_name} ALIAS stb-${library_name})
endfunction()

set(libraries

  # vorbis This is a .c file for some reason?
  hexwave
  image
  truetype
  image_write # The library is called image_writer but the header file is called image_write
  image_resize
  rect_pack
  perlin
  ds
  sprintf
  # textedit Too many definitions needed so leave it out for now
  # voxel_render
  dxt
  easy_font
  # tilemap_editor
  herringbone_wang_tile
  c_lexer
  divide
  # connected_components
  leakcheck
  include
)

foreach(library ${libraries})
  add_stb_library(${library})
endforeach()
