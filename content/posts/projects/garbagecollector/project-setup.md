---
title: Requirements and project setup
summary: We'll setup the environment.
draft: false
showToc: true
tocopen: true
tags: ["garbage collector", "C", "low level", "operating system"]
date: "2023-04-17"
weight: 2
---
**This project requires a good understanding of the C programming language**. You need to be very comfortable with pointers. Some knolege about how operating systems run programs will be useful but it's not a hard requirement.

Since the scope of this project is not to build the best and most efficient garbage collector that has ever existed we'll create a garbage collector that will **depend on the Linux kernel**. If you're not using a operating system dependent on the Linux kernel you can still follow along:

- If you're a Windows user you can use the [Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/install).
- If you're a Mac OS user you can use a [Virtual Machine](https://developer.apple.com/documentation/virtualization/running_gui_linux_in_a_virtual_machine_on_a_mac).

You will also need a text editor and a compiler. I am using [Visual Studio Code](https://code.visualstudio.com/) and [GCC](https://gcc.gnu.org/). You can use your favourite combination or copy mine.

Now we can create a folder and setup the project:

```
mdkir garbage_collector
cd garbage_collector
touch main.c
mdkir lib
cd lib
touch garbage_collector.c
touch garbage_collector.h 
```

We just created a foder named `garbage_collector` with a `main.c` file and a subfolder named `lib` with two files in it: `garbage_collector.c` and `garbage_collector.h`. This folder will contain a library that will implement our garbage collector.

The resulting file structure will look like this:

```
gargbage_collector/
├── main.c
└── lib/
    ├── garbage_collector.c
    └── garbage_collector.h
```

To compile with [GCC](https://gcc.gnu.org/) run

```
gcc main.c lib/garbage_collector.c -o runme
```
