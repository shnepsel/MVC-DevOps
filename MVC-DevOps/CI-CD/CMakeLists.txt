cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Включаем все необходимые библиотеки
find_package(PostgreSQL REQUIRED)
find_package(Boost REQUIRED COMPONENTS system filesystem thread)
find_package(nlohmann_json REQUIRED)

# Добавляем необходимые заголовочные файлы
include_directories(
    ${PostgreSQL_INCLUDE_DIRS}
    ${Boost_INCLUDE_DIRS}
    ${nlohmann_json_INCLUDE_DIRS}
)

# Добавляем директории с библиотеками
link_directories(
    ${PostgreSQL_LIBRARY_DIRS}
    ${Boost_LIBRARY_DIRS}
)

# Настраиваем флаги компиляции для повышения совместимости
if(MSVC)
    add_definitions(-D_WIN32_WINNT=0x0601) # Для Windows 7 и выше
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /std:c++17 /W4")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -Wall -Wextra")
endif()

# Добавляем исполняемый файл
add_executable(pg main.cpp)

# Линкуем библиотеки
target_link_libraries(
    pg
    pqxx
    pq
    ${Boost_LIBRARIES}
    nlohmann_json::nlohmann_json
)
