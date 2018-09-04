.. _bicenter_list:

BICENTER 自由报表常见问题解决方案
====================================

BICENTER常用下载
=========================================
导出PDF重影,字体模糊
-----------------------
 * 原因：缺少字符集，下载字符增量文件，将.ZIP文件解压以后的拷贝到$JAVA_HOME/jre/lib/fonts目录下(注意，拷贝时须包含fallback文件夹)
 * `点击下载字体增量 <https://github.com/bicenter2018/bicenter-build/blob/master/source/_static/downloadRes/chinese.font.patch.zip>`
 
