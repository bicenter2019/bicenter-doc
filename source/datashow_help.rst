.. _dataShow:

前言
===========================
综合看板主要用来做数据的可视化展示和布局，支持多数据源，多种图形组合布局，使用简单，操作时在页面进行拖拽即可完成看板设计。
目前支持的图形组件有HTML片段，饼图，色带仪表，关系图，直角坐标，地理坐标，表格及雷达图，运行状态图，嵌套环形图。其中，色带仪表支持仪表和色带两种图形，直角坐标支持线图，条形图和条形堆叠图，地理坐标支持散点图和动效散点图，线图和动效线图。

安装配置
================
安装
--------------

BICENTER-DataShow是以.zip格式发布的安装包，以jetty内置容器方式启动，运行依赖java，在部署时只需将安装包解压到一个任意目录文件夹，
在安装了java并配置好了java环境变量的的客户端直接启动，否则需要安装java，解压以后如下图:
 .. image :: _static/images/datashow/1.1.png   


配置
-----------------
BICENTER-DataShow的所有配置是通过文件config.properties配置的，主要配置以下内容：
 * 访问的host和端口号（host，port），根据实际配置即可 
 * 前端源文件存放目录（webcontent），
 * 报表文件存放目录（webroot）
 * 数据源，可配置多个数据源
 * 配置完成以后的文件如下：
  .. image :: _static/images/datashow/1.2.png  


启动
----------------------
启动时，如果是Windows环境直接运行start.bat，如果是Linux环境运行start.sh，启动以后，浏览器访问直接访问http://[host]:[port],开始看板的设计

设计
====================
看板设计有两个入口，一是首页的【新建】按钮，一是对应文件后面的【编辑】按钮，进入设计面板即可开始看板的设计工作。

设计全局属性 
--------------------
全局属性是设置看板的全局外观属性，	在设计面板，点击【全局属性】，显示全局属性面板如下：
  .. image :: _static/images/datashow/2.1.png
* 像素宽，像素高：根据需要展示的屏幕分辨率设置，手动拖动调整或者输入分辨率数值
* 水平网格数：主要组织组件的布局
* 背景颜色：画布背景颜色，通过点击选择适合的背景颜色
* 配色主题：设置看板的主题 

组件列表
--------------------
组件列表展示了看板支持的组件，每个组件对应不同的图形展示，在设计页面点击【组件列表】，可以看到支持的组件如下：
  .. image :: _static/images/datashow/2.2.1.png
使用时，设计者可以任意拖动对应的组件到设计器页面位置，当鼠标变成双向箭头时则可以拖动组件大小。
 .. image :: _static/images/datashow/2.2.2.png
  
设置组件外框属性
--------------------
组件的外框属性，定义了每个组件的外观属性，通过设置可以改变组件的外观。双击组件，即显示每个组件各自对应的组件属性面板，显示如下:
 .. image :: _static/images/datashow/2.3.png
* 标题栏：设置组件的标题信息
* 背景：设置组件背景颜色，如果勾选了背景透明，则显示设计页面的背景色
* 框线：设置组件边框属性，可以拖动或者输入框线范围值
* 阴影：设置组件阴影，包括显示和不显示


组件数据绑定
-------------------
HTML片段
~~~~~~~~~~~~
一般用来展示某个指标，标记某个信息等，属性面板包括以下设置:
 .. image :: _static/images/datashow/2.4.1.1.png
* 字体：设置组件要展示的字体样式
* html模板：输入自定义html样式，取变量的方式是${变量名}
* 数据：绑定数据
例如：展示监控报警系统报警的次数，组织SQL查询如下::

	select  count(*)  as  count  from  tbl_ci_base_info 
	
点击数据后面【....】弹出数据绑定对话框进行数据绑定，在数据绑定选择要展示的数据字段名称
 .. image :: _static/images/datashow/2.4.1.2.png
查看数据绑定结果如下：
 .. image :: _static/images/datashow/2.4.1.3.png

饼图
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
平铺饼图
..............................
饼图展示的数据只能是单序列数据，一般用于展示比例百分比，属性面板包括如下设置：
 .. image :: _static/images/datashow/2.4.2.1.png
* 图例：选择图例显示在组件上的位置，可以选择不显示或者居右，底部，居上或者居左
* 数据：绑定数据
例如：展示监控报警系统排名前10的设备报警的次数所占比例，组织SQL查询::

	SELECT t2.name jobName,SUM(t1.event_count) counts FROM alert_event_summary t1,job t2 
	WHERE 	t1.major_ver IN (1,2,3,4,5,7,8,9)	AND t1.job_id = t2.job_id 
	AND t1.node_id  = t2.node_id GROUP BY t1.job_id  ORDER BY counts DESC LIMIT 10
	
