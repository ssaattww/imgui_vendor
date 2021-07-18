cmake_minimum_required(VERSION 3.5)
project(imgui_vendor)

find_package(ament_cmake REQUIRED)

include(ExternalProject)

ExternalProject_Add(imgui_docking
    GIT_REPOSITORY https://github.com/ocornut/imgui
    GIT_TAG a11f36811f1bd4db1d6b30cb1d34022294ba28ec
    TIMEOUT 600
    CMAKE_ARGS
        -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/imgui_docking
        ${extra_cmake_args}
        -Wno-dev
    )

SOURCES += $(IMGUI_DIR)/imgui.cpp $(IMGUI_DIR)/imgui_demo.cpp $(IMGUI_DIR)/imgui_draw.cpp $(IMGUI_DIR)/imgui_tables.cpp $(IMGUI_DIR)/imgui_widgets.cpp
SOURCES += $(IMGUI_DIR)/backends/imgui_impl_glfw.cpp $(IMGUI_DIR)/backends/imgui_impl_opengl3.cpp

# The external project will install to the build folder, but we'll install that on make install.
install(DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/imgui_docking/
    DESTINATION ${CMAKE_INSTALL_PREFIX}/opt/imgui_vendor
    USE_SOURCE_PERMISSIONS
)