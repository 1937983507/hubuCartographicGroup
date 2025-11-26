---
title: 基于正负核密度曲线的线要素局部清晰度估算与自适应分段
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxINY2h4YjIwMjQwNjAxNhoIM2hlYmd4anQ%3D
type: 期刊论文
authors: 
  - 成晓强
  - 刘娜
journal: 测绘学报
categories: 
  - CSTPCD
  - 北大核心
  - EI
  - CSCD
year: 2024
issue: 6期
date: 2024.06.20
abstract: 在线要素化简时,根据特征差异对线要素进行分段是合理运用化简方法的关键.然而现有分段方法侧重分析线要素的形态特征,忽略了线要素在不同比例尺表达的视觉差异.线要素中无法清晰辨识的模糊部位会随比例尺的变化而改变.基于此,本文提出了一种基于局部清晰度的线要素分段方法.首先,在特定比例尺下生成线要素的栅格形态,并将栅格线像素分为3类:单边界像素、双边界像素和内部像素,其中单边界像素和内部像素会严重影响线要素的视觉辨识;其次,建立3类像素与原矢量折点的映射关系,得到两组数据点:对应线要素模糊部位的粘连折点和对应线要素清晰部位的正常折点;然后,基于一维核密度进行两组数据点的聚集性分析,分别生成该尺度下表示线要素清晰度变化的正向核密度曲线和表示线要素模糊程度变化的负向核密度曲线;最后,分析正负核密度交点特征,得到划分线要素清晰与模糊区域的分段点并完成线要素分段.通过与人为分段结果对比可知,本文的分段结果与人眼对线要素模糊与清晰部分的识别基本一致。
keywords: 多尺度 异质性线要素 局部清晰度 核密度 线要素分段
doi: 10.11947/j.AGCS.2024.20230287
language: 中文
---

---
title: 多源传感器协同观测中的图像配准方法研究
link: https://d.wanfangdata.com.cn/thesis/CiBUaGVzaXNOZXdTMjAyNTA2MTMyMDI1MDYxMzE2MTkxNhIJRDAzNjUwNzE3GghuaGpqaXZ6eg%3D%3D
type: 硕士论文
authors: 
  - 金钊
journal: 湖北大学
categories: 
year: 2024
date: 2024
abstract: 随着现代信息技术在农业领域的广泛应用，利用多源传感器进行协同观测，可以获取更为全面的场景信息，帮助有关人员更好地把控细节，提高农业生产的效率和质量。在对不同来源的数据进行处理、分析和展示时，不可避免地需要先进行图像配准预处理操作，确保不同来源的图像在空间上对齐，提高分析结果的准确性。　　由于不同传感器成像方式存在差异，使得有关研究仅能用于特定场景，算法泛化能力较差，并且无法充分考虑到图像包含的信息。引入机器学习不仅可以改善这一困境，并且被认为可以提取到图像更深层的特征信息，从而进行更高精度的图像配准工作。受此启发，本文提出基于CycleGAN的多源图像配准算法。通过生成对抗网络将多模态的图像配准降维为同一模态的配准问题，最后结合稳健配准算子SIFT，更高效地完成多源图像配准。主要研究成果如下：　　（1）提出了一种基于CycleGAN的多源图像配准算法，通过CycleGAN模型对图像的模态进行转换，结合当前相对稳健的 SIFT 特征点匹配算法进行图像配准。通过一系列实验证明，基于 CycleGAN 的多源图像配准算法能适用于多种不同图像配准的场景，具有较高的配准精度以及配准效率。　　（2）针对模态转换模型在复杂场景下模态转换效果不佳的问题，设计了一种基于语义分割的图像增强算法，帮助模型更好地进行特征识别与提取。优化后的模型在复杂场景下进行模态转换时，模型性能有了一定的提升，部分易被模型忽视的细节信息得以保留。　　（3）对比现有基于点特征和基于卷积神经网络的两种多源图像配准算法，在红外与可见光图像、雷达与光学影像以及高分与Landsat影像三种不同多源图像配准应用场景中进行测试。结果表明，基于CycleGAN的多源图像配准算法在三种场景下的配准效果优于基于点特征的配准方法和基于卷积神经网络的配准方法；同时在处理效率上也高于基于卷积神经网络的配准方法。
keywords: 多源传感器协同观测 多源图像配准算法 图像增强 语义分割
doi: 
language: 中文
---

---
title: 基于清晰度的线要素分段及化简算法性能评估
link: https://d.wanfangdata.com.cn/thesis/CiBUaGVzaXNOZXdTMjAyNTA2MTMyMDI1MDYxMzE2MTkxNhIJRDAzNjUwNjM2GghuaGpqaXZ6eg%3D%3D
type: 硕士论文
authors: 
  - 刘娜
