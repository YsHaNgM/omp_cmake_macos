cmake_minimum_required(VERSION 3.12)
project(openmptest CXX)

add_executable(sample sample.cpp)

set(CMAKE_CXX_STANDARD 17)
option(CMAKECATCHMPI_USE_OPENMP "Enable OpenMP" ON)

if(CMAKECATCHMPI_USE_OPENMP)
    find_package(OpenMP)
    # Below was needed in macos and cmake 3.13
    # # If OpenMP wasn't found, try if we can find it in the default Macports location
    # if((NOT OPENMP_FOUND) AND (NOT OPENMP_CXX_FOUND) AND EXISTS "/opt/local/lib/libomp/libomp.dylib")
    #     set(OpenMP_C_FLAGS "-Xpreprocessor -fopenmp -I/opt/local/include/libomp/")
    #     set(OpenMP_C_LIB_NAMES omp)
    #     set(OpenMP_CXX_FLAGS "-Xpreprocessor -fopenmp -I/opt/local/include/libomp/")
    #     set(OpenMP_CXX_LIB_NAMES omp)
    #     set(OpenMP_omp_LIBRARY /opt/local/lib/libomp/libomp.dylib)

    #     find_package(OpenMP)
    #     if (OPENMP_FOUND OR OPENMP_CXX_FOUND)
    #         message(STATUS "Found libomp in macports default location.")
    #     else()
    #         message(FATAL_ERROR "Didn't find libomp. Tried macports default location but also didn't find it.")
    #     endif()
    # endif()

    # # If OpenMP wasn't found, try if we can find it in the default Homebrew location
    # if((NOT OPENMP_FOUND) AND (NOT OPENMP_CXX_FOUND) AND EXISTS "/usr/local/opt/libomp/lib/libomp.dylib")
    #     set(OpenMP_C_FLAGS "-Xpreprocessor -fopenmp -I/usr/local/opt/libomp/include")
    #     set(OpenMP_C_LIB_NAMES omp)
    #     set(OpenMP_CXX_FLAGS "-Xpreprocessor -fopenmp -I/usr/local/opt/libomp/include")
    #     set(OpenMP_CXX_LIB_NAMES omp)
    #     set(OpenMP_omp_LIBRARY /usr/local/opt/libomp/lib/libomp.dylib)

    #     find_package(OpenMP)
    #     if (OPENMP_FOUND OR OPENMP_CXX_FOUND)
    #         message(STATUS "Found libomp in homebrew default location.")
    #     else()
    #         message(FATAL_ERROR "Didn't find libomp. Tried homebrew default location but also didn't find it.")
    #     endif()
    # endif()

    # if((NOT OPENMP_FOUND) AND (NOT OPENMP_CXX_FOUND))
    #     message(FATAL_ERROR "Did not find OpenMP.")
    # endif()
    # SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
else()
    if( (CMAKE_CXX_COMPILER_ID MATCHES "[cC][lL][aA][nN][gG]")
            OR (CMAKE_CXX_COMPILER_ID MATCHES "[gG][nN][uU]"))
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-unknown-pragmas")
    endif()
endif(CMAKECATCHMPI_USE_OPENMP)

# PUBLIC OpenMP::OpenMP_CXX is needed for macos since cmake 3.16
target_link_libraries(sample PUBLIC OpenMP::OpenMP_CXX)
# ${OpenMP_CXX_FLAGS} ${OpenMP_libomp_LIBRARY} ${OpenMP_CXX_LIBRARIES}