cmake_minimum_required(VERSION 3.10)

project(Liftee VERSION 0.1)

set(NUKLEAR_LIB_PATH "" CACHE PATH "Path to nuklear lib root folder")
set(GLFW_LIB_PATH "" CACHE PATH "Path to glfw bin folder")
set(GLEW_LIB_PATH "" CACHE PATH "Path to glew bin folder")
set(GLM_LIB_PATH "" CACHE PATH "Path to glm folder")

add_executable(Liftee main.cpp)

configure_file(LifteeConfig.h.in LifteeConfig.h)

if("${CMAKE_SIZEOF_VOID_P}" STREQUAL "4")
    set(PLATFORM_ARCH "x86")
else()
    set(PLATFORM_ARCH "x64")
endif()

set(SPECIALIZED_GLEW_LIB_PATH "${CMAKE_BUILD_TYPE}/${PLATFORM_ARCH}")

target_include_directories(Liftee PUBLIC
    "${PROJECT_BINARY_DIR}"
    "${NUKLEAR_LIB_PATH}"
    "${GLFW_LIB_PATH}/include"
    "${GLEW_LIB_PATH}/include"
    "${GLM_LIB_PATH}"
    )

target_link_directories(Liftee PUBLIC
    "${GLFW_LIB_PATH}/lib"
    "${GLEW_LIB_PATH}/lib"
    )

target_link_libraries(Liftee OpenGL32.lib)
target_link_libraries(Liftee debug ${SPECIALIZED_GLEW_LIB_PATH}/glew32d.lib debug ${SPECIALIZED_GLEW_LIB_PATH}/glew32sd.lib debug glfw3d.lib)
target_link_libraries(Liftee optimized ${SPECIALIZED_GLEW_LIB_PATH}/glew32.lib optimized ${SPECIALIZED_GLEW_LIB_PATH}/glew32s.lib optimized glfw3.lib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)