---
title: Memory allocation for array of structures with array in CUDA
date: "2019-07-21T10:00:00Z"
template: "post"
draft: false
slug: "/posts/memory-allocation-for-array-of-structures-with-array-in-cuda/"
category: "programming"
tags:
  - "cuda"
description: "It is annoying to write CUDA code about array of structures with array, especially when the number of arrays are decided in run-time.
If you want to keep the data structure in CUDA, you need to use temporary host variable of the structure which has device address of the array defined in the structure, then copy the host variable into device memory. Two times memory copy between host and device memory is required. The more nested array of structure is, the more complex to implement code to copy memory is."
socialImage: "/media/memory-allocation-for-array-of-structures-with-array-in-cuda/aos.png"
---

It is annoying to write CUDA code about array of structures with array, especially when the number of arrays are decided in run-time.
If you want to keep the data structure in CUDA, you need to use temporary host variable of the structure which has device address of the array defined in the structure, then copy the host variable into device memory. Two times memory copy between host and device memory is required. The more nested array of structure is, the more complex to implement code to copy memory is.

Its solution is to change data structure. It is to erase the nested structure, and use an array merged with array defined in the original structure. The data implemented by structure should be re-defined as structure of start / end index for the merged array. 

**Original data structure:**

![array of structure](/media/memory-allocation-for-array-of-structures-with-array-in-cuda/aos.png)

**Modified data structure:**

![structure of array](/media/memory-allocation-for-array-of-structures-with-array-in-cuda/soa.png)

The following code is as sample implementation of the data structures:

`gist:7226eac1ea07ab8f4e053573ab15eb59`

This code requires the number of arrays in advance. I am not very familiar with how to implement code which changes the number of elements in array in run-time. It may uses whole_arr like as memory pool... If you know about it, please feedback.
