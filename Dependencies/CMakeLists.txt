add_subdirectory(YTE)

include(YTE/cmake/YTE_Target_Folder.cmake)

YTE_Target_Folder(YTEditor Dependencies)
YTE_Cleanup_Orphan_Targets(Ignore)
YTE_Target_Set_Outputs(YTEditor ${LinearRegression_Library_Dir} ${LinearRegression_Binary_Dir})
