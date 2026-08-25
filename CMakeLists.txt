cmake_minimum_required(VERSION 4.0.0)

project(CMakeForImGuiThirdParties
  VERSION 0.1.0.0
  DESCRIPTION "CMake build support for thirdparty ImGui software"
  HOMEPAGE_URL "https://github.com/adembudak/CMakeForImGuiThirdParties"
  LANGUAGES CXX)

set(CMAKE_EXPERIMENTAL_EXPORT_PACKAGE_DEPENDENCIES "1942b4fa-b2c5-4546-9385-83f254070067")

include(CMakeDependentOption)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

find_package(ImGui CONFIG REQUIRED)

message(STATUS "DearImGui Version ${ImGui_VERSION}")

option(imgui_club "https://github.com/ocornut/imgui_club" OFF)
option(imgui_markdown "https://github.com/enkisoftware/imgui_markdown" OFF)
cmake_dependent_option(implot "https://github.com/epezent/implot" OFF ${ImGui_VERSION} VERSION_GREATER 18900 OFF)
cmake_dependent_option(implot3d "https://github.com/brenocq/implot3d" OFF ${ImGui_VERSION} VERSION_GREATER 19000 OFF)
cmake_dependent_option(imguifiledialog "https://github.com/aiekick/ImGuiFileDialog" OFF ${ImGui_VERSION} VERSION_GREATER 19200 OFF)
option(imguizmo "https://github.com/CedricGuillemet/ImGuizmo" OFF)
option(imguicolortextedit "https://github.com/BalazsJako/ImGuiColorTextEdit" OFF)
option(imguifilebrowser "https://github.com/AirGuanZ/imgui-filebrowser" OFF)

set(IMGUI_CLUB_SOURCE_DIR "" CACHE PATH "")
set(IMGUI_MARKDOWN_SOURCE_DIR "" CACHE PATH "")
set(IMPLOT_SOURCE_DIR "" CACHE PATH "")
set(IMPLOT3D_SOURCE_DIR "" CACHE PATH "")
set(IMGUIFILEDIALOG_SOURCE_DIR "" CACHE PATH "")
set(IMGUIZMO_SOURCE_DIR "" CACHE PATH "")
set(IMGUICOLORTEXTEDIT_SOURCE_DIR "" CACHE PATH "")
set(IMGUI_FILEBROWSER_SOURCE_DIR "" CACHE PATH "")

if(imgui_club)
  if(NOT IMGUI_CLUB_SOURCE_DIR)
    message(FATAL_ERROR "set IMGUI_CLUB_SOURCE_DIR to proceed:\ne.g. cmake -DIMGUI_CLUB_SOURCE_DIR=path.to.imgui_club ...")
  endif()
  cmake_path(SET _imgui_club_dir ${IMGUI_CLUB_SOURCE_DIR})

  add_library(imgui_club)

  target_sources(imgui_club
    INTERFACE FILE_SET imgui_memory_editor_set
    BASE_DIRS ${_imgui_club_dir}/imgui_memory_editor
    TYPE HEADERS
    FILES ${_imgui_club_dir}/imgui_memory_editor/imgui_memory_editor.h
    PRIVATE ${_imgui_club_dir}/imgui_multicontext_compositor/imgui_multicontext_compositor.cpp # Dear ImGui version > 19080
    PUBLIC FILE_SET imgui_multicontext_compositor_set
    BASE_DIRS ${_imgui_club_dir}/imgui_multicontext_compositor
    TYPE HEADERS
    FILES ${_imgui_club_dir}/imgui_multicontext_compositor/imgui_multicontext_compositor.h
    INTERFACE FILE_SET imgui_threaded_rendering_set
    BASE_DIRS ${_imgui_club_dir}/imgui_threaded_rendering
    TYPE HEADERS
    FILES ${_imgui_club_dir}/imgui_threaded_rendering/imgui_threaded_rendering.h)

  target_link_libraries(imgui_club PUBLIC ImGui::Core)
endif()

