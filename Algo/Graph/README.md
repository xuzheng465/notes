

用队列

有个set，不要让节点重复进队列

```java
public class Node {
  public int value;
  public int in;
  public int out;
  public ArrayList<Node> nexts;
  public ArrayList<Edge> edges; // 边的集合 从这个点发散出去的边是什么
  
  public Node(int value) {
    this.value = value;
    in = 0;
    nexts = new ArrayList<>();
    edges = new ArrayList<>();
  }
}
```



```java
public class Edge {
  public int weight;
  public Node from;
  public Node to;
  
  public Edge(int weight, Node from, Node to) {
    this.weight = weight;
    this.from = from;
    this.to = to;
  }
  
}
```



```java
public class Graph {
  public HashMap<Integer, Node> nodes; // 《编号， 点》
  public HashSet<Edge> edges;
  // 点的集合，和边的集合
  public Graph() {
    nodes = new HashMap<>();
    edges = new HashSet<>();
  }
}
```

[weight, from, to]

[7, 0, 1]

[3,1,2]

[5,0,2]

一个起到过渡作用中间类（接口），把提供的数据类型（以上），转化成符合你自己的算法模版

```java
public class GraphGenerator {
  
  // matrix 所有的边
  // N*3 的矩阵
  // [weight, from节点上的value，to节点上的value]
  public static Graph createGraph(Integer[][] matrix) {
    Graph graph = new Graph();
    for (int i = 0; i < matrix.length; i++) {
      Integer from = matrix[i][1];
      Integer to = matrix[i][2];
      Integer weight = matrix[i][0];
      
      // 查看from的点有没有创建过
      // 如果图中不含有from，则建出一个节点
      if (!graph.nodes.containsKey(from)) {
        graph.nodes.put(from, new Node(from));
      }
      
      // 查看有没有to这个点
      // 如果没有就在图上挂上这个节点
      if (!graph.nodes.containsKey(to)) {
        graph.nodes.put(to, new Node(to));
      }
      
      // 这时无论如何都有from to
      // 拿出from和to
      Node fromNode = graph.nodes.get(from);
      Node toNode = graph.nodes.get(to);
      
      Edge newEdge = new Edge(weight, fromNode, toNode);
      fromNode.nexts.add(toNode);
      fromNode.out++;
      toNode.in++;
      fromNode.edges.add(newEdge);
      graph.edges.add(newEdge);
    }
    return graph;
  }
  
}

```

没有模版库去面试，和裸考一样。

宽度优先遍历

1. 利用**队列**实现
2. 从源节点开始依次按照宽度进队列，然后弹出
3. 每弹出一个点，就把该点所有没有进过队列的邻接点放入队列。
4. 直到队列变空



​	



```java
public static void bfs (Node node) {
  if (node == null) {
    return;
  }
  Queue<Node> queue = new LinkedList<>();
  HashSet<Node> set = new HashSet<>();
  queue.add(node);
  set.add(node);
  while (!queue.isEmpty()) {
    Node cur = queue.poll();
    System.out.println(cur.value);
    for (Node next : cur.nexts) {
      if (!set.contains(next)) {
        set.add(next);
        queue.add(next);
      }
    }
  }
}
```

set 用来标记

节点不会重复的进入队列。



广度优先遍历

1. 利用**栈**实现
2. 从源节点开始把节点按照深度放入栈，然后弹出
3. 每弹出一个点，把该节点下一个没有进过栈的邻接点放入栈
4. 直到栈变空



进栈的时候打印

```java
public static void dfs (Node node) {
  if (node == null) {
    return;
  }
  Stack<Node> stack = new Stack<>();
  HashSet<Node> set = new HashSet<>();
  stack.add(node);
  set.add(node);
  System.out.println(node.value);
  while (!stack.isEmpty()) {
    Node cur = stack.pop();
    
    // 如果cur没有后代 自动跳过for
    for (Node next : cur.nexts) {
      if (!set.contains(next)) {
        stack.push(cur);
        stack.push(next);
        set.add(next);
        System.out.println(next.value);
        break;
      }
    }
  }
}
```



