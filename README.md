# unity-mujoco-error-repo

# Minimal Reproduction of a MuJoCo Unity Plugin Issue

This repro contains the minimum necessary files to reproduce a MuJoCo Unity Plugin error I recently encountered.

When trying to import and run the MJCF for the Robotiq 85mm 2-Finger Adaptive Gripper from the [Mujoco Menagerie](https://github.com/deepmind/mujoco_menagerie/tree/main/robotiq_2f85), I've run into an error that this repository attempts to reproduce.

## Initial Setup

`minimal-error-repro` is the root directory for a Unity project. The error should occur when running the `GripperScene`.
To make it as easy as possible to recreate, the repository for mujoco is included so that the package paths will work generally on one's local machine.

I also included a shared library from `Mujoco 2.3.0` in `Assets/Plugins` named `libmujoco.so`

After cloning this repro, run `git submodule update --init --recursive` to correctly clone the mujoco sub-directory needed to install the MuJoCo Unity package.

To start, open Unity Hub and use `Open->Add Project from Disc` and select `minimal-error-repro` in the File Explorer.

You will then need to add the MuJoCo Plugin package yourself so that the absolute path is correct on your machine.

1. Go to `Window->Package Manager`.
2. Click the + to add a package. `Add package from disc`
3. In the File Explorer, navigate to the file in the repro sub-directory `mujoco/unity/package.json`
4. Unity should re-compile with the MuJoCo plugin.


## MJCF Changes
First, to import the MJCF `2f85.xml` (which is an adaptation of the similarly-titled file from the Menagerie), it was necessary to sub-sample the mesh to not trigger Unity's maximum mesh value limit. Therefore, the standard `.stl` files have been replaced with sub-sampled versions (found in `Models/meshes`) 

## Imoprt Message
When creating the Unity project `minimal-error-repro`, I imported `2f85.xml` using `Assets->Import MuJoCo Scene`. Unity displayed the following message:
```
"anchor in connect connect_45 ignored. Set Transforms in the editor."
"anchor in connect connect_46 ignored. Set Transforms in the editor."
```
## Runtime Error
When attempting to run the scene `GripperScene` with the imported gripper, Unity encounters MuJoCo runtime errors `Failed to creat Mujoco runtime`, which are caused by this error:
```
"IOException: Error loading the model: XML Error: required attribute missing: 'anchor'
Element 'connect', line 1"
```
Running the same file in MuJoCo directly does not cause any errors.