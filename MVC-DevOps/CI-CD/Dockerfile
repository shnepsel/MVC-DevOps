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
    pkg-config \
    libssl-dev \
    libpq-dev

# Устанавливаем vcpkg
WORKDIR /root
RUN git clone https://github.com/microsoft/vcpkg.git
WORKDIR /root/vcpkg

# Выполняем bootstrap для vcpkg
RUN ./bootstrap-vcpkg.sh

# Устанавливаем библиотеки через vcpkg (статическая сборка)
RUN ./vcpkg install asio:x64-linux-static crow:x64-linux-static libpqxx:x64-linux-static nlohmann-json:x64-linux-static boost-system:x64-linux-static boost-filesystem:x64-linux-static boost-thread:x64-linux-static

# Создаем директорию для проекта
WORKDIR /src
COPY . /src

# Указываем пути установки библиотек из vcpkg
ENV VCPKG_ROOT=/root/vcpkg
ENV CMAKE_TOOLCHAIN_FILE=/root/vcpkg/scripts/buildsystems/vcpkg.cmake

# Запуск конфигурации и сборки с использованием CMake
WORKDIR /src/build
RUN cmake -DCMAKE_TOOLCHAIN_FILE=${CMAKE_TOOLCHAIN_FILE} -DBoost_USE_STATIC_LIBS=ON -DBoost_USE_STATIC_RUNTIME=ON ..
RUN make

# Запуск приложения
CMD ["./pg"]
