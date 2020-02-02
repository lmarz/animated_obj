# Extended OBJ
Extended OBJ is an extension to the [Wavefront OBJ](https://www.fileformat.info/format/wavefrontobj/egff.htm) format, which adds textures and animation functionality. The file extension is usually `.obj`  
**Disclaimer: This file format is currently completely theoretical and hasn't been tested yet. Major issues might occur**
## Textures
```
t <texturepath>
```
Adds a texture to the wavefront object. This keyword should only be used once per object  
- `<texturepath>` is the relative path to the texture file
## Animation
### Vertex Joints
```
vj <joint> <joint> <joint> <joint>
```
Adds a joint attribute to a vertex. Each joint is able to influence the position and rotation of the vertex. A vertex can be influenced by up to 4 joints.
- `<joint>` is an integer, which represents the index of the affecting joint. If there are less than 4 joints affecting the vertex, all other joint ids should be set to **-1**
### Vertex Weights
```
vw <weight> <weight> <weight> <weight>
```
Adds a weight attribute to a vertex. The weight sets the influence of the joints. It is directly linked with `vj`
- `<weight>` is a float value representing the influence of the corresponding joint. The four values should be normalized, that means the sum of the four weights should be 1
### Faces
```
f <pos>/<uv>/<normal>/<joints>/<weights>
```
Instead of the usual 3 attributes, you now have five attributes
- `<pos>` is the index of the vertex position
- `<uv>` is the index of the vertex uv
- `<normal>` is the index of the vertex normal
- `<joints>` is the index of the vertex joints (not the real joints)
- `<weights>` is the index of the vertex weights
### Joint Parent
```
jp <joint>
```
Specifies the parent of one joint. It works of the same principal as the vertex implementation. The [example](#Example) at the end of this file should clarify it.
- `<joint>` is the index of the parent joint. If the joint is the root joint (no parent), this value should be set as **-1**
### Animation Position
```
ap <frame> <joint> <x> <y> <z>
```
Adds an animation keyframe for the position
- `<frame>` is the timestamp at which the object should have this pose. Frame 0 is required
- `<joint>` is the index of the joint, which position will be changed
- `<x>`, `<y>`, `<z>` are float values, which determine the new position of the joint
### Animation Rotation
```
ar <frame> <joint> <x> <y> <z> <w>
```
Adds an animation keyframe for the rotation. **Warning: The rotation is in quaternions and not in the usual euler rotation format**
- `<frame>` is the timestamp at which the object should have this pose. Frame 0 is required.
- `<joint>` is the index of the joint, which rotation will be changed
- `<x>`, `<y>`, `<z>`, `<w>` are float values, which determine the new rotation of the joint in quaternions
## Example
```
# Extended OBJ example
o cube

# the texture, which is in the same folder as the .obj file
t image.png

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
vj 0 -1 -1 -1
vj 0 1 -1 -1

# the vertex weights
vw 1 0 0 0
vw 0 1 0 0 

# the faces for the object with the additional vertex joints and vertex weights
f 5/1/1/0/0 3/2/1/0/0 1/3/1/0/0
f 3/4/2/0/0 8/5/2/0/0 4/6/2/0/0
f 7/7/3/0/0 6/8/3/0/0 8/5/3/0/0
f 2/9/4/0/0 8/10/4/0/0 6/8/4/0/0
f 1/11/5/0/0 4/12/5/0/0 2/13/5/0/0
f 5/14/6/0/0 2/9/6/0/0 6/15/6/0/0
f 5/1/1/1/1 7/16/1/1/1 3/2/1/1/1
f 3/4/2/1/1 7/7/2/1/1 8/5/2/1/1
f 7/7/3/1/1 5/17/3/1/1 6/8/3/1/1
f 2/9/4/1/1 4/18/4/1/1 8/10/4/1/1
f 1/11/5/1/1 3/19/5/1/1 4/12/5/1/1
f 5/14/6/1/1 1/20/6/1/1 2/9/6/1/1

# the joints. The first one is the root joint with index 0 and is referenced in vj 0
jp -1
jp 0

# the animation. Move joint 1 to (1.0, 1.0, 1.0) and then to (-2.0, -2.0, -2.0)
ap 0 1 1.0 1.0 1.0
ap 60 1 -2.0 -2.0 -2.0

# rotate joint 0
ar 0 0 0.0 0.0 1.0 0.0
```