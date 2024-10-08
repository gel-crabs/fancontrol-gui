set(LIB_SRCS src/hwmon.cpp
             src/sensor.cpp
             src/temp.cpp
             src/fan.cpp
             src/pwmfan.cpp
             src/loader.cpp
             src/guibase.cpp
             src/config.cpp
             src/pwmfanmodel.cpp
             src/tempmodel.cpp
             src/fancontrolqmlextension.cpp)

set(QML_FILES qml/qmldir
              qml/ErrorDialog.qml
              qml/FanControls.qml
              qml/FanHeader.qml
              qml/FanItem.qml
              qml/PwmPoint.qml
              qml/ProfilesDialog.qml
              qml/SettingsForm.qml
              qml/StatusPoint.qml
              qml/colors.js
              qml/math.js)

set(LIB_PRIVATE_LIBRARIES Qt6::Qml
                          KF6Auth
                          KF6ConfigCore
                          KF6I18n)

set(LIB_PUBLIC_LIBRARIES Qt6::Core)

if(NOT NO_SYSTEMD)

    set(LIB_SRCS ${LIB_SRCS}
                 src/systemdcommunicator.cpp)

    set(LIB_PRIVATE_LIBRARIES ${LIB_PRIVATE_LIBRARIES}
                              Qt6::DBus)

    find_package(Qt6DBus REQUIRED)
    include_directories(${Qt6DBus_INCLUDE_DIRS})

endif(NOT NO_SYSTEMD)

find_package(Qt6Qml REQUIRED)
find_package(KF6 COMPONENTS Auth Config REQUIRED)

include_directories(${Qt6Qml_INCLUDE_DIRS})

add_library(fancontrol_qml_plugin SHARED ${LIB_SRCS})
target_link_libraries(fancontrol_qml_plugin PRIVATE ${LIB_PRIVATE_LIBRARIES} PUBLIC ${LIB_PUBLIC_LIBRARIES})

install(TARGETS fancontrol_qml_plugin DESTINATION "${KDE_INSTALL_QMLDIR}/Fancontrol/Qml/")
install(FILES ${QML_FILES} DESTINATION "${KDE_INSTALL_QMLDIR}/Fancontrol/Qml/")


#install qmltypes file
include(ECMGenerateQmlTypes)
ecm_generate_qmltypes(Fancontrol.Qml 1.0 DESTINATION "${KDE_INSTALL_QMLDIR}/Fancontrol/Qml/")


#tests
if(BUILD_TESTING)
    add_subdirectory(tests)
endif(BUILD_TESTING)
