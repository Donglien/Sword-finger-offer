# 剑指 Offer（第 2 版）

## [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

```java
class Solution {
    public int findRepeatNumber(int[] nums) {
        int[] arr = new int[100001];
        for(int num : nums){
            arr[num]++;
        }
        for(int i = 0; i < 100001; i++){
            if(arr[i] != 0){
                if(arr[i] > 1){
                    return i;
                }
            }
        }
        return -1;
    }
}
```

## [剑指 Offer 04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

每一行二分查找

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        for(int[] arr : matrix){
            if(binarySearch(arr, target) != -1){
                return true;
            }
        }
        return false;
    }

    public int binarySearch(int[] arr,  int target){
        int left = 0;
        int right = arr.length-1;
        while(left <= right){
            int mid = left + (right-left)/2;
            if(arr[mid] == target){
                return mid;
            }else if(arr[mid] > target){
                right = mid-1;
            }else if(arr[mid] < target){
                left = mid+1;
            }
        }
        return -1;
    }
}
```

从左下角或右上角开始查找，类似一颗二叉搜索树

```java
class Solution {
    public boolean findNumberIn2DArray(int[][] matrix, int target) {
        int i = matrix.length-1;
        int j = 0;
        while(i >= 0 && j < matrix[0].length){
            if(matrix[i][j] == target){
                return true;
            }else if(matrix[i][j] > target){
                i--;
            }else if(matrix[i][j] < target){
                j++;
            }
        }
        return false;
    }
}
```

## [剑指 Offer 05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

Java字符串是不可变类型，无法直接修改，需要新建一个来实现

```java
class Solution {
    public String replaceSpace(String s) {
        StringBuilder ans = new StringBuilder();
        for(char ch : s.toCharArray()){
            if(ch == ' '){
                ans.append("%20");
            }else{
                ans.append(ch);
            }
        }
        return ans.toString();
    }
}
```

## [剑指 Offer 06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

```java
class Solution {
    public int[] reversePrint(ListNode head) {
        ListNode tmp = head;
        List<Integer> list = new ArrayList<>();
        while(tmp != null){
            list.add(tmp.val);
            tmp = tmp.next;
        }
        int[] ans = new int[list.size()];
        for(int i = 0; i < list.size(); i++){
            ans[i] = list.get(list.size()-1-i);
        }
        return ans;
    }
}
```
## [剑指 Offer 07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

又重新在纸上画了一遍图，虽然花了好一会儿，但终于十分清楚了

```java
class Solution {
    Map<Integer, Integer> map = new HashMap<>();

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for(int i = 0; i < inorder.length; i++){
            map.put(inorder[i], i);
        }
        TreeNode root = build(preorder, 0, preorder.length, inorder, 0, inorder.length);
        return root;
    }

    public TreeNode build(int[] pre, int pStart, int pEnd, int[] in, int iStart, int iEnd){
        if(pStart == pEnd){
            return null;
        }
        TreeNode root = new TreeNode(pre[pStart]);
        int i = map.get(pre[pStart]);
        int leftNum = i-iStart;
        root.left = build(pre, pStart+1, pStart+1+leftNum, in, iStart, i);
        root.right = build(pre, pStart+1+leftNum, pEnd, in, i+1, iEnd);
        return root;
    }
}
```

## [剑指 Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

```java
class Solution {
    int[][] dir = {{1,0},{-1,0},{0,1},{0,-1}};
    boolean[][] vis;
    char[] ch;
    int len;

    public boolean exist(char[][] board, String word) {
        if(board.length == 0){
            return false;
        }
        
        int r = board.length;
        int c = board[0].length;
        vis = new boolean[r][c];
        ch = word.toCharArray();
        len = word.length();

        for(int i = 0; i < r; i++){
            for(int j = 0; j < c; j++){
                if(dfs(board, i, j, 0)){
                    return true;
                }
            }
        }
        return false;
    }

    public boolean dfs(char[][] board, int r, int c, int begin){
        if(begin == len-1){
            return ch[begin] == board[r][c];
        }
        if(board[r][c] == ch[begin]){
            vis[r][c] = true;
            for(int[] next : dir){
                int x = r + next[0];
                int y = c + next[1];
                if(inArea(board, x, y) && !vis[x][y]){
                    if(dfs(board, x, y, begin+1)){
                        return true;
                    }
                }
            }
            vis[r][c] = false;
        }
        return false;
    }

