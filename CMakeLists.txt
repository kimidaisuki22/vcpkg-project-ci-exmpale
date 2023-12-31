﻿cmake_minimum_required (VERSION 3.24)
set(CMAKE_CXX_STANDARD 20)

project("No_name")

if(MSVC)
add_compile_options("/utf-8") # Fix fmt header that container utf-8 character, SAD
endif()

find_package(fmt CONFIG REQUIRED)
add_executable(main main.cpp)
target_link_libraries(main PRIVATE fmt::fmt)



include (GNUInstallDirs)
install(TARGETS main
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
 )

function(install_dll target)
set(TEMPLATE_STRING [[
 file(GET_RUNTIME_DEPENDENCIES
  RESOLVED_DEPENDENCIES_VAR seta
  EXECUTABLES $<TARGET_FILE:@target@>
  PRE_EXCLUDE_REGEXES ext-ms-* api-ms-*
  DIRECTORIES $<TARGET_FILE_DIR:@target@>
  )

   foreach(_file ${seta})
    message(STATUS "Installing ${_file} ...")
    file(INSTALL
      DESTINATION "${CMAKE_INSTALL_PREFIX}/bin"
      TYPE SHARED_LIBRARY
      FOLLOW_SYMLINK_CHAIN
      FILES "${_file}"
    )
  endforeach()
  list(LENGTH _u_deps _u_length)
  if("${_u_length}" GREATER 0)
    message(WARNING "Unresolved dependencies detected!")
  endif()
]])

string(REPLACE "@target@" "${target}" TEMPLATE_STRING "${TEMPLATE_STRING}")
install(CODE ${TEMPLATE_STRING})
endfunction()

install_dll(main)
# Guide: https://stackoverflow.com/questions/62884439/how-to-use-cmake-file-get-runtime-dependencies-in-an-install-statement

include(CPack)