cmake_minimum_required(VERSION 3.16)
project(pg)

set(CMAKE_CXX_STANDARD 17)

# Пути для vcpkg
set(CMAKE_TOOLCHAIN_FILE "/root/vcpkg/scripts/buildsystems/vcpkg.cmake")

# Добавляем зависимости
find_package(asio CONFIG REQUIRED)
find_package(Boost REQUIRED COMPONENTS system filesystem thread)
find_package(libpqxx CONFIG REQUIRED)

# Подключаем директории
include_directories(${Boost_INCLUDE_DIRS})
include_directories(${CMAKE_SOURCE_DIR}/include)

# Главный файл проекта
add_executable(${PROJECT_NAME} src/main.cpp)

# Линковка библиотек
target_link_libraries(${PROJECT_NAME}
    PRIVATE
        Boost::system
        Boost::filesystem
        Boost::thread
        asio
        pqxx
        ${CMAKE_SOURCE_DIR}/lib/libcrow.a
)
