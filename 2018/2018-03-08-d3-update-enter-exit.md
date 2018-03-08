# 理解 D3 update, enter, exit
[参考文章: 理解 Update、Enter、Exit](http://www.ourd3js.com/wordpress/149/)
这个三个都是面向界面元素的，update表示要更新的现存的界面元素，enter表示马上要添加的界面元素，exit表示要从界面已有中删除的界面元素。  
- 拿到update：一般根据最新的数据更新如文字、位置、颜色等信息；
- 拿到enter：一般通过append来添加新的界面元素
- 拿到exit：一般将其删除掉

当每次选择目标界面元素(n个)，尝试往里面灌数据的时候(m个)，就会获得这三个：
1. n = m
  - update集合是m个，对应的数据就是新的m个数据
  - enter集合为空
  - exit集合为空
2. n < m 新来数据
  - update集合是n个，对应是m的前n个数据
  - enter集合是m - n个，对应的数据是m的后面(m - n)个数据
  - exit集合为空
3. n > m 界面元素多了
  - update集合m个，对应数据就是刚灌的m个数据
  - enter集合为空
  - exit集合是n - m个，一般直接调用remove把他们从界面删除掉

比如界面元素中点是circle，线是line，数据中点是node，边是edge，那么每次数据有更新需要更新d3绘图的时候：
1. 找到所有的circle，把全部的node灌进去
2. 所有的update更新其文字，位置等
3. 所有的enter append circle
4. 所有的exit进行remove

line也同理

> 坑1：直接console.log(update/enter/exit)不是真正foreach的集合，要随便append一个东西，each后看到的集合才是真的；直接打印出来的里面有乱七八糟的东西

> 坑2：一开始灌数据得到的直接是update集合，需要update.enter()和update.exit()后得到的另外两个集合，感觉挺乱的

其中d3的力导向图，由于有tick回调，实时知道位置信息，所以line的update可以不用操作。

力导向图的代码，可以直接在[这里](http://jsfiddle.net/P4Z75/1624/)运行查看，进行了：
1. 先有一个节点
2. 3秒后增加了邻居
3. 再3秒后删除了部分邻居
4. 再3秒后同时删除了部分邻居和增加了新邻居
5. 可以看到每次的update、enter、exit集合的内容
```
const svg = d3.select('svg')
const nodes = [
  { name: 'root' }
]
const edges = []

const force = d3.layout.force()

function log (name, update, enter, exit) {
  console.log(`**********${name}**********`)
  console.log('update')
  update.append('test').text((d, i) => {
    console.log(d)
    return ''
  })
  console.log('enter')
  enter.append('test').text((d, i) => {
    console.log(d)
    return ''
  })
  console.log('exit')
  exit.append('test').text((d, i) => {
    console.log(d)
    return ''
  })
}

function updateEdges (edges) {
  const update = svg.selectAll('line').data(edges)
  const enter = update.enter()
  const exit = update.exit()
  log('edge', update, enter, exit)
  enter.append('line').style('stroke', '#ccc').style('stroke-width', 1)
  exit.remove()
}

function updateNodes (nodes) {
  const color = d3.scale.category20()
  const update = svg.selectAll('circle').data(nodes)
  const enter = update.enter()
  const exit = update.exit()
  log('node', update, enter, exit)
  enter.append('circle').attr('r', 20).style('fill', (d, i) => color(i)).call(force.drag)
  exit.remove()
}

function updateTexts (nodes) {
  const update = svg.selectAll('text').data(nodes)
  const enter = update.enter()
  const exit = update.exit()
  log('text', update, enter, exit)
  update.text((d) => d.name)
  enter.append('text').style('fill', 'black').attr('dx', 20).attr('dy', 8).text((d) => d.name)
  exit.remove()
}

function updateGraph () {
  force.nodes(nodes).links(edges).size([300, 500]).linkDistance(150).charge([-400])
  force.start()
  updateEdges(edges)
  updateNodes(nodes)
  updateTexts(nodes)
}

force.on('tick', function () {
  svg.selectAll('line').attr('x1', (d) => d.source.x).attr('y1', (d) => d.source.y).attr('x2', (d) => d.target.x).attr('y2', (d) => d.target.y)
  svg.selectAll('circle').attr('cx', (d) => d.x).attr('cy', (d) => d.y)
  svg.selectAll('text').attr('x', (d) => d.x).attr('y', (d) => d.y)
})

updateGraph()

function addNeighbors () {
  nodes.push({name: 'child1'}, {name: 'child2'}, {name: 'child3'})
  edges.push({ source: 0, target: 1 }, { source: 0, target: 2 }, { source: 0, target: 3 })
  updateGraph()
}

function removeNodes () {
  nodes.splice(2, 2)
  edges.splice(1, 2)
  updateGraph()
}

function bothAddAndRemove () {
  nodes.splice(1, 1)
  edges.splice(0, 1)
  nodes.push({name: 'anotherChild1'}, {name: 'anotherChild2'}, {name: 'anotherChild3'}, {name: 'anotherChild4'})
  edges.push({ source: 0, target: 1 }, { source: 0, target: 2 }, { source: 0, target: 3 }, { source: 0, target: 4 })
  updateGraph()
}

setTimeout(addNeighbors, 3000)
setTimeout(removeNodes, 6000)
setTimeout(bothAddAndRemove, 9000)
```
