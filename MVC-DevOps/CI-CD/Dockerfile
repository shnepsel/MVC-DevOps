# Используем базовый образ с поддержкой CMake, компилятором и основными инструментами
FROM ubuntu:20.04

# Устанавливаем основные зависимости
RUN apt-get update && \
    apt-get install -y \
    cmake \
    build-essential \
    libpq-dev \
    postgresql-server-dev-all \
    libjsoncpp-dev \
    libasio-dev \
    wget \
    curl \
    unzip

# Установка и сборка только необходимых модулей Boost 1.86
RUN wget https://boostorg.jfrog.io/artifactory/main/release/1.86.0/source/boost_1_86_0.tar.gz && \
    tar xzf boost_1_86_0.tar.gz && \
    cd boost_1_86_0 && \
    ./bootstrap.sh --prefix=/usr/local --with-libraries=system,filesystem,thread && \
    ./b2 install -j$(nproc) --quiet && \
    cd .. && rm -rf boost_1_86_0 boost_1_86_0.tar.gz

# Установка и сборка libpqxx
RUN wget https://github.com/jtv/libpqxx/archive/refs/tags/7.6.0.tar.gz && \
    tar xzf 7.6.0.tar.gz && \
    cd libpqxx-7.6.0 && \
    cmake . && \
    make -j$(nproc) && \
    make install && \
    cd .. && rm -rf libpqxx-7.6.0 7.6.0.tar.gz

# Устанавливаем Crow
RUN wget https://github.com/CrowCpp/Crow/releases/download/v1.0+4/crow_all.h -P /usr/include/crow

# Определяем рабочую директорию
WORKDIR /app

# Копируем проект в контейнер
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
RUN cmake . && make

# Указываем команду запуска
CMD ["./pg"]
