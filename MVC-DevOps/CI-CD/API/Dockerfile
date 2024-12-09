# Base image
FROM ubuntu:22.04

# Install necessary packages
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    libpqxx-dev \
    libnlohmann-json-dev \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy source code and configuration
COPY main.cpp CMakeLists.txt config.json ./

# Build the project
RUN cmake . && make

# Expose the application port
EXPOSE 8080

# Run the application
CMD ["./api_server"]
