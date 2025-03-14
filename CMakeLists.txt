cmake_minimum_required(VERSION 3.10)
project(cuckoofilter CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -fno-strict-aliasing")

include_directories(include src)

add_library(cuckoofilter STATIC src/hashutil.cc)

# test
add_executable(test example/test.cc)
target_link_libraries(test PRIVATE
    cuckoofilter
    ssl
    crypto
)

# install
file(GLOB HEADERS "src/*.h")
install(FILES ${HEADERS} DESTINATION include/cuckoofilter)
install(TARGETS cuckoofilter DESTINATION lib)

# uninstall
add_custom_target(uninstall
    COMMAND rm -f $<TARGET_FILE:cuckoofilter>
    COMMAND rm -rf ${CMAKE_INSTALL_PREFIX}/include/cuckoofilter
    COMMENT "Removing installed files"
)

