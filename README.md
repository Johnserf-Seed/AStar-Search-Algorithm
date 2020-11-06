# 最短路径问题

作者：*JohnserfSeed*

### 在游戏中，当我们需要让角色移动到指定位置时，只需要鼠标轻轻的一点就可以完成这简单的步骤，系统会立即寻找离角色最近的一条路线

![lol_map](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/092610ymfe9mki9clmurrf.png)

### 可是，这背后的行为逻辑又有什么奥秘呢？ 你会怎么写这个寻路算法呢？

一般我们遇到这种路径搜索问题，大家首先可以想到的是<strong>[广度优先搜索算法(Breadth First Search)](https://baike.baidu.com/item/宽度优先搜索)</strong>、还有<strong>[深度优先(Depth First Search)](https://baike.baidu.com/item/深度优先搜索)</strong>、<strong>[弗洛伊德（Floyd）](https://baike.baidu.com/item/Floyd算法)</strong>、<strong>[迪杰斯特拉(Dij)](https://baike.baidu.com/item/迪克斯特拉算法)</strong>等等这些非常著名的路径搜索算法，但是在绝大多数情况下这些算法面临的缺点就暴露了出来：<strong><span style ="border-bottom: 1px dashed red;">时间复杂度比较高</span></strong>。

所以，大部分环境里我们用到的是一个名叫**A* (A star)**的搜索算法

[![A1.gif](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/153658znadyl0hnolw2spu.gif)](https://wx1.sinaimg.cn/mw690/006908GAly1gke8xwvjb5g30hs0br1jf.gif)

<u>(点击图片浏览动图)</u>

说到最短路径呢，我们就不得不提到广度优先遍历（**BFS**），它是一个万能算法，它不单单可以用在
寻路或者搜索的问题上。**windows**的系统工具：画板 中的油漆桶就是其比较典型一个的例子

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/144406jwphh8v97ane9bif.png)

## 这里对路径搜索做一个比较简洁的示例

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/145601lvsy9mtu3jobkxee.png)

假设我们是在一个网格上面进行最短路径的搜索

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/145740wdi0rgrs4pvyuvsx.png)

我们只能上下左右移动，不可以穿越障碍物。算法的目的是为了能让你寻找到一条从起点到站点的最短路径

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/150044egs9zbeyqiuqyesr.png)

假设每次都可以上下左右朝4个方向进行移动

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/150239m5vruhmmyxbumppt.png)

算法在每一轮遍历后会标记这一轮探索过的方块称为边界（Frontier）,就是这些绿色的方块。

[![A3.gif](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/153624zwgpx9gxpe6nqeyb.gif)](https://wx1.sinaimg.cn/mw690/006908GAly1gkegd3lzg7g30en09n7rb.gif)
<u>(点击图片浏览动图)</u>

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/184210r8xhoxoqnbgsyjlb.png)

然后算法呢会循环往复的从这些边界方块开始，朝他们上下左右四个方向进行探索，直到算法遍历到了终点方块才会停止。而最短路径呢就是算法之前一次探索过的路径。为了得到算法探索过的整条路径呢，我们可以在搜索的过程中顺势记录下路径的来向。

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/2119117kuo6smhawhqwgs0.png)

比如这里方块上的白色箭头就代表了之前方块的位置

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/212732i6engi1e4nbrtafb.png)

在每一次探索路径的时候，我们要做的也只是额外的记录下这个信息

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/214940aqwxiugecpdgqbek.png)

要注意，所有探索过的路径我们需要将它们标记成灰色，代表它们“已经被访问过“，这样子算法就不会重复探索已经走过的路径了。

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/215314fxykyhduiryipeil.png)

广度优先算法显然可以帮助我们找到最短路径,不过呢它有点傻,因为它对路径的寻找是没有方向性的，它会向各个方向探测过去。

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/230822nxiwura8xqorfydr.png)

最坏的情况可能是找到终点需要遍历整个地图，因此很不智能，我们需要一个更加高效的算法。

