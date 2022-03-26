include_directories(include)

include(FetchContent)

FetchContent_Declare(
        doctest
        GIT_REPOSITORY https://github.com/doctest/doctest.git
        GIT_TAG        v2.4.8
)

FetchContent_MakeAvailable(doctest)

add_compile_options(-fcoroutines)

set(CORO_TESTS async_generator_tests.cpp async_auto_reset_event_tests.cpp async_latch_tests.cpp
        async_manual_reset_event_tests.cpp async_mutex_tests.cpp cancellation_token_tests.cpp
        generator_tests.cpp io_service_tests.cpp
        ip_address_tests.cpp ip_endpoint_tests.cpp ipv4_address_tests.cpp ipv4_endpoint_tests.cpp
        ipv6_address_tests.cpp ipv6_endpoint_tests.cpp multi_producer_sequencer_tests.cpp
        recursive_generator_tests.cpp scheduling_operator_tests.cpp sequence_barrier_tests.cpp
        shared_task_tests.cpp single_consumer_async_auto_reset_event_tests.cpp
        single_producer_sequencer_tests.cpp static_thread_pool_tests.cpp
        sync_wait_tests.cpp task_tests.cpp when_all_ready_tests.cpp when_all_tests.cpp)

include(CTest)
enable_testing()

foreach(TEST_FILE ${CORO_TESTS})
    get_filename_component(TEST_NAME ${TEST_FILE} NAME_WLE)
    add_executable(${TEST_NAME} ${TEST_FILE})
    target_link_libraries(${TEST_NAME} cppcoro doctest pthread)
    add_test(${TEST_NAME} ${TEST_NAME})
endforeach()
