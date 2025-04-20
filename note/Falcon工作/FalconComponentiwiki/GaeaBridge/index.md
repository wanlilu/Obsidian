
# GaeaBridge

lloydli(李本贵)

创建，最后修改于2023-04-14

### 一、概述

*   Gaea Bridge是Stack里的一个读取Gaea文件（Tor）的通用框架组件，它会读取一个Tor文件的信息并自动生成引擎面板，处理和Gaea的数据交换，调用Gaea计算并将结果写回引擎
    
*   在Stack的Gaea Birdge组件里可以选择所有可用的预设，在第一次加载预设的时候组件会基于所选的预设向服务器Gaea请求参数和IO信息并自动生成面板，生成完成后参数信息会被缓存到用户本地，下次再选择这个预设时会直接使用缓存数据而不会再向服务器请求参数信息，若预设有改动，则需要手动点击Update Preset强制向服务器请求刷新参数和IO信息
    
    ![](1704813805-51760d2c7d332e76e504f1bb38e774a1.png)
    

### 二、全局参数解释：

![](1704813805-c75b21741fe701f106ea91e80a02cbcc.png)

*   **Gaea Preset**：可以选择目前Falcon提供的所有预设，也可以自己制作Gaea预设并放到指定文件夹下，下拉菜单会自动加载，如何制作预设文件可参考 [Gaea预设（Tor）开发流程指南](https://iwiki.woa.com/pages/viewpage.action?pageId=4007959577 "https://iwiki.woa.com/pages/viewpage.action?pageId=4007959577")
    
*   **Cook**：点击请求服务器Cook当前选择的预设
    
*   **Update Preset**：强制请求服务器基于当前选择的预设刷新最新的参数信息而不是使用本地缓存
    
*   **Preview**：在Preview模式下服务器会使用多线程计算，这会加快计算速度，但是对于一些模拟类型的重型节点或预设，例如Erosion或Wizard等节点在多线程计算下无法保证每次Cook得到100%确定性的结果，可能会有微小偏差，对于这类型预设，如果想要每次Cook都得到确定性的结果可以禁用Preview模式，这会强制服务器使用单线程计算，计算速度变慢但是会得到确定性的输出，对于大部分非侵蚀类的重型预设推荐默认保持Preview模式即可
    

### 三、预设效果及相关文档链接解释：

###### 1、Erosion/FalconErosion/WizardErosion：对于侵蚀的深入控制和了解可以参考Gaea官方对于侵蚀的解释和对比：[https://docs.quadspinner.com/Guide/Using-Gaea/Erosion.html](https://docs.quadspinner.com/Guide/Using-Gaea/Erosion.html "https://docs.quadspinner.com/Guide/Using-Gaea/Erosion.html")

*   Erosion是Gaea原生的侵蚀：[https://docs.quadspinner.com/Reference/Erosion/Erosion.html](https://docs.quadspinner.com/Reference/Erosion/Erosion.html "https://docs.quadspinner.com/Reference/Erosion/Erosion.html")
    
*   FalconErosion是Falcon对Erosion的二次轻量级封装，它增加了更多的二次侵蚀的细节控制，包括Fluvial、Sediment以及MicroErosion
    
    *   [https://docs.quadspinner.com/Reference/Erosion/Fluvial.html](https://docs.quadspinner.com/Reference/Erosion/Fluvial.html "https://docs.quadspinner.com/Reference/Erosion/Fluvial.html")
        
    *   [https://docs.quadspinner.com/Reference/Erosion/Sediment.html](https://docs.quadspinner.com/Reference/Erosion/Sediment.html "https://docs.quadspinner.com/Reference/Erosion/Sediment.html")
        
    *   [https://docs.quadspinner.com/Reference/Erosion/MicroErosion.html](https://docs.quadspinner.com/Reference/Erosion/MicroErosion.html "https://docs.quadspinner.com/Reference/Erosion/MicroErosion.html")
        
*   WizardErosion是Gaea对Erosion的二次轻量级封装，它通过精心策划的预设简化了设置，并为额外的处理提供了二级pass，可以把WizardErosion理解为Erosion的封装增强版：[https://docs.quadspinner.com/Reference/Erosion/Wizard.html](https://docs.quadspinner.com/Reference/Erosion/Wizard.html "https://docs.quadspinner.com/Reference/Erosion/Wizard.html")
    
*   Erosion vs. WizardErosion：Erosion允许对各个方面进行绝对控制，Wizard封装了一些直接的向导选择和预设，更易于使用和出效果，两者在底层使用相同的模拟，但提供不同的接口  
      
    

![](1704813805-2f0667633de613136628629d34c79cb1.gif)

###### 2、Breaker：断裂在地形中产生大裂缝，遵循水力侵蚀的一般规则，但不会产生土壤沉积物

*   [https://docs.quadspinner.com/Reference/Erosion/Breaker.html](https://docs.quadspinner.com/Reference/Erosion/Breaker.html "https://docs.quadspinner.com/Reference/Erosion/Breaker.html")
    

![](1704813805-d6f41166007c763b3b501f17256c92fa.gif)

###### 3、Buildup：堆积是侵蚀的一种相反形式，堆积并没有像水力侵蚀那样带走沉积物，而是在地形的凸起边缘增加了硬壳，堆积会再生一些损失的物质，填充因为侵蚀而过度雕刻和凸起的区域，这对于模拟古代景观非常有用

*   [https://docs.quadspinner.com/Reference/Erosion/Buildup.html](https://docs.quadspinner.com/Reference/Erosion/Buildup.html "https://docs.quadspinner.com/Reference/Erosion/Buildup.html")
    

![](1704813805-386aa1f08e1ad1a589583ebdcbdd0e8d.gif)

###### 4、Convector：转换器是一种夸张的热侵蚀形式，只需对其进行一些调整即可实现风格化输出，它同时创造了坚固的斜坡和阶地，提供了独特的侵蚀风味

*   [https://docs.quadspinner.com/Reference/Erosion/Convector.html](https://docs.quadspinner.com/Reference/Erosion/Convector.html "https://docs.quadspinner.com/Reference/Erosion/Convector.html")
    

![](1704813805-11faca7e27851b3b3e4356f354481ec3.gif)

###### 5、Crumble：瓦解会使地形凹陷的地方向下侵蚀并使之粉碎形成凹陷更深的峡谷

*   [https://docs.quadspinner.com/Reference/Erosion/Crumble.html](https://docs.quadspinner.com/Reference/Erosion/Crumble.html "https://docs.quadspinner.com/Reference/Erosion/Crumble.html")
    

![](1704813805-0f9397779739bfa67838a3529f90b314.gif)

###### 6、Stratify：分层允许以非线性方式在地形上创建破碎的地层或岩层，每个分层层都独立于层的其余部分，形成了一个坚固、逼真的地层

*   [https://docs.quadspinner.com/Reference/Erosion/Stratify.html](https://docs.quadspinner.com/Reference/Erosion/Stratify.html "https://docs.quadspinner.com/Reference/Erosion/Stratify.html")
    

![](1704813805-54f326f31bcdc9f22ba7cd7f1470d8d5.gif)

###### 7、Rivers：河流可以在任何地形上立即形成复杂的河网，无论它是否能够维持河流，该预设巧妙地变换地形，为生成河流提供不间断的路径，可以使用该预设来快速塑造合理的河道

*   [https://docs.quadspinner.com/Reference/Water/Rivers.html](https://docs.quadspinner.com/Reference/Water/Rivers.html "https://docs.quadspinner.com/Reference/Water/Rivers.html")
    

![](1704813805-29c86b202b2527b73033322e6d09d771.gif)

###### 8、Sea：海洋用于生成海平面和海岸侵蚀，它的主要目的是创造海洋水面，而海岸侵蚀则创造海滩和悬崖，类似于CoastErosion组件，但该预设的控制更加灵活，生成结果更好，并且该预设即可以全局生成，也可以从边缘填充，而不会影响内陆特征（通过Edge Mask控制）

*   [https://docs.quadspinner.com/Reference/Water/Sea.html](https://docs.quadspinner.com/Reference/Water/Sea.html "https://docs.quadspinner.com/Reference/Water/Sea.html")
    

![](1704813805-01d7623a02ee6a3f196b8f24b9e12977.gif)

###### 9、Snowfalll/SunlightSnowfall：

*   Snowfall基于物理来模拟降雪、融化和沉降，就像在现实世界中一样，还可以控制雪如何沿着地形粘附和流动：[https://docs.quadspinner.com/Reference/Snow/Snowfall.html](https://docs.quadspinner.com/Reference/Snow/Snowfall.html "https://docs.quadspinner.com/Reference/Snow/Snowfall.html")
    
*   SunlightSnowfall是基于Snowfall的封装，增加了Sunlight控制，基于物理的太阳光来控制积雪消融的区域，如果要自由控制积雪融化的区域可以使用Snowfall预设输入Mask手动控制：[https://docs.quadspinner.com/Reference/Data/Sunlight.html](https://docs.quadspinner.com/Reference/Data/Sunlight.html "https://docs.quadspinner.com/Reference/Data/Sunlight.html")
    

![](1704813805-9b49b4ca16563e9096e9fae47cc6c125.gif)

优而赞之，手有余香

coenjin(金沛沛) 点赞

*   本文引用
*   本文被引用

Gaea预设（Tor）开发流程指南

*   ![](1704813805-48f34ae457aeafc603ab0a403f103594.svg)46
    
*   ![](1704813805-0b95f0082c86623bb3ccf41eddc04c8b.png)16
    
*   ![](1704813805-fd7976f7b401fb858c859dda738b7af1.png)0
    

![](1704813805-58e5fa504b5449b7a89a07130def4d77.png)添加标签

![](1704813805-325ac912a48e528af8ab64d72cca36b5.svg)

发表评论，抢沙发...

![](1704813805-d56c24c81b5e5f02b023f9382e1ca21d.svg)
