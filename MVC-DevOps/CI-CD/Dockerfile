# Используем базовый образ Ubuntu 20.04
FROM ubuntu:20.04

# Устанавливаем переменную окружения для неинтерактивной установки
ENV DEBIAN_FRONTEND=noninteractive

# Обновляем пакеты и устанавливаем основные зависимости
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

# Установка Boost 1.86
RUN wget https://boostorg.jfrog.io/artifactory/main/release/1.86.0/source/boost_1_86_0.tar.gz && \
    tar -xzf boost_1_86_0.tar.gz && \
    cd boost_1_86_0 && \
    ./bootstrap.sh --prefix=/usr/local && \
    ./b2 -j$(nproc) install && \
    cd .. && \
    rm -rf boost_1_86_0 boost_1_86_0.tar.gz && \
    ldconfig

# Установка и сборка libpqxx
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

# Устанавливаем Crow
RUN mkdir -p /usr/include/crow && \
    wget https://github.com/CrowCpp/Crow/releases/download/v1.0+4/crow_all.h -O /usr/include/crow/crow_all.h

# Определяем рабочую директорию
WORKDIR /app

# Копируем файлы проекта в контейнер
COPY . .

# Устанавливаем переменные окружения для CMake
ENV PQXX_INCLUDE_DIR=/usr/local/include
ENV PQXX_LIBRARY_DIR=/usr/local/lib
ENV PQ_LIBRARY_DIR=/usr/lib
ENV BOOST_ROOT=/usr/local
ENV BOOST_INCLUDE_DIR=/usr/local/include
ENV NLOHMANN_JSON_DIR=/usr/include
ENV ASIO_INCLUDE_DIR=/usr/include
ENV CROW_INCLUDE_DIR=/usr/include/crow

# Сборка проекта
RUN cmake . && make -j$(nproc)

# Указываем команду запуска
CMD ["./pg"]
