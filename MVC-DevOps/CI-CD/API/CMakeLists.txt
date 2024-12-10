cmake_minimum_required(VERSION 3.10)

project(ApiServer)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find external libraries
find_package(Crow REQUIRED)
find_package(PostgreSQL REQUIRED)
find_package(nlohmann_json REQUIRED)
find_package(Boost REQUIRED COMPONENTS system)

# Add executable
add_executable(api_server main.cpp)

# Link libraries
target_link_libraries(api_server 
    PUBLIC Crow::Crow 
    PUBLIC nlohmann_json::nlohmann_json
    PUBLIC pqxx
    PUBLIC Boost::system
    pthread
)
