cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Пути для зависимостей
set(PQXX_INCLUDE_DIR "/usr/local/include")
set(PQXX_LIBRARY_DIR "/usr/local/lib")
set(PQ_LIBRARY_DIR "/usr/lib/postgresql")

# Подключаем директории с заголовочными файлами
include_directories(${PQXX_INCLUDE_DIR})

# Проверка наличия библиотеки libpq
#find_library(PQ_LIB NAMES pq PATHS ${PQ_LIBRARY_DIR} NO_DEFAULT_PATH)
find_library(PQ_LIB NAMES pq REQUIRED)
find_path(PQ_INCLUDE_DIR NAMES libpq-fe.h REQUIRED)

if(NOT PQ_LIB)
    message(FATAL_ERROR "libpq not found")
else()
    message("libpq found")
endif()

# Добавляем директории с библиотеками
link_directories(${PQXX_LIBRARY_DIR} ${PQ_LIBRARY_DIR})

# Указываем исполняемый файл
add_executable(pg main.cpp)

# Путь к Boost
set(BOOST_ROOT "/usr/local/boost")
set(BOOST_INCLUDE_DIR "/usr/local/include")
set(Boost_USE_STATIC_RUNTIME ON)

# Находим Boost
find_package(Boost REQUIRED COMPONENTS system filesystem thread)

if (Boost_FOUND)
    include_directories(SYSTEM ${Boost_INCLUDE_DIR}) # подключаем заголовочные файлы Boost
    target_link_libraries(pg Boost::Boost) # подключаем библиотеки Boost
else()
    message("Boost not found")
endif()

# Указываем пути для nlohmann_json
set(nlohmann_json_INCLUDE_DIRS "/usr/local/include/nlohmann")
find_package(nlohmann_json REQUIRED)

# Подключаем nlohmann_json
include_directories(${nlohmann_json_INCLUDE_DIRS})

# Указываем путь для ASIO
set(ASIO_INCLUDE_DIR "/usr/local/include/asio")
include_directories(${ASIO_INCLUDE_DIR})

# Указываем путь для Crow
set(CROW_INCLUDE_DIR "/usr/local/include/Crow")
set(CROW_DIR "/usr/local/lib/cmake/Crow")

# Включаем Crow
include_directories(${CROW_INCLUDE_DIR})
find_package(Crow REQUIRED)

# Подключаем библиотеки
target_link_libraries(pg pqxx libpq ${Boost_LIBRARIES} ${CROW_LIBRARIES} wsock32 ws2_32)
