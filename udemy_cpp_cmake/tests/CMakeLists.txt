set(TEST_SOURCES "main.cpp")

add_executable(${TEST_MAIN} ${TEST_SOURCES})

target_link_libraries(${TEST_MAIN} PRIVATE ${LIBRARY_NAME} ${CONAN_CATCH2})
target_compile_features(${TEST_MAIN} PUBLIC cxx_std_17)

add_test(NAME ${LIBRARY_NAME}.${TEST_MAIN} COMMAND ${TEST_MAIN})

if(ENABLE_COVERAGE)
    set(COVERAGE_MAIN "${PROJECT}_Coverage")
    set(COVERAGE_LCOV_EXCLUDES 
        "${PROJECT_SOURCE_DIR}/app/*"
        "${PROJECT_SOURCE_DIR}/cmake/*"
        "${PROJECT_SOURCE_DIR}/docs/*"
        "${PROJECT_SOURCE_DIR}/external/*"
        "${PROJECT_SOURCE_DIR}/tests/*"
        "${CONAN_BOOST_ROOT}/*"
        "${CONAN_CATCH2_ROOT}/*"
        "${CONAN_CELERO_ROOT}/*"
        "${CONAN_CXXOPTS_ROOT}/*"
        "/usr/include/*"
        )
    setup_target_for_coverage_lcov(
        NAME ${COVERAGE_MAIN}
        EXECUTABLE ctest
        DEPENDENCIES ${TEST_MAIN}
    )
endif()