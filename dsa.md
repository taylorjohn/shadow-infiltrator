
# Advanced Rust Algorithms and Data Structures

This document provides implementations of various algorithms and data structures in Rust, organized by category. These implementations are meant to serve as a reference and starting point for understanding how these concepts can be implemented in Rust.

## Table of Contents

1. [Array and String Manipulation](#array-and-string-manipulation)
2. [Searching and Sorting](#searching-and-sorting)
3. [Dynamic Programming](#dynamic-programming)
4. [Graph Algorithms](#graph-algorithms)
5. [Tree Algorithms](#tree-algorithms)
6. [Advanced Data Structures](#advanced-data-structures)

## Array and String Manipulation

### Two Pointers Technique

```rust
fn two_sum(nums: Vec<i32>, target: i32) -> Option<(usize, usize)> {
    let mut left = 0;
    let mut right = nums.len() - 1;
    
    while left < right {
        let sum = nums[left] + nums[right];
        if sum == target {
            return Some((left, right));
        } else if sum < target {
            left += 1;
        } else {
            right -= 1;
        }
    }
    
    None
}
```

### Sliding Window

```rust
fn max_sum_subarray(nums: &[i32], k: usize) -> i32 {
    if nums.len() < k {
        return 0;
    }
    
    let mut window_sum: i32 = nums[..k].iter().sum();
    let mut max_sum = window_sum;
    
    for i in k..nums.len() {
        window_sum = window_sum - nums[i - k] + nums[i];
        max_sum = max_sum.max(window_sum);
    }
    
    max_sum
}
```

### Kadane's Algorithm

```rust
fn max_subarray_sum(nums: &[i32]) -> i32 {
    let mut max_sum = nums[0];
    let mut current_sum = nums[0];

    for &num in nums.iter().skip(1) {
        current_sum = num.max(current_sum + num);
        max_sum = max_sum.max(current_sum);
    }

    max_sum
}
```

## Searching and Sorting

### Binary Search

```rust
fn binary_search<T: Ord>(arr: &[T], target: &T) -> Option<usize> {
    let mut left = 0;
    let mut right = arr.len();

    while left < right {
        let mid = left + (right - left) / 2;
        if &arr[mid] < target {
            left = mid + 1;
        } else if &arr[mid] > target {
            right = mid;
        } else {
            return Some(mid);
        }
    }

    None
}
```

### Quick Sort

```rust
fn quick_sort<T: Ord>(arr: &mut [T]) {
    if arr.len() <= 1 {
        return;
    }
    
    let pivot = partition(arr);
    quick_sort(&mut arr[0..pivot]);
    quick_sort(&mut arr[pivot + 1..]);
}

fn partition<T: Ord>(arr: &mut [T]) -> usize {
    let pivot = arr.len() - 1;
    let mut i = 0;
    
    for j in 0..pivot {
        if arr[j] <= arr[pivot] {
            arr.swap(i, j);
            i += 1;
        }
    }
    
    arr.swap(i, pivot);
    i
}
```

## Dynamic Programming

### Fibonacci Sequence

```rust
fn fibonacci(n: usize) -> u64 {
    if n <= 1 {
        return n as u64;
    }
    
    let mut dp = vec![0; n + 1];
    dp[1] = 1;
    
    for i in 2..=n {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    
    dp[n]
}
```

### 0/1 Knapsack Problem

```rust
fn knapsack(weights: &[i32], values: &[i32], capacity: i32) -> i32 {
    let n = weights.len();
    let mut dp = vec![vec![0; (capacity + 1) as usize]; n + 1];

    for i in 1..=n {
        for w in 0..=capacity {
            if weights[i - 1] <= w {
                dp[i][w as usize] = dp[i - 1][w as usize]
                    .max(values[i - 1] + dp[i - 1][(w - weights[i - 1]) as usize]);
            } else {
                dp[i][w as usize] = dp[i - 1][w as usize];
            }
        }
    }

    dp[n][capacity as usize]
}
```

### Longest Common Subsequence

```rust
fn longest_common_subsequence(text1: &str, text2: &str) -> i32 {
    let (m, n) = (text1.len(), text2.len());
    let (text1, text2) = (text1.as_bytes(), text2.as_bytes());
    let mut dp = vec![vec![0; n + 1]; m + 1];

    for i in 1..=m {
        for j in 1..=n {
            if text1[i - 1] == text2[j - 1] {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = dp[i - 1][j].max(dp[i][j - 1]);
            }
        }
    }

    dp[m][n]
}
```

## Graph Algorithms

### Depth-First Search (DFS)

```rust
use std::collections::HashMap;

fn dfs(graph: &HashMap<i32, Vec<i32>>, start: i32, visited: &mut Vec<bool>) {
    if visited[start as usize] {
        return;
    }
    
    visited[start as usize] = true;
    println!("Visited node: {}", start);
    
    if let Some(neighbors) = graph.get(&start) {
        for &neighbor in neighbors {
            dfs(graph, neighbor, visited);
        }
    }
}
```

### Breadth-First Search (BFS)

```rust
use std::collections::{HashMap, VecDeque};

fn bfs(graph: &HashMap<i32, Vec<i32>>, start: i32) {
    let mut visited = vec![false; graph.len()];
    let mut queue = VecDeque::new();
    
    visited[start as usize] = true;
    queue.push_back(start);
    
    while let Some(node) = queue.pop_front() {
        println!("Visited node: {}", node);
        
        if let Some(neighbors) = graph.get(&node) {
            for &neighbor in neighbors {
                if !visited[neighbor as usize] {
                    visited[neighbor as usize] = true;
                    queue.push_back(neighbor);
                }
            }
        }
    }
}
```

### Dijkstra's Algorithm

```rust
use std::collections::{BinaryHeap, HashMap};
use std::cmp::Reverse;

fn dijkstra(graph: &HashMap<usize, Vec<(usize, i32)>>, start: usize) -> HashMap<usize, i32> {
    let mut distances = HashMap::new();
    let mut heap = BinaryHeap::new();

    distances.insert(start, 0);
    heap.push(Reverse((0, start)));

    while let Some(Reverse((dist, node))) = heap.pop() {
        if dist > *distances.get(&node).unwrap_or(&std::i32::MAX) {
            continue;
        }

        if let Some(neighbors) = graph.get(&node) {
            for &(next_node, weight) in neighbors {
                let next_dist = dist + weight;
                if next_dist < *distances.get(&next_node).unwrap_or(&std::i32::MAX) {
                    distances.insert(next_node, next_dist);
                    heap.push(Reverse((next_dist, next_node)));
                }
            }
        }
    }

    distances
}
```

### Topological Sort

```rust
use std::collections::{HashMap, HashSet};

fn topological_sort(graph: &HashMap<i32, Vec<i32>>) -> Vec<i32> {
    let mut visited = HashSet::new();
    let mut stack = Vec::new();

    for &node in graph.keys() {
        if !visited.contains(&node) {
            dfs_topological(graph, node, &mut visited, &mut stack);
        }
    }

    stack.into_iter().rev().collect()
}

fn dfs_topological(graph: &HashMap<i32, Vec<i32>>, node: i32, visited: &mut HashSet<i32>, stack: &mut Vec<i32>) {
    visited.insert(node);

    if let Some(neighbors) = graph.get(&node) {
        for &neighbor in neighbors {
            if !visited.contains(&neighbor) {
                dfs_topological(graph, neighbor, visited, stack);
            }
        }
    }

    stack.push(node);
}
```

## Tree Algorithms

### Trie (Prefix Tree)

```rust
use std::collections::HashMap;

struct TrieNode {
    children: HashMap<char, TrieNode>,
    is_end: bool,
}

impl TrieNode {
    fn new() -> Self {
        TrieNode {
            children: HashMap::new(),
            is_end: false,
        }
    }
}

struct Trie {
    root: TrieNode,
}

impl Trie {
    fn new() -> Self {
        Trie { root: TrieNode::new() }
    }

    fn insert(&mut self, word: String) {
        let mut node = &mut self.root;
        for ch in word.chars() {
            node = node.children.entry(ch).or_insert(TrieNode::new());
        }
        node.is_end = true;
    }

    fn search(&self, word: String) -> bool {
        let mut node = &self.root;
        for ch in word.chars() {
            if let Some(next_node) = node.children.get(&ch) {
                node = next_node;
            } else {
                return false;
            }
        }
        node.is_end
    }

    fn starts_with(&self, prefix: String) -> bool {
        let mut node = &self.root;
        for ch in prefix.chars() {
            if let Some(next_node) = node.children.get(&ch) {
                node = next_node;
            } else {
                return false;
            }
        }
        true
    }
}
```

## Advanced Data Structures

### Union-Find (Disjoint Set)

```rust
struct UnionFind {
    parent: Vec<usize>,
    rank: Vec<usize>,
}

impl UnionFind {
    fn new(size: usize) -> Self {
        UnionFind {
            parent: (0..size).collect(),
            rank: vec![0; size],
        }
    }

    fn find(&mut self, x: usize) -> usize {
        if self.parent[x] != x {
            self.parent[x] = self.find(self.parent[x]);
        }
        self.parent[x]
    }

    fn union(&mut self, x: usize, y: usize) -> bool {
        let root_x = self.find(x);
        let root_y = self.find(y);

        if root_x == root_y {
            return false;
        }

        if self.rank[root_x] < self.rank[root_y] {
            self.parent[root_x] = root_y;
        } else if self.rank[root_x] > self.rank[root_y] {
            self.parent[root_y] = root_x;
        } else {
            self.parent[root_y] = root_x;
            self.rank[root_x] += 1;
        }

        true
    }
}
```

### Segment Tree

```rust
struct SegmentTree {
    tree: Vec<i32>,
    n: usize,
}

impl SegmentTree {
    fn new(arr: &[i32]) -> Self {
        let n = arr.len();
        let mut tree = vec![0; 4 * n];
        Self::build_tree(arr, &mut tree, 0, 0, n - 1);
        SegmentTree { tree, n }
    }

    fn build_tree(arr: &[i32], tree: &mut Vec<i32>, node: usize, start: usize, end: usize) {
        if start == end {
            tree[node] = arr[start];
            return;
        }
        let mid = (start + end) / 2;
        Self::build_tree(arr, tree, 2 * node + 1, start, mid);
        Self::build_tree(arr, tree, 2 * node + 2, mid + 1, end);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    fn update(&mut self, index: usize, value: i32) {
        self.update_tree(0, 0, self.n - 1, index, value);
    }

    fn update_tree(&mut self, node: usize, start: usize, end: usize, index: usize, value: i32) {
        if start == end {
            self.tree[node] = value;
            return;
        }
        let mid = (start + end) / 2;
        if index <= mid {
            self.update_tree(2 * node + 1, start, mid, index, value);
        } else {
            self.update_tree(2 * node + 2, mid + 1, end, index, value);
        }
        self.tree[node] = self.tree[2 * node + 1] + self.tree[2 * node + 2];
    }

    fn query(&self, left: usize, right: usize) -> i32 {
        self.query_tree(0, 0, self.n - 1, left, right)
    }

    fn query_tree(&self, node: usize, start: usize, end: usize, left: usize, right: usize) -> i32 {
        if left > end || right < start {
            return 0;
        }
        if left <= start && end <= right {
            return self.tree[node];
        }
        let mid = (start + end) / 2;
        let left_sum = self.query_tree(2 * node + 1, start, mid, left, right);
        let right_sum = self.query_tree(2 * node + 2, mid + 1, end, left, right);
        left_sum + right_sum
    }
}