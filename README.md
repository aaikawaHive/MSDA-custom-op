This was tested using `nvcr.io/nvidia/pytorch:22.04-py3` 

To compile the torch custom op run

```
mkdir build
cd build
cmake -DCMAKE_PREFIX_PATH="$(python -c 'import torch.utils; print(torch.utils.cmake_prefix_path)')" ..
make -j
```

This will place the create the shared object file `libms_deform_attn_forward.so`in the `build/` folder.

To run with tritonserver (tested for `nvcr.io/nvidia/tritonserver:22.04-py3`) launch `tritonserver` with:

```
LD_LIBRARY_PATH=/opt/tritonserver/backends/pytorch:$LD_LIBRARY_PATH LD_PRELOAD="/path/to/build/libms_deform_attn_forward.so" tritonserver --model-repository=/path/to/model_repository/ --log-verbose=2 --http-port=8100 --grpc-port=8101 --metrics-port=8102
``` 