    public boolean inArea(char[][] board, int r, int c){
        return 0 <= r && r < board.length && 0 <= c && c < board[0].length;
    }
}
```

## [剑指 Offer 15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

```java
public class Solution {
    public int hammingWeight(int n) {
        return Integer.bitCount(n);
    }
}
```

```java
public class Solution {
    public int hammingWeight(int n) {
        int cnt = 0;
        while(n != 0){
            n &= (n-1);
            cnt++;
        }
        return cnt;
    }
}
```
## [剑指 Offer 17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

找出关系即可`10^n−1`

```java
class Solution {
    public int[] printNumbers(int n) {
        int cnt = (int)Math.pow(10, n)-1;
        int[] ans = new int[cnt];
        for(int i = 0; i < cnt; i++){
            ans[i] = i+1;
        }
        return ans;
    }
}
```
## [剑指 Offer 18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

```java
class Solution {
    public ListNode deleteNode(ListNode head, int val) {
        if(head.val == val){
            return head.next;
        }
        
        ListNode pre = head;
        ListNode cur = head.next;
        
        while(cur != null){
            if(cur.val == val){
                pre.next = cur.next;
                break;
            }
            pre = cur;
            cur = cur.next;
        }
        return head;
    }
}
```


## [剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode-cn.com/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

```java
class Solution {
    public int[] exchange(int[] nums) {
        int[] ans = new int[nums.length];
        int idx = 0;
        int[] book = new int[10001];
        
        for(int num : nums){
            if((num & 1) == 1){ // num % 2 等同于 num & 1
                ans[idx++] = num;
                book[num]++;
            }
        }
        for(int num : nums){
            if(book[num] == 0){
                ans[idx++] = num;
            }
        }
        return ans;
    }   
}
```

`i`从左往右找偶数，`j`从右向左找奇数，然后交换

```java
class Solution {
    public int[] exchange(int[] nums) {
        int i = 0;
        int j = nums.length-1;
        while(i < j){
            while(i < j && (nums[i] & 1) == 1){
                i++;
            }
            while(i < j && (nums[j] & 1) == 0){
                j--;
            }
            swap(nums, i, j);
        }
        return nums;
    }   

    public void swap(int[] a, int x, int y){
        int tmp = a[x];
        a[x] = a[y];
        a[y] = tmp;
    }
}
```
## [剑指 Offer 22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

快慢指针

```java
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode fast = head;
        ListNode slow = head;
        for(int i = 0; i < k; i++){
            fast = fast.next;
        }
        while(fast != null){
            fast = fast.next;
            slow = slow.next;
        }
        return slow;
    }
}
```
## [剑指 Offer 24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

直接存在数组里，然后再新建

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode tmp = head;
        List<Integer> list = new ArrayList<>();
        while(tmp != null){
            list.add(tmp.val);
            tmp = tmp.next;
        }   

        ListNode ans = new ListNode(0);
        ListNode cur = ans;
        for(int i = list.size()-1; i >= 0; i--){
            tmp = new ListNode(list.get(i));
            cur.next = tmp;
            cur = cur.next;
        }
        return ans.next;
    }
}
```

双指针

写过，但是不是很清楚，现在仔仔细细的过了一遍，弄清楚了

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while(cur != null){
            ListNode tmp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
}
```

## [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

```java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode ans = new ListNode(0);
        ListNode cur = ans;

        while(l1 != null && l2 != null){
            if(l1.val >= l2.val){
                cur.next = l2;
                l2 = l2.next;
            }else if(l1.val < l2.val){
                cur.next = l1;
                l1 = l1.next;
            }
            cur = cur.next;
        }
        cur.next = (l1 == null ? l2 : l1);
        return ans.next;
    }
}
```
## [剑指 Offer 27. 二叉树的镜像](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

```java
class Solution {
    public TreeNode mirrorTree(TreeNode root) {
        return mirror(root);
    }

    public TreeNode mirror(TreeNode root){
        if(root == null){
            return null;
        }
        TreeNode tree = new TreeNode(root.val);
        tree.left = mirror(root.right);
        tree.right = mirror(root.left);
        return tree;
    }
}
```

## [剑指 Offer 28. 对称的二叉树](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if(root == null){
            return true;
        }
        return judeg(root.left, root.right);
    }

    public boolean judeg(TreeNode left, TreeNode right){
        if(left == null && right == null){
            return true;
        }
        if(left == null || right == null || left.val != right.val){
            return false;
        }
        return judeg(left.left, right.right) && judeg(left.right, right.left);
    }
}
```

## [剑指 Offer 32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

