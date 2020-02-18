# FunkyQuadZ-Public
FunkyQuadZ documentation and bug tracking.

# General use case:
- Define layout and game logic in C3 on a 2D layer (e.g. a map for doom game, a platformer layouyt)
- On initialization, use the 2D elements to create FQZ based 3D elements on a 3D layer
- Use C3 2D logic for collision, control, etc.
- Use FQZCamera to control the 3D view on the 3D layers.
- Tie FQZ Animations to C3 2D objects (e.g. a player mask, player location)

## FunkyQuadZ plugin (3D sprite)
There are two modes for FQZs:

### 3D Sprite Mode
* Similar to a 2D sprite
* Add a z dimension and depth (similar to x and width for example.)
* Add two more angles to rotate the FQZ (roll and pitch around the center point, the normal angle is used as yaw) 
* x,y,z position of the center point.
* The quad will always be rectangular in 3D space.
* You can set the 3D orientation of the sprite in two ways (mixing the ways is challenging):
  * Set Width, Height, Depth
  * Set Width, Height and then set the angles roll, pitch, yaw
### Vertex Mode
* In vertex mode the angles, center x,y,z position ACEs are ignored.
* Set the four x,y,z vertices of the FunkyQuadZ directly.
* If the quad is not rectangular in 3D space there may be issues with the texture of the quad, due to it being rendered as two triangles by C3, avoid this except for solid color texture.
## Animation
- Use the C3 Animation Editor
- Normal animation ACEs
# Constraints
## ZSort
- Simple Zsort using relative distance of object from camera works well. Create sortables family, with instance variable zOrder.
- `-> ZSort: Set zOrder to 0-((Self.X-Camera3D.CameraX)^2+(Self.Y-Camera3D.CameraY)^2+(Self.Z-Camera3D.CameraZ)^2)`
- `-> System: Sort ZSort Z order by zOrder`
- Look for ways to optimize this (e.g. experiment with Visible condition.)
- The FQZ will render in the same order as if they were other 2d sprite objects. They will render from bottom layer to top layer and from bottom to top within a layer.
- To render properly you must sort the quads depending on their depth and your view. There are some examples in the example project file how to do this depending on camera location vs object location (also see Painter's Algorithm.) - Since the C3 renderer does not use a depth buffer, it's not possible to have quads 'intersect' and render properly. Focus instead on creating non intersecting planes and solids.
- If the quad center is off of the screen it will be culled and not rendered. It may be possible to create a 'guard band' around the screen so this is less intrusive. Also investigating ways to make this not happen until the very edge.
- Using ScrollTo and Layout Scaling help with this (while not changing the view from the Cameral control (se below.)

# FQZCamera plugin - Camera control
- This can be used separately or together with FQZ.
- Each layer requires a FQZ with Camera Control (CC) enabled.
- The FQZ w/ CC must be on the bottom of the layer (so it updates the camera MV matrix before other drawing occurs.)
- Control the camera view by changing Camera Position (viewer) and Look Position (target). For most cases restrict the gamer rotation to not rotate beyond straight up or straight down. For full rotation, use the FQZCamera ACEs to control the 'Up Vector' and keep it ortogonal to the camera direction defined by the Camera Position and Look At position.

# Support
Please use the github issue list for support, so we can track questions, bugs, enhancement requests.

