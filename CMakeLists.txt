cmake_minimum_required(VERSION 2.8)
project(CMake-DocSet)

set(CMAKE_INSTALL_PREFIX ${CMAKE_BINARY_DIR})

set(CMAKE_DOCSET_ROOT CMake.docset)

find_program( SQLITE_COMMAND NAMES sqlite3 sqlite )
if(EXISTS ${CMAKE_BINARY_DIR}/docSet.dsidx)
	file(REMOVE ${CMAKE_BINARY_DIR}/docSet.dsidx)
endif()

install(CODE "execute_process(COMMAND echo .quit
                              COMMAND ${SQLITE_COMMAND} -init ${CMAKE_CURRENT_SOURCE_DIR}/schema.sql ${CMAKE_BINARY_DIR}/docSet.dsidx )")



macro(ADD_SQLITE_INDEX type list docset)
	set(CMAKE_HELP_TYPE ${type})
	set(CMAKE_HELP_LIST ${list})
	set(CMAKE_HELP_DOCSET_TYPE ${docset})
	configure_file(${CMAKE_CURRENT_SOURCE_DIR}/index.sh.in ${CMAKE_BINARY_DIR}/index_${CMAKE_HELP_TYPE}.sh)
	install(CODE "execute_process(COMMAND ${CMAKE_BINARY_DIR}/index_${CMAKE_HELP_TYPE}.sh 
                  COMMAND ${SQLITE_COMMAND} ${CMAKE_BINARY_DIR}/docSet.dsidx )")
	install(CODE "execute_process(COMMAND ${CMAKE_COMMAND}  --help-${list} ${CMAKE_BINARY_DIR}/cmake-${list}.html)")
	install(FILES ${CMAKE_BINARY_DIR}/cmake-${list}.html DESTINATION ${CMAKE_DOCSET_ROOT}/Contents/Resources/Documents/)
endmacro(ADD_SQLITE_INDEX)

ADD_SQLITE_INDEX(command commands Command)
ADD_SQLITE_INDEX(property properties Property)
ADD_SQLITE_INDEX(variable variables Variable)
ADD_SQLITE_INDEX(module modules Module)


install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/icon.png DESTINATION ${CMAKE_DOCSET_ROOT})
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/Info.plist DESTINATION ${CMAKE_DOCSET_ROOT}/Contents)
install(FILES ${CMAKE_BINARY_DIR}/docSet.dsidx DESTINATION ${CMAKE_DOCSET_ROOT}/Contents/Resources/)

set(CPACK_GENERATOR "TGZ")
set(CPACK_PACKAGE_FILE_NAME "CMake")
set(CPACK_INCLUDE_TOPLEVEL_DIRECTORY False)
include(CPack)


