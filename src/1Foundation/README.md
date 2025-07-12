# DSA Foundation Boost - Java Implementation Guide

## 🟩 **Arrays, Strings, and Linked Lists**

### Arrays

**Definition**: A collection of elements stored in contiguous memory locations.

**Key Concepts**:
- **Time Complexity**: Access O(1), Search O(n), Insert/Delete O(n)
- **Space Complexity**: O(n)
- **Memory Layout**: Elements stored consecutively in memory

**Important Array Techniques**:

## 1. Two Pointer Technique
#### 🟩 Two Pointer Technique – Explained Simply
The Two Pointer Technique is a common algorithmic approach used to solve problems on arrays or linked lists by using two pointers (often called left and right) that move through the data structure simultaneously under certain rules.

#### ✅ When to Use Two Pointers?
Array is sorted (but not always required)
You are looking for pairs, subarrays, or want to reduce time complexity
You want to avoid nested loops (i.e., reduce O(n²) → O(n))



```java
// Two Sum - Find pair that adds to target
public int[] twoSum(int[] nums, int target) {
    Arrays.sort(nums); // If array needs to be sorted
    int left = 0, right = nums.length - 1;
    
    while (left < right) {
        int sum = nums[left] + nums[right];
        if (sum == target) {
            return new int[]{left, right};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return new int[]{-1, -1};
}
```
---
#### 2. Sliding Window
Used for: Subarray problems, string matching

```java
// Maximum sum subarray of size k
public int maxSumSubarray(int[] arr, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    int maxSum = windowSum;
    for (int i = k; i < arr.length; i++) {
        windowSum = windowSum - arr[i - k] + arr[i];
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```

#### 3. Prefix Sum
Used for: Range queries, subarray sum problems

```java
// Subarray sum equals k
public int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> prefixSum = new HashMap<>();
    prefixSum.put(0, 1);
    
    int sum = 0, count = 0;
    for (int num : nums) {
        sum += num;
        count += prefixSum.getOrDefault(sum - k, 0);
        prefixSum.put(sum, prefixSum.getOrDefault(sum, 0) + 1);
    }
    return count;
}
```

#### 4. Kadane's Algorithm
Used for: Maximum subarray sum

```java
public int maxSubArray(int[] nums) {
    int maxEndingHere = nums[0];
    int maxSoFar = nums[0];
    
    for (int i = 1; i < nums.length; i++) {
        maxEndingHere = Math.max(nums[i], maxEndingHere + nums[i]);
        maxSoFar = Math.max(maxSoFar, maxEndingHere);
    }
    return maxSoFar;
}
```

### Strings

**Key Properties**:
- Immutable in Java (creates new string on modification)
- Character array representation
- ASCII/Unicode encoding

**Important String Techniques**:

#### 1. Pattern Matching - KMP Algorithm
```java
public int[] computeLPS(String pattern) {
    int[] lps = new int[pattern.length()];
    int len = 0, i = 1;
    
    while (i < pattern.length()) {
        if (pattern.charAt(i) == pattern.charAt(len)) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    return lps;
}
```

#### 2. Anagram Detection
```java
public boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    
    int[] count = new int[26];
    for (int i = 0; i < s.length(); i++) {
        count[s.charAt(i) - 'a']++;
        count[t.charAt(i) - 'a']--;
    }
    
    for (int c : count) {
        if (c != 0) return false;
    }
    return true;
}
```

#### 3. Trie Implementation
```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}

class Trie {
    private TrieNode root;
    
    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (curr.children[index] == null) {
                curr.children[index] = new TrieNode();
            }
            curr = curr.children[index];
        }
        curr.isEndOfWord = true;
    }
    
    public boolean search(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int index = c - 'a';
            if (curr.children[index] == null) {
                return false;
            }
            curr = curr.children[index];
        }
        return curr.isEndOfWord;
    }
}
```

### Linked Lists

**Node Implementation**:
```java
class ListNode {
    int val;
    ListNode next;
    
    ListNode() {}
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```

**Key Techniques**:

