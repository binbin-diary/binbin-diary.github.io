---
layout: post
title: "Typescript入门及介绍"
subtitle: "Typescript入门及介绍"
date: 2021-05-13 11:00:00
author: "Bin"
catalog: false
header-style: text
tags:
  - Typescript
---

##### 1. 什么是 Typescript?

> 1. Typescript 是 javascript 的超集，也就是说 typescript 包含了 javascript 的内容，同时额外定义的自己的静态类型。
> 2. Typescript 不能直接在浏览器上运行，需要编译成 javascript。

##### 2. Typescript 有什么优势?

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

##### 3. Typescript 静态类型有哪些？

    1. 分为基本类型及对象类型

        // 基本类型
        const dNumber: number = 11;
        const sString: string = '22';
        const dBoolean: boolean = true;
        const dnull: null = null;
        const dundefined: undefined = undefined;

        // 对象类型
        const arr: number[] = [1, 2, 3];
        const oData: {
          name: string,
          age: number,
        } = {
          name: 'xiaoming',
          age: 22
        }

    2. implements 约束claas类

      interface User {
        name: string;
        age?: number;
      }

      // Person类，必须包含name属性
      class Person implements User {

      }

##### 4. Typescript 访问类型？

    1. public, private, proceted
    2. readonly
    3. static

##### 5. Enum 枚举类型

    enum Status {
      OFF,
      ONLINE,
      UPDATE
    }

    // Status没有赋值时值从0开始
    console.log(Status.OFF) // 0

    // 赋值时
    enum Status {
      OFF = 1,
      ONLINE,
      UPDATE
    }
    // Status对应值为1, 2, 3

    console.log(Status[1]) // OFF

    switch (status) {
      case Status.OFF:
        break;
      case Status.ONLINE:
        break;
      case Status.UPDATE:
        break;
    }

##### 6. Typescript 中特殊符号问号，及感叹号! 用法

    {
      // 表示name属性为可选
      inferface Form {
        name?: string;
      }

      // 成员，强调成员不能为空值
      // 如何在类A中name是不能为空的，使用!强调name不能为空
      class A implements Form {
        name!: string
      }

    }
