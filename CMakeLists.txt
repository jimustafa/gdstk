cmake_minimum_required(VERSION 3.14)

file(READ include/gdstk/gdstk.hpp HEADER_CONTENTS)
string(REGEX MATCH "GDSTK_VERSION \"([^\"]*)\"" _ ${HEADER_CONTENTS})
set(GDSTK_VERSION ${CMAKE_MATCH_1})

project(gdstk
    VERSION ${GDSTK_VERSION}
    LANGUAGES CXX C)

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    set(CMAKE_CXX_EXTENSIONS OFF)
    set_property(GLOBAL PROPERTY USE_FOLDERS ON)

    if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /nologo /W3 /MD /EHsc /wd4996")
        set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /Zi")
        set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /Ox /GL -DNDEBUG")
    else()
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wshadow -Wvla -Wformat -Wno-missing-field-initializers -Wno-missing-braces")
        set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -g")
        set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3 -DNDEBUG")
    endif()

    include(CTest)
    add_subdirectory(docs/cpp)
endif()

add_subdirectory(external)
add_subdirectory(src)

include(GNUInstallDirs)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/gdstk.pc DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig)
CONFIGURE_FILE("gdstk.pc.in" "gdstk.pc")
