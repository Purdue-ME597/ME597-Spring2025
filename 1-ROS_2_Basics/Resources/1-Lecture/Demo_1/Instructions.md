### Compiling C++ Code 
Let's say you create a C++ calculator program, how is the end-user, who has no idea about programming going to run this program? They need to run the executable. Typically, the end-user clicks on an icon to run the executable, but for our example here, we will run the executable from the command line.

In this demo, we are going to learn how to compile C++ code using g++. In other words, we will create an executable from a `*.cpp` program.

1.  We have created a file called `main.cpp` to hold our C++ code. This code cannot be run as a `*.cpp` file, instead, we need to convert it to an executable in order to run it.
1. We move into the correct folder.
    * $`cd Demo_1`
2.  We will use the g++ compiler to convert the input `main.cpp` into an output executable called `my_executable`.
    * $`g++ main.cpp -o my_executable`

We have already run this command and uploaded this file, so this file should already be present before you run this command. But, if you were to delete this file and run this command yourself, you should see a file called `my_executable` in the same directory.

3. To run this executable:
    * $`./my_executable`
  
  It should print the string `Hello from main.cpp`

And there you have it, you have manually compiled a cpp program into an executable. 

Note: an executable is also a binary because executables are essentially machine code or binary code that the host machine understands.
    

But this is not a scalable option. If you want to compile multiple cpp programs and put them into a single executable file, we need to use something called a build system. Go to [Demo 2](../Demo_2) to learn more.
