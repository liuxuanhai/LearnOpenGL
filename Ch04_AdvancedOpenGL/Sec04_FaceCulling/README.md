# OpenGL学习笔记（二十四）—— Face Culling

---


## 环绕顺序
当定义一组三角形顶点时，通常会以特定的环绕顺序（顺时针(Clockwise)或逆时针(Counter-clockwise)的）来定义；这样每组组成三角形图元的三个顶点就包含了一个**环绕顺序**。`OpenGL` 在渲染图元的时候将使用这个信息来决定一个三角形是一个正向三角形还是背向三角形。（默认情况下，**逆时针** 顶点所定义的三角形将会被处理为 **正向** 三角形。）*实际的环绕顺序是在光栅化阶段进行的，也就是顶点着色器运行之后。*

观察者所面向的所有三角形顶点就是所指定的正确环绕顺序了，而立方体另一面的三角形顶点则是以相反的环绕顺序所渲染的。所以，所面向的三角形将会是正向三角形，而背面的三角形则是背向三角形，如图：

![图片来源于：learnopengl.com](FacecullingFrontback.png)

> 上图中，两个三角都是以形逆时针顺序定义的；但是，在渲染阶段，从观察者的方向来看，背面的三角形将会是以顺时针顺序渲染的，这正是将要剔除（丢弃）的不可见的面！


## 面剔除
**面剔除(Face Culling)：**`OpenGL` 能够检查所有面向(Front Facing)观察者的面，并渲染它们，而丢弃那些背向(Back Facing)的面，从而节省很多的片段着色器调用（开销很大）；而可以通过分析顶点数据的 `环绕顺序(Winding Order)` 来区分那些是正向面，那些是背向面。

> OpenGL能够丢弃那些渲染为背向三角形的三角形图元。

### 启用面剔除
要想启用面剔除，需要启用 `OpenGL` 的 **GL_CULL_FACE**选项：

``` C
glEnable(GL_CULL_FACE);
```

### 面剔除设置
1. 通过调用 **glCullFace(GLenum mode);** 来改变需要剔除的面的类型，其允许设置的类型有三个选项：

	| 选项 | 值 |
	| -------- | -------- |
	| `GL_BACK` | 只剔除背向面(初始默认值) |
	| `GL_FRONT` | 只剔除正向面 |
	| `GL_FRONT_AND_BACK` | 剔除正向面和背向面 |
2. 通过调用 **glFrontFace(GLenum mode);** 来将 `顺时针`的面还是 `逆时针` 的面定义为正向面；其允许设置的类型有两个选项：

	| 选项 | 值 |
	| -------- | -------- |
	| `GL_CCW ` | 逆时针(Counter-Clockwise)的环绕顺序 |
	| `GL_CW ` | 顺时针(Clockwise)环绕顺序 |


---


# 参考
教程来源：[https://learnopengl.com/](https://learnopengl.com/Advanced-OpenGL/Face-culling)。