title:  记一次性能优化的过程
date: 2017年02月26日17:34:35
categories:  "node.js"
tag: 
- node.js
- linux
  comments: true
---

> 网站长期以来都有一个非常大的问题：当请求数过多时，服务器响应的特别慢......

# 定位

思考：造成慢的可能的原因：

* 机器负载过高，超出了应有的承受量
* 数据库查询慢
* 代码问题

通过对服务器的各项指标作监控时，发现在请求数过多时，CPU基本处于满载的状态。因此初步断定是CPU使用率过高导致服务器处理能力下降。

  怎么办？加机器！

于是马上加了机器，结果新的机器情况也是一样：

发现node进程基本占用了整个CPU，而且通过cluster模式开多核发现，几乎所有的核CPU都到达了100%。



通过对两台机器监控，我发现他们在同一时间内CPU使用率的图形趋势是基本一致的：

![cpu usage](https://ww1.sinaimg.cn/large/006tNc79gy1fdj8vy6kvoj30s40bk40d.jpg)

可以看到两台机器的图形是一致的，我基本锁定了是代码的问题。





# 1. 寻找监控工具

这时候，需要的就是对代码的性能作监控了，通过google和baidu，我发现了：

* APM（[Applicaton Performance Management](https://en.wikipedia.org/wiki/Application_performance_management)）
* 火焰图
* AliNode



## 1. APM

APM的使用是非常简单的，只需要以下的操作并作一些相应的配置就可以了：

```javascript
//oneapm
require('oneapm');
//mmtrix
require('mmtrix');
```

优点：

1. 使用非常简单，对代码的侵入性低
2. 在程序没有部署任何监控的时候，可以获得接口响应时间、吞吐量

缺点：

1. 数据只能作为参考，并不一定很准确
2. 只能发现表象的问题，无法发现深层次的问题

我感觉我们需要一个APM来监控我们的代码，国内比较好的有`oneapm`,`性能魔方`，但是，在使用的过程中，我发现这一类的工具并不能发现真正的问题，只能给你一个大概的方向，因为缺乏对底层的监控。

通过性能魔方，我们找到了访问慢的接口，但是并没有找到问题。



## 2. 火焰图

![](http://www.brendangregg.com/FlameGraphs/cpu-bash-flamegraph.png)

看，这就是火焰图，如同一团熊熊燃烧的大火。

每一格代表了一个在栈中的函数。

Y轴表示的是栈的深度，最顶上的格子是当前CPU正在运行的函数，下面的函数是祖先，就像我们平时debug的栈一样。

X轴从左到右的顺序是没有意义的，只是根据字母的顺序进行了排列。

每个格子的宽度表示的是CPU

**作用：快速发现CPU的性能瓶颈。**



# 3. 火焰图的工作原理？

火焰图是根据Perf生成的



# 4. 火焰图的使用方法？

1. 启动node服务器

```javascript
node --perf-basic-prof-only-functions bin/www
```

2. 启动perf监控node程序

   ​

3. 将监控结果导出

4. 去掉一些无用的信息

5. 生成火焰图

```shell
# 使用--perf-basic-prof-only-functions 
node --perf-basic-prof-only-functions bin/www

sudo perf record -m 1 -F 99 -p pgrep -n node -g -- sleep 30

sudo perf script > nodestacks

sed -i '/[unknown]/d' nodestacks

./stackcollapse-perf.pl < ../nodestacks | ./flamegraph.pl --colors js > ../node-flamegraph.svg
```







引用：

http://blog.oneapm.com/apm-tech/618.html