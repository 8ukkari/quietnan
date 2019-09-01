---
title: JavaScript numeric performance
date: "2019-05-30T10:00:00Z"
template: "post"
draft: false
slug: "/posts/javaScript-numeric-performance/"
category: "programming"
tags:
  - "javascript"
  - "typescript"
description: "I confirmed basic calculation performance by TypeScript in this entry.  used. I measured calculation time of addition, subtraction, multiplication, division and square root. These results surprises me a little bit because it is much faster than what I thought. And they are almost same. "
socialImage: ""
---
I confirmed basic calculation performance by TypeScript in this entry. The following table is about system and hardware spec which I used. I measured calculation time of addition, subtraction, multiplication, division and square root. These results surprises me a little bit because it is much faster than what I thought. And they are almost same. I got good impression for them to develop numerical application with JavaScript.

Anyway, I need to know why the 5 arithmetic operation can have almost performance with JavaScript... 

**Systems:**

| system     | version              |
| ---------- | -------------------- |
| Node.js    | 10.15.3              |
| TypeScript | 3.5.1                |
| OS         | Windows 10 Pro 64bit |

**Hardware specification:**

| hardware | spec.                                      |
| -------- | ------------------------------------------ |
| CPU      | Intel Core i7-8550U CPU @ 1.80GHz, 1.99GHz |
| RAM      | 16GB                                       |
| Storage  | SSD                                        |
| GPU      | Intel UHD graphics 620                     |


**Performance:**

| the number of calculations | 1,000 | 10,000 | 100,000 | 1,000,000 | 10,000,000 | 100,000,000 |
| -------------------------- | ----- | ------ | ------- | --------- | ---------- | ----------- |
| add                        | 0.15  | 0.20   | 0.25    | 0.80      | 6.65       | 62.15       |
| sub                        | 0.10  | 0.15   | 0.25    | 0.85      | 7.65       | 62.40       |
| mul                        | 0.15  | 0.15   | 0.25    | 0.75      | 7.10       | 64.10       |
| div                        | 0.15  | 0.20   | 0.30    | 0.95      | 6.80       | 61.50       |
| sqrt                       | 0.25  | 0.25   | 0.30    | 0.90      | 7.70       | 70.80       |

`gist:3bc1d8e50d7720c57b4d014aad02914d`