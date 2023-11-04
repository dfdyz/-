# 第一周

## 一、引擎界面

![image-20221112142455227](https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112142455227.png)

引擎界面分为六个重要基础面板：**Hierarchy、Scene、Inspector、Project、Console、Game**

分别都有不同的作用和右键菜单



### 1.Hierarchy 层级

就是你当前场景里的**GameObject**，有哪些，父子关系如何

右键菜单：各种创建场景物体相关的选项

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112142245522.png" alt="image-20221112142245522" style="zoom: 67%;" />



+ 什么是**GameObject**？

  Unity中用来容纳组件的基本单位

![image-20221112143449125](https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112143449125.png)



### 2.Inspector 检查器

Inspector窗口显示的信息会根据当前选中的GameObject变化而变化

窗口会显示这个GameObject挂载的Component，以及Component当前的参数

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112142922078.png" alt="image-20221112142922078" style="zoom: 67%;" />

+ **拖动引用、修改参数**

  序列化的参数可以在Inspector中直接修改

![image-20221112143915778](https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112143915778.png)

+ **Debug模式**

  可以看到私有变量、刚体速度等等.....

### 3.Project 项目

当前项目所在文件夹的信息

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112144151853.png" alt="image-20221112144151853" style="zoom:50%;" />



+ **文件夹结构**

  **一定要做好文件夹管理！！！！！！！**

  规范参考下图，不一定全都会用到

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112144217329.png" alt="image-20221112144217329" style="zoom:50%;" />

**Packages文件夹**

存放不能修改的外部包

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112144606415.png" alt="image-20221112144606415" style="zoom:50%;" />



### 4.Console 控制台

输出Debug、Warning、Error信息的窗口

![image-20221112144803872](https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112144803872.png)



+ **Debug方法**

  讲完脚本以后再回来说，报错信息、同类折叠、异常处跳转、报错暂停.....



### 5.Scene 场景

场景预览，所见即所得

熟悉快捷键、了解功能即可，没啥好讲的

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112145517851.png" alt="image-20221112145517851" style="zoom: 67%;" />



### 6.Game 游戏预览

游戏画面预览，要点一下Game窗口才能输入，摸一摸就会了，也没啥好讲的

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112145831232.png" alt="image-20221112145831232" style="zoom:50%;" />





## 2. GameObject

