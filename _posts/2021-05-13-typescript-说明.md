---
layout:     post
title:      "Typescript入门及介绍"
subtitle:   "Typescript入门及介绍"
date:       2021-05-13 11:00:00
author:     "Bin"
catalog: false
header-style: text
tags:
  - Typescript
---

##### 1. 什么是Typescript?

> 1. Typescript是javascript的超集，也就是说typescript包含了javascript的内容，同时额外定义的自己的静态类型。
> 2. Typescript不能直接在浏览器上运行，需要编译成javascript。

##### 2. Typescript有什么优势?

    1. 开发过程中，发现潜在问题，提示报错

        {
            function a(data: { x: number, y: number }) {
              return data.x + data.y;
            }

            a() // 提示报错
            a({x: 1, y: 2 }); // 正确写法
        }

    2. 编辑器更好的友好提示，代码语义更简单清晰

        {

          interface point {
            x: number, 
            y: number
          }
          function a(data: point) {
            return data.x + data.y;
          }

        }