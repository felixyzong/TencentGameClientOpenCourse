# 2024腾讯游戏公开课笔记

## 第一课：UE入门

讲师：shannon，光子工作室高级游戏客户端开发



本节课程内容：

1. UE**学习途径和方法**介绍
2. UE**编辑器使用和编程技巧**
3. UE引擎工具了解

本节课程作业：

1. **源码编译UE5**，新建一个**C++工程**，进行简单场景编辑和工程设置
2. **编译并构建安装包**，确保能够用来将游戏工程安装到手机正常运行（若无安卓手机可构建桌面版本）



课程目标：

1. 熟悉UE编辑器操作，了解UE游戏模式构架
2. 能够独立获取编译UE源码
3. 能够在UE引擎中实现蓝图编程和C++编程
4. 能够使用UE引擎构建安卓平台游戏安装包



游戏制作团队典型构架：

![image-20241124221200388](C:\Users\Felix Zong\AppData\Roaming\Typora\typora-user-images\image-20241124221200388.png)

 

### 游戏引擎简介

#### 什么是游戏引擎？

专为游戏而设计的工具以及科技合集。

Game engine are data-driven architectures that are reusable and therefore do not contain game content.

1. 通用性与偏向性
2. 可扩展性
3. 完善的工具链

推荐阅读：Game Engine Architecture by Jason Gregory

游戏引擎结构：

![image-20241124222841230](C:\Users\Felix Zong\AppData\Roaming\Typora\typora-user-images\image-20241124222841230.png)



#### 常见的游戏引擎有那些？

Unreal Engine：堡垒之夜，黑悟空

UE的优势：

1. 渲染品质：电影级别**PBR**，先进的着色模型
2. 美术制作：成熟的美术资产制作管线
3. C++与蓝图：性能与可视化编程并重
4. 开发周期：基于射击类的GamePlay框架
5. 跨平台：移动、主机、PC、VR
6. **开源**：有利于技术提升和定制化改造



Unity：老牌引擎，不开源

CryEngine V：开源，手游化转型做的不好

Open 3D Engine：新开源引擎

Source2：不开源（Dota2，CSGO）from id-software

Frostbite Engine：EA内部引擎（战地）

Naughty Dog, Rstar, Ubisoft, AcTiVision：都有内部引擎



#### 游戏引擎：渲染

Deferred Renderer:

1. 编辑器、PC、Console默认渲染管线
2. Feature levels "SM4", "SM5"

Forward+ Renderer:

1. 用于桌面VR游戏，支持MSAA
2. Feature level "SM5"

Mobile Renderer:

1. Forward Renderer, Deferred Renderer
2. Feature levels "ES2", "ES3_1", "Vulkan"



移动端渲染分Tile，性能与带宽无法与PC端相比，一个Tile一个Tile渲染

Immediate Mode Rendering

Tile Based Rendering

Tile Based Deferred Rendering



移动端问题：显存（片上内存）非常宝贵，处理速度高于主内存。使用显存消耗很低，如果需要内存和显存来回多次传输消耗很大，可能导致过热降频

1. Real-time Rendering（实时渲染经典著作）
2. GAMES101-现代计算机图形学入门-闫令琪
3. GAMES202-高质量实时渲染-闫令琪



#### 游戏引擎：物理

Havok：

1. Physics, Destruction, Cloth, AI, Behavior, Animation, FX
2. 被Intel，微软收购，CPU友好

PhysX：

1. 集成于UE4和Unity
2. 被NVIDIA收购，GPU友好

Bullet：

1. 最早开源，用于GTA5，荒野大镖客



物理引擎包含的内容：

1. 碰撞检测
2. 动态约束（骨骼、关节）
3. 刚体物理
4. 车辆物理
5. 布娃娃系统
6. ...



**Chaos** in UE

实时电影视效级别的大范围破坏：碎块预先处理好



### Unreal Engine 介绍

#### 背景介绍

1998，UE1

2003，UE2

2006，UE3

2014，UE4

2021，UE5

UE学习渠道：官方Youtube，Bilibili，知乎



#### Launcher

Store，Library，Unreal Engine

入门通过Unreal Engine > Samples

建议使用UE5.4，最新5.5并不稳定

下载引擎时注意dependencies



#### 编辑器使用

Project Defaults：C++

#### 项目结构

#### 源码构建





### UE编程技巧

### 引擎工具了解及学习资料

