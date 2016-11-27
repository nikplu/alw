cmake_minimum_required(VERSION 2.8)

project(alw)

find_package(PythonInterp 2.7 REQUIRED)

set(OPENAL_API_HEADERS
	"include/AL/al.h"
	"include/AL/alc.h"
	"include/AL/efx.h")

set(ALW_SOURCES
	"${CMAKE_CURRENT_BINARY_DIR}/src/alw.c"
	"${CMAKE_CURRENT_BINARY_DIR}/include/AL/alw.h")

# generate alw.c/alw.h
add_custom_command(
	OUTPUT ${ALW_SOURCES}
	COMMAND ${PYTHON_EXECUTABLE} "${CMAKE_CURRENT_SOURCE_DIR}/alw_gen.py" "--destdir" "${CMAKE_CURRENT_BINARY_DIR}"
	DEPENDS ${OPENAL_API_HEADERS}
	WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}"
	COMMENT "Generating alw.c/alw.h")

set_source_files_properties(${ALW_SOURCES} PROPERTIES GENERATED True)

add_custom_target(gen_alw DEPENDS ${ALW_SOURCES})

add_library(alw STATIC ${ALW_SOURCES})
add_dependencies(alw gen_alw)
target_include_directories(alw PUBLIC "include" "${CMAKE_CURRENT_BINARY_DIR}/include")

if (UNIX)
  target_link_libraries(alw PUBLIC dl)
endif()

option(ALW_INSTALL_TARGET "Install the alw target" ON)
if (ALW_INSTALL_TARGET)
	install(TARGETS alw ARCHIVE DESTINATION lib)
	install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/" DESTINATION include)
endif ()
