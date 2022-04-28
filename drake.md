# My learning notes on Drake

### Spatial Velocity and Spatial Force 
Drake's [spatial velocity](https://drake.mit.edu/doxygen_cxx/classdrake_1_1multibody_1_1_spatial_velocity.html) and [spatial force](https://drake.mit.edu/doxygen_cxx/classdrake_1_1multibody_1_1_spatial_force.html) are with respect to a specific point, which is slightly different with Modern Robotics book. In my opinion, Drake's definition is more physical and has more representation power. See the [Spatial Vector](https://drake.mit.edu/doxygen_cxx/group__multibody__spatial__vectors.html) page for more information.

### Pydrake
- Pydrake's binding is defined in the /bindings/pydrake directory. You can check the source code to see whether the function you need has been binded to python.

### Pydrake's symbolic comparison operator for Vecotor of `Expression` 
Related information can be found in [this issue](https://github.com/RobotLocomotion/drake/issues/8315) (most relevant), [this pull](https://github.com/RobotLocomotion/drake/pull/11171), [this question](https://stackoverflow.com/questions/64736910/using-le-or-ge-with-scalar-left-hand-side-creates-unsized-formula-array) and [this question](https://stackoverflow.com/questions/68615278/runtimeerror-you-should-not-call-bool-nonzero-on-formula).

It should be noted that, in pydrake, the comparison operator like `<=` can not operate on a vector of expressions. Instead, `lt`, `le`, `eq`, `ne`, `ge` and `gt` should be used. These operators are defined in `/binding/pydrake/_math_extra.py`. These operators are defined as a workaround way to avoid the undesired behaviors when operators like `<=` act on vector of expressions.

I think that `C++` do not have this issue. 

## Bugs and Solutions
### Kernel died after add a handwritten class derived from `LeafSystem` to `DiagramBuilder`
It turns out that I foget to add 
```python
LeafSystem.__init__(self)
```
in the class's `__init__` method. This bug is quiet hard to find for that no useful debug information is given.


# TODO: MOVE Drake related terms into this note.