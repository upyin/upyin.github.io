---
layout: post
title: CSS filter滤镜使用
description: css filter属性滤镜的使用
tag: filter、滤镜
category: 技术、总结
---
filter属性定义了元素的可视效果，例如模糊与饱和度。

## 使用

| 描述           | 值                                          |
| -------------- | ------------------------------------------- |
| 默认值         | none                                        |
| 继承           | no                                          |
| 动画支持       | 是                                          |
| 版本           | CSS3                                        |
| JavaScript语法 | object.style.WebkitFilter="grayscale(100%)" |

## CSS语法

```bash
filter: none | blur() | brightness() | contrast() | drop-shadow() | grayscale() | hue-rotate() | invert() | opacity() | sturate() | sepia() | url();
```

提示：使用空格分隔多个滤镜。

## Filter函数

注意：滤镜通常使用百分比（如：75%），当然也可以使用小数来表示（如：0.75）。

| Filter                                             | 说明                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ |
| none                                               | 默认值，没有效果。                                           |
| blur(*px*)                                         | 给图像设置高斯模糊。"radius"一值设定高斯函数的标准差，或者是屏幕上以多少像素融在一起， 所以值越大越模糊；如果没有设定值，则默认是0；这个参数可设置css长度值，但不接受百分比值。 |
| brightness(*%*)                                    | 给图片应用一种线性乘法，使其看起来更亮或更暗。如果值是0%，图像会全黑。值是100%，则图像无变化。其他的值对应线性乘数效果。值超过100%也是可以的，图像会比原来更亮。如果没有设定值，默认是1。 |
| contrast(*%*)                                      | 调整图像的对比度。值是0%的话，图像会全黑。值是100%，图像不变。值可以超过100%，意味着会运用更低的对比。若没有设置值，默认是1。 |
| drop-shadow(*h-shadow v-shadow blur spread color*) | 给图像设置一个阴影效果。阴影是合成在图像下面，可以有模糊度的，可以以特定颜色画出的遮罩图的偏移版本。 函数接受<shadow>(在CSS3背景中定义)类型的值，除了"inset"关键字是不允许的。该函数与已有的box-shadow box-shadow属性很相似；不同之处在于，通过滤镜，一些浏览器为了更好的性能会提供硬件加速。参数如下：<offset-x><offset-y>(必须)、<blur-raduis>(可选)、<spread-radius>(可选)、<color>(可选)。 |
| grayscale(*%*)                                     | 将图像转换为灰度图像。值定义转换的比例。值为100%则完全转为灰度图像，值为0%图像无变化。值在0%到100%之间，则是效果的线性乘子。若未设置，值默认是0； |
| hue-rotate(*deg*)                                  | 给图像应用色相旋转。"angle"一值设定图像会被调整的色环角度值。值为0deg，则图像无变化。若值未设置，默认值是0deg。该值虽然没有最大值，超过360deg的值相当于又绕一圈。 |
| invert(*%*)                                        | 反转输入图像。值定义转换的比例。100%的价值是完全反转。值为0%则图像无变化。值在0%和100%之间，则是效果的线性乘子。 若值未设置，值默认是0。 |
| opacity(*%*)                                       | 转化图像的透明程度。值定义转换的比例。值为0%则是完全透明，值为100%则图像无变化。值在0%和100%之间，则是效果的线性乘子，也相当于图像样本乘以数量。 若值未设置，值默认是1。该函数与已有的opacity属性很相似，不同之处在于通过filter，一些浏览器为了提升性能会提供硬件加速。 |
| saturate(*%*)                                      | 转换图像饱和度。值定义转换的比例。值为0%则是完全不饱和，值为100%则图像无变化。其他值，则是效果的线性乘子。超过100%的值是允许的，则有更高的饱和度。 若值未设置，值默认是1。 |
| sepia(*%*)                                         | 将图像转换为深褐色。值定义转换的比例。值为100%则完全是深褐色的，值为0%图像无变化。值在0%到100%之间，则是效果的线性乘子。若未设置，值默认是0； |
| url()                                              | URL函数接受一个XML文件，该文件设置了 一个SVG滤镜，且可以包含一个锚点来指定一个具体的滤镜元素。例如：filter:url(svg-url#element-id) |
| initial                                            | 设置属性为默认值。                                           |
| inherit                                            | 从父元素继承该属性。                                         |

## 实例

### 高斯模糊

```css
img {
  -webkit-filter: blur(5px);
  filter: blur(5px);
}
```

### 给图片设置阴影

```css
img {
  -webkit-filter: drop-shadow(8px 8px 10px red);
  filter: drop-shadow(8px 8px 10px red);
}
```

### 网站变灰

```css
html {
  -webkit-filter: grayscale(100%);
  filter: grayscale(100%);
  filter: progid:DXImageTransform.Microsoft.BasicImage(grayscale=1);
}
```

### 反转图片

```css
img {
  -webkit-filter: invert(100%);
  filter: invert(100%);
}
```

