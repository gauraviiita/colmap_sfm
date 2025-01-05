


**Structure from Motion (SfM)** is a computer vision and photogrammetry technique used to reconstruct 3D structures from a series of 2D images taken from different viewpoints. It simultaneously estimates the 3D geometry of a scene and the camera poses (position and orientation) using image correspondences.

**COLMAP** is an open-source software designed for Structure-from-Motion (SfM) and Multi-View Stereo (MVS) workflows. It is widely used in computer vision and photogrammetry for 3D reconstruction tasks. COLMAP automates the process of reconstructing sparse and dense 3D models from a set of overlapping images or video frames.

## 1. Install COLMAP
```
$ sudo apt-get install colmap
```

## 2. Prepare Your Image Frames

 Extract frames from your video using a tool FFmpeg.

 Save the frames as sequentially numbered images in a folder, e.g., frames/00001.jpg, frames/00002.jpg, etc.
```
$ pip install ffmpeg

$ ffmpeg -i input_video.mp4 -qscale:v 1 -qmin 1 -vf fps=4 %04d.jpg
```


## 3: Create Project Structure
```
$ mkdir colmap_project
$ cd colmap_project
$ mkdir images sparse
```


## 4. Run COLMAP for SfM

```
1. Feature Extraction:
$ colmap feature_extractor \
    --database_path database.db \
    --image_path images/ \
    --ImageReader.camera_model PINHOLE \
    --ImageReader.single_camera 1


2. Feature Matching:
$ colmap exhaustive_matcher \
    --database_path database.db

3. Sparse Reconstruction: Create a directory for the sparse model (e.g., sparse/) and run:


$ colmap mapper \
    --database_path database.db \
    --image_path images/ \
    --output_path sparse/

4. Export Files: After running the above steps, the following files will be generated in the sparse/ folder:

Check whether these files are present in the sparse/0 folder.

$ ls sparse/0/

cameras.bin: Contains intrinsic and extrinsic camera parameters.
images.bin: Stores image metadata and pose information.
points3D.bin: Contains the reconstructed 3D points.
points3D.ply: A PLY file representing the reconstructed 3D points.

------------------------

$colmap model_converter \
    --input_path sparse/0/ \
    --output_path sparse/0/points3D.ply \
    --output_type PLY

```

5. Undistort Images: This step converts the camera model to PINHOLE or SIMPLE_PINHOLE. Use:
```
colmap image_undistorter --image_path images/ --input_path sparse/ --output_path undistorted/ --output_type COLMAP
```


## Verify files

The output folder will now contain the following:

cameras.bin

images.bin

points3D.bin

points3D.ply

