# from libdeepvac
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
file(GLOB EXAMPLES_LIST src/*.cpp)
message(STATUS "found EXAMPLES_LIST: " ${EXAMPLES_LIST})

#main program
file(GLOB SYSZUX_EXTRACT */syszux_extract_feature.cpp)

find_library(CRYPTO crypto)
if(CRYPTO)
    message(STATUS "found crypto library: " ${CRYPTO})
    add_executable(syszux_extract_feature ${SYSZUX_EXTRACT})
    target_link_libraries(syszux_extract_feature ${OpenCV_LIBS} ${TORCH_LIBRARIES} ${CRYPTO} deepvac )
    #set_property(TARGET syszux_extract_feature PROPERTY CXX_STANDARD 17)
endif()

#test examples
set(examples_dest "bin")

file( GLOB EXAMPLES_LIST ${CMAKE_SOURCE_DIR}/examples/*/test_*.cpp )
foreach( testsyszuxfile ${EXAMPLES_LIST} )
    get_filename_component(testfile "${testsyszuxfile}" NAME)
    message(STATUS "Add test binary: ${testfile}")
    string(REPLACE ".cpp" "" testname ${testfile} )
    add_executable( ${testname} ${testsyszuxfile} )
    target_link_libraries( ${testname} "${TORCH_LIBRARIES}" ${OpenCV_LIBS} deepvac)
    install(TARGETS ${testname} DESTINATION "${examples_dest}")
endforeach( testsyszuxfile ${EXAMPLES_LIST} )