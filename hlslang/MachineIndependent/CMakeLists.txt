add_library(MachineIndependent
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/ConstantFolding.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/HLSL2GLSL.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/InfoSink.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Intermediate.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/intermOut.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/IntermTraverse.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/ParseHelper.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/PoolAlloc.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/RemoveTree.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/SymbolTable.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang_tab.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Initialize.cpp"
    "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/localintermediate.h"
)

target_link_libraries(MachineIndependent PRIVATE preprocessor OSDependent)
target_include_directories(MachineIndependent PUBLIC  "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/")

if(WIN32)
target_include_directories(MachineIndependent PUBLIC 
"${PROJECT_SOURCE_DIR}/hlslang/OSDependent/Windows/"
)

add_custom_command(OUTPUT ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang.cpp
COMMAND ${PROJECT_SOURCE_DIR}/tools/flex.exe ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang.l
MAIN_DEPENDENCY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang.l
WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/
COMMENT "Executing flex on hlslang.l"
)

add_custom_command(OUTPUT ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang_tab.cpp
COMMAND bison.exe -d -t -v ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang.y
COMMAND move /y hlslang.tab.h ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang_tab.h
COMMAND move /y hlslang.tab.c ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang_tab.cpp
MAIN_DEPENDENCY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang.y
WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/tools/bin/
COMMENT "Executing Bison on hlslang.y"
)



elseif(APPLE)
target_include_directories(MachineIndependent PUBLIC 
"${PROJECT_SOURCE_DIR}/hlslang/OSDependent/Mac/"
)


add_custom_command(OUTPUT ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang.cpp
COMMAND flex hlslang.l
MAIN_DEPENDENCY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang.l
WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/
COMMENT "Executing flex on hlslang.l"
)

add_custom_command(OUTPUT ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang_tab.cpp
COMMAND bison -d -t -v hlslang.y
COMMAND mv -f hlslang.tab.c Gen_hlslang_tab.cpp
COMMAND mv -f hlslang.tab.h hlslang_tab.h
MAIN_DEPENDENCY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang.y
WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/
COMMENT "Executing Bison on hlslang.y"
)


elseif(UNIX)
target_include_directories(MachineIndependent PUBLIC 
"${PROJECT_SOURCE_DIR}/hlslang/OSDependent/Linux/"
)

add_custom_command(OUTPUT ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang.cpp
COMMAND flex hlslang.l
MAIN_DEPENDENCY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang.l
WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/
COMMENT "Executing flex on hlslang.l"
)

add_custom_command(OUTPUT ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang_tab.cpp
COMMAND bison -d -t -v hlslang.y
COMMAND mv -f hlslang.tab.c Gen_hlslang_tab.cpp
COMMAND mv -f hlslang.tab.h hlslang_tab.h
MAIN_DEPENDENCY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/hlslang.y
WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/
COMMENT "Executing Bison on hlslang.y"
)


endif(WIN32)

add_custom_target(parser ALL
DEPENDS ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang.cpp ${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang_tab.cpp
)

set_source_files_properties(${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang.cpp PROPERTIES GENERATED 1)
set_source_files_properties(${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/Gen_hlslang_tab.cpp PROPERTIES GENERATED 1)



add_subdirectory(preprocessor)