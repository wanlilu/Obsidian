
# Breakout 地形权重处理逻辑汇总

coenjin(金沛沛)

创建，最后修改于2023-10-19

所有地形权重的处理都是通过 VarMask 组件来完成，也就是地形权重效果只跟 Mask 有关.

Mask 的来源有以下几种：

*   **手工绘制的 Mask**
    
*   **组件计算得到的 Mask**，如道路、河流等组件
    

基于以上Mask，在 VarMask 中进行一系列的 **Pass** 计算，最终得到各种衍生的 Mask，基于这些 Mask，可以来做各种 地形权重 的表现效果。（VariantMask组件详细功能文档：

Mask混合 Variant Mask Modify

）

在 VarMask 组件中，拾取其他 Mask（Context/Component等）是通过名称来拾取的，因此一旦 Mask 名称确定之后，想要改 Mask 名称，需要考虑后续是否有其他组件会调用这张 Mask，如果有的话，则改名之后需要适配，否则会出现效果问题。

# Breakout中的一些基础Mask来源

## Land\_Plan Layer 中的Mask

Land\_Plan Layer 中会提供一些基础的 Mask 来标注地图上的不同区域，因此整个层里面只有一个 VarMask 组件

点进 ComponentMask 中，可以看到预先**手绘的五张 Mask**，分别表示：

*   蓝色：Lake\_Region，即湖区
    
*   红色：South\_Region，即南部荒地
    
*   黄色：Core\_Region，即核心矿区
    
*   绿色：Station\_Region，即火车站区域
    
*   紫色：Peak\_Region，即山顶缆车区域
    

这几张 手绘Mask 只是大致表示一下这五个区域的分区，并不能直接使用，一个原因是不同区域的手绘Mask可能会有**重叠**；第二个原因是不同区域之间需要处理**过渡与衔接**。

这两个问题将在VarMask中通过程序化的方式自动处理。此外，除了这五个区域Mask之外，还会有一个**外部区域的Mask**，也需要通过程序化的方式自动生成。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-b6bcbf221e73a74a22b9f49479861fc1.png)

整体的处理思路就是，首先将所有区域视作 **外部区域（Outer\_Region）**，然后根据 Lake → South → Core → Peak → Station 的顺序从 Outer\_Region 中逐一将 Mask 区域分走，这样最后这六个区域的Mask都是独一无二，互不干扰的。

分走之后，分别对这六个区域，向外制作falloff效果，用来保证边缘地形衔接是丝滑的。

这样最终每个区域就会有两张 Mask，一张代表所在区域，即Region，另一张代表所在区域+falloff，即Region\_falloff。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-2c2cc74dc4b0817b4f8322448100b077.png)

# Breakout 地形权重生成逻辑

权重生成分成几种，分别是：

*   区域内的权重分布（会根据上述的分区mask）
    
*   特定配方内的权重调整，如河流、湖泊、道路等，这些会在区域权重生成的基础上，再对生成区域内的权重做调整
    

## 特定效果实现

### 基于Curve的灰度调节器（Clampping）和输出调节器（Remapping）

Curve pass 可用来做一些权重分布的灰度控制，以及输出值的控制，类似于 Ps 中的色阶/曲线工具( 详细参数可见: [https://iwiki.woa.com/p/1440192351#%E9%80%9A%E7%94%A8%E5%8F%82%E6%95%B0](https://iwiki.woa.com/p/1440192351#%E9%80%9A%E7%94%A8%E5%8F%82%E6%95%B0 "https://iwiki.woa.com/p/1440192351#%E9%80%9A%E7%94%A8%E5%8F%82%E6%95%B0") )

如下图中，在添加一些如 Noise 的 pass 之后可能会带有很多的灰度值，这可能是不想要的

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-b0b8cd02a6496d1d54453c30f8c024ac.png)

将输入值中的最小值向右边滑动，即可实现将灰度较小的值去掉的效果，即 0-最小值的区域灰度值为0。（如果对Ps曲线调节较熟悉的话，也可以通过以下方式达到同样效果）

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-009a263c0e1f3aa2f0abee43f05de5ba.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-c07b989d39c53475ef1df4f58ae066fe.png)

而将最大值向左边滑动，则可以实现将灰度为1的区域扩大的效果。即 最大值-1的区域灰度值为1。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-44c7d63881f508f903f0de063fde2520.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-6670d979e1c3ecd22914c0036a2aee17.png)

而控制输出值，将最小值向右边滑动，可以实现Mask最小值增大的效果，即 0-1 的值 remap 成 最小值到最大值

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-29b29941c729045b0c93c737900016d4.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-e99b94ee680c1ff3068b8eb2112650b5.png)

