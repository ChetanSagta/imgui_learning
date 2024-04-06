cmake_minimum_required(VERSION 3.22.1)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "-g -Wall -pedantic -Wextra -Werror -Wshadow")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

project(imgui)
# Search pkg-config utility first
find_package(PkgConfig REQUIRED)
find_package(SDL2 REQUIRED COMPONENTS SDL2::TTF)
find_package(spdlog REQUIRED)
PKG_SEARCH_MODULE(SDL2TTF REQUIRED SDL2_ttf>=2.0.0)

set(IMGUI_SOURCES imgui/imgui.cpp imgui/imgui_demo.cpp imgui/imgui_draw.cpp imgui/imgui_widgets.cpp)

set(SOURCES src/main.cpp)
set(IMGUI_PATH "imgui")
# set(IMGUI_HEADER_DIR imgui)


file(GLOB IMGUI_SOURCES ${IMGUI_PATH}/*.cpp)
add_library("ImGui" STATIC ${IMGUI_SOURCES})
target_include_directories("ImGui" PUBLIC ${IMGUI_PATH})
add_executable(${PROJECT_NAME} ${SOURCES} ${IMGUI_SOURCES})

target_link_libraries(${PROJECT_NAME} PRIVATE ${SDL2_LIBRARIES})
target_link_libraries(${PROJECT_NAME} PRIVATE ${SDL2TTF_LIBRARIES})
target_link_libraries(${PROJECT_NAME} PRIVATE spdlog::spdlog)
target_link_libraries(${PROJECT_NAME} PRIVATE ImGui)
