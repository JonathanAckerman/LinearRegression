################################################################################
## This source file is a part of LinearRegression.
## Legal  : All content (c) 2016 DigiPen  (USA) Corporation, all rights reserved.
## Author : Joshua T. Fisher
################################################################################
add_executable(LinearRegression main.cpp
                          WinMain.cpp
                          LinearRegression.rc)

target_include_directories(LinearRegression PRIVATE 
                                ${Source_Root}
                                ${Dependencies_Root}
                                ${YTE_Root}/Source
                                ${YTE_Root}/Dependencies/assimp/contrib/rapidjson/include/
                                ${YTE_Root}/Dependencies/)

set_target_properties(LinearRegression
                      PROPERTIES
                      ARCHIVE_OUTPUT_DIRECTORY ${LinearRegression_Library_Dir}
                      LIBRARY_OUTPUT_DIRECTORY ${LinearRegression_Library_Dir}
                      RUNTIME_OUTPUT_DIRECTORY ${LinearRegression_Binary_Dir})

YTE_Source_Group(LinearRegression_Root LinearRegression)

target_link_libraries(LinearRegression PRIVATE YTE)

#Set the working directory and startup project. 
set_target_properties(LinearRegression PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY ${YTE_Root}/Assets/YTE/Bin)
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT LinearRegression) 

#Set the working directory of the editor.
set_target_properties(YTEditor PROPERTIES VS_DEBUGGER_WORKING_DIRECTORY ${YTE_Root}/Assets/YTE/Bin)

target_compile_options(LinearRegression PRIVATE -permissive- -std:c++17 -WX- -W4)

YTE_Target_Set_LTCG(LinearRegression)

#Have to do /LTCG here because it appears as though setting this property resets it.
set_target_properties(LinearRegression PROPERTIES LINK_FLAGS_PUBLISH "/SUBSYSTEM:WINDOWS /LTCG")

install(TARGETS LinearRegression 
            RUNTIME DESTINATION BIN
            LIBRARY DESTINATION BIN
            ARCHIVE DESTINATION BIN
            COMPONENT applications)
