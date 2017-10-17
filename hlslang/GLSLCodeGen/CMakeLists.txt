file(GLOB source_files "${PROJECT_SOURCE_DIR}/hlslang/GLSLCodeGen/*.cpp" "${PROJECT_SOURCE_DIR}/hlslang/GLSLCodeGen/*.h")
add_library(GLSLCodeGen
    ${source_files}
)
target_include_directories(GLSLCodeGen PUBLIC "${PROJECT_SOURCE_DIR}/hlslang/MachineIndependent/")
target_link_libraries(GLSLCodeGen MachineIndependent)