### 拓扑排序算法

图表示事件的依赖关系，先做哪个时间后做哪个事件。

#### 适用范围

有向图，入度为0的节点，且没有环

1. 找入度为0的点
2. 擦掉该点，和与其相关的边
3. goto 1

```java
// 有向无环图，directed graph and no loop
public static List<Node> sortedTopology(Graph graph) {
  // key: 某一个node
  // value 剩余的入度
  HashMap<Node, Integer> inMap = new HashMap<>();
  // 入度为0的点，才能进这个队列
  // 第一批入度为0的点，在这个队列里。
  Queue<Node> zeroInQueue = new LinkedList<>();
  for (Node node : graph.nodes.values()) {
    inMap.put(node, node.in);
    if (node.in==0) {
      zeroInQueue.add(node);
    }
  }
  // 拓扑排序结果，依次加入result
  List<Node> result = new ArrayList<>();
  while (!zeroInQueue.isEmpty()) {
    Node cur = zeroInQueue.poll();
    // 从零度队列里弹出一个点
    // 把它放到result中去
    result.add(cur);
    for (Node next : cur.nexts) {
      // 当前点所有的后代的入度值 减一
      inMap.put(next, inMap.get(next)-1);
      if (inMap.get(next) == 0) {
        zeroInQueue.add(next);
      }
    }
  }
}
```

最小生成树算法。

一张图，要求所有点必须连通。

### kruskal算法

#### 适用范围：

要求无向图

从最小的边考察，来到某边a时，考察a两头的点是否在一个集合里。

如果在不要这条边，

如果不在，则要，然后把两头连通（算在一个集合里）

初始：每点，在各自集合里。

```java
public static class Querry {
  // 某个描述多个小集合的结构
  public Querry(HashMap<Integer, Node> nodes) {
    // Querry 内部把每一个点放到，单独的小集合里去
  }
  
  // a点和b点是否属于一个集合
  public boolean isSameSet(Node a, Node b) {
    return true;
  }
  // 把a所在集合所有的点，和b所在集合所有的点，合并成一个大集合
  public void union(Node a, Node b) {
    
  }
}
```



```java
// 更快 但麻烦
// Union-Find set
public static class UnionFind {
  // key 某一个节点， value： key节点往上的节点
  private HashMap<Node, Node> fatherMap;
  // key某一个集合的代表节点， value：key所在集合的节点个数
  private HashMap<Node, Integer> sizeMap;
  
  public UnionFind() {
    fatherMap = new HashMap<Node, Node>();
    sizeMap = new HashMap<Node, Integer>();
  }
  
  public void makeSets(Collection<Node> nodes) {
    fatherMap.clear();
    sizeMap.clear();
    for (Node node : nodes) {
      fatherMap.put(node, node);
      sizeMap.put(node, 1);
    }
  }
  
  private Node findFather(Node n) {
    Stack<Node> path = new Stack<>();
    while (n!=fatherMap.get(n)) {
      path.add(n);
      n = fatherMap.get(n);
    }
    while (!path.isEmpty()) {
      fatherMap.put(path.pop(), n);
    }
    return n;
  }
  
  public boolean isSameSet(Node a, Node b) {
    return findFather(a) == findFather(b);
  }
  
  public void union (Node a, Node b) {
    if (a==null||b==null) {
      return;
    }
    Node aDai = findFather(a);
    Node bDai = findFather(b);
    if (aDai != bDai) {
				int aSetSize = sizeMap.get(aDai);
				int bSetSize = sizeMap.get(bDai);
				if (aSetSize <= bSetSize) {
					fatherMap.put(aDai, bDai);
					sizeMap.put(bDai, aSetSize + bSetSize);
					sizeMap.remove(aDai);
				} else {
					fatherMap.put(bDai, aDai);
					sizeMap.put(aDai, aSetSize + bSetSize);
					sizeMap.remove(bDai);
				}
			}
  }
  
}
```



