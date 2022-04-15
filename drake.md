# My learning notes on Drake

### Spatial Velocity and Spatial Force 
Drake's [spatial velocity](https://drake.mit.edu/doxygen_cxx/classdrake_1_1multibody_1_1_spatial_velocity.html) and [spatial force](https://drake.mit.edu/doxygen_cxx/classdrake_1_1multibody_1_1_spatial_force.html) are with respect to a specific point, which is slightly different with Modern Robotics book. In my opinion, Drake's definition is more physical and has more representation power. See the [Spatial Vector](https://drake.mit.edu/doxygen_cxx/group__multibody__spatial__vectors.html) page for more information.


## Bugs and Solutions
### Kernel died after add a handwritten class derived from `LeafSystem` to `DiagramBuilder`
It turns out that I foget to add 
```python
LeafSystem.__init__(self)
```
in the class's `__init__` method. This bug is quiet hard to find for that no useful debug information is given.


# TODO: MOVE Drake related terms into this note.