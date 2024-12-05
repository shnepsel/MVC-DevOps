# Используем официальный образ для Ubuntu 20.04
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# Устанавливаем необходимые пакеты для сборки
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    curl \
    zip \
    unzip \
    tar \
    bison \
    flex \
    autoconf \
    pkg-config \
    libssl-dev \
    libpq-dev

# Устанавливаем vcpkg
WORKDIR /root
RUN git clone https://github.com/microsoft/vcpkg.git
WORKDIR /root/vcpkg

# Выполняем bootstrap для vcpkg
RUN ./bootstrap-vcpkg.sh

# Устанавливаем библиотеки через vcpkg (динамическая версия)
RUN ./vcpkg install asio crow libpqxx nlohmann-json boost-system boost-filesystem boost-thread

# Создаем директорию для проекта
WORKDIR /src
COPY . /src

# Указываем пути установки библиотек из vcpkg
ENV VCPKG_ROOT=/root/vcpkg
ENV CMAKE_TOOLCHAIN_FILE=/root/vcpkg/scripts/buildsystems/vcpkg.cmake
ENV CMAKE_PREFIX_PATH=/root/vcpkg/installed/x64-linux

# Запуск конфигурации и сборки с использованием CMake
WORKDIR /src/build
RUN cmake -DCMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE} -DCMAKE_PREFIX_PATH=${CMAKE_PREFIX_PATH} -DBoost_USE_STATIC_LIBS=OFF -DBoost_USE_STATIC_RUNTIME=OFF ..
RUN make

# Запуск приложения
CMD ["./pg"]