我说怎么内存超出限制，不应该啊，原来是`offer`成根节点的了

```java
class Solution {
    public int[] levelOrder(TreeNode root) {
        List<Integer> list = new ArrayList<>();
        if(root == null){
            return new int[]{};
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            TreeNode node = q.poll();
            list.add(node.val);
            if(node.left != null){
                q.offer(node.left);
            }
            if(node.right != null){
                q.offer(node.right);
            }
        }
        int[] ans = new int[list.size()];
        for(int i = 0; i < list.size(); i++){
            ans[i] = list.get(i);
        }
        return ans;
    }
}
```

## [剑指 Offer 32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root == null){
            return ans;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        while(!q.isEmpty()){
            List<Integer> tmp = new ArrayList<>();
            int cnt = q.size();
            while(cnt != 0){
                TreeNode node = q.poll();
                tmp.add(node.val);
                if(node.left != null){
                    q.offer(node.left);
                }
                if(node.right != null){
                    q.offer(node.right);
                }
                cnt--;
            }
            ans.add(tmp);
        }
        return ans;
    }
}
```

## [剑指 Offer 32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root == null){
            return ans;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        int flag = 1;
        while(!q.isEmpty()){
            List<Integer> tmp = new ArrayList<>();
            int cnt = q.size();
            while(cnt != 0){
                TreeNode node = q.poll();
                tmp.add(node.val);
                if(node.left != null){
                    q.offer(node.left);
                }
                if(node.right != null){
                    q.offer(node.right);
                }
                cnt--;
            }
            if(flag & 2 == 0){
                Collections.reverse(tmp);
            }
            ans.add(tmp);
            flag++;
        }
        return ans;
    }
}
```

## [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

```java
class Solution {
    List<List<Integer>> ans = new ArrayList<>();

    public List<List<Integer>> pathSum(TreeNode root, int target) {
        ArrayList<Integer> track = new ArrayList<>();
        dfs(root, target, track);
        return ans;
    }

    public void dfs(TreeNode root, int target, ArrayList<Integer> tmp){
        if(root == null){
            return;
        }
        tmp.add(root.val);
        target -= root.val;
        if(target == 0 && root.left == null && root.right == null){
            ans.add(new ArrayList(tmp));
        }else{
            dfs(root.left, target, tmp);
            dfs(root.right, target, tmp);
        }
        tmp.remove(tmp.size()-1);
    }
}
```

## [剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)

主要是randon指针的问题，用map

```java
class Solution {
    public Node copyRandomList(Node head) {
        if(head == null){
            return null;
        }

        Map<Node, Node> map = new HashMap<>();
        Node tmp = head;
        while(tmp != null){
            map.put(tmp, new Node(tmp.val));
            tmp = tmp.next;
        }

        tmp = head;
        while(tmp != null){
            Node newNode = map.get(tmp);
            newNode.next = map.get(tmp.next);
            newNode.random = map.get(tmp.random);
            tmp = tmp.next;
        }
        return map.get(head);
    }
}
```

## [剑指 Offer 39. 数组中出现次数超过一半的数字](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

```java
class Solution {
    public int majorityElement(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        for(int num : nums){
            map.put(num, map.getOrDefault(num, 0)+1);
        }
        for(Map.Entry<Integer,Integer> entry : map.entrySet()){
            if(entry.getValue() > nums.length/2){
                return entry.getKey();
            }
        }
        return -1;
    }
}
```

## [剑指 Offer 40. 最小的k个数](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

**快排**。居然还可以用快排，学习了

```java
// 100% 1ms
class Solution {

    public int[] getLeastNumbers(int[] arr, int k) {
        if(arr.length == 0 || k == 0){
            return new int[]{};
        }
        // 最后一个参数表示我们要找的是下标为k-1的数
        return quickSort(arr, 0, arr.length-1, k-1);
    }

    public int[] quickSort(int[] a, int low, int hight, int k) {
        int pivot = partition(a, low, hight);
        if(pivot == k){
            return Arrays.copyOf(a, pivot+1);
        }
        return pivot > k ? 
            quickSort(a, low, pivot-1, k) : quickSort(a, pivot+1, hight, k);
    }

    
    public int partition(int[] a, int low, int hight) {
        int pivotKey = a[low];
        int i = low, j = hight+1;
        while(true){
            while(++i <= hight && a[i] < pivotKey);
            while(--j >= low && a[j] > pivotKey);

            if(i >= j) break;
            swap(a, i, j);
        }
        a[low] = a[j];
        a[j] = pivotKey;
        return j;
    }

    public void swap(int[] a, int i, int j){
        int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }
}
```

