cmake_minimum_required(VERSION 3.16)
project(concurrencpp)

add_library(
	concurrencpp

	include/concurrencpp.h

	src/executors/constants.h
	src/executors/derivable_executor.h
	src/executors/executor.cpp
	src/executors/executor.h
	src/executors/executor_all.h
	src/executors/inline_executor.h
	src/executors/manual_executor.cpp
	src/executors/manual_executor.h
	src/executors/thread_executor.cpp
	src/executors/thread_executor.h
	src/executors/thread_pool_executor.cpp
	src/executors/thread_pool_executor.h
	src/executors/worker_thread_executor.cpp
	src/executors/worker_thread_executor.h

	src/results/constants.h
	src/results/executor_exception.h
	src/results/make_result.h
	src/results/promises.cpp
	src/results/promises.h
	src/results/result.h
	src/results/result_awaitable.h
	src/results/result_core.cpp
	src/results/result_core.h
	src/results/result_fwd_declerations.h
	src/results/when_result.h

	src/runtime/constants.h
	src/runtime/runtime.cpp
	src/runtime/runtime.h

	src/threads/thread.cpp
	src/threads/thread.h

	src/timers/constants.h
	src/timers/timer.cpp
	src/timers/timer.h
	src/timers/timer_queue.cpp
	src/timers/timer_queue.h

	src/utils/bind.h

	src/errors.h
	src/forward_declerations.h
	src/platform_defs.h
)

target_include_directories(concurrencpp PUBLIC include)

if (MSVC)
	target_compile_options(concurrencpp PUBLIC /await /permissive-)
else()
	target_compile_options(concurrencpp PUBLIC -stdlib=libc++ -fcoroutines-ts)
	target_link_options(concurrencpp PUBLIC -stdlib=libc++)
	target_link_libraries(concurrencpp PUBLIC pthread)
endif()

find_library(LIBRT rt)
if(LIBRT)
	target_link_libraries(concurrencpp PUBLIC ${LIBRT})
endif()
