
# Breakout 公路风格道路构建方案

mugongliu(刘松)

创建，

coenjin(金沛沛)

修改于2023-09-20

# 公路风格道路构建逻辑

现实世界中的公路构建逻辑，主要会考虑两个点：**1、公路建造成本；2、汽车在公路上的驾驶体验；**

因此想要构建真实感的公路效果，就需要遵循这两条原则；而这两条原则，延伸出的具体逻辑有以下几点：

*   为了降低公路建造成本，公路需要搭建地**尽可能平直，减少频繁的拐弯和起伏，拐弯尽可能集中在一处解决**；
    
*   为了汽车在公路上能够有一个比较好的驾驶体验，一方面也需要公路尽可能平直，另一方面还需要考虑公路的**上下坡限制**和**拐弯半径限制**，参考下图，**真实世界中的公路坡度最大为 10度 左右，拐弯半径至少 10 m**，游戏世界中如果有特殊设定可以适当调高；由于这个约束，山地地形上才会出现**盘山公路**这样的效果；
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-91751c5c7e9661ab934256481704d6e5.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-4c146c43c07f1c1fe6d781384524e89f.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-046e7547fcfa0ceebb33fe78a7b86eb0.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-520fbf02da66b21542f385bcc98e7bd3.png)

## 使用RoadXSystem工具手动构建

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-ac489b08608ba906d989af65cb045700.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-30a738e6b36dde2780aaaa6854c8c56a.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-adc00a3e7e08eaeb0da02fac8f2bad20.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-311cd4d41322c1efb312a305d3879fba.png)

使用 RoadXSystem 工具手动构建的道路，自由度较高，内置较多与道路相关的功能，可以做出丰富的道路效果。上述约束规则下的公路效果，也可以通过RoadXSystem轻松构建。

RoadXSystem 工具文档：

模组道路

基本步骤与方法如下：

1、首先确定道路走向，即道路的**起始点和结束点，**这些点可以根据 POI 等一些地图上的关键点来确定；

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-e14b01d2caa5e8e09698ab8b1d010f1c.png)

2、观察起始点和结束点之间的坡度差距，如果直接连接**坡度比较陡峭**，则需要向左右寻找合适点来构建**盘山公路，**控制整段公路的坡度在合适的坡度范围内。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-2c9bde45e92ea023be759b05608293a9.png)

3、观察拐弯半径的限制， 在拐弯点前后添加**拐弯控制点，**调整一个合适的拐弯半径。拐弯半径可以通过**两个拐弯控制点的距离**来控制，另外也可以通过控制**拐弯控制点的切线长度**来控制。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-4c11ba106127a9d35b2aa0ad2ab08650.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-385051ddb89264880d9cd5b95461d8d0.png)

4、尽可能**将除了拐弯段之外的道路段拉平直**，平直的路段建造成本最低，并且汽车驾驶体验也最好。基本逻辑是**通过该道路段前后两个点的切线方向，使切向方向指向另一个点。**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-547f69b266ca4e8a2bffb50e9ad0bcc0.png)

这个效果可以通过选中道路段，右击 **AlignToLine** 命令来快速实现；

bandicam 2023-08-08 16-56-49-947.mp4

## 使用寻路算法构建

在 Path 组件和 RoadXSystem 组件上都有寻路模块，寻路相关参数说明可以具体参考以下文档：

寻路（Path Finding）

由于当前版本 寻路功能，在 寻路结束之后，需要进行 resample 才能输出曲线结果，resample 这一操作是破坏性的，因此这里建议**用一个 Path 组件来存储 原始用于寻路的初始曲线数据**，并在其中进行寻路预览，在调整好寻路参数之后，保存为预设，然后使用 RoadXSystem 组件拾取这一初始曲线，导入寻路预设，重新进行寻路生成。

以下是具体操作步骤：

1、创建 Path 组件，并在其中绘制用于寻路的初始曲线，这里需要关闭 **影响地形** 和 **影响mask** 的选项；

在绘制 Path 曲线时，可以进到 **Top 视图** 中进行绘制，便于观察；

在绘制时，可以把 **Stack 合成** 暂时先关闭，然后**将 Path 前面的层和组件**也关闭，避免后续层对地形等做出修改，造成绘制曲线时作出错误的判断。

然后开始绘制用于寻路的初始曲线，这一步只需要绘制大致的曲线走向即可，即确定 **寻路需要经过的点**、**寻路的方向（handle表示）**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-5d478ff2aff9803dec33430028af7be9.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-1f7bee59f41a308fb9392a76527bb598.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-11d8bcacdc33996ee09a336d2ed11ff7.png)

2、绘制完大致的曲线之后，打开Stack合成，然后对 Path 所在 layer，右击点 **update collision 更新地形碰撞**，地形碰撞会影响后续 **曲线控制点 捕捉地形** 以及 **寻路网格的生成**。

