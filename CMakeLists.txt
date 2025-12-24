cmake_minimum_required(VERSION 3.16)
project(ToDoList LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

file(GLOB TODOLIST_SOURCES CONFIGURE_DEPENDS ToDoListProgram/src/*.cpp)

add_subdirectory(extern/wxWidgets EXCLUDE_FROM_ALL)

# Where executables should be placed
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Where libraries should be placed (if you ever build any)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

add_executable(ToDoList ${TODOLIST_SOURCES})

if(WIN32)
    add_custom_command(TARGET ToDoList POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_CURRENT_BINARY_DIR}/extern/wxWidgets/lib/gcc_x64_dll
        $<TARGET_FILE_DIR:ToDoList>
    )
endif()

target_link_libraries(ToDoList PRIVATE
    wx::core
    wx::base
)

target_include_directories(ToDoList PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/extern/wxWidgets/include
    ${CMAKE_CURRENT_SOURCE_DIR}/ToDoListProgram/include
)