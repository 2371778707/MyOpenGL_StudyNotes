## Charpter3中使用的函数

**glutInit()**
	 
	负责初始化 GLUT 库
**glutInitDisplayMode()**

	设置了程序所使用的窗口的类型
**glutInitWindowPosition()**

	设置窗口位置

**glutInitWindowSize()**

	设置所需的窗口大小

**glutCreateWindow()**

	它的功能和它的名字一致
**glutDisplayFunc()**

	设置显示回调（ display callback）
**glutMainLoop()** 

	无限循环



## 初始化顶点数组对象 ##
   

	void glGenVertexArrays(GLsizei n, GLuint arrays);

返回 n 个未使用的对象名到数组 arrays 中，用作**顶点数组对象**。返回的名字可以用来分配更
多的缓存对象，并且它们已经使用未初始化的顶点数组集合的默认状态进行了数值的初始化。
	
	void glBindVertexArray(GLuint array);
如果输入的变量array非 0，并且是**glGenVertexArrays()** 所返回的，那么它将创建一个新的顶点数组对象并且与其名称关联起来。

如果绑定到一个已经创建的顶点数组对象中，那么会激活这个顶点数组对象，并且直接影响对象中所保存的顶点数组状态。

如果输入的变量array 为 0，那么 OpenGL 将不再使用程序所分配的任何顶点数组对象，并且将渲染状态重设为顶点数组的默认状态。

如果array不是**glGenVertexArrays()** 所返回的数值，或者它已经被**glDeleteVertexArrays()** 函数释放了，那么这里将产生一个**GL _ INVALID_OPERATION**错误。

<br>

    void glDeleteVertexArrays(GLsizei n, GLuint *arrays);
	
删除n个在arrays中定义的顶点数组对象，这样所有的名称可以再次用作顶点数组。

如果绑定的顶点数组已经被删除，那么当前绑定的顶点数组对象被重设为 0（类似执行了**glBindBuffer()** 函数，并且输入参数为 0），而默认的顶点数组会变成当前对象。

在arrays当中未使用的名称都会被释放，但是当前顶点数组的状态不会发生任何变化。

	GLboolean glIsVertexArray(GLuint array);
如果 array 是一个已经用 **glGenVertexArrays()** 创建且没有被删除的顶点数组对象的名称，那么返回 GL_TRUE。

如果 array 为 0 或者不是任何顶点数组对象的名称，那么返
回 GL_FALSE。



## 分配顶点缓存对象 ##
	void glGenBuffers(GLsizei n, GLuint *buffers);
返回 n 个当前未使用的缓存对象名称，并保存到 buffers 数组中。

返回到 buffers 中的名称不一定是连续的整型数据。

这里返回的名称只用于分配其他缓存对象，它们在绑定之后只会记录一个可用的状态。

0 是一个保留的缓存对象名称，**glGenBuffers()** 永远都不会返回这个值的缓存对象。

	void glBindBuffer(GLenum target, GLuint buffer);
指定当前激活的缓存对象。 

target 必须设置为以下类型中的一个：

    GL_ARRAY_BUFFER
	GL_ELEMENT_ARRAY_BUFFER
	GL_PIXEL_PACK_BUFFER
    GL_PIXEL_UNPACK_BUFFER
    GL_COPY_READ_BUFFER
    GL_COPY_WRITE_BUFFER 
    GL_TRANSFORM_FEEDBACK_BUFFER
    GL_UNIFORM_BUFFER
buffer 设置的是要绑定的缓存对象名称。

glBindBuffer() 完成了三项工作： 

1）如果是第一次绑定 buffer，且它是一个非零的无符号整型，那么将创建一个与该名称相对应的新缓存对象。 

2） 如果绑定到一个已经创建的缓存对象，那么它将成为当前被激活的缓存对象。

3）如果绑定的 buffer 值为 0，那么OpenGL 将不再对当前 target 应用任何缓存对象。

	void glDeleteBuffers(GLsizei n, const GLuint *buffers);
删除 n 个保存在 buffers 数组中的缓存对象。被释放的缓存对象可以重用（例如，使用 **glGenBuffers()**）。

如果删除的缓存对象已经被绑定，那么该对象的所有绑定将会重置为默认的缓存对象，即相当于用 0 作为参数执行 **glBindBuffer()**的结果。

