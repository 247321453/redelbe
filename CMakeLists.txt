cmake_minimum_required(VERSION 3.4.1)

project(dinput8)

set(CMAKE_CXX_STANDARD 11)
set(BUILD_USE_64BITS on)

set(C_FLAGS "-Wall -std=gnu99 -mms-bitfields -s -O2 -masm=intel -shared -Wl,--subsystem,windows,--kill-at,--enable-stdcall-fixup -static -static-libgcc -static-libstdc++")
set(CPP_FLAGS "-Wall -Wno-strict-aliasing -O2 -std=c++11 -mms-bitfields -DTIXML_USE_STL -DDEV_BUILD -static-libgcc -static-libstdc++ -static -Wl,--subsystem,windows,--kill-at")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${C_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${CPP_FLAGS}")

include_directories(. eternity_common minhook/include)

set(LIB_SOURCES
main.cpp
game.cpp
rdbemu.cpp
layer2.cpp
ui.cpp
misc.cpp
debug_enc.cpp
debug.cpp
eternity_common/crypto/sha1.c
eternity_common/crypto/md5.c
eternity_common/crypto/rijndael.c
eternity_common/Utils.cpp
eternity_common/BaseFile.cpp
eternity_common/Stream.cpp
eternity_common/MemoryStream.cpp
eternity_common/FixedMemoryStream.cpp
eternity_common/FileStream.cpp
eternity_common/IniFile.cpp
eternity_common/DOA6/RdbFile.cpp
eternity_common/DOA6/RnkFile.cpp
eternity_common/PatchUtils.cpp
eternity_common/EPatchFile.cpp
eternity_common/tinyxml/tinyxml.cpp
eternity_common/tinyxml/tinystr.cpp
eternity_common/tinyxml/tinyxmlerror.cpp
eternity_common/tinyxml/tinyxmlparser.cpp
)

add_subdirectory(minhook)
add_subdirectory(zlib)

add_library(dinput8 SHARED ${LIB_SOURCES})

set_target_properties(dinput8 PROPERTIES PREFIX "")
set_target_properties(dinput8 PROPERTIES COMPILE_FLAGS "-D_FILE_OFFSET_BITS=64")

target_link_libraries(dinput8 minhook zlibstatic version)

