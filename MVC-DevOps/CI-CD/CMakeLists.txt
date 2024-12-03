cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Устанавливаем стандарт C++
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Указываем исходный файл
set(SOURCE_FILES main.cpp)

# Добавляем исполняемый файл
add_executable(pg ${SOURCE_FILES})

# Установка путей для поиска библиотек
find_package(Boost 1.65 REQUIRED COMPONENTS system filesystem thread)  # Укажите используемые компоненты Boost
find_package(nlohmann_json REQUIRED)                                   # nlohmann-json
find_package(PkgConfig REQUIRED)                                       # PkgConfig для libpqxx
pkg_check_modules(PQXX REQUIRED libpqxx)                               # libpqxx

# Добавляем include-директории
target_include_directories(pg PRIVATE 
    ${Boost_INCLUDE_DIRS}
    ${PQXX_INCLUDE_DIRS}
    ${nlohmann_json_INCLUDE_DIRS}
)

# Линковка с библиотеками
target_link_libraries(pg PRIVATE 
    ${Boost_LIBRARIES}
    ${PQXX_LIBRARIES}
    nlohmann_json::nlohmann_json
)

# Установка флагов компиляции
if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_compile_options(pg PRIVATE -Wall -Wextra -g)
else()
    target_compile_options(pg PRIVATE -O3)
endif()
