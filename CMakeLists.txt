################################################################################
# This source file is a part of LinearRegression.
# Legal  : All content (c) 2016 DigiPen  (USA) Corporation, all rights reserved.
# Author : Joshua T. Fisher
################################################################################
################################################################################
# set up the required version of cmake and policies related to versions.
################################################################################
cmake_minimum_required(VERSION 3.9.1 FATAL_ERROR) 
set_property(GLOBAL PROPERTY USE_FOLDERS ON)  

################################################################################
# Declaring our Project and it's used languages.
################################################################################
project(LinearRegression CXX C)

################################################################################
# set up some variables that we'll be able to access anywhere.
################################################################################
set(Root                   ${CMAKE_CURRENT_SOURCE_DIR})
set(Dependencies_Root      ${CMAKE_CURRENT_SOURCE_DIR}/Dependencies)
set(YTE_Root               ${Dependencies_Root}/YTE)
set(CMake_Include          ${YTE_Root}/cmake)
set(Source_Root            ${CMAKE_CURRENT_SOURCE_DIR}/Source)
set(LinearRegression_Root        ${Source_Root})
set(LinearRegression_Binary_Dir  ${CMAKE_CURRENT_SOURCE_DIR}/Output/Binary)
set(LinearRegression_Library_Dir ${CMAKE_CURRENT_SOURCE_DIR}/Output/Library)

################################################################################
# Path to modules for finding external libraries.
################################################################################
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${YTE_Root}/cmake/modules/")

