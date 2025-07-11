## 📚 1. Arrays, Strings, and Linked Lists

### 🔹 Array: Kadane’s Algorithm (Max Subarray Sum)

```java
int maxSubArray(int[] nums) {
    int max = nums[0], curr = nums[0];
    for (int i = 1; i < nums.length; i++) {
        curr = Math.max(nums[i], curr + nums[i]);
        max = Math.max(max, curr);
    }
    return max;
}
```

### 🔹 String: Longest Palindromic Substring (DP)

```java
String longestPalindrome(String s) {
    int n = s.length(), start = 0, maxLen = 1;
    boolean[][] dp = new boolean[n][n];

    for (int i = 0; i < n; i++) dp[i][i] = true;

    for (int len = 2; len <= n; len++) {
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            if (s.charAt(i) == s.charAt(j) && (len == 2 || dp[i + 1][j - 1])) {
                dp[i][j] = true;
                if (len > maxLen) {
                    start = i;
                    maxLen = len;
                }
            }
        }
    }
    return s.substring(start, start + maxLen);
}
```

### 🔹 Linked List: Reverse a Linked List

```java
ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

---

## 🌀 2. Recursion & Backtracking

### 🔹 N-Queens

```java
void solve(int n, int row, List<List<String>> res, char[][] board) {
    if (row == n) {
        List<String> list = new ArrayList<>();
        for (char[] r : board) list.add(new String(r));
        res.add(list);
        return;
    }
    for (int col = 0; col < n; col++) {
        if (isSafe(board, row, col, n)) {
            board[row][col] = 'Q';
            solve(n, row + 1, res, board);
            board[row][col] = '.';
        }
    }
}
```

### 🔹 Subsets

```java
void backtrack(List<List<Integer>> res, List<Integer> temp, int[] nums, int start) {
    res.add(new ArrayList<>(temp));
    for (int i = start; i < nums.length; i++) {
        temp.add(nums[i]);
        backtrack(res, temp, nums, i + 1);
        temp.remove(temp.size() - 1);
    }
}
```

### 🔹 Permutations

```java
void permute(List<List<Integer>> res, List<Integer> temp, int[] nums, boolean[] used) {
    if (temp.size() == nums.length) {
        res.add(new ArrayList<>(temp));
        return;
    }
    for (int i = 0; i < nums.length; i++) {
        if (used[i]) continue;
        used[i] = true;
        temp.add(nums[i]);
        permute(res, temp, nums, used);
        used[i] = false;
        temp.remove(temp.size() - 1);
    }
}
```

---

## 🔁 3. Sorting Algorithms

### 🔹 Merge Sort

```java
void mergeSort(int[] arr, int l, int r) {
    if (l < r) {
        int m = (l + r) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}
```

### 🔹 Quick Sort

```java
void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

### 🔹 Heap Sort

```java
void heapify(int[] arr, int n, int i) {
    int largest = i, l = 2*i+1, r = 2*i+2;
    if (l < n && arr[l] > arr[largest]) largest = l;
    if (r < n && arr[r] > arr[largest]) largest = r;
    if (largest != i) {
        swap(arr, i, largest);
        heapify(arr, n, largest);
    }
}
```

### 🔹 Radix Sort

```java
void countingSort(int[] arr, int exp) {
    int[] output = new int[arr.length];
    int[] count = new int[10];
    for (int num : arr) count[(num / exp) % 10]++;
    for (int i = 1; i < 10; i++) count[i] += count[i - 1];
    for (int i = arr.length - 1; i >= 0; i--) {
        int idx = (arr[i] / exp) % 10;
        output[count[idx] - 1] = arr[i];
        count[idx]--;
    }
    System.arraycopy(output, 0, arr, 0, arr.length);
}
```

---

## 🧠 4. Binary Search

### 🔹 Normal Binary Search

```java
int binarySearch(int[] arr, int key) {
    int l = 0, r = arr.length - 1;
    while (l <= r) {
        int m = (l + r) / 2;
        if (arr[m] == key) return m;
        else if (arr[m] < key) l = m + 1;
        else r = m - 1;
    }
    return -1;
}
```

### 🔹 Search in Rotated Sorted Array

```java
int search(int[] nums, int target) {
    int l = 0, r = nums.length - 1;
    while (l <= r) {
        int mid = (l + r) / 2;
        if (nums[mid] == target) return mid;
        if (nums[l] <= nums[mid]) {
            if (nums[l] <= target && target < nums[mid]) r = mid - 1;
            else l = mid + 1;
        } else {
            if (nums[mid] < target && target <= nums[r]) l = mid + 1;
            else r = mid - 1;
        }
    }
    return -1;
}
```

### 🔹 Binary Search on Answer (e.g., Min in Allocation Problems)

```java
boolean isPossible(int[] pages, int students, int maxPages) {
    int count = 1, sum = 0;
    for (int page : pages) {
        if (page > maxPages) return false;
        if (sum + page > maxPages) {
            count++;
            sum = page;
        } else {
            sum += page;
        }
    }
    return count <= students;
}

int findPages(int[] pages, int students) {
    int low = 0, high = Arrays.stream(pages).sum(), res = -1;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (isPossible(pages, students, mid)) {
            res = mid;
            high = mid - 1;
        } else low = mid + 1;
    }
    return res;
}
```

