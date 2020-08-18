# Working With GameObject

## Transform
---
### __Position__
- GameObjects has 2 position information
    - Uses `Vector3` : A vector with 3 float data parameter.
    - __gameObject.transform.position__ : Objects postion in the global co-ordinate. Generally the value shown in the inspector is the global position (if not child)
    - __gameobject.transform.localPosition__ : Objects postion in the local co-ordinate. Position of a child object relative to the Parents position. 
        - *__local position `(0f,0f,0f)`: child and parent has the same center of gravity__*
        - *__local position `(.5f,0f,0f)`: childs center of gravity is at the right most point of the parent__*
        - *__local position `(1f,0f,0f)`: distance between childs center and parents center is exactly the `x` value of the parent, child will be on `positive x` axis__*
        - *__local position `(-.5f,0f,0f)`: childs center of gravity is at the left most point of the parent__*
        - *__local position `(1f,0f,0f)`: distance between childs center and parents center is exactly the `x` value of the parent, child will be on `negative x` axis__*

### __Rotation__
- Uses `Quternion`: A Vector with 4 float data parameter.
- `Editor` uses 3 point `Degree` data.
- GameObjects has 2 position information
    - __gameObject.transform.rotation__ : Objects rotation in the global co-ordinate. Generally the value shown in the inspector is the global position (if not child)
    - __gameobject.transform.localRotation__ : Objects rotation in the local co-ordiante. Position of a child object relative to the Parents position.

### __Scale__
- Uses `Vector3`
- Only One scale information. Editor: `Scale` , Script : `localScale`.

