# Base image
FROM ubuntu:22.04

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    libpq-dev \
    libboost-all-dev \
    libasio-dev \
    && rm -rf /var/lib/apt/lists/*

# Install Crow from source
WORKDIR /crow
RUN git clone https://github.com/CrowCpp/Crow.git . \
    && mkdir build && cd build \
    && cmake .. -DCROW_BUILD_EXAMPLES=OFF -DCROW_BUILD_TESTS=OFF \
    && make install

# Set working directory for your project
WORKDIR /app

# Copy source files and configuration
COPY main.cpp CMakeLists.txt config.json ./

# Build your project
RUN mkdir build && cd build \
    && cmake .. \
    && make

# Expose the application port
EXPOSE 8080

# Run the application
CMD ["./build/api_server"]
