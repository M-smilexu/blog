---
title: CSS (Cascading Style Sheet) 基础
date: 2015-02-14 14:28:17
categories: [coding, css]
description: CSS（层叠样式表）,级联样式表是一种用来表现HTML（标准通用标记语言的一个应用）或XML（标准通用标记语言的一个子集）等文件样式的计算机语言。相对于传统HTML的表现而言，CSS能够对网页中的对象的位置排版进行像素级的精确控制。
---

### CSS规则的优先级关系

优先级可以精确计算:

!important | Style属性 | #ID | .Class/伪类/属性选择器 | 标签
---- | --- | --- | --- | ---
0 | 0 | 0 | 0 | 0

同值，按规则定义的先后顺序。

避免用!important和标签的style属性。

### 层叠和继承

HTML:

    <div class="list list-member">
        <ul>
            <li>Member ID1</li>
            <li>Member ID2</li>
            <li>Member ID3</li>
        </ul>
    </div>

CSS:

    .list li {
        margin: 10px 0 0 0;
        font-size: 14px;
        color: #666;
    }

    .list-member li {
        font-size: 12px;
    }

根据CSS优先级将两条规则加在一起。实际为：

    margin: 10px 0 0 0;
    font-size: 12px;
    color: #666;

继承，子元素可以继承父级的属性:

* 文本样式: 
    * `font-family`
    * `font-size`
    * `font-style`
    * `font-variant`
    * `font-weight`
    * `letter-spacing`
    * `line-height`
    * `text-align`
    * `text-indent`
    * `text-transform`
    * `word-spacing`
* 列表样式:
    * `list-style-image`
    * `list-style-position`
    * `list-style-type`
    * `list-style`
* 颜色: `color`
* 字号是相对值：如 `font-size:80%` ，会计算相对值

[查看更多信息](http://kejun.github.io/bootcamp_htmlcss/?12)
