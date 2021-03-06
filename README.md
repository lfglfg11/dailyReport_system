# 日报数据处理系统

​	分享一个python程序化处理日报的完整的实际例子，给python初学者以及想利用python减轻日常工作的同学一些参考。

- [简介](#简介)
- [界面展示](#界面展示)
- [数据准备](#数据准备)
- [数据处理](#数据处理)
- [输出结果](#输出结果)
- [算法介绍](#算法介绍（待更新）)
- [维护和更新](#维护和更新)

## 简介

​	在数据化未完全发展的时代，企业的管理者总是通过“日报”对业务进行及时了解，非常多的同学还是通过excel函数或excel插件进行日报出数，效率较低。我在使用python批量处理日报数据后，日报处理时间由原来的3小时缩短为半小时，时间减少了83%，大大提高了工作效率。

​	此系统使用的是python语言，主要应用pandas库实现数据的各种运算。

## 界面展示

![界面展示](/005-others/source/jiemian.png)

## 数据准备

​	相同类型的数据统一存储。所有当月日报数据均存储于"/002-new_data"文件夹中，历史月份数据以及程序的中间数据存储于"/001-mid&old_data"文件夹中，结果数据存储于"/003-result"文件夹中。

在"/002-new_data"文件夹中数据需每日更新：

* **协同O2O数据**
  * **001-OSC_shouli.csv** 当月OSC系统总订单
  * **002-导入清单统计.xls** 当月系统导入订单
  * **003-dwddqd.csv** 合伙人提供的多维订单清单
  * **004-Others.xlsx** 其他数据源得到的数据
  * **受理订单_2018-05-28.xlsx** OSC系统当天新增订单及当天状态变更订单

* **宽带** 当天导出的宽带订单，存于"/002-new_data/KD"文件夹中
* **ITPV**  当天导出的IPTV订单，存于"/002-new_data/IPTV"文件夹中
* **提速** 当天导出的提速订单，存于"/002-new_data/TISU"文件夹中

跨周、跨月时需注意，详见[数据更新](#跨周、跨月数据更新)；

## 数据处理

​	在准备好数据后，运行Data_System.py。各业务数据处理可分步执行，也可一键处理。

下面以处理宽带数据为例，演示此系统操作流程：

#### 1.打开数据

正常打开数据后，状态栏会显示"已读入宽带数据！"

![打开数据演示](/005-others/source/yanshi1.png)

#### 2.检查数据

点击检查缺数按钮，在状态栏会显示结果，如下图则是缺数情况，结合处理数据中的各地市发展日环比图可更细化地检查数据异常情况。

![检查数据演示](/005-others/source/yanshi2.png)

#### 3.处理数据

点击处理数据按钮，处理完成后会弹出2个图像窗口(用户监控数据发展情况)，并且状态栏会显示"宽带数据已更新"。图中发展量未说明的均为电渠发展量。

![处理数据演示](/005-others/source/yanshi3.png)

**图像弹窗1**：分月份入网量，左侧为电子渠道上月同期累计入网量，右侧为本月累计入网量。

![分月份入网量演示](/005-others/source/yanshi4.png)

**图像弹窗2**:弹窗由4幅图像组成

![](/005-others/source/yanshi5.png)

- **左上**：各地市发展量的日环比，即 $$\frac{当日发展量}{上一日发展量}-1$$ 可用于监控数据变动和及时发现数据异常
- **右上**：发展量前5地市，当月发展量对比上月发展量，用于监控主要地市的月累计发展量变动情况
- **左下**：全省当月分天入网量，用于看全省发展量走势
- **右下**：分阵地入网量，用于分析发展渠道的发展量变动情况，可用于追溯某地市发展量变动的阵地来源

#### 4.一键处理

一键执行"打开数据"、"检查数据"、"处理数据"，点击后，可以先去喝一杯热茶再回来看结果。

## 输出结果

- "/002-new_data/001-OSC_shouli.csv"：在"协同O2O->处理数据"更新，把OSC当天数据加到当月累计订单中
- "/003-result/007-combined.xlsx"：在"协同O2O->处理数据"更新，O2O当月数据聚合后的结果
- "/003-result/kuandai.xlsx"：在"宽带数据->处理数据"更新，宽带数据处理结果
- "/003-result/iptv.xlsx"：在"IPTV数据->处理数据"更新，IPTV数据处理结果
- "/003-result/tisu.xlsx"：在"提速数据->处理数据"更新，提速数据处理结果

## 算法介绍（待更新）

* 日期处理
* OSC数据合并更新
* 缺数检查
* 环比占比
* 按规则读取文件
* 钻取分析

## 运行环境

推荐在windows系统下Anaconda(py3.6版本)下运行

## 维护和更新

#### 跨周、跨月数据更新

- **跨周**：由于OSC系统订单需要每天叠加，在周一时需要下周三份数据（上周五、上周六、上周日），然后通过"/005-others/合并周末OSC/osc_hb.py"程序更新(会自动按照命名规则更新到"/002-new_data"目录下)
- **跨月**：需要把当月的数据对应地"搬运"到"001-mid&old_data"目录下。
  - 在"/003-result/"目录下打开007-combined.xlsx筛选当月数据，复制到"\001-mid&old_data\"下的002-DR_history.xlsx，并且删除掉"/002-new_data/"目录下相关O2O数据当月的记录
  - 宽带、IPTV、提速数据，需要把最后一天数据搬迁"\001-mid&old_data\"下

#### 阵地对应表更新

对应表是用于根据销售渠道、渠道明细匹配对应的阵地，当发现有阵地为空白的订单，就需要更新对应表了，对应表存放于"001-mid&old_data"目录下

- **088-zddyb.csv**：宽带对应表
- **089-zddyb.csv**：IPTV对应表

部分对应表：

| 销售渠道 | 渠道明细     | 阵地     |
| :------- | :----------- | -------- |
| 地市销售 | 网厅         | 地市网厅 |
| 省销售   | 网厅         | 省网厅   |
| 省销售   | 索答         | 装维毛细 |
| 省销售   | 宽带客服导购 | 外呼导购 |
| 省销售   | 深圳傲天     | 省分销   |

#### OSC系统增加新业务

​	打开oscpc.py，搜索"OSC系统新增活动"，根据新活动的筛选条件，修改场景名称以及业务小类，如新增活动"517订单"，筛选条件为字段'工作组名称'为'省公司电子渠道运营中心-京东淘宝'。需要增加如下代码：

```python
#修改场景名称，osc_tt是一个dataframe，代表当月OSC系统全量订单，osc_total的缩写
osc_tt.loc[osc_tt['工作组名称']=='省公司电子渠道运营中心-京东淘宝','场景名称']='517订单'
#修改业务小类
osc_tt.loc[osc_tt['场景名称']=='517订单','业务小类']='融合'
```

​	在代码更新后，需要同时更新O2O活动对应表——"\003-result\006-对应表.xlsx"，主要更新此工作簿中"场景对应表"以及"业务对应表"。下面就是517活动对应表新增的记录，其中业务对应表只有当新增业务才需要增加新记录，由于业务对应表此前已有融合，因此无需新增。

| 场景细类 | 场景名称 | 业务小类 | 业务平台       | 业务平台划分 | 订单类型划分 |
| -------- | -------- | -------- | -------------- | ------------ | ------------ |
| 517订单  | 517订单  | 融合     | 光宽升级及提速 | 线上引流     | 正式单       |

| 业务小类 | 业务类型 | 业务价值类型 |
| -------- | -------- | ------------ |
| 融合     | 融合     | 新装         |