点击数据后面【...】弹出数据绑定对话框，数据绑定时，“指标名称”可以是自定义的指标名称，可以不填，“值字段”对应的饼图的数值，“名称字段”对应饼图的类别，“位置”对应图形在页面的显示的位置
 .. image :: _static/images/datashow/2.4.2.2.png
查看绑定结果如下:
 .. image :: _static/images/datashow/2.4.2.3.png
 

同心圆
..................................
如果要展示为同心圆的图形，在数据绑定时，填写位置栏，数字小在内圈，数字大的在外圈，例如：
 .. image :: _static/images/datashow/2.4.2.4.png
展示结果为：
 .. image :: _static/images/datashow/2.4.2.5.png
 
色带仪表
~~~~~~~~~~~~~~~~~~~~~~~~~~
色带仪表组件可以制作仪表和色带两种图形，设计时根据需求在属性面板选择不同的图形类别：
 .. image :: _static/images/datashow/2.4.3.1.png
* 图形：可以选择仪表和色带两种图形展示，如果选择仪表则设置仪表属性
* 最小值：仪表刻度的最小刻度值
* 最大值：仪表刻度的最大刻度值
* 起始角度：仪表开始的角度
* 截止角度：仪表截止的角度
* 图形占比：仪表所占组件的比例，可以拖动或者输入数值
* 表盘线宽：仪表的宽度
* 刻度长度：标记仪表刻度的长度
* 指针宽度：仪表指针的大小宽度
* 色带背景色：选择图形为色带时，色带的背景色
* 值域色：选择图形为色带时，色带数值的显示颜色
* 数据：仪表或者色带都是用过数据绑定数据

仪表
..............................
属性面板选择图形类别为“仪表”，点击数据后面【...】弹出数据绑定对话框
准备查询SQL::

	select  count(*)  as  count  from  tbl_ci_base_info 

数据绑定时，“指标名称”可以是自定义或者不填，“值字段” 选择仪表显示的数值来源，“分段设置”填写是比例值，填写0到1之间的连惯数，表示把指标分成几个等级 ，例如设置0.2,0.6,1，表示的是按0-20%，20%-60%，60%-100%将指标分成3段，代表3个等级
 .. image :: _static/images/datashow/2.4.3.2.png
数据绑定结果如下：
 .. image :: _static/images/datashow/2.4.3.3.png

色带
...............................
属性面板选择图形类别为“色带”，在绑定数据可以不填写分段设置，建议填上指标名称：
 .. image :: _static/images/datashow/2.4.3.4.png
绑定结果如下：
 .. image :: _static/images/datashow/2.4.3.5.png

关系图
~~~~~~~~~~~~~~~~~~~
一般可以直观的展示数据之间的联系，比如网络环境拓扑关系，亲戚关系等等，他有两种图形展示，圆环型和引导型，两种图形只是外观展示不一样，数据绑定方式都是一样的，以引导型说明，属性面板设置以下属性：
 .. image :: _static/images/datashow/2.4.4.1.png
* 图例：展示图例是否显示及位置
* 图形样式：图形展示样式，可以选择圆环型和引导型两种，圆环型对应圆环型设置，引导型对应引导型设置
* 节点样式：图形连接点的样式
* 标签显示：勾选表示显示图形标签
* 节点拖拽：勾选表示允许拖拽节点
* 点大小系数：控制节点显示的大小，绑定数据时选择了值字段时生效
* 旋转标签：勾选表示标签可以旋转展示
* 斥力因子：引导型图形设置，表示节点间的间隔系数
* 引力因子：引导型图形设置，节点间的靠近系数
* 节点间距：引导型图形设置，表示节点之前的间距，可以拖动或者输入数值
* 数据：绑定数据
例如：展示网络的拓扑关系图，组织查询SQL，需要查询出数据的关联关系::

	select c.node_width,c.node_name  ,
	(SELECT node_name from ic_res_node_position WHERE node_id = a.uplink_node_id LIMIT 1) as uplink_name ,
	 c.node_type   from ic_res_topo_line a  LEFT JOIN ic_res_node_position c on a.node_id = c.node_id 
	or a.uplink_node_id = c.node_id  WHERE a.topo_id = 4 and a.line_type = 2 and c.node_name != ""

