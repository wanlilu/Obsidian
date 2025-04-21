
# Breakout 植被撒点效果生成逻辑汇总

coenjin(金沛沛)

创建，最后修改于2023-10-19

植被撒点效果, 主要是使用 **RealtimeFoliageScatter** 工具来实现的, 该工具使用的基本流程分成三步:

1.  创建物种 **Spawner**;
    
2.  准备分布的**Mask**(使用VariantMask或者使用ComponentMask);
    
3.  在 RealtimeFoliageScatter 工具上**配置分布参数**, 实现撒点效果;
    

在 RealtimeFoliageScatter 组件中, 可以快速地实现点云的**聚集/圈层**效果, 以及点云之间的**依赖/互斥**效果.

在这篇文档中, 主要介绍 Breakout 地图中植被撒点效果(如树木/灌木/岩石等)制作的逻辑, 对于如何配置组件, 以及参数说明, 请参考以下文档:

*   工具详细文档请参考:
    
    实时植被分布 Realtime Foliage
    
*   工具教程文档请参考:
    
    05-撒点
    
    NRC 8x8 地图温带植被程序化生成
    
*   如果想要实现程序化程度比较高的撒点效果, 需要比较好地掌握 **VariantMask** 组件的一些相关用法, 用于调出好的分布Mask, 该工具的使用可以参考文档:
    
    Breakout 地形权重处理逻辑汇总
    
    Mask混合 Variant Mask Modify
    

# 植被散布依赖的基本逻辑

植被散布逻辑体现在许多维度, 比如**植被分布的Mask生成逻辑**, **植被分布的Pattern类型,** 以及 **植被的剔除逻辑** 等等, 这些也都是做一个不错植被分布效果需要考虑的基础.

## 分布Mask生成逻辑

### **依据地表权重**

比如在草地, 土地上会有不同的资产分布, 这种方式也被用于地表材质中快速生成 GrassType, 而这种逻辑同样也适用于程序化植被分布;

在 VariantMask 组件中, 在 Mask 下添加一个 pass, 可以轻松地获取任意地表权重的Mask, 比如下面获取 草地(layer7) 区域的 mask.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-d010c409bb2b3715557c3b79024c76b0.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-ecc2d64cb0882483f86c4dbc8521dbf7.png)

### **依据地形坡度**

比如一般树木/灌木等都会在坡度比较低, 也就是比较平坦的区域生长, 而比如崖壁资产, 就适合生成在坡度比较高的区域;

在 VairantMask 组件中, 在 Mask 下可以添加一个 Slope pass, 可以快速计算当前地形的坡度分布, 计算Slope有**计算灰度**和**二值化**两种计算模式.(详细文档参见: [https://iwiki.woa.com/p/1440192351#Landscape-Height-Range-/-%E5%9C%B0%E5%BD%A2%E9%AB%98%E5%BA%A6](https://iwiki.woa.com/p/1440192351#Landscape-Height-Range-/-%E5%9C%B0%E5%BD%A2%E9%AB%98%E5%BA%A6 "https://iwiki.woa.com/p/1440192351#Landscape-Height-Range-/-%E5%9C%B0%E5%BD%A2%E9%AB%98%E5%BA%A6") / [https://iwiki.woa.com/p/1440192351#Landscape-Height-Gradient-/-地形高度过渡](https://iwiki.woa.com/p/1440192351#Landscape-Height-Gradient-/-%E5%9C%B0%E5%BD%A2%E9%AB%98%E5%BA%A6%E8%BF%87%E6%B8%A1 "https://iwiki.woa.com/p/1440192351#Landscape-Height-Gradient-/-%E5%9C%B0%E5%BD%A2%E9%AB%98%E5%BA%A6%E8%BF%87%E6%B8%A1"))

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-fa24fcfe268da7370ef4ef4770bacf89.png)

二值化的坡度计算模式

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-e90150ec1af77b462d023b4c9f38b63a.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-8d1dc2e4aa479e556fe01633bdf689ae.png)

带灰度的坡度计算模式

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-8edff5019d20d19a261298b5b439890d.png)

### **依据视野远近**

比如在游戏场景中的核心活动区域周边, 距离越近的植被细节量和物种丰富度一般都需要更加高一些, 而距离较远的植被, 一般只需要体积较大的树木即可, 用于填充远景.

比如 Breakout 需要跑载具, 因此核心活动区域会在道路附近, 就可以基于这一区域, 生成距离核心区域近的区域和远的区域, 这主要利用到了 SDF pass, 如下面两张图, 可以快速生成距离道路近的区域和远的区域, 并带有渐变过渡

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-289f9f8c00b3b08b5b43933bc148348a.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-54afabb4d872ef876ed18e96b6a2467d.png)

## 植被分布Pattern类型

### 均匀分布

即比较**均匀地分布**在 Mask 上

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-48c44c0ce7f8415f562589a6624431bf.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-8c05eb33b13a50f5bc98df11dd640c11.png)

realtimeFoliageSpawner 默认即是均匀散布的效果, 调整一个合适的密度参数即可实现

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-c67e717e81c4ad50f335918ec1590709.png)

