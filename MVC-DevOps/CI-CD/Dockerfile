FROM ubuntu:22.04

# Установка необходимых пакетов
RUN apt-get update && apt-get install -y \
    cmake g++ libpqxx-dev libpq-dev libboost-all-dev \
    wget git && \
    rm -rf /var/lib/apt/lists/*

# Установка CMake
RUN wget https://github.com/Kitware/CMake/releases/download/v3.27.2/cmake-3.27.2-linux-x86_64.tar.gz && \
    tar -xvzf cmake-3.27.2-linux-x86_64.tar.gz --strip 1 -C /usr/local

# Копируем проект
WORKDIR /app
COPY . .

# Сборка проекта
RUN mkdir build && cd build && \
    cmake .. && \
    cmake --build .

# Указываем порт
EXPOSE 8080

# Запуск приложения
CMD ["./build/pg"]
