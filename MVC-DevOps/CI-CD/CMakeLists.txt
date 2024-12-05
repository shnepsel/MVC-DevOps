cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Настройка путей для vcpkg
set(CMAKE_TOOLCHAIN_FILE $ENV{CMAKE_TOOLCHAIN_FILE} CACHE STRING "Vcpkg toolchain file")

# Указываем стандарт C++
set(CMAKE_CXX_STANDARD 17)

# Установка путей к исходным файлам
set(SOURCE_FILES main.cpp)

# Добавление исполняемого файла
add_executable(pg ${SOURCE_FILES})

# Поиск и подключение libpqxx
find_package(PQXX REQUIRED)
include_directories(${PQXX_INCLUDE_DIRS})
target_link_libraries(pg PRIVATE ${PQXX_LIBRARIES})

# Поиск и подключение Boost
set(Boost_USE_STATIC_LIBS OFF)    # Динамические библиотеки
set(Boost_USE_STATIC_RUNTIME OFF) # Динамический рантайм
find_package(Boost REQUIRED COMPONENTS system filesystem thread)

if (Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
    target_link_libraries(pg PRIVATE ${Boost_LIBRARIES})
else()
    message(FATAL_ERROR "Boost not found")
endif()

# Поиск и подключение nlohmann-json
find_package(nlohmann_json REQUIRED)
target_link_libraries(pg PRIVATE nlohmann_json::nlohmann_json)

# Поиск и подключение asio
find_path(ASIO_INCLUDE_DIR asio.hpp REQUIRED)
include_directories(${ASIO_INCLUDE_DIR})

# Поиск и подключение Crow
find_path(CROW_INCLUDE_DIR crow.h REQUIRED)
include_directories(${CROW_INCLUDE_DIR})
