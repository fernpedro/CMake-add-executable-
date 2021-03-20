# CMake create add executable

We want to replicate a version of CMake's add_executable() functionality. 

**Workflow**: 

- We will extract compilation into the macro compile(). For this, create object file names main.o from main.cpp and hello.o from hello.cpp respectively using get_filename_component().
- Compile the source files main.cpp and hello.cpp into object files main.o and hello.o respectively and store them in variable using add_custom_command().
- In function creat_executable() we can use the macro compile() to compile the source files main.cpp and hello.cpp given via the terminal.