将最大值向左边滑动，可以实现 Mask 最大值减小的效果。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-4f027836d451a1babcc3801224757607.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-7bfc3dfb767227d423cde00d62b565ef.png)

### 基于Curve的HeightBlend

不同层的地形材质之间存在特定的**混合方式**，这个混合方式写在shader里面，Breakout地形材质使用的是 HeightBlend，即基于材质的高度来计算混合，这样可以超越地形精度的限制而产生更多的细节

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-b4e7605722104237b496f5af0fba54ff.png)

但是想要实现这样的混合效果，需要以特定的值来混合两个不同的地形材质层。而这个值可以通过 Curve pass 来设置。

以 Layer7 混在 Layer0 上为例，在VarMask中添加这两个Weight层之后，可以分别添加一个Curve pass，在下面的Weight层（即Layer7，下面的Layer覆盖上面的Layer）上调整输出值，即可看到不同的 HeightBlend 效果

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-0251f29ea7b875214b3e6b74a1901c18.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-8dbc7152fd6f5ffd03904f4f9d99424e.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-85eb00683a09976fee4629c003abd10f.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-e6d65232cef1ac9a9f5ad16d2aa297f0.png)

## 场景生成实例

### Land\_South、Land\_Core、Land\_Station

湖区的权重逻辑制作偏早期，Land\_Peak的权重逻辑沿用的是湖区的权重逻辑，这两部分后续补充介绍

Land\_South、Land\_Core、Land\_Station 这三层用的是类似的逻辑，即先侵蚀（侵蚀分两步，一个是大侵蚀，一个是小侵蚀），然后基于侵蚀得到的Mask，来分布地形权重。**这里以 Land\_South 为例**。

权重的生成逻辑都在最后一个 VariantMask 中。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-328a5848d81db67885295b4a0544819e.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-ed25e2681e6d17575fbfd0e68e16cf3c.png)

首先用 Layer11 荒野草地 打个底，这里不一定是固定用草地打底，也可以用崖壁打底，这里使用草地打底的原因是因为后续没有特定的逻辑来铺草地，而是有特定的逻辑来铺泥土、崖壁等材质，因此使用草地来打底。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-15051e9eb2834da7863d3f2d63c40a45.png)

首先使用大侵蚀输出的 Deposition Mask 来给大的泥沙冲刷区域，赋予一定的潮湿泥土材质，这里有使用 Slope pass 来针对性地给 **6-20 度坡范围内**的 Deposition 区域赋予材质，这是因为 Deposition 往往会覆盖大部分平地，如果把所有区域都赋予泥土，就会导致草地分布过少，所以限制了0-6度的区域，不要分布泥土。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-3ca1fda9695aac0ee0325bc84c2ebf4b.png)

对于 **崖壁** 区域的材质分布，较为简单，即选出坡度较大的范围应用 崖壁材质，而在崖壁材质边缘，使用小石子过渡渐变即可，过渡渐变的方法就是使用 Dilation 向外扩展，然后使用 Distort 对边缘做一定的扰动。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-413fd3a1e42f9a9fced781db0fab7355.png)

在小侵蚀得到的 Deposition 区域上添加泥沙材质，这个泥沙材质希望是分布在侵蚀滑落的一些区域，并且分布希望会随机一点，因此加了 Noise 控制，并且限制在 0-20 度的区域范围内分布

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-6851c3003dec5920886f1bba4b0225c4.png)

在下面这种大片的泥沙区域中，用 Layer11 的草地去打破一下，可以通过打开 Preview Layer Content 来查看当前层的 weight 分布。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-ea834f9ae0b75c6b014815778dc23c05.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-52aae7e0ddf9e50b4cc5767d0f4d760a.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-ed718bbc76f52229cb8242dd094e8460.png)

在小侵蚀得到的 flow 区域，赋予一些泥沙材质，这里的泥沙材质同样也是为了避免入侵到平地上，也是限制在 6-90 范围内。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-f714d61cbd572824e064a405ff7909ad.png)

为了让平原范围内，产生更多的草地，并且也不破坏原本的泥沙泥土材质变化，使用小侵蚀得到的 flow mask 区域范围，在低坡度（0-6度范围）内应用材质材质

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-ca8bd2f287f529f83f1a2591a6e95704.png)

在上述操作之后，会发现场景中很大的区域都被覆盖了 草地 和 泥土，这样可能会有较大的重复感，因此希望在这两个材质中引入 带有碎石的泥土材质，来打破这种单一感，因此首先是取这两种材质叠加的区域，然后使用 Noise 来得到其中随机分布的小区域，再加一些坡度限制（0-15度）和随机扰动

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-7e25e78291ab7ab5a70b8ad9c06cb242.png)

