cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Настройка пути к vcpkg
set(CMAKE_TOOLCHAIN_FILE "${CMAKE_CURRENT_LIST_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake" CACHE STRING "Toolchain file for vcpkg")

# Установка статических библиотек и статического рантайма
set(Boost_USE_STATIC_LIBS ON)       # Используем статические библиотеки Boost
set(Boost_USE_STATIC_RUNTIME ON)    # Включаем статический рантайм

# Найти и подключить Boost
find_package(Boost 1.86 REQUIRED COMPONENTS system filesystem thread)

if (Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
else()
    message(FATAL_ERROR "Boost not found")
endif()

# Настройка для libpqxx
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIBPQ REQUIRED libpq)
include_directories(${LIBPQ_INCLUDE_DIRS})
link_directories(${LIBPQ_LIBRARY_DIRS})

# Найти и подключить libpqxx
find_package(libpqxx REQUIRED)
include_directories(${libpqxx_INCLUDE_DIRS})

# Подключение nlohmann-json
find_package(nlohmann_json REQUIRED)

# Подключение Asio
find_package(asio REQUIRED)

# Подключение Crow (если доступно через vcpkg)
find_package(Crow REQUIRED)

# Установка директорий для библиотек
include_directories(
    ${Boost_INCLUDE_DIRS}
    ${LIBPQ_INCLUDE_DIRS}
    ${libpqxx_INCLUDE_DIRS}
    ${asio_INCLUDE_DIRS}
    ${Crow_INCLUDE_DIRS}
    ${nlohmann_json_INCLUDE_DIRS}
)

# Укажите исполняемый файл
add_executable(pg main.cpp)

# Линковка статических библиотек
target_link_libraries(pg
    Boost::system
    Boost::filesystem
    Boost::thread
    ${LIBPQ_LIBRARIES}
    pqxx
    asio
    Crow::Crow
    nlohmann_json::nlohmann_json
    wsock32
    ws2_32
)
