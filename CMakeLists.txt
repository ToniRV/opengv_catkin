cmake_minimum_required(VERSION 2.8.3)
project(opengv_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(
  opengv_src
  GIT_REPOSITORY https://github.com/laurentkneip/opengv.git
  GIT_TAG master
  GIT_PROGRESS 1
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../opengv_src && cmake . -DCMAKE_BUILD_TYPE=Release \
      -DEIGEN3_INCLUDE_DIR=/usr/local/include/gtsam/3rdparty/Eigen \
      -DEIGEN3_INCLUDE_DIRS=/usr/local/include/gtsam/3rdparty/Eigen \
     # Change the path to GTSAM's eigen to your own! ideally use gtsam_catkin...
      -DCMAKE_INSTALL_PREFIX=/usr/local
  BUILD_COMMAND cd ../opengv_src && make -j 8
  INSTALL_COMMAND ""
)

install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/opengv
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})
cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  LIBRARIES opengv)
