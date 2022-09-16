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

## 8. String to Integer (atoi)

[Problem link (MEDIUM)](https://leetcode.com/problems/string-to-integer-atoi)

```js
var myAtoi = function(s) {
  let num = Number.isNaN(parseInt(s)) ? 0 : parseInt(s);
  return num < -1 * 2**31 ? -1 * 2**31 : num > 2**31 - 1 ? 2**31 - 1 : num;
};
```

## 11. Container With Most Water

[Problem link (MEDIUM)](https://leetcode.com/problems/container-with-most-water)

### Two Pointer

```js
const maxArea = function(height) {
  let maxArea = 0;
  let left = 0;
  let right = height.length - 1;
  
  while (left < right) {
    const area = Math.min(height[left], height[right]) * (right - left);
    maxArea = Math.max(maxArea, area);
    if (height[left] <= height[right]) {
      left++;
    } else {
      right--;
    }
  }
  return maxArea;
};
```

## 14. Longest Common Prefix

```js
const commonPrefix = (str1, str2) => {
  let len = Math.min(str1.length, str2.length);
  for (let i = 0; i < len; i++) {
    if (str1[i] !== str2[i]) {
      len = i;
      break;
    }
  }
  return str1.slice(0, len);
}

const longestCommonPrefix = function(strs) {
  return strs.reduce((prev, curr, index) => {
    if (index === 0) return curr;
    return commonPrefix(prev, curr);
  }, '')
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

## 362. Design Hit Counter

[Problem link (**MEDIUM**)](https://leetcode.com/problems/design-hit-counter)

```js
class HitCounter {
  constructor() {
    this.hits = [];
  }
  hit(timestamp) {
    this.hits.push(timestamp);
  }
  getHits(timestamp) {
    while(this.hits.length !== 0 && this.hits[0] <= timestamp - 300) {
      this.hits.shift();
    }
    return this.hits.length;
  }
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

## 1383. Maximum Performance of a Team

[Problem link (**HARD**)](https://leetcode.com/problems/maximum-performance-of-a-team)

```js
const insertWindow = (num, window) => {
  let index = window.length;
  for (let i = 0; i < window.length; i++) {
    if (window[i] >= num) {
      index = i;
      break;
    }
  }
  window.splice(index, 0, num);
}

const maxPerformance = function(n, speed, efficiency, k) {
  const modulo = BigInt(10 ** 9 + 7);
  let maxP = BigInt(0);
  let speed_sum = BigInt(0);
  let speed_window = [];
  const EandS = efficiency.map((e, i) => [e, speed[i]]).sort((a, b) => b[0] - a[0]);
  
  for (let [e, s] of EandS) {
    maxP = BigInt(e) * (speed_sum + BigInt(s)) > maxP ? BigInt(e) * (speed_sum + BigInt(s)) : maxP;
    
    if (speed_window.length < k - 1) {
      speed_sum += BigInt(s);
      insertWindow(s, speed_window);
    } else if (speed_window[0] < s) {        
      speed_sum = speed_sum - BigInt(speed_window.shift()) + BigInt(s);
      insertWindow(s, speed_window);      
    }
  }
  return maxP % modulo;
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

## 1457. Pseudo-Palindromic Paths in a Binary Tree

### DFS

[Problem link (MEDIUM)](https://leetcode.com/problems/pseudo-palindromic-paths-in-a-binary-tree)

```js
const triggleSet = (val, set) => set.has(val) ? set.delete(val) : set.add(val);

const traverse = (root, nums, res) => {
  if (!root) return;
  
  triggleSet(root.val,nums);  
  if (!root.left && !root.right && nums.size < 2) res[0] = res[0] + 1;     
  traverse(root.left, nums, res);
  traverse(root.right, nums, res);  
  triggleSet(root.val,nums);
}

const pseudoPalindromicPaths  = function(root) {

  const nums = new Set();
  const res = [0];
  traverse(root, nums, res);
  return res[0];
};
```

## 2007. Find Original Array From Doubled Array

[Problem link (MEDIUM)](https://leetcode.com/problems/find-original-array-from-doubled-array)

```js
const findOriginalArray = function(changed) {
  const len = changed.length;
  if (len % 2 === 1) return [];
  const res = [];
  changed.sort((a, b) => a - b);
  const doubled = [];
  
  while (changed.length !== 0) {
    const num = changed.pop();
    if (doubled[0] === num * 2) {
      doubled.shift();
      res.push(num);
    } else {
      doubled.push(num);
    }
  }
  return res.length === len / 2 ? res : [];
};
```

## 78. Subsets

[Problem link (MEDIUM)](https://leetcode.com/problems/subsets)

```js
const subsets = function(nums) {
  const res = [];
  const track = [];
  
  const backtrack = (start) => {
    res.push([...track]);
    for (let i = start; i < nums.length; i++) {
      track.push(nums[i]);
      backtrack(i + 1);
      track.pop();
    }
  }
  
  backtrack(0);
  return res;
};
```

## 90. Subsets II

[Problem link (MEDIUM)](https://leetcode.com/problems/subsets-ii)

```js
const subsetsWithDup = function(nums) {
  nums.sort();
  const res = [];
  const track = [];
  
  const backtrack = (start) => {
    res.push([...track]);
    for (let i = start; i < nums.length; i++) {
      if (i > start && nums[i] === nums[i - 1]) {
        continue;
      }
      track.push(nums[i]);
      backtrack(i + 1);
      track.pop();
    }
  }
  backtrack(0);
  return res;
};
```

## 46. Permutations

[Problem link (MEDIUM)](https://leetcode.com/problems/permutations)

```js
const permute = function(nums) {
  const res = [];
  const track = [];
  const used = Array.from({length: nums.length}, _ => false);

  const trackback = () => {
    if (track.length === nums.length) res.push([...track]);
    for (let i = 0; i < nums.length; i++) {
      if (used[i]) continue;
      used[i] = true;
      track.push(nums[i]);
      trackback();
      track.pop();
      used[i] = false;
    }
  }
  trackback();
  return res;
};
```

## 47. Permutations II

[Problem link (MEDIUM)](https://leetcode.com/problems/permutations-ii)

```js
const permuteUnique = function(nums) {
  nums.sort();
  const res = [];
  const track =[];
  const used = Array.from({length: nums.length}, _ => false);
  
  const trackback = () => {
    if (track.length === nums.length) res.push([...track]);
    for (let i = 0; i < nums.length; i++) {
      if (used[i]) continue;
      if (i > 0 && nums[i] === nums[i - 1] && !used[i-1]) continue;
      track.push(nums[i]);
      used[i] = true;
      trackback();
      used[i] = false;
      track.pop();
    }
  }
  
  trackback();
  return res;
};
```

## 39. Combination Sum

[Problem link (MEDIUM)](https://leetcode.com/problems/combination-sum)

```js
const combinationSum = function(candidates, target) {
  const res = [];
  const track = [];
  let sum = 0;
  const trackback = (start) => {
    if (sum === target) res.push([...track]);
    if (sum > target) return;
    for (let i = start; i < candidates.length; i++) {
      sum += candidates[i];
      track.push(candidates[i]);
      trackback(i);
      track.pop();
      sum -= candidates[i];
    }
  }
  trackback(0);
  return res;
};
```

## 167. Two Sum II - Input Array Is Sorted

[Problem link (MEDIUM)](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted)

### Hash Table

```js
const twoSum = function(numbers, target) {
  const hash = new Map();
  let first, diff
  for (let i = 0; i < numbers.length; i++) {
      first = numbers[i];
      diff = target - first;
      if (hash.has(diff)) return [hash.get(diff) + 1, i + 1];
      hash.set(first, i);
  }
  return [];
};
```

### Two Pointer

```js
const twoSum = function(numbers, target) {
  let left = 0;
  let right = numbers.length - 1;
  while (left < right) {
    const sum = numbers[left] + numbers[right];
    if (sum === target) {
      return [left + 1, right + 1];
    } else if (sum < target) {
      left++;
    } else {
      right--;
    }
  }
  return [];
};
```

## 15. 3Sum

[Problem link (MEDIUM)](https://leetcode.com/problems/3sum)

### Trackback

```js
const threeSum = function(nums) {

  const trackback = (start) => {
    if (track.length === 3) {
      if (sum === 0) {
        res.push([...track]);
      }
      return;
    }
    for (let i = start; i < nums.length; i++){
      if (i > start && nums[i] === nums[i - 1]) continue;
      sum += nums[i];
      track.push(nums[i]);
      trackback(i + 1);
      track.pop();
      sum -= nums[i];
    }
  }

  nums.sort();
  const res = [];
  const track = [];
  let sum = 0;
  trackback(0);
  return res;
};
```

### Two pointer

```js
const threeSum = function(nums) {

  const twoSum = (index) => {
    const num = nums[index];
    let left = index + 1; 
    let right = nums.length - 1;
    while (left < right) {
      const sum = num + nums[left] + nums[right];
      if (sum === 0) {
        res.push([num, nums[left], nums[right]]);
        left++;
        right--;
        while (left < right && nums[left] === nums[left - 1]) {
          left++;
        }
      } else if (sum > 0) {
        right--;
      } else {
        left++;
      }
    }
  }

  nums.sort((a, b) => a - b);
  const res = [];
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] > 0) break;
    if (i > 0 && nums[i] === nums[i - 1]) continue;
    twoSum(i);
  }
  return res;
};
```

### Hashset

```js
const threeSum = function(nums) {
  
  const twoSum = (index) => {
    const num = nums[index];
    const set = new Set();
    let i = index + 1;
    while (i < nums.length) {
      const complement = -1 * num - nums[i];
      if (set.has(complement)) {
        res.push([num, nums[i], complement]);
        while (i + 1 < nums.length && nums[i] === nums[i+1]) {
          i++;
        }
      }
      set.add(nums[i]);
      i++;
    }
  }
  
  nums.sort((a, b) => a - b);
  const res = [];
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] > 0) break;
    if (i > 0 && nums[i] === nums[i - 1]) continue;
    twoSum(i);
  }
  return res;
};
```

## 159. Longest Substring with At Most Two Distinct Characters

[Problem link (MEDIUM)](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters)

### Sliding Window

```js
const lengthOfLongestSubstringTwoDistinct = function(s) {
  let left = 0;
  let right = 0;
  let len = 0;
  const window = new Map();
  
  while (right < s.length) {
    const char = s[right];
    right++;
    
    window.set(char, (window.get(char) || 0) + 1);
   
    while (window.size > 2) {
      len = Math.max(right - left - 1, len);
      const d = s[left];
      left++;
      if (window.get(d) > 1) {  
        window.set(d, window.get(d) - 1);
      } else {
        window.delete(d);
      }
    }
  }
  return Math.max(right - left, len);
};
```

## 76. Minimum Window Substring

[Problem link (HARD)](https://leetcode.com/problems/minimum-window-substring)

### Sliding Window

```js
const minWindow = function(s, t) {
  let left = 0;
  let right = 0;
  let valid = 0;
  const window = new Map();
  const needed = t.split("").reduce((prev, char) => {
    return prev.set(char, (prev.get(char) || 0) + 1)
  }, new Map())

  let start = 0;
  let len = s.length + 1;
  while (right < s.length) {
    const char = s[right];
    right++;
    if (needed.has(char)) {
      window.set(char, (window.get(char) || 0) + 1);
      if (window.get(char) === needed.get(char)) valid++;
    }
    while (valid === needed.size) {
      if (right - left < len) {
        start = left;
        len = right - left;
      }
      const d = s[left];
      left++;
      if (needed.has(d)) {
        if (window.get(d) === needed.get(d)) valid--;
        window.set(d, (window.get(d) || 0) - 1);
      }
    }
  }
  return len === s.length + 1 ? "" : s.slice(start, start + len);
};
```

## 567. Permutation in String

[Problem link (MEDIUM)](https://leetcode.com/problems/permutation-in-string)

### Sliding Window

```js
const checkInclusion = function(s1, s2) {
  let left = 0;
  let right = 0;
  const needed = s1.split("").reduce((prev, char) => {
    return prev.set(char, (prev.get(char) || 0) + 1)
  }, new Map())
  const window = new Map();
  let valid = 0;
  while (right < s2.length) {
    const char = s2[right];
    right++;
    if (needed.has(char)) {
      window.set(char, (window.get(char) || 0) + 1);
      if (needed.get(char) === window.get(char)) valid++;
    }
    
    while (right - left === s1.length) {
      if (valid === needed.size) return true;
      const d = s2[left];
      left++;
      
      if (needed.has(d)) {
        if (needed.get(d) === window.get(d)) valid--;
        window.set(d, window.get(d) - 1);
      }
    }
  }
  return false;
};
```

## 438. Find All Anagrams in a String

[Problem link (MEDIUM)](https://leetcode.com/problems/find-all-anagrams-in-a-string)

### Sliding Window

```js
const findAnagrams = function(s, p) {
  const res = [];
  
  const needed = p.split("").reduce((prev, char) => {
    return prev.set(char, (prev.get(char) || 0) + 1)
  }, new Map())
  const window = new Map();
  let valid = 0;
  let left = 0;
  let right = 0;
  while (right < s.length) {
    const char = s[right];
    right++;
    if (needed.has(char)) {
      window.set(char, (window.get(char) || 0) + 1);
      if (needed.get(char) === window.get(char)) valid++;
    }
    while (right - left === p.length) {
      if (valid === needed.size) res.push(left);
      const d = s[left];
      left++;
      if (needed.has(d)) {
        if (needed.get(d) === window.get(d)) valid--;
        window.set(d, window.get(d) - 1);
      }
    }
  }
  return res;
};
```

## 3. Longest Substring Without Repeating Characters

[Problem link (MEDIUM)](https://leetcode.com/problems/longest-substring-without-repeating-characters)

### Sliding Window

```js
const lengthOfLongestSubstring = function(s) {
  const window = new Map();
  let res = 0;
  let left = 0;
  let right = 0;
  while (right < s.length) {
    const char = s[right];
    right++;
    window.set(char, (window.get(char) || 0) + 1);
    
    while (window.get(char) > 1) {  
      const d = s[left];
      left++;
      window.set(d, window.get(d) - 1);
    }
    res = Math.max(res, right - left);
  }
  return res;
};
```

## 1770. Maximum Score from Performing Multiplication Operations

[Problem link (MEDIUM)](https://leetcode.com/problems/maximum-score-from-performing-multiplication-operations)

### Dynamic Programming

```js
const maximumScore = function(nums, multipliers) {
  const n = nums.length;
  const m = multipliers.length;
  const best = new Array(m + 1).fill(0).map(() => new Array(m + 1).fill(0));
  
  for (let i = 1; i <= m; i += 1) {
    best[i][0] += best[i-1][0] + nums[n-i] * multipliers[i-1];
    best[0][i] += best[0][i-1] + nums[i-1] * multipliers[i-1];
  }
  
  let max = Math.max(best[m][0], best[0][m]);
  
  for (let i = 1; i <= m; i += 1) {
    for (let j = 1; j <= m - i; j += 1) {
      best[i][j] = Math.max(
        best[i-1][j] + nums[n - i] * multipliers[i + j - 1],
        best[i][j-1] + nums[j - 1] * multipliers[i + j - 1],
      );
    }
    max = Math.max(max, best[i][m-i]);
  }
  return max;
};
```
