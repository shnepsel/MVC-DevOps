# Используем официальный образ для Ubuntu 20.04
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# Устанавливаем необходимые пакеты для сборки
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    curl \
    wget \
    pkg-config \
    libssl-dev \
    libpq-dev \
    libboost-all-dev

# Устанавливаем vcpkg
WORKDIR /root
RUN git clone https://github.com/microsoft/vcpkg.git
WORKDIR /root/vcpkg
RUN ./bootstrap-vcpkg.sh

# Устанавливаем библиотеки через vcpkg
RUN ./vcpkg install asio crow libpqxx nlohmann-json

# Создаем директорию для проекта
WORKDIR /src
COPY . /src

# Указываем пути установки библиотек из vcpkg
ENV VCPKG_ROOT=/root/vcpkg
ENV CMAKE_TOOLCHAIN_FILE=/root/vcpkg/scripts/buildsystems/vcpkg.cmake

# Запуск конфигурации и сборки с использованием CMake
WORKDIR /src/build
RUN cmake -DCMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE} ..
RUN make

# Запуск приложения
CMD ["./pg"]