#### 1. Two Pointer (Tortoise and Hare)
```java
// Detect cycle
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) return false;
    
    ListNode slow = head;
    ListNode fast = head.next;
    
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}

// Find middle node
public ListNode findMiddle(ListNode head) {
    ListNode slow = head;
    ListNode fast = head;
    
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    return slow;
}
```

#### 2. Reverse Linked List
```java
public ListNode reverseList(ListNode head) {
    ListNode prev = null;
    ListNode curr = head;
    
    while (curr != null) {
        ListNode nextTemp = curr.next;
        curr.next = prev;
        prev = curr;
        curr = nextTemp;
    }
    return prev;
}
```

#### 3. Merge Two Sorted Lists
```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    ListNode dummy = new ListNode(0);
    ListNode current = dummy;
    
    while (l1 != null && l2 != null) {
        if (l1.val <= l2.val) {
            current.next = l1;
            l1 = l1.next;
        } else {
            current.next = l2;
            l2 = l2.next;
        }
        current = current.next;
    }
    
    current.next = (l1 != null) ? l1 : l2;
    return dummy.next;
}
```

**Practice Problems - Arrays, Strings, and Linked Lists**:
1. Two Sum (LeetCode 1)
2. Maximum Subarray (LeetCode 53)
3. Container With Most Water (LeetCode 11)
4. Longest Substring Without Repeating Characters (LeetCode 3)
5. Valid Anagram (LeetCode 242)
6. Group Anagrams (LeetCode 49)
7. Reverse Linked List (LeetCode 206)
8. Merge Two Sorted Lists (LeetCode 21)
9. Linked List Cycle (LeetCode 141)
10. Remove Nth Node From End of List (LeetCode 19)

---

## 🟩 **Recursion & Backtracking**

### Recursion Fundamentals

**Definition**: A function that calls itself to solve smaller instances of the same problem.

**Basic Recursion Example**:
```java
// Factorial
public int factorial(int n) {
    if (n <= 1) return 1;  // Base case
    return n * factorial(n - 1);  // Recursive case
}

// Fibonacci
public int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

### Backtracking

**Definition**: Systematic way to iterate through all possible solutions by building candidates incrementally.

**Backtracking Template**:
```java
public void backtrack(List<Integer> current, List<List<Integer>> result, /* other parameters */) {
    // Base case - valid solution found
    if (isValidSolution(current)) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    // Try all possible choices
    for (int choice : getPossibleChoices()) {
        // Make choice
        current.add(choice);
        
        // Recurse
        backtrack(current, result, /* updated parameters */);
        
        // Undo choice (backtrack)
        current.remove(current.size() - 1);
    }
}
```

### N-Queens Problem

```java
public List<List<String>> solveNQueens(int n) {
    List<List<String>> result = new ArrayList<>();
    char[][] board = new char[n][n];
    
    // Initialize board
    for (int i = 0; i < n; i++) {
        Arrays.fill(board[i], '.');
    }
    
    backtrackQueens(board, 0, result);
    return result;
}

private void backtrackQueens(char[][] board, int row, List<List<String>> result) {
    if (row == board.length) {
        result.add(construct(board));
        return;
    }
    
    for (int col = 0; col < board.length; col++) {
        if (isValid(board, row, col)) {
            board[row][col] = 'Q';
            backtrackQueens(board, row + 1, result);
            board[row][col] = '.';
        }
    }
}

private boolean isValid(char[][] board, int row, int col) {
    // Check column
    for (int i = 0; i < row; i++) {
        if (board[i][col] == 'Q') return false;
    }
    
    // Check diagonal
    for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q') return false;
    }
    
    // Check anti-diagonal
    for (int i = row - 1, j = col + 1; i >= 0 && j < board.length; i--, j++) {
        if (board[i][j] == 'Q') return false;
    }
    
    return true;
}
```

### Subsets (Power Set)

```java
public List<List<Integer>> subsets(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrackSubsets(nums, 0, new ArrayList<>(), result);
    return result;
}

