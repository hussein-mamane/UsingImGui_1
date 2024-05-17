cmake_minimum_required(VERSION 3.28)
project(usingDearImGui_1)

set(CMAKE_CXX_STANDARD 11)
cmake_policy(SET CMP0072 NEW) # this is not needed in CLion (that I use) but it is
                              # to select the newest OpenGL that is found
# glfw3 is here
include_directories(${CMAKE_SOURCE_DIR}/inc)
link_directories(${CMAKE_SOURCE_DIR}/lib)
#include() #load and run another CMake code
# for building the libimgui.a, cmake  will search in cmake-build-debug,
# it will not find imgui files, so add them globally
include_directories(${CMAKE_SOURCE_DIR}/imgui)
include_directories(${CMAKE_SOURCE_DIR}/imgui/backends)

#set(IMGUI_SRC ${CMAKE_SOURCE_DIR}/imguiGL/)
#set(DEPENDENCIES ${CMAKE_SOURCE_DIR}/)


# -------------- IMGUI static library creation`
#

set(IMGUI_PATH ${CMAKE_SOURCE_DIR}/imgui)
file(GLOB IMGUI_GLOB
        ${IMGUI_PATH}/imgui.h
        ${IMGUI_PATH}/imgui.cpp
        ${IMGUI_PATH}/imconfig.h
        ${IMGUI_PATH}/imgui_demo.cpp
        ${IMGUI_PATH}/imgui_draw.cpp
        ${IMGUI_PATH}/imgui_internal.h
        ${IMGUI_PATH}/imstb_rectpack.h
        ${IMGUI_PATH}/imstb_textedit.h
        ${IMGUI_PATH}/imstb_truetype.h
        ${IMGUI_PATH}/imgui_tables.cpp
        ${IMGUI_PATH}/imgui_widgets.cpp

        # specific bindings...
        ${IMGUI_PATH}/backends/imgui_impl_glfw.h
        ${IMGUI_PATH}/backends/imgui_impl_glfw.cpp
        ${IMGUI_PATH}/backends/imgui_impl_opengl3.h
        ${IMGUI_PATH}/backends/imgui_impl_opengl3.cpp
        ${IMGUI_PATH}/backends/imgui_impl_opengl3_loader.cpp)

add_library("imgui" STATIC ${IMGUI_GLOB}) # build a libimgui.a

find_package(OpenGL REQUIRED)
add_executable(
        usingDearImGui_1 main.cpp
#        ${IMGUI_SRC} # not needed
#        ${DEPENDENCIES}# not needed
)

add_executable(opengl1 testogl.cpp dependencies_code/glad.c dependencies_code/stb.cpp)

#add_executable(usingDearImGui_1_2 main2.cpp)
target_include_directories( usingDearImGui_1 PUBLIC ${IMGUI_PATH})
target_include_directories( opengl1 PUBLIC ${IMGUI_PATH})
target_link_libraries(usingDearImGui_1 PUBLIC imgui glfw3 ${OPENGL_LIBRARIES})
target_link_libraries(opengl1 PUBLIC imgui glfw3 ${OPENGL_LIBRARIES})
#target_link_libraries(imgui PRIVATE glfw3)

# imgui is here
#include_directories(${CMAKE_SOURCE_DIR}/imgui)
#include_directories(${CMAKE_SOURCE_DIR}/imgui/backends)
#----------------------------------------------------#
## Set where the ImGui files are stored
#set(IMGUI_PATH ${CMAKE_SOURCE_DIR}/imgui)
##
### Compile as static library
#file(GLOB IMGUI_SOURCES ${IMGUI_PATH}/*.cpp)
#add_library("ImGui" STATIC ${IMGUI_SOURCES})
#target_include_directories("ImGui" PUBLIC ${IMGUI_PATH})
#
##
## excluding vulkan, which is in its own folder
#file(GLOB IMGUI_SOURCES_BACKENDS ${IMGUI_PATH}/backends/*.cpp)
#add_library("ImGuiBackends" STATIC ${IMGUI_SOURCES_BACKENDS})
#target_include_directories("ImGuiBackends" PUBLIC ${IMGUI_PATH}/backends ${IMGUI_PATH})
#----------------------------------------------------#
