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

project(flash-runtime-extensions VERSION 1.0
             DESCRIPTION "Required headers to be able to compile an Adobe Native Extension on Windows/MacOs/Linux"
             HOMEPAGE_URL "https://github.com/al-sabr/FlashRuntimeExtensions")

add_library(${PROJECT_NAME} INTERFACE)

target_link_libraries(${PROJECT_NAME} INTERFACE ${CMAKE_SOURCE_DIR}/${OBJ_LIB})

#target_include_directories(${PROJECT_NAME} INTERFACE include)


install(TARGETS ${PROJECT_NAME}
        EXPORT ${PROJECT_NAME}_Targets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})

message("ARCHIVE DESTINATION " ${CMAKE_INSTALL_LIBDIR})
message("LIBRARY DESTINATION " ${CMAKE_INSTALL_LIBDIR})
message("RUNTIME DESTINATION " ${CMAKE_INSTALL_BINDIR})

message(CMAKE_INSTALL_PREFIX " ${CMAKE_INSTALL_PREFIX}")
message(CMAKE_INSTALL_NAME_DIR " ${CMAKE_INSTALL_NAME_DIR}")

install(EXPORT ${PROJECT_NAME}_Targets
        FILE ${PROJECT_NAME}-targets.cmake
        NAMESPACE ${PROJECT_NAME}::
        DESTINATION ${CMAKE_INSTALL_PREFIX}/share/${PROJECT_NAME})

include(CMakePackageConfigHelpers)

configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/cmake/FlashRuntimeExtensionsConfig.cmake.in"
  "${PROJECT_BINARY_DIR}/${PROJECT_NAME}-config.cmake"
  INSTALL_DESTINATION
  ${CMAKE_INSTALL_PREFIX}/share/${PROJECT_NAME})

write_basic_package_version_file("${PROJECT_NAME}-config-version.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion)

install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-config-version.cmake
  DESTINATION ${CMAKE_INSTALL_PREFIX}/share/${PROJECT_NAME})

install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/include/FlashRuntimeExtensions.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/${PROJECT_NAME})
install(FILES ${OBJ_LIB} DESTINATION ${CMAKE_INSTALL_LIBDIR}/)