```java
// 比较粗糙的实现
public static class MySets{
		public HashMap<Node, List<Node>>  setMap;
		public MySets(List<Node> nodes) {
			for(Node cur : nodes) {
				List<Node> set = new ArrayList<Node>();
				set.add(cur);
				setMap.put(cur, set);
			}
		}
		
		
		public boolean isSameSet(Node from, Node to) {
			List<Node> fromSet  = setMap.get(from);
			List<Node> toSet = setMap.get(to);
			return fromSet == toSet;
		}
		
		
		public void union(Node from, Node to) {
			List<Node> fromSet  = setMap.get(from);
			List<Node> toSet = setMap.get(to);
			for(Node toNode : toSet) {
				fromSet.add(toNode);
				setMap.put(toNode, fromSet);
			}
		}
	}
```



```java
	public static class EdgeComparator implements Comparator<Edge> {

		@Override
		public int compare(Edge o1, Edge o2) {
			return o1.weight - o2.weight;
		}

	}

	public static Set<Edge> kruskalMST(Graph graph) {
		UnionFind unionFind = new UnionFind();
		unionFind.makeSets(graph.nodes.values());
		PriorityQueue<Edge> priorityQueue = new PriorityQueue<>(new EdgeComparator());
		for (Edge edge : graph.edges) { // M 条边
			priorityQueue.add(edge);  // O(logM)
        }
        
		Set<Edge> result = new HashSet<>();
		while (!priorityQueue.isEmpty()) { // M 条边
			Edge edge = priorityQueue.poll(); // O(logM)
			if (!unionFind.isSameSet(edge.from, edge.to)) { // O(1)
				result.add(edge);
				unionFind.union(edge.from, edge.to);
			}
		}
		return result;
	}
```



```java

```



### Prim算法

#### 适用范围：

要求无向图

由一个点，解锁一票边，选这些边中最小的那一个，解锁新的点，再解锁一票新的边，再在解锁了的边中，找最小的边，以此类推

```java

```



```java

```

### Dijkstra算法

#### 适用范围：

没有权值为负数的边

```java
public class Dijkstra {
  public static HashMap<Node, Integer> dijkstra1(Node from) {
    // 此时从from出发到所有点的最小距离
    // key ： 从from出发到达key
    // value ： 从from出发到达key的最小距离
    // 如果在表中，没有T的记录，含义是从from出发到T这个点的距离为正无穷
    HashMap<Node, Integer> distanceMap = new HashMap<>();
    distanceMap.put(from, 0);
    // 已经求过距离的节点，存在selectedNodes中，以后不再碰
    HashSet<Node> selectedNodes = new HashSet<>();
    Node minNode = getMinDistanceAndUnselectedNode(distanceMap, selectedNodes);
    while (minNode != null) {
      // 此时选的最小记录， from...>minNode（distance）
      int distance = distanceMap.get(minNode);
      for (Edge edge : minNode.edges) {
        Node toNode = edge.to;
        if (!distanceMap.containsKey(toNode)) {
          distanceMap.put(toNode, distance+edge.weight);
        } else {
          distanceMap.put(edge.to, Math.min(distanceMap.get(toNode), distance+edge.weight));
        }
      }
      selectedNodes.add(minNode);
      minNode = getMinDistanceAndUnselectedNode(distanceMap, selectedNodes);
    }
    return distancesMap
  }
  
  public static Node getMinDistanceAndUnselectedNode(HashMap<Node, Integer> distanceMap, HashSet<Node> touchedNodes) {
		Node minNode = null;
		int minDistance = Integer.MAX_VALUE;
		for (Entry<Node, Integer> entry : distanceMap.entrySet()) {
			Node node = entry.getKey();
			int distance = entry.getValue();
			if (!touchedNodes.contains(node) && distance < minDistance) {
				minNode = node;
				minDistance = distance;
			}
		}
		return minNode;
	}
}
```



```java

```



```java

```

