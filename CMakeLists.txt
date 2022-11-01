cmake_minimum_required(VERSION 3.1 FATAL_ERROR)
project(ms_deform_attn_forward LANGUAGES CXX CUDA)

find_package(Torch REQUIRED)
# include src
include_directories(${PROJECT_SOURCE_DIR}/cpu ${PROJECT_SOURCE_DIR}/cuda ${PROJECT_SOURCE_DIR})

# Define our library target
add_library(ms_deform_attn_forward SHARED src/vision.cpp src/cuda/ms_deform_attn_cuda.cu src/cuda/ms_deform_im2col_cuda.cuh src/cpu/ms_deform_attn_cpu.cpp)
# Link against LibTorch
target_link_libraries(ms_deform_attn_forward "${TORCH_LIBRARIES}")

target_compile_options(ms_deform_attn_forward PRIVATE $<$<COMPILE_LANGUAGE:CUDA>:
-DCUDA_HAS_FP16=1,
-D__CUDA_NO_HALF_OPERATORS__,
-D__CUDA_NO_HALF_CONVERSIONS__,
-D__CUDA_NO_HALF2_OPERATORS__,
                       >)

set_property(TARGET torch_cuda PROPERTY INTERFACE_COMPILE_OPTIONS "")
set_property(TARGET torch_cpu PROPERTY INTERFACE_COMPILE_OPTIONS "")
