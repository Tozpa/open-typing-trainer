cmake_minimum_required(VERSION 3.10)
project(TypingApp CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")  # Optimization for compilation

# Define subsystem to prevent console window from opening on Windows
if (WIN32)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -mwindows")
endif()

# Add ImGui source files
set(IMGUI_FILES
    include/imgui/imgui.cpp
    include/imgui/imgui_draw.cpp
    include/imgui/imgui_widgets.cpp
    include/imgui/imgui_tables.cpp
    include/imgui/backends/imgui_impl_glfw.cpp
    include/imgui/backends/imgui_impl_opengl3.cpp
)

# Include ImGui and GLFW directories
include_directories(include/imgui include/imgui/backends external/glfw/include)

# Add GLFW
set(DOXYGEN_FOUND FALSE)
add_subdirectory(external/glfw)

# Add executable before linking libraries
add_executable(typing_app src/main.cpp ${IMGUI_FILES})

# Detect Windows and link appropriate libraries
if (WIN32)
    target_link_libraries(typing_app glfw opengl32)
else()
    target_link_libraries(typing_app glfw GL)
endif()