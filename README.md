# leetcode-solutions

## 4. Median of Two Sorted Arrays

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
