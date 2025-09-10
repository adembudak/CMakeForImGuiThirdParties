cmake_minimum_required(VERSION 4.0.0)

project(X CXX)

find_package(ImPlot REQUIRED)
find_package(ImPlot3D REQUIRED)
find_package(imgui_markdown REQUIRED)
find_package(imgui_club REQUIRED)
find_package(ImGuiFileDialog REQUIRED)

add_executable(a a.cpp)
target_link_libraries(a
  PRIVATE Unofficial::ImPlot::ImPlot
  PRIVATE Unofficial::ImPlot3D::ImPlot3D
  PRIVATE Unofficial::imgui_markdown::imgui_markdown
  PRIVATE Unofficial::imgui_club::imgui_club
  PRIVATE Unofficial::ImGuiFileDialog::imguifiledialog)
