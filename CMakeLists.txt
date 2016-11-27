cmake_minimum_required(VERSION 2.8)

project(alw)

add_library(alw STATIC src/alw.c)
target_include_directories(alw PUBLIC include)

if (UNIX)
  target_link_libraries(alw PUBLIC dl)
endif()

option(ALW_INSTALL_TARGET "Install the alw target" ON)
if (ALW_INSTALL_TARGET)
	install(TARGETS alw ARCHIVE DESTINATION lib)
	install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/" DESTINATION include)
endif ()