完成上述操作之后，点击 **PathFinding Enable** 进入寻路，下图有寻路面板中的一些参数说明（更详细的说明，请参考文档：

寻路（Path Finding）

），选中曲线右击 **select all segments**，然后点 **Find**，即可以显示寻路结果。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-870a75e2394e7de887a60809abf9ea23.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-80e5f9091c2dbb571c584ea9149044ce.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-cae2c76466bca8bf574ba3d30cdef469.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-5ef4d36c10d6d34b9f66478343677675.png)

3、可以一边调节曲线，一边预览寻路结果。

bandicam 2023-09-19 20-02-01-182.mp4

其中控制点左右两侧的 Handle 会影响寻路的方向，可以右击控制点，打开 **Allow Discontinuous Splines**，来对控制点两侧的 handle 进行分开调节。handle 的长度会影响该方向的影响强度

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-32fb5da917ed7a94aee1c87aa78b4822.png)

如果在调整控制点时，遇到控制点不贴地的话，可以右击 控制点，然后点击 **snap ground**，即可贴地。

如果想要对当前所有控制点都重新贴地，也可以右击控制点，点 **Select All Point**，然后右击 Snap Ground。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-5901ecf10f947d404f2f314fa5e5d5cb.png)

4、调整好了之后，可以将当前配置**保存成一个配置资产**，这样后续在对同一根曲线进行寻路时，使用同一个配置，就可以得到相同的寻路结果。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-d3d079f68efc40d73ce710ef783db694.png)

5、因为需要保存 Path 中的初始曲线，因此 Path 的寻路结果只是作为预览，用来观察结果并调整曲线，如果对当前寻路结果满意，则**直接关闭 Path Finding**，然后新建 RoadXSystem 组件，在 **BuildFromOuter** 中选择当前这个 Path 组件，并且点击 **BuildFromSplines**，这样就可以将 Path 里面用于寻路的曲线数据拷贝到 RoadXSystem 中

为了避免在编辑器界面中，有太多的曲线控制器干扰，可以在 BuildFromSplines 之后，把刚刚使用的 Path 组件隐藏。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-b2cbbd7ce530f7f5df72a8d3ab980c6b.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-ee48afacdfe95210f5d18d431ac19062.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-5ef415f1b9d5fce51a197bd607a21011.png)

6、在 RoadXSystem 组件中，进入 PathFinding 界面，**导入**刚才在 Path 组件内寻路之后保存的配置文件

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-c1c03aa3a04b30cf0124df388922badb.png)

然后右击道路曲线， **Select All Segments**，然后点击 **Find** 重新寻路，得到的寻路结果会是与刚才在 Path 中的寻路结果时一模一样的。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-cc5569c834f9c90df52d0a08faedce07.png)

在这一步寻路时，可以**对寻路结果打开 snap ground**，这样寻路结果就会从新进行贴地

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-a9afba5128abd9c000064045fe9faaf1.png)

7、生成寻路曲线之后，需要通过 **Resample** 才能导出成可用的曲线。调整 Resample 右侧的参数，这一参数表示寻路曲线上，多少间距插入一个控制点，该值越小生成的点越多，拟合的也越准确，但是后续调整起来也会更加麻烦。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-60f666b7c7c3542e3a37fd9992e45a9f.png)

调整好一个合适 resample 间距值之后，点击 resample，即可生成最终的道路曲线，然后**关闭 PathFinding**，才会生成最终的道路 Mesh

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-c53d17585e5015c0c88e1e9f60269258.png)

8、然后在 RoadXSystem 组件中调整道路宽度、使用的模组和材质，以及输出 Mask 等参数。

### 注意事项

1、如果遇到寻路网格生成错误，可以点击一下 refresh 来进行重新生成

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-055b501c77560b75b3ec6eeb2a184991.png)

2、如果在 RoadXSystem 中遇到控制点两段道路 Mesh 没有接上的情况，是因为控制点当前处于 **Allow Discontinuous Splines** 的状态，想要修复，可以点击控制点将 **Allow Discontinuous Splines** 状态关闭，如果想要控制全部控制点，则 **Select All Points**，然后右击关闭。

调整之后如果道路 Mesh 没有更新，则可以稍微挪动一下控制点，即可以更新。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-742bd362dd6585a057096d71f5e61f97.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-47e1cf7b34a84d368a385d2b22bebd99.png)

# 道路工具开发进度

此处会贴一些TK项目组支持的道路feature开发与bug修复的进度.

为更好的向您提供相关服务，请前往腾讯文档进行授权

前往授权

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

寻路（Path Finding）

模组道路

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-48f34ae457aeafc603ab0a403f103594.svg)102
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-0b95f0082c86623bb3ccf41eddc04c8b.png)17
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698750-d56c24c81b5e5f02b023f9382e1ca21d.svg)
