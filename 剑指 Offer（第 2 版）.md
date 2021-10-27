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
                ans[idx++] = snum;
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
            if(nums[j] == targetd){
                break;
            }
            j--;
        }
        return j-i+1;
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

双指针

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





