# WebGPU-Gaussian-Splat-Viewer

**University of Pennsylvania, CIS 565: GPU Programming and Architecture, Project 5**

* (TODO) YOUR NAME HERE
* Tested on: (TODO) **Google Chrome 222.2** on
  Windows 22, i7-2222 @ 2.22GHz 22GB, GTX 222 222MB (Moore 2222 Lab)

This video is compressed. See live demo for better results:
### [Live Demo]

## Performance Analysis

All numbers are frame times in milliseconds, averaged over 5 seconds at 1920×1080 resolution.

#### Point Cloud vs. Gaussian Renderer

![Point cloud vs. Gaussian renderer](readme_data/graph0.png)

Gaussian splatting is about 17–18× slower in both scenes. The point cloud draws one pixel per Gaussian. The Gaussian renderer computes each Gaussian's shape and color, sorts them by depth, and draws a blended quad of many pixels for each. The bicycle scene is slower in both because it has more Gaussians.

#### Workgroup Size

Frame time (ms) by preprocess workgroup size:

| Workgroup Size (threads) | bicycle_cleaned.ply (ms) | bonsai.ply (ms) |
|---:|---:|---:|
| 256 | 9.24 | 2.78 |
| 128 | 9.11 | 3.20 |
| 64  | 9.20 | 2.75 |
| 32  | 8.80 | 2.95 |

Workgroup size has almost no effect. Each thread handles its own Gaussian independently, so grouping matters little.

#### Number of Gaussians

![Frame time vs. fraction of Gaussians kept](readme_data/graph1.png)

Bicycle scene (1,063,091 Gaussians total).

Frame time drops steadily with fewer Gaussians: 1/8 of them is about 4× faster. It's not exactly proportional, because some costs are fixed (pass setup, screen clear, the sort's fixed steps), and fewer Gaussians also means less overlap per pixel. 

#### Culling vs. No Culling

Frame time (ms) with and without view frustum culling:

| Frustum Culling | bicycle_cleaned.ply (ms) | bonsai.ply (ms) |
|---|---:|---:|
| On  | 9.20 | 2.90 |
| Off | 9.17 | 3.00 |

### Credits

- [Vite](https://vitejs.dev/)
- [tweakpane](https://tweakpane.github.io/docs//v3/monitor-bindings/)
- [stats.js](https://github.com/mrdoob/stats.js)
- [wgpu-matrix](https://github.com/greggman/wgpu-matrix)
- Special Thanks to: Shrek Shao (Google WebGPU team) & [Differential Guassian Renderer](https://github.com/graphdeco-inria/diff-gaussian-rasterization)
