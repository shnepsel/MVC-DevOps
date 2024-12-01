cmake_minimum_required(VERSION 3.10)
   project(MyProject)

    set(PQXX_INCLUDE_DIR "D:\\libpqxx\\INSTALL\\include")
    message("\n\nInclude dirs: " ${PQXX_INCLUDE_DIR})
    set(PQXX_LIBRARY_DIR "D:\\libpqxx\\INSTALL\\lib")
    message("Library dirs: " ${PQXX_LIBRARY_DIR} "\n")
    include_directories(${PQXX_INCLUDE_DIR})

    #message("Library dirs: " ${PQ_LIBRARY_DIR} "\n")
    # if(POLICY CMP0167)
    #      cmake_policy(SET CMP0167 NEW)
    # endif()


    # Найти библиотеку libpq
    #find_library(PQ_LIB NAMES pq PATHS "D:\\PostgreSQL\\17\\lib" NO_DEFAULT_PATH)
    set(PQ_LIBRARY_DIR "D:\\PostgreSQL\\17\\lib")

    if(NOT PQ_LIBRARY_DIR)
        message(FATAL_ERROR "libpq.dll not found")
    else(PQ_LIBRARY_DIR)
        message("libpq.dll found")
    endif()


    # Добавьте директорию с библиотеками
    link_directories(${PQXX_LIBRARY_DIR} ${PQ_LIBRARY_DIR})

     # Укажите исполняемый файл
    add_executable(pg main.cpp)

    # Укажите путь к Boost
    set(BOOST_ROOT "D:/boost-build") # Путь к библиотеке Boost
    set(BOOST_INCLUDE_DIR "D:/boost-build/include/boost-1_86/boost" "D:/boost-build/include/boost-1_86")
    set (Boost_USE_STATIC_RUNTIME ON)
    set(Boost_DIR "d:/boost-build/lib/cmake/Boost-1.86.0")
    # Подключаем необходимые модули.
    find_package (Boost 1.86 REQUIRED COMPONENTS system filesystem thread) 


   


    if (Boost_FOUND)

        include_directories (SYSTEM ${Boost_INCLUDE_DIR}) # подключаем заголовочные файлы
        target_link_libraries (pg ${Boost_LIBRARIES}) # подключаем библиотеку

    else()
        message("Boost not found")
    endif()


    set(nlohmann_json_DIR "D:/json/build")
    set(nlohmann_json_INCLUDE_DIRS "D:/json/include")
    find_package(nlohmann_json REQUIRED)
    include_directories(${nlohmann_json_INCLUDE_DIRS})

    set(ASIO_INCLUDE_DIR "D:/asio-1.30.2/include")
    include_directories(${ASIO_INCLUDE_DIR})

    set(CROW_INCLUDE_DIR "d:/Crowlib/include")
    set(CROW_DIR "D:/Crowlib/lib/cmake/Crow")

    #add_definitions(-DCROW_USE_BOOST OFF)
    #find_package(CROW REQUIRED)
    set(CROW_USE_BOOST OFF)
    message(STATUS ${CROW_USE_BOOST})
    # Укажите заголовочные файлы для подключения
    include_directories(${PQXX_INCLUDE_DIR} ${CROW_INCLUDE_DIR})
   
    # Подключите библиотеку
    target_link_libraries(pg  pqxx libpq ${PQ_LIB} ${CROW_LIBRARIES} wsock32 ws2_32) # или pqxx.lib для Windows
