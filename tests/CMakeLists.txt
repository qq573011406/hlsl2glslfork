add_executable(hlsl2glsltest
"${PROJECT_SOURCE_DIR}/tests/hlsl2glsltest/hlsl2glsltest.cpp"
)
if(WIN32)
target_link_libraries(hlsl2glsltest PRIVATE GLSLCodeGen opengl32)
elseif(APPLE)
FIND_LIBRARY(OPENGL_LIBRARY OpenGL)
FIND_LIBRARY(COCOA_LIBRARY Cocoa)
target_link_libraries(hlsl2glsltest PRIVATE GLSLCodeGen ${OPENGL_LIBRARY} ${COCOA_LIBRARY})
elseif(UNIX)
target_link_libraries(hlsl2glsltest PRIVATE GLSLCodeGen GL glut GLEW pthread)
endif()
