
cmake_minimum_required (VERSION 2.8.12 FATAL_ERROR)
# XXX This can be potentially done in future, but there still exist
# some dependent project using cmake 2.8 - this can't be done this way.
#cmake_minimum_required (VERSION 3.0.2 FATAL_ERROR)
#project(SRT VERSION "1.4.2")
project(SRT C CXX)

set (SRT_VERSION 1.4.2)

# Use application-defined group reader
# (currently the only one implemented)
add_definitions(-DSRT_ENABLE_APP_READER)

# XXX This was added once as experimental, it is now in force for
# write-blocking-mode sockets. Still unclear if all issues around
# closing while data still not written are eliminated.
add_definitions(-DSRT_ENABLE_CLOSE_SYNCH)

add_definitions(
	-D_GNU_SOURCE
	-DHAI_PATCH=1
	-DHAI_ENABLE_SRT=1
	-DSRT_VERSION="${SRT_VERSION}"
)

macro(srt_make_application name)
	set(CMAKE_CXX_FLAGS  "-g -Wall -std=c++11")
	target_link_libraries(${name} libsrt.a pthread crypto stdc++ -lm)
endmacro()

macro(srt_add_example mainsrc)
	get_filename_component(name ${mainsrc} NAME_WE)
	add_executable(${name} examples/${mainsrc} ${ARGN})
endmacro()

srt_add_example(sendfile.cpp)
srt_make_application(sendfile)

srt_add_example(recvfile.cpp)
srt_make_application(recvfile)

srt_add_example(recvlive.cpp)
srt_make_application(recvlive)

srt_add_example(test-c-client.c)
srt_make_application(test-c-client)

srt_add_example(test-c-server.c)
srt_make_application(test-c-server)

#srt_add_example(test-c-client-bonding.c)
#srt_make_application(test-c-client-bonding)

#srt_add_example(test-c-server-bonding.c)
#srt_make_application(test-c-server-bonding)

srt_add_example(testcapi-connect.c)
target_link_libraries(testcapi-connect -lsrt)








