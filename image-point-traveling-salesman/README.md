# Image Point Traveling Salesman

## Brief Description
This project generates single-line and colored drawings from images using large-scale Euclidean Traveling Salesman Problem (TSP) optimization.

An input image is converted into a set of 2D points via density-driven sampling and dithering.
The resulting point set is then optimized using the LKH-3 heuristic solver to produce a continuous Hamiltonian tour, which is rendered as a single-line drawing.

The system focuses on large-scale artistic TSP instances (10⁴–10⁵+ cities) and emphasizes:

1. Density-based sampling strategies

2. Edge-aware point reduction

3. SAM-based block segmentation

4. Multi-core parallel LKH execution

5. Robust color-consistent rendering

The goal is not merely to solve TSP, but to study how sampling design and solver behavior influence visual structure, runtime, and aesthetic quality.
## Tutorial Lead
Dengyuhan Dai
## Demos
<img width="1000" height="1000" alt="girl_line_drawing1 2" src="https://github.com/user-attachments/assets/eec8228d-74d7-45ea-a923-db622e9d080f" />
<img width="1000" height="1000" alt="Hamuko_line_drawing_0 7" src="https://github.com/user-attachments/assets/e0690a6f-06c9-42f7-a79c-80595e91fad9" />
<img width="1000" height="750" alt="line_drawing_layers_Kinkakuji" src="https://github.com/user-attachments/assets/63e0ecea-7c53-4be5-9045-0288d39c1f4a" />

## References
Helsgaun, K. (LKH-3) — State-of-the-art Lin–Kernighan heuristic solver

Applegate et al. — Concorde TSP Solver

Google OR-Tools Routing Library

Floyd–Steinberg Dithering

Meta AI — Segment Anything Model (SAM)
