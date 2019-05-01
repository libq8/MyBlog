---
title: 多核程序设计（二）CUDA基础知识
permalink: 多核程序设计（二）CUDA基础知识
date: 2019-04-23 17:10:27
categories: 
- 课程笔记
- 多核程序设计
tags:  CUDA
---

多核程序设计课程第二部分

<!--more-->



## 2.1GPU架构
### 2.1.1 架构分类

* Flynn’s taxonomy
  1972	年由费林（Michael J. Flynn ）提出根据指令和数据进入CPU的方式进行分类
  基础类别包含
  SISD: 经典的冯诺依曼架构
  ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/01.png?raw=true)
  SIMD：多个处理单元使用同样的指令处理不同的数据
  ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/02.png?raw=true)
  MISD：流水线架构
  ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/03.png?raw=true)
  MIMD：多个处理单元使用不同的指令处理不同的数据，MIMD扩展类别包含SPMD,MPMD
  ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/04.png?raw=true)
### 2.1.2 CPU vs GPU
* 性能指标
  * 延迟：一个操作熊开始到完成所需的时间（ms）
  * 带宽：单位时间可处理的数据量（MB/s,GB/s）
  * 吞吐量：单位时间可成功处理的运算量（gflops）

* CPU
  * 目标：降低延迟
    * 大缓存（降低存储器延迟），强大运算器（降低运算延迟），复杂控制机制（分支预测等），线程上下文切换开销大
  * 提高串行代码的性能，更加适用于复杂的单任务
    ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/05.png?raw=true)
* GPU
  * 目标提高吞吐量
    * 小缓存（存储速度更快）节能计算器，简单的控制流机制（无分支），轻量级线程
  * 大规模并行架构，适用于大量的相似任务
    ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/06.png?raw=true)
### 2.1.3 NVIDIA GPU架构
* 二级架构
  * 每个GPU拥有多个SM，SM之间共享显存（device memory）
  * 每个SM拥有多个CUDA core，core之间共用调度器和指令缓存
    ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/07.png?raw=true)
* 线程束
  * CUDA线程以32个为一组在GPU上执行，在不同的数据上执行相同的指令（SIMT）
  * SM负责调度并执行线程束，调度时发生上下文切换

* 计算能力
  * 由硬件决定，计算能力\neq 性能

### 2.1.4 CUDA编程模型
* Host与device
  * Host（CPU相关）：运行在CPU上的代码以及主机内存
  * Device（GPU相关）：运行在GPU上的代码和显存（设备内存）
  * 通过在host端调用核函数kernel在GPU上执行

* Host与Device代码
  * \_\_host\_\_从主机端调用，在主机端执行
  * \_\_global\_\_从主机端调用，在设备端执行（只有一个？）
  * \_\_device\_\_从设备端调用，在设备端执行
  * \_\_host\_\_和\_\_device\_\_可以混用

* 核函数kernel
  * 只能访问设备内存
  * 必须返回void
  * 不支持可变数量参数
  * 不支持引用类型参数
  * 不支持静态变量
  * 执行配置（线程分配）
    * 指明网格grid和块block的维度大小
    * 形式为<<<grid, block>>>，grid和block为dim3类型
    * 大小受计算能力的限制（SM和block有着最大限制数目，内存也会影响线程分配）

* GPU架构和线程组织
  * 架构模型
  * 确定线程编号——使用内置变量threadIdx,blockIdx,blockDim
    ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/08.png?raw=true)

### 2.1.5 CUDA编程举例

* 向量相加
  * 内存管理
  * 错误处理
  * 方案一：使用单个block
    * block存在最大线程数限制1024
    * 同一个block只在一个SM中执行，没有充分利用GPU资源
  * 方案二：使用多个block


## 2.2CUDA线程


## 2.3内存结构
### 2.3.1线程组织与内存结构
例子：矩阵相加
直接将一维grid和block扩展至二维
* 需要二级指针构建二维数组
* block中线程访问的内存空间不连续，内存访问的局部性差
* 在X,Y维度上都可能出现余数，线程的利用率低

### 2.3.2 CUDA内存模型
* 每个线程thread拥有独立的寄存器、本地内存
* 每个线程块block使用同一个共享内存
* grid中所有线程共享同一个全局内存、常量内存和纹理内存
  ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/10.png?raw=true)
