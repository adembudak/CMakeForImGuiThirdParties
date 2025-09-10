[CMake](https://cmake.org) build script support some thirdparty [Dear ImGui](https://github.com/ocornut/imgui) projects build with [CMakeForImGui](https://github.com/adembudak/CMakeForImGui).

So far following projects are supported:

| Project | Variable must be set | Build option(s) | Generated target(s) |
|--------|---------------------|-----------------|---------------------|
| [imgui_club](https://github.com/ocornut/imgui_club) | IMGUI_CLUB_SOURCE_DIR | imgui_club<br>imgui_memory_editor<br>imgui_multicontext_compositor<br>imgui_threaded_rendering | `Unofficial::imgui_club::imgui_club` |
| [imgui_markdown](https://github.com/enkisoftware/imgui_markdown) | IMGUI_MARKDOWN_SOURCE_DIR | imgui_markdown | `Unofficial::imgui_markdown::imgui_markdown` |
| [ImPlot](https://github.com/epezent/implot) | IMPLOT_SOURCE_DIR | implot | `Unofficial::ImPlot::implot` |
| [ImPlot3D](https://github.com/brenocq/implot3d) | IMPLOT3D_SOURCE_DIR | implot3d | `Unofficial::ImPlot3D::implot3d` |
| [ImGuiFileDialog](https://github.com/aiekick/ImGuiFileDialog) | IMGUIFILEDIALOG_SOURCE_DIR | imguifiledialog | `Unofficial::ImGuiFileDialog::imguifiledialog` |


