# leetcode-solutions

## 1448. Count Good Nodes in Binary Tree

[Problem link](https://leetcode.com/problems/count-good-nodes-in-binary-tree)

```js
const countGoodNodes = (node, maxNumFromNode) => {
  let numOfGoodNodes = 0;
  if (node) {
    if (node.val >= maxNumFromNode) {
      numOfGoodNodes++;
      maxNumFromNode = node.val;
    }
    numOfGoodNodes += countGoodNodes(node.left, maxNumFromNode);
    numOfGoodNodes += countGoodNodes(node.right, maxNumFromNode);
  }
  return numOfGoodNodes;
}

const goodNodes = (root) => {
  return countGoodNodes(root, root.val);
}
```

Time Complexity = O(N)
Space Complexity = O(N)
