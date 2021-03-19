# Source: https://lowlevelbits.org/bottom-up-cmake-introduction/

# Esto puede ser de gran ayuda:
# https://stackoverflow.com/questions/63529654/cmake-how-to-run-a-custom-command-on-a-number-of-files-to-generate-source-files

cmake_minimum_required(VERSION 3.15)

project(botton_up)

#[[
set (CMAKE_C_COMPILER gcc)
set (CMAKE_C_FLAGS -g -O0)

add_library(hello-target hello.cpp)
add_executable(hello main.cpp)
target_link_libraries(hello PRIVATE hello-target)
]]

# **** To run:  
#         botton-up-cmake> cmake .
#         botton-up-cmake> make hello-target main.cpp hello.cpp

# When funtion create_executable(hello-target main.cpp hello.cpp) the CMake builds 
# the target hello-target but not executable hello
#
# When the function create_executable(hello-target hello main.cpp hello.cpp)  no hello-target
# is built but all the main.cpp, main.o, hello.cpp and hello.o appear under configure step 
# 
# **** To remove building files:
#         botton-up-cmake> rm -rf CMakeCache.txt CMakeFiles/ cmake_install.cmake Makefile 

# **** Notes:
# 1. Tener como referencia el projecto: ~/pedro.2021/low-level-bits/function-cleaner/
# 2. It seems like this project can be run from build/. Need to be check

# 3. Otra dos referencias que me podrían ayudar es: pedro-2021/functions-macros/generate-executable/
#    and pedro-2021/functions-macros/generate-exec/
#    El primero es una adaptacion alproyecto en cuestion, incluye mani.cpp and hello.cpp
#    El segundo es una copia del original del blog pero modificado para no usar el paquete que 
#    hacen referencia en el blog lo que me parece mas practico. En ambos casos CMake da errores 
#    pero muestra interesantes resultados. La forma como ejecutar cada uno de ellos esta indicado
#    en el respectivo CMakeLists.txt file

# 3. ~/pedro-2021/functions-macros/custom-com-target/ by Blancas,gives insight into 
#     interaction between add_custom_comman() and add_custom_library()


set(CPP_COMPILER g++)
set(CPP_FLAGS -g -00)

macro(compile source_file)
    get_filename_component(output_file ${source_file} NAME_WE) # get full name without directory
    #message("${source_file}")         # main.cpp / hello.cpp
    set(output_file ${output_file}.o)                          # create two file names
    #message("${output_file}")         # main.o / hello.o
    add_custom_command(OUTPUT ${output_file}}                  # Object files: main.o, hello.o
                       COMMAND ${CPP_COMPILER} ${CPP_FLAGS}  -c ${source_file}
                       DEPENDS ${source_file} )
endmacro()    

function(create_executable name exe)
    foreach(file ${ARGN})
        compile(${file})  
       #message("${file}")            # main.cpp / hello.cpp
        set(output_files ${output_files} ${output_file})  # output_files={main.o, hello.o}?
        #message("${output_files}")   # main.o / main.o;hello.o
    endforeach()

    add_custom_command(OUTPUT ${output_files} 
                      COMMAND ${CPP_COMPILER} ${ouput_file} -o ${exe}
                      DEPENDS ${output_file})
    add_custom_target(${name} DEPENDS ${exe})
    message("${name}")       # hello-target
    message("${exe}")        # hello
endfunction()
 create_executable(hello-target hello  main.cpp hello.cpp)  #invoking function and execution of <commands>
# Estoy casi convencido que mi error es que no estoy implementando mal target_link_libraries()
# o en el peor de los casos, en general, no lo estoy implementando.

#**************************


#[[
add_custom_target(hello-target COMMAND cmake -E echo "Hello, CMake World")

add_custom_target(my-custom-target COMMAND ${CMAKE_COMMAND} -E echo Hello this is a custom target)

message("${CMAKE_COMMAND}")  # /usr/local/bin/cmake
]]

# To run both custom targets:
# build$ cmake --build . --target hello-target my_custom_target
# or
# build$ make hello-target my-custom-target
