[CMake](https://cmake.org) build script support some thirdparty [Dear ImGui](https://github.com/ocornut/imgui) projects build with [CMakeForImGui](https://github.com/adembudak/CMakeForImGui).

So far following projects are supported:

| Project | Variable must be set | Build option(s) | Package name  | Generated target(s) |
|---------|----------------------|-----------------|---------------|---------------------|
| [imgui_club](https://github.com/ocornut/imgui_club) | IMGUI_CLUB_SOURCE_DIR | imgui_club<br>imgui_memory_editor<br>imgui_multicontext_compositor<br>imgui_threaded_rendering | `imgui_club` | `Unofficial::imgui_club::imgui_club` |
| [imgui_markdown](https://github.com/enkisoftware/imgui_markdown) | IMGUI_MARKDOWN_SOURCE_DIR | imgui_markdown | `imgui_markdown` | `Unofficial::imgui_markdown::imgui_markdown` |
| [ImPlot](https://github.com/epezent/implot) | IMPLOT_SOURCE_DIR | implot | `ImPlot` | `Unofficial::ImPlot::ImPlot` |
| [ImPlot3D](https://github.com/brenocq/implot3d) | IMPLOT3D_SOURCE_DIR | implot3d | `ImPlot3D` | `Unofficial::ImPlot3D::ImPlot3D` |
| [ImGuiFileDialog](https://github.com/aiekick/ImGuiFileDialog) | IMGUIFILEDIALOG_SOURCE_DIR | imguifiledialog | `ImGuiFileDialog` | `Unofficial::ImGuiFileDialog::ImGuiFileDialog` |
| [ImGuizmo](https://github.com/CedricGuillemet/ImGuizmo) | IMGUIZMO_SOURCE_DIR | imguizmo | `ImGuizmo` | `Unofficial::ImGuizmo::ImGuizmo` |

---
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![GitHub Actions](https://github.com/adembudak/CMakeForImGuiThirdParties/actions/workflows/main.yml/badge.svg)](https://github.com/adembudak/CMakeForImGuiThirdParties/actions/workflows/main.yml)
