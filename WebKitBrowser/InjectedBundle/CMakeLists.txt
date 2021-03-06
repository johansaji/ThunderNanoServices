set(PLUGIN_NAME InjectedBundle)
set(MODULE_NAME WPE${PLUGIN_NAME})

find_package(${NAMESPACE}Plugins REQUIRED)

option(PLUGIN_AMAZON_HAWAII "Enable the Amazon Player interface (Hawaii) in javascript." OFF)

add_library(${MODULE_NAME} SHARED 
    main.cpp
    WhiteListedOriginDomainsList.cpp
    Utils.cpp
    JavaScriptFunction.cpp
    NotifyWPEFramework.cpp
    Milestone.cpp
    ClassDefinition.cpp
    Tags.cpp
)

if(PLUGIN_AMAZON_HAWAII)
    target_sources(${MODULE_NAME} PRIVATE HawaiiMessage.cpp)
endif()

set_target_properties(${MODULE_NAME} PROPERTIES
        CXX_STANDARD 11
        CXX_STANDARD_REQUIRED YES)

target_link_libraries(${MODULE_NAME}
    PRIVATE 
        ${NAMESPACE}Plugins::${NAMESPACE}Plugins
        ${GLIB_LIBRARIES}
        ${WPE_WEBKIT_LIBRARIES})

target_include_directories(${MODULE_NAME} 
    PRIVATE 
        ${WPE_WEBKIT_INCLUDE_DIRS}
        ${WPE_WEBKIT_INCLUDE_DIRS}/WPE
        ${GLIB_INCLUDE_DIRS})

install(TARGETS ${MODULE_NAME} 
    DESTINATION ${CMAKE_INSTALL_PREFIX}/share/${NAMESPACE}/WebKitBrowser
    COMPONENT ${MODULE_NAME})

if(PLUGIN_WEBKITBROWSER_AMAZON)
    install(TARGETS ${MODULE_NAME} 
        DESTINATION ${CMAKE_INSTALL_PREFIX}/share/${NAMESPACE}/Amazon
        COMPONENT ${MODULE_NAME})
endif()
