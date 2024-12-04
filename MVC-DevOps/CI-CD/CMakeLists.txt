cmake_minimum_required(VERSION 3.16)
project(MyProject)

# Укажите путь к vcpkg
set(CMAKE_TOOLCHAIN_FILE "/root/vcpkg/scripts/buildsystems/vcpkg.cmake" CACHE STRING "Vcpkg toolchain file")

# Требуемая версия Boost
set(Boost_USE_STATIC_LIBS ON) # Для статической линковки
set(Boost_USE_MULTITHREADED ON)
set(Boost_USE_STATIC_RUNTIME ON)
find_package(Boost 1.86 REQUIRED COMPONENTS system filesystem thread)

if (Boost_FOUND)
    message(STATUS "Boost found: ${Boost_VERSION}")
    include_directories(${Boost_INCLUDE_DIRS})
    link_directories(${Boost_LIBRARY_DIRS})
else()
    message(FATAL_ERROR "Boost not found")
endif()

# Укажите другие зависимости
find_package(nlohmann_json REQUIRED)
find_package(asio REQUIRED)
find_package(CROW REQUIRED)
find_package(PQXX REQUIRED)

# Укажите исполняемый файл
add_executable(pg main.cpp)

# Подключите библиотеки
target_link_libraries(pg PRIVATE Boost::system Boost::filesystem Boost::thread pqxx nlohmann_json asio crow)
