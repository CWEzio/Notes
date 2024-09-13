# [shape bits](https://jax.readthedocs.io/en/latest/notebooks/Common_Gotchas_in_JAX.html)
## Dynamic shapes
In general, avoid dynamic shapes for code within `jax.jit`, `jax.vmap`, etc.,  as they require tracing the code with some abstraction class, like `ShapedArray`. Dynamic shape can cause error in the tracing process.
