project(tpm-tools)
cmake_minimum_required (VERSION 3.5)

include(ExternalProject)

ExternalProject_Add(
  tpm-tools
  SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/
  CONFIGURE_COMMAND ./bootstrap.sh COMMAND ./configure --prefix=/usr/local 
  BUILD_COMMAND $(MAKE) CPPFLAGS=-DTGRUB_DEBUG COMMAND echo 'done'
  INSTALL_COMMAND $(MAKE) DESTDIR=${CMAKE_CURRENT_BINARY_DIR}/tpm-tools install
  BUILD_IN_SOURCE 1
)

add_custom_target(
  tpm-tools.clean
  COMMAND $(MAKE) -C ${CMAKE_CURRENT_SOURCE_DIR} distclean || true
  COMMAND rm -rf ${CMAKE_CURRENT_BINARY_DIR} || true
)

install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/tpm-tools/ DESTINATION / USE_SOURCE_PERMISSIONS COMPONENT boot)
