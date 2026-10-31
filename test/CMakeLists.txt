cmake_minimum_required(VERSION 4.0.0)

project(X CXX)

find_package(ImPlot CONFIG REQUIRED)
find_package(ImPlot3D CONFIG REQUIRED)
find_package(imgui_markdown CONFIG REQUIRED)
find_package(imgui_club CONFIG REQUIRED)
find_package(ImGuiFileDialog CONFIG REQUIRED)
find_package(ImGuizmo CONFIG REQUIRED)
find_package(ImGuiColorTextEdit CONFIG REQUIRED)
find_package(imfilebrowser CONFIG REQUIRED)
find_package(imgui-node-editor CONFIG REQUIRED)

add_executable(test main.cpp)
target_link_libraries(test
  PRIVATE Unofficial::ImPlot::ImPlot
  PRIVATE Unofficial::ImPlot3D::ImPlot3D
  PRIVATE Unofficial::imgui_markdown::imgui_markdown
  PRIVATE Unofficial::imgui_club::imgui_club
  PRIVATE Unofficial::ImGuizmo::ImGuizmo
  PRIVATE Unofficial::ImGuiFileDialog::ImGuiFileDialog
  PRIVATE Unofficial::ImGuiColorTextEdit::ImGuiColorTextEdit
  PRIVATE Unofficial::imfilebrowser::imfilebrowser
  PRIVATE Unofficial::imgui_node_editor::imgui_node_editor)

install(TARGETS test)