journal: 湖北大学
categories: 
year: 2024
date: 2024
abstract: 线要素表示了现实世界中的绝大多数线状地物，也是地图内容的重要组成部分。若线要素难辨细节，影响地图表达地物信息。因此，保证线要素的清晰度是有必要的。随着比例尺从大到小，异质性线要素中无法清晰辨识部分会逐渐增加，清晰区域也发生改变。简化措施能够极大程度提高线要素清晰度，但也仅用在必要之处：清晰区域没有简化必要，模糊区域则考虑简化，因此，根据多尺度下的线要素清晰度的程度决定“是否需要简化”和“简化哪些部分”是可行的。与此同时，简化算法数量众多，又各有所长，不仅要保证线要素清晰度，还要考虑简化前后其特征保留能力。因此，本研究的着重点为两方面：耦合视觉感受的异质性线要素分段和简化算法性能评估。本文首先提出了一种基于清晰度变化的异质性线要素自动分段方法。首先在指定尺度下将矢量线要素栅格化；然后基于核密度提出了清晰度的度量方法；最后根据清晰度的变化度将线要素识别为清晰部分与模糊部分。本方法识别出的模糊部分可继续按照形态特征差异划分线要素类型，进而选择更加合适的综合算子算法；识别出的清晰区域则保持不变。减少线要素因不必要的简化而导致的信息损失。本方法对于线要素清晰和模糊区域的划分结果与人眼识别区域基本一致，能够精确判断线要素在特定尺度下需要综合的部分。其次，本文从简化前后关键点保持能力、简化结果与原形态相似度和清晰度的保持能力三个方面对四类简化算法进行评估，评估结果表明，WA（WeightedArea，WA）算法在保持和提升清晰度方面最具优势；BS（BendSimplify，BS）算法的简化结果相较于其他三类算法能够较好维持原线要素形态和关键点，综合考虑三类指标可知，WA算法在保持清晰度的同时，又相较其他算法能够极大程度的保留关键点和维持简化前后形态相似度。最后，选取三条多尺度下的实验线要素来验证本文方法的有效性，结果表明，WA算法能够明显提升清晰度，并且兼顾形态保持、特征点保留能力。
keywords: 清晰度 线要素表示 简化算法 地图综合
doi: 
language: 中文
---

---
title: 以用户地理位置为中心的兴趣点标签云
link: https://d.wanfangdata.com.cn/periodical/dqxxkx202401009
type: 期刊论文
authors:
  - 成晓强
  - 刘仲宇
  - 吴华意
  - 唐岭军
journal: 地球信息科学学报
categories: 
  - CSTPCD
  - 北大核心
  - CSCD
year: 2024
issue: 1期
date: 2024.04.12
abstract: 以基于位置的服务(Location-Based Service,LBS)为研究场景,针对常规地图表达兴趣点的局限,结合标签云的表达优势,设计了一种以用户地理位置为中心、面向兴趣点可视化的"LBS标签云",并初步实现了一种基于标签径向移位的布局方法.LBS标签云的主要创新是将一个布局中心点引入常规标签云,并根据标签与中心点的空间关系来确定标签的摆放位置.本文设计的布局方法如下:首先,将LBS用户的地理位置作为布局中心点;然后,基于兴趣点名称生成文字标签,并根据兴趣点属性确定标签的字号、字色及其他视觉变量;最后,在极坐标下根据标签与中心点的关系计算标签的初始摆放位置及布局优先级,并按优先级将标签从初始摆放位置依次向外径向移位至与其他标签无压盖的位置.在标签移位过程中,着重考虑角度相邻关系以确保标签距离远近的顺序关系,利用四叉树剖分字形轮廓提高了标签碰撞检测的效率.实验以景点类兴趣点为例,探讨了LBS标签云在3个场景下的可用性及可扩展性.结果表明,相比常规地图,LBS标签云不仅可以展示更多的兴趣点,而且可以有效突出用户关注的信息,如兴趣点的热度、评分及通行时间等.虽然LBS标签云包含一定的距离变形,但多种视觉变量的协同有效缓解了距离变形导致的认知误差.综上,LBS标签云可完整、直观地表达兴趣点的空间分布、多维属性及其与用户位置的关系,是一种新型的、适宜用户高效认知周边环境的可视化形式.
keywords: 基于位置的服务 兴趣点 标签云 标签地图 中心型地图 径向移位 可视化 地理信息可视化
doi: 10.12082/dqxxkx.2024.230069
language: 中文
---

---
title: Lbs tag cloud: a centralized tag cloud for visualization of points of interest in location-based services
link: https://www.mdpi.com/2220-9964/12/9/360
type: 期刊论文
authors:
  - Cheng Xiaoqiang
  - Liu Zhongyu
  - Wu Huayi
  - Xiao Haibo
journal: ISPRS International Journal of Geo-Information
categories: 
year: 2023
issue: 
date: 2023
abstract: 
Taking location-based service (LBS) as the research scenario and aiming at the limitation of visualizing LBS points of interest (POI) in conventional web maps, this article proposes a visualization method of LBS-POI based on tag cloud, which is called "LBS tag cloud". In this method, the user location is taken as the layout center, and the name of the POI is converted into a text tag and then placed around the center. The tags' size, color, and placement location are calculated based on other attributes of the POI. The calculation of placement location is at the core of the LBS tag cloud. Firstly, the tag's initial placement position and layout priority are calculated based on polar coordinates, and the tags are placed in the initial placement position in the order of layout priority. Then, based on the force-directed model, a repulsive force is applied to the tag from the layout center to make it move to a position without overlapping with other tags. During the move, the quadtree partition of the text glyph is used to optimize the detection of overlaps between tags. Taking scenic spots as an example, the experimental results show that the LBS tag cloud can present the attributes and distribution of POIs completely and intuitively and can effectively represent the relationship between the POIs and user location, which is a new visualization form suitable for spatial cognition.
keywords: location-based services；points of interest；tag cloud；visualization
doi: 10.3390/ijgi12090360
language: 英文
---

