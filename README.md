# Animated OBJ
Animated OBJ is an extension to the [Wavefront OBJ](https://www.fileformat.info/format/wavefrontobj/egff.htm) format, which adds animation functionality.
**Disclaimer: This file format is currently completely theoretical and hasn't been tested yet. Major issues might occur**
## Table of Contents
- [File Format](#File-Format)
- [Animated Object](#Animated-Object)
- [Vertex Joints](#Vertex-Joints)
- [Vertex Weights](#Vertex-Weights)
- [Addition to Faces](#Addition-to-Faces)
- [Joints](#Joints)
- [Animation group](#Animation-Group)
- [Animate Position](#Animate-Position)
- [Animate Rotation](#Animate-Rotation)
- [Example](#Example)
- [Available Tools](#Available-Tools)
## File Format
Since this extension focuses mostly on animation functionality, the file containing this code is named **Animated OBJ file** with the file extension of
```
<filename>.amo  
```
It builds on top of the Wavefront OBJ syntax and uses the same patterns
___
___
## Animated Object
```
ao <name>
```
Instead of `o`, you can specify this keyword to make sure the file loader interprets the animation keywords the right way. `o` can be used for inanimate objects

| argument | type   | description                     |
|:--------:|:------:|:-------------------------------:|
| `<name>` | string | the name of the animated object |
___
## Vertex Joints
```
vj <joint> <joint> <joint> <joint>
```
Adds a joint attribute to a vertex. Each joint is able to influence the position and rotation of the vertex. A vertex can be influenced by up to 4 joints.

| argument  | type | description |
|:---------:|:----:|:-----------:|
| `<joint>` | int |  the index of the affecting joint. If there are less than 4 joints affecting the vertex, all other joint ids should be set to **0** with a weight of **0** |
___
## Vertex Weights
```
vw <weight> <weight> <weight> <weight>
```
Adds a weight attribute to a vertex. The weight sets the influence of the joints. It is directly linked with `vj`

| argument  | type  | description |
|:---------:|:-----:|:-----------:|
|`<weight>` | float | the influence of the corresponding joint. The four values should be normalized, that means the sum of the four weights should be 1 |
___
## Addition to Faces
```
f <pos>/<uv>/<normal>/<joints>/<weights>
```
Instead of the usual 3 attributes, you now have five attributes

| argument    | type | description |
|:-----------:|:----:|:-----------:|
| `<pos>`     | int  | the index of the vertex position |
| `<uv>`      | int  | the index of the vertex uv |
| `<normal>`  | int  | the index of the vertex normal |
| `<joints>`  | int  | the index of the vertex joints (not the real joints) |
| `<weights>` | int  | the index of the vertex weights |
___
## Joints
```
j <name> <joint>
```
Specifies one joint/bone with its name and its parent. It works of the same principal as the vertex implementation. So the first `j` has index 1, the second `j` index 2, etc. The [example](#Example) at the end of this file should clarify it

| argument  | type   | description |
|:---------:|:------:|:-----------:|
| `<name>`  | string | the name of the joint |
| `<joint>` | int    | the index of the parent joint. If the joint is the root joint (no parent), this value should be set as **-1** |
___
## Animation group
```
a <name>
```
All animation keywords after this command get grouped together, so you can create different animations for the same object (idle, walking, etc.)

| argument | type   | description               |
|:--------:|:------:|:-------------------------:|
| `<name>` | string | the name of the animation |
___
## Animate Position
```
ap <timestamp> <joint> <x> <y> <z>
```
Adds an animation keyframe for the position

| argument            | type   | description |
|:-------------------:|:------:|:-----------:|
| `<timestamp>`       | float  | the timestamp at which the object should have this pose. Timestamp 0 is required |
| `<joint>`           | int    | the index of the joint, which position will be changed |
| `<x>`, `<y>`, `<z>` | float  | the new position of the joint |
___
## Animate Rotation
```
ar <timestamp> <joint> <x> <y> <z> <w>
```
Adds an animation keyframe for the rotation. **Warning: The rotation is in quaternions and not in the usual euler rotation format**

| argument                   | type   | description |
|:--------------------------:|:------:|:-----------:|
| `<timestamp>`              | float  | the timestamp at which the object should have this pose. Timestamp 0 is required |
| `<joint>`                  | int    | the index of the joint, which rotation will be changed
| `<x>`, `<y>`, `<z>`, `<w>` |  float | the new rotation of the joint in quaternions |
___
## Example
```obj
# Animated OBJ example
ao cube

# The vertex positions
v 1.0 1.0 -1.0
v 1.0 -1.0 -1.0
v 1.0 1.0 1.0
v 1.0 -1.0 1.0
v -1.0 1.0 -1.0
v -1.0 -1.0 -1.0
v -1.0 1.0 1.0
v -1.0 -1.0 1.0

# The vertex uvs
vt 0.625 0.00
vt 0.375 0.25
vt 0.375 0.00
vt 0.625 0.25
vt 0.375 0.50
vt 0.375 0.25
vt 0.625 0.50
vt 0.375 0.75
vt 0.625 0.75
vt 0.375 1.00
vt 0.375 0.50
vt 0.125 0.75
vt 0.125 0.50
vt 0.875 0.50
vt 0.625 0.50
vt 0.625 0.25
vt 0.625 0.75
vt 0.625 1.00
vt 0.375 0.75
vt 0.875 0.75

# the vertex normals
vn 0.0 1.0 0.0
vn 0.0 0.0 1.0
vn -1.0 0.0 1.0
vn 0.0 -1.0 0.0
vn 0.0 -1.0 0.0
vn 1.0 0.0 0.0
vn 0.0 0.0 -1.0

# the vertex joints
vj 1 -1 -1 -1
vj 1 2 -1 -1

# the vertex weights
vw 1 0 0 0
vw 0 1 0 0 

# the faces for the object with the additional vertex joints and vertex weights
f 5/1/1/1/1 3/2/1/1/1 1/3/1/1/1
f 3/4/2/1/1 8/5/2/1/1 4/6/2/1/1
f 7/7/3/1/1 6/8/3/1/1 8/5/3/1/1
f 2/9/4/1/1 8/10/4/1/1 6/8/4/1/1
f 1/11/5/1/1 4/12/5/1/1 2/13/5/1/1
f 5/14/6/1/1 2/9/6/1/1 6/15/6/1/1
f 5/1/1/2/2 7/16/1/2/2 3/2/1/2/2
f 3/4/2/2/2 7/7/2/2/2 8/5/2/2/2
f 7/7/3/2/2 5/17/3/2/2 6/8/3/2/2
f 2/9/4/2/2 4/18/4/2/2 8/10/4/2/2
f 1/11/5/2/2 3/19/5/2/2 4/12/5/2/2
f 5/14/6/2/2 1/20/6/2/2 2/9/6/2/2

# the joints. The first one is the root joint with index 1 and is referenced in vj 1
j Joint_1 -1
j Joint_2 1

# Now follows the idle animation
a idle

# the animation. Move joint 1 to (1.0, 1.0, 1.0) and then to (-2.0, -2.0, -2.0)
ap 0 2 1.0 1.0 1.0
ap 0.5 2 -2.0 -2.0 -2.0

# rotate joint 1
ar 0 1 0.0 0.0 1.0 0.0
```

## Available Tools
- [editamo](https://github.com/lmarz/editamo): Coversion tool from the glTF file format to the Animated OBJ file format
- [amoloader](htpps://github.com/enudstudios/amoloader): Simple library that loads and parses Animated OBJ files written in C89

## License
[![Public Domain](https://i.creativecommons.org/p/mark/1.0/88x31.png  "Public Domain")](https://creativecommons.org/publicdomain/mark/1.0/)  
This work is free of known copyright restrictions.