就是本次我们要介绍的**A * （A star）**搜索算法



## A* Search Algorithm


”A*搜索算法“也被叫做“启发式搜索”

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/233104fdaotgrhavqt0hhz.png)

与广度优先不同的是，我们在每一轮循环的时候不会去探索所有的边界方块（**Frontier**），而会去选择当前“代价（**cost**）”最低的方块进行探索。

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/233443gzf5kxxtbezlzcte.png)

这里的“代价”就很有意思了，也是A*算法智能的地方。

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/05/2337103szdyyddb8zpr1dc.png)

我们可以把这里的代价分成两部分，一部分是“当前路程代价（可表示成**f-cost**）”：比如你从起点出发一共走过多少个格子，**f-cost**就是几

另一部分是“预估代价（可表示成**g-cost**）”：用来表示从当前方块到再终点方块大概需要多少步，预估预估所以它不是一个精确的数值，也不代表从当前位置出发就一定会走那么远的距离，不过我们会用这个估计值来指导算法去优先搜索更有希望的路径。


最常用到的“预估代价”有欧拉距离（**Euler Distance**）“，就是两点之间的直线距离**(x1 - x2)^2 + (y1 - y2)^2**

当然还有更容易计算的“曼哈顿距离（**Manhattan Distance**）”，就是两点在竖直方向和水平方向上的距离总和**|x1 - x2|+|y1 - y 2|**

曼哈顿距离不用开方，速度快，所以在A* 算法中我们可以用它来充当<strong>g-cost</strong>

接下来，我们只要把之前讲到的这两个代价相加就得出了总代价：**f-cost + g-cost**

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/000226bussojsazawbojws.png)

然后在探索方块中，优先挑选总代价最低的方块进行探索，这样子就会少走很多弯路

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/000328yb9vf13dlnnhxeiz.png)

而且搜索到的路径也一定是最短路径。

在第一轮循环中，算法对起点周围的四个方块进行探索，并计算出“当前代价”和“预估代价”。

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/000652sn5mrn4xkylpnz3f.png)

比如这里的**1**代表从起步到当前方块走了1步

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/001558epvekfdmdxxfabwo.png)

这里的4代表着方块到终点的曼哈顿距离，在这四个边界方块中，右边方块代价最低，因此在下一轮循环中会优先对它进行搜寻

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/001812xs42vaayvv0estmn.png)

在下一轮循环中，我们已同样的方式计算出方块的代价，发现最右边的方块价值依然最低，因此在下一轮的循环中，我们对它进行搜寻

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/002242gvmh43eap3owwgmt.png)

算法就这样子循环往复下去，直到搜寻到终点为止

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/002404qziqtjnrygbzguuh.png)

增加一下方块的数量级，**A***算法同样可以找到正确的最短路径

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/002527crucrzm681jyzbrp.png)

最为关键的是，它搜寻的方块个数明显比广度优先遍历少很多，因此也就更高效。

理解了算法的基本原理后，接下来就是上代码了，这里我直接引用**redblobgames**的**Python**代码实现，因为人家实在写的太好了！

```python
def heuristic(a, b):	#Manhattan Distance
    (x1, y1) = a
    (x2, y2) = b
    return abs(x1 - x2) + abs(y1 - y2)

def a_star_search(graph, start, goal):
	frontier = PriorityQueue()
    frontier.put(start, 0)
    came_from = {}
    cost_so_far = {}
    came_from[start] = None
    cost_so_far[start] = 0
    
    while not frontier.empty():
        current = frontier.get()
        
        if current = goal:
            break
            
        for next in graph.neighbors(current):
            new_cost = cost_so_far[current] + graph.cost(current, next)
            if next not in cost_so_far or new_cost < cost_so_far[next]:
                cost_so_far[next] = new_cost
                priority = new_cost + heuristic(goal, next)
                frontier.put(next, priority)
                came_from[next] = current
                
    return came_from, cost_so_far
```

