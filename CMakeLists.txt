cmake_minimum_required(VERSION 3.2)
project(binapi)

set(CMAKE_CXX_STANDARD 14)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wl,-fsanitize=address")

add_definitions(
    -UNDEBUG
    -DDTF_HEADER_ONLY
    -D__FLATJSON__CHILDS_TYPE=std::uint32_t
    -D__FLATJSON__VLEN_TYPE=std::uint32_t
)

include_directories(
    ./include
)

message(BINAPI_BOOST_DIR=${BINAPI_BOOST_DIR})

set(BINAPI_HEADERS
    binapi/api.hpp
    binapi/flatjson.hpp
    binapi/dtf.hpp
    binapi/double_type.hpp
    binapi/enums.hpp
    binapi/errors.hpp
    binapi/invoker.hpp
    binapi/message.hpp
    binapi/pairslist.hpp
    binapi/reports.hpp
    binapi/tools.hpp
    binapi/types.hpp
    binapi/websocket.hpp
)

set(BINAPI_SOURCES
    src/api.cpp
    src/enums.cpp
    src/errors.cpp
    src/pairslist.cpp
    src/reports.cpp
    src/tools.cpp
    src/types.cpp
    src/websocket.cpp
)

add_library(
    ${PROJECT_NAME} STATIC
    ${BINAPI_SOURCES}
)

# Boost
find_package(Boost 1.66 REQUIRED system filesystem thread locale)
if(Boost_FOUND)
    target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES})
    include_directories(${Boost_INCLUDE_DIRS})
endif()

target_link_libraries(
    ${PROJECT_NAME}
    z
    crypto
    ssl
    pthread
)
