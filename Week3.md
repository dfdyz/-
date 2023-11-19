# 第三周



## 题前话



## SceneManager（场景管理）



> ## Scene（场景）是指包含游戏世界中各种对象、元素和环境的容器

AssetBundle

###  游戏开发中，通常会将游戏拆分为多个场景，每个场景代表一个特定的游戏区域或关卡。通过场景，可以切换不同的游戏界面，比如游戏开始画面、关卡地图、战斗场景等。就涉及到场景管理，和资源加载，卸载



麦扣新教程用Addressables来管理资源（包括场景）

[可寻址对象 |可寻址对象 |1.19.19 (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.addressables@1.19/manual/index.html)



SceneManager类

![image-20231116115540508](../AppData/Roaming/Typora/typora-user-images/image-20231116115540508.png)

[SceneManagement.SceneManager - Unity 脚本 API](https://docs.unity.cn/cn/2018.4/ScriptReference/SceneManagement.SceneManager.html)

[Unity3D 11-SceneManager场景管理用法总结-CSDN博客](https://blog.csdn.net/maxdong24/article/details/74332747)



### 切换关卡时要做什么

举个例子：

> 玩家进入一个房间时按照顺序做下面的事情
>
> 1. **加载**房间内部的场景
> 2. **设置**玩家、相机、UI等不随场景变化的元素
> 3. **卸载**房间外部的场景

案例-Toem：
![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/Video_20221126_042727_411.gif)

### SceneManager.LoadScene()

函数声明

```C#
public static void LoadScene(int sceneBuildIndex, SceneManagement.LoadSceneMode mode = LoadSceneMode.Single);

public static void LoadScene(string sceneName, SceneManagement.LoadSceneMode mode = LoadSceneMode.Single);
```

- buildIndex
  在 `File => Build Settings` 里面就可以知道场景的编号

- sceneName
  在 `File => Build Settings` 里面就可以知道场景的**完全名称**（本质上是场景文件的路径）

<img src="https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221126114152.png" style="zoom:33%;" />

使用时只需要填写一个参数：场景的索引（序号或名称）

```C#
{
	SceneManager.LoadScene(0);
	SceneManager.LoadScene("Scenes/SampleScene");
}
```

上面提到了切换关卡要做什么，代码上就不是简简单单的一行加载了

```C#
{
	DontDestroyOnLoad(this); // 避免加载场景后物体被销毁
	SceneManager.LoadScene(1); // 加载场景
	OnSceneLoaded(); // 自己定义的切换场景后的行为
}
```

思考：**不加DontDestoryOnLoad会怎么样？**

### 小尾巴1：第二个参数mode

Unity提供了两种场景加载方式

- Single（默认值）关闭当前所有的场景，加载新场景
- Additive 不关闭当前的场景，以添加方式加入新场景

第二种管理方式会影响部分代码的运行，一般不会使用
如果有需要可以再去了解

### 小尾巴2：DontDestroyOnLoad

> SceneManager.LoadScene() 的调用时间为下一帧，也就是说，这个脚本将会执行完这个函数，而不是到这一行就开始切场景
>
> 因此 OnSceneLoaded 并没有在新场景去执行
>
> SceneManager.LoadSceneAsync() 又有不同
>
> 反正这个函数我常用于托管场景切换时恒定的对象或全局存在的对象



### 异步加载LoadSceneAsync

什么是异步：

多个任务同步执行，不需要管别的任务的执行情况



我的理解是 ：在不阻塞当前任务的执行，允许程序在等待某个耗时操作完成的同时继续执行其他任务。



异步加载下， 程序会继续执行，不会等待场景加载完成。



例子：后台加载场景，不影响前台的UI进度条/加载动画，~~暗示玩家游戏没有卡死~~ 在维持当前场景不动的情况下

函数声明

```C#
public static AsyncOperation LoadSceneAsync(string sceneName, SceneManagement.LoadSceneMode mode = LoadSceneMode.Single);

public static AsyncOperation LoadSceneAsync(int sceneBuildIndex, SceneManagement.LoadSceneMode mode = LoadSceneMode.Single);
```

同样，使用时只需要填写一个参数：场景的索引（序号或名称）

细心的已经注意到了，函数的返回值是 `AsyncOperation` ，这是Unity提供的异步操作处理类
暂时只需要知道两个东西

- isDone 异步操作是否完成，判断一下就知道场景有没有加载完
- progress 异步操作进度，加个进度条

```c#
using System.Collections;
using UnityEngine;
using UnityEngine.SceneManagement;

public class LoadSceneAsync : MonoBehaviour
{
    public string sceneName;
    //异步加载对象
    private AsyncOperation asyncLoad;

    void Start()
    {
        StartCoroutine(LoadScene());
    }

    IEnumerator LoadScene()
    {
        //获取加载对象
        asyncLoad = SceneManager.LoadSceneAsync(sceneName);
        //设置加载完成后不跳转
        asyncLoad.allowSceneActivation = false;

        while (!asyncLoad.isDone)
        {
            //输出加载进度
            Debug.Log(asyncLoad.progress);
            //进度.百分之九十后进行操作,当进度为百分之90其实已经完成了大部分的工作,就可以进行下面的逻辑处理了
            if (asyncLoad.progress >= 0.9f)
            {
                asyncLoad.allowSceneActivation = true;
            }

            yield return null;
        }
    }
} 
```



## 实现敌人巡逻



### 基础方法







#### 演示

### 有限状态机

#### 什么是有限状态机？

状态机指的是，这个东西是用来解决对象多个状态切换问题的，有限的意思是，我们需要切换到的状态都是提前制定好的已知状态，状态数量是有限的

Finite-State-Machine，一般我们会用FSM来简称这种决策模式

核心思想是**状态模式**，将每个状态需要用到的数据和逻辑封装到一个类里，即一个状态一个类，从这个状态切换到下一个状态的条件、这个状态的方法、变量，全都存在这个类里，不在这个类时，我们暂时就不需要生成这个类对象的实例

**优点是可以严格保证状态之间切换清晰，且永远只会在一个状态**

**缺点是可拓展性较差**，状态多了变成蜘蛛网



Animator上也是一种有限状态机



#### 为什么要了解状态机？

提供了一种更清晰、更模块化的方式来管理复杂的游戏对象行为

可能听我上面说了那些还是不直观，不能体会到状态机的优势，那大概率是因为我讲的不好

可以看看这篇文章：[状态模式](https://zhuanlan.zhihu.com/p/22976065)

不严谨的粗略示意图：



![image-20221119183428088](https://image-wai.oss-cn-guangzhou.aliyuncs.com/img/image-20221119183428088.png)



####  ![image.png](https://s2.loli.net/2023/11/19/uObmCNw3UzxaL2J.png)



### 行为树



#### 行为树是控制“任务”执行流的**分层节点树**。



> ## Execion Nodes （执行节点）
>
> + ### Condition node： **条件节点** 条件判断，返回true     false
>
> + ### Action node：  行为节点  逻辑行为，返回 runing    true    false
>
> ## Control Nodes（控制节点）
>
> 
>
> + ### **Sequence Node**： 顺序节点 按层次执行子节点，全部成功返回true，否则返回false并break（与）
>
> + ### **Selector Node**： **选择节点**  按照一定顺序执行子节点，直到有一个子节点返回成功 （逻辑或）
>
> + ### **Parallel Node**：**并行节点**  并行执行所有子节点，返回可以自己定
>
> + ### **Decorator Node**：**装饰节点** 修改或扩展其子节点的行为。例如，限制某个节点的运行次数、延迟执行、检查子节点是否成功等 



![image.png](https://s2.loli.net/2023/11/16/KeE4jZJ8CORrQIb.png)

![image-20231116172033260](../AppData/Roaming/Typora/typora-user-images/image-20231116172033260.png)

[【Games104】经典决策算法](https://www.bilibili.com/video/BV1r34y1J7Sg/?spm_id_from=333.788&vd_source=4c0460322df3d1b4c932782b1bfef91f)  Games出品，不多说了，我讲的不如王希一根









### 优缺点

> ### 优缺点是横向对比出来的
>
> ### 举个例子吧 
>
> ### 假如有一个 山贼
>
> ### 山贼的AI需求描述如下：
>
> - ### 视野内没有敌人则在一定范围内巡逻
>
> - ### 视野出现敌人则走过去攻击敌人
>
> - ### 当自己血量 < 20% 则逃跑
>
> ### 下面将分别用状态机和行为树来描述山贼的AI



![image.png](https://s2.loli.net/2023/11/16/pZY8PuCdLzGSeqQ.png)

> ### 上图是用状态机来描述山贼的AI, 目前来看状态机的逻辑还是非常清晰，代码实现也比较简单。但随着项目的推进，策划随时可能增加需求。假如现在需要加一个 “石化”状态，描述如下：
>
> 
>
> #### 在逃跑的过程中，如果追击者数量>2，则立即把自己石化，持续5s，此时自己不能被攻击也不能攻击目标
>
> 
>
> ### 在状态机中加入“石化”状态时，我们需要考虑它跟现有每个状态之间是否有联系，输入输出状态分别有哪些。
>
> #### 如果状态越来越多，任何一点小修改都会产生很大的工作量，代码中会出现大量的判断跳转，代码的逻辑会变得越来越混乱



![image.png](https://s2.loli.net/2023/11/16/SWgkFhoAYR76UEb.png)



> ###  对于程序来说，只需要写”石化”的逻辑即可，至于这个行为用在哪里，执行顺序以及和其它行为的关系，则由策划来决定。











> ##  总结 ：状态机更多地关注对象的行为状态和转换。对象主要受到外界的影响而改变自身的状态，再根据当前状态执行相应的行为，且同一时刻只能处于一种状态。
>
> ## 实现简单、执行效率高。
>
> ## 随着状态数量的增多，状态之间的关系会越来越复杂，代码变得难以维护。





> ## 行为树更关注于控制和组织AI的行为，对游戏中的各种信息进行决策以挑选接下来将要执行的行动并且监控该行动的执行，可以处于多种状态。
>
> ## 结构清晰、节点间关系弱，可拓性强，程序大部分工作是丰富行为节点，AI流程能交由策划完成。
>
> ## 每次tick都会遍历整棵行为树(Mem节点除外)，若树的深度很深，效率将变得低下。



结合 巡逻，死亡，逃跑 这些简单逻辑用状态机实现，进入战斗的Enemies用行为树 



[游戏中的AI-行为树 | lifan's blog](https://lifan.tech/2020/02/15/game/behavior-tree/#:~:text=游戏中的AI-行为树 | lifan's,blog 游戏中，常见用AI实现方式有2种，状态机和行为树。 下面主要简介绍行为树，行为树是用一棵多叉树来表示AI，树的中间节点为控制节点控制着AI的执行流程，叶子节点为行为节点，描述了AI的具体行为。)

[怪物行为树案例_游戏开发中怪物AI实现方案总结-CSDN博客](https://blog.csdn.net/weixin_39612896/article/details/111784374?spm=1001.2101.3001.6650.14&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-14-111784374-blog-73613558.235^v38^pc_relevant_default_base3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-14-111784374-blog-73613558.235^v38^pc_relevant_default_base3&utm_relevant_index=20)

​			

## 游戏循环（Game Loop）

[游戏编程模式之游戏循环_ZhuSenLin_BLOG的博客-CSDN博客](https://blog.csdn.net/qq_38134452/article/details/120900207)



可能不是程序方面的（

梳理游戏的状态，确保游戏不会进入一个无法退出的死状态
以2D平台跳跃为例
![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221126172636.png)

如果是在自己做Demo，需要确保整个游戏是个闭环，~~避免导致玩家必须AltF4关闭游戏重来~~













## ECS框架



#### ECS架构是一种游戏开发架构，有别于传统面向对象它将游戏对象分为实体（Entity）、组件（Component）和系统（System）三个部分。



![Snipaste_2023-11-15_22-46-30.png](https://s2.loli.net/2023/11/15/3yAGdEOFiBuxgXL.png)

> ### **实体代表游戏中的物体或对象。通常情况下，实体只是一个唯一的标识符或ID**
>
> 
>
> ###  组件描述实体的属性。通常是纯数据，比如位置、颜色、生命值等。
>
> ### （行为组件：输入处理，动画，要继承mono类）
>
> 
>
> ### **系统描述行为逻辑，例如移动、攻击、碰撞检测等。（逻辑操作）**
>
> 
>
> ### **分离属性和行为，在系统中从组件中获取数据，并针对符合特定条件的组件集合执行逻辑操作。是由组件关联到实体的，而不是对遍历GameObject筛选Component；例如，当需要对所有游戏对象进行碰撞检测时，只需要对所有具有碰撞组件的游戏对象进行检测，而不需要遍历所有游戏对象。**
>
> 
>
> ### **ECS架构便于处理大量实体和数据，主要**用于高并发，大数量（>10000），少逻辑，需要复用的场景。**。**
>
> 
>
> ### **分离属性和行为，能直接修改一类行为逻辑，使系统更好维护、**





#### **unity有相关的DOTS包**



#### **[实体组件系统 |实体 |0.17.0-预览版.42 (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.entities@0.17/manual/index.html)**





# 考核要求

### **需要实现的功能：**

+ 开始菜单，更多UI
+ 完善场景中的内容
+ 设置通关条件（杀敌数？）
+ 实现简单的巡逻AI（mob形象自己决定）



### **进阶（整活&预习）：**

+ 实现攻击判定（血条，受击）



### 勇士传说特供作业（必做）：

+ 实现装备系统（盔甲减伤，长剑增加攻击范围，大剑增加攻击伤害，鞋子增加移动速度，可以直接拾取）

