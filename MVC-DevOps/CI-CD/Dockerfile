# Этап 1: Сборка
FROM ubuntu:22.04 AS build

# Установим необходимые пакеты
RUN apt-get update && apt-get install -y \
    cmake \
    build-essential \
    libpqxx-dev \
    libssl-dev \
    git \
    wget \
    libasio-dev && \
    apt-get clean

# Установим Boost 1.86
RUN mkdir -p /tmp/boost && \
    cd /tmp/boost && \
    wget https://boostorg.jfrog.io/artifactory/main/release/1.86.0/source/boost_1_86_0.tar.gz && \
    tar -xvzf boost_1_86_0.tar.gz && \
    cd boost_1_86_0 && \
    ./bootstrap.sh --with-libraries=system,filesystem,thread --prefix=/usr/local && \
    ./b2 install && \
    cd / && rm -rf /tmp/boost

# Установим Crow
RUN mkdir -p /tmp/crow && \
    cd /tmp/crow && \
    git clone https://github.com/CrowCpp/Crow.git && \
    cd Crow && \
    cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DCROW_BUILD_EXAMPLES=OFF . && \
    make install && \
    cd / && rm -rf /tmp/crow

# Скопируем исходный код
WORKDIR /app
COPY . .

# Соберем проект
RUN cmake -DBoost_USE_STATIC_LIBS=ON -DBoost_USE_STATIC_RUNTIME=ON . && make

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
