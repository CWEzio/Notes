# [shape bits]
Shape bits that I encounted when using `jax`. [This official doc](https://jax.readthedocs.io/en/latest/notebooks/Common_Gotchas_in_JAX.html) also gives common pitfalls of `jax`.
## Dynamic shapes
In general, avoid dynamic shapes for code within `jax.jit`, `jax.vmap`, etc.,  as they require tracing the code with some abstraction class, like `ShapedArray`. Dynamic shape can cause error in the tracing process.
## `jax ArrayImpl` type object is not hashable, use python object instead.
One example of this problem is
```python

@partial(jax.jit(), static_argnums=(2,))
def func(a, b, c):
    ...

mat = jnp.array(1., 2., 3.)
max_in_mat = jnp.max(mat).astype(jax.int32)
func(a, b, max_in_mat)
```
Above program will raise error, complaining that `max_in_mat` is not hashable. The reason is that `max_in_mat` is result of `jax` calculation, which makes it a `ArrayImpl` object. To deal with this problem, convert it to a python `int` before giving it to func:
```python
max_in_mat = int(jnp.max(mat))
```