---
title: Mslf: multi-scale legibility function to estimate the legible scale of individual line features
link: https://www.researchgate.net/publication/348583390_MSLF_multi-scale_legibility_function_to_estimate_the_legible_scale_of_individual_line_features
type: 期刊论文
authors:
  - Cheng Xiaoqiang
  - Liu Zhongyu
  - Zhang Qian
journal: Cartography and Geographic Information Science
categories: 
year: 2021
issue: 
date: 2021, 48(2): 151-168
abstract: 
keywords: 
doi: 
language: 英文
---

---
title: 国土资源调查数据地图综合狭长图斑的中剖分处理
link: https://d.wanfangdata.com.cn/periodical/bjch202103016
type: 期刊论文
authors:
  - 徐海江
  - 成晓强
  - 艾廷华
journal: 北京测绘
categories: 
year: 2021
issue: 3期
date: 2021.05.24
abstract: 国土资源调查获得的土地利用图在服务不同层次、不同级别国土规划管理应用中,面临着多级数据库建设与地图综合缩编的任务需求.土地利用图中狭长图斑的中轴化是其中不可缺少的关键操作之一.针对土地利用数据全覆盖、无重叠、无缝隙及语义上多层次的特点,本文提出一种密集覆盖多边形数据的中轴化处理与拓扑关系维护的方法,用“剖分”与“归并”的思想模拟邻域多边形的扩张过程.本文算法实现建立在Delaunay三角网数据结构上,兼顾了语义相似关系的影响,经第三次国土资源调查成果的检验,本文所述中轴化及拓扑维护方法能够满足土地利用图综合实际生产的需要.
keywords: 土地利用 中轴化 Delaunay三角网 拓扑关系 邻近度
doi: 10.19580/j.cnki.1007-3000.2021.03.016
language: 中文
---

---
title: 面向GIS教育的地理空间协作云平台的设计与实现
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxIaUUtCSkJEMjAyMTIwMjEwNzEwMDAwMDQ4MDgaCDl2ZzZzaWpv
type: 期刊论文
authors: 
  - 成晓强
journal: 当代教育实践与教学研究（电子刊）
categories: 
year: 2021
issue: 10期
date: 2021
abstract: 与地理信息系统(GIS)相关的教育工作离不开地理数据和地理空间软件的支持.虽然互联网上有大量的地理信息资源,但如何探索、处理和整合这些资源目前还存在很多问题.研究人员和教师通常借助常规搜索引擎搜索地理数据,但搜索到的结果大多都无法令人满意,不仅如此,他们还经常需要花费大量经费和精力购买和维护各种地理空间软件.针对这些问题,我们设计并开发基于云计算的地理空间协作云平台GeoSquare.GeoSquare通过高度自由化的互操作和富有表现力的图形界面促进地理空间数据、信息以及知识的共享.利用平台的一站式解决方案,研究人员和教师可以便捷有效地解决他们的问题.
keywords: 云计算 地理空间协作 模型共享 GIS教育
doi: 
language: 中文
---

---
title: 矢量曲线的视觉清晰度及在网络地图综合中的应用
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxINY2h4YjIwMjAwMjAxMRoIZDNqZXlmM3A%3D
type: 期刊论文
authors: 
  - 安晓亚
  - 成晓强
journal: 测绘学报
categories: 
  - 北大核心
  - CSTPCD
  - EI
year: 2020
issue: 2期
date: 2020
abstract: 互联网用户参与的地图制图容易出现视觉冲突、压盖、拥挤等地图表达问题,需要引入地图自动综合协助解决.网络地图中由于原图比例尺和综合后比例尺均难以准确量化,常规地图自动综合基于"原图比例尺-综合后比例尺"判断是否需要综合的方法已不再适用.矢量数据在可视化后会产生视觉粘连,视觉粘连越明显,地图表达效果越差,综合的需求也越强烈.基于此规律,本文提出对视觉粘连进行定量描述并据此判断是否需要综合.首先,从人类视觉感受出发,结合栅格化思想设计了矢量曲线视觉粘连的量化指标---视觉清晰度.然后,基于"金字塔式"的尺度空间计算曲线在多个比例尺表达的清晰度,并拟合了清晰度的变化函数.最后,将该函数应用于众源地理数据的网络地图综合决策.试验结果表明,本文方法可准确判断每条矢量曲线是否需要综合,能有效解决地理数据尺度异质性带来的可视化难题.同时,清晰度变化函数将曲线的尺度描述由静态数值扩展到连续函数,有望更好地支持多尺度空间数据处理及网络地图综合等问题
keywords: 网络地图综合 视觉粘 连视觉清晰度 清晰度变化函数 空间粒度
doi: 
language: 中文
---