数据绑定时，“source名称”表示关系图形起点，“target名称”表示关系图形终点，“值字段”表示节点的数值，“类目名称”表示用于图形的类别
 .. image :: _static/images/datashow/2.4.4.2.png
数据绑定结果为：
 .. image :: _static/images/datashow/2.4.4.3.png
直角坐标
~~~~~~~~~~~~~~~~~~~~~
直角坐标组件可以绘制线图，条图和条图堆叠，不同的图形是通过绑定数据时，改变绑定的方式来实现。组件提供一下外观设置
 .. image :: _static/images/datashow/2.4.5.1.png
* 坐标区：控制直角坐标系的横纵坐标在组件的显示位置，可以调节上、下、左、右边距，运行拖动或者手动输入数值
* 显示栅格：勾选表示显示
* 坐标轴：设置坐标的显示属性，包括下横坐标，上横坐标，左纵坐标和右纵坐标属性设置，可以设置坐标的显示名称和显示位置，值标签设置坐标点的旋转角度
* 图形：设置图例是否显示和显示位置
* 数据：绑定数据

线图
....................
需要明确横纵左边的属性，例如，要展示一周内的监控报警趋势，组织查询sql::

	SELECT tttt,COUNT(1) counts FROM  (
	SELECT DATE_FORMAT(create_time,'%Y-%m-%d') tttt FROM alert_event_list WHERE  major_ver IN (1,2,3,4,5,7,8,9) and priority!=1
	and DATE_SUB('2018-05-25', INTERVAL 7 DAY) <= DATE(create_time)
	) temp_table GROUP BY tttt 

数据绑定时，设置横坐标为类目，纵坐标为数值，根据查询结果设置如下：
 .. image :: _static/images/datashow/2.4.5.2.png
-- 注意：设置坐标时，需要勾选并选择坐标值的字段才能生效
数据绑定结果为：
 .. image :: _static/images/datashow/2.4.5.3.png

条图
.....................
在绑定数据时，选择数值为条图，即可绘制条形图，例如：
 .. image :: _static/images/datashow/2.4.5.4.png
数据绑定结果如下:
 .. image :: _static/images/datashow/2.4.5.5.png
 
条图堆叠
.....................
堆叠图一般用于有多个指标展示时，指标可以进行堆叠展示，组织查询SQL::

	SELECT t2.name jobName,SUM(t1.event_count) counts,avg(t1.event_count) as avg FROM alert_event_summary t1,
	job t2 WHERE t1.major_ver IN (1,2,3,4,5,7,8,9)AND t1.job_id = t2.job_id AND t1.node_id  = t2.node_id
	GROUP BY t1.job_id  ORDER BY counts DESC LIMIT 10

绑定数据时选择条形堆叠：
 .. image :: _static/images/datashow/2.4.5.6.png
数据绑定结果如下：
 .. image :: _static/images/datashow/2.4.5.7.png

地理坐标
~~~~~~~~~~~~~~~~~~~~~~~~~~~
地理坐标组件现在支持散点图，动效散点图，线图和动效散点图，设计者根据需求在数据绑定时选择不同的图形展示种类，地理坐标组件可以设置如下属性
 .. image :: _static/images/datashow/2.4.6.1.png
* 地图：选择要显示的地图类型
* 地标：选择地图要显示的地区
* 图例：设置图例是否显示和在组件上的显示位置
* 数据：绑定数据

散点图及动效散点图
............................
地理坐标组件支持一般散点和动效散点图，这两种图形绑定数据的方式是一样的，如果要显示动态效果，图形类别选择动效散点
例如展示全国主要城市分布，组织查询SQL::

	 select * from testGeo

查询结果如下：
 .. image :: _static/images/datashow/2.4.6.2.png
在绑定数据时选择图形种类选择“散点”或者“动效散点”，“值字段”表示散点的数值
 .. image :: _static/images/datashow/2.4.6.3.png
数据绑定结果如下：
 .. image :: _static/images/datashow/2.4.6.4.png

线图及动效线图
.............................
线图和动效线图数据绑定方式是一样的，只是动效线图是有动态效果的，在绑定数据时选择对应的图形类别，并且设置“值2”此时，“值字段”代表线段的起点，“值2”代表线段的终点，数据绑定设置如下：
 .. image :: _static/images/datashow/2.4.6.5.png
数据绑定结果
 .. image :: _static/images/datashow/2.4.6.6.png
 
表格
~~~~~~~~~~~~~~~~~~~~~~~
表格就是列表，它是把查询结果展示出来，表格组件面板可以设置如下表格属性：
 .. image :: _static/images/datashow/2.4.7.1.png
