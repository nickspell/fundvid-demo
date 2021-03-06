ADD_SUBDIRECTORY(libImage)

FIND_PACKAGE(OpenMP) 
if (OPENMP_FOUND) 
  SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
endif (OPENMP_FOUND)

# Executable demo to estimate the fundamental matrix from SIFT matches
ADD_EXECUTABLE(demo_orsa_fundamental orsa_fundamental.cpp Rect.cpp)
TARGET_LINK_LIBRARIES(demo_orsa_fundamental Orsa image sift)

# Utility to overlay points and lines on images (used by IPOL demo)
ADD_EXECUTABLE(put_epipolar put_epipolar.cpp Rect.cpp)
TARGET_LINK_LIBRARIES(put_epipolar image)

IF(UNIX)
    SET_TARGET_PROPERTIES(demo_orsa_fundamental PROPERTIES
                          COMPILE_FLAGS "-std=c++98 -Wall -Wextra -Werror")
    SET_TARGET_PROPERTIES(put_epipolar PROPERTIES
                          COMPILE_FLAGS "-std=c++98 -Wall -Wextra -Werror")
ENDIF(UNIX)

INSTALL(FILES CMakeLists.txt
              cmdLine.h
              orsa_fundamental.cpp
              put_epipolar.cpp
              Rect.hpp
              Rect.cpp
              siftMatch.hpp
              warping.hpp
        DESTINATION src/demo)
