---
layout: post
title: "jspdf下载导出pdf格式"
subtitle: "jspdf下载导出pdf格式"
date: 2021-06-04 10:00:00
author: "Bin"
catalog: false
header-style: text
tags:
  - jspdf
---

##### 实现导出 pdf

> 通过 html2canvas 把 DOM 转化为 canvas, 获取 canvas 的宽高，再将 canvas 转化为图片。
> 利用 jspdf，实例化 jspdf，将内容图片放到 pdf 中，实现导出。

    代码如下:

    1. 安装html2canvas, jspdf
    2. 利用html2canvas生成canvas
    3. 实例化jspdf

    {
        import html2canvas from 'html2canvas';
        import jsPdf from 'jspdf';

        const html2pdf = function(el) {
        const scale = 2;
        html2canvas(el, {
            allowTaint: true, // 跨域
            scale, // 画质
            background: "#FFFFFF",
            onpreloaded: function(){
                el.parentNode.style.overflow = 'visible'; // 当内容溢出时，通过onpreloaded钩子实现
            },
            onparsed: function(){
                el.parentNode.style.overflow = 'hidden';
            },
        }).then(canvas => {
            // 内容的宽度
            let contentWidth = canvas.width;
            // 内容高度
            let contentHeight = canvas.height;
            // canvas转图片数据
            let pageData = canvas.toDataURL('image/jpeg', 1.0);

            // 设置pdf的尺寸，pdf要使用pt单位 已知 1pt/1px = 0.75 pt = (px/scale)* 0.75
            // 2为上面的scale 缩放了2倍
            let pdfX = (contentWidth + 10) / scale * 0.75
            let pdfY = (contentHeight + 60) / scale * 0.75 // 60为底部留白

            // 设置内容图片的尺寸
            let imgX = pdfX;
            let imgY = (contentHeight / scale * 0.75); // 内容图片这里不需要留白的距离

            // 初始化jspdf 第一个参数方向：默认''时为纵向，第二个参数设置pdf内容图片使用的长度单位为pt，第三个参数为PDF的大小，单位是pt
            let PDF = new jsPdf('', 'pt', [pdfX, pdfY])

            // 将内容图片添加到pdf中，因为内容宽高和pdf宽高一样，就只需要一页，位置就是 0,0
            PDF.addImage(pageData, 'jpeg', 0, 0, imgX, imgY);
            // 下载文件标题
            const fileTitle = 'demo';
            PDF.save(fileTitle + '.pdf');
        })

        };

        export default html2pdf;

    }
