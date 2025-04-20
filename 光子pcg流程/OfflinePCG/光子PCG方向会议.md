## 目标
1. 落实实际效率的提升，提高项目使用评价
2. 服务更多项目
	1. 什么用的项目可以只发工具，如version pro 海岛项目
	2. 什么样的项目，需要TA驻足支持

## 用户关系
1. 强链接项目：
	1. 公司重点项目，ES、OG2、PGame
	2. 大量定制化工具，工具复杂高
	3. 定制流程，如PGame跨引擎，OG2城市多人协作
2. 弱链接项目：
	4. 实验性项目，或轻量项目需要某个工具
	5. vision pro 海岛项目，制作野外地形，要求底
	6. 只需要单独使用某个工具，如LOD批量制作、瀑布工具

## 光子PCG工具模块
1. OfflinePCG 地形系统
2. UE PCG、USD-Houdini、独立工具


## 连接用户方法
1. 与项目负责人推广
	1. 需要做项目时候Demo及简明教程（参考 world creator）
	2. 阶段宣传
2. 企微机器人推广
	1. 方便通过企微机器人下载简明工具体验，如GPU地形，weightmap生成、uepcg植被生成，5步、20分钟，快速创建野外场景

## OfflinePCG
1. 面板调整 
	1. 参数面板
	2. 参数绑定及面板精简

2. 曲线控制器
	1. 支持道路
3. GPU工具
	1. mask/weightmap调整
		1. blur、smooth、noise（）
	2. Filters
	3. MeshProjection、Spline Terrain、TextureProjection
4. UE PCG
	4. 解耦offlinePCG & USD、



## UE  PCG & Houdini-USD
6. 植被、河流、土路 https://github.com/freetimecoder/unreal-pcg-examples
	1. ![Pasted image 20240407190000](https://raw.githubusercontent.com/wanlilu/imgBed/main/notePasted%20image%2020240407190000.png)
	2. 拓展UE PCG 
		1. https://nebukam.github.io/PCGExtendedToolkit/
		2. USD Houdini Connector
		3. 支持读取、写入Editor Layer
7. UE PCG 与USD结合需求初筛选：建模？
8. 资产批处理工具是否需要？比如生成LOD等 
