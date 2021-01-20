
cmake_minimum_required(VERSION 3.0)

project(examples)


set(CMAKE_CXX_FLAGS "-DENABLE_LOGGING=1 -DENABLE_HEAVY_LOGGING=1 -Wall -Wextra -g -fPIE -g -pg -std=gnu++11")
#SET(CMAKE_BUILD_TYPE "Debug")
#SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
#SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

add_definitions(
	-DENABLE_SOCK_CLOEXEC=1 
	-DHAI_ENABLE_SRT=1 
	-DHAI_PATCH=1 
	-DHAVE_INET_PTON=1 
	-DLINUX=1 
	-DSRT_ENABLE_APP_READER 
	-DSRT_ENABLE_BINDTODEVICE 
	-DSRT_ENABLE_CLOSE_SYNCH 
	-DSRT_ENABLE_ENCRYPTION 
	-DSRT_VERSION=\"1.4.2\" 
	-DUSE_OPENSSL=1 
	-D_GNU_SOURCE
)

macro(srt_make_application name)
	target_link_libraries(${name} libsrt.a -ldl -lssl -lcrypto -pthread)
endmacro()

macro(srt_add_example mainsrc)
	get_filename_component(name ${mainsrc} NAME_WE)
	add_executable(${name} examples/${mainsrc})
endmacro()

add_executable(sendfile examples/sendfile.cpp)
target_link_libraries(sendfile libsrt.a -ldl -lssl -lcrypto -pthread)

add_executable(recvfile examples/recvfile.cpp)
target_link_libraries(recvfile libsrt.a -ldl -lssl -lcrypto -pthread)

#srt_add_example(recvlive.cpp)
#srt_make_application(recvlive)

#srt_add_example(test-c-client.c)
#srt_make_application(test-c-client)

#srt_add_example(test-c-server.c)
#srt_make_application(test-c-server)

#srt_add_example(testcapi-connect.c)
#target_link_libraries(testcapi-connect -lsrt)








