## C++ 笔记

#### 定义在A.cpp文件中的方法，想要B.cpp文件中使用，不能用include引入A.cpp文件，而是用两种方法：

- 将A.cpp中定义的方法在B.cpp中声明一下，即可使用。编辑器会自己找到对应的定义。
- 创建一个A.hpp，将A.cpp中定义的方法在A.hpp中声明，然后将A.hpp在B.cpp中用include引入。

####  #inlucde<A.hpp>文件只是单纯将A.hpp文件中的内容粘贴到inlcude的位置。

```c++
# A.hpp
}
```

```c++
# A.cpp

#include <iostream>
int main()
{
	std::cout << "hah";
#include <A.hpp> //这时A.hpp中的 "}"会被粘贴到这里
```

#### 编译和链接

编译会将每个cpp文件转换为obj文件 `ctrl + F7`

链接会将各个obj文件组合起来 (build、`ctrl+F5`)

build操作会先进行编译，再进行链接

1. **编译错误（Compilation Errors）**：
   - 编译错误发生在编译阶段，即在源代码被转换为可执行文件之前。
   - 编译器在尝试编译源代码时会检测到语法错误、语义错误或类型错误等问题，并生成错误消息。
   - 典型的编译错误包括语法错误（如拼写错误、缺少分号等）、类型错误（如将整数值赋给指针变量等）、未声明的标识符使用等。
2. **链接错误（Linker Errors）**：
   - 链接错误发生在编译之后，链接器在将各个目标文件合并成可执行文件时出现了问题。
   - 链接错误通常是由于符号未定义、符号重复定义、库文件缺失等原因导致的。
   - 典型的链接错误包括找不到符号的定义、找到了多个符号的定义、使用的库文件不存在或不完整等。

#### 头文件中定义的方法被多个cpp文件调用

因为用include调用hpp文件相当于直接将对应hpp文件中的内容粘贴到指定位置，所以如果有多个cpp文件同时引用一个hpp文件，那么定义在这个hpp文件中的方法就会被多次定义，报链接错误。

解决方法：

1. 在hpp中定义的方法前加上static标识符

   ```c++
   \\ a.hpp
   static void log()
   {
   
   }
   ```

   此时，多个cpp同时引用a.hpp，每个cpp中的log函数对其它cpp编译成的obj文件都是不可见的。

2. 在hpp中定义的方法前加上inline标识符

   ```c++
   \\ a.hpp
   inline void log()
   {
   
   }
   ```

3. 不在hpp中定义方法，hpp之进行声明，对应的cpp中定义

#### 声明float类型变量

```c++
float a = 1.1; //实际上定义的是一个double类型的变量
float b = 1.1f; //加上f或者F后才是定义的float类型的变量
```

#### sizeof xxx 查询xxx占用的字节数

#### 函数声明与定义

- 在 C++ 中，函数**可以在多个地方进行声明**，但在同一个作用域内，函数只能有一个定义。允许在多个文件中对函数进行声明，但**只能在一个文件中对函数进行定义**。

- 函数的声明必须在使用之前。函数的定义可以在使用之后。

#### #pragma once

- 所有以#开头的命令都是预处理指令

- #pragma once 表示：若以此命令开头的头文件在一个cpp文件中被多次include，那么只会include一次。（可以防止重复声明定义）

  ```c++
  // 也可以使用下面的方式来代替 #pragma once  (更推荐使用#pragma once)
  // 如果定义了_LOG_H,那么就跳过下面的代码
  #ifndef _LOG_H
  #define _LOG_H
  
  void Log();
  
  struct Player{};
  
  #endif
  ```

  

#### 调试

在程序想要中断的地方打断点，然后在debug下运行即可。

![image-20240228192227970](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240228192227970.png)

1. 逐条语句往下执行，遇到函数调用就进入。

2. 逐条语句往下执行，遇到函数调用**不进入**。

3. 跳出当前黄色箭头所在的函数

   ![image-20240228193943996](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240228193943996.png)

- 这个按钮可以直接跳到下一个断点处

#### if语句

- if语句会使程序变慢（因为cpu会进行比较和跳转操作）

#### vs项目设置

![image-20240228201203194](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240228201203194.png)

- 红框中的叫做过滤器，在实际的磁盘中并不存在，只是为了过滤区分项目中文件。

  ![image-20240228201323668](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240228201323668.png)

点击这个可以显示项目中实际的文件结构。