在上面的基础上，最后想要在泥土地上，随机分布一些车辙印，因为这种材质带有各项异形，因此需要使用 TextureAsset 来引入一张各向异性的贴图，并叠加 Noise 、 Blur、Slope 等Pass做一些随机的变化。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-0c1e98adfd768fcfdde50f280da1d6e2.png)

### Lake

湖泊有两个层，分别是 Lake\_A01 和 lake\_B01，这两个湖泊的权重处理是类似的

湖泊的生成逻辑, 是通过 Ring 组件来压平湖泊底部区域的地形高度, 然后通过 CoastErosion 组件来确定湖泊水面的区域和高度, 最后用 Water 组件来生成 水面Mesh. 在这个过程中, Ring组件和CoastErosion组件都会输出 Mask

Ring组件输出的是湖泊压平区域的Mask, 这张 Mask 可以表示湖泊底部和湖泊边缘影响的最小范围, 因此在后面可以向外 Dilation 一定距离, 用来表示湖泊整体的影响区域, 输出到 Lake\_A01 / Lake\_B01 Mask 上

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-d61de3f374c9531f4d32c6d558b18b48.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-08e837b5ff650773a815b801ac674bae.png)

CoastErosion 组件会根据设定的海平面高度来确定 湖泊的区域范围, 输出到 Coast Mask 上, Water组件会根据这张Mask生成一个高度一致的湖泊Mesh

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-8742decc8d822a0b9f66a4e5eb2a158c.png)

有了上述几张 Mask 作为源材料之后, 即可基于这几张 Mask 来生成湖泊边缘的地形权重

首先先用**草地材质**在 Lake\_A01 区域打个底

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-504f4c77f392bdefc673960a9d300f2d.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-8e05cf358909544a3c3582b4a0366a26.png)

然后在 Lake\_A01 区域内, 计算出坡度在 45-90 度范围内的区域, 赋予**崖壁材质**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-00064af2587863ddb4cb3a36f4c17991.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-b808a5bf513f0987a04625593cd6f26c.png)

然后基于 Coast Mask 在湖泊底部和边缘区域赋予**泥土材质**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-c0cedb4f7b9ca2a936faa61d73f46685.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-6e3d9bd456d0bc3868d713750340b762.png)

### River

River 的生成逻辑实际上与 Lake 是大同小异的, 首先也是通过 Path 组件压平河流附近的区域, 并且输出 River Mask, 然后使用 Water 组件模拟河流平面, 然后 Water 组件会输出水平面所在 Mask. 结合这些 Mask, 首先可以计算得到下列 Mask

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-e767d87089363a129e370c620ff1fb67.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-efdb6f4ba9a437063e755d1bb44f8272.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-da07c5e14666bc7d25540a65e66ef366.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-00bfb8b0315e10cb5a1c19b167d6b6bb.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-5638cc0665e3179a7ab0766bbfd7d2ab.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-4c8486d9bf5b73084f6a3c8ea8731fd9.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-4ff652256be80cbf138470b912de89d2.png)

计算出以上 Mask 之后, 即可用来计算河流边缘的地形权重.

首先做河流边缘的 **泥土材质** 分布, 使用 Layer3 和 Layer13 两种材质在 RiverSideMud 区域上进行 HeightBlend.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-ca72831aa205e292ecaedb0b0e3fa764.png)

然后做河流边缘的 **草地材质** 分布, 直接应用在 RiverGrass 上.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-559322c93ee7f7316f1d4319f6fd6cd3.png)

然后对 河流 边缘坡度较大的区域, 应用**小石子和崖壁的材质**, 这里小石子材质, 相比崖壁材质会用 Dilation 向外扩展一圈, 用来做崖壁材质的边缘过渡效果.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-cd63fe17fcaca1f64a753b025c5a7e61.png)

最后对 RiverUnderMud 区域(即河底区域), 应用**潮湿泥土材质**.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-72520e711a8f1772a70382e86c57c74d.png)

### HighRoad

HighRoad 层中，基本的处理逻辑如下：

*   在 RoadXSystem 中绘制曲线，可以是直接手绘，也可以是结合寻路来生成道路，在这个阶段会输出对应道路的 Mask。
    
*   在 VarMask 中调整输出的 Mask，得到道路边缘的区域 Mask。
    
*   在道路边缘的 Mask 区域上使用一定的 filter 来给道路边缘添加一定的细节。
    
*   结合 道路区域的Mask，以及道路边缘的 Mask，给不同的道路添加地形权重变化。
    

目前 HighRoad 中会有以下几种道路，分别输出不同的道路 Mask：

*   **RoadX**，主要公路
    
*   **DirtRoad**，手工土路
    
