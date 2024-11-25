cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Пути к библиотекам
set(PQXX_INCLUDE_DIR "$ENV{PQXX_INCLUDE_DIR}" CACHE PATH "Path to libpqxx include directory")
set(PQXX_LIBRARY_DIR "$ENV{PQXX_LIBRARY_DIR}" CACHE PATH "Path to libpqxx library directory")
set(BOOST_ROOT "$ENV{BOOST_ROOT}" CACHE PATH "Path to Boost root directory")
set(BOOST_INCLUDE_DIR "$ENV{BOOST_INCLUDE_DIR}" CACHE PATH "Path to Boost include directory")
set(nlohmann_json_DIR "$ENV{NLOHMANN_JSON_DIR}" CACHE PATH "Path to nlohmann_json library directory")
set(CROW_INCLUDE_DIR "$ENV{CROW_INCLUDE_DIR}" CACHE PATH "Path to Crow library include directory")

# Уведомление о путях
message("\n\nInclude dirs: " ${PQXX_INCLUDE_DIR})
message("Library dirs: " ${PQXX_LIBRARY_DIR} "\n")

# Подключение каталогов с библиотеками
include_directories(${PQXX_INCLUDE_DIR} ${BOOST_INCLUDE_DIR} ${nlohmann_json_INCLUDE_DIRS} ${CROW_INCLUDE_DIR})

# Ищем библиотеки Boost
find_package(Boost REQUIRED COMPONENTS system filesystem program_options)

# Если Boost найден, используем его
if (Boost_FOUND)
    include_directories(${Boost_INCLUDE_DIRS})
    link_directories(${Boost_LIBRARY_DIRS})
endif()

# Подключение nlohmann_json
find_package(nlohmann_json REQUIRED)
include_directories(${nlohmann_json_INCLUDE_DIRS})

# Настройка Crow
set(CROW_USE_BOOST OFF)
include_directories(${CROW_INCLUDE_DIR})

# Добавление исходных файлов
# Убедитесь, что файл main.cpp существует в проекте и находится в правильной директории
add_executable(pg main.cpp)

# Подключение всех необходимых библиотек
target_link_libraries(pg pqxx pq ${Boost_LIBRARIES} ${CROW_LIBRARIES} wsock32 ws2_32)
