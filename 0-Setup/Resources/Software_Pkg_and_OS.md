## Software Packaging

![Binary](https://upload.wikimedia.org/wikipedia/commons/f/fb/Basic_Idea_of_a_Compiler.png)

* Generally speaking there are two ways that software can arrive at "your computer" and the way this software arrives dictates how it is installed. Most of this is review, but a refresher is helpful for understanding ROS, and Linux.
* *Pre-Compiled Binaries* -- often called binaries, are software programs that have be *compiled* into objects that are ready to run on your computer.
    * These binaries have two main types:
        * Libraries -- sections of code that are reused over and over, like common math functions.
        * Executables -- also called programs are pieces of code that do some task and then end.
    * Most executables, or programs, will have a main part that then makes use of multiple libraries.
    * Many libraries are already installed on your computer, but some you may need to download and install.
* *Source Code* -- source code, or just code, is written in a human readable language that is compiled (i.e. translated) into executables.
    * Open Source Code is code that is freely available and shareable. You'll find it on places like Github or Gitlab.
    * Closed Source Code is code that is only shared via binaries, that you must often pay for.
* There are many exceptions to this rule, programming languages like Python sit somewhere in between the two. Languages like C and C++ have a clear distinction between binaries and source code.

### Binaries vs. Source Code

* Why does the difference between source and binaries matter?
* Because the difference between the two dictates how you install it on a robot.
* ROS has utilities to:
    * Build binaries from source code.
    * Install binaries that were compiled somewhere else.
    * Mix your source code, with other source code, with binaries. o
    * Keep track of everything that is going on!

## Operating Systems

![ROS and Linux Distros](https://upload.wikimedia.org/wikipedia/commons/8/89/Logo_Collage_Linux_Distro.png)

* An operating system is what makes your computer do what you want, and there are a lot of them.
* A distro, or *distribution*, is a specific version of software
    * Some folks also call a distro a release.
    * These are often given both a code name and a number.
        * These numbers usually denote either a number in a series or a release date.
        * e.g. **Jammy Jellyfish 22.04** or **ROS 2 Humble Hawksbill**