*   **Railway**，铁路
    
*   **DirtRoad\_Mine**，矿区土路
    
*   **DirtRoad\_6m**，6m宽寻路生成土路
    
*   **DirtRoad\_2m**，2m宽寻路生成土路
    

在输出 Mask 之后，需要对这些 Mask 进行一定处理，得到几张衍生的 Mask。

*   SideEffect 结尾的几张 Mask，会给到后续处理道路边缘的地表权重，如崖壁等
    
*   SideErosion 结尾的 Mask，用于在该 Mask 区域添加地形细节（filters）
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-8fa3b6f05ce9f87665267d6df99af753.png)

道路区域地形权重的生成逻辑，大致分成两部分：

*   道路附近的崖壁地表权重
    
*   道路处的地表权重
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-47665262053265ed92261151d9caae5f.png)

首先是道路附近的崖壁地表权重，需要先选出**道路附近坡度较大的区域**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-92d52b0478f8531ef3740171dc8f0771.png)

然后用三层地表材质，做出崖壁地表材质逐渐向外扩散的效果，分别是 最内部——崖壁，中间——石子，外部——碎沙

Land\_South 和 Land\_Core 因为地表相对比较荒，因此选择的崖壁材质 与 Land\_Lake 和 Land\_Peak 区域不一样。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-035b7ddc48136a781fcf3a22c83fcdc4.png)

然后下面是不同类型道路处的地表权重，虽然使用的层很多，但是基本逻辑是重复的，即一种类型的道路，会有一张自己的Mask，然后利用这张Mask来混合两种（Base/Upper）地表材质

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-813f73a87133934308030b420b33c5ed.png)

### Road\_POIs

Road\_POIs 层上主要使用沿道路分布地块建筑工具, 该工具会生成次级道路, 以及输出次级道路Mask, 地块Mask, 建筑区域Mask, 最后一级联通路 Mask. 然后基于这些 Mask, 可以赋予相应的地形权重.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-04d98bda5eb9f87f4fd981e169682025.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-bd8866554a87d8eee88fb49f5c92985c.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-703070980b52b8915f2faad08d9f25f3.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-553ef9e94b30f6c36d5f4b30d8039cdb.png)

首先是在次级路Mask周边赋予**泥土材质**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-f18d7bc34b464576d9359d6a28423e69.png)

然后首先修改 parcel mask, 在该 mask 边缘添加一些扰动, 减少原本强烈的规则感, 然后使用**石子和沙子**两种地表材质做 HeightBlend 效果

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-a48e46822d1ce9c27d7870d2a3bcf60d.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-4a309c2aeceecd3537d1444299d59858.png)

最后在 联通路 上赋予**车辙印**的地表材质.

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-5ac669473b3b22a7a7def2fd73b6ad80.png)

### Foliage

在植被撒点之后，也会存在修改地形权重的情况，比如分布岩石之后，在岩石所在区域，需要有一定的地形权重来呼应（如下图）。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-4c2b52da52622d1fc7874f8dd8864c1b.png)

这种效果首先需要在植被工具上, 输出点云的Mask

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-6e289ff5ff72c5cf9d9583a21bf11bec.png)

也可以通过FoliageType导出StaticMesh的流程, 导出植被的StaticMesh(参考: [https://iwiki.woa.com/p/4008947235#%E6%A4%8D%E8%A2%AB%E7%82%B9%E4%BA%91%E5%AF%BC%E5%87%BAStaticMesh](https://iwiki.woa.com/p/4008947235#%E6%A4%8D%E8%A2%AB%E7%82%B9%E4%BA%91%E5%AF%BC%E5%87%BAStaticMesh "https://iwiki.woa.com/p/4008947235#%E6%A4%8D%E8%A2%AB%E7%82%B9%E4%BA%91%E5%AF%BC%E5%87%BAStaticMesh")), 然后通过 MeshProjection组件来投射一个更加清晰的Mask.

因为在计算上希望尽可能投射次数, 所以这在 TK Breakout 地图上, 点云导出的 StaticMesh 都保存在同一个关卡上, 一并投射到同一张 Mask 上

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-a4a9fd6c8fc8d5b2f06ce8d77bd19842.png)

然后利用输出的 Mask, 赋予**沙石地表材质**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-e7ef7bb4d3b7538f34c20ec625cc2e38.png)

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

Variant Pass

Mask混合 Variant Mask Modify

Breakout 植被撒点效果生成逻辑汇总

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-48f34ae457aeafc603ab0a403f103594.svg)57
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-0b95f0082c86623bb3ccf41eddc04c8b.png)10
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704698437-d56c24c81b5e5f02b023f9382e1ca21d.svg)
