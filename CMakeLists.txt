cmake_minimum_required(VERSION 2.8)
PROJECT(simplecalc)


# copy parser.y to build directory. We want a shaddow build that does not litter
# the source directory
add_custom_command(
    OUTPUT ${CMAKE_BINARY_DIR}/parser.y
    COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_SOURCE_DIR}/parser.y ${CMAKE_BINARY_DIR}/parser.y
    DEPENDS ${CMAKE_SOURCE_DIR}/parser.y)

# run lemon
add_custom_target(parser
    #OUTPUT ${CMAKE_BINARY_DIR}/parser.c   OUTPUT ${CMAKE_BINARY_DIR}/parser.c
    COMMAND lemon ${CMAKE_BINARY_DIR}/parser.y
    DEPENDS ${CMAKE_BINARY_DIR}/parser.y
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)

# run ragel
add_custom_command(
    OUTPUT ${CMAKE_BINARY_DIR}/lexer.cpp
    COMMAND ragel ${CMAKE_SOURCE_DIR}/lexer.rl -o ${CMAKE_BINARY_DIR}/lexer.cpp
)


# build executable
add_executable(calc ${CMAKE_BINARY_DIR}/lexer.cpp)

add_dependencies(calc parser )
