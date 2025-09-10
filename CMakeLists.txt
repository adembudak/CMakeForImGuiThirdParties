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

get_property(is_generator_multi_config GLOBAL PROPERTY GENERATOR_IS_MULTI_CONFIG)
if(NOT is_generator_multi_config)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS Debug Release RelWithDebInfo MinSizeRel)
endif()

find_package(DearImGui CONFIG REQUIRED)

message(STATUS "DearImGui Version ${DearImGui_VERSION}")

option(imgui_club "" OFF)
option(imgui_markdown "" OFF)
option(implot "" OFF)
option(implot3d "" OFF)
option(imguifiledialog "" OFF)

cmake_dependent_option(implot "" OFF ${DearImGui_VERSION} VERSION_GREATER 18900 OFF)
cmake_dependent_option(implot3d "" OFF ${DearImGui_VERSION} VERSION_GREATER 19000 OFF)
cmake_dependent_option(imguifiledialog "" OFF ${DearImGui_VERSION} VERSION_GREATER 19200 OFF)

cmake_dependent_option(imgui_memory_editor "" OFF imgui_club OFF)
cmake_dependent_option(imgui_multicontext_compositor "" OFF "imgui_club AND (${DearImGui_VERSION} VERSION_GREATER 19080)" OFF)
cmake_dependent_option(imgui_threaded_rendering "" OFF imgui_club OFF)

option(BUILD_SHARED_LIBS "" OFF)

option(install "Install the library" ON)
option(uninstall "A custom target 'uninstall' is generated to undo latest install step" ON)

option(visualize_targets "A custom target 'graph' generated to visualize linkage graph" OFF)
mark_as_advanced(FORCE visualize_targets)

if(imgui_club)
  if(NOT IMGUI_CLUB_SOURCE_DIR)
    message(FATAL_ERROR "set IMGUI_CLUB_SOURCE_DIR to proceed:\ne.g. cmake -DIMGUI_CLUB_SOURCE_DIR=path.to.imgui_club ...")
  endif()
  cmake_path(SET _imgui_club_dir ${IMGUI_CLUB_SOURCE_DIR})

  if(imgui_memory_editor)
    add_library(imgui_memory_editor INTERFACE)
    target_sources(imgui_memory_editor
      INTERFACE FILE_SET imgui_memory_editor_set
      BASE_DIRS ${_imgui_club_dir}/imgui_memory_editor
      TYPE HEADERS
      FILES ${_imgui_club_dir}/imgui_memory_editor/imgui_memory_editor.h)
  endif()

  if(imgui_multicontext_compositor)
    add_library(imgui_multicontext_compositor)
    target_sources(imgui_multicontext_compositor
      PRIVATE ${_imgui_club_dir}/imgui_multicontext_compositor/imgui_multicontext_compositor.cpp
      PUBLIC FILE_SET imgui_multicontext_compositor_set
      BASE_DIRS ${_imgui_club_dir}/imgui_multicontext_compositor
      TYPE HEADERS
      FILES ${_imgui_club_dir}/imgui_multicontext_compositor/imgui_multicontext_compositor.h)
    target_link_libraries(imgui_multicontext_compositor PUBLIC ImGui::Core)

    list(APPEND imgui_thirdparty_list imgui_multicontext_compositor)
 endif()

  if(imgui_threaded_rendering)
    add_library(imgui_threaded_rendering INTERFACE)
    target_sources(imgui_threaded_rendering
      INTERFACE FILE_SET imgui_threaded_rendering_set
      BASE_DIRS ${_imgui_club_dir}/imgui_threaded_rendering
      TYPE HEADERS
      FILES ${_imgui_club_dir}/imgui_threaded_rendering/imgui_threaded_rendering.h)
  endif()
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

  add_library(implot)
  target_sources(implot
    PRIVATE ${_implot_dir}/implot.cpp ${_implot_dir}/implot_items.cpp ${_implot_dir}/implot_demo.cpp
    PUBLIC FILE_SET implot_set
    BASE_DIRS ${_implot_dir}
    TYPE HEADERS
    FILES ${_implot_dir}/implot.h ${_implot_dir}/implot_internal.h)
  target_link_libraries(implot PUBLIC ImGui::Core)

  list(APPEND imgui_thirdparty_list implot)
