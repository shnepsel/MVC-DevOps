# Базовый образ
FROM ubuntu:22.04

# Установка необходимых инструментов
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    libboost-all-dev \
    libpqxx-dev \
    libnlohmann-json-dev \
    pkg-config \
    git \
    && apt-get clean

# Установка рабочей директории
WORKDIR /app

# Копируем проект в контейнер
COPY . .

# Сборка проекта
RUN mkdir build && cd build && cmake .. && make

# Указываем порт
EXPOSE 8080

# Запуск приложения
CMD ["./build/pg"]
