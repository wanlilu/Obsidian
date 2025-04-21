
# Breakout 地图地形地势自适应道路

coenjin(金沛沛)

创建，最后修改于2023-08-07

**目标：4x4 地图玩家活动主要是以道路空间为核心的赛车活动，因此需要在编辑道路时，可以自适应地调整地形，来适配道路，生成一个舒适的道路截面空间。**

# 相比之前主要做的改动

1、**道路升级**（新版道路，较好的贴地效果，支持 planeRoad，较好的边缘过渡效果）；

2、**path拾取道路曲线**，path falloff 效果优化；调整 path 的 falloff 可以更好地与地形过渡，调整 path 的 width 可以使得 path 压平地形的范围更大；

3、**surface blend 支持拾取 道路 和 poi 点**，让地形地势可以简单适应 道路 和 poi 所在高度；

4、调整了 layer 的顺序，path 现在优先级会比 山体/lake 更高，因此**将 Road 调整到 Mountain2/lake 上**，这样的好处是道路附近的地形会更自然，但有些山体可能会被 path 压低，因为 path falloff 和 surface blend 的影响（相对比原来要矮一些），这个可以按照 需求/设计 调整 ring 组件来调高山体；

5、调整 HighRoad 与 POIs 的顺序，**将 POIs 放在 HighRoad 上**，因为 道路工具 本身会产生一定的 falloff ，如果 poi 与 道路 挨的很近，就会导致 poi 的地形被道路影响，因此将 poi 往上移，并且在 poi 的 region mask 上添加一张道路的 mask（自定义，带一点falloff），用来不影响道路mesh下的地形，并且还可以融合的比较好；

# **基本操作步骤**

1、在 HighRoad 层中**调整道路曲线**效果；一些关键参数如下图所示，包括**道路宽度、道路压平mask区域宽度、道路falloff宽度**等；

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-742c2b2332c3c01e02dda5dbc1239b4b.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-286e757c40c34f89db797ff9f9e33310.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-d05f38a2a9eba37a6093fa363a12b2ab.png)

2、在 Path 组件层，点击 External Spline Actor，选择道路曲线， 可以是通过吸附按钮，选中场景中道路的SplineActor，或者也可以下拉菜单选中道路的SplineActor；

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-950aa7019af19d3e301ccb7d894aaae5.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-17cbf3e4885f9b69f4bfe9f6be3f1411.png)

选择好了之后，Path 组件会基于道路曲线生成一条新的曲线，用来做道路对地形的 压平 和 falloff。但目前 Path 对于道路 spline 的转换是不完全的，这个转换过程是无法对路口处的控制点进行转换的（正在想办法解决中），目前更快的方法是在路口控制点附近，手动添加一个非路口控制点

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-073b1efbaa60053bbb92f4b5de573d04.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-b4440817c931e0a0de255285588ed6ca.png)

另外，在道路 spline 转换成 path spline 之后，可能 width 和 falloff 的参数会改变成默认值。这个可以手动再设置回来。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-bdf0da7516e41477abd07a66b7b7f5ec.png)

3、在 Terrain\_Erosion 层上，选择侵蚀组件，点击cook，重新侵蚀。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-29192ab131efcbc1d68858a09e1956bd.png)

如果远端侵蚀失败，可以尝试本地侵蚀。先安装 World Machine，然后勾上 Use Local Cook，再点击 Cook 即可。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-47638520c436ffc8e0d8b1f4d403bdc0.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-9330126f93416666a0b3e79c433cffa8.png)

4、因为地形有修改，因此需要更新一下草和植被的散布。这一步就点击一下 stack 面板右上角的 **草** 按钮即可。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-fb7f4825cae684c63c7d855b6580d9c7.png)

# **地形地势自适应道路做法核心逻辑**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-0f93bfe4b5ab746efd6232ed66515f24.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-f08b4a4e4f1b8f4a5504463808d7e870.png)

最基本的思路是通过 **Path/Surface Blend** 来调整道路与周边地形的过渡效果，针对不同尺度进行过渡（大尺度/小尺度）；可以通过下面两张图对比 **关闭/启动地形过渡** 前后的效果。（所有地形过渡的效果都是做在 Road layer 上）

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-e402ec2baed45c85f3cbb08ccaa6e35b.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-0b4e765d636da6790273b9f9c28ef6de.png)