if(imgui_markdown)
  if(NOT IMGUI_MARKDOWN_SOURCE_DIR)
    message(FATAL_ERROR "set IMGUI_MARKDOWN_SOURCE_DIR to proceed:\ne.g. cmake -DIMGUI_MARKDOWN_SOURCE_DIR=path.to.imgui_markdown...")
  endif()
  cmake_path(SET _imgui_markdown_dir ${IMGUI_MARKDOWN_SOURCE_DIR})

  add_library(imgui_markdown INTERFACE)
  target_sources(imgui_markdown
    INTERFACE FILE_SET imgui_markdown_set
    BASE_DIRS ${_imgui_markdown_dir}
    TYPE HEADERS
    FILES ${_imgui_markdown_dir}/imgui_markdown.h)
endif()

if(implot)
  if(NOT IMPLOT_SOURCE_DIR)
    message(FATAL_ERROR "set IMPLOT_SOURCE_DIR to proceed:\ne.g. cmake -DIMPLOT_SOURCE_DIR=path.to.implot ...")
  endif()
  cmake_path(SET _implot_dir ${IMPLOT_SOURCE_DIR})

  add_library(ImPlot)
  target_sources(ImPlot
    PRIVATE ${_implot_dir}/implot.cpp ${_implot_dir}/implot_items.cpp ${_implot_dir}/implot_demo.cpp
    PUBLIC FILE_SET implot_set
    BASE_DIRS ${_implot_dir}
    TYPE HEADERS
    FILES ${_implot_dir}/implot.h ${_implot_dir}/implot_internal.h)
  target_link_libraries(ImPlot PUBLIC ImGui::Core)
endif()

if(implot3d)
  if(NOT IMPLOT3D_SOURCE_DIR)
    message(FATAL_ERROR "set IMPLOT3D_SOURCE_DIR to proceed:\ne.g. cmake -DIMPLOT3D_SOURCE_DIR=path.to.implot3D ...")
  endif()
  cmake_path(SET _implot3d_dir ${IMPLOT3D_SOURCE_DIR})

  add_library(ImPlot3D)
  target_sources(ImPlot3D
    PRIVATE ${_implot3d_dir}/implot3d.cpp ${_implot3d_dir}/implot3d_items.cpp ${_implot3d_dir}/implot3d_meshes.cpp ${_implot3d_dir}/implot3d_demo.cpp
    PUBLIC FILE_SET implot3d_set
    BASE_DIRS ${_implot3d_dir}
    TYPE HEADERS
    FILES ${_implot3d_dir}/implot3d.h ${_implot3d_dir}/implot3d_internal.h)
  target_link_libraries(ImPlot3D PUBLIC ImGui::Core)
endif()

if(imguifiledialog)
  if(NOT IMGUIFILEDIALOG_SOURCE_DIR)
    message(FATAL_ERROR "set IMGUIFILEDIALOG_SOURCE_DIR to proceed:\ne.g. cmake -DIMGUIFILEDIALOG_SOURCE_DIR=path.to.imguifiledialog...")
  endif()
  cmake_path(SET _imguifiledialog_dir ${IMGUIFILEDIALOG_SOURCE_DIR})

  find_path(Dirent_PATH dirent.h PATHS ${_imguifiledialog_dir} PATH_SUFFIXES dirent DOC "https://github.com/tronkko/dirent" NO_CACHE REQUIRED NO_DEFAULT_PATH NO_CMAKE_SYSTEM_PATH NO_CMAKE_FIND_ROOT_PATH)
  find_path(stb_PATH std_image.h stb_image_resize2.h PATHS ${_imguifiledialog_dir} PATH_SUFFIXES stb DOC "https://github.com/nothings/stb" NO_CACHE REQUIRED NO_DEFAULT_PATH NO_CMAKE_SYSTEM_PATH NO_CMAKE_FIND_ROOT_PATH)

  add_library(ImGuiFileDialog)
  target_sources(ImGuiFileDialog
    PRIVATE ${_imguifiledialog_dir}/ImGuiFileDialog.cpp
    PUBLIC FILE_SET imguifiledialog_set
    BASE_DIRS ${_imguifiledialog_dir}
    TYPE HEADERS
    FILES ${_imguifiledialog_dir}/ImGuiFileDialog.h ${_imguifiledialog_dir}/ImGuiFileDialogConfig.h
    PRIVATE FILE_SET HEADERS
    BASE_DIRS ${stb_PATH}
    FILES ${stb_PATH}/stb_image.h ${stb_PATH}/stb_image_resize2.h
    PRIVATE FILE_SET HEADERS
    BASE_DIRS $<$<PLATFORM_ID:Windows>:${Dirent_PATH}>
    FILES $<$<PLATFORM_ID:Windows>:${Dirent_PATH}/dirent.h>)
  target_compile_features(ImGuiFileDialog PRIVATE cxx_std_17)
  target_compile_definitions(ImGuiFileDialog PRIVATE USE_STD_FILESYSTEM)
  target_link_libraries(ImGuiFileDialog PUBLIC ImGui::Core)
