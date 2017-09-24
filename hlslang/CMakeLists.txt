
file(GLOB source_files "${PROJECT_SOURCE_DIR}/hlslang/GLSLCodeGen/*.cpp")
add_library(hlslang
    ${source_files}
)

add_subdirectory(GLSLCodeGen)
add_subdirectory(OSDependent)
add_subdirectory(MachineIndependent)