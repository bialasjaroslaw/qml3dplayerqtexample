cmake_minimum_required(VERSION 3.14)

project(qt3dplayer VERSION 0.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt5 COMPONENTS Core Multimedia Widgets 3DCore 3DRender 3DExtras 3DInput REQUIRED)

set(PROJECT_SOURCES
        main.cpp videoplayer.cpp videoplayer.h
)

if(ANDROID)
    add_library(qt3dplayer SHARED
        ${PROJECT_SOURCES}
    )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
else()
    add_executable(qt3dplayer
      ${PROJECT_SOURCES}
    )
endif()

target_compile_definitions(qt3dplayer
  PRIVATE $<$<OR:$<CONFIG:Debug>,$<CONFIG:RelWithDebInfo>>:QT_QML_DEBUG>)
target_link_libraries(qt3dplayer
  PRIVATE
  Qt${QT_VERSION_MAJOR}::Gui
  Qt${QT_VERSION_MAJOR}::Multimedia
  Qt${QT_VERSION_MAJOR}::Widgets
  Qt${QT_VERSION_MAJOR}::3DCore
  Qt${QT_VERSION_MAJOR}::3DExtras
  Qt${QT_VERSION_MAJOR}::3DInput
  Qt${QT_VERSION_MAJOR}::3DRender
)

set_target_properties(qt3dplayer PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_import_qml_plugins(qt3dplayer)
    qt_finalize_executable(qt3dplayer)
endif()