先来看看最上面几行，**frontier**中存放了我们这一轮探测过的所有边界方块(之前图中那些绿色的方块)

```python
frontier = PriorityQueue()
```

**PriorityQueue**代表它是一个优先队列，就是说它能够用“代价”自动排序，并每次取出”代价“最低的方块

```python
frontier.put(start, 0)
```

队列里面呢我们先存放一个元素，就是我们的起点

```python
came_from = {}
```

接下来的的 **came_from** 是一个从当前方块到之前的映射，代表路径的来向

```python
cost_so_far = {}
```

这里的 **cost_so_far** 代表了方块的“当前代价”

```python
came_from[start] = None
cost_so_far[start] = 0
```

这两行将起点的 **came_from** 置空，并将起点的当前代价设置成0，这样子就可以保证算法数据的有效性

```python
while not frontier.empty():
	current = frontier.get()
```

接下来，只要 **frontier** 这个队列不为空，循环就会一直执行下去，每一次循环，算法从优先队列里抽出代价最低的方块

```python
if current = goal:
	break
```

然后检测这个方块是不是终点块，如果是算法结束，否则继续执行下去

```python
for next in graph.neighbors(current):
```

接下来，算法会对这个方块上下左右的相邻块，也就是循环中 **next** 表示的方块进行如下操作

```python
new_cost = cost_so_far[current] + graph.cost(current, next)
```

算法会先去计算这个 **next** 方块的“新代价”，它等于之前代价 加上从 **current** 到 **next** 块的代价

由于我们用的是网格，所以后半部分是 **1** 

```python
if next not in cost_so_far or new_cost < cost_so_far[next]:
```

然后只要 **next** 块没有被检测过，或者 **next** 当前代价比之前的要低

```python
frontier.put(next, priority)
```

我们就直接把他加入到优先队列，并且这里的总代价**priority**等于**“当前代价”**加上**”预估代价“**

```python
priority = new_cost + heuristic(goal, next)
```

预估代价就是之前讲到的“曼哈顿距离”

```python
def heuristic(a, b):
    (x1, y1) = a
    (x2, y2) = b
    return abs(x1 - x2) + abs(y1 - y2)
```

之后程序就会进入下一次循环，重复执行之前的所有步骤

这段程序真的是写的特别巧妙，可能比较难以理解可是多看几遍说不定你就突然灵光乍现了呢



## 拓展

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/135259grddrjp4fww7c1rt.png)

如果把地图拓展成网格形式（**Grid**），因为图的节点太多，遍历起来会非常的低效

于是我们可以吧网格地图简化成 节点更少的路标形式（**WayPoints**）

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/1356534w6gfm4eg6pxrgkc.png)

然后需要注意的是：<u>***这里任意两个节点之间的距离就不再是1了，而是节点之间的实际距离***</u>

我们还可以用自上而向下分层的方式来存储地图

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/135938xj2qk6dx9ydp9u5u.png)

比如这个**四叉树（Quad Tree）**

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/140119og8ulxeqwb0nrlaq.png)

又或者像unity中使用的**导航三角网（Navigation Mesh）**，这样子算法的速度就会得到进一步优化


另外，我还推荐**redblobgames**的教程

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/140616crukbk0ggzzq9k43.png)

各种算法的可视化，以及清楚的看见各种算法的遍历过程、中间结果

![image.png](https://bbs-img-cbc-cn.obs.cn-north-1.myhuaweicloud.com/data/attachment/forum/202011/06/140737ifqctqryrmiumeqh.png)

以及各种方法之间的比较，非常的直观形象，对于算法的理解也很有帮助。




<hr>

参考：

[1]周小镜. 基于改进A*算法的游戏地图寻径的研究[D].西南大学,2010.
[2]https://www.redblobgames.com/pathfinding/a-star/introduction.html
[3]https://en.wikipedia.org/wiki/A*_search_algorithm
