file(GLOB source_files "${PROJECT_SOURCE_DIR}/hlslang/GLSLCodeGen/*.cpp")
add_library(GLSLCodeGen
    ${source_files}
)
target_link_libraries(GLSLCodeGen MachineIndependent)
target_include_directories(GLSLCodeGen PUBLIC "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/")