add_executable(hlsl2glsltest
"${PROJECT_SOURCE_DIR}/tests/hlsl2glsltest/hlsl2glsltest.cpp"
)

target_link_libraries(hlsl2glsltest PRIVATE GLSLCodeGen opengl32)