cmake_minimum_required(VERSION 3.10)

project(ApiServer)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find required libraries
find_package(nlohmann_json REQUIRED)
find_package(PostgreSQL REQUIRED)

# Include directories
include_directories(${PostgreSQL_INCLUDE_DIRS})

# Source files
add_executable(api_server main.cpp)

# Link libraries
target_link_libraries(api_server 
    ${PostgreSQL_LIBRARIES} 
    nlohmann_json::nlohmann_json
    pthread
)
