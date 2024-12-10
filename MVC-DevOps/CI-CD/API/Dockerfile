FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    libpq-dev \
    libboost-all-dev \
    libasio-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /crow
RUN git clone https://github.com/CrowCpp/Crow.git . \
    && mkdir build && cd build \
    && cmake .. -DCROW_BUILD_EXAMPLES=OFF -DCROW_BUILD_TESTS=OFF \
    && make install

WORKDIR /app

COPY main.cpp CMakeLists.txt config.json ./

RUN mkdir build && cd build \
    && cmake .. \
    && make

EXPOSE 8080

CMD ["./build/api_server"]