### 成团分布

在一个大的区域内, 植被有明显**成团分布**的特点, 形成植被区域和留白的区域

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-d0138e9ea081c1133b4a2ce31f3aa5a1.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-75ec72943ac50580a10768a94f890991.png)

这种效果可以通过两种方式来实现, 一种是在 Mask 上 Multiply 一个合适参数的 Noise, 调整Noise的Scale来控制成团的大小; 另一种是直接使用 Mask 来分布, 在分布效果上调整 cluster 相关的参数

下图演示了使用 Noise 来制作成团的植被效果

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-1f63a87490f4b74c252bbf9a2dedc912.png)

下图演示了使用 cluster 的方式来制作成团的植被效果

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-c5e0ac8206bd35cd2b61907b622458c7.png)

## 剔除逻辑

在分布植被时, 需要考虑剔除逻辑, 一方面是为了**防止散布的植被资产之间产生穿插**, 比如树木和岩石; 另一方面是为了**防止植被生成到了不应该生成植被资产的区域**, 比如道路/河流/湖泊等区域;

剔除逻辑有以下两种方式可以实现

### realtimeFoliageScatter内部的剔除

在同一个组件内, 只要设置了 Exclude Radius(剔除关系下) / Depended Exclude Radius(依赖关系下) , 并保持Disable Exclude or Depend Below 不勾选, 即可在组件内实现物种之间的相互剔除, (详细文档介绍:

多物种互斥/依赖

)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-7732d166b8a8a123abe9f5231119debd.png)

可以可视化依赖互斥距离, 也可以同时打开多个物种的可视化, 来查看当前物种的互斥距离是否合理. **一般会将剔除值设置的跟当前物种外围差不多大, 或者稍微再大一圈.** (不过这个逻辑会根据物种而异, 比如树木就只需要根据**树干的半径**来设置剔除距离即可)

注: **因为地形精度的影响, 互斥依赖距离的计算并不一定会很精确, 因此如果有比较精确的控制需求, 可以适当将剔除距离值调大一些.**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-d6a54fcffa7319f8704c1063612e1081.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-3246f3a9190073071cd7535480f3962b.png)

### 基于 Mask 的剔除

有的时候**需要拆分多个 组件** 或者**在不同 layer** 上进行植被资产的分布, 这个不能利用到 realtimeFoliageScatter 组件内剔除的功能, 而需要使用基于 Mask 的剔除.

另外对于无法生成植被的区域, 比如 道路/河流/湖泊/建筑 等, 同样也只能将剔除逻辑做在 Mask 里.

在使用的时候, 一般 道路/河流/湖泊/建筑 都会生成对应的 Mask, 在植被分布 Mask 计算的 pass 中, 将这些 Mask 添加进来并 reverse range 进行反相处理, 即可对这些区域进行剔除(使用 subtract 也可以)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-f086e292ef625dc2b1dc7781f3704e9f.png)

基于 Mask 的剔除需要注意一个点, 就是后续所有的植被分布都会需要用到剔除 Mask, 为了避免增加太多重复的逻辑, **最好是将剔除Mask整合到一张 Mask 上, 随着合成流程不断往这张 Mask 上添加新的剔除区域**

比如在 Breakout 地图中, 所有植被共享的剔除 Mask 为 **Foliage\_Cull\_All**, 这张 Mask 在一开始的 Lake\_Foliage 中被创建, 将一些需要剔除的区域, 逐步添加进来, 比如 Road/POI/Lake/River 等, 用于当前 Lake\_Foliage 层散布植被时的剔除

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-f3d6e42dcb55c20bfc7d088981934ae4.png)

在 Lake\_Foliage 层的植被生成完毕后, 将一些不希望后续产生碰撞穿插问题的物种(如树木/岩石) **分布Mask/点云输出Mask** 也添加到 Foliage\_Cull\_All中(如下图), 用于后续其他Layer上的植被剔除

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-2bf031954aaa74a8bec10b23e609a9dd.png)

其他层也是类似上述的处理, 这样可以使得剔除的区域Mask能够尽可能被复用, 节省每次植被生成所需计算时间

注: Foliage\_Cull\_All 这种**通用的剔除 Mask 不一定能适应每一个物种的剔除逻辑**, 可以根据物种分布的特定逻辑来确定不同的 剔除Mask;

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-277fd83288762592c7946b21973e40c9.png)

## 植被点云导出StaticMesh

由于一些项目特定流程的情况, 需要 Foliage type 导出成 StaticMesh 来使用, 并指定导出到某个具体的关卡上

这个功能是一个全局的功能, 入口在 HandcraftFoliageHolder 上(如下图), 因此需要使用该功能, 需要在任意位置创建一个 HandcraftFoliageHolder 组件

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-7efec82765eb52b37ed8b082d3dd49e1.png)

打开后可以选择全图植被, 也可以选择具体某个植被组件(可以是 RealtimeFoliage 或者是 HandcraftFoliage 或者是小生态或者是植被Preafab), 这里选择一个 RealtimeFoliage 组件为例

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-cd8fdd325b768787e6dddb5f55261317.png)

