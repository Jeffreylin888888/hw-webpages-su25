---
title: Homework 1 Write-Up (Jeffrey, Katrina)
layout: home
---

## Task 1: Drawing Single-Color Triangles

- To rasterize a triangle, we first computed its axis-aligned bounding box. Then, for each pixel that is centered within that bounding box, we check whether it lies inside the triangle using edge functions based on the triangle's vertices. If a pixel passes all edge tests, we fill it with the appropriate color.

- My algorithm ensures correctness because the bounding box is the minimum rectangle enclosing the triangle. Although we test every pixel inside the box, the edge function method ensures only pixels truly inside the triangle are drawn. Therefore, our approach is just as accurate as sampling every point inside the bounding box.

- To speed up triangle rasterization, we applied the following optimizations:
  - Precomputed the constants for the edge functions (A, B, C) to avoid redundant arithmetic within the inner loop.
  - Used a tight bounding box via floor and ceil to limit the number of pixels tested.
  - Used the signs of the edge functions to quickly determine whether a sample lies within the triangle.
  - Added a bounds check before calling `fill_pixel` to avoid unnecessary memory access.
  
  These optimizations significantly reduced render time—from around 1.35 ms to 0.62 ms for `test4.svg`—resulting in roughly a 2× speedup.

### Screenshot of `test4.svg`
![test4](assets/realtask1.png)

### Code Snippets for Optimizations
![optimization1](assets/realtask1-1.png)
![optimization2](assets/realtask1-2.png)

### Timing Comparison
![timing](assets/realtask1-3.png)