* CUDA变量与类型修饰符
  * 没有修饰符的变量将被置于寄存器
    * 超过寄存器限制的变量将被置于本地内存，极大地降低程序效率！
  * 没有修饰符的数组将被置于寄存器/本地内存

| 变量声明            | 存储器            | 作用域 | 生存周期 |
| ------------------- | ----------------- | ------ | -------- |
| int var;            | 寄存器            | 线程   | 线程     |
| int array_var[100]; | 寄存器/本地       | 线程   | 线程     |
| \_\_shared\_\_      | int shared_var;   | 共享   | 线程块   |
| \_\_device\_\_      | int global_var;   | 全局   | 全局     |
| \_\_constant\_\_    | int constant_var; | 常量   | 全局     |

* **GPU DRAM访问模式**
* 全局内存
  * 通过二级缓存或配置共享内存
* 常量内存
  * 通过二级缓存或线程块的常量缓存（需Kepler以后的设备）
* 只读/纹理内存
  * 通过二级缓存或线程块的只读缓存（需Kepler以后的设备）

* **存储器位置与访问开销**

| 变量声明	| 存储器	| 片上/片外	| 缓存	| 访问开销 |
| ------ | ------ | ------ | ------ | ------ |
| int var;	| 寄存器	| 片上	| NA	| 1 |
| int array_var[100];	| 本地	| 片外	| 是	| 1 （L1缓存）200-800 |
| \__shared\__ int shared_var;	| 共享	| 片上	| NA	| ~1 |
| \__device\__ int global_var;	| 全局	| 片外	| 是	| 200-800 |
| \__constant\__ int constant_var;	| 常量	| 片外	| 是	 | ~1（缓存）|

### 2.3.3 全局内存

#### 2.3.3.1 动态与静态全局内存
* **动态全局内存管理**
  cudaMalloc(), cudaMemcpy(), cudaFree()
* **静态全局内存**
  * 通过\__device__修饰符声明
  * 使用cudaMemcpyToSymbol()与cudaMemcpyFromSymbol()在主机端与设备端之间拷贝
#### 2.3.3.2 统一内存寻址（unified memory）
* **使用相同的内存地址（指针）在主机和设备上进行访问**
  * 统一内存中创建托管内存池
  * 底层系统在统一内存空间中自动在主机和设备间进行传输
  * 简化程序员视角中的内存管理
    ![](https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/11.png?raw=true)
* **优点**
  * 更容易将C代码移植到CUDA
  * 更方便数据管理
    * 不需要显式控制数据在主机与设备端的传输
    * 与操作系统管理虚拟内存方式相似
* **缺点**
  * 内存只是“虚拟统一”
    * GPU上内存依然独立于主机内存外
    * 依然需要通过PCIe或NVLINK传输数据
  * 需要适当的页面调度及同步保证内存中数据正确
  * 与显示控制内存相比效率往往更低
#### 2.3.3.3 深度拷贝
* **复杂的数据结构需要多次拷贝**
  * 如结构体，一次拷贝结构体成员变量，一次拷贝数组并动态分配内存

### 2.3.4 常量内存
* 每个SM上有专用的片上缓存
* 常量缓存中读取的延迟比常量内存中低的多
* 在运行时设置
* **使用**
  * 变量定义：使用__constant__修饰词
  * 值拷贝：使用cudaMemcpyToSymbol（与静态全局变量一致），用于少量只读数据
* **访问行为**
  * 线程束中线程访问不同地址，则访问需要串行
  * 常量内存读取成本与线程束中线程读取唯一地址数量呈线性关系
* **访问举例**
  * 方案一：
    ```
    \__constant__ int const_var[16];
    \__global__ void kernel(){
    		int i = blockIdx.x;
    		int value = const_var[i%16];
    }
    ```
    * 常量内存的最佳访问模式
      * 基于blockIdx访问
      * 所有线程访问同一内存（广播访问）
    * 无串行访问
      * 只需要一次内存读取
    * 线程块中其他线程所需数据也同样会命中缓存

  * 方案二：
     ```
     	\__global__ void kernel(){
     			int i = blockIdx.x * blockDim.x + threadIdx.x;
     			int value = const_var[i%16];
     	}
     ```
     * 常量内存的最差访问模式
       * 基于threadIdx访问
       * 线程访问多个不同内存
     * 需要串行访问
       * 需要16次内存读取
     * 线程块中其他线程所需数据可能不会命中缓存