![image-20240228201530851](https://raw.githubusercontent.com/poinne/md-pic/main/image-20240228201530851.png)

实际写代码时可以使用实际的文件结构，然后自己创建src，lib等文件夹。

#### 引用&

```c++
//1.指针传递
void Increment(int* value)
{
	(*value)++;
}

int main()
{
	int a = 5;
	Increment(&a);

}//2.引用传递（效果和1是一样的）
void Increment(int& value)
{
	value++;
}

int main()
{
	int a = 5;
	Increment(a);

}
```

- 一个引用类型的变量不能更改指向的值。

  ```c++
  int a = 5;
  int b = 8;
  int& ref = a;//创建一个对a变量的引用
  ref = b;//这行是不对的，相当于将b的值赋值给a（引用类型赋值后不会改变，所以ref一直代表a）
  ```

#### 类和结构体

- 没有什么本质区别。

- 类的成员变量默认是私有的，结构体的成员变量默认是公开的。除此之外没有区别。

- 如果只是为了表示一种结构，将几个变量组织起来方便使用，推荐使用strcut

- 如果为了表示一个对象，包括对象的属性和对应的操作，推荐使用class

- 如果需要继承的话，推荐使用class

- 在一个类中，public和private可以写多次，比如下面这样：

  ```
  class Log
  {
  // 定义公共变量
  public:
  	const int LogLevelError = 0;
  	const int LogLevelWarning = 1;
  	const int LogLevelInfo = 2;
  // 定义私有变量
  private:
  	int m_LogLevel;
  
  // 定义公共方法
  public:
  	void SetLevel(int level)
  	{
  		m_LogLevel = level;
  	}
  };
  ```

#### linux

- wsl -l -v 查看wsl状态
- rm -rf * 强制删除当前目录下所有文件
-  tree -L 1 树形结构展示目录

#### cmake [文档](https://subingwen.cn/cmake/CMake-primer/)

- 基础结构

  ```txt
  cmake_minimum_required(VERSION 3.1)
  
  // 项目名称可以随便写？
  project(test)
  
  add_executable(TEST test1.cpp)
  ```

- CMake 使用 # 进行行注释，可以放在任何位置。

- add_executable

  ```
  add_executable(TEST test1.cpp) 这里的TEST是最终生成的可执行文件名称
  ```

- set命令 

  - 用于定义变量
  - set( )命令里面只能是字符串
  - 使用定义过的变量：${value}

  ```
  # 方式1: 各个源文件之间使用空格间隔
  # set(SRC_LIST add.c  div.c   main.c  mult.c  sub.c)
  
  # 方式2: 各个源文件之间使用分号 ; 间隔
  set(SRC_LIST add.c;div.c;main.c;mult.c;sub.c)
  add_executable(app  ${SRC_LIST})
  ```

  

#### openmesh操作基础属性 [博客](https://blog.csdn.net/morgan777/article/details/108694441)

- 获取网格中的标准属性：

  ```c++
  mesh.normal(*v_it)
  mesh.normal(*f_it)
  mesh.point(*v_it)
  ...
  ```

- `.norm()`、`.normalize()` 和 `.normalized()` 分别是用于处理向量的方法，具体功能如下：
  1. **`.norm()`**：
     - `.norm()` 方法用于计算向量的长度（或称为模长、范数），返回一个标量值。对于二维向量 `(x, y)`，其长度为 `sqrt(x^2 + y^2)`；对于三维向量 `(x, y, z)`，其长度为 `sqrt(x^2 + y^2 + z^2)`。
     - 该方法用于获取向量的长度，但不会修改向量本身。
  2. **`.normalize()`**：
     - `.normalize()` 方法用于将向量归一化，即将向量的长度归一为 1，从而生成一个与原向量方向相同的单位向量。具体实现是将每个分量除以向量的长度。
     - 该方法会修改原始向量，将其转换为单位向量。
  3. **`.normalized()`**：
     - `.normalized()` 方法与 `.normalize()` 类似，用于返回向量的单位向量，但不会修改原始向量，而是返回一个新的单位向量。
     - 该方法不会修改原始向量，而是返回一个新的单位向量。

- **request_face_normals() 与update_face_normals()**

  `request_face_normals()` 方法用于请求计算网格的面法线。在 OpenMesh 中，面法线不是默认计算的，而是需要显式请求。这是因为计算面法线可能需要耗费一定的计算资源，而并不是所有的应用场景都需要使用到面法线。因此，OpenMesh 提供了这个方法来灵活地控制是否计算面法线。

  在调用 `update_face_normals()` 方法之前，通常需要先调用 `request_face_normals()` 方法。这是因为 `update_face_normals()` 方法需要基于请求的信息来执行相应的计算。如果在调用 `update_face_normals()` 前没有调用 `request_face_normals()`，则 OpenMesh 将不知道您希望计算哪些数据，因此无法正确地计算面法线。

  因此，为了确保正确地计算面法线，通常需要先调用 `request_face_normals()` 来请求计算，然后再调用 `update_face_normals()` 来实际进行计算并更新数据。