---
title: 居民地变化的空间分布及社会经济驱动力分析——以浙江省为例
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxIReGR0c3FianMyMDIwMDkwMDgaCG9obGw0NG8y
type: 期刊论文
authors:
  - 周衡
  - 陈张建
  - 李爱勤
  - 成晓强
  - 吴华意
journal: 数据分析与知识发现
categories: 
  - 北大核心
  - CSSCI
  - CSTPCD
year: 2020
issue: 9期
date: 2020.12.09
abstract: [目的]准确把握测绘地理要素的变化特征及驱动机理,提高基础测绘效益.[方法]以浙江省居民地为例,综合运用GIS叠置分析、集聚分析和相关性分析方法,系统剖析居民地变化的社会经济驱动力.[结果]研究表明,居民地变化集中分布在浙江省的北部、中部和东南部;第二产业发展与居民地变化数目的相关系数为0.336,是居民地变化的主要驱动力;第三产业发展、政府公共投入与居民地变化数目的相关系数分别为-0.054和-0.100,对居民地变化有负驱动作用.[局限]制图综合等人为因素导致居民地存在"伪变化",因此变化统计数据的精确性有待进一步提升.[结论]浙江省居民地变化呈现明显的区域差异,且不同经济指标对居民地变化驱动作用的程度、方向各异.
keywords: 居民地变化 驱动力 社会经济因子 偏最小二乘回归
doi: 10.11925/infotech.2096-3467.2020.0156
language: 中文
---

---
title: A quad-tree-based fast and adaptive kernel density estimation algorithm for heat-map generation
link: https://www.researchgate.net/publication/330399293_A_quad-tree-based_fast_and_adaptive_Kernel_Density_Estimation_algorithm_for_heat-map_generation
type: 期刊论文
authors:
  - Yuan Kunxiaojia
  - Cheng Xiaoqiang
  - Gui Zhipeng
  - Li Fa
  - Wu Huayi
journal: International journal of geographical information 
science : IJGIS
categories: 
year: 2019
issue: 
date: 2019, 33(12): 2455-2476
abstract: 
keywords: 
doi: 
language: 英文
---

---
title: 几何刚性和法向量采样一致性的点云配准算法
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxINY2hreDIwMTkwMTAyMBoIYzd6dWtjZWw%3D
type: 期刊论文
authors:
  - 张谦
  - 李梦瑶
  - 成晓强
journal: 测绘科学
categories: 
  - 北大核心
  - CSTPCD
year: 2019
issue: 1期
date: 2019.03.13
abstract: 针对三维激光点云配准中随机采样一致性(RANSAC)算法存在采样次数多、准确度低的缺点,该文提出了一种结合几何刚性和法向量一致性的点云配准算法.该算法通过改进采样策略降低采样次数并提升匹配精度,首先将点云的法向量信息加入采样集,使得每次的采样点从三对减少为两对;接着以两对采样点的刚性和法向量一致性计算置信度来确定当前采样是否置信;最后以迭代运算选取采样内点数最高的样本来估算变换矩阵实现点云精确配准.对激光三维点云进行配准试验,结果表明,本文方法在匹配效率及匹配性能上均优于传统RANSAC算法,且配准精度更高.
keywords:采样一致性 几何刚性 法向量 三维点云 点云配准 
doi: 10.16251/j.cnki.1009-2307.2019.01.020
language: 中文
---

---
title: 基于服务监测的WMS服务可用性调查及其影响因素探究
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxIRZGx4eWd0eWoyMDE4MDIwMDIaCG5oamppdnp6
type: 期刊论文
authors: 
  - 成晓强
  - 盛稳
  - 成晓强
  - 等
journal:地理与地理信息科学 
categories: 
  - 北大核心
  - CSSCI
  - CSTPCD
year: 2018
issue: 2期
date: 2018.07.30
abstract: 互联网上地图服务的数量一直在稳步增长,但其质量并没有显著提升,服务可用性偏低成为用户普遍诟病的问题.为了客观地探讨网络地图服务在可用性方面的问题,该文基于服务监测的方法对全球WMS服务的可用性进行调查和分析,并选取ArcGIS地图服务作为研究对照,揭示了WMS服务和ArcGIS服务在响应时间、出错情况、请求成功率以及接口功能等方面的现状和差异.实验表明:WMS的服务稳定性、响应时间和接口丰富性均不理想,无法有效支持用户与地图服务的可靠连接与稳定交互,而且与ArcGIS服务存在明显差距;商用云平台中驻存的地图服务具备显著的性能优势,表明硬件基础设施的改善是提升网络地图服务可用性的有效途径.该项研究有助于掌握网络地图服务的可用性现状,并为进一步的服务改善与优化提供科学依据.
keywords: WMS服务 ArcGIS服务 服务监测 可用性 服务质量
doi: 10.3969/j.issn.1672-0504.2018.02.002
language: 中文
---

---
title: 基于夜间灯光亮度的OpenStreetMap数据完整性检验
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxIReGR0c3FianMyMDE5MDkwMDQaCDFlcW43aXY5
type: 期刊论文
authors:
  - 刘菲
  - 成晓强
  - 吴华意
journal: 数据分析与知识发现
categories: 
  - 北大核心
  - CSSCI
  - CSTPCD
