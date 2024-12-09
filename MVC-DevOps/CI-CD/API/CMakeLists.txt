cmake_minimum_required(VERSION 3.10)

project(ApiServer)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# External libraries
include(FetchContent)

# Fetch Crow library
FetchContent_Declare(
    crow
    GIT_REPOSITORY https://github.com/CrowCpp/Crow.git
    GIT_TAG v1.0+2  # Укажите актуальную версию из репозитория
)
FetchContent_MakeAvailable(crow)

# Fetch nlohmann/json
FetchContent_Declare(
    nlohmann_json
    GIT_REPOSITORY https://github.com/nlohmann/json.git
    GIT_TAG v3.11.2  # Укажите актуальную версию из репозитория
)
FetchContent_MakeAvailable(nlohmann_json)

# Fetch libpqxx
FetchContent_Declare(
    libpqxx
    GIT_REPOSITORY https://github.com/jtv/libpqxx.git
    GIT_TAG 7.7.5  # Укажите актуальную версию из репозитория
)
FetchContent_MakeAvailable(libpqxx)

# Add executable
add_executable(api_server main.cpp)

# Include directories
target_include_directories(api_server PRIVATE ${libpqxx_INCLUDE_DIRS})

# Link libraries
target_link_libraries(api_server 
    crow
    nlohmann_json::nlohmann_json
    pqxx
    pthread
)
