# Этап 1: Сборка
FROM ubuntu:22.04 AS build

# Установим необходимые пакеты
RUN apt-get update && apt-get install -y \
    cmake \
    build-essential \
    libpqxx-dev \
    libssl-dev \
    git \
    wget && \
    apt-get clean

# Установим Boost 1.86
RUN mkdir -p /tmp/boost && \
    cd /tmp/boost && \
    wget https://boostorg.jfrog.io/artifactory/main/release/1.86.0/source/boost_1_86_0.tar.gz && \
    tar -xvzf boost_1_86_0.tar.gz && \
    cd boost_1_86_0 && \
    ./bootstrap.sh && ./b2 install && \
    cd / && rm -rf /tmp/boost

# Установим nlohmann_json и asio
RUN mkdir -p /tmp && \
    cd /tmp && \
    git clone https://github.com/nlohmann/json.git && \
    cd json && cmake . && make install && \
    cd /tmp && rm -rf json && \
    wget https://github.com/chriskohlhoff/asio/archive/refs/tags/asio-1-30-2.tar.gz && \
    tar -xvf asio-1-30-2.tar.gz && \
    cp -r asio-asio-1-30-2/asio/include/asio /usr/local/include

# Скопируем исходный код
WORKDIR /app
COPY . .

# Соберем проект
RUN cmake . && make

# Этап 2: Развертывание
FROM ubuntu:22.04

# Установим runtime зависимости
RUN apt-get update && apt-get install -y \
    libpqxx-dev \
    libssl-dev && \
    apt-get clean

# Копируем скомпилированное приложение
COPY --from=build /app/pg /usr/local/bin/pg

# Указываем рабочую директорию
WORKDIR /usr/local/bin

# Запускаем приложение
CMD ["./pg"]
