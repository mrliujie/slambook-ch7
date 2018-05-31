cmake_minimum_required( VERSION 2.8 )
project( vo1 )

set( CMAKE_BUILD_TYPE "Release" )
set( CMAKE_CXX_FLAGS "-std=c++11 -O3" )

# 添加cmake模块以使用g2o
list( APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules )

find_package( OpenCV 3.1 REQUIRED )
# find_package( OpenCV REQUIRED ) # use this if in OpenCV2 
find_package( G2O REQUIRED )
find_package( CSparse REQUIRED )

include_directories( 
    ${OpenCV_INCLUDE_DIRS} 
    ${G2O_INCLUDE_DIRS}
    ${CSPARSE_INCLUDE_DIR}
    "/usr/include/eigen3/"
)

set(G2O_LIBS ${G2O_CORE_LIBRARY} 
    ${G2O_STUFF_LIBRARY} 
    ${G2O_SOLVER_CSPARSE} 
    ${G2O_SOLVER_CSPARSE_EXTENSION} 
    ${G2O_SOLVER_PCG}
    ${G2O_TYPES_SBA}
    )

message(STATUS "g2o libs " ${G2O_LIBS})
message(STATUS "CSPARSE_LIBRARY " ${CSPARSE_LIBRARY})

add_executable( feature_extraction feature_extraction.cpp  )
target_link_libraries( feature_extraction ${OpenCV_LIBS} )

# add_executable( pose_estimation_2d2d pose_estimation_2d2d.cpp extra.cpp ) # use this if in OpenCV2 
add_executable( pose_estimation_2d2d pose_estimation_2d2d.cpp )
target_link_libraries( pose_estimation_2d2d ${OpenCV_LIBS} )

# add_executable( triangulation triangulation.cpp extra.cpp) # use this if in opencv2 
add_executable( triangulation triangulation.cpp )
target_link_libraries( triangulation ${OpenCV_LIBS} )

add_executable( pose_estimation_3d2d pose_estimation_3d2d.cpp )
target_link_libraries( pose_estimation_3d2d 
   ${OpenCV_LIBS}
   ${CSPARSE_LIBRARY}
   ${G2O_LIBS}
#    g2o_core g2o_stuff g2o_types_sba g2o_csparse_extension
)

add_executable( pose_estimation_3d3d pose_estimation_3d3d.cpp )
target_link_libraries( pose_estimation_3d3d 
   ${OpenCV_LIBS}
   ${G2O_LIBS}
#    g2o_core g2o_stuff g2o_types_sba g2o_csparse_extension 
   ${CSPARSE_LIBRARY}
)