**堆排序**

```java
// 击败69% 7ms
class Solution {
    int[] ans;
    int idx = 0;

    public int[] getLeastNumbers(int[] arr, int k) {
        ans = new int[k];
        heapSort(arr, k);
        return ans;
    }

    public void heapSort(int[] arr, int k){
        int n = arr.length-1;
        for(int i = n/2; i >= 0; i--){
            sink(arr, i, n);
        }
        while(n >= 0){
            if(k == 0) break;
            ans[idx++] = arr[0];
            swap(arr, 0, n--);
            sink(arr, 0, n);
            k--;
        }
    }

    public void sink(int[] a, int k, int n){
        while(2*k+1 <= n){
            int j = 2*k+1;
            if(j < n && a[j] > a[j+1]) j++;
            if(a[k] < a[j]) break;
            swap(a, j, k);
            k = j;
        }
    }

    public void swap(int[] a, int i, int j){
        int tmp = a[i];
        a[i] = a[j];
        a[j] = tmp;
    }
}
```

**优先队列**，用Java自带的，额，居然还没有手写的快。。。

```java
// 36% 11ms
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(arr.length == 0 || k == 0){
            return new int[]{};
        }
        // 默认是小根堆，实现大根堆需要重写一下比较器。
        Queue<Integer> q = new PriorityQueue<>((a, b) -> b-a);
        for(int num: arr){
            if(q.size() < k){
                q.offer(num);
            }else if(num < q.peek()){
                q.poll();
                q.offer(num);
            }
        }
        int[] ans = new int[q.size()];
        int idx = 0;
        for(int i: q){
            ans[idx++] = i;
        }
        return ans;
    }
}
```

**基数排序**，如果数据量不大的话，并且全部为正

```java
// 击败 100%, 1ms
class Solution {
    public int[] getLeastNumbers(int[] arr, int k) {
        if(arr.length == 0 || k == 0){
            return new int[]{};
        }
        int[] cnt = new int[10001];
        for(int i: arr){
            cnt[i]++;
        }
        int[] ans = new int[k];
        int idx = 0;
        for(int i = 0; i < cnt.length; i++){
            while(cnt[i]-- > 0){
                if(idx < k){
                    ans[idx++] = i;
                }
            }
            if(idx == k) break;
        }
        return ans;
    }
}
```

## [剑指 Offer 50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

```java
class Solution {
    public char firstUniqChar(String s) {
        if(s.length() == 0){
            return ' ';
        }
        int[] book = new int[26];
        for(char ch : s.toCharArray()){
            book[ch-'a']++;
        }
        for(char ch : s.toCharArray()){
            if(book[ch-'a'] == 1){
                return ch;
            }
        }
        return ' ';
    }
}
```
## [剑指 Offer 52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

好吧，明显想多了，是双指针，不是快慢指针

```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        if(headA == null || headB == null){
            return null;
        }
        ListNode fast = headA;
        ListNode slow = headB;

        while(fast != slow){
            fast = (fast == null ? headB : fast.next);
            slow = (slow == null ? headA : slow.next);
        }
        return fast;
    }
}
```

## [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

先从头找，再从尾找，最后相减即可

```java
class Solution {
    public int search(int[] nums, int target) {
        int i = 0;
        int j = nums.length-1;
        while(i < nums.length){
            if(nums[i] == target){
                break;
            }
            i++;
        }
        if(i == nums.length){
            return 0;
        }
        while(j >= 0){
            if(nums[j] == target){
                break;
            }
            j--;
        }
        return j-i+1;
    }
}
```

二分法

```java
class Solution {
    public int search(int[] nums, int target) {
        int left = leftBound(nums, target);
        if(left == -1){
            return 0;
        }
        int right = rightBound(nums, target);
        return right-left+1;
    }

    public int leftBound(int[] nums, int target){
        int left = 0;
        int right = nums.length;
        while(left < right){
            int mid = left + (right-left)/2;
            if(nums[mid] == target){
                right = mid;
            }else if(nums[mid] > target){
                right = mid;
            }else if(nums[mid] < target){
                left = mid+1;
            }
        }
        if(left == nums.length) return -1;
        return nums[left] == target ? left : -1;
    }

    public int rightBound(int[] nums, int target){
        int left = 0;
        int right = nums.length;
        while(left < right){
            int mid = left + (right-left)/2;
            if(nums[mid] == target){
                // 这里加一了，后面循环结束的时候，nums[left]不一定等于target。而nums[left-1]可能是target
                left = mid+1; 
            }else if(nums[mid] > target){
                right = mid;
            }else if(nums[mid] < target){
                left = mid+1;
            }
        }
        if(left == 0) return -1;
        return nums[left-1] == target ? left-1:-1;
    }
}
```