endif()

if(imguizmo)
  if(NOT IMGUIZMO_SOURCE_DIR)
    message(FATAL_ERROR "set IMGUIZMO_SOURCE_DIR to proceed:\ne.g. cmake -DIMGUIZMO_SOURCE_DIR=path.to.imguizmo...")
  endif()
  cmake_path(SET _imguizmo_dir ${IMGUIZMO_SOURCE_DIR})

  add_library(ImGuizmo)
  target_sources(ImGuizmo
    PRIVATE
      ${_imguizmo_dir}/src/GraphEditor.cpp
      ${_imguizmo_dir}/src/ImCurveEdit.cpp
      ${_imguizmo_dir}/src/ImGradient.cpp
      ${_imguizmo_dir}/src/ImGuizmo.cpp
      ${_imguizmo_dir}/src/ImSequencer.cpp
      ${_imguizmo_dir}/src/ImVectorEditor.cpp
    PUBLIC FILE_SET imguizmo_set
    BASE_DIRS ${_imguizmo_dir}/src
    TYPE HEADERS
    FILES
      ${_imguizmo_dir}/src/GraphEditor.h
      ${_imguizmo_dir}/src/ImCurveEdit.h
      ${_imguizmo_dir}/src/ImGradient.h
      ${_imguizmo_dir}/src/ImGuizmo.h
      ${_imguizmo_dir}/src/ImLightRig.h
      ${_imguizmo_dir}/src/ImSequencer.h
      ${_imguizmo_dir}/src/ImVectorEditor.h
      ${_imguizmo_dir}/src/ImZoomSlider.h)
  target_compile_features(ImGuizmo PRIVATE cxx_std_17)
  target_link_libraries(ImGuizmo PUBLIC ImGui::Core)
endif()

if(imguicolortextedit)
  if(NOT IMGUICOLORTEXTEDIT_SOURCE_DIR)
    message(FATAL_ERROR "set IMGUICOLORTEXTEDIT_SOURCE_DIR to proceed:\ne.g. cmake -DIMGUICOLORTEXTEDIT_SOURCE_DIR=path.to.imguicolortextedit...")
  endif()
  cmake_path(SET _imguicolortextedit_dir ${IMGUICOLORTEXTEDIT_SOURCE_DIR})

  add_library(ImGuiColorTextEdit)
  target_sources(ImGuiColorTextEdit
    PRIVATE ${_imguicolortextedit_dir}/TextEditor.cpp
    PUBLIC FILE_SET imguicolortextedit_set
    BASE_DIRS ${_imguicolortextedit_dir}
    TYPE HEADERS
    FILES ${_imguicolortextedit_dir}/TextEditor.h)
  target_link_libraries(ImGuiColorTextEdit PUBLIC ImGui::Core)
endif()

if(imguifilebrowser)
  if(NOT IMGUI_FILEBROWSER_SOURCE_DIR)
    message(FATAL_ERROR "set IMGUI_FILEBROWSER_SOURCE_DIR to proceed:\ne.g. cmake -DIMGUI_FILEBROWSER_SOURCE_DIR=path.to.imgui-filebrowser...")
  endif()

  add_library(imfilebrowser INTERFACE)
  target_sources(imfilebrowser
    INTERFACE FILE_SET HEADERS
    BASE_DIRS ${IMGUI_FILEBROWSER_SOURCE_DIR}
    FILES ${IMGUI_FILEBROWSER_SOURCE_DIR}/imfilebrowser.h)
  target_compile_features(imfilebrowser INTERFACE cxx_std_17)
  target_link_libraries(imfilebrowser INTERFACE ImGui::Core)