然后选择具体的 foliage type, 可以按照 Spawner 来过滤

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-16f0455467b8e3b5bed41fae8aa453db.png)

最后选择导出的目标关卡, 点击 **将植被导出为StaticMesh** 按钮即可

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-4cb9f98f609b688a1e53d5d05eb0aef4.png)

# Lake\_Foliage(湖区植被)

湖区的植被, 首先先根据 草地 的地形权重范围来得到大致上植被可以分布的区域, 并剔除地图上不能生长植被的区域, 比如道路/河流等等

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-0b9dcaf9174941350756ac12c33915d0.png)

然后根据道路区域作为核心活动区域, 根据视野远近, 得到**近景植被分布区域**, 以及**远景植被分布区域**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-e6645eeb8973e146f3b9d17d4e7ceb07.png)

使用 Noise 来生成两种不同物种的 花 的成团的效果

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-e5e5ab60fa09ba941f27d4411af4df32.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-894736798280d33998a679cf73e3a32c.png)

同样也是使用 Noise 来生成两种不同物种的 湖边芦苇 的成团的效果

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-d85e33d0b2466853f85eca80e4edb36e.png)

然后分别创建 两个 realtimeFoliageScatter 组件, 一个用来生成远景的植被, 一个用来生成近景的植被

按照以下流程来配置需要的植被物种即可

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-2e1a6727f6fd55e6dc46363034eba12d.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-e528c76740b0fdf64d4ee1df7588cf3f.png)

根据上述计算得到的Mask, 来分布对应物种的资产即可, 基于上述生成逻辑, 调整 **密度** 以及 **Mask/Cluster** 参数, 即可实现成团的效果, 需要仔细调整的是每个物种的**剔除半径**, 因为地形精度的限制, 这个值可以比实际距离稍微大一些.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-26f689a06d0a8765c9e5b2aa0a9d8fb5.png)

在一些岩石物种里面, 可以配置输出点云 Mask, 用来更改岩石物种下方的地表权重

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-d22bbbc126f908cfc0f4a0dfef0b8e16.png)

# South\_Foliage/Core\_Foliage(荒野植被)

South\_Foliage 和 Core\_Foliage 的植被生成逻辑是基本一致的, 这里以 South\_Foliage 为例

首先还是先定一下植被的 Mask 分布, 因为荒野区域需要分成 **平原区域(Ground)** 以及 **台地区域(Platform)**, 因此需要使用 Height Gradient 来确定这两种不同的区域, 另外荒野区域在 **泥土地(Dirt)** 和 **草地(Grass)** 上都会分布不同的植被, 因此在生成相应 Mask 时, 都会加上相应的关键字

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-bd78e1f99e89e3d158b162fd25df84f1.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-d4c37f2e4668c240483f7331543d7bff.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-a405a073effd51710061aa026fd7d888.png)

然后基于上述计算得到的 Mask, 创建 realtimeFoliageScatter 来分布对应的植被物种

然后同样是输出岩石底部区域的 Mask, 然后更新地表材质

另外在得到岩石区域的Mask之后, 还可以在岩石周边分布一些杂草, 这里也同样区分一下平原和台地区域的Mask

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-87e29dfc079a0bce298b2d88e74f5c6b.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-645f6baa3b57befffbede17b5a803e8b.png)

# River\_Foliage(河流植被)

在河流区域附近需要单独配置植被生成逻辑

首先还是生成河流植被会分布资产的区域Mask

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-cfbe41551d62d3fa8bd8d5e8ad866ad3.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-83ea5ad6ecac73568d58b42e5218ab82.png)

然后基于得到的 mask, 来分布 岩石/树木/灌木资产, 参数中调整好密度和Cluster参数, 基本上就现在图中呈现的效果了

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-4fbcc20064136b0612bb0cced9046196.png)

# Road\_Foliage(道路植被)

道路旁边也会根据特定的需要, 来散布一定的资产, 主要是小的岩石

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-33f1da5ed0f2bde85a7087573fc97620.png)

首先依然还是先生成相关的 Mask, 这里需要根据 湖区 和 荒野 分布不同的岩石资产, 因此基于这个逻辑需要生成两张不同的 mask

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-460633241e3b24d0532510f31c7f536f.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-c4737c6fa29042054e4f99bc15fb6595.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-65fd015c95ac51dff142ad3aba2e70dd.png)

然后基于得到 Mask 来分布 湖区 和 荒野 区域两种不同的岩石资产

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-c63e7e03c31a4997a2593b6fb53919ef.png)

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

实时植被分布 Realtime Foliage

多物种互斥/依赖

Variant Pass

Mask混合 Variant Mask Modify

NRC 8x8 地图温带植被程序化生成

05-撒点

Breakout 地形权重处理逻辑汇总

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-48f34ae457aeafc603ab0a403f103594.svg)37
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-0b95f0082c86623bb3ccf41eddc04c8b.png)6
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699250-d56c24c81b5e5f02b023f9382e1ca21d.svg)
