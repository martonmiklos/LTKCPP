find_program(DOXYGEN_COMMAND doxygen)

if(DEFINED DOXYGEN_COMMAND)
  set(LTKCPP_DOC_DEPENDS ltkcpp ${CMAKE_CURRENT_BINARY_DIR}/ltkcpp.dox)
  if(LTKCPP_BUILD_IMPINJ)
    list(APPEND LTKCPP_DOC_DEPENDS ltkcppimpinj)
  endif()
  if(LTKCPP_BUILD_LLRPORG)
    list(APPEND LTKCPP_DOC_DEPENDS ltkcppllrporg)
  endif()

  set(LTKCPP_DOXYGEN_INPUT_DIR ${CMAKE_CURRENT_LIST_DIR}/..)
  set(LTKCPP_DOXYGEN_OUTPUT_DIR ${CMAKE_CURRENT_BINARY_DIR}/Documentation)
  configure_file(ltkcpp.dox.in ${CMAKE_CURRENT_BINARY_DIR}/ltkcpp.dox @ONLY)

  add_custom_target(docs
    COMMENT "Generating documentation"
    DEPENDS ${LTKCPP_DOC_DEPENDS}
    COMMAND ${CMAKE_COMMAND} -E remove ${CMAKE_CURRENT_BINARY_DIR}/ltkcpp_dox.log
    COMMAND ${CMAKE_COMMAND} -E remove_directory ${CMAKE_CURRENT_BINARY_DIR}/Documentation
    COMMAND ${CMAKE_COMMAND} -E make_directory ${CMAKE_CURRENT_BINARY_DIR}/Documentation
    COMMAND ${DOXYGEN_COMMAND} ${CMAKE_CURRENT_BINARY_DIR}/ltkcpp.dox
  )
endif()