cmake_minimum_required (VERSION 2.8)
project(luaset)

if(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    set(CMAKE_SHARED_LINKER_FLAGS "-undefined dynamic_lookup")
endif()

if(WIN32)
	set(suffix ".dll")
    add_definitions(-DLUA_BUILD_AS_DLL -DLUA_LIB -DHTTP_PARSER_STRICT=0)
else()
	set(suffix ".so")
endif()

macro (add_lua_library lname)
    add_library(${lname} SHARED ${ARGN})
    set_target_properties(${lname} PROPERTIES PREFIX "")
    set_target_properties(${lname} PROPERTIES SUFFIX ${suffix})
endmacro()


include_directories($ENV{HOME}/local/lua53/include)

# levent.module
set(CSOURCE lua-skiplist.c skiplist.c)
add_lua_library(skiplist ${CSOURCE})
