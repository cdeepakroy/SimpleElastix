cmake_minimum_required(VERSION 2.8)
project( SimpleITKExamples )

if( NOT SimpleITK_SOURCE_DIR )
  find_package( SimpleITK REQUIRED )
  include( ${SimpleITK_USE_FILE} )

  # Add compiler flags needed to use SimpleITK.
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${SimpleITK_REQUIRED_C_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${SimpleITK_REQUIRED_CXX_FLAGS}")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${SimpleITK_REQUIRED_LINK_FLAGS}")
  set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} ${SimpleITK_REQUIRED_LINK_FLAGS}")
  set(CMAKE_MODULE_LINKER_FLAGS "${CMAKE_MODULE_LINKER_FLAGS} ${SimpleITK_REQUIRED_LINK_FLAGS}")
endif()

find_package( ITK REQUIRED )
include( ${ITK_USE_FILE} )

if( NOT EXISTS ${ELASTIX_USE_FILE} )
  set( ELASTIX_USE_FILE ${ELASTIX_DIR}/UseElastix.cmake )
endif()

if( NOT EXISTS ${ELASTIX_USE_FILE} )
  # UseElastix.cmake path NOT passed to build. Ask user to specify custom elastix install or use the SuperBuild.
  set( ELASTIX_DIR "" CACHE PATH "Path to elastix build folder" )
  message(FATAL_ERROR "Could not find UseElastix.cmake. Point ELASTIX_DIR to folder containing UseElastix.cmake or use SuperBuild.")
endif()

include( ${ELASTIX_USE_FILE} )

# Add individual cxx executables
add_executable ( SimpleGaussian SimpleGaussian.cxx )
target_link_libraries ( SimpleGaussian ${SimpleITK_LIBRARIES} )

add_executable ( SimpleGaussianFunctional SimpleGaussianFunctional.cxx )
target_link_libraries ( SimpleGaussianFunctional ${SimpleITK_LIBRARIES} )

add_executable ( BufferImportExport BufferImportExport.cxx )
target_link_libraries ( BufferImportExport ${SimpleITK_LIBRARIES} )

add_executable ( FilterProgressReporting FilterProgressReporting.cxx )
target_link_libraries ( FilterProgressReporting ${SimpleITK_LIBRARIES} )

# Add subdirectories
add_subdirectory(Segmentation)
add_subdirectory(SimpleElastix)
