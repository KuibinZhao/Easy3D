cmake_minimum_required(VERSION 3.1)

get_filename_component(PROJECT_NAME ${CMAKE_CURRENT_SOURCE_DIR} NAME)
project(${PROJECT_NAME})

include(../../cmake/UseQt5.cmake)
if (NOT QT5_FOUND)
    message(WARNING "${PROJECT_NAME} requires Qt but Qt was not found. You can set 'Qt5_DIR' to the "
            "directory containing 'Qt5Config.cmake' or 'qt5-config.cmake'. "
            "Optionally, you can set the Qt5 root directory 'QT5_ROOT_PATH' to the directory "
            "containing the 'bin' folder.")
    return()
endif ()


# some algorithms may use OpenMP
include(../../cmake/UseOpenMP.cmake)


set(${PROJECT_NAME}_HEADERS
        main_window.h
        paint_canvas.h

        dialogs/dialog_gaussian_noise.h
        dialogs/dialog_poisson_reconstruction.h
        dialogs/dialog_ransac_primitive_extraction.h
        dialogs/dialog_surface_mesh_sampling.h
        dialogs/dialog_snapshot.h
        dialogs/dialog_point_cloud_simplification.h

        widgets/widget_checker_sphere.h
        widgets/widget_drawable.h
        widgets/widget_drawable_points.h
        widgets/widget_drawable_lines.h
        widgets/widget_drawable_triangles.h
        widgets/widget_global_setting.h

        tools/canvas.h
        tools/tool.h
        tools/tool_manager.h
        tools/tool_selection_point_cloud.h
        tools/tool_selection_surface_mesh.h
        )

set(${PROJECT_NAME}_SOURCES
        main.cpp
        main_window.cpp
        paint_canvas.cpp
        paint_canvas_snapshot.cpp

        dialogs/dialog_gaussian_noise.cpp
        dialogs/dialog_poisson_reconstruction.cpp
        dialogs/dialog_ransac_primitive_extraction.cpp
        dialogs/dialog_surface_mesh_sampling.cpp
        dialogs/dialog_snapshot.cpp
        dialogs/dialog_point_cloud_simplification.cpp

        widgets/widget_checker_sphere.cpp
        widgets/widget_drawable.cpp
        widgets/widget_drawable_points.cpp
        widgets/widget_drawable_lines.cpp
        widgets/widget_drawable_triangles.cpp
        widgets/widget_global_setting.cpp
        widgets/widget_model_list.cpp

        tools/tool.cpp
        tools/tool_manager.cpp
        tools/tool_selection_point_cloud.cpp
        tools/tool_selection_surface_mesh.cpp
        )

set(${PROJECT_NAME}_FORMS
        main_window.ui

        dialogs/dialog_gaussian_noise.ui
        dialogs/dialog_poisson_reconstruction.ui
        dialogs/dialog_ransac_primitive_extraction.ui
        dialogs/dialog_surface_mesh_sampling.ui
        dialogs/dialog_snapshot.ui
        dialogs/dialog_point_cloud_simplification.ui

        widgets/widget_drawable_points.ui
        widgets/widget_drawable_lines.ui
        widgets/widget_drawable_triangles.ui
        widgets/widget_global_setting.ui
        widgets/widget_model_list.h
        )

#set(${PROJECT_NAME}_RESOURCES
#    Resources/Mapple.qrc
#    )

if (MSVC)
    add_executable(${PROJECT_NAME} ${${PROJECT_NAME}_HEADERS} ${${PROJECT_NAME}_SOURCES} ${${PROJECT_NAME}_FORMS} ${${PROJECT_NAME}_RESOURCES})
    # if you want to get rid of the (system) console, use
    #	add_executable( ${PROJECT_NAME} WIN32 ${${PROJECT_NAME}_HEADERS} ${${PROJECT_NAME}_SOURCES} ${${PROJECT_NAME}_FORMS} ${${PROJECT_NAME}_RESOURCES} )
    #       target_link_libraries( ${PROJECT_NAME} Qt5::WinMain )
elseif (APPLE)
    add_executable(${PROJECT_NAME} MACOSX_BUNDLE ${${PROJECT_NAME}_HEADERS} ${${PROJECT_NAME}_SOURCES} ${${PROJECT_NAME}_FORMS} ${${PROJECT_NAME}_RESOURCES})
else ()
    add_executable(${PROJECT_NAME} ${${PROJECT_NAME}_HEADERS} ${${PROJECT_NAME}_SOURCES} ${${PROJECT_NAME}_FORMS} ${${PROJECT_NAME}_RESOURCES})
endif ()

set_target_properties(${PROJECT_NAME} PROPERTIES FOLDER "applications")

target_include_directories(${PROJECT_NAME} PRIVATE ${EASY3D_INCLUDE_DIR})
target_include_directories(${PROJECT_NAME} PRIVATE ${EASY3D_glog_INCLUDE_DIR})

# The following define makes your compiler emit warnings if you use
# any feature of Qt which has been marked as deprecated (the exact warnings
# depend on your compiler). Please consult the documentation of the
# deprecated API in order to know how to port your code away from it.
target_compile_definitions(${PROJECT_NAME} PRIVATE QT_DEPRECATED_WARNINGS GLEW_NO_GLU)

if (MSVC)
    target_compile_definitions(${PROJECT_NAME} PRIVATE _CRT_SECURE_NO_DEPRECATE)
endif ()

target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Gui Qt5::Widgets Qt5::OpenGL)
target_link_libraries(${PROJECT_NAME} viewer algo gui)

if (EASY3D_HAS_CGAL)
    target_link_libraries(${PROJECT_NAME} algo_ext)
endif ()


## Note: On macOS, high-DPI support is enabled by settings in the Info.plist file. Make sure they are present.
## For more information, see: http://doc.qt.io/qt-5/highdpi.html
## Enable high DPI support by using my Info.plist file in which I added NSHighResolutionCapable to be true.
if (APPLE)
    set_target_properties(${PROJECT_NAME} PROPERTIES MACOSX_BUNDLE_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/Mac/Mapple.plist)
endif ()

## Resources
# Copy resources dirs into our shadow build directory
#file( COPY ${CMAKE_CURRENT_SOURCE_DIR}/Resources DESTINATION ${CMAKE_CURRENT_BINARY_DIR} )
file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/Resources DESTINATION ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})
