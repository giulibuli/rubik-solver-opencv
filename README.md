# rubik-solver-opencv
Python pipeline using OpenCV and Kociemba's Two-Phase Algorithm for color recognition and automated solving of a 3x3x3 Rubik's Cube via Computer Vision.

# Rubik Solver & Computer Vision Pipeline

This project implements a complete Python software pipeline for the automatic recognition and resolution of a 3x3x3 Rubik's Cube starting from digital photographs of its six faces. 

The system combines **Computer Vision** techniques, **Combinatorial Optimization**, and **Search Algorithms** to map the cube's state into a standardized string, subsequently computing the optimal sequence of moves for its resolution.

---

## System Architecture & Algorithms

The software is structured into 5 sequential macro-phases:

### 1. Preprocessing and Cropping (Image Processing)
The program loads the face images from a dedicated directory following a strict geometric order. For each image, a central crop (`center_crop`) is executed to isolate the cube grid from the background, drastically reducing environmental noise and frame artifacts.

### 2. Color Feature Extraction (CIELAB Color Space)
To overcome issues related to reflections, shadows, and varying photo brightness, the algorithm processes the cube stickers as follows:
- Converts the image from the RGB/BGR color space to the **CIELAB (LAB)** color space, which effectively decouples lightness ($L$) from the chromatic components ($A$ and $B$).
- Divides the crop into a $3 \times 3$ grid and extracts the **median** of the central pixels of each sticker to sample the pure color.
- Uses the center sticker (index 4) of each face as a fixed **real centroid** to dynamically determine the 6 reference colors.

### 3. Global Optimization (Hungarian Algorithm)
Instead of classifying stickers individually (a greedy approach highly prone to false positives caused by shadows), the system computes the Euclidean distance in the LAB space between all 54 stickers and the 6 center centroids.
- The **Hungarian Algorithm** (`linear_sum_assignment`) is applied to an expanded cost matrix.
- This combinatorial optimization mathematically forces the system to assign **exactly 9 stickers to each of the 6 colors**, fundamentally solving misclassification errors caused by chromatic ambiguities.

### 4. Topological Mapping (Singmaster Notation)
The predicted colors are translated into standard English initials. The final 54-character string is generated respecting the cube's topology and the spatial orientation of the centers, ensuring that every "edge" and "corner" piece exists physically once and only once.

### 5. Resolution Algorithm (Kociemba's Two-Phase Algorithm)
The validated string is passed to the solver. Utilizing Kociemba's Two-Phase Algorithm, the software models the cube as a mathematical group and computes the shortest, most efficient sequence of moves (expressed in clockwise, counterclockwise, and double rotations) to restore the cube to its solved state.

---

## Requirements & Dependencies

To run this project, you need to install the following Python modules:

```bash
pip install opencv-python numpy scipy rubik-solver