## [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

```java
class Solution {
    public int missingNumber(int[] nums) {
        int[] arr = new int[10001];
        for(int num:nums){
            arr[num]++;
        }
        int i = 0;
        for(; i < 10001; i++){
            if(arr[i] == 0){
                break;
            }
        }
        return i;
    }
}
```

二分法

缺失的数字等于 **“右子数组的首位元素”** 对应的索引

```java
class Solution {
    public int missingNumber(int[] nums) {
        int left = 0;
        int right = nums.length-1;
        while(left <= right){
            int mid = left + (right-left)/2;
            
            // 如果nums[mid] == mid，右子数组的首位元素必定在[mid+1, right]
            if(nums[mid] == mid){
                left = mid+1;
            }else{ // 则左子数组的末位元素在[left, mid-1]
                right = mid-1;
            }
        }
        return left;
    }
}
```

## [剑指 Offer 54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

中序遍历变成数组，直接倒序返回索引-k即可

```java
class Solution {
    List<Integer> arr = new ArrayList<>();

    public int kthLargest(TreeNode root, int k) {
        inOrder(root);
        return arr.get(arr.size()-k);
    }

    public void inOrder(TreeNode root){
        if(root == null){
            return;
        }
        inOrder(root.left);
        arr.add(root.val);
        inOrder(root.right);
    }
}
```

找到就提前终止

```java
class Solution {
    int cnt = 0;
    int ans = 0;

    public int kthLargest(TreeNode root, int k) {
        cnt = k;
        inOrder(root);
        return ans;
    }

    public void inOrder(TreeNode root){
        if(root == null){
            return;
        }
        inOrder(root.right);
        if(cnt == 0){
            return;
        }
        if(--cnt == 0){
            ans = root.val;
        }
        inOrder(root.left);
    }
}
```

## [剑指 Offer 55 - I. 二叉树的深度](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

```java
class Solution {
    public int maxDepth(TreeNode root) {
        return deep(root);
    }

    public int deep(TreeNode root){
        if(root == null){
            return 0;
        }
        int left = deep(root.left);
        int right = deep(root.right);
        return Math.max(left,right)+1;
    }
}
```


## [剑指 Offer 58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

套模板

```java
class Solution {
    public String reverseWords(String s) {
		s += " ";
        StringBuilder tmp = new StringBuilder();
        List<String> list = new ArrayList<>();

        for(char ch : s.toCharArray()){
            if(ch == ' '){
                if(tmp.length() != 0){
                    list.add(tmp.toString());
                    tmp.delete(0, tmp.length());
                }
            }else{
                tmp.append(ch);
            }
        }

        tmp = new StringBuilder();
        for(int i = list.size()-1; i >= 0; i--){
            if(i != list.size()-1){
                tmp.append(" ");
            }
            tmp.append(list.get(i));
        }
        return tmp.toString();
    }
}
```

双指针（目前不太懂）

```java
class Solution {
    public String reverseWords(String s) {
        s = s.trim();
        int j = s.length()-1;
        int i = j;
        StringBuilder ans = new StringBuilder();
        while(i >= 0){
            while(i >= 0 && s.charAt(i) != ' '){
                i--;
            }
            ans.append(s.substring(i+1, j+1) + " "); // 从[i+1,j+1)，不写默认到末尾
            while(i >= 0 && s.charAt(i) == ' '){
                i--;
            }
            j = i;
        }
        return ans.toString().trim();
    }
}
```

使用内置函数

```java
class Solution {
    public String reverseWords(String s) {
        String[] word = s.trim().split(" ");
        StringBuilder ans = new StringBuilder();
        for(int i = word.length-1; i >= 0; i--){
            if(word[i].equals("")){
                continue;
            }
            if(i != word.length-1){
                ans.append(" ");
            }
            ans.append(word[i]);
        }
        return ans.toString();
    }
}
```

## [剑指 Offer 68 - I. 二叉搜索树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

## [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

```java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null){
            return null;
        }
        if(root == p || root == q){
            return root;
        }
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null){
            return right;
        }
        if(right == null){
            return left;
        }
        if(left != null && right != null){
            return root;
        }
        return null;
    }
}
```

