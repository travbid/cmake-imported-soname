cmake_minimum_required(VERSION 3.24)

project(myproj)

add_library(my_imported_lib SHARED IMPORTED)

if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    set(MY_DLL_PATH "${CMAKE_SOURCE_DIR}/add.dll")
else()
    set(MY_DLL_PATH "${CMAKE_SOURCE_DIR}/libadd.so")
endif()
set_target_properties(my_imported_lib PROPERTIES
    IMPORTED_LOCATION "${MY_DLL_PATH}"
    IMPORTED_SONAME "libadd.so"
    IMPORTED_IMPLIB "${CMAKE_SOURCE_DIR}/add.lib"
)

add_executable(main main.cpp)

target_link_libraries(main PRIVATE my_imported_lib)

install(TARGETS main
    RUNTIME DESTINATION runtime
        COMPONENT main
    LIBRARY DESTINATION library
        COMPONENT main
)

install(IMPORTED_RUNTIME_ARTIFACTS my_imported_lib
    RUNTIME DESTINATION runtime
        COMPONENT main
    LIBRARY DESTINATION library
        COMPONENT main
)
