---
title: Rusty Raytracing
description: Discover the art of Rusty Raytracing! Unraveling graphics with Gambetta's insights, we paint visual wonders in code. 🚀🎨
summary: Discover the art of Rusty Raytracing! Unraveling graphics with Gambetta's insights, we paint visual wonders in code. 🚀🎨
date: 2023-11-13
tags: ["raytracing", "rust", "graphics"]
---

Diving into the captivating world of ray tracing and computer graphics has always been a thrilling journey for me. Initially, the intricate mathematics behind these concepts seemed daunting, but my perspective transformed when I stumbled upon a game-changing book on Amazon— ["Computer Graphics From Scratch" by Gabriel Gambetta](https://gabrielgambetta.com/computer-graphics-from-scratch/). This gem of a resource unravels the complexities of computer graphics with an approachable style, demystifying the underlying math without resorting to hand waving.

Armed with the knowledge gained from this book, I embarked on a personal project in Rust, crafting a simple ray tracer program capable of rendering scenes and transforming them into mesmerizing `.png` files. Join me in this blog post as we explore the fascinating world of ray tracing, demystify its mathematical intricacies, and witness the magic it brings to computer graphics.

## What is raytracing?

Ray tracing is a technique in computer graphics that simulates the way light interacts with objects in a virtual environment. It involves tracing the path of rays of light as they travel through a scene, calculating how they interact with surfaces to produce realistic lighting, shadows, reflections, and refractions in rendered images.

## How does it work?

Imagine you're a painter assigned to depict a landscape, armed with a canvas, paint, and a brush. However, there's a catch—you're not artistically inclined. But fear not, ray tracing comes to the rescue! In a way, it's like putting an insect net on a stick. By looking through this net, you can observe the landscape. Now, envision dividing the canvas into squares, mirroring those in the net. For each square in the net, identify the color of the landscape seen through it and paint that color onto the canvas in the corresponding position. Repeat this process for every hole in the net, and you've successfully crafted a painting! Here's the algorithm in a nutshell:

```Pseudocode
set everything up
for each square in the canvas
    find the corresponding square in the net
    observe through that square and determine the corresponding color
    paint the color in the right spot on the canvas
enjoy the painting
```

Now let's see the code that I wrote in Rust to do this:

```Rust
fn main() {
    let render_settings = get_settings();
    let mut canvas = Image::new(
        render_settings.canvas_width + 1,
        render_settings.canvas_height + 1,
    );
    canvas.set_all_pixels(|pos| {
        let direction = raytrace::canvas_to_viewport(&render_settings, pos.x, pos.y);
        raytrace::ray_trace(
            &render_settings,
            render_settings.camera_position,
            direction,
            render_settings.projection_plane_distance,
            f64::INFINITY,
            4,
        )
    });
    canvas.export(get_filename().as_str());
}
```

The first step in the pseudocode we wrote earlier is to set up everything. The `get_settings()` function does just that: it creates the scene based on what the user wants to render, and decides the camera position, the size of the final image etc etc.
The second line of the `main` function creates the netted canvas (in our case prepares a blank image with a certain resolution).


