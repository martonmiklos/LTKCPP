set(LTKCPP_CODE_GEN_H_XSLT "${CMAKE_CURRENT_LIST_DIR}/ltkcpp_gen_h.xslt")
set(LTKCPP_CODE_GEN_CPP_XSLT "${CMAKE_CURRENT_LIST_DIR}/ltkcpp_gen_cpp.xslt")

function(generate_code registry_name source_file output_file)
  get_filename_component(output_file ${output_file} NAME_WE)
  set(output_h ${output_file}.h)
  set(output_inc ${output_file}.inc)
  add_custom_command(
    OUTPUT ${output_h}
    COMMENT "Generating ${output_h}"
    COMMAND ${LIBXSLT_XSLTPROC_EXECUTABLE} --stringparam RegistryName ${registry_name} -o ${output_h} ${LTKCPP_CODE_GEN_H_XSLT} ${source_file}
    DEPENDS ${LTKCPP_CODE_GEN_H_XSLT} ${source_file})
  add_custom_command(
    OUTPUT ${output_inc}
    COMMENT "Generating ${output_inc}"
    COMMAND ${LIBXSLT_XSLTPROC_EXECUTABLE} --stringparam RegistryName ${registry_name} -o ${output_inc} ${LTKCPP_CODE_GEN_CPP_XSLT} ${source_file}
    DEPENDS ${LTKCPP_CODE_GEN_CPP_XSLT} ${source_file})
endfunction()

configure_file(version.inc.in ${CMAKE_CURRENT_BINARY_DIR}/version.inc @ONLY)

add_library(
  ltkcpp
  ltkcpp_array.cpp
  ltkcpp_connection.cpp
  ltkcpp_element.cpp
  ltkcpp_encdec.cpp
  ltkcpp_error.cpp
  ltkcpp_framedecode.cpp
  ltkcpp_frameencode.cpp
  ltkcpp_frameextract.cpp
  ltkcpp_hdrfd.cpp
  ltkcpp_xmltextencode.cpp
  ltkcpp_xmltextdecode.cpp
  ltkcpp_typeregistry.cpp
  ltkcpp_genout.cpp
  ltkcpp.h
  ltkcpp_base.h
  ltkcpp_connection.h
  ltkcpp_frame.h
  ltkcpp_platform.h
  ltkcpp_xmltext.h
  ${CMAKE_CURRENT_BINARY_DIR}/out_ltkcpp.inc
  ${CMAKE_CURRENT_BINARY_DIR}/out_ltkcpp.h
  ${CMAKE_CURRENT_BINARY_DIR}/version.inc
  )

target_include_directories(ltkcpp PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}>
  $<BUILD_INTERFACE:${LIBXML2_INCLUDE_DIR}>
  $<INSTALL_INTERFACE:include/ltkcpp>
  )
target_link_libraries(ltkcpp ${LIBXML2_LIBRARIES})
set_target_properties(ltkcpp PROPERTIES
  VERSION "${PROJECT_VERSION}"
  SOVERSION "${PROJECT_VERSION}"
  )

set(LLRPDEF "${DEFINITIONS_PATH}/Core/llrp-1x0-def.xml")
generate_code(Core ${LLRPDEF} out_ltkcpp.inc)

if(LTKCPP_BUILD_IMPINJ)
  add_subdirectory(Impinj)
endif()
if(LTKCPP_BUILD_LLRPORG)
  add_subdirectory(LLRP.org)
endif()

install(TARGETS
  ltkcpp
  DESTINATION ${CMAKE_INSTALL_LIBDIR}
  )
install(FILES
  ltkcpp.h
  ltkcpp_base.h
  ltkcpp_connection.h
  ltkcpp_frame.h
  ltkcpp_platform.h
  ltkcpp_xmltext.h
  ${CMAKE_CURRENT_BINARY_DIR}/out_ltkcpp.h
  ${CMAKE_CURRENT_BINARY_DIR}/version.inc
  DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  )