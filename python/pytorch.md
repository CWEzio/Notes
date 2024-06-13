# Problems
## Bug when using tensorboard with pytorch
* Problem detail
```python
---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
<ipython-input-13-39c22274501e> in <module>
     19 writer.add_embedding(features,
     20                     metadata=class_labels,
---> 21                     label_img=images.unsqueeze(1))
     22 writer.close()

~/anaconda3/envs/legged_env/lib/python3.7/site-packages/torch/utils/tensorboard/writer.py in add_embedding(self, mat, metadata, label_img, global_step, tag, metadata_header)
    779         save_path = os.path.join(self._get_file_writer().get_logdir(), subdir)
    780 
--> 781         fs = tf.io.gfile.get_filesystem(save_path)
    782         if fs.exists(save_path):
    783             if fs.isdir(save_path):

~/.local/lib/python3.7/site-packages/tensorflow_core/python/util/module_wrapper.py in __getattr__(self, name)

AttributeError: module 'tensorflow._api.v1.io.gfile' has no attribute 'get_filesystem'

```
* solution
  
  It seems that pytorch and tensorflow conflict with each other. Deleting pytorch, tensorflow, tensorboard then reinstalling pytorch and tensorboard fix this bug.

See this issue https://github.com/pytorch/pytorch/issues/30966