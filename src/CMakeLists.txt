set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

find_package(Qt6 COMPONENTS Quick REQUIRED)

set(SOURCES
    graphicaleffects.qrc
    graphicaleffects_global.h
    graphicaleffects.h
    graphicaleffects.cpp
    qgfxsourceproxyv2.h
    qgfxsourceproxyv2.cpp
)

if(WIN32 AND BUILD_SHARED_LIBS)
    enable_language(RC)
    list(APPEND SOURCES graphicaleffects.rc)
endif()

add_library(${PROJECT_NAME} ${SOURCES})
add_library(wangwenx190::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

target_include_directories(${PROJECT_NAME} PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>"
)

if(NOT BUILD_SHARED_LIBS)
    target_compile_definitions(${PROJECT_NAME} PUBLIC
        GRAPHICALEFFECTS_STATIC
    )
endif()

target_link_libraries(${PROJECT_NAME} PRIVATE
    Qt6::Quick
    Qt6::QuickPrivate
)

target_compile_definitions(${PROJECT_NAME} PRIVATE
    QT_NO_CAST_FROM_ASCII
    QT_NO_CAST_TO_ASCII
    QT_NO_KEYWORDS
    QT_DEPRECATED_WARNINGS
    QT_DISABLE_DEPRECATED_BEFORE=0x060100
    GRAPHICALEFFECTS_BUILD_LIBRARY
)

if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE /utf-8)
    if(NOT (CMAKE_BUILD_TYPE STREQUAL "Debug"))
        target_compile_options(${PROJECT_NAME} PRIVATE /guard:cf)
        target_link_options(${PROJECT_NAME} PRIVATE /GUARD:CF)
    endif()
endif()
