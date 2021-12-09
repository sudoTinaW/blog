---
published: true
layout: post
date: '2020-10-20 14:50:00 -0000'
categories: BFS
---
BFS is often used to find shortest path on trees or graphs.

#### BFS Usage:

- It can be used to get a shortest path between a start and an end node in a simple graph (weight = 1 and no directions).

- It can be used to traverse a graph.
- It can be used in topological sort (weight = 1 with directions).

#### BFS Template:

We can implement BFS with queues in 3 different ways. 

- We can use 2 queues to store one level of nodes and its next level of nodes. 
- We can use one queue with a marker stored in the queue to store and separate 2 levels of nodes.
- We can use one queue with a marker stored outside the queue to store and separate 2 levels of nodes . 

Here we will use one queue with a maker outside the queue. Every time, before we add any new element belonging to the next level, we will save the current level's size. When we finish visiting the size of nodes in the queue, the rest of elements in queue will be the next level's elements. 

[labuladong](https://labuladong.gitbook.io/algo/di-ling-zhang-bi-du-xi-lie/bfs-kuang-jia) has offer a template. The template is a while (loop the queue) + a for (loop level by level only when we need level's information) + a for (loop all neighbors of a current level node)

- BFS template layer's loop is not necessary all the time. If our goal is visited all nodes in a graph, we can ignore layer's for loop.
- If The graph doesn't have a cycle, like a tree, we don't need to maintain the visited set even.
- We need to mark a node as visited when it is pushed into queue rather than when it is popped from the queue. That is because if we marked as visited when it is pushed into the queue, it will be only pushed and popped once. If we marked as visited when it is popped, it can be pushed multiple times already and waste memory. Every time, when we pop a node, we will push multiple nodes. Mark visit when push will skip more duplicated push operation.
- When we need to collect a node's value, we can collect either when it is pushed or popped. However, if we collect the data when it is pushed, we need to handle the first node as the special case. However, if we collect the node's value at pop operation, we won't have any special case. What is more, as above step, we have already make sure each node will be pushed and popped from the queue once, time complexity won't be influenced.

```java
// 计算从起点 start 到终点 target 的最近距离
int bfs(Node start, Node target) {
    Queue<Node> q; // 核心数据结构
    Set<Node> visited; // 避免走回头路

    q.offer(start); // 将起点加入队列
    visited.add(start);
    int step = 0; // 记录扩散的步数

    while (q not empty) {
        int sz = q.size();
        /* 将当前队列中的所有节点向四周扩散 */
        for (int i = 0; i < sz; i++) {
            Node cur = q.poll();
            /* 划重点：这里判断是否到达终点 */
            if (cur is target)
                return step;
            /* 将 cur 的相邻节点加入队列 */
            for (Node x : cur.adj())
                if (x not in visited) {
                    q.offer(x);
                    visited.add(x);
                }
        }
        /* 划重点：更新步数在这里 */
        step++;
    }
}
```

BFS usually use queue not stack, so that the members in the same level will be visited as the original order. If we are using stack, the order of members in the same level will be in reverse.

BFS **time complexity** will be O(n) and space will be O(n). Even though, we have multiple loops while + for. The worst of case of multiple loops won't happen at the same time. What is more, if you think how many times each node will be visited, it will be O(m + n). Therefore, the time complexity is O(n), but not O(n ^2) or O(n ^3).

#### Topological Sort

Topological Sort is used to offering a sort of elements having dependencies between each other.

-  A graph can have multiple topological sort. If a graph have a topological sort, it won't have cycles. 2 of its most famous usage are course selection and compiler compiling objects with dependencies. 
-  It can be implemented by DFS and BFS. DFS needs to use the program stack which are not allowed to be very deep. Therefore, it is not recommended. DFS cares about the outcoming degree of each node. Every round of calculation will start from any node with 0 outcoming degree. 
-  BFS is more often used. BFS cares about the incoming degrees. Every round of calculation will start from any node with 0 incoming degree. 
-  Both DFS and BFS time complexity will be O(m + n).

#### Bidirectional BFS

Some BFS questions have an optimization, which is bidirectional BFS. Bidirectional BFS will spread both from start and end point until their visited points meet. 

- Bidirectional BFS time complexity is also `O(n)` in general, but if the n is big, its time complexity can decrease to nearly `O(sqrt(n))`. The picture, from [labuladong](https://labuladong.gitbook.io/algo/di-ling-zhang-bi-du-xi-lie-qing-an-shun-xu-yue-du/bfs-kuang-jiashows), shows how bidirectional BFS can save time.

![img](https://gblobscdn.gitbook.com/assets%2F-M_pUcR-oaySVbYadzDh%2Fsync%2F96d7b1ab7db024a1831170c128f5dcacfb68abb8.jpeg?alt=media)

![img](https://gblobscdn.gitbook.com/assets%2F-M_pUcR-oaySVbYadzDh%2Fsync%2F946f50b8251df56bfaa1d60a133affd736e4ebb3.jpeg?alt=media)

- Bidirectional BFS can only be used in the question that we both know start and end.

- Bidirectional BFS template : Create 2 queues start queue and end queue. As long as 2 queues don't intersects with each other(visited set1 doesn't contain any element from visited set2), we will continue to move the pointer current queue to the shorter queue.

  ```java
  int bfs(Node start, Node target) {
      //Since bi directional search starts from its neighbors, it doesn't consider the case start is the target. You need to consider the conner case first.
      if(start == target) {
          return 0;
      }
      Queue<Node> q1;
      Queue<Node> q2;
      
      Set<Node> visited1;
      Set<Node> visited2;
      
      QueueCur<Node> qCur;
      Set<Node> visitedCur;
      Set<Node> visitedOp;
      
  
      q1.offer(start); 
      visited1.add(start);
      q2.offer(end);
      visited2.add(end);
      
      int step = 0; 
  
      while (q1 not empty && q2 not empty) {
          //Dynamically choose the shorter queue to visit
          if(q1.size() < q2.size()) {
              qCur = q1;
              visitedCur = visited1;
              visitedOp = visited2;
          }else {
              qCur = q2;
              visitedCur = visited2;
              visitedOp = visited1;
          }
          int sz = qCur.size();
          
          for (int i = 0; i < sz; i++) {
              Node cur = q.poll();
              
              for (Node x : cur.adj()) {
                  //Quit condition: there is an element intersiting with the other side of queue
                  if (x is in visitedOp) {
                      return step;
                  }
              	if(x is not in visitedCur) {
                      qCur.offer(x);
                      visitedCur.add(x);
                  }
          }
          
          step++;
      }
  }
  ```

  

### Problems

### Traverse a Graph:

#### [69. Binary Tree Level Order Traversal](https://www.lintcode.com/problem/binary-tree-level-order-traversal/description)

#### [71. Binary Tree Zigzag Level Order Traversal](https://www.lintcode.com/problem/binary-tree-zigzag-level-order-traversal/description)

#### [242. Convert Binary Tree to Linked Lists by Depth](https://www.lintcode.com/problem/convert-binary-tree-to-linked-lists-by-depth/description)

#### [(LeetCode)117.Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)

#### [137. Clone Graph](https://www.lintcode.com/problem/clone-graph/description)

#### [618. Search Graph Nodes](https://www.lintcode.com/problem/search-graph-nodes/description)

####  [431. Connected Component in Undirected Graph](https://www.lintcode.com/problem/connected-component-in-undirected-graph/description)

#### [178. Graph Valid Tree](https://www.lintcode.com/problem/178/)

####  [127. Topological Sorting](https://www.lintcode.com/problem/topological-sorting/description)

### Shortest Path

#### [814. Shortest Path in Undirected Graph](https://www.lintcode.com/problem/shortest-path-in-undirected-graph/solution)

#### [120. Word Ladder](https://www.lintcode.com/problem/word-ladder/my-submissions)

#### [796. Open the Lock](https://www.lintcode.com/problem/open-the-lock/solution)

#### [794. Sliding Puzzle II](https://www.lintcode.com/problem/sliding-puzzle-ii/description)

### BFS in Matrix

A graph can be represented in 2 ways. 

- Map: (node, list of direct neighbors)
- 0 and 1Matrix: If any 2 nodes are connected, the value of indexes with 2 nodes will be 1, otherwise, it will be 0.

#### [433. Number of Islands](https://www.lintcode.com/problem/number-of-islands/solution)

#### [611. Knight Shortest Path](https://www.lintcode.com/problem/knight-shortest-path/description)

#### [630. Knight Shortest Path II](https://www.lintcode.com/problem/knight-shortest-path-ii/description)

#### [598. Zombie in Matrix](https://www.lintcode.com/problem/zombie-in-matrix/description)

