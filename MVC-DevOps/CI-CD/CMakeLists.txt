cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Включаем C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Подключаем vcpkg
if(DEFINED ENV{VCPKG_ROOT})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake")
elseif(EXISTS "${CMAKE_SOURCE_DIR}/vcpkg.json")
    set(CMAKE_TOOLCHAIN_FILE "${CMAKE_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake")
else()
    message(FATAL_ERROR "vcpkg not found! Please define VCPKG_ROOT or clone vcpkg into the project.")
endif()

# Найти зависимости
find_package(Crow CONFIG REQUIRED)
find_package(libpqxx CONFIG REQUIRED)
find_package(nlohmann_json CONFIG REQUIRED)

# Добавляем исполняемый файл
add_executable(pg main.cpp)

# Линкуем библиотеки
target_link_libraries(pg PRIVATE 
    Crow::Crow
    pqxx
    nlohmann_json::nlohmann_json
)
