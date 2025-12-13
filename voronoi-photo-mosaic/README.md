# Voronoi Photo Mosaic

## Brief Description

This tutorial demonstrates how Voronoi diagrams can be used to create photo mosaics from images. Given an input image, we sample points across the image, compute a Voronoi tessellation, and color each Voronoi cell using the average color of the underlying image region. The result is a geometric reconstruction of the original image that preserves large-scale structure while introducing a stylized, polygonal appearance. This technique connects computational geometry with image processing and visual design.

## Tutorial Lead

Tucker Nielson

## Demos

| Original Image | Voronoi Mosaic |
|---------------|----------------|
| <img src="assets/bird.jpg" width="300"> | <img src="assets/bird_voronoi.png" width="300"> |
| <img src="assets/peach.jpg" width="300"> | <img src="assets/peach_voronoi.png" width="300"> |
| <img src="assets/butterfly.jpg" width="300"> | <img src="assets/butterfly_voronoi.png" width="300"> |


## References

- Aurenhammer, F. "Voronoi Diagrams - A Survey of a Fundamental Geometric Data Structure." ACM Computing Surveys, 1991.
