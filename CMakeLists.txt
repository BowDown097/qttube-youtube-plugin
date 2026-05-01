cmake_minimum_required(VERSION 3.21)

project(qttube-youtube-plugin LANGUAGES CXX)

add_library(qttube-youtube-plugin SHARED)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

include(deps/interface/QtTubePlugin.cmake)
create_qttube_plugin(qttube-youtube-plugin)

set_target_properties(qttube-youtube-plugin PROPERTIES
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON)

target_sources(qttube-youtube-plugin
    PRIVATE
        res/resources.qrc
        src/youtubeauth.cpp
        src/youtubeplayer.cpp
        src/youtubeplugin.cpp
        src/settings/channelfiltertable.cpp
        src/settings/channelfiltertable.ui
        src/settings/youtubesettings.cpp
        src/settings/youtubesettingsform.cpp
        src/settings/youtubesettingsform.ui
        src/settings/data-wizards/basicinfopage.cpp
        src/settings/data-wizards/datawizard.cpp
        src/settings/data-wizards/entityselecttablemodel.cpp
        src/settings/data-wizards/intropage.cpp
        src/settings/data-wizards/import/grayjayimportwizard.cpp
        src/settings/data-wizards/import/newpipeimportwizard.cpp
        src/settings/data-wizards/import/pipedimportwizard.cpp
        src/settings/data-wizards/import/takeoutimportwizard.cpp
        src/settings/data-wizards/import/shared/chooseentitiespage.cpp
        src/settings/data-wizards/import/shared/choosesubspage.cpp
        src/settings/data-wizards/import/shared/choosewatchhistorypage.cpp
        src/settings/data-wizards/import/shared/importfileselectpage.cpp
        src/settings/data-wizards/richtableview/richitemdelegate.cpp
        src/settings/data-wizards/richtableview/richtableview.cpp
        src/utils/conversion.cpp
        src/utils/replydata.cpp
        src/utils/stringutils.cpp
        src/utils/tubeutils.cpp
    PUBLIC
        FILE_SET HEADERS FILES
            src/youtubeauth.h
            src/youtubeplayer.h
            src/youtubeplugin.h
            src/settings/channelfiltertable.h
            src/settings/youtubesettings.h
            src/settings/youtubesettingsform.h
            src/settings/data-wizards/basicinfopage.h
            src/settings/data-wizards/conclusionpage.h
            src/settings/data-wizards/datawizard.h
            src/settings/data-wizards/entityselecttablemodel.h
            src/settings/data-wizards/intropage.h
            src/settings/data-wizards/import/grayjayimportwizard.h
            src/settings/data-wizards/import/newpipeimportwizard.h
            src/settings/data-wizards/import/pipedimportwizard.h
            src/settings/data-wizards/import/takeoutimportwizard.h
            src/settings/data-wizards/import/shared/chooseentitiespage.h
            src/settings/data-wizards/import/shared/choosesubspage.h
            src/settings/data-wizards/import/shared/choosewatchhistorypage.h
            src/settings/data-wizards/import/shared/importfileselectpage.h
            src/settings/data-wizards/richtableview/richitemdelegate.h
            src/settings/data-wizards/richtableview/richtableview.h
            src/utils/conversion.h
            src/utils/replydata.h
            src/utils/stringutils.h
            src/utils/tubeutils.h)

find_package(Qt6 REQUIRED COMPONENTS Sql)

find_package(ICU COMPONENTS i18n uc QUIET)
if(ICU_FOUND)
    target_compile_definitions(qttube-youtube-plugin PRIVATE QTTUBE_HAS_ICU)
    target_include_directories(qttube-youtube-plugin PRIVATE ${ICU_INCLUDE_DIRS})
    target_link_libraries(qttube-youtube-plugin PRIVATE ${ICU_LIBRARIES})
endif()

option(ENABLE_WEB_COMPONENTS "" ON)

add_subdirectory(deps/innertube-qt)
add_subdirectory(deps/interface)

target_include_directories(qttube-youtube-plugin PRIVATE src)
target_link_libraries(qttube-youtube-plugin PRIVATE innertube-qt Qt::Sql QtTube::PluginInterface)

