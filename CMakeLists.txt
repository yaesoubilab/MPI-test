include(FindMPI)

cmake_minimum_required(VERSION 2.8)

# Require C++14 compilation
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)
set(CLANG_DEFAULT_CXX_STDLIB "libc++")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")

find_package(Boost REQUIRED)
find_package(MPI REQUIRED COMPONENTS CXX)

# Set source files
set(src_primes primes.cpp)
set(src_hello hello_world.cpp)

# include_directories(${Boost_INCLUDE_DIRS})

add_executable(primes ${src_primes})
add_executable(hello ${src_hello})
target_link_libraries(primes PUBLIC ${MPI_CXX_LIBRARIES})
target_link_libraries(hello PUBLIC ${MPI_CXX_LIBRARIES})

enable_testing()
add_test(NAME primes_exec
         COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} ${MPIEXEC_MAX_NUMPROCS}
                   ${MPIEXEC_PREFLAGS} ${CMAKE_CURRENT_BINARY_DIR}/primes)
add_test(NAME hello_exec
         COMMAND ${MPIEXEC_EXECUTABLE} ${MPIEXEC_NUMPROC_FLAG} ${MPIEXEC_MAX_NUMPROCS}
                   ${MPIEXEC_PREFLAGS} ${CMAKE_CURRENT_BINARY_DIR}/hello)
