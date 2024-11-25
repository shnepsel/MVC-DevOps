cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Пути к библиотекам
set(PQXX_INCLUDE_DIR "$ENV{PQXX_INCLUDE_DIR}" CACHE PATH "Path to libpqxx include directory")
set(PQXX_LIBRARY_DIR "$ENV{PQXX_LIBRARY_DIR}" CACHE PATH "Path to libpqxx library directory")
set(PQ_LIBRARY_DIR "$ENV{PQ_LIBRARY_DIR}" CACHE PATH "Path to PostgreSQL library directory")
set(BOOST_ROOT "$ENV{BOOST_ROOT}" CACHE PATH "Path to Boost root directory")
set(BOOST_INCLUDE_DIR "$ENV{BOOST_INCLUDE_DIR}" CACHE PATH "Path to Boost include directory")
set(NLOHMANN_JSON_DIR "$ENV{NLOHMANN_JSON_DIR}" CACHE PATH "Path to nlohmann_json library directory")
set(ASIO_INCLUDE_DIR "$ENV{ASIO_INCLUDE_DIR}" CACHE PATH "Path to Asio library include directory")
set(CROW_INCLUDE_DIR "$ENV{CROW_INCLUDE_DIR}" CACHE PATH "Path to Crow library include directory")

# Вывод информации о путях
message(STATUS "PQXX Include Directory: ${PQXX_INCLUDE_DIR}")
message(STATUS "PQXX Library Directory: ${PQXX_LIBRARY_DIR}")
message(STATUS "PostgreSQL Library Directory: ${PQ_LIBRARY_DIR}")
message(STATUS "Boost Include Directory: ${BOOST_INCLUDE_DIR}")
message(STATUS "Nlohmann JSON Directory: ${NLOHMANN_JSON_DIR}")
message(STATUS "Asio Include Directory: ${ASIO_INCLUDE_DIR}")
message(STATUS "Crow Include Directory: ${CROW_INCLUDE_DIR}")

# Подключение каталогов с библиотеками
include_directories(${PQXX_INCLUDE_DIR} ${BOOST_INCLUDE_DIR} ${NLOHMANN_JSON_DIR} ${ASIO_INCLUDE_DIR} ${CROW_INCLUDE_DIR})
link_directories(${PQXX_LIBRARY_DIR} ${PQ_LIBRARY_DIR})

# Добавление исполняемого файла
add_executable(pg main.cpp)

# Подключение Boost
set(Boost_USE_STATIC_LIBS ON)
find_package(Boost 1.86 REQUIRED COMPONENTS system filesystem thread)

if (Boost_FOUND)
    message(STATUS "Boost found: ${Boost_INCLUDE_DIRS}")
    include_directories(${Boost_INCLUDE_DIRS})
    target_link_libraries(pg ${Boost_LIBRARIES})
else()
    message(FATAL_ERROR "Boost not found or version is incorrect")
endif()

# Подключение nlohmann_json
find_package(nlohmann_json REQUIRED)
if (nlohmann_json_FOUND)
    message(STATUS "nlohmann_json found: ${nlohmann_json_DIR}")
    target_link_libraries(pg nlohmann_json::nlohmann_json)
else()
    message(FATAL_ERROR "nlohmann_json not found")
endif()

# Подключение Crow
include_directories(${CROW_INCLUDE_DIR})
message(STATUS "Crow include directory: ${CROW_INCLUDE_DIR}")

# Подключение libpqxx и других библиотек
target_link_libraries(pg pqxx pq ${Boost_LIBRARIES})