################################################################################
# Packaging Configuration
################################################################################
File(GLOB_RECURSE BIN_FILES ${LinearRegression_Root}/Release/*.*)
set(DIRECTORIES
      ${Root}/Assets/Animations
      ${Root}/Assets/Archetypes
      ${Root}/Assets/Bin
      ${Root}/Assets/Levels
      ${Root}/Assets/Models
      ${Root}/Assets/Textures
      ${Root}/Assets/Wwise)
install(DIRECTORY ${DIRECTORIES} DESTINATION ".")

set(DIRECTORIES
      ${YTE_Root}/Assets/YTE/Archetypes
      ${YTE_Root}/Assets/YTE/Levels
      ${YTE_Root}/Assets/YTE/Models
      ${YTE_Root}/Assets/YTE/Textures
      ${YTE_Root}/Assets/YTE/Shaders)
install(DIRECTORY ${DIRECTORIES} DESTINATION ".")

set(DIRECTORIES ${Root}/Redists)
install(DIRECTORY ${DIRECTORIES} DESTINATION ".")

install(PROGRAMS "$<TARGET_FILE_DIR:LinearRegression>/VkHLF.dll" 
                   DESTINATION BIN)

set(CMAKE_INSTALL_SYSTEM_RUNTIME_LIBS_SKIP TRUE)
include(InstallRequiredSystemLibraries)
install(PROGRAMS ${CMAKE_INSTALL_SYSTEM_RUNTIME_LIBS}
                  DESTINATION BIN
                  COMPONENT applications)

set(CPACK_GENERATOR NSIS)
set(CPACK_PACKAGE_NAME "LinearRegression")
set(CPACK_PACKAGE_VENDOR "DigiPen")
#set(CPACK_PACKAGE_DESCRIPTION_FILE filehere)
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "LinearRegression, a CS399 Project by Jonathan Ackerman.")
set(CPACK_RESOURCE_FILE_LICENSE "${Source_Root}/DigiPen_EULA.txt")
#set(CPACK_RESOURCE_FILE_WELCOME filehere)
set(CPACK_PACKAGE_EXECUTABLES LinearRegression;LinearRegression)
set(CPACK_CREATE_DESKTOP_LINKS LinearRegression)
set(CPACK_NSIS_INSTALL_ROOT "$PROGRAMFILES64")
set(CPACK_NSIS_DEFINES "  !define MUI_STARTMENUPAGE_DEFAULTFOLDER \\\"DigiPen\\\\LinearRegression\\\"")
set(CPACK_PACKAGE_VERSION "0.0.1")
set(CPACK_PACKAGE_VERSION_MAJOR "0")
set(CPACK_PACKAGE_VERSION_MINOR "0")
set(CPACK_PACKAGE_VERSION_PATCH "1")
set(CPACK_PACKAGE_INSTALL_DIRECTORY "DigiPen\\\\LinearRegression")
set(CPACK_NSIS_DISPLAY_NAME "LinearRegression")
set(CPACK_NSIS_MODIFY_PATH ON)
set(CPACK_NSIS_MUI_UNIICON "${LinearRegression_Root}/IconUninstall.ico")
set(CPACK_NSIS_MUI_ICON "${LinearRegression_Root}/IconInstall.ico")
set(CPACK_NSIS_EXECUTABLES_DIRECTORY "bin/")
#set(CPACK_PACKAGE_ICON)
set(CPACK_NSIS_INSTALLED_ICON_NAME "bin/LinearRegression.exe")
set(CPACK_NSIS_MUI_FINISHPAGE_RUN "LinearRegression.exe")

################################################################################
## Includes
################################################################################
include(CPack)

################################################################################
## Setting compiler warnings based on platform.
################################################################################
if (${CMAKE_CXX_COMPILER_ID} STREQUAL MSVC)
  string(REGEX REPLACE "/W[0-9]" "" CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS})
  string(REGEX REPLACE "/W[0-9]" "" CMAKE_C_FLAGS ${CMAKE_C_FLAGS})
  set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} )
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /permissive-")
  set(CMAKE_EXE_LINKER_FLAGS_DEBUG "${CMAKE_EXE_LINKER_FLAGS_DEBUG} /debug:FASTLINK")
endif()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /DGLM_FORCE_CXX14 ")

################################################################################
# Adding additional configuration types
################################################################################
set(CMAKE_CONFIGURATION_TYPES "${CMAKE_CONFIGURATION_TYPES};Publish")
set(CMAKE_CXX_FLAGS_PUBLISH "${CMAKE_CXX_FLAGS_RELEASE}")
set(CMAKE_C_FLAGS_PUBLISH "${CMAKE_C_FLAGS_RELEASE}")
set(CMAKE_EXE_LINKER_FLAGS_PUBLISH "${CMAKE_EXE_LINKER_FLAGS_RELEASE}")
set(CMAKE_SHARED_LINKER_FLAGS_PUBLISH "${CMAKE_SHARED_LINKER_FLAGS_RELEASE}")
set(CMAKE_STATIC_LINKER_FLAGS_PUBLISH "${CMAKE_STATIC_LINKER_FLAGS_RELEASE}")
set(STATIC_LIBRARY_FLAGS_PUBLISH "${STATIC_LIBRARY_FLAGS_RELEASE}")


#if (${CMAKE_CXX_COMPILER_ID} STREQUAL MSVC)
#  set(CMAKE_CXX_FLAGS_PUBLISH "${CMAKE_CXX_FLAGS_PUBLISH} /GL")
#  set(CMAKE_EXE_LINKER_FLAGS_PUBLISH "${CMAKE_EXE_LINKER_FLAGS_PUBLISH} /LTCG")
#  set(STATIC_LIBRARY_FLAGS_PUBLISH "${STATIC_LIBRARY_FLAGS_PUBLISH} /LTCG")
#endif()

#if(CMAKE_BUILD_TYPE STREQUAL "Publish")
#      set(config_dir "./Config")
#else()
#      set(config_dir "../../../../../Assets/Bin/Config")
#endif()
#add_definitions(-DYTE_ENGINE_CONFIG="${config_dir}")

################################################################################
## Separate Projects into folders for IDEs.
################################################################################
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

################################################################################
## Call the CMakeLists.txt in our engine folder, and game folder.
################################################################################
add_subdirectory(Dependencies)
add_subdirectory(Source)