* 常量内存 vs 宏定义
  * 宏定义由预处理器进行文字替换
    * 不占用寄存器
    * 存在于指令空间中
  * 何时使用常量内存/宏定义？
    * 宏定义中的值成为应用程序的一部分适用于编译后不再修改的值
    * 常量内存适用于在执行中可能更改的值（在GPU代码执行过程中不变）


### 2.3.5 只读/纹理内存
* 只读内存与纹理内存
  * Kepler架构中相互独立
  * 在此后架构中占用同一块内存空间（GPU DRAM）
  * 每个SM具有数个texture fetch units
* 特点
  * 数据均为只读,不能在设备端代码中修改
  * 满足空间局部性的读取更有效率(图形学代码访问纹理的特性)
* 使用
  * 通过绑定到底层内存的纹理引用读取
    * 使用内部函数__ldg()
    * \__ldg()用于代替标准指针解引用，并且强制通过只读数据缓存加载
    ```
    	\__global__ void kernel(int *buffer) {
    	int i = blockIdx.x * blockDim.x + threadIdx.x;
    	int x = \__ldg(&buffer[i]);
    }
    ```
  * 通过修饰词指示编译器使用只读内存
    * 使用全局内存的限定指针
    * 使用const \__restrict__表明数据应该通过只读缓存被访问
      ```
      \__global__ void kernel(const int* \__restrict__ buffer){
      	int i = blockIdx.x * blockDim.x + threadIdx.x;
      	int x = buffer[i];
      }
      ```
    ```
    
    ```
  * **绑定纹理内存使用**
    * 声明全局纹理引用

      * texture<Datatype, Type, ReadMode> tex;
    * 调用过程

      * 分配内存空间-绑定纹理-核函数调用-解除绑定-释放内存
    * 举例
      ```
      #define N 1024
      texture<float, 1, cudaReadModeElementType> tex;
      \__global__ void kernel() {
      	int i = blockIdx.x * blockDim.x + threadIdx.x;
      	float x = tex1Dfetch(tex, i);
      }
      int main() {
      	float *buffer;
      	cudaMalloc(&buffer, N*sizeof(float));
      	cudaBindTexture(0, tex, buffer, N*sizeof(float));
      	kernel <<<grid, block >>>();
      	cudaUnbindTexture(tex);
      	cudaFree(buffer);
      }
      – cudaBindTexture() cudaBindTexture2D()
      – cudaUnbindTexture()
      ```

* **常量缓存与只读缓存**
  * 常量缓存与之都缓存都是只读的
  * 在SM上为相互独立的硬件
  * 常量缓存更适用于统一读取
    * 线程束中的每一个线程都访问相同的地址
  * 只读缓存更适用于分散读取
    * 线程束中的每一个线程访问不同地址


### 2.3.6 共享内存
* 可编程的缓存
  * 可以显式控制载入/同步数据
  * 片上存储
  * 读写速度非常快
    * 带宽 > 1 TB/s
  * 基于线程块
    * 允许同一线程块中的线程共享部分数据
    * 无法同步不同线程块中的线程
* **基于线程块的共享内存读写模型
  * 线程块往往只需要部分数据
    * 将全局数据切分成小块
    * 在核函数中将线程块所需的一个小块数据载入共享内存
    * 运行核函数进行计算
    * 核函数结束前将数据拷贝至全局内存
* **共享内存分配**
  * 静态：编译时指定（常数、宏定义）
     ```
     	 \__global__ void kernel(int *in){
     			\__shared__ int smem[N];
     			...
     	}
     ```
  * 动态：运行时通过执行配置指定
    * 注意：\__shared__ int *ptr是共享变量（指针）而非空间本身
       ```
       	 \__global__ void kernel(int *in){
       		extern \__shared__ int smem[];
       		...
       	}
       	kernel<<<grid, block, smem_size>>>(in);
       ```
     ```
    
     ```
