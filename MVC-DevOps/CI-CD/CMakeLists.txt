cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Указываем стандарт C++
set(CMAKE_CXX_STANDARD 17)

# Настройка путей для vcpkg
if(DEFINED ENV{CMAKE_TOOLCHAIN_FILE})
    set(CMAKE_TOOLCHAIN_FILE $ENV{CMAKE_TOOLCHAIN_FILE} CACHE STRING "")
endif()

# Установка исходников
set(SOURCE_FILES main.cpp)

# Добавление исполняемого файла
add_executable(pg ${SOURCE_FILES})

# Поиск libpqxx через vcpkg
find_path(PQXX_INCLUDE_DIR pqxx/pqxx REQUIRED PATH_SUFFIXES include)
find_library(PQXX_LIBRARY NAMES pqxx REQUIRED)

if(PQXX_INCLUDE_DIR AND PQXX_LIBRARY)
    message(STATUS "Found libpqxx")
    include_directories(${PQXX_INCLUDE_DIR})
    target_link_libraries(pg PRIVATE ${PQXX_LIBRARY})
else()
    message(FATAL_ERROR "libpqxx not found")
endif()

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

# Подключение nlohmann-json через vcpkg
find_package(nlohmann_json REQUIRED)
target_link_libraries(pg PRIVATE nlohmann_json::nlohmann_json)

# Указание Asio
find_path(ASIO_INCLUDE_DIR asio.hpp REQUIRED PATH_SUFFIXES include)
include_directories(${ASIO_INCLUDE_DIR})

# Указание Crow
find_path(CROW_INCLUDE_DIR crow.h REQUIRED PATH_SUFFIXES include)
include_directories(${CROW_INCLUDE_DIR})
