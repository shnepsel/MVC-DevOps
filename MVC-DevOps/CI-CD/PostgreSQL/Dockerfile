FROM postgres:latest

ENV POSTGRES_DB=arhiv
ENV POSTGRES_USER=postgres
ENV POSTGRES_PASSWORD=111

COPY tempDB.txt /docker-entrypoint-initdb.d/tempDB.sql

RUN chmod 644 /docker-entrypoint-initdb.d/tempDB.sql

EXPOSE 5432