[重要的类 - GameObject - Unity 手册 (unity3d.com)](https://docs.unity3d.com/cn/2022.3/Manual/class-GameObject.html)



## 3. 脚本&生命周期

### 什么是脚本？

通过写的逻辑和UnityAPI控制场景游戏物体



### MonoBehaviour

Unity提供的一个基类，只有继承这个类的脚本才可以被当做Component挂载在GameObject上

先留个概念，以后慢慢研究慢慢了解

​	<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112151903914.png" alt="image-20221112151903914" style="zoom:67%;" />

+ 注意：在旧版本的Unity中，自动创建的脚本名要和类名相同，因为Unity是通过反射找到的指定Mono类

### 生命周期

[MonoBehaviour - Unity 脚本 API (unity3d.com)](https://docs.unity3d.com/cn/2022.3/ScriptReference/MonoBehaviour.html)



[Start](https://docs.unity3d.com/cn/2022.3/ScriptReference/MonoBehaviour.Start.html) 
[Update](https://docs.unity3d.com/cn/2022.3/ScriptReference/MonoBehaviour.Update.html)
[FixedUpdate](https://docs.unity3d.com/cn/2022.3/ScriptReference/MonoBehaviour.FixedUpdate.html)
[LateUpdate](https://docs.unity3d.com/cn/2022.3/ScriptReference/MonoBehaviour.LateUpdate.html)
[OnGUI](https://docs.unity3d.com/cn/2022.3/ScriptReference/MonoBehaviour.OnGUI.html)
[OnDisable](https://docs.unity3d.com/cn/2022.3/ScriptReference/MonoBehaviour.OnDisable.html)
[OnEnable](https://docs.unity3d.com/cn/2022.3/ScriptReference/MonoBehaviour.OnEnable.html)



Unity官方的图

![](https://raw.githubusercontent.com/dfdyz/MyPicBed/picgo/Typora/202310302034487.png)

[【精选】【Unity】使用FixedUpate以及Upate的注意点_unity fixedupdate_鱼儿-1226的博客-CSDN博客](https://blog.csdn.net/qq_21743659/article/details/127619559)

[Unity中的Update与FixedUpdate_unity update和fixedupdate_Hello Bug.的博客-CSDN博客](https://blog.csdn.net/LLLLL__/article/details/127770056)



## 4. RigidBody2D & Collider2D

### 2D刚体（RigidBody2D）

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221112154849290.png" alt="image-20221112154849290" style="zoom: 80%;" />

**什么是刚体**？

[Unity官方手册对刚体的定义](https://docs.unity.cn/cn/2019.4/Manual/class-Rigidbody2D.html)，里面还有一些讲解刚体参数的，建议细看

就是把这个物体注册到物理系统里，并且可以通过修改刚体组件的参数来控制它

### 2D碰撞体（Collider2D）

#### 脚本事件

OnCollisionEnter2D      当两个碰撞体碰到一起的瞬间触发一次

OnCollisionStay2D		当两个碰撞体贴贴的时候，每一物理帧触发一次

OnCollisionExit2D         当两个碰撞体分开的瞬间触发一次

#### Trigger (触发器模式)

OnTriggerEnter2D      当两个碰撞体碰到一起的瞬间触发一次

OnTriggerStay2D		当两个碰撞体贴贴的时候，每一物理帧触发一次

OnTriggerExit2D         当两个碰撞体分开的瞬间触发一次



### Effector

[2D 效应器 - Unity 手册 (unity3d.com)](https://docs.unity3d.com/cn/2022.3/Manual/Effectors2D.html)



### 对象移动

[UnityAPI 关于刚体的部分](https://docs.unity.cn/cn/2019.4/ScriptReference/Rigidbody2D.html)

#### 1.`Transform.Translate()`

```c#
void Update()
{
    float speed = Input.GetAxis("Horizontal") * moveSpeed;
    transform.Translate(speed * Time.deltaTime, 0, 0);
}
```

直接改变坐标，相当于直接修改Transform的数值

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/transform移动.gif" alt="transform移动" style="zoom: 25%;" />

+ 优点：.... 角色移动没有优点，一般用在物体上
+ 缺点：卡墙、先改变位置再被物理系统弹出来



#### 2.`RigBody2D.velocity = new Vector2(speedx, speedy)`

改变刚体的速度

```c#
void Update()
{
    float speed = Input.GetAxis("Horizontal") * moveSpeed;
    rig.velocity = new Vector2(speed, rig.velocity.y);
}
```

<img src="https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/rig移动.gif" alt="rig移动" style="zoom:25%;" />

+ 优点：方便控制，不穿模
+ 缺点：速度需要自己计算（也不算是缺点了，稍微麻烦而已）



#### 3.`RigBody2D.AddForce(forcex,forcey)`

给刚体施加力

```c#
void Update()
    {
        float speed = Input.GetAxis("Horizontal") * moveSpeed;
        if(speed != 0)
            rig.AddForce(new Vector2(speed,0f));
    }
```

+ 优点：简单直观
+ 缺点：速度不能精确控制，而且unity物理系统很老坛





## 5.InputManager&InputSystem

### InputManager

[Input Manager - Unity 手册 (unity3d.com)](https://docs.unity3d.com/cn/2022.3/Manual/class-InputManager.html)



### InputSystem

[Input System | Input System | 1.6.3 (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.6/manual/index.html)

[【Unity_Input System】Input System新输入系统（一）_unity新inputsystem_铃兰148的博客-CSDN博客](https://blog.csdn.net/weixin_61427881/article/details/130556928)





## 6.协程

#### 什么是协程？

[hz博客讲协程讲的很透彻](https://www.cnblogs.com/KillerAery/p/10336388.html)

概括来说就是单条线程里的异步，协同线程，能有异步的效果但是底层不是真正的异步



#### 用来干什么？

大多数时候我们用协程来延时执行逻辑，例如N秒后调用指定函数

例如：

````c#
    IEnumerator Test()
    {
        for (var timer = 0f; timer < 3.0f; timer += Time.deltaTime)
        {
            yield return null;
        }
        Debug.Log("开启协程3s后");
    }
````

+ 需要注意的是 `yield return` 、`yield break`、`StartCoroutine()` 的用法



## 7.文件结构&代码规范

下面是一个常见的Unity项目目录结构示例：

- Assets（Unity默认）

- - Animations：存放动画文件

  - Audio：存放音频文件

  - Editor：存放编辑器脚本和自定义编辑器扩展

  - Fonts：存放字体文件

  - Materials：存放材质和纹理文件

  - Models：存放模型文件

  - Prefabs：存放预制体文件

  - Scenes：存放场景文件

  - Scripts：存放脚本文件

  - - Controllers：存放控制器脚本
    - Managers：存放管理器脚本
    - UI：存放UI相关脚本
    - Utils：存放工具类脚本



- - Shaders：存放着色器文件
  - Sprites：存放精灵图文件
  - Textures：存放纹理文件
  - UI：存放UI素材文件
  - VisualEffects：存放视觉效果文件



- Packages：存放导入的插件和资源包（Unity默认）
- ProjectSettings：存放项目的设置文件（Unity默认）
- Library：存放Unity生成的临时文件和缓存文件（Unity默认）
- Temp：存放临时文件和备份文件（Unity默认）



#### 代码规范

可以参考 **鹅厂 阿里巴巴 冈易** 等大厂的代码规范文档

[阿里巴巴 - C#代码规范（附条文及目录）.pdf-原创力文档 (book118.com)](https://max.book118.com/html/2020/0926/6212031210003001.shtm)





# 考核要求

### **需要实现的功能：**

+ 角色移动、转向、跳跃（不要无限踏空跳）
+ 场景中的小机关（移动的方块或者地雷等）
+ 相机跟随



### **进阶（整活&预习）：**

+ 给角色添加点特色功能（技能？普通攻击？）
+ 给角色添加动画（下节课会讲动画机）
+ 添加一些UI（下节课会讲UI）
+ 做一些有意思的机关或者道具（自由发挥）



### Tips

第一周需要先强调一下学习方法

我们每周都会给出几个讲的还不错的**系统教程**，和一些和实现类似需求的**补充资料**

系统教程会一步步教你如何操作，但往往不会告诉你为什么，我们应该尽己所能结合API手册和网上各种能查到的资料搞明白他具体在做什么，为什么要这样做，有没有更好的实现方式。

在往后的学习中，我们摆脱萌新的第一阶段就是需要**先从会抄转变到会写**，一开始能照着教程实现功能，理解作者为什么要这么做，逐渐转变到从策划那接到一个全新的需求，能独立设计解决方案并实现出来。这个过程是需要时间和经验积累的，最关键的是先让自己积极的动起来，现在就**先从让游戏世界里的物件活起来让自己获得正反馈吧**

**规范代码（命名，缩进，注释）**

**规范目录结构**



# 附件

### 视频教程

标出的节次只表示这部分和我们本周要实现的需求强相关，节次要求以外的也可以自行学习

1.[设置人物及基本组件｜Unity2022.2 最新教程《勇士传说》入门到进阶｜4K_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Fs4y127A4/?spm_id_from=333.788&vd_source=0fd2ef88be43b335dec5c144c2a76158) 设置人物基本组件~物理环境监测及Gizmos绘制

2.[秦无邪，讲课内容会更精炼一些，可能不适合完全的新手入门，但实现了很多特殊功能](https://space.bilibili.com/335835274/channel/collectiondetail?sid=206444&ctype=0) Lesson01~05

### 补充资料

补充的资料会涉及比较多的方面，可能是一些专门针对某个需求的专项讲解，也可能是从游戏设计的角度来说为什么这么实现比较好，也有可能是分析业内顶尖同类型作品的视频，总之就是包罗万象，私货满满

1.[【GMTK】从设计角度分析，蔚蓝如何做出平台跳跃天花板手感的](https://www.bilibili.com/video/BV1M441197sr)

2.[【Mix&Jam】从实现角度拆解蔚蓝，只讲思路，实现难度对新手会比较大](https://www.bilibili.com/video/BV1xr4y1s71V/)

3.[如何做出一个优秀手感的角色控制器，新手了解设计即可](https://www.bilibili.com/video/BV14S4y1o79L/)

4.[想做ACT的可以看看怎么设计游戏的手感](https://www.bilibili.com/video/BV1AV4y1T7Wx/?spm_id_from=333.337.search-card.all.click&vd_source=0fd2ef88be43b335dec5c144c2a76158)