* 显示风格：根据需求选择不同的风格
* 滚动方式：表格支持滚动，可以选择滚动的方式或者不滚动
* 数据：绑定数据
例如：组织查询SQL如下::

	 SELECT DISTINCT  concat(t.ID) as id,  t.INC_NO AS INC_NO,  t.INC_TOPIC AS INC_TOPIC, 
	 v.CATEGORY_NAME AS CATEGORY_NAME,  v.TYPE_NAME AS TYPE_NAME,  v.ITEM_NAME AS ITEM_NAME, 
	 g.GROUP_NAME AS GROUP_NAME,  dict.DICT_NAME AS STATUS   FROM tbl_itsm_incident_info t 
	 LEFT JOIN   VIEW_SYSTEM_ITSM_CTIINFO v  ON t.inc_class = v.item_id  LEFT JOIN   
	 TBL_ITSM_GROUP_INFO g  ON g.id = t.DEAL_GROUP_ID   LEFT JOIN   tbl_system_dict_info dict
	 ON dict.DICT_VALUE = t.INC_STATUS AND dict.dict_type='ITSM_HPD_STATUS'  AND dict.edit_status=0   
	 where t.inc_type=20
 
查询结果如下:
 .. image :: _static/images/datashow/2.4.7.2.png
根据查询结果绑定数据值的名称：
 .. image :: _static/images/datashow/2.4.7.3.png
样式列，支持自定义显示样式，分为通用样式和对数据进行标记，分别对应css语法和json字符串语法两种写法。
例如：标记标题列为蓝色：color:#07e2ff
标记状态列中的数据，显示已解决为绿色，处理中为红色，已分派为黄色，添加如下jsos串::

	 [{"value":"已解决" ,"style":"color:#fff;background:#007aff;padding:1px  5px;border-radius: 2px;"},
	 {"value":"处理中" ,"style":"color:#fff;background:red;padding:1px 5px;border-radius: 2px;"}, 
	 {"value":"已分派" ,"style":"color:#fff;background:#f0ad4e;padding:1px 5px;border-radius: 2px;"} ]

数据绑定结果为：
 .. image :: _static/images/datashow/2.4.7.4.png

雷达图
~~~~~~~~~~~~~~~~~~~~~~~~~
雷达图一般用于多维度分析，使用不同的指标分析事物的属性，雷达图有如下设置
 .. image :: _static/images/datashow/2.4.8.1.png
* 指示器设置：主要设置雷达图外框属性，是否显示名称，绘制类型，指示器段数和指示器颜色等
* 图形设置：图形显示设置，可以设置标记的图形，标记的大小，和图表的颜色
* 图例：设置图例显示位置
* 数据：绑定数据
例如：按日期统计工单的来源数量，组织SQL如下::

	 select date_format(prob_create_date,'%Y-%m') as date, count(PROB_ORIGIN=10 or null) as '事件流程升级',
	 count(PROB_ORIGIN=20 OR null) as  '主动事件分析',count(PROB_ORIGIN=30 OR null) as  '日常运维发现'
	 from tbl_itsm_problem_info where date_format(prob_create_date,'%Y%m') between '201801' and '201805'
	 group by date_format(prob_create_date,'%Y-%m')

查询结果:
 .. image :: _static/images/datashow/2.4.8.2.png
根据查询结果绑定数据data为类目，其他属性为指标：
 .. image :: _static/images/datashow/2.4.8.3.png
数据绑定结果
 .. image :: _static/images/datashow/2.4.8.4.png
 
运行状态图
~~~~~~~~~~~~~~~~~~~~~~~
运行状态一般用于展示机器状态，有如下设置：
 .. image :: _static/images/datashow/2.4.9.1.png
* 列数：设置每行显示几个图标
例如：要显示机器运行状态情况，准备SQL如下::

	 select * from testHtmlList

查询结果如下：
 .. image :: _static/images/datashow/2.4.9.2.png
根据查询结果绑定数据如下:
 .. image :: _static/images/datashow/2.4.9.3.png
数据绑定结果:
 .. image :: _static/images/datashow/2.4.9.4.png

嵌套环形图
~~~~~~~~~~~~~~~~~~~
环形嵌套图主要是用于展示有层级关系的数据，数据绑定参考饼图，需要注意的是，在查询数据时，需要查询多列指标且指标之间有层级关系展示才有意义，
例如::

	 select t.num, v.item_id, v.category_name as 一级,v.type_name as 二级, v.ITEM_NAME as 三级 
	 from (select count(id) as num ,inc_class from tbl_itsm_incident_info  
	 where inc_class is not null group by inc_class)t left join  view_system_itsm_ctiinfo v
	 on t.inc_class = v.item_id   where v.category_name is not null  and v.item_id!=19 order by v.category_name

