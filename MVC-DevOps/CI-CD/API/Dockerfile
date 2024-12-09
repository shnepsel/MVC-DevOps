# Base image
FROM ubuntu:22.04

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /app

# Copy source files and configuration
COPY main.cpp CMakeLists.txt config.json ./

# Build and install dependencies
RUN mkdir build && cd build \
    && cmake .. \
    && make

# Expose the application port
EXPOSE 8080

# Run the application
CMD ["./build/api_server"]