private void backtrackSubsets(int[] nums, int start, List<Integer> current, List<List<Integer>> result) {
    result.add(new ArrayList<>(current));
    
    for (int i = start; i < nums.length; i++) {
        current.add(nums[i]);
        backtrackSubsets(nums, i + 1, current, result);
        current.remove(current.size() - 1);
    }
}
```

### Permutations

```java
public List<List<Integer>> permute(int[] nums) {
    List<List<Integer>> result = new ArrayList<>();
    backtrackPermutations(nums, new ArrayList<>(), result);
    return result;
}

private void backtrackPermutations(int[] nums, List<Integer> current, List<List<Integer>> result) {
    if (current.size() == nums.length) {
        result.add(new ArrayList<>(current));
        return;
    }
    
    for (int num : nums) {
        if (current.contains(num)) continue;
        current.add(num);
        backtrackPermutations(nums, current, result);
        current.remove(current.size() - 1);
    }
}
```

**Practice Problems - Recursion & Backtracking**:
1. Generate Parentheses (LeetCode 22)
2. Permutations (LeetCode 46)
3. Permutations II (LeetCode 47)
4. Subsets (LeetCode 78)
5. Subsets II (LeetCode 90)
6. N-Queens (LeetCode 51)
7. Sudoku Solver (LeetCode 37)
8. Word Search (LeetCode 79)
9. Letter Combinations of a Phone Number (LeetCode 17)
10. Combination Sum (LeetCode 39)

---

## 🟩 **Sorting Algorithms**

### Merge Sort

```java
public void mergeSort(int[] arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

private void merge(int[] arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    int[] leftArr = new int[n1];
    int[] rightArr = new int[n2];
    
    System.arraycopy(arr, left, leftArr, 0, n1);
    System.arraycopy(arr, mid + 1, rightArr, 0, n2);
    
    int i = 0, j = 0, k = left;
    
    while (i < n1 && j < n2) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k] = leftArr[i];
            i++;
        } else {
            arr[k] = rightArr[j];
            j++;
        }
        k++;
    }
    
    while (i < n1) {
        arr[k] = leftArr[i];
        i++;
        k++;
    }
    
    while (j < n2) {
        arr[k] = rightArr[j];
        j++;
        k++;
    }
}
```

### Quick Sort

```java
public void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

private int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr, i, j);
        }
    }
    
    swap(arr, i + 1, high);
    return i + 1;
}

private void swap(int[] arr, int i, int j) {
    int temp = arr[i];
    arr[i] = arr[j];
    arr[j] = temp;
}
```

### Heap Sort

```java
public void heapSort(int[] arr) {
    int n = arr.length;
    
    // Build heap
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    
    // Extract elements one by one
    for (int i = n - 1; i > 0; i--) {
        swap(arr, 0, i);
        heapify(arr, i, 0);
    }
}

