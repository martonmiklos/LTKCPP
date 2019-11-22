add_library(ltkcppllrporg
  llrporg_ltkcpp_genout.cpp
  ${CMAKE_CURRENT_BINARY_DIR}/out_llrporg_ltkcpp.inc
  ${CMAKE_CURRENT_BINARY_DIR}/out_llrporg_ltkcpp.h
  )

set_target_properties(ltkcppllrporg PROPERTIES
  VERSION "${PROJECT_VERSION}"
  SOVERSION "${PROJECT_VERSION}"
  )

target_include_directories(ltkcppllrporg PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
  $<INSTALL_INTERFACE:include/ltkcpp>
  )
target_link_libraries(ltkcppllrporg PRIVATE ltkcpp)

set(LLRPORGDEF "${DEFINITIONS_PATH}/LLRP.org/LLRPOrgExampleDef.xml")
generate_code(LLRPOrg ${LLRPORGDEF} out_llrporg_ltkcpp.inc)

install(TARGETS
  ltkcppllrporg
  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  )
install(FILES
  llrporg_ltkcpp.h
  ${CMAKE_CURRENT_BINARY_DIR}/out_llrporg_ltkcpp.h
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  )