# Installation
For `python3.8`, install `jax` with
```
pip install -U "jax[cuda12_pip]" -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html

pip install -U "jaxlib" -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html
```

For newer version python, I can follow the official guidance.

> Need to have cuda-toolkit installed. Follow the steps in [nvidia.md](../deep_learning/nvidia.md#installation).
# Shape Bits
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

## Avoid using `jnp.array` as the default value
```python
# Don't do this
def process_data(data, weights=jnp.ones(1000)):
    return data * weights

# Do this instead
def process_data(data, weights=None):
    if weights is None:
        weights = jnp.ones(1000)
    return data * weights
```
When use the `jnp.array` as the default value of a function, even if this function is not called, the `jax` will still reserve a lot of GPU memory. This can influence other program. Therefore, generally avoid using `jnp.array` as the default value.

# Bugs
## Could not create cudnn handle: CUDNN_STATUS_INTERNAL_ERROR
When I run `jnp.zeros(3)`, I encounter the above error.

This error is caused by that `jax` cannot find `libcudnn.so`. This can be solved by:
```
sudo apt install libcudnn8
```
where *8* is the cudnn version. Replace it as needed.

In solving this issue, I find that `jnp.zeros(3)` works fine in the Jupyter notebook, but not in python script. After digging a while, I find that Jupyter notebook can lazy load libraries, and in this case, it loads `torch`. `torch` configures the `libcudnn` correctly, and by loads `torch`, `cudnn` is also loaded. Therefore, in Jupyter notebook, the code works without error. If I also `import torch`, in the python script, it can also run without error. Quiet interesting.