year: 2019
issue: 9期
date: 2019.11.18
abstract: [目的]解决OpenStreetMap (OSM)数据完整性评价中参考数据集难获取、更新慢等问题.[方法]引入夜光遥感影像作为新的参考数据集,以综合竞争力较强的城市作为样本,研究夜间灯光亮度与OSM数据完整性之间的相关关系,探究中国OSM数据的质量分布规律.[结果]建立OSM建筑物密度和夜间灯光亮度的回归模型,相关系数为0.8522.中国约84.2％的城市OSM建筑物密度实际值与预测值相近,差异小于0.5％;东莞、厦门等城市的实际值偏低,差异百分比分布在2％～7％范围内,数据完整性差.[局限]该模型的可扩展性有待提升.[结论]两个数据集的融合实现了低成本、大规模、多尺度的OSM数据完整性评估,反映了中国部分“空城”、“鬼城”的分布.
keywords: 夜光遥感 OSM 完整性 相关性
doi: 10.11925/infotech.2096-3467.2018.1473
language: 中文
---

---
title: 多核学习与用户反馈结合的WMS图层检索方法
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxINY2h4YjIwMTkxMDAxMhoIenRlYXVsYzM%3D
type: 期刊论文
authors:
  - 李牧闲
  - 桂志鹏
  - 成晓强
  - 吴华意
  - 秦昆
journal: 测绘学报
categories: 
  - CSTPCD
  - 北大核心
  - EI
  - CSCD
year: 2019
issue: 10期
date: 2019.11.04
abstract: 现有WMS检索方法多基于服务元数据文本匹配,缺乏对地图内容的"感知",无法应对元数据缺失或图文不符的情境.本文设计了一种多特征多核学习和用户反馈结合的WMS图层检索方法,利用多核学习算法融合颜色、形状与纹理特征,实现图层分类和相似度排序,并通过采集检索结果展示页面中的兴趣图层标记进行用户反馈,以优化分类模型和提高检索精度.试验结果表明,该方法查准率高且检索用时较短,能够与现有基于文本检索的地理信息资源门户集成,实现WMS的快速检索与有效发现.
keywords: 地理信息资源检索 多核学习 多特征融合 用户反馈 网络地图服务
doi: 
language: 中文
---

---
title: 一种WMS领域主题文本提取及元数据扩展方法
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxITd2hjaGtqZHh4YjIwMTkxMTAxORoIanA0a2xtejU%3D
type: 期刊论文
authors:
  - 张敏
  - 桂志鹏
  - 成晓强
  - 等
journal: 武汉大学学报(信息科学版)
categories: 
  - 北大核心
  - CSTPCD
  - EI
year: 2019
issue: 11期
date: 2019.12.31
abstract: 由于网络地图服务(Web map service,WMS)元数据缺乏显式的领域主题描述机制,用户很难准确、全面地发现目标领域的地图数据资源.提出了一种面向地理信息资源检索的WMS领域主题文本提取及元数据扩展方法.首先,设计了一种非监督文本分类算法,利用地球与环境术语集语义网(semantic Web of Earth and environmental terminology,SWEET)和大型英语词汇语义网WordNet,综合计算WMS元数据能力文档中地学术语、通识型词汇与领域主题的语义相关度,为WMS及其图层提取多标签主题.然后,基于ISO191152003地理信息元数据标准,为WMS元数据组织模型扩展领域主题.实验结果表明,所提出的WMS元数据主题分类算法取得了较高的查准率和查全率,且相较于朴素贝叶斯、线性支持向量机(support vector machine,SVM)和逻辑回归等方法,整体上有较大的优势.该方法有望应用于当前的地理信息门户和目录服务,辅助用户快速、准确地定位目标领域的地图服务资源.
keywords: 网络地图服务 文本分类 元数据标准 资源发现 语义距离 领域主题
doi: 10.13203/j.whugis20180083
language: 中文
---

---
title: Detail Resolution: A New Model to Describe Level of Detail Information of Vector Line Data
link: https://www.researchgate.net/publication/316728527_Detail_Resolution_A_New_Model_to_Describe_Level_of_Detail_Information_of_Vector_Line_Data
type: 会议论文
authors:
  - Cheng Xiaoqiang
  - Wu Huayi
  - Ai Tinghua
  - et al.
journal: Spatial Data Handling in Big Data Era. Advances in Geographic Information Science. Springer, Singapore
categories: 
year: 2017
issue: 
date: 2017
abstract: 
keywords: 
doi: 
language: 英文
---

---
title: 信息量与相似度约束下的网络地图服务缩略图自动生成算法
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxINY2h4YjIwMTcxMTAxMRoIZzlkbXU4cjI%3D
type: 期刊论文
authors:
  - 成晓强
  - 杨敏
  - 桂志鹏
  - 等
journal: 测绘学报
categories: 
  - 北大核心
  - CSTPCD
  - EI
