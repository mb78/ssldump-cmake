cmake_minimum_required (VERSION 3.10)

project (ssldump)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY build)

add_compile_options( -g -O2 -Wall -Wframe-larger-than=524288 -Wno-unused-variable -Wno-unused-function -Wno-parentheses -isystem./common )
add_definitions( -DLINUX -DOPENSSL -DDEBUG -DHAVE_STRDUP -DHAVE_SYS_TIME_H )

find_library(PCAP_LIB pcap /usr/lib /usr/lib64 /usr/lib/x86_64-linux-gnu REQUIRED)
find_package(OpenSSL REQUIRED)
link_directories(${OPENSSL_LIBRARIES})

#find_package(PkgConfig REQUIRED)
#pkg_search_module(OPENSSL REQUIRED openssl)
message(">>>${OpenSSL_INCLUDE_DIR} ${OpenSSL_FOUND}")
include_directories(${OPENSSL_INCLUDE_DIR}/openssl)
include_directories(common common/include null base ssl)

set(SOURCES
	./null/null_analyze.c
	./common/lib/r_errors.c
	./common/lib/r_time.c
	./common/lib/r_bitfield.c
	./common/lib/debug.c
	./common/lib/r_data.c
	./common/lib/r_replace.c
	./common/lib/r_list.c
	./common/lib/r_assoc_test.c
	./common/lib/r_assoc.c
	./ssl/lex.yy.c
	./ssl/ciphersuites.c
	./ssl/ssl_enum.c
	./ssl/main.c
	./ssl/y.tab.c
	./ssl/ssl_analyze.c
	./ssl/ssldecode.c
	./ssl/ssl.enums.c
	./ssl/sslprint.c
	./ssl/sslxprint.c
	./ssl/ssl_rec.c
	./base/pcap-snoop.c
	./base/proto_mod.c
	./base/network.c
	./base/tcpconn.c
	./base/debug.c
	./base/tcppack.c
	./base/common.c
	./base/print_utils.c
	)

add_executable(ssldump ${SOURCES})
target_link_libraries(ssldump ${OPENSSL_LIBRARIES})
