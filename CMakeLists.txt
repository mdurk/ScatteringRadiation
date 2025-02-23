cmake_minimum_required(VERSION 3.10)

# set the project name
project(ScatteringRadiation)

# Require C++14 CMake standards
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Include Geant4
find_package(Geant4 REQUIRED ui_all vis_all)
find_package(ROOT REQUIRED)
#find_package(geant4 REQUIRED)

# Set it up
include(${Geant4_USE_FILE})

# Include my headers dir
include_directories(include
	${Geant4_INCLUDE_DIR}
	${ROOT_INCLUDE_DIRS})

# Make me a setup script directly in the build directory
configure_file(tools/setup.sh.in setup.sh @ONLY)

# add the executable
add_executable(run_beam src/run_beam.cxx
	src/Construction.cxx
	src/Beam.cxx
	src/Action.cxx
	src/SensitiveSurface.cxx
	src/Run.cxx
	src/Event.cxx
	src/Stepping.cxx
	src/Track.cxx)

add_executable(analyze_data src/analyze_data.cxx)
	
add_executable(predict_distributions src/predict_distributions.cxx
	src/Functions.cxx)

target_link_libraries(run_beam ${Geant4_LIBRARIES} ${ROOT_LIBRARIES})
target_link_libraries(analyze_data ${Geant4_LIBRARIES} ${ROOT_LIBRARIES})
target_link_libraries(predict_distributions ${Geant4_LIBRARIES} ${ROOT_LIBRARIES})

