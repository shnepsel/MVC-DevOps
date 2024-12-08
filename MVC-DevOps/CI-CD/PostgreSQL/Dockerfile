FROM postgres:latest

# Устанавливаем локаль и переменные окружения для базы данных
ENV POSTGRES_DB=arhiv
ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=111

# Копируем файл с SQL-скриптами для инициализации
COPY tempDB.txt /docker-entrypoint-initdb.d/tempDB.sql

# Устанавливаем права на файл
RUN chmod 644 /docker-entrypoint-initdb.d/tempDB.sql

# Порт, на котором работает PostgreSQL
EXPOSE 5432
