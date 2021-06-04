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

            // 不分页
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

            // 分页处理
            // 一页pdf显示html页面生成的canvas高度;
            let pageHeight = contentWidth / 592.28 * 841.89
            // 未生成pdf的html页面高度
            let leftHeight = contentHeight
            // pdf页面偏移
            let position = 0
            // a4纸的尺寸[595.28,841.89]，html页面生成的canvas在pdf中图片的宽高
            let imgWidth = 595.28
            let imgHeight = 592.28 / contentWidth * contentHeight

            // eslint-disable-next-line
            let PDF = new jsPdf('', 'pt', 'a4')
            // 有两个高度需要区分，一个是html页面的实际高度，和生成pdf的页面高度(841.89)
            // 当内容未超过pdf一页显示的范围，无需分页
            if (leftHeight < pageHeight) {
                PDF.addImage(pageData, 'JPEG', 0, 0, imgWidth, imgHeight)
            } else {
                while (leftHeight > 0) {
                    PDF.addImage(pageData, 'JPEG', 0, position, imgWidth, imgHeight)
                    leftHeight -= pageHeight
                    position -= 841.89
                    // 避免添加空白页
                    if (leftHeight > 0) {
                        PDF.addPage()
                    }
                }
            }

            // 下载文件标题
            const fileTitle = 'demo';
            PDF.save(fileTitle + '.pdf');
        })

        };

        export default html2pdf;

    }
