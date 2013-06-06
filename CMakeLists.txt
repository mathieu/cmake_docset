cmake_minimum_required(VERSION 2.8)
project(CMake-DocSet)

set(CMAKE_DOCSET_ROOT CMake.docset/Contents)
set(CMAKE_DOCSET_DOCUMENT_ROOT ${CMAKE_DOCSET_ROOT})

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
	install(FILES ${CMAKE_BINARY_DIR}/cmake-${list}.html DESTINATION ${CMAKE_DOCSET_ROOT}/Resources/Documents/)
endmacro(ADD_SQLITE_INDEX)

ADD_SQLITE_INDEX(command commands Command)
ADD_SQLITE_INDEX(property properties Property)
ADD_SQLITE_INDEX(variable variables Variable)
ADD_SQLITE_INDEX(module modules Module)


install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/Info.plist DESTINATION ${CMAKE_DOCSET_ROOT})
install(FILES ${CMAKE_BINARY_DIR}/docSet.dsidx DESTINATION ${CMAKE_DOCSET_ROOT}/Resources/)