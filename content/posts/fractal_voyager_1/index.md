---
title: Fractal Voyager | Intro
description: What is Fractal Voyager and project setup.
summary: What is Fractal Voyager and project setup.
date: 2024-01-09
tags: ["C", "fractals", "OpenGL"]
series: ["Fractal Voyager"]
draft: true
series_order: 1
---

I've always found the realm of computer graphics fascinating. Within this domain of computer science, mathematics plays a significant role in crafting breathtaking visuals. It never ceases to amaze me that such artistic effects can be generated from something as inherently logical as a computer.

In a previous article, [I delved into coding a rudimentary raytracer](../rusty_raytracing/) using Rust. Now, in this series, I aim to develop a program that empowers users to dynamically explore three-dimensional fractals in real time. This program, aptly named *Fractal Voyager*, is the focus of this article, where I'll outline my plan for bringing it to life.

## Motivation

I'm not the pioneer of this idea; numerous programs akin to the one I aspire to create already exist. However, my motivation stems from the prospect of acquiring a wealth of knowledge in computer graphics, mathematics, and programming throughout this endeavor. For me, that's reason enough to embark on this journey.

My first encounter with a concept like this occurred through a [video](https://www.youtube.com/watch?v=N8WWodGk9-g&t=188s) on YouTube by [CodeParade](https://www.youtube.com/c/CodeParade). At that moment, I was left in awe by the possibilities showcased in the video. Although I yearned to develop a similar program, my mathematical and programming skills fell short. Now, thanks to the challenges posed by university exams, I believe I possess the requisite skills to bring this vision to life.

On a personal note, it is immensely satisfying to finally pursue an idea that has lingered in the recesses of my mind for an extended period. I hope this endeavor inspires you to join me on this exciting journey!

## Prerequisites

Despite what I just mentioned, the prerequisites for this project are quite reasonable. I'll assume you have knowledge in:

- The C programming language (which I'll use for the examples).
- Calculus, particularly derivatives and integrals.
- Linear Algebra, covering matrices, vectors, and various operations involving them.
- Basics of trigonometry, as familiarity with its fundamentals is necessary.

## Understanding the Graphics Pipeline

The graphics pipeline is a series of stages within computer graphics processing, converting 3D scene data into 2D images for display. It involves operations like geometry processing, rasterization, and shading, all contributing to the creation of the final rendered image.

For the purposes of this project, we won't delve deeply into this concept. It's essential to recognize the role of shaders in this process—programs written in a shading language like GLSL that execute on your GPU to perform various tasks. To render a fractal, we'll specifically utilize a Fragment Shader, also known as a Pixel Shader. This shader operates on every pixel on the screen, determining the ultimate color of each pixel in the final image.



