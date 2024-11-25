cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Пути к библиотекам
set(PQXX_INCLUDE_DIR "$ENV{PQXX_INCLUDE_DIR}" CACHE PATH "Path to libpqxx include directory")
set(PQXX_LIBRARY_DIR "$ENV{PQXX_LIBRARY_DIR}" CACHE PATH "Path to libpqxx library directory")
set(PQ_LIBRARY_DIR "$ENV{PQ_LIBRARY_DIR}" CACHE PATH "Path to PostgreSQL library directory")
set(BOOST_ROOT "$ENV{BOOST_ROOT}" CACHE PATH "Path to Boost root directory")
set(BOOST_INCLUDE_DIR "$ENV{BOOST_INCLUDE_DIR}" CACHE PATH "Path to Boost include directory")
set(nlohmann_json_DIR "$ENV{NLOHMANN_JSON_DIR}" CACHE PATH "Path to nlohmann_json library directory")
set(ASIO_INCLUDE_DIR "$ENV{ASIO_INCLUDE_DIR}" CACHE PATH "Path to Asio library include directory")
set(CROW_INCLUDE_DIR "$ENV{CROW_INCLUDE_DIR}" CACHE PATH "Path to Crow library include directory")
set(CROW_DIR "$ENV{CROW_DIR}" CACHE PATH "Path to Crow library config directory")

# Уведомление о путях
message("\n\nInclude dirs: " ${PQXX_INCLUDE_DIR})
message("Library dirs: " ${PQXX_LIBRARY_DIR} "\n")

# Подключение каталогов с библиотеками
include_directories(${PQXX_INCLUDE_DIR} ${BOOST_INCLUDE_DIR} ${nlohmann_json_INCLUDE_DIRS} ${ASIO_INCLUDE_DIR} ${CROW_INCLUDE_DIR})
link_directories(${PQXX_LIBRARY_DIR} ${PQ_LIBRARY_DIR})

# Добавьте исполняемый файл
add_executable(pg main.cpp)

# Подключение Boost
set (Boost_USE_STATIC_RUNTIME ON)
find_package(Boost 1.86 REQUIRED COMPONENTS system filesystem thread)

if (Boost_FOUND)
    include_directories(SYSTEM ${Boost_INCLUDE_DIR})
    target_link_libraries(pg ${Boost_LIBRARIES})
else()
    message("Boost not found")
endif()

# Подключение nlohmann_json
find_package(nlohmann_json REQUIRED)
include_directories(${nlohmann_json_INCLUDE_DIRS})

# Настройка Crow
set(CROW_USE_BOOST OFF)
message(STATUS ${CROW_USE_BOOST})
include_directories(${CROW_INCLUDE_DIR})

# Подключение всех необходимых библиотек
target_link_libraries(pg pqxx pq ${Boost_LIBRARIES} ${CROW_LIBRARIES} wsock32 ws2_32)