如果试图删除不存在的缓存对象，或者缓存对象为 0，那么将忽略该操作（不会产生错误）

	GLboolean glIsBuffer(GLuint buffer);
如果 buffer 是一个已经分配并且没有释放的缓存对象的名称，则返回 GL_TRUE。

如果 buffer 为 0 或者不是缓存对象的名称，则返回 GL_FALSE。

## 将数据载入缓存对象 ##
	void glBufferData(GLenum target, GLsizeiptr size, const GLvoid *data, GLenum usage);
在 OpenGL 服务端内存中分配 size 个存储单元（通常为 byte），用于存储数据或者索引。

如果当前绑定的对象已经存在了关联的数据，那么会首先删除这些数据。

对于顶点属性数据， target 设置为 **GL _ ARRAY _ BUFFER**；

索引数据为  **GL _ ELEMENT _ ARRAY _ BUFFER** ； 

OpenGL的像素数据为  **GL _ PIXEL _ UNPACK _ BUFFER** ； 

对于从OpenGL中获取的像素数据为 **GL _ PIXEL _ PACK _ BUFFER** ；

对于缓存之间的复制数据为 **GL _ COPY _ READ _ BUFFER** 和 **GL _ COPY _ WRITE _ BUFFER**；
 
对于纹理缓存中存储的纹理数据为 **GL _ TEXTURE _ BUFFER**；

对于通过 transform feedback 着色器获得的结果设置为 **GL _ TRANSFORM _ FEEDBACK _ BUFFER**；

而一致变量要设置为 **GL _ UNIFORM _ BUFFER**。

size 表示存储数据的总数量。

这个数值等于 data 中存储的元素的总数乘以单位元素存储空间的结果。

data 要么是一个客户端内存的指针，以便初始化缓存对象，要么是 **NULL**。如果传入的指针合法，那么将会有 size 大小的数据从客户端拷贝到服务端。

如果传入 **NULL**，那么将保留 size 大小的未初始化的数据，以备后用。

<font color="blue" size=5px>**usage**</font> 用于设置分配数据之后的读取和写入方式。

可用的方式包括 

    GL_STREAM_DRAW
	GL_STREAM_READ
	GL_STREAM_COPY 
	GL_STATIC_DRAW 
	GL_STATIC_READ 
	GL_STATIC_COPY 
	GL_DYNAMIC_DRAW 
	GL_DYNAMIC_READ
	GL_DYNAMIC_COPY

如果所需的 size 大小超过了服务端能够分配的额度，那么 **glBufferData()** 将产生一
个 **GL _ OUT _ OF _ MEMORY** 错误。

如果 usage 设置的不是可用的模式值，那么将产生
**GL _ INVALID _ VALUE** 错误

## 初始化顶点与片元着色器 ##
	void glVertexAttribPointer(GLuint index,GLint size,GLenum type,GLboolean normalized,GLsizei stride, const GLvoid *pointer)

设置 index（着色器中的属性位置）位置对应的数据值。 

pointer 表示缓存对象中，从起始位置开始计算的数组数据的偏移值（假设起始地址为 0），使用基本的系统单位
（ byte）。 

size表示每个顶点需要更新的分量数目，可以是 1、2、3、4 或者 GL_BGRA。

type指定了数组中每个元素的数据类型

    GL_BYTE
    GL_UNSIGNED_BYTE 
    GL_SHORT
    GL_UNSIGNED_SHORT
    GL_INT
    GL_UNSIGNED_INT
    GL_FIXED
    GL_HALF_FLOAT
    GL_FLOAT
    GL_DOUBLE
    
normalized 设置顶点数据在存储前是否需要进行归一化

（或者使用 <b>glVertexAttribFourN*()</b> 函数）。 

stride 是数组中每两个元素之间的大小偏移值（ byte）。

如果 stride 为 0，那么数据应该紧密地封装在一起。

	void glEnableVertexAttribArray(GLuint index);
	void glDisableVertexAttribArray(GLuint index);
设置是否启用与 index 索引相关联的顶点数组。 

index 必须是一个介于 0 到 GL _ MAX _ VERTEX _ ATTRIBS - 1之间的值。


<hr>

函数暂时介绍到此！
