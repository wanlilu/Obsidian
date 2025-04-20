## Part 1 
### prim
sopcreate 


### reference  sublayer

![[Pasted image 20231104155630.png]]

sublayer / reference / payloads/ opinion

sublayer是将几个usd合成
reference 可操作对象更小，可以将usd中的指定prim 引用到当前USD里，并可以对其非破坏性override

**payloads**
在运行时，他们可以选择不加载DukeCaboom的**负载(payload)**。在这种情况下，负载的内容将在合成期间被忽略，从而节省内存和运行时成本。
注意，**负载(payload)**加载/卸载是一个运行时flag，因此加载/卸载负载不会对层产生任何更改。

**opinion**
在Pixar的流程中，我们大量使用**负载(payload)**，让用户只加载他们需要的资产内容。但我们发现，资产中有一些**属性(properties)**或**元数据(metadata )**我们希望始终可用，即使负载已卸载。为了支持这一点，我们通常会在我们的资产结构中引入一个中间层，它包含了我们一直希望在场景图中看到的效果。

### purpose
proxy or render
configure primitive

### Merging primitives

![[Pasted image 20231104171936.png]]


### SOP import

instance 用法

### Variants

![[Pasted image 20231105074602.png]]

### Kind ： defining roles

![[Pasted image 20231105074758.png]]

### Material Basics

material x 和 原始材质

### layers and exports
![[Pasted image 20231105081923.png]]

### Loading saved USD



## Part2 导入资产 从bridge到Houdini

![[Pasted image 20231105110631.png]]


### 环境配置
![[Pasted image 20231105110655.png]]
在houdini中配置插件

主要讲了Bridge中的资产如何导入 Houdini 
利用插件 配合 component build ，将各种各样的资产适配USD vriants、MaterialX 

![[Pasted image 20231108155613.png]]

![[Pasted image 20231108155638.png]]



## Part3 Layout and scattering USD assets
1. solaris里要用 绝对路径，避免重复修改prim名称导致报错

![[Pasted image 20231120154747.png]]
![[Pasted image 20231121115414.png]]
![[Pasted image 20231121155633.png]]


## Part4 Procedural Environment Creation

![[Pasted image 20231222164846.png]]



## Houdini Solaris 
云雾渲染 https://www.youtube.com/watch?v=lXzkxlnQErQ