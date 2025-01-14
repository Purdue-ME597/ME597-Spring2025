### Compiling C++ Code Using a Build System
In this demo, we are going to learn how to create an executable from a `*.cpp` program using the C++ build system- CMake. We have created a file called `main.cpp`. We need to convert this program into an executable, how do we do it?

We will use CMake to generate the compilation files from the input `main.cpp`. Then, we will use CMake again to use these intermediate generated files to get our executable called `my_executable`

The steps can be summarized as follows:
  1. Write your source code, `main.cpp`
  1. Call CMake to generate compilation files
  1. Call CMake to use these compilation files to get our executable

As you can see, it's not exactly straight-forward to get an executable from the source code for large projects. Furthermore, these intermediate files clutter our workspace- we can end up having our source code, our intermediate Make files and our executable all in the same folder. 

To follow good software development practices, we must segregate our workspace!
1. Our source code should go under a `src` folder
2. Our intermediate build files should go into a `build` folder
3. Our final executable will go in the `build` folder

Now, to generate the executable, follow these steps:
  1. Go to the root folder
      * $`cd Demo_2`
  2. Create a `build` folder and `cd` into it
      * $`mkdir build/ && cd build/`
  3. Invoke CMake to generate the make files from the source folder
      * $`cmake -S ../src/`
  4. Invoke CMake to build the final executable within the build folder
      * $`cmake --build .`

And there you have it, you have compiled a cpp program into an executable. And this principle is followed for large Cpp projects and all ROS 2 projects too!
* Note: if you want to try this example yourself, clear the contents of the `build/` folder and run the above commands