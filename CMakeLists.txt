cmake_minimum_required(VERSION 3.16)
project(imgui LANGUAGES CXX)

# 1. 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
    
# 1. 显式定义核心源文件 (Step 4: Add core)
set(IMGUI_CORE_SOURCES
    "imconfig.h"
    "imgui.h"
    "imgui.cpp"
    "imgui_draw.cpp"
    "imgui_internal.h"
    "imgui_tables.cpp"
    "imgui_widgets.cpp"
    "imstb_rectpack.h"
    "imstb_textedit.h"
    "imstb_truetype.h"
    "imgui_demo.cpp"
)

# =================================================================
# 2. 显式定义后端文件 (Step 4: Add backends)
# =================================================================
set(IMGUI_BACKEND_SOURCES
    "backends/imgui_impl_glfw.h"
    "backends/imgui_impl_glfw.cpp"
    "backends/imgui_impl_opengl3.h"
    "backends/imgui_impl_opengl3.cpp"
    "backends/imgui_impl_opengl3_loader.h"
)

# =================================================================
# 3. 显式定义扩展工具 (Step: Misc)
# =================================================================
set(IMGUI_MISC_SOURCES
    "misc/cpp/imgui_stdlib.h"
    "misc/cpp/imgui_stdlib.cpp"
)

set(GLFW_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/../glfw/include")

# 3. 定义静态库
add_library(imgui STATIC 
    ${IMGUI_CORE_SOURCES} 
    ${IMGUI_BACKEND_SOURCES}
    ${IMGUI_MISC_SOURCES}
)

# 2. 设置 C++ 标准
set_target_properties(imgui PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
    CXX_EXTENSIONS OFF
)


# 4. 配置包含路径 (等同于外部引用的 include 目录)
target_include_directories(imgui PUBLIC 
    "${CMAKE_CURRENT_SOURCE_DIR}"             # 让主项目能直接 #include <imgui.h>
    "${CMAKE_CURRENT_SOURCE_DIR}/backends"    # 让主项目能直接 #include <imgui_impl_glfw.h>
    "${CMAKE_CURRENT_SOURCE_DIR}/misc/cpp"    # 方便使用 std::string 扩展
    "${GLFW_INCLUDE_DIR}"  # <--- 关键：把 GLFW 的 include 文件夹直接给 ImGui
)

target_link_libraries(imgui PUBLIC glfw Glad)
