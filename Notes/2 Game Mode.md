# 2024腾讯游戏公开课笔记

## 第二课：游戏模式

讲师：tutu，光子工作室高级游戏客户端开发，现负责光子工作室PUBGM客户端战斗玩法开发



本节课程内容：

1. UE游戏模式框架
2. UE游戏模式中的重要对象
3. 示例以及课程作业



课程目标：

1. 了解UE游戏模式框架和游戏运行流程
2. 了解UE游戏模式框架下常用重要对象作用和职责
3. 能够基于官方射击Demo拓展实现简单Gameplay玩法



什么是好游戏？

精品游戏 = 仿真 + 抽象 ？



### UE游戏模式框架

**游戏模式**：游戏世界里组织数据和运作规则的方式。

#### 万物之源：UObject

1. 元数据，反射生成，GC垃圾回收，序列化
2. 通用的属性和接口（Equals，Clone，GetHashCode，ToString，GetName，GetMetaData）

U开头的继承自UObject，A开头继承自AActor



#### 物体的表达：AActor + AActorComponent

1. EC架构（Entity-Component Framework）：一种游戏中常用的设计模式，一个实体和多种能力组合
2. 一个非常重要的Component：Scene Component 赋予 Actor 空间变化信息，FTransform：Location, Rotation, Scale

舞台上的演员（Actor），各自身怀绝技（Component）。



#### 世界的表达：UWorld + ULevel

因为硬件有限引入Level概念，无法将整个World里的物体全部加载。

1. 平行世界：GameWorld, PIEWorld (Play in Editor), PreviewWorld, ...
2. 关卡构成：主干卡 PersistentLevel + 子关卡
3. 关卡加载：LevelStreaming 流式异步加载 (只加载Layer覆盖的区域) 
   1. World Partition (UE5)
   2. World Composition
   3. Load by Logic
4. 关卡大小和加载距离：Level Bounds + Streaming Distance 分层
5. 关卡蓝图 LevelScriptActor



#### 世界之上：UGameInstance + UEngine

UGameInstance：信息存在于整个游戏的生命周期，不随着地图的切换和销毁，非常适合非业务逻辑的全局管理操作，比如全局UI，设置，预加载

UEngine：管理UGameInstance，拉起游戏重要流程，如Browse，LoadMap，SetClientTravel等等



#### UE游戏世界拉起流程

![image-20241129172056684](C:\Users\Felix Zong\AppData\Roaming\Typora\typora-user-images\image-20241129172056684.png)

 

### UE游戏模式中的重要对象

#### AActor：游戏中最重要的实体

根组件(Root Component \\ Scene Component)提供世界变化信息

作为网络同步的基础单位：Actor值同步 + 组件值同步

标志所有权的Owner指针

标志本地权限的Role枚举



### AActor的生命周期

AActor生成：

1. 关卡内摆放的静态Actor
2. SpawnActor创建的动态Actor：本地Spawn 或 网络序列化

![image-20241129172937734](C:\Users\Felix Zong\AppData\Roaming\Typora\typora-user-images\image-20241129172937734.png)

![image-20241129173145305](C:\Users\Felix Zong\AppData\Roaming\Typora\typora-user-images\image-20241129173145305.png)

![image-20241129173631564](C:\Users\Felix Zong\AppData\Roaming\Typora\typora-user-images\image-20241129173631564.png)

重要的生命周期函数：

1. BeginPlay
2. EndPlay
3. Tick（一帧，类似Unity Update？）

帧率卡顿的原因：每一帧需要计算的东西过多，无法在设计的每帧时间内算完



AActor销毁：标记为PendingKill后 GC完成工作

需要注意有效性判断：IsValid: not empty pointer and not pending kill



#### APawn：可操控的棋子

有多种多样的形式

可以被Controller控制

基础的输入，移动框架支持



常用派生类：

**ADefaultPawn**:

