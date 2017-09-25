cmake_minimum_required(VERSION 3.0) # Проверка версии CMake

set(CMAKE_CXX_STANDARD 11) #Установка переменных со списком исходных файлов
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print) #название проекта

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp) #Добавление статической библиотеки,используя указанные исходные файлы

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE}) #Создает исполняемый файл с именем EXAMPLE_NAME из исходника EXAMPLE_SOURCE.cpp
    target_link_libraries(${EXAMPLE_NAME} print) #Линковка команды с библиотекой
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)
