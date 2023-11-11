# 动画

## 帧动画与骨骼动画

### 帧动画

帧动画是一种基于时间轴的动画制作方式，它通过将一系列静态图像按照一定的时间间隔连续播放，从而形成动态的效果。 在帧动画中，每一张静态图像被称为一帧，而播放速度则由每秒钟播放的帧数决定。 帧动画的原理可以简单地概括为“**快速切换静态图像**”。

### 骨骼动画

大概就是给角色添加骨架，骨架包含骨骼、关节、皮肤，通过移动骨架等方式先制作关键帧，然后利用填值等方式补充关键帧之间的过程，做出完整的动画。

### 差异

逐帧动画随时间切换的属性就是图片本身，随时间变化切换图片产生动画的运动效果。骨骼动画的话是记录骨骼的transform属性，中间不存在资源的切换，是那个资源的各个部分在做运动。(鱼大吃说的)

### Spine（感兴趣的自行了解）

[Spine官网: 专注于游戏的2D动画软件 (esotericsoftware.com)](http://zh.esotericsoftware.com/)

## 动画机(Animator)

动画机本质上是Unity自带的有限状态机。通过达成预先设定的条件(event) 触发动作(Action)，使得物体从一个状态(State)转换(Transform)到另一个状态。比如角色处于待机状态(State)，因为玩家输入了移动指令(Event)，角色就开始移动(Action)，角色就从待机状态切换(Transform)到移动状态。
动画机就是这么一个东西。
![[9G2_IJW5Q6GDGPWKOW(50OF.png]]

### 动画混合

适用于骨骼动画，简单来说就是多个动画混合在一起组成一个新的动画。例如将正常走路的动画和受伤走路的动画按照受伤的不同程度进行相应的混合，得到不同受伤程度的走路动画。

### 动画器(Animation)

他是一个时间线记录器，记录的是物体的属性。在时间线上标记之后在播放动画的时候，物体的属性就会按照一定的函数关系随时间变化。不但可以播放动画本身，还可以修改组件的属性，甚至可以引用函数，太强大辣！(还是鱼大吃说的)
![[YY`XIPJ9_BW)R89YHXC2$Y0.png]]

### 动画事件

就是在动画实行过程中引用函数。

# Cinemachine组件

是unity提供的用于控制一个或多个相机的组件。

## 虚拟摄像机(Virutal Camera)

是相机的一些设置，用于控制相机的移动、旋转、朝向、跟踪等。

### 虚拟相机的几个属性

- **Follow**指定了相机跟随移动的目标。
- **Look At**指定了相机瞄准的目标。
- **Dead zone**：Cinemachine会将目标保持在这个区域，目标在这个区域时，镜头保持不动。
- **Soft zone**：如果目标进入这个区域，会触发相机的移动和旋转，将目标重新移回dead zone。这个过程可能很快，也可能很慢，取决于**Damping**属性设置。
- **Screen**：Dead zone区域的中心在屏幕上的位置，可以不在整个游戏屏幕的正中间。
  ![[5)UY5]UQJ0AG[DR]JC%4RZ1.png]]

# UI

## 必备：Canvas & Event System

当创建UI对象时，Unity会自动生成好这两个GameObject，使用Unity的UI系统时必须要有这两个GameObject。

## 什么是Canvas：

Canvas为我们提供了一个2D坐标空间，可以在其中放置和排列UI元素。通过Canvas，我们可以搭建游戏的各种UI

## Canvas:

## RenderMode（渲染模式）:

- Screen Space - Overlay 画布会填满整个屏幕空间，并将画布下面的所有的 UI 元素永远 “覆盖” 在 其他普通的游戏物体上，如果屏幕尺寸被改变，画布将自动改变尺寸来匹配屏幕。

- Screen Space - Camera 选择一个相机渲染UI画面，画布会被放置到摄影机前方。在这种渲染模式下，画布看起来绘制在一个与摄影机固定距离的平面上。所有的 UI 元素都由该摄影机渲染，因此摄影机的设置会影响到 UI 画面。

- World Space（世界控件模式）

  在此模式下，画布被视为与场景中其他普通游戏对象性质相同的类似于一张面片（Plane）的游戏物体。画布的尺寸可以通过 RectTransform 设置，所有的 UI 元素可能位于普通物体的前面或者后面显示。当 UI 为场景的一部分时，可以使用这个模式。

- Pixel Perfect（抗锯齿） 是否为了准确的显示图片的像素而关闭抗锯齿（勾选，抗锯齿就没有用了）

- TargetDisplay

  选择哪个显示器渲染画布。

## Canvas Scaler:

此组件用来限制UI的边界，顾名思义，通过控制画布的缩放来使UI达到自适应屏幕分辨率的效果。

- Constant Pixel Size：使 UI 元素保持固定的像素大小，无论屏幕大小。

- Scale With Screen Size：屏幕越大，UI 元素越大。，如果屏幕分辨率较大，UI就会被放大;如果屏幕分辨率较小，UI就会被缩小;

- Constant Physical Size：使 UI 元素保持相同的物理大小，而不考虑屏幕大小和分辨率。

小技巧：可以通过强制锁定分辨率和参考分辨率的方法防止UI出现错误。

## RectTransform:

描述:矩形的位置、大小、锚点和轴心信息。

RectTransforms 用于 GUI，不过也可以用于其他情况。 它用于存储和操作矩形的位置、大小和锚点，并支持各种形式的缩放（基于父 RectTransform）。

RectTransForm作为矩形用于控制UI元素的位置，大小和旋转，其相对坐标基于父物体的RectTransform。

### Anchors 锚点：

表示的是相对于父物体的位置计算方式

锚点可以是一个点、一条边、或者是一个面 Unity预设了16种锚点

![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221126002617.png)

你也可以按照自己的需求手动设置锚点

- AnchorMin 相对于父物体Rect计算的锚点左下角，使用的是线性系数，取值为(0,1)

- AnchorMax 相对于父物体Rect计算的锚点右上角，使用的是线性系数，取值为(0,1)

### Pivot 握点：

矩形操作的“中心”，使用的是线性系数，取值为 $(0,1)$ 旋转、缩放、锚定坐标将会使用定义的握点计算

![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221126004304.png)

![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221126165841.png)

### Offset 偏移：

用于表示矩形相对于锚点的位置（实际坐标值）

- OffsetMin 矩形左下角相对于左下锚点的偏移

- OffsetMax 矩形右上角相对于右上锚点的偏移

这个在Inspector是隐藏的，而是通过Anchor计算了另一套方便理解的参数进行填充

![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221126164431.png)

### Width & Height <=> SizeDelta

SizeDelta= = offSetMax - offSetMin

在锚点情况下，sizeDelta.x表示该UI的宽度，sizeDelta.y表示该UI的高度。 在锚框情况下，sizeDelta.x表示锚框的宽度与该UI的宽度的差值，sizeDelta.y表示锚框的高度与该UI的高度的差值。 sizeDelta在锚点情况下表示的是该UI的size（大小），在锚框的情况下表示的是该UI的Delta（差值）。

控制矩形的大小 在Inspector里面是以Width和Height给出，在脚本里面是以二维向量sizeDelta给出

调整大小一般在这里设置，而不是调整Scale

### Anchored Position 锚定坐标

Rect Transform组件通过给定的锚点（Anchor）和给定的握点（Pivot）计算的坐标

### 继承自Transform

- Position：物体的Pivot相对于父物体的左下角的坐标

- Rotation：物体相对于Pivot的旋转

- Scale：与Transform的Scale基本无异

- ...

### 修改形状的多种方法

- 修改变量Offset

- 修改变量SizeDelta

- 函数SetInsetAndSizeFromParentEdge() 可以根据父物体的Edge（某一边）去布局。其中第一个参数就是用于确定基准的边，第二个参数是UI元素的该边界与父物体该边界的距离，第三个元素是设定选定轴上UI元素的大小

- 函数SetSizeWithCurrentAnchors(Axis axis, float size); 第一个参数用于是水平轴还是垂直轴，第二个值决定width或height

使用这些方法可以在代码里修改RectTransForm的形状。

## 什么是EventSystem：

EventSystem是Unity自带的事件系统。在UI部分里，EventSystem主要实现按钮点击响应、拖动拖把等功能。

## 绑定事件的方法

### 在组件的Inspector添加

![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221126103144.png)

↑：简单粗暴，可视化；内置了按下特效等等一系列的UI动效 ↓：当物体多起来调试成本高；缺少了一些“动态”

### EventTrigger

![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221126103446.png)

用起来和上面的Button一样，不过它可以选择实际的行为类型

↑：简单粗暴，可视化 ↓：当物体多起来调试成本高；缺少了一些“动态”

### EventSystem配合代码

↑：具有一定的“动态”，也能够使用组件的UI动效；代码集中书写 ↓：代码量大

```c#
{  
    var button = GetComponent<Button>();  
    button.onClick.AddListener(() => { Debug.Log("Button Down");});//按钮组件订阅了这个事件,当按下按钮后，会执行里面的代码  
}
```

### Mono类接入接口

前置知识：面向对象编程——接口

```c#
public class Show : MonoBehaviour, IPointerEnterHandler, IPointerExitHandler  
{    
    public GameObject gameObject { get; set; }  //获取一个游戏物体  
    
    private void Start()    
    {    
          
    }    
    
    public void OnPointerEnter(PointerEventData eventData)  //当鼠标进入这个物体之后，执行这个函数  
    {    
        gameObject.SetActive(true);  //使物体显示  
    }    
    
    public void OnPointerExit(PointerEventData eventData)  //当鼠标离开这个物体之后，执行这个函数  
    {    
        gameObject.SetActive(false);  //使物体消失  
    }    
}
```

## LayerOut

Unity再带的UI自动布局模式，当你创建UI物体时，使用LayerOut可以让UI自动布局并进行排版。

## LayerOutGroup（布局组）

可以对一组元素进行动态布局，这里说的动态是，组内元素数量发生变化时，Layout Group可以智能的帮助你重新排版。

## 几种布局

## Horizontal Layout Group：

即水平布局方式，子元素只会按照水平的方式排列，就算子元素太多超过父元素也不会换行排列

## Vertical Layout Group：

垂直布局的方式，与Horizontal Layout Group相对应，同样当元素超过父元素也不会换行：

## Grid Layout Group（网格布局组）：

这种方式的布局，就是可以让元素换行的布局，就是综合Horizontal Layout Group与Vertical Layout Group这两种布局的综合体

## Layout Group的具体使用：

首先对相关需要排版的父元素添加需要的排版类型，然后就可以在Inspector窗口里更改对应属性

- Padding：类似网页设计内边距

- Spacing：布局之间的元素间距

- Start Corner:类似元素居中、靠左、靠右等

- Start Axis: 沿着哪个主轴放置元素。在开始新行之前，水平将填满整个行。在开始新列之前，Vertical将填充整个列。

- Child Alignment:如果布局元素未填满所有可用空间，则用于这些元素的对齐方式

- Constraint:将网格限制为固定数量的行或列，以辅助自动布局系统

# Audio组件

## Audio Settings

控制音效系统的地方，现阶段看一眼了解一下就好了 
入口：`Edit => Project Settings => Audio`

![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221125193818.png)

## Audio Listener

当在场景中添加了这个组件才能够听到各种各样的音效，相当于游戏空间的耳朵 注意：**一个场景中只能有一个Audio Listener**

> This class implements a microphone-like device. It records the sounds around it and plays that through the player's speakers. You can only have one listener in a Scene.

这个组件是没有任何变量可以在Inspector设置的 设置音量和暂停使用的是静态字段（类名点出来）

{  
    AudioListener.pause = true; // 将音效暂停  
    AudioListener.volume = 0.5f; // 将音量设置为50%  
}

为什么是静态？这是单例设计模式的一种实现方式。关于设计模式，可以等到以后再了解。

## Audio Source

音源组件，相当于游戏空间内的喇叭

![](https://phantasia-piclib.oss-cn-shenzhen.aliyuncs.com/RealityWork/20221125203906.png)

想要让发出声音，只需要理解下面的变量

- Audio Clip 想要让这个音源播放的片段

- Mute 静音这个音源

- Play On Awake 在音源被初始化时（给AudioSource打勾）就播放，**如果是音效取消打勾**

- Loop 循环播放音频

代码控制

```c#
{  
    AudioSource source;  
    source.Play(); // 播放（从头开始）  
    source.Stop(); // 停止（进度设置为开始）  
    source.Pause(); // 暂停（保留进度）  
    source.UnPause(); // 取消暂停（从暂停的进度继续播放）  
}
```

# 预制体(Prefab)

## 简介

预制体(Prefab)可以类比为预制菜，其主要特点是规格统一且经过简单加工即可制成成品。我们可以把现实中的预制菜比作unity中的预制体，厨师加工预制菜的过程就是预制体实例化的过程，这里的厨师其实就是instantiate函数，通过函数将预制体实例化。厨师用预制菜制作出了一道成品菜，就是instantiate函数实例化了一个预制体，将该预制体呈现在场景中。

## 形式

预制体本质是将多种元素打包起来，这些元素可以是图片素材、函数、音效等。这些被打包起来的元素形成的整体就是预制体。

## 用途

用来制作场景中的需重复出现的物体，例如敌人、收集物、陷阱等。
生成子弹。

## instantiate函数(了解一下)

用于实例化预制体

```c#
public class Test:MonoBehaviour
{
	public GameObject prefab;
	private void Start()
	{
		Instantiate(prefab);
	}
}
```

同时也可以用Instantiate函数对预制体设置参数，例：

```c#
Instantiate(prefab,new Vector3(0,0,0));
```


# 考核目标!!!!!

+ 添加动画，并使动画能符合玩家操作
+ 相机跟随玩家
+ 让玩家可以在场景中吃金币等收集品
+ 使用Prefab快速使用和更改重复物品
+ 添加简单的UI(收集品计数器)

# 进阶

- 给角色添加音效，给游戏添加BGM(为什么要演奏……😭)
- 加入更加复杂的UI(暂停界面，调整声音大小)
- 自由发挥(例如添加敌人，给主角更多的能力)



# 补充资料

[Unity教程:背包系统02:GUI 图形界面设置_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1mJ41197EV/?spm_id_from=333.337.search-card.all.click)
