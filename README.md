# CIS 6730 Spring 2023 HW #2

## Timeline
I'll give an in-class tutorial on Spin on Thursday 3/2. I'll release the HW assignment right after that class. I'll also be around after class to help install Spin.

HW #2 will be due the week after Spring Break, at 11:59pm on March 16. The assignment will be shorter than the Dafny assignment. I will host office hours 1:35-2:30pm on March 14 and 16 (after class). I will also host office hours 3:30-4:30pm on Wednesday March 15. All my OH are in GRW 057. If you can't make these OH times, please send me an email (ccwatson@seas.upenn.edu).


## Installing Spin
Please read all these instructions before you start installing Spin. There are multiple approaches, so make sure you make a good choice of which tutorial to follow.

In lecture, I just used Spin via the command line. Lots of installation guides also tell you how to install iSpin, the Spin GUI. This will lead to a nicer user experience, but you don't need to install iSpin to complete this assignment.

### C compiler and preprocessor
Spin needs you to have a working C compiler and preprocessor. If you have linux/mac, you likely already have one installed by default (probably gcc or clang). If you have Windows, you would have had to install one yourself. Popular ways to do this include [MinGW](https://www.mingw-w64.org/) and [Cygwin](https://www.cygwin.com/) (pick one, probably MinGW if you're just doing it for the first time). 

Spin is also sensitive to the path to your C preprocessor. The install tutorials address this.

### Official instructions

The official installation instructions are [here](https://spinroot.com/spin/Man/README.html). They explain how to install using pre-built binaries, or by building from source. If you choose to use pre-built binaries, make sure you get the right one for your OS (there is not one for Apple Silicon (M1 or M2) macs).

If you would prefer to build from source, you can clone the [official Github repo](https://github.com/nimble-code/Spin) and build from source. This looks something like `make; sudo cp Src/spin /usr/local/bin`, depending on your OS. Many students had success with this last year.

### Windows install
For a more in-depth guide on how to install (including MinGW), see [this blog post](https://blog.nathanv.me/posts/spin-windows/).

### Mac install
Whether you have an Intel or an Apple Silicon mac, you should be able to build from source as described in the official instructions.

Alternatively, there is a homebrew formula for Spin, but I haven't tried it. This [extremely boring YouTube video](https://www.youtube.com/watch?v=ADZBpmOJU_c) seems to show it working.

### Check your install
To check that your installation is working you can run `spin -V`.

## HW #2
All the instructions are in announcement.md !

The [Spin manual](https://spinroot.com/spin/Man/Manual.html) and [man pages](https://spinroot.com/spin/Man/promela.html) are good reference resources. 


