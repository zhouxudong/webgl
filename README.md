#webgl study

## webgl基础
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
