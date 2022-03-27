cmake_minimum_required(VERSION 3.14)
project(cppcoro)

set(CMAKE_CXX_STANDARD 20)

include_directories(include)

set(CPPCORO_INC_DETAILS include/cppcoro/detail/any.hpp include/cppcoro/detail/get_awaiter.hpp
        include/cppcoro/detail/lightweight_manual_reset_event.hpp include/cppcoro/detail/manual_lifetime.hpp
        include/cppcoro/detail/remove_rvalue_reference.hpp include/cppcoro/detail/remove_rvalue_reference.hpp
        include/cppcoro/detail/sync_wait_task.hpp include/cppcoro/detail/unwrap_reference.hpp
        include/cppcoro/detail/void_value.hpp include/cppcoro/detail/when_all_counter.hpp
        include/cppcoro/detail/when_all_ready_awaitable.hpp include/cppcoro/detail/when_all_task.hpp)

set(CPPCORO_INC_NET include/cppcoro/net/ip_address.hpp include/cppcoro/net/ip_endpoint.hpp
        include/cppcoro/net/ipv4_address.hpp include/cppcoro/net/ipv4_endpoint.hpp
        include/cppcoro/net/ipv6_address.hpp include/cppcoro/net/ipv6_endpoint.hpp)

set(CPPCORO_INC include/cppcoro/async_auto_reset_event.hpp include/cppcoro/async_generator.hpp
        include/cppcoro/async_latch.hpp include/cppcoro/async_manual_reset_event.hpp
        include/cppcoro/async_mutex.hpp include/cppcoro/async_scope.hpp
        include/cppcoro/awaitable_traits.hpp include/cppcoro/broken_promise.hpp
        include/cppcoro/cancellation_registration.hpp include/cppcoro/cancellation_source.hpp
        include/cppcoro/cancellation_token.hpp include/cppcoro/config.hpp
        include/cppcoro/file_buffering_mode.hpp include/cppcoro/file_open_mode.hpp
        include/cppcoro/file_share_mode.hpp include/cppcoro/fmap.hpp
        include/cppcoro/generator.hpp include/cppcoro/inline_scheduler.hpp include/cppcoro/io_service.hpp
        include/cppcoro/is_awaitable.hpp include/cppcoro/multi_producer_sequencer.hpp
        include/cppcoro/on_scope_exit.hpp include/cppcoro/operation_cancelled.hpp
        include/cppcoro/recursive_generator.hpp include/cppcoro/resume_on.hpp include/cppcoro/round_robin_scheduler.hpp
        include/cppcoro/schedule_on.hpp include/cppcoro/sequence_barrier.hpp include/cppcoro/sequence_range.hpp
        include/cppcoro/sequence_traits.hpp include/cppcoro/shared_task.hpp
        include/cppcoro/single_consumer_async_auto_reset_event.hpp include/cppcoro/single_consumer_event.hpp
        include/cppcoro/single_producer_sequencer.hpp include/cppcoro/static_thread_pool.hpp
        include/cppcoro/sync_wait.hpp include/cppcoro/task.hpp include/cppcoro/when_all.hpp
        include/cppcoro/when_all_ready.hpp)

set(CPPCORO_SRC lib/async_auto_reset_event.cpp lib/async_mutex.cpp lib/async_manual_reset_event.cpp
        lib/auto_reset_event.hpp lib/auto_reset_event.cpp lib/cancellation_registration.cpp
        lib/cancellation_source.cpp lib/cancellation_state.hpp lib/cancellation_state.cpp
        lib/cancellation_token.cpp lib/io_service.cpp lib/ip_address.cpp lib/ip_endpoint.cpp
        lib/ipv4_address.cpp lib/ipv4_endpoint.cpp lib/ipv6_address.cpp lib/ipv6_endpoint.cpp
        lib/lightweight_manual_reset_event.cpp lib/spin_mutex.hpp lib/spin_mutex.cpp
        lib/spin_wait.hpp lib/spin_wait.cpp lib/static_thread_pool.cpp)

enable_testing()
option(CPPCORO_BUILD_TESTS "Build test programs" ON)
if(CPPCORO_BUILD_TESTS)
    add_subdirectory(test)
endif()

if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    add_compile_options(-fcoroutines)
endif()

add_library(${PROJECT_NAME} SHARED ${CPPCORO_INC_DETAIL} ${CPPCORO_INC_NET} ${CPPCORO_INC} ${CPPCORO_SRC})
target_link_libraries(${PROJECT_NAME} pthread)
