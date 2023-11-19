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

## The painting analogy

Imagine you're a painter assigned to depict a landscape, armed with a canvas, paint, and a brush. However, there's a catch—you're not artistically inclined. But fear not, ray tracing comes to the rescue! In a way, it's like putting an insect net on a stick. By looking through this net, you can observe the landscape. Now, envision dividing the canvas into squares, mirroring those in the net. For each square in the net, identify the color of the landscape seen through it and paint that color onto the canvas in the corresponding position. Repeat this process for every hole in the net, and you've successfully crafted a painting! Here's the algorithm in a nutshell:

```Pseudocode
set everything up
for each square in the canvas
    find the corresponding square in the net
    observe through that square and determine the corresponding color
    paint the color in the right spot on the canvas
enjoy the painting
```

## Understanding the code

Let's see the code I wrote in Rust to do this:

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

### Setting the scene

```Rust
let render_settings = get_settings();
```

In the painting analogy, setting the scene is akin to choosing the landscape to paint and preparing the artistic tools. The `get_settings()` function defines the parameters of our virtual scene, including the camera position, canvas size, and other crucial details.

### Creating the canvas

```Rust
let mut canvas = Image::new(
    render_settings.canvas_width + 1,
    render_settings.canvas_height + 1,
);
```

Just as an artist prepares a canvas to capture the envisioned scene, our Rust program creates a blank canvas. This is a digital representation of the final image. The canvas dimensions are determined by the user-defined width and height from the settings.

### Painting time

```Rust
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

```

Here, the artist's brush is replaced by code that iterates over each pixel on the canvas. For every pixel, the program uses the `ray_trace` function to determine the color that corresponds to the landscape seen through a specific point. This mirrors the process of looking through a hole in the insect net, identifying the color, and applying it to the corresponding spot on the canvas.

### Saving the masterpiece

```Rust
canvas.export(get_filename().as_str());
```

As you would sign your work and preserve it for others to admire, our Rust program exports the completed image to a file, ready to be shared and appreciated. The filename is determined by user input.

## Try it

You can find the code used for this projet in a github repository:
{{< github repo="TheCaptainCraken/raytracer" >}}

To try it just ensure you have [rust installed on your system](https://www.rust-lang.org/tools/install) then:

```bash
git clone 'https://github.com/TheCaptainCraken/raytracer.git'
cd raytracer
cargo run --release filename
```

Where `filename` is the name (without extension) of the exported image. To change the scene, modify the `get_settings()` function in `main.rs`.

## Upcoming

Considering the complexity and richness of the ray tracing process, I'm contemplating transforming this exploration into a series. This series would meticulously dissect the raytrace function, guiding you through each facet and empowering you to grasp the essence of this foundational element in computer graphics.

### Next features

- **Arbitrary camera rotation**: right now it is possible to place the camera anywere but there is no way to change its rotation.
- **Parallelization**: raytracing is a very simple algorithm to parallelize because of its nature. It calculates each pixel independently thus opening the door to efficient parallel processing, ensuring faster rendering times.
- **Shadow optimization**: shadows can be optimized by  by strategically excluding unnecessary shadow calculations, optimizing for speed.
- **Subsampling & supersampling**: there is nothing preventing us from casting more or less then one ray per pixel.
- **Support other primitives**: right know the program only support spheres because they are very simple to manipulate. In the future I would like to add triangles and planes. Adding triangles would be very useful because this way it's easy to support any other polygon as they can be all constructed from triangles.
- **Transparency**: it would be nice to implement transparent materials allowing light to interact with transparent objects in a visually accurate manner.
- **Refractions**: it would be nice to implement refractions simulating the bending of light as it passes through different materials, such as water.
