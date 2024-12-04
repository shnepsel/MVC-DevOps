cmake_minimum_required(VERSION 3.16)
project(pg)

set(CMAKE_CXX_STANDARD 17)

# Подключаем CMake toolchain для vcpkg
set(CMAKE_TOOLCHAIN_FILE "/root/vcpkg/scripts/buildsystems/vcpkg.cmake")

# Поиск библиотек через vcpkg
find_package(asio CONFIG REQUIRED)
find_package(Boost REQUIRED COMPONENTS system filesystem thread)
find_package(libpqxx CONFIG REQUIRED)
find_package(OpenSSL REQUIRED)

# Указываем исходные файлы
add_executable(${PROJECT_NAME} src/main.cpp)

# Включаем необходимые директории
target_include_directories(${PROJECT_NAME} PRIVATE 
    ${CMAKE_SOURCE_DIR}/include
)

# Линкуем библиотеки
target_link_libraries(${PROJECT_NAME} PRIVATE
    Boost::system
    Boost::filesystem
    Boost::thread
    pqxx
    OpenSSL::SSL
    OpenSSL::Crypto
)
