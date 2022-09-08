# leetcode-solutions

## 4. Median of Two Sorted Arrays

[Problem link](https://leetcode.com/problems/median-of-two-sorted-arrays)

```js
const findMedianSortedArrays = function(nums1, nums2) {
  const len = nums1.length + nums2.length;
  
  const getNextNumber = () => {
    let nextNumber;
    
    if (nums1.length === 0) {
      nextNumber = nums2.shift();
    } else if (nums2.length ===0) {
      nextNumber = nums1.shift();
    } else if (nums1[0] < nums2[0]) {
      nextNumber = nums1.shift();
    } else {
      nextNumber = nums2.shift();
    }
    return nextNumber;
  }
  
  let smallNumber;
  
  for (let i = 0; i < Math.floor(len / 2); i++) {
    smallNumber = getNextNumber();
  }
  
  let nextNumber = getNextNumber();
  
  return len % 2 === 1 ? nextNumber : (nextNumber + smallNumber) / 2
};
```

Time Complexity = O(M + N)
Space Complexity = O(1)

## 5. Longest Palindromic Substring

[Problem link](https://leetcode.com/problems/longest-palindromic-substring)

```js
const expandFromCentre = (s, left, right) => {
  while (left >= 0 && right < s.length && s[left] === s[right]) {
    left--;
    right++;
  }
  return [left + 1, right];
}

const longestPalindrome = (s) => {
  if (s.length < 1) return "";
  const maxIndices = [...s].reduce((prev, _, i) => {
    let [l1, r1] = expandFromCentre(s, i, i);
    let [l2, r2] = expandFromCentre(s, i, i + 1);
    let max = r1 - l1 > r2 - l2 ?  [l1, r1] : [l2, r2];
    return max[1] - max[0] > prev[1] - prev[0] ? max : prev
  }, [0, 0])

  return s.slice(...maxIndices);
}
```

## 6. Zigzag Conversion

[Problem link](https://leetcode.com/problems/zigzag-conversion)

```js
const convert = function(s, numRows) {
  if (numRows === 1) return s;
  const arrS = s.split("");
  const mapRows = new Map();
  
  let direction = 1;
  let i = 0;
  
  while (arrS.length !== 0) {
    const char = arrS.shift();        
    if (mapRows.has(i)) {
      mapRows.get(i).push(char);
    } else {
      mapRows.set(i, [char]);
    }        
    i += direction;
    if (i === 0 || i === numRows - 1) {
      direction = 0 - direction;
    }
  }
  
  let res = "";
  
  for (let i = 0; i < numRows; i++) {
    if (mapRows.has(i)) res += mapRows.get(i).join("");
  }
  return res
};
```

## 7. Reverse Integer

[Problem link](https://leetcode.com/problems/reverse-integer)

```js
const reverse = function(x) {
  let res = parseInt(x.toString().split("").reverse().join(""));
  if (res > 2**31 - 1) return 0;
  return x > 0 ? res : -1 * res;
};
```

## 42. Trapping Rain Water

[Problem link](https://leetcode.com/problems/trapping-rain-water)

### Two Pointer

```js
const trap = function(height) {
  let capacity = 0;
  let left_max = 0;
  let right_max = 0;
  let left = 0;
  let right = height.length - 1;
  while (right > left) {
    if (height[left] < height[right]) {
      height[left] >= left_max ? left_max = height[left] : capacity += (left_max - height[left])
      left++;
    } else {
      height[right] >= right_max ? right_max = height[right] : capacity += (right_max - height[right])
      right--;
    }
  }
  return capacity;
};
```

### Dynamic Programming

```js
const trap = (height) => {
  let size = height.length;
  if(size === 0) return 0;
  let left = Array(size).fill(0);
  let right = Array(size).fill(0);
  left[0] = height[0];
  right[size - 1] = height[size - 1];
  for (let i = 1; i < size; i++) {
    left[i] = Math.max(height[i], left[i - 1]);
  }
  for (let i = size - 2; i >= 0; i--) {
    right[i] = Math.max(height[i], right[i + 1]);
  }
  let res = 0;
  for (let i = 0; i < size ; i++) {
    res += Math.min(left[i], right[i]) - height[i];
  }
  return res;
};
```

## 56. Merge Intervals

[Problem link](https://leetcode.com/problems/merge-intervals)

```js
const merge = (intervals) => {
  intervals.sort((a, b) => a[0] - b[0])

  const res = [intervals[0]];
  for (let i = 1; i < intervals.length; i++){
    const last = res[res.length - 1];
    const curr = intervals[i];
    curr[0] > last[1] ? res.push(curr) : last[1] = Math.max(curr[1], last[1])
  }
  return res
};
```

## 94. Binary Tree Inorder Traversal

[Problem link](https://leetcode.com/problems/binary-tree-inorder-traversal)

```js
var inorderTraversal = function(root, ans = []) {
  if (!root) return ans;
  
  inorderTraversal(root.left, ans);
  ans.push(root.val);
  inorderTraversal(root.right, ans);
  
  return ans;
};
```

Time Complexity - O(N);
Space Complexity - O(1);

## 200. Number of Islands

[Problem link](https://leetcode.com/problems/number-of-islands)

```js
const dfs = (grid, row, col) => {
  if (row < 0 || row >= grid.length || col < 0 || col >= grid[0].length) {
      return
  }
  if (grid[row][col] === "0") return;
  
  grid[row][col] = "0";
  [[0, 1], [0, -1], [1, 0], [-1, 0]].map(([x, y]) => {
    dfs(grid, row + x, col + y)
  })
}

const numIslands = function(grid) {
  let numIslands = 0;
  for (let row = 0; row < grid.length; row++) {
    for (let col = 0; col < grid[0].length; col++) {
      if (grid[row][col] === "1") {
        numIslands++;
        dfs(grid, row, col);
      }
    }
  }
  return numIslands;
};
```

## 298. Binary Tree Longest Consecutive Sequence

[Problem link](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence)

```js
const longestConsecutive = function(root) {
    
  const traverse = (root, prev, length) => {
    if (!root) return length;
    length = root.val - prev === 1 ? length + 1 : 1;
    return Math.max(length, traverse(root.left, root.val, length), traverse(root.right,root.val, length));
  }
  return traverse(root, root.val, 1);
};
```

## 429 N-ary Tree Level Order Traversal

[Problem link](https://leetcode.com/problems/n-ary-tree-level-order-traversal)

```js
const levelOrder = function(root) {
  if (!root) return [];
  const queue = [];
  const res = [];
  queue.push(root);
  while (queue.length > 0) {
    const length = queue.length;
    const thisRes = [];
    for (let i=0; i<length; i++) {
      const node = queue.shift();
      thisRes.push(node.val);
      for (let child of node.children) {
        queue.push(child);
      }
    }
    res.push(thisRes);
  }
  return res;
};
```

## 606. Construct String from Binary Tree

[Problem link](https://leetcode.com/problems/construct-string-from-binary-tree)

```js
var tree2str = function(root) {
  if (!root) return '';
  const left2str = tree2str(root.left);
  const right2str = tree2str(root.right);
  
  let tail = '';
  if (right2str !== '') {
    tail = `(${left2str})(${right2str})`
  } else if (left2str !== '') {
    tail = `(${left2str})`
  } 
  return String(root.val) + tail;
};
```

## 814. Binary Tree Pruning

[Problem link](https://leetcode.com/problems/binary-tree-pruning)

```js
const pruneTree = function(root) {
    
  const hasZero = (root) => {
    if (!root) return true;
    if (root.val === 0 && hasZero(root.left) && hasZero(root.right)) return true;
    if (hasZero(root.left)) root.left = null;
    if (hasZero(root.right)) root.right = null;
  }
  return hasZero(root) ? null : root;
};
```

## 967. Numbers With Same Consecutive Differences

[Problem link](https://leetcode.com/problems/numbers-with-same-consecutive-differences)

```js
const numsSameConsecDiff = function(n, k) {
  let numbers = [];
  
  const DFS = (num) => {
    if (String(num).length === n) {
      numbers.push(num);
      return
    }
    const lastDigit = num % 10;
    if (k === 0) {
      DFS(num* 10 + lastDigit);
    } else {
      lastDigit - k >= 0 && DFS(num* 10 + lastDigit - k);
      lastDigit + k < 10 && DFS(num* 10 + lastDigit + k);
    }
  }
  
  for (let i = 1; i < 10; i++) {
    DFS(i);
  }
  
  return numbers;
};
```

## 987. Vertical Order Traversal of a Binary Tree

[Problem link](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree)

```js
const verticalTraversal = function(root) {
  const columns = new Map();
  
  const traverse = (root, column, row) => {
    if (!root) return;
    if (columns.has(column)) {
      columns.get(column).push([row, root.val])
    } else {
      columns.set(column, [[row, root.val]])
    }
    traverse(root.left, column - 1, row + 1);
    traverse(root.right, column + 1, row + 1);
  }
  traverse(root, 0, 0);
  
  const result = [];
  
  const minCol = Math.min(...columns.keys());
  const maxCol = Math.max(...columns.keys());
  
  const sortFunction = (a, b) => {
    if (a[0] != b[0]) return a[0] - b[0];
    return a[1] - b[1];
  }
  
  for (let i = minCol; i <= maxCol; i++) {
    const column = columns.get(i);
    column.sort(sortFunction);
    const currentCol = [];
    for (let [row, val] of column) {
      currentCol.push(val);
    }
    result.push(currentCol);
  }
  return result;
};
```

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
