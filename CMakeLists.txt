#
# Copyright (C) 2023, Inria
# GRAPHDECO research group, https://team.inria.fr/graphdeco
# All rights reserved.
#
# This software is free for non-commercial, research and evaluation use 
# under the terms of the LICENSE.md file.
#
# For inquiries contact  george.drettakis@inria.fr
#

cmake_minimum_required(VERSION 3.20)
set( CMAKE_CXX_COMPILER "/usr/bin/g++" )
set( CMAKE_C_COMPILER "/usr/bin/gcc" )
set( CMAKE_CUDA_COMPILER "/usr/local/cuda-11.8/bin/nvcc")

project(DiffRast LANGUAGES CUDA CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CUDA_STANDARD 17)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)

set(CMAKE_PREFIX_PATH "/usr/local/include/libtorch")
find_package(Torch REQUIRED)
include_directories("/usr/local/cuda-11.8/include")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D_GLIBCXX_USE_CXX11_ABI=0")

add_library(CudaRasterizer
	cuda_rasterizer/backward.h
	cuda_rasterizer/backward.cu
	cuda_rasterizer/forward.h
	cuda_rasterizer/forward.cu
	cuda_rasterizer/auxiliary.h
	cuda_rasterizer/rasterizer_impl.cu
	cuda_rasterizer/rasterizer_impl.h
	cuda_rasterizer/rasterizer.h
)

set_target_properties(CudaRasterizer PROPERTIES CUDA_ARCHITECTURES "70;75;86")

target_include_directories(CudaRasterizer PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/cuda_rasterizer)
target_include_directories(CudaRasterizer PRIVATE third_party/glm ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})
target_link_libraries(CudaRasterizer "${TORCH_LIBRARIES}")