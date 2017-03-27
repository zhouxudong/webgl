# webgl study

## WebGL基础
### WebGL坐标系统
    WebGL处理的是三维图形，所以它使用三维坐标系统，具有X轴、Y轴、Z轴。
    X轴是正方向为右，Y轴的正方向向下，Z轴的正方向向外（右手坐标）
    中心点（0.0, 0.0, 0.0)
    上边缘和下边缘（-1.0, 0.0, 0.0） 和 （1.0, 0.0, 0.0）
    左边缘和右边缘（0.0, -1.0, 0.0)  和 （0.0, 1.0, 0.0)
### 设置canvas背景色
    /*
    * 指定背景色（一旦指定背景色之后，背景色会常驻WebGL系统，直到下一次调用gl.clearColor）
    * red:   指定红色值（0.0 - 1.0);
    * green: 指定绿色值（0.0 - 1.0);
    * blue:  指定蓝色值（0.0 - 1.0);
    * alpha: 指定透明度（0.0 - 1.0);
    * 任何小于0.0 或者大于1.0, 都会分别截断为 0.0 或 1.0
    * default: (0.0, 0.0, 0.0, 0.0);
    */
    gl.clearColor(red, green, blue, alpha);

### 清空 canvas
    /*
    * 用之前指定的背景色清空绘图区域
    * buff: 指定待清空的缓冲区，位操作符 OR(|) 可用来指定多个缓冲区
    * gl.COLOR_BUFFER_BIT:  指定颜色缓冲区
    * gl.DEPTH_BUFFER_BIT:  指定深度缓存区
    * gl.STENCIL_BUFFER_BIT:指定模板缓冲区
    */
    gl.clear(gl.COLOR_BUFFER_BIT);
### 着色器
    //要使用WebGL进行绘图就必须使用着色器，在代码中，着色器程序以字符串的
    //形式"嵌入"在Javascript文件中，在程序真正开始运行前它就已经设置好了

    * 顶点着色器（Vertex shader): 顶点着色器是用来描述顶点特性（如位置、颜色等）
      的程序。 顶点（Vertex）是指二维或三维空间中的一个点，比如 顶点或交点

    * 片元着色器（Fragment shader): 进行逐片元处理过程（如光照）的程序。
      片元（fragment)是一个WebGL术语，可以理解为像素（图像的单元）

    //GLSE中的数据类型
    float: 表示浮点数
    vec4: 表示由四个浮点数组成的矢量（x,y,z,w）
    （x,y,z,w）等价于三维坐标 (x/w, y/w,z/w), 如果w = 1.0,就可以当作三维坐标使用。
     w的值必须是大于等于0的。

    //顶点着色器内置变量
    vec4  gl_Position  表示顶点位置
    float gl_PointSize 表示点的尺寸（像素数）

    //片元着色器内置变量
    vec4 gl_FragColor  指定片元颜色（RGBA格式）
### 绘制操作
    /*
    * mode  指定绘制的方式： gl.POINTS, gl.LINES, gl.LINE_STRIP, gl.LINE_LOOP, gl.TRIANGLES,
    *                      gl.TRIANGLE_STRIP, gl.TRIANGLE_FAN
    * fist  指定从哪个顶点开始绘制 （整数）
    * count 指定绘制需要用到多少个顶点（整数）
    */
    gl.drawArrays(mode, first, count);
### 使用attribute变量动态绘制
    attribute变量是一个GLSL ES变量，被用来从外部向顶点着色器内传输数据，只有顶点着色器能用它；
    使用步骤：
    1：在顶点着色器中，声明attribute变量；
    2: 将attribute变量赋值给gl_Position变量；
    3: 向attribute变量传输数据

    /*
    * program  指定包含顶点着色器和片元着色器的着色器程序对象
    * name     指定想要获取其存储地址attribute变量的名称
    */
    gl.getAttribLocation(program, name);

    /*
    * 将数据（v0,v1,v2）传给由location参数指定的attribute变量
    */
    gl.vertexAttrib3f(location, v0, v1, v2);
### 使用缓冲区对象
    1. 创建缓冲区对象（gl.createBuffer()）.
    2. 绑定缓冲区对象（gl.bindBuffer()).
    3. 将数据写入缓冲区对象（gl.bufferData()).
    4. 将缓冲区对象分配给一个attribute变量（gl.vertexAttribPointer()).
    5. 开启attribute变量 （gl.enableVertexAttribArray()).

#### 使用缓冲对象
    var vertexBuffer = gl.createBuffer();

#### 绑定缓冲对象
    //允许使用buffer表示的缓冲区对象，并将其绑定到target表示的目标上
    ／*
    * target可以是
    *    gl.ARRAY.BUFFER: 表示缓冲区对象中包含了顶点的数据
    *    gl.ELEMENT_ARRAY_BUFFER： 表示缓冲区对象包含了顶点的索引值
    * buffer： 指定之前由gl.createBuffer()返回的待绑定的缓冲区对象
    *
    *／
    gl.bindBuffer(target, buffer);

#### 向缓冲区对象写入数据

     /*
     * target:  gl.ARRAY_BUFFER 或 gl.ELEMENT_ARRAY_BUFFER
     * data: 写入缓冲区对象的数据
     * usage: 表示程序将如何使用存储在缓冲区对象中的数据
     *   gl.STATIC_DRAW: 只会向缓冲区对象写入一次数据，但需绘制多次
     *   gl.STREAM_DRAW: 只会向缓冲区对象中写入一次数据，然后绘制若干次
     *   gl.DYNAMIC_DRAW: 会向缓冲区对象中多次写入数据，并绘制多次。
     */
     gl.bufferData(target, data, usage)；

#### 将缓冲区对象分配给attribute变量
     /*
     * location  :指定待分配attribute变量的存储位置
     * size：    指定缓冲区每个顶点的分量个数（1-4）。若size比attribute变量需要的分数小，缺失默认补全（2，3 默认0，4默认1）
     * type：    用以下类型之一来指定数据格式：
     *           gl.FLOAT:  浮点型                 gl.UNSIGNED_BYTE: 无符号字节，Uint8Array
     *           gl.SHORT  :       短整型          gl.UNSIGNED_SHORT:  无符号短整型
     *           gl.INT: 整型                      gl.UNSIGNED_INT: 无符号整型
     * normalize： true 或 false，表明是否将非浮点型的数据归一化到[0,1]或[-1,1]区间
     * stride:   指定相邻两个顶点间的字节数，默认为0
     * offset:   指定缓冲区对象中的偏移量,即attribute变量从缓冲区中的何处开始存储。其实位置为0。
     */
     gl.vertexAttribPointer(location, size, type, normalized, stride, offset)

#### 开启attribute变量
    //开启location指定的attribute变量
    gl.enableVertexArray(location);
    //可以用gl.disableVertexArray(location) 关闭location指定的attribute变量
