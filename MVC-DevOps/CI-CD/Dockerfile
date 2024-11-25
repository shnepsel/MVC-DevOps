# Этап 1: Сборка зависимостей
FROM ubuntu:20.04 AS builder

ENV DEBIAN_FRONTEND=noninteractive

# Установка базовых инструментов и библиотек
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

# Установка необходимых зависимостей
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        libpq-dev \
        libjsoncpp-dev \
        libasio-dev \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Копирование библиотек и заголовков из этапа сборки
COPY --from=builder /usr/local /usr/local
COPY --from=builder /usr/include/crow/crow_all.h /usr/include/crow/crow_all.h

# Рабочая директория
WORKDIR /app

# Копирование файлов проекта
COPY . .

# Установка переменных окружения для CMake
ENV PQXX_INCLUDE_DIR=/usr/local/include
ENV PQXX_LIBRARY_DIR=/usr/local/lib
ENV PQ_LIBRARY_DIR=/usr/lib
ENV BOOST_ROOT=/usr/local
ENV BOOST_INCLUDE_DIR=/usr/local/include
ENV NLOHMANN_JSON_DIR=/usr/include
ENV ASIO_INCLUDE_DIR=/usr/include
ENV CROW_INCLUDE_DIR=/usr/include/crow

# Сборка проекта
RUN apt-get update && \
    apt-get install -y --no-install-recommends cmake build-essential && \
    rm -rf /var/lib/apt/lists/* && \
    cmake -DCMAKE_BUILD_TYPE=Release . && make -j$(nproc)

# Указание команды запуска
CMD ["./pg"]