* 存储体（bank）和访问模式
  * 共享内存被分为32个同样大小的内存模型（存储体）
    * 不同存储体可同时被访问
  * 访问模式
    * 并行访问：多个地址访问多个存储体
    * 串行访问：多个地址访问同一存储体
    * 广播访问：单一地址读取单一存储体
  * 存储体冲突（bank conflict）
    * 多个地址访问同一个存储体（串行）
    * 广播访问不引发存储体冲突
    * 只发生在同一个线程束的线程中
      * 32个存储体与32个线程
  * 每4字节为同一存储体
  * 例子
    * 步长为1
      * 访问整型、单精度浮点数
      * 无冲突
      * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/12.png?raw=true" width=50% height=50%  />
    * 步长为2
      * 访问双精度浮点数
      * 1-way 冲突
      * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/13.png?raw=true" width=50% height=50%  />
    * 步长为3
      * 如12字节的结构体
      * 无冲突
      * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/14.png?raw=true" width=50% height=50%  />


### 2.3.7 本地内存
* 每个线程独立读写
* 并非物理存在，与全局内存在同一块存储区域
   * 计算能力2.0以上的CPU中，存储在SM的一级缓存以及设备的二级缓存
* 可能存放到本地内存的变量：
   * 编译时使用未知索引引用的本地数组
     * 可能占用大量寄存器空间的本地数组
     * 不满足寄存器限定的变量

## 2.4通信同步机制
### 2.4.1 线程执行模型
* **逻辑视图**
  * 每个线程块由一个SM执行，由硬件调度
  * 所有线程块在硬件上都是一维的，三维线程块展开：threadIdx.z*blockDim.y*blockDim.x+threadIdx.y*blockDim.x+threadIdx.x
  * 展开的一维线程以32个为一个线程束，最后不足32部分独立为一个线程束

* **线程束调度**
  * 线程束切换开销为0
    * SM保存每个线程束的上下文
      *SM中的常驻线程块的数量受到可用资源的限制
    * 资源：程序计数器，寄存器，共享内存
    * 活跃线程束：具备计算资源的线程束

* **线程执行**
  * 每个线程束以SIMD方式在SM上执行
    * 线程束内同时执行同样的语句
  * 分支分化：线程束出现不同的控制流变为串行化执行，降低效率
  * 减少分支分化的影响
    * 减少判断语句
      * 尤其是基于threadIds的判断语句
      * 判断语句未必导致分支分化
    * 平衡分支执行时间

### 2.4.2 原子操作与同步
* 原子指令
  * 执行过程中不能分解为更小的部分：不被中断
  * 避免竞态条件race condition的产生

* CUDA原子操作
  * 加减：atomicAdd(), atomicSub(), atomicInc(), atomicDec()
  * 比较与交换：atomicMin(), atomicMax(), atomicExch(), atomicCAS()
  * 位运算：atomicAnd(), atomicOr(), atomicXor()
  * 支持的原子操作依据GPU架构而异
  * 基础操作：atomicCAS()
    * 其他所有的额原子操作都可以基于atomicCAS()实现
    * CAS：compare and swap
      * 读取目标位置address并与预期值old_val进行比较
        * 相等则将new_val写入目标位置
        * 否则不做改变
          *返回目标位置中原值：检查操作是否成功

* CUDA同步机制
  * 核函数调用结束时同步
    * host端使用cudaThreadSynchronize()
  * 线程束中同步
    * 隐含的同步执行
  * 使用原子操作
    * 实现原子锁
    * 不指明执行顺序

### 2.4.3 并行编程模型

* 并行模式
  * 并行程序的高级抽象，不关心具体的数据类型或操作
  * 常见的并行模式：gather, scatter, map, stencial, reduce, scan, sort, segmentedscan, map-reduce

#### 2.4.3.1 规约算法

* 对传入的O(n)个数据使用二元操作符生成O(1)个结果
  * 二元操作符需满足结合律，如最值，求和等
