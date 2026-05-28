# Intro To Kernel Hacking

_Credit: this assignment has been adapted from [here](https://github.com/remzi-arpacidusseau/ostep-projects/tree/master/initial-xv6)._

To develop a better sense of how an operating system works, you will also do a few projects _inside_ a real OS kernel. The kernel we'll be using is [xv6](https://github.com/mit-pdos/xv6-riscv); it is a port of the original Unix (version 6). It was developed at MIT and is a small and relatively understandable OS and thus an excellent focus for simple projects.

This project is a warmup, and thus relatively light on work. The goal of the project is simple: to add a system call to xv6. Your system call, `getreadcount()`, simply returns how many times that the `read()` system call has been called by user processes since the time the kernel was booted.

Your new system call should have the following signature:

```c
int getreadcount(void);
```

Your system call returns the value of a counter (perhaps called `readcount` or something like that) which is incremented every time any process calls the `read()` system call. That's it!

## Background

The xv6 source code is stored in the `src/` directory. To compile and launch the xv6 OS using the QEMU emulator, navigate into this directory in the terminal and execute the following:

```
make qemu
```

The source code for the OS will be compiled, then the QEMU emulator will be launched and the OS will be booted. You should see a shell prompt appear after the OS boots in the emulator:

```
xv6 kernel is booting

hart 2 starting
hart 1 starting
init: starting sh
$
```

When finished, you can stop the emulator by pressing `Ctrl` + `a`, then `x` (press the control key and the 'a' key together, then release, then press the 'x' key).

## Background

The xv6 source in this repository comes from a [fork](https://github.com/eu-cpsc415/xv6-riscv) of the original with only minimal changes to ensure it compiles inside the dev container.

For additional context on this assignment, here are two resources that may help:

- A [discussion video](https://www.youtube.com/watch?v=vR6z2QGcoo8) from the textbook author.
- A [background writeup](https://github.com/remzi-arpacidusseau/ostep-projects/blob/master/initial-xv6/background.md), also by the textbook author.

Note that these resources are referencing an older version of the xv6 kernel tareting x86. We are using the RISC-V variant of xv6 in this class. So you may see some differences in the code shown in the discussion video, for example. However, the main concepts are the same and these resources are still very valuable.

More information about xv6, including a very useful book written by the MIT folks who built xv6, is available [here](https://pdos.csail.mit.edu/6.828/2025/xv6.html).

## Tips

You may find this [discussion video](https://www.youtube.com/watch?v=vR6z2QGcoo8) helpful. For this class, things will be slightly different (i.e., the xv6 code is already available in your assignment repository, we are using the newer RISC-V variant of xv6, etc.). However, the core concepts are still applicable.

One good way to start hacking inside a large code base is to find something similar to what you want to do and to carefully copy/modify that. Here, you should find some other system call, like `getpid()` (or any other simple call). Copy it in all the ways you think are needed, and then modify it to do what you need.

Most of the time will be spent on understanding the code. There shouldn't be a whole lot of code added.

Using gdb (the debugger) may be helpful in understanding code, doing code traces, and is helpful for later projects too. You can also use the debug launch targets that have been configured for VS Code using the **Run and Debug** panel (see also `.vscode/launch.json` and `.vscode/tasks.json`).

# Running Tests

Running tests for your system call is easy. Run the `test-getreadcounts.sh` script from the root of the repository:

```sh
prompt> ./test-getreadcounts.sh
```

If you implemented things correctly, you should get some notification that the tests passed. If not ...

The tests assume that xv6 source code is found in the `src/` subdirectory. If it's not there, the script will complain.

The test script does a one-time clean build of your xv6 source code using a newly generated makefile called `Makefile.test`. You can use this when debugging (assuming you ever make mistakes, that is), e.g.:

```sh
prompt> cd src/
prompt> make -f Makefile.test qemu
```

You can suppress the repeated building of xv6 in the tests with the `-s` flag. This should make repeated testing faster:

```sh
prompt> ./test-getreadcounts.sh -s
```

The other usual testing flags are also available. See [the testing README](https://github.com/remzi-arpacidusseau/ostep-projects/blob/master/tester/README.md) for details.
