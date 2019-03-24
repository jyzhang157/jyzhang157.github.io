---
layout: post
title:  "课程项目-安卓开发学习笔记"
date:   2019-03-24 +0800
---
>课程资料来自udacity 《Android基础》
> 课程将讲授三个部分：
> 1. 视图（select views）
> - 复选框(check box)
> - 单选按钮(radio button)
> - 图片(image)
> - 某种形式的文本
> 2. 自定义视图(style views)
> 更改图片文本的颜色或者大小
> 3. 布置视图(position views)

## 视图
**视图是屏幕上可见的矩形区域。视图具有宽度和高度，有时还具有背景色。**
> [链接](https://developer.android.com/guide)是开发过程中需要使用的文档

> [链接](https://material.io/design/typography/#applying-the-type-scale)提供了许多辅助设计的排版指南 

插图显示了三种不同类型的视图。
- ImageView显示图像，如图标或照片。
- TextView 显示文本。  
Button是对触摸敏感的。  
TextView：用手指点击时即会做出响应。
- ViewGroup 是大视图，通常不可见，其内部包含并可放置较小视图。

视图是构建应用视图的基石

视图名称一般用驼峰式命名（单词之间没有空格，首字母大写）

## XML(一种Extensible markable language)
**XML 代表“可扩展标记语言”。它是一种表示法，用于编写以层次结构或家族树形式组织的信息。**

示例包括主题的罗马数字轮廓、部门和分支的企业组织结构图或州、县和市的列表。  
一个州可以包含许多县，且一个县可以包含许多市。但是每个市只能属于一个县，且每个县只能属于一个州。

在 XML 中，我们称每个数据项目可包含许多子项，但每个子项只能包含在一个父项中。
```
代码示例
<LinearLayout android:layout_width="match_parent"
android:layout_height="match_parent" 
android:orientation="vertical">
<TextView
android:layout_width="match_parent"
android:layout_height="wrap_content" 
android:text="Hello"/> 
<ImageView android:layout_width="match_parent"
android:layout_height="wrap_content" 
android:src="@drawable/mountain"/>
<Button 
android:layout_width="match_parent"
android:layout_height="wrap_content" 
android:text="Press me"
android:onClick="doSomething"/>
</LinearLayout>
```
家族树结构使 XML 成为描述 Android 应用的屏幕布局的理想语言，应用的屏幕布局由称为视图的矩形区域组成。  
该布局总是以大视图包含小视图，小视图继而包含更小视图的形式存在。
### 元素
视图中TextVIew, ImageView等就是元素名称，紧跟在'<'之后  
驼峰式命名法
### 标记
编写的文件包含称为元素的信息片段。为表明元素的开始和结束，我们编写**标记**。


始终以字符 < 和 >开始和结束。标记还包括所标记开始和结束元素（如 LinearLayout）的名称。

*结束要有‘/’*
#### 自闭合标记
不需要括任何内容的元素可由单一标记组成。这种情况下，标记以字符 /> 结束，我们称这种标记为**自闭合标记**。
```
<ImageView android:layout_width="match_parent"
android:layout_height="wrap_content" 
android:src="@drawable/mountain"/>
```
#### 独立标记
```
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
</LinearLayout>
```
#### 属性(Attributes)
元素初始标记内部可写入称为属性的小块信息。每个属性都由名称和值组成。  
例如，TextView元素可能具有一个名称为 "text"、值为 "Hello" 的属性。  
我们将属性名写在左侧，值写在右侧，中间用等号连接。  
**请务必用双引号将值括上。**

决定行为或者试图效果

任何属性都有默认值，只有需要修改的属性需要明确赋值

#### dp (Density-Independent Pixel)
为在不同屏幕密度的设备间实现一致物理大小的视图，我们使用称为**与密度无关**的像素的度量单位（dp 或 dip，发音为 "dee pee" 或 "dip"）

**按照材料设计指南，屏幕上的任何触摸目标均应至少为 48dp 宽乘以 48dp 高**
```
<TextView
android:layout_width="160dp"
android:layout_height="80dp"
android:background="#00FF00"
android:text="Hello"/>
```

#### sp (Scale-Independent Pixel)
与比例无关的像素 (sp) 是用于指定**字体**类型大小的长度单位。其长度取决于用户的字体大小首选项。该首选项在 Android 设备的“设置”应用中设置。

**为尊重用户的首选项，应使用与比例无关的像素指定所有字体大小**。应在与设备无关的像素 (dp) 中给定所有其他尺寸。

> 对于字体大小的设置  
> When choosing font sizes for your app, using too many different sizes can be visually distracting and confuse the user as to what information is important. To be consistent with other apps on the platform, you can use the standard set of type sizes provided by the framework: small, medium, or large.

>To use a standard text size (and color), instead of setting the TextView's android:textSize directly, set the android:textAppearance to a predefined theme attribute [1] such as android:textAppearanceLarge:

```
<TextView
    …
    android:textAppearance="?android:textAppearanceLarge" />
``` 
   
#### 调试步骤：

1. 阅读错误消息
2. 与可行的代码做对比
3. 撤消操作
4. 寻求帮助

#### 硬编码（Hard coding）与wrap_content

##### 硬编码
计算机是按照一系列称为程序的指令运行的机器。Android设备便是计算机，应用是程序。

向应用提供信息的方式之一是将信息写入应用指令中：add 10 + 20，或使 TextView 的宽为100dp，这称为向应用中**硬编码信息**。

上述指令中的TextView 是视图示例，是屏幕上可显示信息的矩形区域。视图可以包含在较大视图中，这个大视图称为其父视图。假设包含 TextView 的父视图宽度为 100dp，若希望 TextView与父视图等宽，则可以写入上述指令。将 100dp 值用硬编码写入指令的缺点之一是，如果想要更改 TextView 父视图的宽度则必须要重新写入该值。

这是我们**避免写入硬编码值**的原因之一。如果能够写入一条指令，指示 TextView 自动从其父视图获取宽度，就可以减少我们的记忆负担。减少维护也就意味着减少程序错误。
##### wrap_content
视图是屏幕上的矩形区域，通常包含一些内容。例如，TextView 包含文本，ImageView 包含图像，而称为 ViewGroup 的特殊类型视图内部包含较小的视图。

我们可以用给定距离指定视图的宽度或高度。我们也可以将其指定为特殊值 wrap_content，以围绕其内容压缩视图。为防止视图把自身包围得过紧，我们还可以指定特定的内边距量。

**推荐使用wrap_content**
```
<TextView
android:layout_width="120dp"
android:layout_height="40dp"
android:background="#FFC300"
android:text="HELLO"/> 
<TextView
android:layout_width="wrap_content"
android:layout_height="wrap_content"
android:background="#FFC300"
android:text="HELLO"/>
```
#### Hexadecimal Color (Hex Color)（十六进制颜色）
通过按红、绿、蓝顺序将颜色混合到一起可创建各种颜色。写入井号 (#)，然后使用一对“十六进制数字”指定各成分的量。00 是最小量，FF 是最大量，80 是中间量。

使用以上三个数值创建颜色后，请访问[材料设计网站](https://material.io/design/color/#)体验更多微妙的调色。

### ImageView
```
<ImageView
    android:src="@drawable/cake"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:scaleType="center"/>
```
'@'是指这是引用资源，资源类型是drawable，cake指文件名（不需要加后缀）

需要预先添加相应的资源文件

##### 属性：scaleType:
- center(居中)
- **centerCrop**经常使用，用于生成无边框图片
