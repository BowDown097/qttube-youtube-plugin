cmake_minimum_required(VERSION 3.21)

project(qttube-youtube-plugin
    VERSION 1.0
    DESCRIPTION "A plugin providing YouTube support in QtTube."
    HOMEPAGE_URL "https://github.com/BowDown097/qttube-youtube-plugin"
    LANGUAGES CXX)

add_library(qttube-youtube-plugin SHARED)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

include(interface/QtTubePlugin.cmake)
create_qttube_plugin(qttube-youtube-plugin)

set_target_properties(qttube-youtube-plugin PROPERTIES
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED ON)

target_sources(qttube-youtube-plugin
    PRIVATE
        youtubeauth.cpp
        youtubeplayer.cpp
        youtubeplugin.cpp
        res/resources.qrc
        settings/channelfiltertable.cpp
        settings/channelfiltertable.ui
        settings/youtubesettings.cpp
        settings/youtubesettingsform.cpp
        settings/youtubesettingsform.ui
        settings/data-wizards/basicinfopage.cpp
        settings/data-wizards/datawizard.cpp
        settings/data-wizards/entityselecttablemodel.cpp
        settings/data-wizards/intropage.cpp
        settings/data-wizards/import/grayjayimportwizard.cpp
        settings/data-wizards/import/newpipeimportwizard.cpp
        settings/data-wizards/import/pipedimportwizard.cpp
        settings/data-wizards/import/takeoutimportwizard.cpp
        settings/data-wizards/import/shared/chooseentitiespage.cpp
        settings/data-wizards/import/shared/choosesubspage.cpp
        settings/data-wizards/import/shared/choosewatchhistorypage.cpp
        settings/data-wizards/import/shared/importfileselectpage.cpp
        settings/data-wizards/richtableview/richitemdelegate.cpp
        settings/data-wizards/richtableview/richtableview.cpp
        utils/conversion.cpp
        utils/replydata.cpp
        utils/stringutils.cpp
        utils/tubeutils.cpp
    PUBLIC
        FILE_SET HEADERS FILES
            youtubeauth.h
            youtubeplayer.h
            youtubeplugin.h
            settings/channelfiltertable.h
            settings/youtubesettings.h
            settings/youtubesettingsform.h
            settings/data-wizards/basicinfopage.h
            settings/data-wizards/conclusionpage.h
            settings/data-wizards/datawizard.h
            settings/data-wizards/entityselecttablemodel.h
            settings/data-wizards/intropage.h
            settings/data-wizards/import/grayjayimportwizard.h
            settings/data-wizards/import/newpipeimportwizard.h
            settings/data-wizards/import/pipedimportwizard.h
            settings/data-wizards/import/takeoutimportwizard.h
            settings/data-wizards/import/shared/chooseentitiespage.h
            settings/data-wizards/import/shared/choosesubspage.h
            settings/data-wizards/import/shared/choosewatchhistorypage.h
            settings/data-wizards/import/shared/importfileselectpage.h
            settings/data-wizards/richtableview/richitemdelegate.h
            settings/data-wizards/richtableview/richtableview.h
            utils/conversion.h
            utils/replydata.h
            utils/stringutils.h
            utils/tubeutils.h)

find_package(Qt6 REQUIRED COMPONENTS Sql)

find_package(ICU COMPONENTS i18n uc QUIET)
if(ICU_FOUND)
    target_compile_definitions(qttube-youtube-plugin PRIVATE QTTUBE_HAS_ICU)
    target_include_directories(qttube-youtube-plugin PRIVATE ${ICU_INCLUDE_DIRS})
    target_link_libraries(qttube-youtube-plugin PRIVATE ${ICU_LIBRARIES})
endif()

option(ENABLE_WEB_COMPONENTS "" ON)

add_subdirectory(innertube-qt)
add_subdirectory(interface)

target_link_libraries(qttube-youtube-plugin PRIVATE innertube-qt Qt::Sql QtTube::PluginInterface)

