cmake_minimum_required(VERSION 3.16)
project(pg)

set(CMAKE_CXX_STANDARD 17)

# Указываем путь к toolchain vcpkg
set(CMAKE_TOOLCHAIN_FILE "/root/vcpkg/scripts/buildsystems/vcpkg.cmake" CACHE STRING "Vcpkg toolchain file" FORCE)

# Ищем зависимости через vcpkg
find_package(asio CONFIG REQUIRED PATHS "/root/vcpkg/installed/x64-linux/share/asio")
find_package(Boost REQUIRED COMPONENTS system filesystem thread)
find_package(libpqxx CONFIG REQUIRED)

# Добавляем исполнимые файлы и директории
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