endif()

if(imgui_club)
  install(EXPORT imgui_club-targets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/imgui_club NAMESPACE Unofficial::imgui_club:: FILE imgui_club-config.cmake EXPORT_PACKAGE_DEPENDENCIES)
  install(TARGETS imgui_club EXPORT imgui_club-targets FILE_SET imgui_threaded_rendering_set FILE_SET imgui_memory_editor_set FILE_SET imgui_multicontext_compositor_set)
  install(FILES ${_imgui_club_dir}/LICENSE.txt DESTINATION ${CMAKE_INSTALL_DATADIR}/imgui_club)
endif()

if(imgui_markdown)
  install(EXPORT imgui_markdown-targets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/imgui_markdown NAMESPACE Unofficial::imgui_markdown:: FILE imgui_markdown-config.cmake EXPORT_PACKAGE_DEPENDENCIES)
  install(TARGETS imgui_markdown EXPORT imgui_markdown-targets FILE_SET imgui_markdown_set)
  install(FILES ${_imgui_markdown_dir}/License.txt DESTINATION ${CMAKE_INSTALL_DATADIR}/imgui_markdown)
endif()

if(implot)
  install(EXPORT implot-targets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImPlot NAMESPACE Unofficial::ImPlot:: FILE ImPlotConfig.cmake EXPORT_PACKAGE_DEPENDENCIES)
  install(TARGETS ImPlot EXPORT implot-targets FILE_SET implot_set)
  install(FILES ${_implot_dir}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/ImPlot)
endif()

if(implot3d)
  install(EXPORT implot3d-targets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImPlot3D NAMESPACE Unofficial::ImPlot3D:: FILE ImPlot3DConfig.cmake EXPORT_PACKAGE_DEPENDENCIES)
  install(TARGETS ImPlot3D EXPORT implot3d-targets FILE_SET implot3d_set)
  install(FILES ${_implot3d_dir}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/ImPlot3D)
endif()

if(imguifiledialog)
  install(EXPORT imguifiledialog-targets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImGuiFileDialog NAMESPACE Unofficial::ImGuiFileDialog:: FILE ImGuiFileDialogConfig.cmake EXPORT_PACKAGE_DEPENDENCIES)
  install(TARGETS ImGuiFileDialog EXPORT imguifiledialog-targets FILE_SET imguifiledialog_set FILE_SET HEADERS)
  install(FILES ${_imguifiledialog_dir}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/ImGuiFileDialog)
  install(FILES ${_imguifiledialog_dir}/Documentation.md DESTINATION ${CMAKE_INSTALL_DATADIR}/ImGuiFileDialog/docs)
endif()

if(imguizmo)
  install(EXPORT imguizmo-targets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImGuizmo NAMESPACE Unofficial::ImGuizmo:: FILE ImGuizmoConfig.cmake EXPORT_PACKAGE_DEPENDENCIES)
  install(TARGETS ImGuizmo EXPORT imguizmo-targets FILE_SET imguizmo_set)
  install(FILES ${_imguizmo_dir}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/ImGuizmo)
endif()

if(imguicolortextedit)
  install(EXPORT imguicolortextedit-targets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImGuiColorTextEdit NAMESPACE Unofficial::ImGuiColorTextEdit:: FILE ImGuiColorTextEditConfig.cmake EXPORT_PACKAGE_DEPENDENCIES)
  install(TARGETS ImGuiColorTextEdit EXPORT imguicolortextedit-targets FILE_SET imguicolortextedit_set)
  install(FILES ${_imguicolortextedit_dir}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/ImGuiColorTextEdit)
endif()

if(imguifilebrowser)
  install(EXPORT imfilebrowser-targets DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/imfilebrowser NAMESPACE Unofficial::imfilebrowser:: FILE imfilebrowser-config.cmake EXPORT_PACKAGE_DEPENDENCIES)
  install(TARGETS imfilebrowser EXPORT imfilebrowser-targets FILE_SET HEADERS)
  install(FILES ${IMGUI_FILEBROWSER_SOURCE_DIR}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/imfilebrowser)
endif()
