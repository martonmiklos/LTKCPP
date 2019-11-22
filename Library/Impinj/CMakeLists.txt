add_library(ltkcppimpinj
  impinj_ltkcpp_genout.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/out_impinj_ltkcpp.inc
  ${CMAKE_CURRENT_BINARY_DIR}/out_impinj_ltkcpp.h
  )

set_target_properties(ltkcppimpinj PROPERTIES
  VERSION "${PROJECT_VERSION}"
  SOVERSION "${PROJECT_VERSION}"
  )

target_include_directories(ltkcppimpinj PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
  $<INSTALL_INTERFACE:include/ltkcpp>
  )
target_link_libraries(ltkcppimpinj PRIVATE ltkcpp)

set(IMPINJDEF "${DEFINITIONS_PATH}/Impinj/Impinjdef.xml")
generate_code(Impinj ${IMPINJDEF} out_impinj_ltkcpp.inc)

install(TARGETS
  ltkcppimpinj
  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  )
install(FILES
  impinj_ltkcpp.h
  ${CMAKE_CURRENT_BINARY_DIR}/out_impinj_ltkcpp.h
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  )