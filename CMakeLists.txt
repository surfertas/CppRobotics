cmake_minimum_required(VERSION 3.1)
project(CPP_ROBOTICS)

set(CMAKE_CXX_STANDARD 11)
# set(CMAKE_BUILD_TYPE Debug)
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#############################
#Dependencies
#############################

find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

find_package(OpenCV REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

# include_directories(/home/tai/Software/Ipopt-3.12.13/build/include)
# link_directories(/home/tai/Software/Ipopt-3.12.13/build/lib)

include_directories(./include)

##############################
#Localization
##############################

add_executable(ekf src/extended_kalman_filter.cpp)
target_link_libraries(ekf ${OpenCV_LIBS} )

add_executable(pf src/particle_filter.cpp)
target_link_libraries(pf ${OpenCV_LIBS} )

##############################
#PathPlanning
##############################

add_executable(dwa src/dynamic_window_approach.cpp)
target_link_libraries(dwa ${OpenCV_LIBS} )

add_executable(mptg src/model_predictive_trajectory_generator.cpp)
target_link_libraries(mptg ${OpenCV_LIBS})

add_executable(slp src/state_lattice_planner.cpp)
target_link_libraries(slp ${OpenCV_LIBS})

add_executable(csp src/cubic_spline_planner.cpp)
target_link_libraries(csp ${OpenCV_LIBS})

add_executable(frenet src/frenet_optimal_trajectory.cpp)
target_link_libraries(frenet ${OpenCV_LIBS})


##############################
#PathTracking
##############################

add_executable(lqr src/lqr_steer_control.cpp)
target_link_libraries(lqr ${OpenCV_LIBS})

add_executable(lqr_full src/lqr_speed_steer_control.cpp)
target_link_libraries(lqr_full ${OpenCV_LIBS})

# add_executable(mpc src/model_predictive_control.cpp)
# target_link_libraries(mpc ${OpenCV_LIBS} ipopt)

cmake_minimum_required(VERSION 2.8.9)
project (PrimeUtil)
set(CMAKE_CXX_STANDARD 11)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

##############################
#googletest
##############################

# Download and unpack googletest at configure time
configure_file(CMakeLists.txt.googletest googletest-download/CMakeLists.txt)
execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" .
  RESULT_VARIABLE result
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/googletest-download )
if(result)
  message(FATAL_ERROR "CMake step for googletest failed: ${result}")
endif()
execute_process(COMMAND ${CMAKE_COMMAND} --build .
  RESULT_VARIABLE result
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/googletest-download )
if(result)
  message(FATAL_ERROR "Build step for googletest failed: ${result}")
endif()

# Prevent overriding the parent project's compiler/linker
# settings on Windows
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

# Add googletest directly to our build. This defines
# the gtest and gtest_main targets.
add_subdirectory(${CMAKE_CURRENT_BINARY_DIR}/googletest-src
                 ${CMAKE_CURRENT_BINARY_DIR}/googletest-build
                 EXCLUDE_FROM_ALL)

# The gtest/gtest_main targets carry header search path
# dependencies automatically when using CMake 2.8.11 or
# later. Otherwise we have to add them here ourselves.
if (CMAKE_VERSION VERSION_LESS 2.8.11)
  include_directories("${gtest_SOURCE_DIR}/include")
endif()

