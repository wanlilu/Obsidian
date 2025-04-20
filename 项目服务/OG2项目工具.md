## OG2项目管理及团队协作
开通权限 jira confluence slack gsuite

Jira 是一款由 Atlassian 公司开发的项目管理工具，主要用于问题追踪、问题管理和敏捷项目管理。
问题1 ： 用jira 不用tapd吗


Slack 类似企业微信的东西
https://zhuanlan.zhihu.com/p/592230584


Confluence 是 Atlassian 公司开发的一款团队协作软件，主要用于团队成员共享知识、协作文档和讨论项目
类似 iwiki


GSuite，现在已经更名为Google Workspace，是Google提供的一套云端办公工具集。

![[Pasted image 20240117103012.png]]
OG2等权限 ，继续进行 ta的项目配置


## Hub3 pcg 

#### 需要安装的程序
Unreal Engine, OfflinePCG, Ominverse Nucleus, Houdini 之间的关系
![[Pasted image 20240117115101.png]]
#### HDA库
<font color="#e36c09">上传HDA 工具会涉及到至少两个仓库，HoudiniTAPackages 和 项目仓库</font> 

#### Houdini注册

安装Houdini , 在License Administrator 添加中心服务器 [lightspeed-lic-pcg.woa.com:56000](http://lightspeed-lic-pcg.woa.com:56000/ "http://lightspeed-lic-pcg.woa.com:56000")

#### Houdini环境配置
一些简单的[准备工作，配置Houdini 本地调试工具](https://iwiki.woa.com/p/4009434081 "https://iwiki.woa.com/p/4009434081")

安装cuda，ominiverse launcher ， houdini connector，Farm agent
CUDA依赖于特定的NVIDIA显卡驱动才能运行，但它不仅仅是驱动。CUDA提供了一种更高级别的编程接口，允许开发者利用GPU的并行处理能力进行复杂的计算任务。

HoudiniTAPackage库  该库是存放所有云端HDA 节点的库 根据具体项目需求，每个项目可能有单独的库
PhotonPCG工具栏

ServiceBase 本地运行houdini 调试必须的

#### 工具使用



## 文档漏洞修改

应强调 HoudiniTAPackages库放在C盘
![[Pasted image 20240117121511.png]]


SerivceBase转换到新目录，权限提升至 developer
![[Pasted image 20240117121547.png]]

ServiceBase库环境配置   install-tech.bat  已经换了新库 并且新库里没有子模块
![[Pasted image 20240117140808.png]]


## PCG工程

![[Pasted image 20240117144201.png]]


![[Pasted image 20240117144447.png]]
![[Pasted image 20240117144519.png]]

## Offline pcg BUG

1. Es_City_Road  master 不能用


## falcon 与 offline pcg区别
