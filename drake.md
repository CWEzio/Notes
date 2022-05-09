# My learning notes on Drake

### Spatial Velocity and Spatial Force 
Drake's [spatial velocity](https://drake.mit.edu/doxygen_cxx/classdrake_1_1multibody_1_1_spatial_velocity.html) and [spatial force](https://drake.mit.edu/doxygen_cxx/classdrake_1_1multibody_1_1_spatial_force.html) are with respect to a specific point, which is slightly different with Modern Robotics book. In my opinion, Drake's definition is more physical and has more representation power. See the [Spatial Vector](https://drake.mit.edu/doxygen_cxx/group__multibody__spatial__vectors.html) page for more information.

## Pydrake
- Pydrake's binding is defined in the /bindings/pydrake directory. You can check the source code to see whether the function you need has been binded to python.

### Pydrake's symbolic comparison operator for Vecotor of `Expression` 
Related information can be found in [this issue](https://github.com/RobotLocomotion/drake/issues/8315) (most relevant), [this pull](https://github.com/RobotLocomotion/drake/pull/11171), [this question](https://stackoverflow.com/questions/64736910/using-le-or-ge-with-scalar-left-hand-side-creates-unsized-formula-array) and [this question](https://stackoverflow.com/questions/68615278/runtimeerror-you-should-not-call-bool-nonzero-on-formula).

It should be noted that, in pydrake, the comparison operator like `<=` can not operate on a vector of expressions. Instead, `lt`, `le`, `eq`, `ne`, `ge` and `gt` should be used. These operators are defined in `/binding/pydrake/_math_extra.py`. These operators are defined as a workaround way to avoid the undesired behaviors when operators like `<=` act on vector of expressions.

I think that `C++` do not have this issue. 

## Optimization
### Unbounded optimization variable will casue the slover fail
Recently I encounter the problem to solve for the Lyapunov function for linear system $\dot{x}=Ax$. 
>Note that for a continuous linear system, it is stable if and only if all its eigen values have negative real part; while for discrete continuous linear system, it is stable if and only if its eigen values all have norm less than 1.
To find the Lyapunov function for this system, we can solve the following optimization problem
$$\text{find } P \\  \text{subject to } P \succeq 0 \text{ and } \ PA+A^TP \preceq 0 \$$
The code for solving this problem is 
```python
# SDP for stability
# Example Linear systems
A = np.mat('-1,0;3,-2')

# A = np.mat('-1,2;1,2')
print(np.linalg.eig(A))

prog = MathematicalProgram()
P = prog.NewSymmetricContinuousVariables(2,'P')
alpha = prog.NewContinuousVariables(1,'alpha')

prog.AddCost(-alpha[0])

prog.AddPositiveSemidefiniteConstraint(P-0.01*np.eye(2))
prog.AddPositiveSemidefiniteConstraint(-A.transpose().dot(P) - P.dot(A) - alpha[0]*np.eye(2))
result = Solve(prog)
resultP = result.GetSolution(P)
resultAlpha = result.GetSolution(alpha)

print(resultP)
print(f"alpha = {resultAlpha}")
print(result.is_success())
P = result.GetSolution(P)
print(f"eig(P) = {np.linalg.eig(P)[0]}")
print("eig(Pdot) = " +
    str(np.linalg.eig(A.transpose().dot(P) + P.dot(A))[0]))
```
However, the solver fails to solve this probelm. It turns out that for this optimization objective $-\alpha$, the problem is unbounded above for decision variable $\alpha$. Adding a bounding box constraint will solve the problem
```python
prog.AddBoundingBoxConstraint([0.01],  [10], alpha)
```
Note that alpha will achieve the upper bound.

> It quiet suprise me first that the solver can fail for a feasible problem. But the solver cannot return a solution say that the best solution is $\infty$. Therefore the solver will not only fail at infeasible problem, but for problem that the optimal solution is unatainable.


## Bugs and Solutions
### Kernel died after add a handwritten class derived from `LeafSystem` to `DiagramBuilder`
It turns out that I foget to add 
```python
LeafSystem.__init__(self)
```
in the class's `__init__` method. This bug is quiet hard to find for that no useful debug information is given.


# TODO: MOVE Drake related terms into this note.