endif()

if(implot3d)
  if(NOT IMPLOT3D_SOURCE_DIR)
    message(FATAL_ERROR "set IMPLOT3D_SOURCE_DIR to proceed:\ne.g. cmake -DIMPLOT3D_SOURCE_DIR=path.to.implot3D ...")
  endif()
  cmake_path(SET _implot3d_dir ${IMPLOT3D_SOURCE_DIR})

  add_library(implot3d)
  target_sources(implot3d
    PRIVATE ${_implot3d_dir}/implot3d.cpp ${_implot3d_dir}/implot3d_items.cpp ${_implot3d_dir}/implot3d_meshes.cpp ${_implot3d_dir}/implot3d_demo.cpp
    PUBLIC FILE_SET implot3d_set
    BASE_DIRS ${_implot3d_dir}
    TYPE HEADERS
    FILES ${_implot3d_dir}/implot3d.h ${_implot3d_dir}/implot3d_internal.h)
  target_link_libraries(implot3d PUBLIC ImGui::Core)

  list(APPEND imgui_thirdparty_list implot3d)
endif()

if(imguifiledialog)
  if(NOT IMGUIFILEDIALOG_SOURCE_DIR)
    message(FATAL_ERROR "set IMGUIFILEDIALOG_SOURCE_DIR to proceed:\ne.g. cmake -DIMGUIFILEDIALOG_SOURCE_DIR=path.to.imguifiledialog...")
  endif()
  cmake_path(SET _imguifiledialog_dir ${IMGUIFILEDIALOG_SOURCE_DIR})

  find_path(Dirent_PATH dirent.h PATHS ${_imguifiledialog_dir} PATH_SUFFIXES dirent DOC "https://github.com/tronkko/dirent" NO_CACHE REQUIRED NO_DEFAULT_PATH NO_CMAKE_SYSTEM_PATH NO_CMAKE_FIND_ROOT_PATH)
  find_path(stb_PATH std_image.h stb_image_resize2.h PATHS ${_imguifiledialog_dir} PATH_SUFFIXES stb DOC "https://github.com/nothings/stb" NO_CACHE REQUIRED NO_DEFAULT_PATH NO_CMAKE_SYSTEM_PATH NO_CMAKE_FIND_ROOT_PATH)

  add_library(imguifiledialog)
  target_sources(imguifiledialog
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
  target_compile_features(imguifiledialog PRIVATE cxx_std_17)
  target_compile_definitions(imguifiledialog PRIVATE USE_STD_FILESYSTEM)
  target_link_libraries(imguifiledialog PUBLIC ImGui::Core)
endif()

if(install)
  set_property(GLOBAL PROPERTY INSTALL_PARALLEL ON)

  if(imgui_club)
    install(EXPORT imgui_club_export DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/imgui_club NAMESPACE Unofficial::imgui_club:: FILE imgui_club-config.cmake EXPORT_PACKAGE_DEPENDENCIES)

    if(imgui_threaded_rendering)
      install(TARGETS imgui_threaded_rendering EXPORT imgui_club_export FILE_SET imgui_threaded_rendering_set)
    endif()

    if(imgui_memory_editor)
      install(TARGETS imgui_memory_editor EXPORT imgui_club_export FILE_SET imgui_memory_editor_set)
    endif()

    if(imgui_multicontext_compositor)
      install(TARGETS imgui_multicontext_compositor EXPORT imgui_club_export FILE_SET imgui_multicontext_compositor_set)
    endif()

    install(FILES ${_imgui_club_dir}/LICENSE.txt DESTINATION ${CMAKE_INSTALL_DATADIR}/imgui_club)
  endif()

  if(imgui_markdown)
    install(EXPORT imgui_markdown_export DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/imgui_markdown NAMESPACE Unofficial::imgui_markdown:: FILE imgui_markdown-config.cmake EXPORT_PACKAGE_DEPENDENCIES)
    install(TARGETS imgui_markdown EXPORT imgui_markdown_export FILE_SET imgui_markdown_set)
    install(FILES ${_imgui_markdown_dir}/License.txt DESTINATION ${CMAKE_INSTALL_DATADIR}/imgui_markdown)
  endif()

  if(implot)
    install(EXPORT implot_export DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImPlot NAMESPACE Unofficial::ImPlot:: FILE ImPlotConfig.cmake EXPORT_PACKAGE_DEPENDENCIES)
    install(TARGETS implot EXPORT implot_export FILE_SET implot_set)
    install(FILES ${_implot_dir}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/ImPlot)
  endif()

  if(implot3d)
    install(EXPORT implot3d_export DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImPlot3D NAMESPACE Unofficial::ImPlot3D:: FILE ImPlot3DConfig.cmake EXPORT_PACKAGE_DEPENDENCIES)
    install(TARGETS implot3d EXPORT implot3d_export FILE_SET implot3d_set)
    install(FILES ${_implot3d_dir}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/ImPlot3D)
  endif()

  if(imguifiledialog)
    install(EXPORT imguifiledialog_export DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/ImGuiFileDialog NAMESPACE Unofficial::ImGuiFileDialog:: FILE ImGuiFileDialogConfig.cmake EXPORT_PACKAGE_DEPENDENCIES)
    install(TARGETS imguifiledialog EXPORT imguifiledialog_export FILE_SET imguifiledialog_set)
    install(FILES ${_imguifiledialog_dir}/LICENSE DESTINATION ${CMAKE_INSTALL_DATADIR}/ImGuiFileDialog)
    install(FILES ${_imguifiledialog_dir}/Documentation.md DESTINATION ${CMAKE_INSTALL_DATADIR}/ImGuiFileDialog/docs)
  endif()
endif()

if(uninstall)
  file(CONFIGURE
    OUTPUT cmake_uninstall.cmake
    CONTENT
[=[
if(NOT EXISTS @PROJECT_BINARY_DIR@/install_manifest.txt)
  message(FATAL_ERROR "Couldn't find @PROJECT_BINARY_DIR@/install_manifest.txt")
endif()

file(STRINGS @PROJECT_BINARY_DIR@/install_manifest.txt files_to_uninstall)

foreach(file IN LISTS files_to_uninstall)
  execute_process(COMMAND @CMAKE_COMMAND@ -E rm $ENV{DESTDIR}${file} COMMAND_ECHO STDOUT)
endforeach()
]=] @ONLY)

  add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${PROJECT_BINARY_DIR}/cmake_uninstall.cmake
    WORKING_DIRECTORY ${PROJECT_BINARY_DIR})
endif()

if(visualize_targets)
  if(NOT EXISTS ${PROJECT_BINARY_DIR}/CMakeGraphVizOptions.cmake)
    file(WRITE ${PROJECT_BINARY_DIR}/CMakeGraphVizOptions.cmake
[=[
set(GRAPHVIZ_GENERATE_PER_TARGET FALSE)
set(GRAPHVIZ_GENERATE_DEPENDERS FALSE)
]=])
  endif()

  find_program(DOT_COMMAND dot REQUIRED)

  add_custom_target(graph
    COMMAND ${CMAKE_COMMAND} --graphviz=targets.dot ${PROJECT_BINARY_DIR}
    COMMAND ${DOT_COMMAND} -Tpng targets.dot -o targets.png
    WORKING_DIRECTORY ${PROJECT_BINARY_DIR}
    VERBATIM)
endif()