year: 2017
issue: 1期
date: 2017
abstract: 缩略图可显著提高图片和视频等图形图像资源的展现效率,有效改善信息检索过程中的用户体验.地图服务是一种耦合空间和尺度信息的图像资源,其设计创作、检索筛选和存储管理均需要缩略图的支撑.设计精巧的缩略图带给人良好、鲜活的第一印象,帮助用户实现高效的交互与探索;粗糙凌乱的缩略图则让用户产生抵触情绪,打消其继续探索地图服务的主动性.本文借鉴视频关键帧的思路并结合地图表达特点,提出了地图服务关键位置和关键尺度的概念,并设计了相应的量化指标和提取算法,重点解决了缩略图视觉效果不佳、地图信息量不足和自动化程度不高等问题.算法基于信息量识别地图中内容丰富的关键位置,然后利用跨比例尺相似度判断发生显著变化的关键尺度,最后自动筛选出若干张代表地图服务内容的缩略图.试验表明本方法提取的缩略图数量适中、内容丰富、代表性强,可高度还原地图服务的内容构成和外观样式
keywords: 网络地图服务 关键尺度 缩略图 信息量 相似度
doi: 
language: 中文
---

---
title: 标准化地图服务(WMS)与私有格式服务(ArcGIS MapServer)性能对比研究报告
link: https://d.wanfangdata.com.cn/conference/ChtDb25mZXJlbmNlTmV3U29scjlTMjAyNTExMTcSBzk1Nzc3MTMaCDc4ZnNiZG1k
type: 会议论文
authors: 
  - 盛稳
  - 吴华意
  - 成晓强
  - 桂志鹏
journal: 2016中国地理信息科学理论与方法学术年会
categories: 
year: 2016
issue: 
date: 2016.09.23
abstract: 网络地图服务可分为遵循OGC规范的标准地图服务(WMS)和其他私有化地图服务。在私有化服务中，由于ESRI ArcGIS服务数量巨大且应用广泛，具有典型性和普遍性，故本文选取ArcGIS MapServer作为私有服务研究对象。为了客观全面的比较和分析两者性能状况，本文对全球分布的46296条WMS(其中41700个可获取并解析服务元数据文档，含268971个图层)和20597条ArcGIS MapServer(含206390个图层)进行了长达1个多月的同步监测。同时获取了资源的空间位置、发布者、服务主题/图层主题、图层覆盖范围、坐标参考及版本等重要属性的统计信息。分析了两类服务的访问成功率、出错类型、以及GetMap/ExportMap和GetCapabilities/GetWSDL响应总时间分布、服务器处理访问请求时间分布、请求地图/XML文档大小、下载文档/地图数据速度等能够反映服务性能的关键指标。
keywords: 标准与私有格式服务 WMS与ArcGIS MapServer 服务监测 服务性能对比
doi: 
language: 中文
---

---
title: 一种顾及空间分布模式的跨尺度HeatMap生成方法
link: https://d.wanfangdata.com.cn/conference/ChtDb25mZXJlbmNlTmV3U29scjlTMjAyNTExMTcSBzk1Nzc3MzQaCDc4ZnNiZG1k
type: 会议论文
authors: 
  - 袁昆筱佳
  - 成晓强
  - 桂志鹏
  - 吴华意
journal: 2016中国地理信息科学理论与方法学术年会
categories: 
year: 2016
issue: 
date: 2016.09.23
abstract: 全面、准确的兴趣点(Point of interest,POI)信息是许多地理信息产品制作的基础,科学地挖掘POI点群的分布特征与潜在信息具有十分重大的意义.在处理大规模POI点群数据时,通常需要对数据进行空间分析以及相关可视化工作.作为一种新兴的可视化方法,热力图(HeatMap)可聚合大量数据,用渐变色带直观地表达出POI点群的疏密程度或频率高低,效果通常优于离散点的直接显示,现已被广泛应用于许多领域的研究.HeatMap用热度来表示地理对象的某种要素、特征、属性的数值,并可适当进行线性拉伸,在表达地理对象空间位置基础上对其密度、强度等特征进行表达
keywords: HeatMap 核密度估计法 参数自动决策 POI点群 空间点模式分析
doi: 
language: 中文
---

---
title: Content-Based Discovery for Web Map Service using Support Vector Machine and User Relevance Feedback
link: https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0166098
type: 期刊论文
authors:
  - HU K
  - GUI Z
  - CHENG X
  - et al
journal: PLOS ONE
categories: 
year: 2016
issue: 
date: 2016,11(11): e166098
abstract: 
keywords: 
doi: 
language: 英文
---

---
title: 自发地理信息兴趣点数据在线综合与多尺度可视化方法
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxIOY2h4YjIwMTUwMjAwMTcaCG5oamppdnp6
type: 期刊论文
authors: 
  - 杨敏
  - 艾廷华
  - 卢威
  - 成晓强
  - 周启 
journal: 测绘学报
categories: 
  - 北大核心
  - CSTPCD
  - EI
