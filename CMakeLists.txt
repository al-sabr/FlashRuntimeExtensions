cmake_minimum_required(VERSION 3.12)

if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(IS_64BITS TRUE)
elseif(CMAKE_SIZEOF_VOID_P EQUAL 4)
    set(IS_64BITS FALSE)
endif()

if (WIN32)
  if(IS_64BITS)
    set(OBJ_LIB "lib/win64/FlashRuntimeExtensions.lib")
  else()
    set(OBJ_LIB "lib/win32/FlashRuntimeExtensions.lib")
  endif()
endif()

project(FlashRuntimeExtensions VERSION 0.1
             DESCRIPTION "Required headers to be able to compile an Adobe Native Extension on Windows/MacOs/Linux"
             HOMEPAGE_URL "https://github.com/al-sabr/FlashRuntimeExtensions")

add_library(${PROJECT_NAME} INTERFACE)

target_link_libraries(${PROJECT_NAME} INTERFACE ${CMAKE_SOURCE_DIR}/${OBJ_LIB})

target_include_directories(
  ${PROJECT_NAME} INTERFACE include)

message(BUILD_INTERFACE " $<BUILD_INTERFACE:include>")
message(INSTALL_INTERFACE " $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>")

install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}_Targets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

message(ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR})
message(LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
message(RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

include(CMakePackageConfigHelpers)
write_basic_package_version_file("${PROJECT_NAME}ConfigVersion.cmake"
                                 VERSION ${PROJECT_VERSION}
                                 COMPATIBILITY SameMajorVersion)

configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/cmake/${PROJECT_NAME}Config.cmake.in"
  "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION
  ${CMAKE_INSTALL_PREFIX}/${PROJECT_NAME}/cmake)

message(INSTALL_DESTINATION " ${CMAKE_INSTALL_PREFIX}/${PROJECT_NAME}/cmake")

install(EXPORT ${PROJECT_NAME}_Targets
        FILE ${PROJECT_NAME}Targets.cmake
        NAMESPACE ${PROJECT_NAME}::
        DESTINATION ${CMAKE_INSTALL_PREFIX}/${PROJECT_NAME}/cmake)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include DESTINATION ${CMAKE_INSTALL_LIBDIR}/include/${PROJECT_NAME})
install(FILES ${OBJ_LIB} DESTINATION ${CMAKE_INSTALL_LIBDIR}/lib)