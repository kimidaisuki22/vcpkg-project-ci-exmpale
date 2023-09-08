cmake_minimum_required (VERSION 3.24)
set(CMAKE_CXX_STANDARD 20)

project("No_name")

add_compile_options("/utf-8")

find_package(fmt CONFIG REQUIRED)
add_executable(main main.cpp)
target_link_libraries(main PRIVATE fmt::fmt)



include (GNUInstallDirs)
install(TARGETS main
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
 )

include(CPack)