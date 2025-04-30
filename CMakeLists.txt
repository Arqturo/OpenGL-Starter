cmake_minimum_required(VERSION 3.20.0 FATAL_ERROR)

project(OpenGLStarter)

# Add all Source Files
file(GLOB_RECURSE SOURCE_FILES 
    ${CMAKE_SOURCE_DIR}/src/*.cpp
)

# Add all Header Files
file(GLOB_RECURSE HEADER_FILES 
    ${CMAKE_SOURCE_DIR}/src/*.h
    ${CMAKE_SOURCE_DIR}/src/*.hpp
)

add_executable(${PROJECT_NAME} ${HEADER_FILES} ${SOURCE_FILES})

# Find the Required Packages (if Any -> OpenGL/Vulkan or Any)
find_package(OpenGL REQUIRED)

# Variable for the Libs to add to the Linkers
if (WIN32)
    set(LIBS glfw opengl32 glad)
elseif (UNIX)
    set(LIBS glfw GL glad)
endif ()

# Variables for Paths of External Libraries
set(GLFW_ROOT_DIR external/glfw)
set(GLAD_ROOT_DIR external/glad)

# Add the External Libraries / Files Directory
add_subdirectory(${GLFW_ROOT_DIR})
add_subdirectory(${GLAD_ROOT_DIR})


# Add the Link Directories for the Libraries / Files
target_link_directories(${PROJECT_NAME} 
    PRIVATE ${GLFW_ROOT_DIR}/src
    PRIVATE ${GLAD_ROOT_DIR}/src
)

target_include_directories(${PROJECT_NAME} 
    PUBLIC ${GLFW_ROOT_DIR}/include
    PUBLIC ${GLAD_ROOT_DIR}/include
)

# Add the Libs to the Linker for the Project
target_link_libraries(${PROJECT_NAME} ${LIBS})
