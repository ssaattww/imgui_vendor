cmake_minimum_required(VERSION 3.5)
project(imgui_vendor)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

find_package(ament_cmake REQUIRED)

set(OpenGL_GL_PREFERENCE GLVND)

find_package(glfw3 3.3 REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)

add_library(imgui SHARED
    imgui/imgui.cpp
    imgui/imgui_demo.cpp
    imgui/imgui_draw.cpp
    imgui/imgui_tables.cpp
    imgui/imgui_widgets.cpp
    imgui/backends/imgui_impl_glfw.cpp
    imgui/backends/imgui_impl_opengl3.cpp)
target_include_directories(imgui PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/imgui>
    $<INSTALL_INTERFACE:include>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/imgui/backends>
    $<INSTALL_INTERFACE:include>)

target_link_libraries(imgui PUBLIC glfw OpenGL::GL GLEW::GLEW)
target_compile_definitions(imgui PUBLIC IMGUI_DISABLE_INCLUDE_IMCONFIG_H)
ament_export_targets(export_imgui HAS_LIBRARY_TARGET)

add_executable(imgui_example src/main.cpp)
target_link_libraries(imgui_example PRIVATE imgui)

ament_export_include_directories(include)
ament_export_libraries(imgui)
ament_export_dependencies(glfw3 OpenGL GLEW)

install(FILES
  imgui/imgui.h
  imgui/imgui_internal.h
  imgui/imstb_textedit.h
  imgui/backends/imgui_impl_opengl3.h
  imgui/backends/imgui_impl_glfw.h
  DESTINATION include)

install(TARGETS imgui EXPORT export_imgui
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION lib/${PROJECT_NAME}
)

# install(TARGETS imgui_example
#   ARCHIVE DESTINATION lib
#   LIBRARY DESTINATION lib
#   RUNTIME DESTINATION lib/${PROJECT_NAME}
# )

ament_package()
