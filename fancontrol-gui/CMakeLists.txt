set(Fancontrol_GUI_SRCS src/main.cpp
                        src/systemtrayicon.cpp)

set(LIBRARIES Qt6::Gui
              Qt6::Widgets
              KF6::CoreAddons
              KF6::DBusAddons
              KF6Declarative
              KF6::I18n
              KF6::ConfigGui
              KF6::ConfigCore
              KF6::Notifications)

find_package(Qt6 COMPONENTS Gui Widgets REQUIRED)
find_package(KF6 COMPONENTS CoreAddons DBusAddons Package Declarative Config Notifications REQUIRED)

include_directories(${Qt6Widgets_INCLUDE_DIRS})
add_definitions(${Qt6Widgets_DEFINITIONS})

add_executable(fancontrol_gui ${Fancontrol_GUI_SRCS})
target_link_libraries(fancontrol_gui ${LIBRARIES})

install(TARGETS fancontrol_gui ${KDE_INSTALL_TARGETS_DEFAULT_ARGS})
install(PROGRAMS org.kde.fancontrol.gui.desktop DESTINATION ${KDE_INSTALL_APPDIR})
install(FILES org.kde.fancontrol.gui.appdata.xml DESTINATION ${KDE_INSTALL_METAINFODIR})

kpackage_install_package(package org.kde.fancontrol.gui genericqml)
