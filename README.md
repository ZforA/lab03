Задание 1
 
Создание CMakeLists.txt для автоматической сборки данной библиотеки

$ cd formatter_lib

$ cat > CMakeLists.txt <<EOF //удаляет (если они есть) данные из файла и создает чистый файл

cmake_minimum_required(VERSION 3.10) //ставим подходящую минимальную версию cMake

project(formatter) //название проекта

EOF

Установка стандарта языка

$ cat >> CMakeLists.txt <<EOF //добавляет в файл данные

set(CMAKE_CXX_STANDARD 20) //ставим 20 версию CMake

set(CMAKE_CXX_STANDARD_REQUIRED ON) // устанавливаем стандарт язык

EOF

Сборка статической библиотеки с именем formatter и данным файлом formatter.cpp

$ cat >> CMakeLists.txt <<EOF
                              
add_library(formatter STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)
                              
EOF
                              
Добавление директории с заголовочными файлами
                              
$ cat >> CMakeLists.txt <<EOF
  
include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
  
EOF
  
Создаем директорию для использования корневой папки для сборки
  
$ cmake -H. -B_build
  
Проверка библиотеки formatter на наличие.
  
$ ls _build/libformatter.a
  
_build/libformatter.a
  
Задание 2
  
Создание CMakeLists.txt для автоматизированной сборки библиотеки в папке formatter_ex_lib
$ cd formatter_ex_lib
  
$ cat > CMakeLists.txt <<EOF
  
cmake_minimum_required(VERSION 3.10)
  
project(formatter_ex)
  
EOF
  
Установка стандарта языка и делаем его обязательным
  
$ cat >> CMakeLists.txt <<EOF
  
set(CMAKE_CXX_STANDARD 20)
  
set(CMAKE_CXX_STANDARD_REQUIRED ON)
  
set(CMAKE_CURRENT_SOURCE_DIR /home/lab03)
  
EOF
  
Сборка статической библиотеки с именем formatter_ex_lib и исходным файлом formatter.cpp
  
$ cat >> CMakeLists.txt <<EOF
  
add_library(formatter_ex STATIC \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex.cpp)
  
EOF
  
Добавляем директории с заголовочными файлами
  
$ cat >> CMakeLists.txt <<EOF
  
include_directories(\${CMAKE_CURRENT_SOURCE_DIR})
  
EOF
  
Указание директории где находятся заголовочные файлы
  
$ cat >> CMakeLists.txt <<EOF
  
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
  
target_link_libraries(formatter_ex formatter)
  
EOF
  
Создание директории для использования корневой папки для сборки
  
$ cmake -H. -B_build
  
Проверка библиотеки formatter на наличие.

$ ls _build/libformatter_ex.a
  
_build/libformatter_ex.a
  
Задание 3
  
Создание CMakeLists.txt для автоматизированной сборки библиотеки в папке formatter_ex_lib
  
$ cd hello_world_application
  
$ cat > CMakeLists.txt <<EOF
  
cmake_minimum_required(VERSION 3.10)
  
project(hello_world)
  
EOF
  
Установливаем стандарт языка и делаем его обязательным
  
$ cat >> CMakeLists.txt <<EOF
  
set(CMAKE_CXX_STANDARD 20)
  
set(CMAKE_CXX_STANDARD_REQUIRED ON)
  
set(CMAKE_CURRENT_SOURCE_DIR /home/lab03)
  
EOF
  
Добавление исполняемого файла hello_world.cpp
  
$ cat >> CMakeLists.txt <<EOF
  
add_executable(hello_world \${CMAKE_CURRENT_SOURCE_DIR}/hello_world_application/hello_world.cpp)
  
EOF
  
Указываем директории где находятся заголовочные файлы библиотек formatter и formatter_ex.
  
$ cat >> CMakeLists.txt <<EOF

include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)

find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
  
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
  
EOF
  
Линкуем проект

$ cat >> CMakeLists.txt <<EOF
  
target_link_libraries(hello_world \${formatter} \${formatter_ex})
  
EOF
  
Собираем проект с помощью CMake в директории _build
  
$ cmake -B_build
  
Запускаем проект

$ _build/hello_world

solver, приложение которое использует статические библиотеки formatter_ex и solver_lib

$ cd solver_application
  
$ cat > CMakeLists.txt <<EOF
  
cmake_minimum_required(VERSION 3.10)
  
project(solver)
  
EOF
  
Установливаем стандарт языка и делаем его обязательным, 
  
$ cat >> CMakeLists.txt <<EOF
  
set(CMAKE_CXX_STANDARD 20)
  
set(CMAKE_CXX_STANDARD_REQUIRED ON)
  
set(CMAKE_CURRENT_SOURCE_DIR /home/lab03)
  
EOF
  
Сборка статической библиотеки с именем solver_lib и исходным файлом solver.cpp

$ cat >> CMakeLists.txt <<EOF
  
add_library(solver_lib STATIC \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib/solver.cpp)
  
EOF
  
Указываем директорию где находятся заголовочные файлы библиотек

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/solver_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)

Создаем директорию для сборки проекта с помощью Cmakе

$ cmake -H. -B_build

Добавляем исполняемый файл


$ cat >> CMakeLists.txt <<EOF
  
add_executable(solver \${CMAKE_CURRENT_SOURCE_DIR}/solver_application/equation.cpp)
  
EOF
  
Указываем директории, где находятся заголовочные файлы библиотек.
  
$ cat >> CMakeLists.txt <<EOF
  
find_library(formatter NAMES libformatter.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)
  
find_library(formatter_ex NAMES libformatter_ex.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)
  
find_library(solver_lib NAMES libsolver_lib.a PATHS \${CMAKE_CURRENT_SOURCE_DIR}/solver_lib)
  
EOF
  
Линкуем проект

$ cat >> CMakeLists.txt <<EOF
  
target_link_libraries(solver \${formatter} \${formatter_ex} \${solver_lib})
  
EOF
  
Собираем проект с помощью Cmake

$ cmake -B.

Собираем проект с помощью CMake в директории _build

$ cmake --build _build # Сборка всего проекта
  
$ cmake --build _build --target solver_lib 
  
$ cmake --build _build --target solver 
  
Запускаем проект

$ _build/solver && echo