## **Road Layer**

1、核心是通过 Path 组件对道路曲线附近的地形作一个较大尺度的 falloff 效果。

首先先获取 道路曲线，获取方法参考 基本操作步骤中的第二步，通过拾取道路工具中的曲线来生成 path 的曲线。

然后调整道路的 width 和 falloff 值，这两个值会影响 path 组件影响地形的基础效果。然后注意将 Mixing Strategy 改成 V2 模式，V2 模式是改良之后的 falloff 算法，效果更好。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-4ad5333091e94a19187f63d6a488a2c4.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-ae3288eec6ca14626ed0ba0a1b08de18.png)

2、在原本 path 生成的 mask 基础上，增加一个 RoadSmooth，基本效果是为了得到一张从 道路边缘 开始值为 0 过渡到 1 再过渡到 0 这样的一张 mask，目的是为了在给道路周边区域做效果的时候，可以与道路和道路附近的山体有一个较好的衔接过渡效果。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-bc7aeae669fb2948be05e626b16f53ed.png)

3、利用上面这一步得到的 RoadSmooth mask，来对 path falloff 之后的地形作 smooth，目的是为了平滑道路周边因为falloff算法而产生的一些小尺度的褶皱（对比下面两张图，关闭/启用 smooth 的对比效果）

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-246e3d0ee81f439649a6729ad2e9d580.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-6e98fb0bd4fbed9b56d3a998bb3d8b08.png)

4、用 surface blend 分别处理一下 poi 和 path spline 上的地形过渡效果。（这一步属于微处理）

针对 pois，在添加 surface blend，启用 input 模式，勾选对所有实例起效，设置合适的 step size，并添加 RoadSmooth mask，让surface blend只作用于道路附近过渡区域。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-5e1faed17c140d5621be97dca1363718.png)

针对 path spline，在添加 surface blend 之后，启用 input 模式，添加 Path 的 spline，然后设置合适的 sample size，以及合适的 step size，并添加 RoadSmooth mask，让surface blend只作用于道路附近过渡区域。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-db07af6c067d3081787dd047ab43652f.png)

## **HighRoad**

在经过上述操作之后，地形跟道路的衔接效果就已经较好了，HighRoad 层主要处理的是 道路mesh 的生成，以及 道路mesh 所在区域的地形压平。

新版道路对于道路mesh与地形的贴合效果做了较大的优化和改善，默认参数即可达到一个不错的效果，不过可以将 height Ext Width 和 Falloff Ext Width 参数调整一下，来做出更好地过渡效果。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-6da57c414e24f93441b832292096d8d3.png)

# 道路 与 POI 之间相互 falloff 影响的解决办法

道路工具本身为了与地形有一个较好的过渡，都会给一定的 falloff 值，比如 1000（10m），但是 POI 也会因为同样的原因给一定的 falloff 值，比如 2000（20m，因为 poi 内部地形会更加复杂，设置更大的 falloff 值才能做到比较好的过渡效果）。因此当 道路 和 pois 挨得很近时，就会产生相互影响的问题。

这个问题有几种方法可以解决：

1、设置 **region mask**

4x4 地图中道路会先生成，因此道路在下方，poi 在上方，这个时候可以在 capturedPresetSpawner 组件上设置道路所在的mask作为 region mask，这样 poi 影响地形的效果就不会影响到 道路mesh 区域

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-8701e7d9857548698362a6951fdd6f06.png)

另外也可以自己制作一张 mask，来处理一下过渡效果，如下图。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-08e88d3e24fdf1cbbe9c3a4b34f578c3.png)

2、添加**手工层**，手动修复

第一种方法可以很好地避免 poi 影响道路所在地形，但设置 region mask 之后可能会导致 poi 本身的地形受到 道路 falloff 的影响，原因就是在道路所在falloff区域上摆放了 poi ，如下图。这种情况很难自动化地处理，因此最好的方法就是在放置 poi 之后加一个 手工层，来手动修复这些地形区域。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-78b20475a0b421686f20aeb623bfccf0.png)

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

Breakout 地图PCG制作思路

2023-06-26 coenjin

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-48f34ae457aeafc603ab0a403f103594.svg)46
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-0b95f0082c86623bb3ccf41eddc04c8b.png)15
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699112-d56c24c81b5e5f02b023f9382e1ca21d.svg)