查询结果如下：
 .. image :: _static/images/datashow/2.4.9.5.png
数据绑定结果如下：
 .. image :: _static/images/datashow/2.4.9.6.png
 
圆环
~~~~~~~~~~~~~~~~~~~~~~~
圆环其实是饼图的变形，区别在于圆环可以设置图例的位置和文字大小，且可以针对圆环做具体的颜色设定。
例如要查看网络设备的使用情况，准备数据如下::

	 select count(*) as num,ci_status,(select status_name from tbl_ci_status_define where id= ci_status) 
	 as status_name  from tbl_ci_base_info where geog_id=100002  group by ci_status

查询结果下:
 .. image :: _static/images/datashow/2.4.9.7.png
绑定数据做如下设置：
 .. image :: _static/images/datashow/2.4.11.1.png
绑定结果如下：
 .. image :: _static/images/datashow/2.4.11.2.png

分组框
~~~~~~~~~~~~~~~~~~~~~
分组框主要用于将不同的组件组合在一起，方便组件移动和布局。分组框有个
锁的状态，用来控制组件是否为一个组合，当锁处于打开状态时，
组件未锁定，单个组件可以自由拖动，当锁处于关闭状态时，组件锁定为一个组合，只能一起移动。

定制圆环
~~~~~~~~~~~~~~~~~~~~~~~
暂无

联动
=====================
数据联动
--------------------
发送源和接收源
~~~~~~~~~~~~~~~~~~~~~~~

设计联动模板时，首先要确定发送源和接收源，在发送源勾选联动属性，目的是将数据发送出来便于接收源接收；在接收源通过设置变量来接收发送源发送的数据，
变量的写法是固定的格式，使用EEL语法：${var.参数名}。例如下图一（发送源）统计的是各个事件来源情况，点击每个来源查看对应的详情如图二（接收源）
 .. image :: _static/images/datashow/2.5.1.1.1.png 
 .. image :: _static/images/datashow/2.5.1.1.2.png
设计时，发送源勾选联动属性：
 .. image :: _static/images/datashow/2.5.1.1.3.png
在接收源设置变量，在数据绑定对话框，将要设置的条件参数用EEL表达式替换，'${var.inc_origin}' ，另外还需要填写调试值，
调试值的目的是为了sql语句能正常运行。如果有多个参数都可以用EEL表达式替换，每个组件都可以是发送源或者接收源，在使用中根据实际设置即可。
 .. image :: _static/images/datashow/2.5.1.1.4.png
 
联动变量赋值
~~~~~~~~~~~~~~~~~~~~~~~

在确定了发送源和接收源以后，需要给联动变量赋值。在全局属性设置栏，点击联动变量，给联动变量赋值。接上例，只有一个参数var.inc_origin，
其中url是指在url参数中传递默认值，用于接收的参数；监听组件是选择发送源的card；事件选择数据项点击；数据列名选择是用于指定传递的参数值的列名；
缺省值填写初始值。一个看板可以有多个联动变量，所有的联动变量都在时间设置列表设置，如果有同名的变量，会自动合并为一个。
 .. image :: _static/images/datashow/2.5.1.2.1.png
 
事件联动
--------------------
暂无 

删除组件
=====================
组件的删除有两种方式，一是页面删除，双击选择要删除的组件，组件右上角出现删除图标，点击即删除。
二是快捷键删除，双击选择要删除的组件，按住键盘的【Ctrl】+【Delete】键删除组件。

复制组件
========================
有时候在设计报表时，多个组件重复用到，且设置类似时就可以用到复制功能。
选择要复制的组件，鼠标右键，点击复制即可

保存
============================
点击设计页面的保存按钮，弹出保存对话框，输入要保存的看板名称，设计的看板会以文件的形式存放到配置的目录。

 
查看
==================================
在浏览器输入地址http://[host]:[port]访问，查看保存好的看板，点击看板后面对应的【查看】，或者【全屏查看】按钮查看设计好的看板，如果有需要调整，点击【编辑】按钮，可以进行重新设计。
例如设计好的看板展示：
 .. image :: _static/images/datashow/3.1.png

附件
=====================================
bicenter-datashow 视频演示
-----------------------------
链接：https://pan.baidu.com/s/1BHfeXvNZCNJkdZ5SCoW6qg 密码：v8pn

												   