
# Breakout 手工层高度Mask和权重Mask分离的手工层升级指导

carmackzhu(朱宇航)

创建，最后修改于2023-08-20

## 前言

为了让手工层的高度绘制区域和材质绘制区域相互不受影响，Falcon手工层进行了**高度Mask和权重Mask分离的优化**。与之对应, 之前使用过手工层的地图得进行一定手动的升级(升级工作量极小)，

下面是Falcon手工层升级的步骤。

## Falcon地图的手工层升级步骤

\[1\]把项目的Falcon插件更新到具备“**高度Mask和权重Mask分离**”的版本

\[2\]打开项目，做到四个检查

1.检查地图合成效果和之前是否一致？

2.检查手工层是否同时具备Region/Height/Weight三个Mask, 如果不是，则是异常。如下所示:

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-38b0316d8538a3ea007ce001bbf9688f.png)

3.检查手工层的RegionMask是否完全被清空，如果不是，则是异常。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-9f4cd0f6e85ec606e6c5aace7743ed15.png)

4.检查手工层的Height/Weight Mask是否相同，并且这两个Mask和没升级前的RegionMask是一模一样

**HeightMask = WeightMask = 未更新前的RegionMask**

如果不满足，则是异常。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-d65e958f652ac0a46d98226678f10d85.png)

\[3\]如果上面的检查都没问题，则直接在SVN提交手工层代表的地图关卡. 比如手工层**Land\_GreenGround\_Repair**

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-8d620881bcf04a80ceebb0be5b8967a8.png)

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-f2503a0b3130e1891ffe5ac284fca6bc.png)

优而赞之，手有余香

成为第一个点赞的人

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-48f34ae457aeafc603ab0a403f103594.svg)37
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-0b95f0082c86623bb3ccf41eddc04c8b.png)12
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704699190-d56c24c81b5e5f02b023f9382e1ca21d.svg)
