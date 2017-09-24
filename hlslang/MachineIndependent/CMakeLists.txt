file(GLOB source_files "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/*.cpp")
add_library(MachineIndependent
    ${source_files}
)

target_link_libraries(MachineIndependent PRIVATE preprocessor OSDependent)

if(WIN32)
target_include_directories(MachineIndependent PUBLIC 
"${PROJECT_SOURCE_DIR}/hlslang/OSDependent/Windows/"
)
elseif(APPLE)
target_include_directories(MachineIndependent PUBLIC 
"${PROJECT_SOURCE_DIR}/hlslang/OSDependent/Mac/"
)
elseif(UNIX)
target_include_directories(MachineIndependent PUBLIC 
"${PROJECT_SOURCE_DIR}/hlslang/OSDependent/Linux/"
)
endif(WIN32)


add_subdirectory(preprocessor)