cmake_minimum_required(VERSION 4.0.0)

project(X CXX)

find_package(ImPlot REQUIRED)
find_package(ImPlot3D REQUIRED)
find_package(imgui_markdown REQUIRED)
find_package(imgui_club REQUIRED)
find_package(ImGuiFileDialog REQUIRED)

add_executable(a a.cpp)
target_link_libraries(a
  PRIVATE Unofficial::ImPlot::implot
  PRIVATE Unofficial::ImPlot3D::implot3d
  PRIVATE Unofficial::imgui_markdown::imgui_markdown
  PRIVATE Unofficial::imgui_club::imgui_memory_editor
  PRIVATE Unofficial::imgui_club::imgui_threaded_rendering
  PRIVATE Unofficial::imgui_club::imgui_multicontext_compositor
  PRIVATE Unofficial::ImGuiFileDialog::imguifiledialog)
