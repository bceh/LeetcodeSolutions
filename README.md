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

## 336. Palindrome Pairs

[Problem link (HARD)](https://leetcode.com/problems/palindrome-pairs)

Input and Output:

Input is a array of words; the output is index
We need a map: word -> index;

Combinations:

1. Case 1: Same length.
   - "abcd" - "dcba"; "dcba" - "abcd";
2. Case 2: Left longer:
   - "s" - "lls" -> "s" - "ll" - "s"
3. Case 3: Right longer
   - "sll" - "s" -> "s" - "ll" - "s"

```js
const isPalindromeBetween = (word, f, b) => {
  while (f < b) {
    if (word[f] !== word[b]) return false;
    f++;
    b--;
  }
  return true;
}

const getPrefix = (word) => {
  const pres = [];
  for (let i = 0; i < word.length; i++) {
    if (isPalindromeBetween(word, i, word.length - 1)) {
      pres.push([0, i]);
    }
  }
  return pres;
}

const getSuffix = (word) => {
  const suf = [];
  for (let i = 0; i < word.length; i++) {
    if (isPalindromeBetween(word, 0, i)) {
      suf.push([i + 1, word.length]);
    }
  }
  return suf;
}


const palindromePairs = function(words) {
  const hash = new Map();
  const res = [];
  for (let i = 0; i < words.length; i++) {
    const rev = words[i].split("").reverse().join("");
    hash.set(rev, i);
  }
  for (let i = 0; i < words.length; i++) {
    const word = words[i];
    if (hash.has(word) && hash.get(word) !== i) res.push([i, hash.get(word)]);
    getPrefix(word).forEach(([f, b]) => {
      const pre = word.slice(f, b);
      if(hash.has(pre)) res.push([i, hash.get(pre)])
    })
    getSuffix(word).forEach(([f, b]) => {
      const suf = word.slice(f, b);
      if(hash.has(suf)) res.push([hash.get(suf), i])
    })
  }
  return res;
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

## 609. Find Duplicate File in System

[Problem link (MEDIUM)](https://leetcode.com/problems/find-duplicate-file-in-system)

### HashMap

```js
const findDuplicate = function(paths) {
  const hash = new Map();
  for (let path of paths) {
    const rf = path.split(' ');
    const root = rf[0];
    const files = rf.slice(1);
    for (let file of files) {
      const nc = file.split('(');
      const name = nc[0];
      const content = nc[1].split(')')[0];
      const p = `${root}/${name}`;
      hash.has(content) ? hash.get(content).push(p) : hash.set(content, [p]);
    }
  }
  return [...hash.values()].filter(arr=>arr.length > 1);
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
  const dp = new Array(m + 1).fill(0)
              .map((_, i) => new Array(m + 1 - i).fill(0));
  
  for (let i = 1; i <= m; i += 1) {
    dp[i][0] += dp[i-1][0] + nums[n-i] * multipliers[i-1];
    dp[0][i] += dp[0][i-1] + nums[i-1] * multipliers[i-1];
  }
  
  let max = Math.max(dp[m][0], dp[0][m]);
  
  for (let i = 1; i <= m; i += 1) {
    for (let j = 1; j <= m - i; j += 1) {
      dp[i][j] = Math.max(
        dp[i-1][j] + nums[n - i] * multipliers[i + j - 1],
        dp[i][j-1] + nums[j - 1] * multipliers[i + j - 1],
      );
    }
    max = Math.max(max, dp[i][m-i]);
  }
  return max;
};
```

## 322. Coin Change

[Problem link (MEDIUM)](https://leetcode.com/problems/coin-change)

### Dynamic Programming

```js
const coinChange = function(coins, amount) {
  const dp = new Array(amount + 1).fill(amount + 1);
  dp[0] = 0;
  for (let i = 1; i < amount + 1; i++) {
    for (let j = 0; j < coins.length; j++) {
      const rest = i - coins[j];
      if (rest >= 0) {
        dp[i] = Math.min(dp[rest] + 1, dp[i]);
      }
    }
  }
  return dp[amount] === amount + 1 ? -1 : dp[amount];
};
```

## 931. Minimum Falling Path Sum

[Problem link (MEDIUM)](https://leetcode.com/problems/minimum-falling-path-sum)

### Dynamic Programming

```js
const minFallingPathSum = function(matrix) {
  if (matrix.length === 1) return matrix[0][0];
  const dp = new Array(matrix.length).fill(0)
              .map(_ => new Array(matrix.length).fill(Infinity));
  for (let i = 0; i < matrix.length; i++) {
    dp[0][i] = Math.min(matrix[0][i], dp[0][i]);
  }
  let min = Infinity;
  for (let i = 1; i < matrix.length; i++) {
    for (let j = 0; j < matrix.length; j++) {
      let prev;
      if (j === 0) prev = Math.min(dp[i-1][j], dp[i-1][j+1]);
      else if (j === matrix.length - 1) prev = Math.min(dp[i-1][j], dp[i-1][j-1]);
      else prev = Math.min(dp[i-1][j], dp[i-1][j-1], dp[i-1][j+1])
      dp[i][j] =  prev + matrix[i][j];
      if (i === matrix.length - 1) {
        min = Math.min(dp[i][j], min);
      }
    }
  }
  return min;
};
```

## 300. Longest Increasing Subsequence

[Problem link (MEDIUM)](https://leetcode.com/problems/longest-increasing-subsequence)

### Dynamic Programming

```js
const lengthOfLIS = function(nums) {
  const dp = new Array(nums.length).fill(1);
  let max = 1;
  for (let i = 0; i < nums.length; i++) {
    for (let j = 0; j < i; j++) {
      if (nums[i] > nums[j]) dp[i] = Math.max(dp[j] + 1, dp[i]);      
    }
    max = Math.max(max, dp[i]);
  }
  return max;
}
```

### Binary Search

```js
const lengthOfLIS = function(nums) {
  const arr = [];
  for (let num of nums) {
    if (arr.length === 0 || num > arr[arr.length - 1]) {
      arr.push(num);
      continue;
    }
    let left = 0;
    let right = arr.length - 1;
    while (left < right) {
      const mid = Math.floor((left + right) / 2);
      if (num > arr[mid]) {
        left = mid + 1;
      } else if (num <= arr[mid]){
        right = mid;
      }
    }
    arr[right] = num;
  }
  return arr.length;
}
```

## 53. Maximum Subarray

[Problem link (MEDIUM)](https://leetcode.com/problems/maximum-subarray)

### Dynamic Programming

```js
const maxSubArray = function(nums) {
  let max = nums[0];
  let prev = nums[0];
  for (let i = 1; i < nums.length; i++) {
    prev = prev > 0 ? prev + nums[i] : nums[i];
    max = Math.max(prev, max);
  }
  return max;
};
```

## 1143. Longest Common Subsequence

[Problem link(MEDIUM)](https://leetcode.com/problems/longest-common-subsequence)

### Dynamic Programming

```js
const longestCommonSubsequence = function(text1, text2) {
  const memo = new Array(text1.length).fill(0).map(_ => new Array(text2.length))
  const dp = (i, j) => {
    if (i === text1.length || j === text2.length) {
      return 0;
    }
    if (memo[i][j] !== undefined) return memo[i][j];
    if (text1[i] === text2[j]) {
      memo[i][j] = dp(i+1, j+1) + 1;
    } else {
      memo[i][j] = Math.max(dp(i+1, j), dp(i, j+1));
    }
    return memo[i][j];
  }
  return dp(0,0);
};
```

## 583. Delete Operation for Two Strings

[Problem link(MEDIUM)](https://leetcode.com/problems/delete-operation-for-two-strings)

### Dynamic Programming

```js
const minDistance = function(word1, word2) {
  const m = word1.length;
  const n = word2.length;
  const memo = new Array(m).fill(0).map(_ => new Array(n));
  const dp = (i, j) => {
    if (i === word1.length || j === word2.length) return 0;
    if (memo[i][j] !== undefined) return memo[i][j];
    if (word1[i] === word2[j]) {
      memo[i][j] = dp(i+1, j+1) + 1;
    } else {
      memo[i][j] = Math.max(dp(i, j+1), dp(i+1, j));
    }
    return memo[i][j];
  }
  return m + n - dp(0, 0) * 2;
};
```

## 72. Edit Distance

[Problem link(HARD)](https://leetcode.com/problems/edit-distance)

Describe

- word1: horse
- word2: ros

h !== r:

1. insert -> rhorse
  dp(i, j+1) + 1;
2. delete -> orse
  dp(i+1,j) + 1;
3. replace -> rorse
  dp(i+1,j+1) + 1;

r === r:

rorse -> orse;
  dp(i+1, j+1)

### Dynamic Programming

```js
const minDistance = function(word1, word2) {
  const memo = new Array(word1.length).fill(0)
                .map(_ => new Array(word2.length));
  const dp = (i, j) => {
    if (i === word1.length) return word2.length - j;
    if (j === word2.length) return word1.length - i;
    if (memo[i][j] !== undefined) return memo[i][j];
    if (word1[i] === word2[j]) {
      memo[i][j] = dp(i+1, j+1);
    } else {
       memo[i][j] = Math.min(
        dp(i, j+1) + 1, dp(i+1, j) + 1, dp(i+1, j+1) + 1
      )
    }   
    return memo[i][j];
  }
  return dp(0,0);
};
```

## 10. Regular Expression Matching

[Problem link (HARD)](https://leetcode.com/problems/regular-expression-matching)

### Dynamic Programming

```js
const isMatch = function(s, p) {
  const memo = new Array(s.length).fill(0)
                .map(_ => new Array(p.length));
  const dp = (i, j) => {
    if (j === p.length) return i === s.length
    if (i === s.length) {
      const rest = p.slice(j);
      if (rest.length % 2 === 1) return false;
      for (let k = 0; k < rest.length; k = k+2) {
        if (rest[k] === "*" || rest[k+1] !== "*" ) {
          return false;
        }
      }
      return true;
    }
    if (memo[i][j] !== undefined) return memo[i][j];
    if (s[i] === p[j] || p[j] === ".") {
      if (j < p.length - 1 && p[j+1] === "*") {
       memo[i][j] = dp(i, j+2) || dp(i+1, j);
      } else {
        memo[i][j] = dp(i+1, j+1);
      }
    } else {
      if (j < p.length - 1 && p[j+1] === "*") {
        memo[i][j] = dp(i, j+2);
      } else {
        memo[i][j] = false;
      }
    }
    return memo[i][j];
  }
  return dp(0, 0);
};
```

## 518. Coin Change II

### Dynamic Programming

#### Recursion

```js
const change = function(amount, coins) {
  const memo = new Array(amount+1).fill(0)
                .map(_ => new Array(coins.length + 1));
  const dp = (a, c) => {
    if (a <= 0) return 1;
    if (c <= 0) return 0;
    if (memo[a][c] !== undefined) return memo[a][c];
    const coin = coins[c-1];
    let count = a % coin === 0 ? 1 : 0;
    for (let i = 0; i * coin < a; i++) {
      count += dp(a - i * coin, c-1);
    }
    memo[a][c] = count;
    return memo[a][c];
  }
  return dp(amount, coins.length);
};
```

#### Iteration (2D-Array)

```js
const change = function(amount, coins) {
  const dp = new Array(amount+1).fill(0)
                .map(_ => new Array(coins.length + 1).fill(0));
  for (let j = 0; j < coins.length + 1; j++) {
    dp[0][j] = 1;
  }
  for (let i = 1; i < amount + 1; i++) {
    for (let j = 1; j < coins.length + 1; j++) {
      const coin = coins[j-1];
      if (i - coin >= 0) {
        dp[i][j] = dp[i][j-1] + dp[i-coin][j];
      } else {
        dp[i][j] = dp[i][j-1];
      } 
    }
  }
  return dp[amount][coins.length];
};
```

#### Iteration (1D-Array)

```js
const change = function(amount, coins) {
  const dp = new Array(amount+1).fill(0)
  dp[0] = 1;
  for (let i = 0; i < coins.length; i++) {
    for (let j = 1; j < amount + 1; j++) {
      if (j - coins[i] >= 0) {
        dp[j] += dp[j-coins[i]];
      }
    }
  }
  return dp[amount];
};
```

## 416. Partition Equal Subset Sum

[Problem link(MEDIUM)](https://leetcode.com/problems/partition-equal-subset-sum)

### Dynamic Programming

#### Iteration (2D-Array)

```js
const canPartition = function(nums) {
  const sum = nums.reduce((sum, val)=> sum + val,0);
  if (sum % 2 !== 0) return false;
  const target = sum / 2;
  const dp = new Array(target+1).fill(0).map( _=> new Array(nums.length + 1).fill(false));
  for (let i = 0; i <= nums.length; i++) {
    dp[0][i] = true;
  }
  for (let i = 1; i <= target; i++) {
    for (let j = 1; j <= nums.length; j++) {
      const num = nums[j];
      if (i - num >= 0) {
        dp[i][j] = dp[i][j-1] || dp[i-num][j-1];
      } else {
         dp[i][j] = dp[i][j-1]
      }
    }
  }
  return dp[target][nums.length]
};
```

#### Iteration (1D-Array)

```js
const canPartition = function(nums) {
  const sum = nums.reduce((sum, val)=> sum + val,0);
  if (sum % 2 !== 0) return false;
  const target = sum / 2;
  const dp = new Array(target+1).fill(false);
  dp[0] = true;
  for (let i = 0; i < nums.length; i++) {
    const num = nums[i];
    for (let j = target; j >=0; j--) {
      if (j - num >= 0) {
        dp[j] = dp[j] || dp[j-num];
      } 
    }   
  }
  return dp[target]
};
```

## 121. Best Time to Buy and Sell Stock

[Problem link(EASY)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock)

### Array

```js
const maxProfit = function(prices) {
  const n = prices.length
  const buy = new Array(n);
  const sell = new Array(n);
  buy[0] = prices[0];
  sell[n-1] = prices[n-1];
  let profit = prices[n-1] - prices[0];
  for (let i = 1; i < n; i++) {
    buy[i] = Math.min(buy[i-1], prices[i]);
  }
  for (let i = n - 2; i >= 0; i--) {
    sell[i] = Math.max(sell[i+1], prices[i]);
    profit = Math.max(sell[i] - buy[i], profit);
  }
  return profit;
};
```

### One pass

```js
const maxProfit = function(prices) {
  let profit = 0;
  let min_price = prices[0];
  for (let i = 1; i <  prices.length; i++) {
    profit = Math.max(prices[i] - min_price, profit);
    min_price = Math.min(prices[i], min_price);
  }
  return profit;
};
```

### State Machine

```js
const maxProfit = function(prices) {
  let sell = 0; 
  let buy = -1 * prices[0];
  for (let i = 1; i <  prices.length; i++) {
    sell = Math.max(sell, buy + prices[i]);
    buy = Math.max(buy, -1 * prices[i]);
  }
  return sell;
};
```

### Dynamic Programming

#### Iteration (2-D Array)

```js
const maxProfit = function(prices) {
  const n = prices.length;
  const dp = new Array(n).fill(0).map(_=>new Array(2));
  dp[0][0] = 0;
  dp[0][1] = -1 * prices[0];
  for (let i = 1; i <  n; i++) {
    dp[i][0] = Math.max(dp[i-1][0], dp[i-1][1] + prices[i]);
    dp[i][1] = Math.max(dp[i-1][1], -1 * prices[i]);
  }
  return dp[n-1][0];
};
```

## 122. Best Time to Buy and Sell Stock II

[Problem link(MEDIUM)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii)

### State Machine

```js
const maxProfit = function(prices) {
  let buy = -1 * prices[0];
  let sell = 0;
  for (let i = 1; i < prices.length; i++) {
    const last_buy = buy;
    buy = Math.max(buy, sell - prices[i]);
    sell = Math.max(sell, last_buy + prices[i]);
  }
  return sell;
};
```

## 123, Best Time to Buy and Sell Stock III

[Problem link(HARD)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii)

### State Machine

```js
const maxProfit = function(prices) {
  let buy_f = -1*prices[0];
  let sell_f = -Infinity;
  let buy_s = -Infinity;
  let sell_s = -Infinity;
  
  for (let i = 1; i < prices.length; i++) {
    sell_s = Math.max(sell_s, buy_s + prices[i])
    buy_s = Math.max(buy_s, sell_f - prices[i]);
    sell_f = Math.max(sell_f, buy_f + prices[i]);
    buy_f = Math.max(buy_f, -1 * prices[i]);
  }
  return Math.max(0, sell_f, sell_s);
};
```

## 188. Best Time to Buy and Sell Stock IV

[Problem link(HARD)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv)

### Dynamic Programming

#### Iteration (3-D Array)

```js
const maxProfit = function(k, prices) {
  const dp = new Array(prices.length+1).fill(0)
              .map(_ => new Array(2*k+1).fill(0)
                  .map(_ => new Array(2).fill(-Infinity)));
  for (let i = 0; i <= prices.length; i++) {
    dp[i][0][0] = 0;
  }
  for (let j = 0; j <= 2*k; j++) {
    dp[0][j][0] = 0;
  }
  
  for (let i = 1; i <= prices.length; i++) {
    for (let j = 1; j <= 2*k; j++) {
      dp[i][j][0] = Math.max(dp[i-1][j][0], dp[i-1][j-1][1]+prices[i-1]);
      dp[i][j][1] = Math.max(dp[i-1][j][1], dp[i-1][j-1][0]-prices[i-1]);
    }
  }
  return dp[prices.length][2*k][0];
};
```

## 309. Best Time to Buy and Sell Stock with Cooldown

[Problem link(MEDIUM)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown)

### State Machine

```js
const maxProfit = function(prices) {
  let buy = -prices[0];
  let sell = 0;
  let pre_sell = 0;
  for (let i = 1; i < prices.length; i++) {
    const temp = sell;
    sell = Math.max(sell, buy + prices[i]);
    buy = Math.max(buy, pre_sell - prices[i]);
    pre_sell = temp;
  }
  return Math.max(0, pre_sell, sell);
};
```

## 714. Best Time to Buy and Sell Stock with Transaction Fee

[Problem link(MEDIUM)](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee)

### State machine

```js
const maxProfit = function(prices, fee) {
  let buy = -prices[0];
  let sell = 0;
  for (let i = 1; i< prices.length; i++) {
    const temp = buy;
    buy = Math.max(buy, sell - prices[i]);
    sell = Math.max(sell, temp + prices[i] - fee);
  }
  return Math.max(0, sell);
};
```

## 198. House Robber

[Problem link(MEDIUM)](https://leetcode.com/problems/house-robber)

### Dynamic Programming

#### Iteration (2-D Array)

```js
const rob = function(nums) {
  const dp = new Array(nums.length).fill(0).map(_=>new Array(2));
  dp[0][0] = nums[0];
  dp[0][1] = 0;
  for (let i = 1; i < nums.length; i++) {
    dp[i][0] = dp[i-1][1] + nums[i];
    dp[i][1] = Math.max(dp[i-1][1], dp[i-1][0]);
  }
  return Math.max(...dp[nums.length-1])
};
```

#### Iteration (State Compression)

```js
const rob = function(nums) {
  let rob = nums[0];
  let no_rob = 0;
  for (let i = 1; i < nums.length; i++) {
    const last_rob = rob;
    rob = no_rob + nums[i];
    no_rob = Math.max(last_rob, no_rob);
  }
  return Math.max(rob, no_rob)
};
```

## 213. House Robber II

[Problem link(MEDIUM)](https://leetcode.com/problems/house-robber-ii)

### Dynamic Programming (State Compression)

```js
const rob = function(nums) {
  let f_rob_0 = nums[0];
  let f_rob_1 = 0;
  let f_no_rob_0 = 0;
  let f_no_rob_1 = 0;
  
  for (let i = 1; i < nums.length - 1; i++) {
    const f_rob_last = f_rob_0;
    const f_no_rob_last = f_no_rob_0;
    f_rob_0 = f_rob_1 + nums[i];
    f_rob_1 = Math.max(f_rob_1, f_rob_last);
    f_no_rob_0 = f_no_rob_1 + nums[i];
    f_no_rob_1 = Math.max(f_no_rob_1, f_no_rob_last);
  }
  return Math.max(f_rob_0, f_rob_1, f_no_rob_0, f_no_rob_1 + nums.at(-1))
};
```

## 337. House Robber III

[Problem link(MEDIUM)](https://leetcode.com/problems/house-robber-iii)

### Dynamic Programming

#### Recursion with memo

```js
const rob = function(root) {
  const memo = new Map();
  const traverse = (root) => {
    if (!root) return 0;
    if (memo.has(root)) {
      return memo.get(root);
    }
    const rob_left = root.left ? traverse(root.left.left) + traverse(root.left.right) : 0;
    const rob_right = root.right ? traverse(root.right.left) + traverse(root.right.right) : 0;
    const rob = rob_left + rob_right + root.val;
    const no_rob = traverse(root.left) + traverse(root.right);
    const res = Math.max(rob, no_rob);
    memo.set(root, res)
    return res;
  }
  return traverse(root);
};
```

#### Recursion without memo

```js
const rob = function(root) {
  const traverse = (root) => {
    if (!root) return [0, 0];
    const left = traverse(root.left);
    const right = traverse(root.right);
    const rob = root.val + left[1] + right[1];
    const no_rob = Math.max(...left) + Math.max(...right);
    return [rob, no_rob];
  }
  return Math.max(...traverse(root));
};
```

## 877. Stone Game

[Problem link (MEDIUM)](https://leetcode.com/problems/stone-game)

### Dynamic Programming

```js
const stoneGame = function(piles) {
  const n = piles.length;
  const memo = Array(n).fill(0).map(() => Array(n));
  const dp = (i, j) => {
    if (memo[i][j] !== undefined) return memo[i][j];
    if (j - i === 1) {
      memo[i][j] = Math.abs(piles[i] - piles[j]);
    } else {
      memo[i][j] = Math.max(piles[i] - dp(i+1, j), piles[j] - dp(i, j-1));
    }
    return memo[i][j];
  }
  return dp(0, n-1) > 0;
};
```
