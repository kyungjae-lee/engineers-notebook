<a href="../../">Home</a> > <a href="../notebook">Notebook</a> > <a href="./">Data Structures & Algorithms</a> > Breadth-First Search (BFS)

# Breadth-First Search (BFS)



## Breadth-First Search (BFS)

* The BFS algorithm systematically explores a graph or tree **level-by-level**, ensuring that nodes at the same depth are visited before moving on to deeper levels.
* This property makes BFS particularly useful for finding the shortest path between nodes in unweighted graphs or to explore all nodes in a connected component of a graph.

### Algorithm

1. Initialize a queue (FIFO) data structure to keep track of nodes to be visited. Enqueue the starting node into the queue.
2. Initialize a set or array to keep track of visited nodes to avoid revisiting them. Mark the starting node as visited.
3. While the queue is not empty: 
   1. Dequeue a node from the front of the queue.
   2. Process the node (print its value, perform some operation, etc.)
   3. Enqueue all unvisited neighbors of the current node into the queue and mark them as visited.

4. Continue the process until the queue becomes empty, meaning all nodes have been visited.

### Pseudo-code

```plain
BFS(Graph, startNode)
	queue = new Queue()
	visited = new Set()
	
	queue.euqueue(startNode)
	visited.add(startNode)
	
	while queue is not empty:
		currentNode = queue.dequeue()
		process(currentNode)	// Process the node (e.g., print its value)
		
		for each neighbor in neighbors of currentNode:
			if neighbor is not in visited:
				queue.enqueue(neighbor)
				visited.add(neighbor)
```

> In this algorithm,
>
> * `Graph` represents the graph or data structure you're traversing
> * `startNode` is the initial node from which you want to start the traversal
> * `neighbors of currentNode` refers to the neighboring nodes of the current node that are connected by edges. 



## Code (C++)

```cpp

```

```plain

```



## Code (C)

```c

```

```plain

```

