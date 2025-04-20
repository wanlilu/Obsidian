
# Breakout 地图PCG制作思路

coenjin(金沛沛)

创建，最后修改于2023-08-07

**更新：**

*   20230625：地形自适应道路流程：[https://iwiki.woa.com/pages/viewpage.action?pageId=4008318564](https://iwiki.woa.com/pages/viewpage.action?pageId=4008318564 "https://iwiki.woa.com/pages/viewpage.action?pageId=4008318564")
    

**4x4地图制作的基本思路：**

复刻 MGSV 阿富汗地图 的地形地势效果，可以在地图上关闭上面层，看到 MGSV 阿富汗地图的高度图；

![](1704698874-96e4aa705a6929a45915a1c2977c6dbf.png)

在 MGSV 中游戏场景都是围绕道路，玩家活动范围和视野也是以 **道路空间** 为核心；

可以从下图中看到 MGSV 道路基本都是搭建在山体之间，因此所构建出的道路空间呈现 v 字型；

地形拆解参考图.png

![地形拆解参考图.png 0](1704698874-f9a921015849e7bfe3a9a2898401755e.png)

1/1

因此对于 MGSV 地形地貌的还原，**基本思路就是先构建道路，然后基于道路去拟合地形地势起伏，产生各种 v 字型空间。**

# Procedural\_Terrain - 地形地势塑造

## Terrain\_Base - 地形形态塑造

1、首先新建一个layer，将layer的混合模式调整为replace（将MGSV的高度图覆盖）。调整地形高度，使用flattern将地形调低到 MGSV 高度图的最低高度

![](1704698874-e1e1b8f07a756cf4c57097232b6e01e9.png)

2、使用Pattern构建一个从左到右升高的地形，然后用curve组件重映射高度变化

![](1704698874-b1400f9e5f56c779b052c3046eb0f518.png)

![](1704698874-4e2825a88706c1eca346bc41eb7e0b89.png)

3、使用noise往地形上添加基本的地形起伏，用于生成一些山包

![](1704698874-15f2dd712a4d277db295652c10b08bf9.png)

4、可以在 MGSV 高度图上看到右侧的山体要明显高耸一些，因此首先在 variant mask 里面添加一张 自右向左 渐变的mask，然后用这张 mask 作为 region mask，使用 noise 在全图添加一些山体，这样效果就是山体在右侧会高一些

![](1704698874-cb1279e55845f537a5b2a3df13ad2672.png)

![](1704698874-bb4f84578dfbb38399b6dc1c3c0ff3db.png)

![](1704698874-096e9663f8cc53e4794ba76b8d2b81d6.png)

5、使用 surface blend 组件对道路附近的地形做地势调整（需要先知道道路曲线的位置吗，然后将 surface blend 的控制点放置在道路曲线上），调整各个控制点的影响范围

![](1704698874-49b9ae7ec39e5b4d3aa7f7b44833aabe.png)

## Road - 道路空间地形形态塑造

1、使用 path 压平道路所在位置的地形，设置道路压平宽度，设置 falloff （包括 falloff 距离和形态）

![](1704698874-85277a4bdf1756385fdf32b7e8407d50.png)

![](1704698874-fb25b226b64a29e7f109cec081b2dee7.png)

道路宽度、道路falloff参数 也可以在 path 曲线控制点上逐点去调节。

![](1704698874-556c3e8a445dced4acf0af9f2f5cd9fa.png)

2、生成并管理道路相关的 mask。使用 variant mask，生成道路周边渐变的mask，用于后续制作地形细节效果时，可以不影响道路进行混合

![](1704698874-8cf74b62aee15b4783a0987af8daabbc.png)

![](1704698874-1d0ce47c13c85a4bcd6cf7ba35f1668d.png)

![](1704698874-7837cbef35ca341193384ca2c7b8ca31.png)

![](1704698874-922566a91f6d4b1d83fb20f5c11b55be.png)

## Mountain2 - 道路附近山体形态控制

1、使用 Ring 组件，绘制各个山体区域的范围，然后调整Ring组件参数，向内 falloff 形成山体效果，移动 Ring 组件可以控制山体的高度和坡度，使用falloff距离也可以控制坡度，按需调整

![](1704698874-de8658195f9cef6a9335eb53053becb2.png)

2、使用 smooth，平滑山体形态，将山体所在范围mask 输入到组件中作为region mask（即Ring）

![](1704698874-f660aed757e88f0d2cafebb95eb1a54f.png)

3、使用 Distort、ridge、rocky 添加山体细节，调整强度等参数，将山体所在范围mask 输入到组件中作为region mask（即Ring）

![](1704698874-1cbb53a278ca01be07f95c4077e24d9c.png)

4、生成并管理山体相关的mask，新建 mountain mask 来存储 Ring mask 上的数据，并清空 Ring（为了后续使用 Ring 这张 Mask 时，可以不与山体的 mask 相互干扰）

![](1704698874-6da21f9e515fbb45cad12ec3d6f5dfec.png)

## lake - 湖泊、河流区域地形塑形

1、制作湖泊湖面面片，然后使用 Ring 组件将所在地形向下压。

注：制作湖泊湖面棉片这一步，可以是用 Water 组件来模拟

![](1704698874-83190af53d8b7d6c5d28e2fb47aa5a1b.png)

2、添加湖泊所在区域细节，方法与山体添加细节方法类似，smooth → distort → ridge → rocky，注意需要 将Ring mask 输入到 region mask 上，以便于控制效果只影响 lake 所在区域

![](1704698874-b7f2f90227cafe5c1ef2c0909f37a8f6.png)

3、使用 mesh projection 输出湖泊面片 mesh 所在范围的 mask，用于控制后续地形权重、植被岩石资产散布等效果

![](1704698874-efb25988ff10792ddb65cde6767704f2.png)

4、跟Ring制作山体效果类似，新建 lake\_region mask 来存储 Ring mask 上的数据，然后清空 Ring mask。

注：下面其实没有清空 Ring mask，而是将 mountain mask 的数据重新给到 Ring mask，这样做是因为历史原因，早期做的组件都是没有将 Ring mask 单独存到 mountain 上，导致后续大量组件直接用 Ring mask 来制作效果，这里为了避免影响后面的效果，所以将 mountain mask 重新赋值给 Ring mask，但是正规流程应该是清空，后续使用 mountain mask。

![](1704698874-3b7ee66c58331cd31b6073c2d0ec310d.png)

## Random\_Mountain - 生成地形周边的随机山体造型

1、首先获取周边地形的mask，如下图所示，简单将道路、山体所在mask反向，并用sdf处理过渡即可，命名为 RandomMountain

![](1704698874-9d1cb9fd5e1c1f2033eae0c08403e21e.png)

2、使用 height offset 将周边山体地形整体抬高一些，让玩家在主要场景中的视野不穿越周边地形。

![](1704698874-a76aefed2c49e5f411a2d017388faa5e.png)

3、使用noise 初步构建山体起伏效果，然后使用 fills 减少山沟形态，再使用 ridge → rocky 添加山体细节效果。注：将RandomMountain输入给region mask

![](1704698874-5b520540a44e1ea9236d9fe1ff3d70df.png)

![](1704698874-e2b16559f878c68ad568c1832c2d8109.png)

![](1704698874-03f2e500d4eb9b68d96dc097ab5b9043.png)

## Terrain\_Modify - 中心山体/道路附近地形细节添加

1、首先生成需要用到的Mask，主要是一张后续给道路附近添加细节需要的Mask，因为不希望影响到道路地形，并且同时跟道路和山体区域有个较好的过渡，因此将之前做的 RoadEdgeSmooth 和 RoadTransition 两张 mask 相乘，得到 RoadEdgeTransition mask

![](1704698874-bfae819934976544e39869c92c99fb2a.png)

2、使用cliff，下压道路附近的地形，注：将 RoadEdgeTransition 作为组件的 region mask

![](1704698874-fd0ec578016f846554ff737f5bc3e074.png)

3、使用 smooth 平滑道路附近的地形，主要是为了去掉 falloff 所带来的褶皱瑕疵

![](1704698874-e41fdd500472600165a68624bb5f6356.png)

4、使用 distort 添加道路附近地形细节，并加一个 smooth，防止distort 产生的一些过头的效果，然后在这个基础上添加小尺度的 noise，用来增加地形细节，最后添加ridge、rocky，同样也是添加更多的细节，注：将 RoadEdgeTransition 添加到组件 region mask 用来指定影响范围

![](1704698874-694a1807d3efbb5ff1bcaed066259e32.png)

## Terrain\_Erosion - 添加地形侵蚀效果

1、因为erosion，会生成堆积效果，因此不能让erosion影响道路和湖泊这种处于地势低位的地形区域，所以需要生成一张 erosion 希望影响的范围 mask，大致上就是山体的区域，命名为 erosion

![](1704698874-84c281555fbaf7582ffcb4b3d7bb1915.png)

2、添加侵蚀组件，将模式调整为 advanced，然后将 erosion mask 放到 region mask 上

![](1704698874-c7aeba7db8b225a36cf0f8d94e6efbc8.png)

注：如果 erosion 组件右侧出现这个标识，表示下层地形有了改动，需要重新侵蚀

![](1704698874-331b6370555f290cc976d8d87a353f35.png)

3、利用侵蚀出的 mask （flow、deposition等）来添加地形材质效果

![](1704698874-31a077e443abbca16585a08c33285124.png)

## Terrain\_Material - 道路附近地形材质调整

1、因为侵蚀只在山体范围上侵蚀，所以得到的 mask 也只影响山体附近，对于道路附近地形材质权重还需要再调整

![](1704698874-34b294bfd4a742625fef3d076bb900ae.png)

# 人文构建

## POI

1、POI capture 出来的资产分成两部分，如下图，往PCG场景（包含stack的场景）中拖动POI时，只需要拖动 preset 即可，会在PCG关卡文件夹下 的 PresetInstance 文件夹下创建相对应的POI关卡，然后加载到PCG场景中。（需要注意，如果poi中有地形，在pcg中需要使用同样的地形材质，才能生成正确的效果）

![](1704698874-b5b7f3890f2cf1320854a2770271d3a0.png)

![](1704698874-a8cd73e82abfeb7c08a912098bf17407.png)

2、在pcg关卡中放置 poi actor之后，还需要添加 CapturedPresetSpawner 来使得 poi 物体能够正确生成，并且决定 poi 对象在 stack 上的合成顺序（会输出 poi mask 合成）

![](1704698874-22465fe3130370d8325b1e382c102f31.png)

## 道路

1、道路组件使用方式跟path组件非常类似，因为前置中已经使用 path 来修改地形，使得地形的过渡融合效果可以与道路衔接更加自然，因此现在的道路组件，主要负责生成道路 mesh、道路附近资产，压平道路地形，输出道路所在mask用于后面添加道路底部道路材质

注：道路组件仍然还在更新，等待更新完毕会替换为新版道路工具，会对道路贴地效果、道路工具面板做出优化。

![](1704698874-cc844aba71bd8c025831d5c6e6eb68c4.png)

2、根据道路组件输出的mask，修改道路所在区域的地形材质

## 

![](1704698874-1ed2eef3bb57d0980a9cff55f8ead102.png)

![](1704698874-bf422f48795b8efb46f51313347420db.png)

# 资产散布

散布植被、岩石等，待更新

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

Breakout 地图地形地势自适应道路

*   ![](1704698874-48f34ae457aeafc603ab0a403f103594.svg)74
    
*   ![](1704698874-0b95f0082c86623bb3ccf41eddc04c8b.png)18
    
*   ![](1704698874-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](1704698874-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](1704698874-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](1704698874-d56c24c81b5e5f02b023f9382e1ca21d.svg)
