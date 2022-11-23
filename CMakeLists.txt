# Copyright 2022 David Brown <d.brown@bigdavedev.com>
# SPDX-License-Identifier: MIT
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.
cmake_minimum_required (VERSION 3.21)

project (
	RingSpan
	VERSION 0.1.0
	LANGUAGES CXX
	DESCRIPTION "C++23 implementation of p0059 ring_span"
)

option (
	RING_SPAN_BUILD_TESTS
	"Build the unit tests for bdd::ring_span"
	${PROJECT_IS_TOP_LEVEL}
)
option (
	RING_SPAN_ENABLE_COVERAGE
	"Enable coverage reporting"
	${RING_SPAN_BUILD_TESTS}
)

add_library (ring_span INTERFACE)
add_library (BDD::RingSpan ALIAS ring_span)

target_compile_features (
	ring_span

	INTERFACE
	cxx_std_23
)

target_sources (
	ring_span

	INTERFACE
	${PROJECT_SOURCE_DIR}/include/bdd/ring_span.h
)

target_include_directories (
	ring_span

	INTERFACE
	$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
	$<INSTALL_INTERFACE:include>
)

if (PROJECT_IS_TOP_LEVEL)
	include (FetchContent)

	if (RING_SPAN_BUILD_TESTS)
		include (CTest)
		enable_testing ()

		# Use boost::ut v1.1.9 for unit testing
		FetchContent_Declare(
			BoostUt
			GIT_REPOSITORY https://github.com/boost-ext/ut
			GIT_TAG cd12498349362cc646a7140451bf51db2a2dac00
		)
		FetchContent_MakeAvailable(BoostUt)

		add_subdirectory (tests)
	endif ()
endif ()
