FROM ubuntu:22.04

# Установка базовых инструментов
RUN apt-get update && apt-get install -y \
    build-essential cmake git curl wget unzip pkg-config \
    libssl-dev && \
    rm -rf /var/lib/apt/lists/*

# Установка vcpkg
WORKDIR /opt
RUN git clone https://github.com/microsoft/vcpkg.git && \
    cd vcpkg && \
    ./bootstrap-vcpkg.sh && \
    ./vcpkg integrate install

# Установка зависимостей через vcpkg
RUN /opt/vcpkg/vcpkg install crow libpqxx nlohmann-json

# Устанавливаем переменные окружения для использования vcpkg в CMake
ENV VCPKG_ROOT=/opt/vcpkg
ENV CMAKE_TOOLCHAIN_FILE=/opt/vcpkg/scripts/buildsystems/vcpkg.cmake

# Копируем проект в контейнер
WORKDIR /app
COPY . .

# Сборка проекта
RUN mkdir build && cd build && \
    cmake .. -DCMAKE_TOOLCHAIN_FILE=/opt/vcpkg/scripts/buildsystems/vcpkg.cmake && \
    cmake --build .

# Указываем порт для работы приложения
EXPOSE 8080

# Запуск приложения
CMD ["./build/pg"]