1. 简单球形碰撞 USphereComponent
2. 简单外显 UStaticMeshComponent
3. 简单移动组件 UFloatingPawnMovement
4. 基础的键盘手柄映射

**ASpectatorPawn**：

1. 去掉外显 UStaticMeshComponent
2. 移动组件替换成忽略时间缩放的 USpectatorPawnMovement



#### ACharacter：人形角色

近似仿真人形的胶囊体碰撞盒 UCapsuleComponent，在保证一定真实性的同时节约性能。

骨骼模型 USkeletalMeshComponent 动画蓝图赋予人物生动表现

人物移动组件 UCharacterMovementComponent

1. 配合胶囊提完成Walking\Falling\Swimming\Flying等多种仿真移动计算
2. 提供Custom自定义移动模式供扩展
3. 网络游戏移动同步构架
   1. 主控端预表现
   2. 服务端校验
   3. 模拟端预测



#### AController 和 APawn 的双向奔赴

Possess 与 PossessedBy，从此就是一个有 Owner 的 Pawn 了

1. Controller、PlayerState 指针赋值
2. 网络游戏中 Role 的改变



#### APlayerController：提线木偶操控者

UInputComponent：绑定输入映射

APlayerCameraManager：通过ViewTarget上相机臂作用后的UCameraComponent计算相机位置

AHUD（heads-up display)：注意和UI的区别，逐渐被更灵活的UMG代替

网络连接所有权：注意仅在主控端以及服务器存在PlayerController



#### AGameMode：真 游戏模式

仅服务器拥有，掌控整体游戏流程

定义游戏模式用的基础类型

纯服务器逻辑的运作，如AI

AGameMode和AGameModeBase的区别：

1. AGameModeBase 是所有 GameMode 的基类，是经典的 AGameMode 简化版本
2. AGameMode是AG啊meMo的Base的子类。AGameMode更适用于标准对抗类游戏（如多人射击游戏），完善了对局和比赛的概念。



#### AGameState：游戏状态

所有端都共享同步的游戏数据

AGameState 和 AGameStateBase 区别：

1. 类似 AGameMode 和 AGameModeBase 的关系
2. AGameState 是 AGameStateBase 的子类。AGameState 更适用于标准对抗类游戏（如多人射击游戏），完善了对局和比赛的概念。



#### APlayerState：玩家状态

所有端都共享同步的游戏数据？

PlayerState, Character, Controller 的职责区别：

1. Character 遵从一般 Actor 的规则：只能看到近处Actor，保存靠近了才能获取的数据
2. Controller 只在主控端、服务器端才有
3. PlayerState 可以在本地拥有所有玩家的信息，但是是否有必要？

**课后思考**：如果你来设计游戏，以下数据应该存放在哪里？

1. 玩家血量
2. 玩家得分
3. 队友位置
4. 毒圈、空投信息



### 示例以及课程作业

思考：目前Demo有什么缺陷？

> 看不到角色的位置，蓝色方块位置不对 => 网络同步架构有问题



FP示例代码浅析

1. PlayerController 输入映射配置
2. Character 输入绑定行为
3. Character创造默认组件 
4. 拾取物Overlap交互
5. WeaponComponent，赋予Controller额外输入映射
6. 武器射击，完成投射物的生成，投射物Actor为动态Spawn
7. 投射物：移动组件+球型碰撞+外显mesh



课程作业要求：

基于ue5官方的First Person示例，实现一个游戏demo，本课程作业为实现以下功能

1. 物件规则：
   1. 射击命中方块，获得积分X分
   2. 方块被子弹命中后缩放为Y倍，再次命中后被销毁
2. 游戏流程：
   1. 游戏开始时随机N个方块成为“重要目标”，射击命中后获得双倍积分
   2. 游戏开始后限时T秒，时间到后游戏结算，打印日志输出每个玩家获得的积分和所有玩家获得的总积分
3. 附加题，Nice to have
   1. 利用 UMG 制作结算 UI 替代日志打印
   2. 支持多人联机

