# Этап 1: Сборка зависимостей
FROM ubuntu:20.04 AS builder

ENV DEBIAN_FRONTEND=noninteractive

# Установка необходимых инструментов
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        cmake \
        build-essential \
        libpq-dev \
        postgresql-server-dev-all \
        libjsoncpp-dev \
        libasio-dev \
        wget \
        curl \
        unzip \
        python3-dev \
        libicu-dev \
        libbz2-dev \
        liblzma-dev \
        libssl-dev \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Установка Boost
RUN wget https://boostorg.jfrog.io/artifactory/main/release/1.86.0/source/boost_1_86_0.tar.gz && \
    tar -xzf boost_1_86_0.tar.gz && \
    cd boost_1_86_0 && \
    ./bootstrap.sh --prefix=/usr/local && \
    ./b2 -j$(nproc) install && \
    cd .. && \
    rm -rf boost_1_86_0 boost_1_86_0.tar.gz && \
    ldconfig

# Установка libpqxx
RUN wget https://github.com/jtv/libpqxx/archive/refs/tags/7.6.0.tar.gz && \
    tar -xzf 7.6.0.tar.gz && \
    cd libpqxx-7.6.0 && \
    mkdir build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release && \
    make -j$(nproc) && \
    make install && \
    cd ../../ && \
    rm -rf libpqxx-7.6.0 7.6.0.tar.gz && \
    ldconfig

# Установка Crow
RUN mkdir -p /usr/include/crow && \
    wget https://github.com/CrowCpp/Crow/releases/download/v1.0+4/crow_all.h -O /usr/include/crow/crow_all.h

# Этап 2: Финальный образ
FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

# Установка необходимых библиотек
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        libpq-dev \
        libjsoncpp-dev \
        libasio-dev \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Копируем зависимости из этапа сборки
COPY --from=builder /usr/local /usr/local
COPY --from=builder /usr/include/crow/crow_all.h /usr/include/crow/crow_all.h

# Определяем рабочую директорию
WORKDIR /app

# Копируем файлы проекта
COPY . .

# Устанавливаем переменные окружения для CMake
ENV BOOST_ROOT=/usr/local
ENV BOOST_LIBRARYDIR=/usr/local/lib
ENV BOOST_INCLUDEDIR=/usr/local/include
ENV PQXX_INCLUDE_DIR=/usr/local/include
ENV PQXX_LIBRARY_DIR=/usr/local/lib
ENV CROW_INCLUDE_DIR=/usr/include/crow

# Сборка проекта
RUN apt-get update && \
    apt-get install -y --no-install-recommends cmake build-essential && \
    rm -rf /var/lib/apt/lists/* && \
    cmake -DCMAKE_VERBOSE_MAKEFILE:BOOL=ON -DCMAKE_PREFIX_PATH=/usr/local . && \
    make -j$(nproc)

# Указываем команду запуска
CMD ["./pg"]
