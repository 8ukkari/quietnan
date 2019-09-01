---
title: Building Boost with VisualStudio
date: "2019-07-14T09:00:00Z"
template: "post"
draft: false
slug: "/posts/building-boost-with-visualstudio/"
category: "programming"
tags:
  - "c++"
description: "Boilerplate to build Boost library."
socialImage: ""
---

This is note to self to build Boost C++ library. I need to build Boost C++ library sometimes, but I completely forget how to build every time.

[Overview of Boost.Build User Manual](https://www.boost.org/doc/libs/1_65_0/doc/html/bbv2/overview.html#bbv2.overview.configuration) is helpful but too specific to finish quickly...

## Procedure

1. Download code from [download page](https://www.boost.org/users/download/) and unzip it
2. Launch command prompt at the folder where you download and unzip the code
3. Run "booststrap.bat", then "b2.exe" will be made
4. Run below commands which I often use :

```sh
b2 toolset=msvc-14.0 threading=multi variant=debug,release link=static runtime-link=static address-model=64 --stagedir=stage/x64 -j 4 
b2 toolset=msvc-14.0 threading=multi variant=debug,release link=shared runtime-link=shared address-model=64 --stagedir=stage/x64 -j 4
b2 toolset=msvc-14.0 threading=multi variant=debug,release link=static runtime-link=shared address-model=64 --stagedir=stage/vc10/x64 -j 4
```

## Building options

| option        | role                                                                                                                                                            |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| toolset       | to choose a compiler. "msvc-14.0 is for VisualStudio 2015. If you want to build with VisualStudio 2010, you need to set "msvc-10.0"                             |
| threading     | to choose multi thread                                                                                                                                          |
| variant       | to choose configuration of built files, "debug", "release" or both                                                                                              |
| link          | to choose which type of boost library you use, static link or dynamic link. "static" is as static link library, and "shared" is as dynamic link library         |
| runtime-link  | to choose which type of C/C++ runtime library you use, static link or dynamic link. "static" is as static link library, and "shared" is as dynamic link library |
| address-model | to choose platform of built output files. "64" means that the files are for 64bit. If you want to build for 32 bit, you need to set "32"                        |
| --stagedir    | to choose folder where built lib and dll files are saved. "stage/x64" means that "<current folder>\stage\x64" is the target folder                              |
| -j            | to set how many core you use to build the code. "4" means to use 4 cores                                                                                        |