year: 2015
issue: 2期
date: 2015.03.23
abstract: 移动及 Web 环境下，集成各种自发地理信息 POI 数据与地理框架背景数据的混搭式地图应用，越来越多地出现在主流地理信息平台及 LBS 服务中。由于缺乏适宜的在线多尺度可视化机制，这种POI 数据表达上通常出现拥挤、压盖等冲突现象。针对该问题，本研究将传统的尺度变换方法与在线环境相结合，提出一种面向城市设施 POI 数据的多尺度可视化策略。即由服务器端通过预处理方式对POI 数据进行多层次结构化组织；在此基础上，客户端依据显示比例尺导出对应层次的 POI 目标，并通过移位操作解决局部存在的符号表达冲突现象。试验表明，该方法符合数字化网络应用的在线实时需求，同时也能获得较高质量的多尺度表达效果。
keywords: 自发地理信息 城市设施兴趣点数据 多尺度可视化 在线综合
doi: 
language: 中文
---

---
title: 全球网络地图服务质量监测报告-2015
link: https://d.wanfangdata.com.cn/conference/ChtDb25mZXJlbmNlTmV3U29scjlTMjAyNTExMTcSBzk1Mzc1MDMaCDc4ZnNiZG1k
type: 会议论文
authors: 
  - 吴华意
  - 桂志鹏
  - 成晓强
  - 曹军
  - 刘宵婧
journal: 2015中国地理信息科学理论与方法学术年会
categories: 
year: 2015
issue: 
date: 2015.08.08
abstract: 地理信息服务是专业地理信息对接大众用户需求的重要手段,也是地理信息产业与其他产业沟通、融合的桥梁.目前,地理信息服务依赖的技术、规范已十分成熟,网络上已经存在相当数量的地理信息服务,但这些服务的可用性普遍较低,受众认可度不高.本项研究以网络地图服务为研究对象,利用地理信息门户、通用搜索引擎及自定义网络爬虫等方法,共收集了2304个分布在全球的网络地图服务.经过基本的统计分析,本项研究初步理清了这些服务的数量、类型及分布情况,其中符合OGC标准的WMS服务共2064个,占服务总数的89.6％,分布在全球6个大洲,43个国家.通过元数据提取及解析,归纳了网络地图服务侧重表达的主题,其中Geology是最常出现的关键词.利用云平台在全球部署服务质量监测节点,本项研究获取了时空无缝的服务性能数据,并基于此数据进行了规律挖掘与服务性能预测,发现了服务响应速度与访问时间、客户端空间位置之间的明显相关关系；服务的平均响应时间(获取400*200大小的地图切片)为1.31秒,80％的服务响应时间在2秒以下,基本满足用户对等待时间的要求,其中WMS服务的响应时间大部分差于其他类型服务.基于地图感受理论,对服务的地图可视化质量进行了量化分析,发现服务中普遍存在色彩配置不和谐、符号布局不清晰、多尺度支持不完善等严重的可视化问题.
keywords: 网络地图服务 服务质量 地理信息服务 平均响应时间 专业地理信息 元数据提取 可视化质量 分布
doi: 
language: 中文
---

---
title: A CLOUD-BASED PLATFORM SUPPORTING GEOSPATIAL COLLABORATION FOR GIS EDUCATION: ISPRS Workshop of Commission VI 1-3, Advances in Web-based Education Services
link: https://isprs-archives.copernicus.org/articles/XL-6-W1/1/2015/
type: 会议论文
authors:
  - CHENG X
  - GUI Z
  - HUA K
  - et al
journal: ISPRS Workshop of Commission VI 1-3, Advances in Web-based Education Services
categories: 
year: 2015
issue:
date: 2015, Berlin, Germany
abstract: 
keywords: 
doi: 
language: 英文
---

---
title: 基于内容的地理信息服务在线检索
link: https://d.wanfangdata.com.cn/conference/ChtDb25mZXJlbmNlTmV3U29scjlTMjAyNTExMTcSBzg4Njk2OTEaCG5oamppdnp6
type: 会议论文
authors: 
  - 成晓强
  - 吴华意
journal: 中国地理信息科学2014学术年会
categories: 
year: 2014
issue: 
date: 2014.10.11
abstract: 极大丰富的网络地理信息资源使人们不再受困于"数据匮乏",但随之而来的"信息过载"(Information Overload)又给研究者们带来了新的挑战,高效的地理信息资源检索发现便是其中之一.目前,Web服务是地理信息资源最主流、正统的存在形式,原因之一是其拥有较好的互操作性,更重要的是地理信息服务领域已经制定了一系列公认、成熟的标准规范可供遵循,现有的地理信息资源检索研究也多以"地理信息服务"为研究对象. 网络地理信息资源首先是一种网络资源，是动态变化的，具有一定的生命期;进而是一类地理空间数据，具有多维描述特征，如空间、时间和语义，且在各个维度上均具有相应的度量方法，如比例尺、空间范围、采样频率和表达粒度等。搜索引擎和推荐系统的成功离不开对其处理对象的完备描述，该规律同样适用于地理信息服务。利用网络爬虫抓取到地理信息服务之后，从时间、空间和语义维度抽取其包含的主题特征，并从广度、粒度和频度三个层次对抽取到的主题特征进行组织，建立索引。其中，空间、时间和语义可认为是定性描述，广度、粒度和频度为定量描述，一共可组成9种描述指标，如空间与粒度的组合可表述为传统地图制图中的比例尺或者Web制图中的LOD、分辨率等。为了支持地理信息服务的多维度描述特征，必须建立针对的服务质量模型，该模型应该满足如下条件:1)全面支持空间、时间和语义信息的一体化存储，并可有效支撑各维度的索引建立。2)顾及地理信息服务在三个维度上的间断性和异质性，实现细粒度的精细化管理。3)具有较高的灵活性和可扩展性，在处理非标准化地理信息资源时，如文本、图片、网页、视频等非结构数据，仍能有效管理其质量信息。
keywords: 地理信息资源 在线检索 网络技术 服务质量
doi: 
language: 中文
---