private void heapify(int[] arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    
    if (largest != i) {
        swap(arr, i, largest);
        heapify(arr, n, largest);
    }
}
```

**Practice Problems - Sorting**:
1. Sort Colors (LeetCode 75)
2. Merge Intervals (LeetCode 56)
3. Largest Number (LeetCode 179)
4. Kth Largest Element in an Array (LeetCode 215)
5. Sort List (LeetCode 148)

---

## 🟩 **Binary Search**

### Basic Binary Search

```java
public int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;
}
```

### Binary Search on Rotated Array

```java
public int searchRotated(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            return mid;
        }
        
        // Left half is sorted
        if (nums[left] <= nums[mid]) {
            if (target >= nums[left] && target < nums[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        // Right half is sorted
        else {
            if (target > nums[mid] && target <= nums[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}
```

### Binary Search on Answer

```java
// Find square root
public int mySqrt(int x) {
    if (x < 2) return x;
    
    int left = 2, right = x / 2;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        long square = (long) mid * mid;
        
        if (square == x) {
            return mid;
        } else if (square < x) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return right;
}
```

### First and Last Position

```java
public int[] searchRange(int[] nums, int target) {
    int[] result = new int[2];
    result[0] = findFirst(nums, target);
    result[1] = findLast(nums, target);
    return result;
}

private int findFirst(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            result = mid;
            right = mid - 1;  // Continue searching left
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

private int findLast(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] == target) {
            result = mid;
            left = mid + 1;  // Continue searching right
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}
```

**Practice Problems - Binary Search**:
1. Binary Search (LeetCode 704)
2. Search Insert Position (LeetCode 35)
3. Find First and Last Position of Element (LeetCode 34)
4. Search in Rotated Sorted Array (LeetCode 33)
5. Find Minimum in Rotated Sorted Array (LeetCode 153)
6. Sqrt(x) (LeetCode 69)
7. Find Peak Element (LeetCode 162)
8. Search a 2D Matrix (LeetCode 74)
9. Median of Two Sorted Arrays (LeetCode 4)
10. Koko Eating Bananas (LeetCode 875)

---

## 🔧 **Problem-Solving Tips**

### General Approach
1. **Understand the Problem**: Read carefully, identify constraints
2. **Think of Examples**: Work through small examples
3. **Identify Pattern**: Look for known algorithms/techniques
4. **Start Simple**: Brute force first, then optimize
5. **Test Edge Cases**: Empty input, single element, duplicates

### Common Optimization Techniques
1. **Two Pointers**: Reduce O(n²) to O(n)
2. **Sliding Window**: For subarray problems
3. **Hash Maps**: For lookups and counting
4. **Sorting**: Can enable greedy or two-pointer approaches
5. **Binary Search**: For sorted data or answer spaces

### Time/Space Complexity Analysis
- **Identify Loops**: Nested loops often indicate O(n²)
- **Recursive Calls**: Use master theorem or recurrence relations
- **Data Structures**: Know complexity of operations
- **Space Trade-offs**: Sometimes space can reduce time complexity

### Java-Specific Tips
- Use `ArrayList` instead of arrays for dynamic sizing
- Use `HashMap` for O(1) lookups
- Use `Collections.sort()` for sorting collections
- Use `Arrays.sort()` for sorting arrays
- Be careful with integer overflow - use `long` when needed
- Use `StringBuilder` for string concatenation in loops

## 📚 **Complete Practice Problem List**

### **Easy Level (Start Here)**:
1. Two Sum (LeetCode 1)
2. Reverse Integer (LeetCode 7)
3. Palindrome Number (LeetCode 9)
4. Valid Parentheses (LeetCode 20)
5. Merge Two Sorted Lists (LeetCode 21)
6. Remove Duplicates from Sorted Array (LeetCode 26)
7. Search Insert Position (LeetCode 35)
8. Maximum Subarray (LeetCode 53)
9. Sqrt(x) (LeetCode 69)
10. Climbing Stairs (LeetCode 70)

### **Medium Level**:
11. Add Two Numbers (LeetCode 2)
12. Longest Substring Without Repeating Characters (LeetCode 3)
13. Container With Most Water (LeetCode 11)
14. 3Sum (LeetCode 15)
15. Letter Combinations of a Phone Number (LeetCode 17)
16. Generate Parentheses (LeetCode 22)
17. Search in Rotated Sorted Array (LeetCode 33)
18. Find First and Last Position of Element (LeetCode 34)
19. Combination Sum (LeetCode 39)
20. Permutations (LeetCode 46)
21. Group Anagrams (LeetCode 49)
22. N-Queens (LeetCode 51)
23. Merge Intervals (LeetCode 56)
24. Unique Paths (LeetCode 62)
25. Sort Colors (LeetCode 75)
26. Subsets (LeetCode 78)
27. Word Search (LeetCode 79)
28. Remove Duplicates from Sorted List II (LeetCode 82)
29. Reverse Linked List II (LeetCode 92)
30. Binary Tree Inorder Traversal (LeetCode 94)

### **Hard Level (Advanced)**:
31. Median of Two Sorted Arrays (LeetCode 4)
32. Regular Expression Matching (LeetCode 10)
33. Merge k Sorted Lists (LeetCode 23)
34. Sudoku Solver (LeetCode 37)
35. Trapping Rain Water (LeetCode 42)

