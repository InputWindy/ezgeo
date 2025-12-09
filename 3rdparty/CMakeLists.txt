set(THIRD_PARTY_DIR "ThirdParty")
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

include(vcglib.cmake)

if(NOT TARGET metis)
  add_subdirectory(metis)
  set_target_properties(metis PROPERTIES FOLDER ${THIRD_PARTY_DIR}/metis)
endif()

if(NOT TARGET glm)
  option(BUILD_STATIC_LIBS "" ON)
  option(BUILD_TESTING "" OFF)
  option(GLM_TEST_ENABLE "" OFF)
  add_subdirectory(glm)
  set_target_properties(glm_static PROPERTIES FOLDER ${THIRD_PARTY_DIR}/glm)
endif()

if(NOT TARGET stb)
  include(stb.cmake)
  set_target_properties(stb PROPERTIES FOLDER ${THIRD_PARTY_DIR}/stb)
endif()

if(NOT TARGET assimp)
  option(BUILD_SHARED_LIBS "BUILD_SHARED_LIBS" OFF)

  # open fbx\ply\obj importer
  set(ASSIMP_BUILD_FBX_IMPORTER TRUE)
  set(ASSIMP_BUILD_OBJ_IMPORTER TRUE)
  set(ASSIMP_BUILD_PLY_IMPORTER TRUE)

  # open fbx\ply\obj exporter
  set(ASSIMP_BUILD_FBX_EXPORTER TRUE)
  set(ASSIMP_BUILD_OBJ_EXPORTER TRUE)
  set(ASSIMP_BUILD_PLY_EXPORTER TRUE)

  add_subdirectory(assimp)
  include_directories(assimp/include/)

  if(CMAKE_CXX_COMPILER_ID MATCHES "MSVC")
    target_compile_options(assimp PRIVATE /WX-)
  else()
    target_compile_options(assimp PRIVATE -Wno-error)
  endif()

  # target_include_directories(assimp PUBLIC ./assimp/include )
  set_target_properties(assimp PROPERTIES FOLDER ${THIRD_PARTY_DIR}/assimp)
endif()