---
title: 一种决策驱动的地图综合服务语义增强方法
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxITd2hjaGtqZHh4YjIwMTQwNTAxMRoIZmtjcWZycjU%3D
type: 期刊论文
authors:
  - 成晓强
  - 艾廷华
  - 杨敏
journal: 武汉大学学报(信息科学版)
categories: 
  - 北大核心
  - CSTPCD
  - EI
year: 2014
issue: 5期 
date: 2014.06.03
abstract: 从地图综合经典决策模式出发,提出了“算子、算法、参量”的三层决策模式与WPS交互流程之间的隐性对应关系,并由此扩展到通用的服务链创建;然后,利用这种对应关系梳理了服务语义增强必需的4类语义描述信息;最后,使用该信息扩展了WPS规范.该方法归纳的语义信息简洁精确,可分层次嵌入WPS服务的交互流程,在顾及标准的同时提升了地图综合服务的语义互操作能力.
keywords: 地理信息服务 地图综合 语义互操作 WPS规范
doi: 10.13203/j.whugis20120208
language: 中文
---

---
title: A shape analysis and template matching of building features by the Fourier transform method
link: https://www.sciencedirect.com/science/article/abs/pii/S019897151300063X
type: 期刊论文
authors:
  - AI T
  - CHENG X
  - LIU P
  - et al
journal: Computers, Environment and Urban Systems
categories:
year: 2013
issue: 
date: 2013,41(0): 219-233
abstract: 
keywords: 
doi: 
language: 英文
---

---
title: 等高线与水网数据集成中的匹配及一致性改正
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxINY2h4YjIwMTIwMTAyOBoIZmtjcWZycjU%3D
type: 期刊论文
authors: 
  - 杨敏
  - 艾廷华
  - 刘鹏程
  - 成晓强 
journal: 测绘学报
categories: 
  - 北大核心
  - CSTPCD
  - EI
year: 2012
issue: 1期
date: 2012.04.28
abstract: 等高线作为地势起伏与地形凹凸特征的表达,与水网表达有着密切的约束关系。在空间数据库集成与匹配中,由于来源不同,这两种数据往往出现不一致,导致水网＂爬坡＂现象。针对该问题提出一种等高线与水网匹配及一致性改正方法,利用Delaunay三角网模型在等高线基础上通过弯曲分析提取地形特征,基于河流分布与谷地走势的相关性,建立水网与等高线间的匹配关系,进而识别两者间的一致性。在此基础上利用仿射变换等几何方法完成一致化操作,包括水网移位匹配等高线和等高线移位匹配水网。
keywords: 空间数据匹配 Delaunay三角网 空间数据质量
doi: 
language: 中文
---

---
title: 基于原型模板形状匹配的建筑多边形化简
link: https://d.wanfangdata.com.cn/periodical/CiBQZXJpb2RpY2FsQ0hJU29scjkyMDI1MTExNzE2MDExNxITd2hjaGtqZHh4YjIwMTAxMTAyNhoIZmtjcWZycjU%3D
type: 期刊论文
authors: 
  - 刘鹏程
  - 艾廷华
  - 胡晋山
  - 成晓强
journal: 武汉大学学报（信息科学版）
categories: 
  - 北大核心
  - CSTPCD
  - EI
year: 2010
issue: 11期
date: 2011.04.15
abstract: 在大比例地图综合中,建筑多边形的形状化简占有很大的比重,本文用形状数来描述建筑多边形的形状,通过形状数间的最小编辑距离在动态模板库中匹配相似的模板,用匹配成功的模板来置换原要素而实现建筑多边形的化简.实验证明,基于形状数的建筑物形状相似性匹配结果与人的视觉感知基本一致;基于形状匹配的建筑物化简方法具有很强的实用性.
keywords: 地图综合 形状数 原型模板 形状匹配
doi: 
language: 中文
---

---
title: 在线式地图综合与尺度变换服务框架的设计与实现
link: https://d.wanfangdata.com.cn/conference/ChtDb25mZXJlbmNlTmV3U29scjlTMjAyNTExMTcSBzgwNzAyNjcaCDc4ZnNiZG1k
type: 会议论文
authors: 
  - 成晓强
  - 艾廷华
journal: 中国地理学会2009百年庆典学术大会
categories: 
year: 2009
issue: 
date: 2009.10.17
abstract: Web服务(Web Service)是目前服务器端逻辑最受推荐的实现方式，在地理信息领域有着广泛的应用，网络地图服务、天气预报服务、实时交通流量查询等都极大的促进了GIS的大众化。
keywords: 在线式 地图综合 尺度变换 服务框架 网络地图服务 天气预报服务 信息领域 实现方式
doi: 
language: 中文
---