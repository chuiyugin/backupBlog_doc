---
title: 计量方法与误差理论
tags:
  - 误差理论
categories:
  - 误差理论
date: 2024-11-16 15:40:00
excerpt: 计量方法与误差理论知识汇总！
---
# 计量方法与误差理论
## 误差部分
### 经典误差理论
#### 误差的基本概念
+ 真值：在一定条件下被测量的真实数值（通常用字母 `A` 表示真值），测量的目的就是为了获得真值。
+ 绝对误差：由测量所得到的**被测量值**与其**真值**之差。
+ 相对误差：由**绝对误差**作为分母。

![相对误差](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411161550613.png)

+ 分贝误差：

![分贝误差](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411161605508.png)

+ 分贝：

![分贝](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411161606580.png)

+ 误差分类：
	+ 随机误差：统计特性、计算方法、评价标准、不可消除
+ 系统误差：
	+ 消除或者减弱的方法、各类判据
+ 粗大误差：
	+ 测量误差

#### 随机误差的基本性质
+ 随机误差的**大小和方向都不固定**，也无法精确测量或校正，但它的规律性是在**大量观测数据**中表现出来的统计规律。在**多次**测量的过程中，产生的随机误差具有以下规律:
	+ 对称性: 绝对值相等的正误差与负误差出现的概率相同。
	+ 单峰性: 绝对值小的误差比绝对值大的误差出现的概率大。
	+ 有界性: 绝对值很大的误差出现的概率接近于零，即随机误差的绝对值不会超过一定界限。
	+ 抵偿性: 当测量次数 `n→∞` 时，全部误差的代数和趋于零。

#### 随机误差的分布特征
+ 随机误差的分布特征有：
	+ 正态分布
	+ t 分布
	+ 非均匀分布等

#### 随机误差的数字特征
##### 数学期望
+ 算术平均值为数学期望的估计值：

![算术平均值](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411171814947.png)
##### 标准差
+ 离散数据标准差的估计值：

![标准差](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411171817282.png)

+ 由于真值无法得知，因此测量数据的误差不能准确给出，因此不能够按照上述式子求得标准差，必须结合测量数据对标准差进行合理估计，通常采用贝塞尔公式进行估计。
+ 贝塞尔公式：

![贝塞尔公式](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411171820446.png)

+ 贝赛尔公式估算条件：测量次数 `n` 比较大。

##### 标准差的其他估算方法
+ 别捷尔斯法
+ 极差法

![极差法概念](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411171830090.png)

![极差法例题](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411171830816.png)

+ 最大误差法：

![最大误差法理论](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411171831104.png)

![最大误差法例题](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411171831993.png)

+ 四种计算方法的优缺点：

![四种计算方法的优缺点](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411171840477.png)

##### 多次测量结果的精度指标
+ 多次测量的算术平均值的标准差：

![多次测量的算术平均值的标准差](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411172118051.png)

+ 算术平均值的精度指标（常用的有 4 个）：

![算术平均值的精度指标](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411172120867.png)

+ 总结：

![总结](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411172121469.png)
#### 非等精度测量
##### 权的概念
+ “权”可以理解为各组测量结果相对的可信赖程度，测量结果越可靠，其“权”越大，即可靠性越大的测量结果在最后结果中所占的比重越大。

##### 方差已知
+ 权与方差成反比！权表示相对可靠程度，是一个无量纲的数，允许给各组的权数同时增大或者减小若干倍，而比例关系不变。

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411202221945.png)
##### 多次重复测量的权
+ 以多组重复测量为例，测量次数决定权值，即

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411202223699.png)

##### 加权平均表达式
+ 知道权值便可以求加权平均表达式：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411202224912.png)

##### 加权平均的精度参数
+ 误差合成原理：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411202226913.png)
+ 总结：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411202227886.png)

##### 例题
+ 已知方差：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411202228761.png)

+ 方差未知，知道测量次数：

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411202229204.png)

![](https://yugin-blog-1313489805.cos.ap-guangzhou.myqcloud.com/202411202230441.png)







