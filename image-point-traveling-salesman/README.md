# Image Point Traveling Salesman

## Brief Description
This project generates single-line and colored drawings from images using large-scale Euclidean Traveling Salesman Problem (TSP) optimization, based on the TSP art technique pioneered by [Bosch & Herman (2004)](https://www.sciencedirect.com/science/article/abs/pii/S0167637703001275) and [Kaplan & Bosch (2005)](https://archive.bridgesmathart.org/2005/bridges2005-301.pdf).

An input image is converted into a set of 2D points via density-driven sampling and [Floyd-Steinberg dithering](https://en.wikipedia.org/wiki/Floyd%E2%80%93Steinberg_dithering).
The resulting point set is then optimized using the LKH-3 heuristic solver to produce a continuous Hamiltonian tour, which is rendered as a single-line drawing.

The system focuses on large-scale artistic TSP instances and emphasizes:

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

## Environment And Installation
This program depends on three parts: Python packages, the SAM Python package plus checkpoint, and the external LKH-3 executable.

Python packages:

```bash
pip install numpy pillow matplotlib opencv-python
```

SAM:

```bash
git clone https://github.com/facebookresearch/segment-anything
pip install -e segment-anything
```

Then download a SAM checkpoint file, for example `sam_vit_h_4b8939.pth`.

LKH-3:

1. Download or build the LKH-3 solver executable.
2. Put `LKH-3.exe` in the same directory as `LKH-3_SAM_demo.ipynb`, or pass it with `--lkh_exe`, or set the environment variable `LKH_EXE`.

## Typical Run
```bash
python LKH-3_SAM_colored_connected_parallel.py ^
  --image input.jpg ^
  --sam_ckpt path\to\sam_vit_h_4b8939.pth ^
  --lkh_exe path\to\LKH-3.exe
```

Preview only, without running LKH:

```bash
python LKH-3_SAM_colored_connected_parallel.py ^
  --image input.jpg ^
  --sam_ckpt path\to\sam_vit_h_4b8939.pth ^
  --preview_only
```

## Full Program Flow
The actual pipeline of `LKH-3_SAM_demo.ipynb` is:

1. Parse command-line arguments and build an `ExperimentConfig`.
2. Create an output run directory and save the current config for reproducibility.
3. Load the input image, resize it to the target width, and prepare RGB and grayscale versions.
4. Apply grayscale preprocessing: contrast enhancement, percentile clipping, and optional gamma correction.
5. Build the density field according to `density_mode`: `gray`, `inv`, `grad`, `sat`, or `mix_gray_grad`.
6. Run SAM on the RGB image to generate segmentation masks.
7. Filter masks by area, sort/select layers, optionally remove overlap, and optionally add a background layer.
8. For each selected layer, split disconnected regions into connected components so each sub-layer is spatially connected.
9. Allocate a city budget to each layer and component according to area.
10. For each component, save previews, apply the mask onto the density map, run Floyd-Steinberg dithering, extract black pixels as TSP cities, and subsample cities if they exceed the component budget.
11. If `--preview_only` is enabled, stop here after generating masks and previews.
12. Otherwise, dispatch each component as an independent LKH job and run them in parallel with `ProcessPoolExecutor`.
13. For each component, write TSPLIB `.tsp` and `.par` files, call LKH-3, and read back the generated `.tour`.
14. Sort all finished tours by layer/component order.
15. Render all tours into the final colored line drawing using the original image colors.
16. Save the final result to `line_drawing_layers.png` and keep all intermediate outputs in the run directory.

## Output Structure
Each run creates a timestamped directory under `outputs/runs/`.

Typical contents:

- `config.json`: saved parameters of the run
- `run.log`: runtime log
- `layers/layer_xxx/...`: per-layer or per-component intermediate files
- `problem.tsp`, `problem.par`, `problem.tour`: LKH input/output files for each component
- `line_drawing_layers.png`: final rendered colored TSP artwork

## References

### TSP Art Methodology
- **[Procedure]** Robert Bosch and Adrianne Herman, [Continuous Line Drawings via the Traveling Salesman Problem](https://www.sciencedirect.com/science/article/abs/pii/S0167637703001275), *Operations Research Letters*, 2004, Volume 32, pages 302-303.
- **[Point Placements]** Craig Kaplan and Robert Bosch, [TSP Art](https://archive.bridgesmathart.org/2005/bridges2005-301.pdf), *Renaissance Banff: Bridges 2005: Mathematical Connections in Art, Music and Science*, pages 301-308.

### TSP Solvers
- **[TSP Software: Concorde]** https://www.math.uwaterloo.ca/tsp/concorde/
- Helsgaun, K. (LKH-3), state-of-the-art Lin-Kernighan heuristic solver
- Applegate et al., Concorde TSP Solver
- Google OR-Tools Routing Library

### Image Processing
- **[Floyd-Steinberg Dithering]** https://en.wikipedia.org/wiki/Floyd%E2%80%93Steinberg_dithering
- Meta AI, Segment Anything Model (SAM)