* CPU串行规约-执行时间O(n)
* <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/20.PNG?raw=true"/>
* GPU规约：二叉树-执行时间O(lgn)
* <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/21.PNG?raw=true"/>
* 例子：数组求和
  * 使用全局内存
    * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/22.PNG?raw=true" />
    * 需要额外分配空间，访问速度慢，线程工作量小
  * 使用共享内存
    * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/23.PNG?raw=true" />
    * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/24.PNG?raw=true" />
  * 使用atomicAdd（）
    * 一次核函数或数次不带atomic的核函数加上一次带atomic的核函数
    * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/29.PNG?raw=true" />
  * 使用线程束洗牌指令
    * 共享内存使线程块中线程快速交换数据，必要时使用\__syncthreads()同步
    * 洗牌指令使线程块中线程快速交换数据
      * 允许线程直接读取线程束内其他线程的寄存器
      * 不通过共享内存或全局内存，延迟更低，不消耗额外内存
      * 隐式同步
      * 四种洗牌指令
        * 支持int，float，half
        * var：需要交换的变量；srcLane：数据来源的束内线程；Width：线程束的分段宽度
        * 1. T \__shlf(T var, int srcalne, int width = warpSize)
        * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/25.PNG?raw=true" />
        * 2.  T \__shlf_up(T var, int delta, int width = warpSize)  -delta:移位的偏移量
        * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/26.PNG?raw=true" />
        * 3. T \__shlf_down(T var, int delta, int width = warpSize) -delta:移位的偏移量
        * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/27.PNG?raw=true" />
        * 4. T \__shlf_xor(T var, int laneMask, int width = warpSize) -laneMask:指明异或的bit
        * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/28.PNG?raw=true" />

#### 2.4.3.2 扫描算法
* 包含扫描scan，前缀扫描prefix scan，前缀求和prefix sum，并行前缀求和parallel prefix sum
* 扫描是许多算法的基本组成模块
  * 基数排序radix sort
  * 快速排序quick sort
  * 流压缩stream compaction和流拆分stream split

## 2.5性能优化
* 性能影响因素
  * 内存使用
  * 线程束执行模式（分支）
  * 原子操作

### 2.5.1 优化全局内存访问
* 对齐与合并访问
  * 三种缓存路径
    * 一级和二级缓存（默认）
      * 一级缓存128B的cache line，通常用于寄存器溢出到本地内存
      * 二级缓存32B的cache line，所有的全局内存访问都有经过二级缓存
    * 常量缓存（显示说明）
    * 只读缓存（显示说明）

  * 二级缓存
    * 对齐与合并访问
    * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/15.PNG?raw=true" />
    * 对齐访问，但引用地址不连续
    * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/16.PNG?raw=true" />
    * 非对齐访问，引用地址连续
    * <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/17.PNG?raw=true" />

* 一级缓存的合并访问
  <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/18.PNG?raw=true" />


### 2.5.2 优化占用率
* 占用率occupancy
  * 表明SM的使用情况：占用率=活跃线程束数量/最大活跃线程束的数量
  * 影响因素：寄存器数量、共享内存使用量、block中的线程数限制
  * 增加占用率的影响：
    * 好处：掩盖存储器延迟
    * 缺点：为增加占用率减少共享内存可能导致缓存效率降低
    * 应该在占用率和分配资源间寻找平衡
  * 自动决定线程块的大小（CUDA6.5）
    * \__host__ cudaError_t cudaOccupancyMaxPotentialBlockSize ( int* minGridSize,
      int* blockSize, T func, size_t dynamicSMemSize, int blockSizeLimit) [inline]
    * minGridSize：返回达到最优占有率所需的最小网格大小
    * blockSize：返回达到最优占用率的线程块的大小
    * func：优化目标的核函数
    * dynamicSMenSize：动态内存的大小
    * blockSizeLimit：核函数支持的最大线程块的大小

### 2.5.3 其他优化方法
* **选择合适的线程块大小**
  * wave：在设备上同时并行的线程块
  * Tail：处理无法被整除的工作量中余下部分的线程块
  * 意义：过大的线程块可能无法充分利用硬件资源，过小的线程块可能限制利用率
    * SM上同时活跃的线程块数量有限制
      <img src="https://github.com/libq8/MyBlog/blob/master/image/courses-note/multicores/19.PNG?raw=true" />
* **降低分支的工作量**
  * 若不同的分支完成相同的任务，尝试将其从分支中抽离

* **循环展开**
  * 使用编译器
  * 手动循环展开