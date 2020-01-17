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

```cuda
/////////////////////////////////////////////////////////////////////
// array of structures
/////////////////////////////////////////////////////////////////////
strcut MyArray
{
 int num;
 float* arr;
};


void PrepareMyArray(int& num, MyArray*& my_data;)
{
    num = 3;
    my_data = (MyArray*)malloc(sizeof(MyArray) * num);

    my_data[0].num = 2;
    my_data[0].arr = (float*)malloc(sizeof(float) * my_data[0].num);
    for(int i = 0; i < my_data[0].num; ++i) { my_data[0].arr[i] = i; }

    my_data[1].num = 5;
    my_data[1].arr = (float*)malloc(sizeof(float) * my_data[1].num);
    for(int i = 0; i < my_data[1].num; ++i) { my_data[1].arr[i] = i; }

    my_data[2].num = 3;
    my_data[2].arr = (float*)malloc(sizeof(float) * my_data[2].num);
    for(int i = 0; i < my_data[2].num; ++i) { my_data[2].arr[i] = i; }
};


float Access(int num, const MyArray* my_data, int index, int arr_index)
{
    if(0 > index || index >= num) {  // error handling }
    if(0 > arr_index || arr_index >= my_data[index].num) { // error handling }
    return my_data[index].arr[arr_index];
}

      
/////////////////////////////////////////////////////////////////////
// structure of arrays
/////////////////////////////////////////////////////////////////////
struct Index
{
    int start;
    int end;
};


void PrepareMyArrayForCuda(int& num, Index*& indices, int& num_whole_arr, float*& whole_arr)
{
    num = 3;
    indices = (Index*)malloc(sizeof(Index) * num);

    indices[0].start = 0;
    indices[0].end = 2;
    indices[1].start = 2;
    indices[1].end = 7;
    indices[2].start = 7;
    indices[2].end = 10;
    num_whole_arr =  indices[num-1].end;
    whole_arr = (float*)malloc(sizeof(float) * num_whole_arr);
    for(int i = 0; i < num; ++i)
    {
        for(int j = indices[i].start; j < idices[i].end; ++j)
        {
            whole_arr[j] = j - indices[i].start;
        }
    }
}


float AccessForCuda(int num, const Indices*indices, const float* whole_arr, int index, int arr_index)
{
    if(0 > index || index >= num) {  // error handling }
    if(0 > arr_index || arr_index >= (indices[index].end - indices[index].start)) { // error handling }
    return whole_arr[indices[index] + arr_index];
}
```

This code requires the number of arrays in advance. I am not very familiar with how to implement code which changes the number of elements in array in run-time. It may uses whole_arr like as memory pool... If you know about it, please feedback.
