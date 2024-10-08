cmake_minimum_required(VERSION 3.17) 

macro(add_k1ee_test _TEST_NAME)
    add_executable(test_${_TEST_NAME} test_${_TEST_NAME}.cpp)
    target_link_libraries(test_${_TEST_NAME} PRIVATE
        k1ee::k1ee)
    add_test(NAME k1ee_test_${_TEST_NAME} COMMAND test_${_TEST_NAME})
    # Group under the "tests" project folder in IDEs such as Visual Studio.
    set_property(TARGET test_${_TEST_NAME} PROPERTY FOLDER "Tests")
    if(WIN32 AND BUILD_SHARED_LIBS)
        add_custom_command(TARGET test_${_TEST_NAME} POST_BUILD COMMAND ${CMAKE_COMMAND}
            -E copy $<TARGET_FILE:k1ee> $<TARGET_FILE_DIR:test_${_TEST_NAME}>)
    endif()
endmacro()

add_k1ee_test(file)
add_k1ee_test(hook)
add_k1ee_test(encoding)

add_custom_command(TARGET test_file POST_BUILD COMMAND ${CMAKE_COMMAND}
    -E make_directory $<TARGET_FILE_DIR:test_file>/data)
add_custom_command(TARGET test_file POST_BUILD COMMAND ${CMAKE_COMMAND}
    -E copy ${CMAKE_CURRENT_SOURCE_DIR}/data/test_data.dat
    $<TARGET_FILE_DIR:test_file>/data/test_data.dat)
