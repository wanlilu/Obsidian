
# Mask混合 Variant Mask Modify

machzhang(张诚)

创建，

randiliu(刘逸凡)

修改于2023-09-05

_采用_ [_Variant Pass_](https://iwiki.woa.com/pages/viewpage.action?pageId=1440192351 "/pages/viewpage.action?pageId=1440192351") _方式配置的 Mask Modify 组件，用来以程序化的方式修改地形权重和 Mask_

# General

通过组件的配置修改 Stack 中的 Mask 和权重，配置由层叠而来的层组成

组件会按照从上到下的顺序，为每一层根据其包含的 Variant Pass 列表计算出一个结果，并按照层的类型输出到对应地形权重或 Mask 上

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-70cf5336d813e3c751743595d65f9ff7.png)

*   ① 这一层的输出目标
    
*   ② 这一层的结果预览或权重材质预览
    
*   ③ 这一层的计算方法配置（Variant Pass）
    
*   ④ 这一层的混合方式（Variant Pass 结果与之前内容的合成方式）
    
*   ⑤ 切换预览模式
    
*   ⑥ 预览目标的选择
    
*   ⑦ 预览阶段的选择
    
*   ⑧ 其他的全局选项
    
*   ⑨. 刷新
    

# 对层的操作

*   **添加层**：向 Layer Data 数组添加新元素，并在出现的 Choose 下拉菜单里选择类型。层分为 Mask 层和权重层两种，代表计算结果的输出目的地  
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-6bfa67f1acc2373f1cfe53022ae486f1.png)
    
      
      
      
    
*   **更改层的内容**：编辑层中的 Variant Pass 数组。[Variant Pass 的详细介绍](https://iwiki.woa.com/pages/viewpage.action?pageId=1440192351 "/pages/viewpage.action?pageId=1440192351")
    
*   **删除，复制，调整顺序**：按照标准的数组编辑方法操作
    
*   **启用，禁用层**：单击层名左边的单选框
    
*   **改变类型（Change As...）**：改变当前层的输出目标  
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-5f34c90d45f1a26c88df4ee566592e9b.png)
    
      
      
      
    
*   **坍塌（Collapse Layer）**：将层的结果固定到到新手绘 Mask 中，使得输出不再随输入变化而变化  
      
      
    
*   **导出（Export Baked Layer）**：将层的结果保存到图片文件
    
*   **察看结果（View Baked Texture）**：在新窗口察看层的结果。也可以双击右边的缩略图
    
*   **预览**：单击层右边的
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-45ae4e4054c4e77621e013f8bc22db1e.png)
    

# 输出方式

请注意_**层的结果**_和_**最终输出结果**_之间的区别，前者来自 Variant Pass 的计算，后者则由前者与前一组件或前一层的结果混合得到。

如果在 Variant Pass 中引用了对应的 Mask 或权重，那么同一组件里靠后的层的结果可能会受到靠前层结果的影响。

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-565034eaa877e695bbb6f3dfbb76a76e.png)

### Mask 层的混合

组件按照 Mask Blend Mode 配置的方式与 Stack 原有结果混合

默认为 Replace，也就是完全使用层的结果作为最终输出结果

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-6b6485d502b06acac10f20d39353e5e7.png)

### 权重层的混合

对于权重层，组件按照线性（Lerp）的方式与现有权重混合，模拟类似颜色覆盖的效果

*   层的结果为 1 时，最终输出结果为 1 ，同位置的其他所有权重层会被清空为 0
    
*   层的结果为 0 时，包括当前层在内的所有权重层的结果不变
    
    细节...
    
    具体来说，如果层的结果为 x：
    
    **当前层最终权重 = x + 当前层旧权重 · (1-x)**
    
    **其他层最终权重 = 其他层旧权重 · (1-x)**
    
    从而使得当所有旧权重总和为 1 时，混合后的所有权重总和仍是 1
    

# 预览

可以使用预览功能在视口中的地形上预览结果，预览由**目标**和**阶段**两部分配置而成

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-234530ba8193a86fa287425f2fd3773e.png)

预览目标

*   **对指定 Mask 或权重的预览**：在下方 Preview 菜单中选择一项进入
    
*   **对指定层的预览**：单击层右边的
    
    ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-45ae4e4054c4e77621e013f8bc22db1e.png)
    
    进入

也就是说，可以在同一个 Mask Modify 组件中，把同一个 Mask 或权重添加两次以上。通过选择预览目标，可以切换选择预览其中一层的效果还是所有层的共同效果

### 预览阶段

![](1704813382-a3530f583593b1fc6bdb843958e4fcff)

预览分为四个阶段，代表着当前预览目标的不同合成阶段。可以在 Configs 菜单下更改

*   **输入（Preview Input）**：显示当前预览目标混合前的状态
    
*   **输出（Preview Output）**：显示当前预览目标混合后的状态
    
*   **最终结果（Preview Final Result）**：显示 Stack 最后一个组件混合后的最终结果
    
    *   对权重来说就是视口里 Falcon 地形正在使用的那个结果。如果不一致的话，请检查是不是关闭了 Stack 更新
        
        ![](1704813382-a32817aa69af5aa4719d8eb467745e48)
        
        或者其他会影响更新的 Falcon 全局选项
*   **实时预览（Realtime Preview）**：显示_层的结果_
    
    *   只有_对指定层的预览_有实时预览 
        
    *   即使选择了其他的预览阶段，在打开预览的情况下编辑 Variant Pass 参数，在鼠标拖动的时间段里，组件也会暂时地切换到实时预览  
          
        
        *   为了速度考虑，此时 Variant Pass 的计算是以 Falcon Stack 的最终输出作为输入的，所以实时预览的结果可能不准确
            

# 其他配置

*   **Affects Weight, Affects Mask**：是否启用这个组件影响权重或者 Mask 的能力
    
*   **Supersampling Scale**：采用更大或者更小的分辨率计算层的结果
    
*   **Hide Non-Landscape Actors**：是否只显示地形，可以用这个选项去除当前不关心的遮挡
    

# TroubleShootings

### Q：似乎改了参数，但地形颜色没有改变

先用_查看结果_功能检查一下计算结果是不是有问题

再在 Falcon 设置里检查下面的的参数：

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-3b30426d4417b871f692bf260c41f0b8.png)
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-f9c4559c9216ade46fdfb4e3dbebadee.png)
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-d7e35e22c19fc504da686940e310c4e3.png)
    
    ，如果项目使用了 Indexed Map 就打开，否则关闭

### Q：怎么找到一个Mask像素对应的世界位置

按下Ctrl 后，缩略图就可以点击跳转了

优而赞之，手有余香

成为第一个点赞的人

*   本文引用
*   本文被引用

功能盘点

道路配方制作流程(TA向)

Breakout 地形权重处理逻辑汇总

Breakout 植被撒点效果生成逻辑汇总

*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-48f34ae457aeafc603ab0a403f103594.svg)529
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-0b95f0082c86623bb3ccf41eddc04c8b.png)61
    
*   ![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](https://raw.githubusercontent.com/wanlilu/imgBed/main/1704813382-d56c24c81b5e5f02b023f9382e1ca21d.svg)
