---
title: Scrape for cod intv
layout: post
tags: [article, patterns]
date: 2024-10-14
---

# Phone Screen Interview Questions

## Detect Visits from User Snaps

Suppose that once per minute, a user is "snapped" to the nearest place. We want to detect visits from these snaps. Rules:
- Use a 4 minute sliding window
- If 3 out of last 4 snaps are the same ID, a visit begins
- A visit ends when 3 out of 4 are NOT the current ID

Examples:
- [A A B A] => [A]
- [A A A A A A A A] => [A]
- [A A B B A A] => []
- [A A A B C B A A A] => [A A]
- [A A A B C B B A B] => [A B]

## Common

Certainly! I'll go through each problem one by one, providing an optimal TypeScript solution and explaining the key points in a simple, kid-friendly manner.

1. Count number of Islands

```typescript
function numIslands(grid: string[][]): number {
    let count = 0;
    
    function dfs(i: number, j: number) {
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length || grid[i][j] === '0') {
            return;
        }
        grid[i][j] = '0';
        dfs(i+1, j);
        dfs(i-1, j);
        dfs(i, j+1);
        dfs(i, j-1);
    }
    
    for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[0].length; j++) {
            if (grid[i][j] === '1') {
                count++;
                dfs(i, j);
            }
        }
    }
    
    return count;
}
```

Explanation for a kid:
Imagine you're playing a game where you have a map made of squares. Some squares are land ('1') and some are water ('0'). Your job is to count how many islands there are.

The trick is:
1. Look at each square on the map.
2. If you find land, you've found an island! Count it.
3. Then, explore all the connected land around it (up, down, left, right) and mark it as visited so you don't count it again.
4. Keep doing this until you've looked at every square.

The number of times you find new land is the number of islands!

2. Sum and product of nodes in the n-ary tree

```typescript
class TreeNode {
    val: number;
    children: TreeNode[];
    constructor(val?: number) {
        this.val = (val===undefined ? 0 : val);
        this.children = [];
    }
}

function sumAndProductOfTree(root: TreeNode | null): [number, number] {
    if (!root) return [0, 1];
    
    let sum = root.val;
    let product = root.val;
    
    for (let child of root.children) {
        const [childSum, childProduct] = sumAndProductOfTree(child);
        sum += childSum;
        product *= childProduct;
    }
    
    return [sum, product];
}
```

Explanation for a kid:
Imagine you have a family tree, but instead of people, it's numbers. Your job is to add up all the numbers (sum) and multiply all the numbers (product).

The trick is:
1. Start at the top of the tree.
2. Look at the number there and remember it.
3. Then, for each branch below, do the same thing.
4. When you come back up, add the number you remembered to all the sums from below, and multiply the number you remembered by all the products from below.
5. Keep doing this until you've looked at every number in the tree.

3. Generate k random options from a list of n

```typescript
function getRandomOptions<T>(options: T[], k: number): T[] {
    const result: T[] = [];
    const n = options.length;
    
    for (let i = 0; i < Math.min(k, n); i++) {
        const randomIndex = i + Math.floor(Math.random() * (n - i));
        [options[i], options[randomIndex]] = [options[randomIndex], options[i]];
        result.push(options[i]);
    }
    
    return result;
}
```

Explanation for a kid:
Imagine you have a bag of marbles and you want to pick a few randomly.

The trick is:
1. Shake the bag to mix up the marbles.
2. Pick a marble without looking.
3. Put that marble in a special pile.
4. Keep doing this until you have enough marbles or the bag is empty.

This way, every marble has a fair chance of being picked!

4. Implement a simple tree traversal solution

```typescript
class TreeNode {
    val: number;
    left: TreeNode | null;
    right: TreeNode | null;
    constructor(val?: number, left?: TreeNode | null, right?: TreeNode | null) {
        this.val = (val===undefined ? 0 : val);
        this.left = (left===undefined ? null : left);
        this.right = (right===undefined ? null : right);
    }
}

function inorderTraversal(root: TreeNode | null): number[] {
    const result: number[] = [];
    
    function traverse(node: TreeNode | null) {
        if (node) {
            traverse(node.left);
            result.push(node.val);
            traverse(node.right);
        }
    }
    
    traverse(root);
    return result;
}
```

Explanation for a kid:
Imagine you're exploring a tree house. Each room in the tree house has a number, and can have up to two more rooms connected to it.

The trick is:
1. Start at the entrance (root).
2. Always go to the left room first if there is one.
3. Write down the number of the room you're in.
4. Then go to the right room if there is one.
5. Keep doing this until you've visited every room.

The order you write down the numbers is called an "inorder traversal"!

I'll continue with the remaining problems in the next response.



5. Sum a list, product of a list, sum of elements of a tree, product of elements in a tree, generalize a tree structure to handle any user defined function

```typescript
class TreeNode {
    val: number;
    children: TreeNode[];
    constructor(val?: number) {
        this.val = (val===undefined ? 0 : val);
        this.children = [];
    }
}

function sumList(list: number[]): number {
    return list.reduce((sum, num) => sum + num, 0);
}

function productList(list: number[]): number {
    return list.reduce((product, num) => product * num, 1);
}

function treeOperation(root: TreeNode | null, operation: (a: number, b: number) => number, initialValue: number): number {
    if (!root) return initialValue;
    
    let result = root.val;
    for (let child of root.children) {
        result = operation(result, treeOperation(child, operation, initialValue));
    }
    
    return result;
}

// Usage:
const sumTree = (root: TreeNode | null) => treeOperation(root, (a, b) => a + b, 0);
const productTree = (root: TreeNode | null) => treeOperation(root, (a, b) => a * b, 1);
```
    