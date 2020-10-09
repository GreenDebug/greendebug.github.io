csjue的力扣

暂时跳过的题目q384 q32 q10 q40

这学期开始水水力扣。按照这个[顺序](https://www.zhihu.com/question/280279208/answer/1118675237)刷完了,语言主要是Java

欢迎关注俺的力扣 [csjue](https://leetcode-cn.com/u/csjue/)

### Q1两数之和 

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。
难度 简单
示例:
给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]

~~~java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int [] answer = new int[2];
        for(int i = 0;i<nums.length;i++){
            for(int j = i+1;j<nums.length;j++){
                if(nums[i]+nums[j]==target){
                    answer[0] = i;
                    answer[1] = j;
                    return answer;
                }
            }
        }
        return answer;
    }
}
~~~
这题也很容易想到n2的解法。竟然也击败了43%

~~~java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int [] answer = new int[2];
        HashMap<Integer,Integer> m = new HashMap<>();
        for(int i = 0;i<nums.length;i++){
            if(m.get(target-nums[i])!=null){
                answer[0] = m.get(target-nums[i]);
                answer[1] = i;
                return answer;
            }
            else{
                m.put(nums[i],i);
            }
        }
        return answer;
    }
}
~~~
改用map来写，map存nums[i]和位置。复杂度n。击败99%

### Q387字符串中的第一个唯一字符
给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。
难度 简单
示例：
s = "leetcode"
返回 0
s = "loveleetcode"
返回 2

~~~java
class Solution {
    public int firstUniqChar(String s) {
        if(s.length()==1)
            return 0;
        for(int i = 0;i<s.length();i++){
            int flag = 1;
            for(int j = 0;j<s.length();j++){
                if(i!=j&&s.charAt(i) == s.charAt(j)){
                    flag = 0;
                    break;
                }
            }
            if(flag==1)
                return i;
        }
        return -1;
    }
}
~~~

随便写了个n2的算法，击败8.25%

改用HaspMap
~~~java
class Solution {
    public int firstUniqChar(String s) {
        HashMap<Character,Integer> count = new HashMap<>();
        for(int i = 0;i < s.length();i++){
            count.put(s.charAt(i),count.getOrDefault(s.charAt(i),0)+1);
        }
        for(int i = 0;i<s.length();i++){
            if(count.get(s.charAt(i))==1)
                return i;
        }
        return -1;
    }
}
~~~
之前没怎么用过这个方法
default V getOrDefault(Object key, V defaultValue) 
返回到指定键所映射的值，或 defaultValue如果此映射包含该键的映射。  

这个n的算法，没想到才击败29%

看了下最快的
~~~java
class Solution {
    public int firstUniqChar(String s) {
        int res = Integer.MAX_VALUE;
        for (char ch = 'a'; ch <= 'z'; ch++) {
            int index = s.indexOf(ch);
            if ( index!= -1 && index == s.lastIndexOf(ch)) {
                res = Math.min(res, index);
            }
        }
        return res == Integer.MAX_VALUE ? -1 : res;
    }
}
~~~
有点投机啊，赌都在a~z中。

前面的算法，基本都像上者，笃定是a~z中

### Q2两数相加
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。
如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。
您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
难度 中等
示例：
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807

之前用C语言解出来过。这一题的通过率只有40%+，我还在想：就这？

接下来一小时，力扣教育了我（也就交了十一次

~~~java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode p1 = l1,p2=l2,pFro = l1;
        int more=0;
        while(p1!=null&&p2!=null){
            p1.val = p1.val + p2.val +more;
            more = p1.val/10;
            p1.val%=10;
            pFro = p1;
            p1 = p1.next;
            p2 = p2.next;
        }
        if(p1==null&&p2!=null){
            pFro.next = p2;
            p2.val = p2.val+more;
            more = p2.val/10;
            p2.val%=10;
            while(more!=0){
                if(p2.next!=null)
                    p2.next.val += more;
                else if(p2.next==null){
                    p2.next = new ListNode(more);
                }
                more = p2.next.val/10;
                p2.next.val%=10;
                p2=p2.next;
                
            }
            
            return l1;
        }
        else if(p1!=null&&p2==null){
            p1.val = p1.val+more;
            more = p1.val/10;
            p1.val%=10;
            while(more != 0){
                if(p1.next!=null)
                    p1.next.val += more;
                else{
                    p1.next = new ListNode(more); 
                }
                more = p1.next.val/10;
                p1.next.val%=10;
                p1 = p1.next;
            }
           
            return l1;
        }
        else{
            if(more !=0){
                pFro.next = new ListNode(more);
            }
            return l1;
        }
    }
}
~~~
思路其实很简单。第一个递归很容易就写出来了。但是递归完，有三种情况，l1长，l2长，l1 l2一样长。前两种，只要把next改一下就好了。想到这我就交了一次

结果没有考虑。 1->9;9;这种情况，然后又稍微改了一下。

又没有考虑9->9->9;1;这种情况...

期间由于魔改代码，多次改错，有时连一个例子都没过。

最后写出来，击败99% 

### Q19删除链表的倒数第N个节点
给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
难度 中等
示例：
给定一个链表: 1->2->3->4->5, 和 n = 2.
当删除了倒数第二个节点后，链表变为 1->2->3->5.
说明：
给定的 n 保证是有效的。

进阶：
你能尝试使用一趟扫描实现吗？


这个题之前应该是写过类似的，数据结构笔试的时候好像有个删除倒二个节点，相比这个要简单得多，只要三个指针就搞定了。

直接尝试一趟扫描。用四个指针，f2为删除的那个点，f1之前那个，f3之后那个。l为最后一个节点。有点类似快慢指针，只要f3比l慢n-2个就可以了 
但是就是四个指针法只能应付比较长的数组

我们就得解决另外三种情况。
1：f1 null  f2  不为null  f3不为null
2：f1 null  f2  null      f3不为null
3: f1 null  f2  null      f3 null
只要稍微调几次参数，人肉跑一遍代码，基本就能调出来。

~~~java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode f1=null,f2=null,f3=null, l = head;
        if(n==1){
            if(l.next==null)
                return null;
            for(int i = 0;l.next!=null;l = l.next){
                f1 = l;
            }
            f1.next = null;
            return head;
        }
        for(int i = 0;l.next!=null;i++,l=l.next){
            if(i>=n-2){
                if(f3 == null){
                    f3 = head.next;
                    f2 = head;
                }
                else{
                    f1 = f2;
                    f2 = f3;
                    f3 = f3.next;
                }
            }
        }
        if(f1!=null)
            f1.next = f3;
        else if(f2!=null&&f3!=null)
            return f3;
        else if(f3!=null)
            return f3;
        return head;
    }
}
~~~
击败100%
### Q61旋转链表
给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。
难度 中等
示例 1:
输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL
示例 2:
输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL

其实就是先把链表弄成环，然后再找到新的头。

算准新头的位置很重要。 1-2-3-4-5，k=1时 头是5，k=2时 头是4。可以看出来 头的位置大概就是length - k%length 

~~~java
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        ListNode last = head;
        if(k==0) return head;
        if(head==null) return head;  
        int num = 1;
        while(last.next!=null){
            last = last.next;
            num++;
        }
        last.next = head;
        int pos = k%num;
        ListNode p = head,realHead;
        int i = 1;      
        while(i<num- pos){
            p = p.next;
            i++;
        }
        realHead = p.next;
        p.next =null;
        return realHead;
    }
}
~~~

写的时候 头的位置一直没调好，花了一二十分钟调参。
击败89%

### Q206反转链表
反转一个单链表。
难度中等
示例:
输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
进阶:
你可以迭代或递归地反转链表。你能否用两种方法解决这道题？

~~~java
class Solution {
    public ListNode reverseList(ListNode head) {
        if(head == null) return null;
        ListNode pre = null, now = head,next = head.next;
       
        while(now!=null){
            now.next = pre;
            pre = now;
            now = next;
            next = (next!=null)?next.next:null;
        }
        return pre;
    }
}
~~~

这题算是比较常规的一道题，只要有三个指针 pre now next，把now的next修改一下 每次向前移一下就好了。
击败100%

### Q25K个一组翻转链表
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。
k 是一个正整数，它的值小于或等于链表的长度。
如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。
难度 困难
示例：
给你这个链表：1->2->3->4->5
当 k = 2 时，应当返回: 2->1->4->3->5
当 k = 3 时，应当返回: 3->2->1->4->5

看到这题是困难，第一次做就直接跳过了。后来仔细一想，其实并不困难。

就只要两个指针加一个栈就好了。头指针指向k个之前那个，尾指针指向k个之后的那个，然后把中间的全塞进栈里。

栈塞满后，就把里面的东西一次加在链上。

特殊情况
可能栈塞不满（tflag）：直接退出即可
需要创立一个头节点。
要修改最后一个节点的next为null（防止成环）

~~~java
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
        Stack<ListNode> s = new Stack<>();
        ListNode first,last = head;
        first = new ListNode(0);
        first.next = head;
        head = first;
        int flag = 0,tflag = 0;
        while(last!=null){
            flag = 0;
            for(int i = 0;i<k;i++){
                if(last==null){
                    flag = 1;
                    break;
                }
                s.push(last);
                last = last.next;
            }
            if(flag == 1&& s.size()!=k){
                tflag = 1;
                break;
            }
            while(s.empty()==false){
                first.next = s.pop();
                first = first.next;
            }
            first.next=last;
        }
        if(tflag == 0)
            first.next = null;
        return head.next;
    }
}
~~~

这个困难的题其实通过率也有60%

竟然一发就搞定了。挺奇怪的是，按理用栈效率要递归高，竟然才击败6%

### Q138复制带随机指针的链表
给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。
要求返回这个链表的 深拷贝。 
我们用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：
val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。
难度 中等

示例 1：
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
示例 2：
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
示例 3：
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]
示例 4：
输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。

提示：
-10000 <= Node.val <= 10000
Node.random 为空（null）或指向链表中的节点。
节点数目不超过 1000 。

通过率只有56%
刚看这题感觉特别麻烦，但是也想到了用hashmap来做。不过突然犹豫了下，map存的到底是新的节点，还是旧的地址。

过了一天再来做的。map存的应该是地址。所以就很简单了，只要每创立一个节点，就在hashmap存下原节点和新链的节点，按照next的顺序遍历（顺带把random建立出来），每次检查hashmap里有没有对应的，没有就创建放进map里
~~~java
class Solution {
    public Node copyRandomList(Node head) {
        HashMap<Node,Node> map = new HashMap<Node,Node>();
        Node first=null,t1=null,t2=null,ran=null,last=null,h1 = head;
        int count = 0;
        while(h1 != null){
            if(map.containsKey(h1)==false){
                t2 = new Node(h1.val);
                map.put(h1,t2);
                if(count == 0){
                    count++;
                    first = t2;
                }
            }
            else{
                t2 = map.get(h1);    
            }
            if(t1!=null)
                t1.next = t2;
            if(h1.random!=null&&map.containsKey(h1.random)==false){
                ran = new Node(h1.random.val);
                map.put(h1.random,ran);
                t2.random = ran;
            }
            else if(h1.random!=null){
                t2.random = map.get(h1.random);    
            }
            h1 = h1.next;
            t1 = t2;
        }
       return first;
    }
}
~~~
击败100%

### Q3无重复字符的最长子串
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。
难度 中等

示例 1:
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。

自己只能想到n2的暴力算法。看到官方的题解

利用窗口（两个指针 first last）来解。建立一个HashMap存下某个字符最后一次出现的位置。first指向第一个，last一直往后走。遇到重复的字符，若在窗口内，将first移到记录位置的下一个，将窗口大小改变。

~~~java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if(s.length()==0)
            return 0;
        HashMap<Character,Integer> map = new HashMap<>();
        int f=0,l=0,length = 1,maxLength = 1;
        map.put(s.charAt(f),f);
        for(int i=1;i<s.length();i++){
            int pos = map.getOrDefault(s.charAt(i),-1);
            map.put(s.charAt(i),i);
            if(pos<f){
                //map.put(s.charAt(i),i);
                l = i;
                length = l-f+1;
                maxLength=(length>maxLength)?length:maxLength;
            }
            else{
                f = pos+1;
                l = i;
                length = l-f+1;
                maxLength=(length>maxLength)?length:maxLength;
            }
        }
        return maxLength;
    }
}
~~~
击败69%

### Q11盛最多水的容器
给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
说明：你不能倾斜容器，且 n 的值至少为 2。
图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

难度 中等

示例：
输入：[1,8,6,2,5,4,8,3,7]
输出：49

去年就尝试过这个题，放弃了。
今天自己写了一遍，写的时候没用max min函数，写的特别复杂。用了四个指针来写，搞的特别复杂。

借鉴了下别人的。其实就是两指针，当哪边是短板时，就往中间移。（证明：长为x+1的底一定比长为x的底长，如果高没有变大，水一定比原来少。所以一定是移短板才有可能增高。

~~~java
class Solution {
    public int maxArea(int[] height) {
        int left=0,right=height.length-1,ans=Math.min(height[left],height[right])*(right-left);
        while(left<right){
            ans = Math.max(Math.min(height[left],height[right])*(right-left),ans);
            if(height[left]>height[right]) --right;
            else ++left;
        }
        return ans;
    }
}
~~~
击败67%

### Q15三数之和
给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
注意：答案中不可以包含重复的三元组。

难度 中等

示例：
给定数组 nums = [-1, 0, 1, 2, -1, -4]，
满足要求的三元组集合为：
[
  [-1, 0, 1],
  [-1, -1, 2]
]

这题一开始除了 排序+暴力 没啥特别的思路。看到题解里的标题都写着 排序+双指针，就大概懂了，这题没有特别好的解法。

先利用java自带的排序，然后for-each遍历（一个指针），期间另外两个指针往中间走。

但没写好，写成了三个for，不出意外挂了。仔细想了下。不应该这么写，应该是像上一题的短板问题 >0:则f3过大 <0：则f2过大。这样可以进一步优化。

还有个比较棘手的就是不能有重复的三元组。应该每次找到一组，就得让f2 f3往前走。（仔细想想应该让f2 f3一直都跳过重复的数字，可以提高效率）

~~~java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> l = new ArrayList<>();
        for(int f1 = 0;f1<nums.length-2;){
            int f2 = f1+1,f3 = nums.length-1;
            while(f2<f3){
                if(nums[f1]+nums[f2]+nums[f3]==0){
                    List<Integer> tempt = new ArrayList<>();
                    tempt.add(nums[f1]);
                    tempt.add(nums[f2]);
                    tempt.add(nums[f3]);
                    l.add(tempt);
                    int numf2 = nums[f2], numf3 = nums[f3];
                    while(f2<f3&&numf2 == nums[f2])
                        f2++;
                    while(f3>f2&&numf3==nums[f3])
                        f3--;
                }
                else{
                    while(f2<f3&&nums[f1]+nums[f2]+nums[f3]<0)
                        f2++;
                    while(f3>f2&&nums[f1]+nums[f2]+nums[f3]>0)
                        f3--;
                }
                
            }
            int pref1 = f1;
            while(f1<nums.length&&nums[f1]==nums[pref1])
                f1++;
        }
        return l;
    }
}
~~~
击败84%

### Q16最接近的三数之和
给定一个包括 n 个整数的数组 nums 和 一个目标值 target。找出 nums 中的三个整数，使得它们的和与 target 最接近。返回这三个数的和。假定每组输入只存在唯一答案。

难度 中等

示例：
输入：nums = [-1,2,1,-4], target = 1
输出：2
解释：与 target 最接近的和是 2 (-1 + 2 + 1 = 2) 。

这一题的思路跟上题几乎一样。所以我就直接cv了。魔改之后，却总是过不了。

区别还是有些的。target相等/逼近。相等很容易判断出来。而哪个接近总是要麻烦一点，每次移指针基本都要判断一次，经常还要用abs函数 
~~~java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int ans = nums[0] + nums[1] + nums[2];
        Arrays.sort(nums);
        for (int f1 = 0; f1 < nums.length - 2; f1++) {
            int f2 = f1 + 1, f3 = nums.length - 1;
            while (f2 < f3) {
                if (nums[f1] + nums[f2] + nums[f3] == target) {
                    return target;
                }
                if (Math.abs(nums[f1] + nums[f2] + nums[f3] - target) < Math.abs(ans - target))
                    ans = nums[f1] + nums[f2] + nums[f3];

                while (f2 < f3 && nums[f1] + nums[f2] + nums[f3] < target) {
                    f2++;

                    if (f2 < f3 && Math.abs(nums[f1] + nums[f2] + nums[f3] - target) < Math.abs(ans - target))
                        ans = nums[f1] + nums[f2] + nums[f3];
                }
                while (f3 > f2 && nums[f1] + nums[f2] + nums[f3] > target) {
                    f3--;
                    if (f2 < f3 && Math.abs(nums[f1] + nums[f2] + nums[f3] - target) < Math.abs(ans - target))
                        ans = nums[f1] + nums[f2] + nums[f3];
                }
            }

        }
        return ans;
    }
}
~~~
只击败了32%，可能是指针移动的部分，优化还不够。

仿照官方题解优化了一下，竟然只击败了19%，反而还降低了。

然后发现自己少了相等的情况。

~~~java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int ans = nums[0] + nums[1] + nums[2];
        Arrays.sort(nums);
        for (int f1 = 0; f1 < nums.length - 2; ) {
            int f2 = f1 + 1, f3 = nums.length - 1;
            while (f2 < f3) {
                int sum = nums[f1] + nums[f2] + nums[f3];
                if (Math.abs(sum - target) < Math.abs(ans - target))
                    ans = sum;
                if(sum<target)
                {
                    int f = f2;
                    while(f<f3&&nums[f]==nums[f2])
                        f++;
                    f2 = f;
                }
                else
                    {
                        int f = f3;
                        while(f>f2&&nums[f]==nums[f3])
                            f--;
                        f3 = f;
                    }
            }
            int f = f1;
            while(f<nums.length&&nums[f]==nums[f1])
                f++;
            f1 = f;
        }
        return ans;
    }
}
~~~

~~~java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int ans = nums[0] + nums[1] + nums[2];
        Arrays.sort(nums);
        for (int f1 = 0; f1 < nums.length - 2; ) {
            int f2 = f1 + 1, f3 = nums.length - 1;
            while (f2 < f3) {
                int sum = nums[f1] + nums[f2] + nums[f3];
                if(sum == target)
                    return sum;
                if (Math.abs(sum - target) < Math.abs(ans - target))
                    ans = sum;
                if(sum<target)
                {
                    int f = f2;
                    while(f<f3&&nums[f]==nums[f2])
                        f++;
                    f2 = f;
                }
                else
                    {
                        int f = f3;
                        while(f>f2&&nums[f]==nums[f3])
                            f--;
                        f3 = f;
                    }
            }
            int f = f1;
            while(f<nums.length&&nums[f]==nums[f1])
                f++;
            f1 = f;
        }
        return ans;
    }
}
~~~
加了一行代码，击败96%。没想到特殊情况竟然对整体影响这么大（Amadha定律

### Q26删除排序数组中的重复项
给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

难度 简单

示例 1:
给定数组 nums = [1,1,2], 
函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
你不需要考虑数组中超出新长度后面的元素。

示例 2:
给定 nums = [0,0,1,1,1,2,2,3,3,4],
函数应该返回新的长度 5, 并且原数组 nums 的前五个元素被修改为 0, 1, 2, 3, 4。
你不需要考虑数组中超出新长度后面的元素。

看到是简单，还以为很简单。想了一会也没有思路。看了评论里的解答，才反应过来，就是双指针问题。

j一直向后走，而i记录有效的位置。
自己写了下，由于不是很简洁，就借鉴了
~~~java
class Solution {
    public int removeDuplicates(int[] nums) {
        if(nums==null)
            return 0;
        if(nums.length==1)
            return 1;
        int i=0,j=1;
        while(j<nums.length)
            if(nums[i] == nums[j]){
                j++;
            }else{
                i++;
                nums[i] = nums[j];
                j++;
            }
            return i+1;
    }
}
~~~
击败98%

### Q42接雨水
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。
上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 感谢 Marcos 贡献此图。

难度 困难

示例:
输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6

去年就尝试写这个题，放弃了。这题没啥思路去写，暴力也不是很好写。

看了下题解，里面提到了找到最高值。这个就是关键点。只要你找到了最高值，这一题就迎刃而解了。从两边向最高值靠拢，每次找到的最高值，就是装水的板子。（因为有中间最高的板兜底）。所以每次向中间靠拢，如果有比最高值大就更新，小就增加水。

~~~java
class Solution {
    public int trap(int[] height) {
        int max = 0,water = 0;
        for(int i=0;i<height.length;i++)
            if(height[max]<height[i])
                max = i;
        int lmax = 0,rmax = 0;
        for(int i = 0;i<max;i++){
            if(lmax>height[i])
                water+=(lmax-height[i]);
            else{
                lmax = height[i];
            }
        }
        for(int i = height.length-1;i>max;i--){
            if(rmax>height[i])
                water+=(rmax-height[i]);
            else{
                rmax = height[i];
            }
        }
        return water;
    }
}
~~~
击败99%

### Q121买卖股票的最佳时机
给定一个数组，它的第 i 个元素是一支给定股票第 i 天的价格。
如果你最多只允许完成一笔交易（即买入和卖出一支股票一次），设计一个算法来计算你所能获取的最大利润。
注意：你不能在买入股票前卖出股票。

难度 简单

示例 1:
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2:
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。

是一个究极简单的题目。刚开始把自己绕晕了，竟然交了五发还没过。

其实就是遍历一次，然后记住自己能买到的最低价格，然后拿当前价格减去最低价格就可以获得利润，拿去与最大利润相比较。
~~~java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length==0)    return 0;
        int ans = 0,lmin = prices[0];
        for(int i = 1;i<prices.length;i++){
            if(ans<prices[i]-lmin)
                ans = prices[i]-lmin;
            if(lmin>prices[i])
                lmin = prices[i];
        }
        return ans;
    }
}
~~~
击败98%

### Q209长度最小的子数组
给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的 连续 子数组，并返回其长度。如果不存在符合条件的子数组，返回 0。

难度中等

示例：
输入：s = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。

进阶：
如果你已经完成了 O(n) 时间复杂度的解法, 请尝试 O(n log n) 时间复杂度的解法。

我感觉这个进阶比较迷。

题目不是很难，一个滑窗问题，两个针就可以解决。后面的针每次往后移一下，就检测前面针可以移几次。

但是被特例搞了两次，数组大小为0时，数组的和都没有目标大时

用数组时，一定要注意有没有越界。java在这方面还比较好，有空指针报错

~~~java
class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        if(nums.length==0) return 0;
        if(nums.length==1)  return 1;
        int f=0,l =0,sum=nums[0],minLength=1;
        while(sum<s){
            l++;
            minLength++;
            if(l==nums.length) return 0;
            sum+=nums[l];
        }
        while(l<nums.length){
            while(sum-nums[f]>=s){
                sum-=nums[f];
                f++;
            }
            if(l-f+1<minLength)
                    minLength = l-f+1;
            l++;
            if(l<nums.length)
                sum+=nums[l];
        }
        return minLength;
    }
}
~~~

### Q141环形链表
给定一个链表，判断链表中是否有环。
为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。

难度 简单

示例 1：
输入：head = [3,2,0,-4], pos = 1
输出：true
解释：链表中有一个环，其尾部连接到第二个节点。


示例 2：
输入：head = [1,2], pos = 0
输出：true
解释：链表中有一个环，其尾部连接到第一个节点。


示例 3：
输入：head = [1], pos = -1
输出：false
解释：链表中没有环。

进阶：
你能用 O(1)（即，常量）内存解决此问题吗？

一看到这题就想到了HashMap，这题跟之前一个原地复制 随机指针的题有很多共同点。
~~~java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null) return false;
        HashMap<ListNode,Integer> m = new HashMap<>();
        while(head.next!=null){
            m.put(head,1);
            if(m.containsKey(head.next)==true)
                return true;
            head =  head.next;
        }
        return false;
    }
}
~~~
没想到效率特别低，只击败16%

看了下题解，有个龟兔赛跑的视频，还挺有意思的。兔子一次走两个，乌龟一次走一个。理论上他们永远不会相遇，但是如果赛道变了，有个环后，他们可能就会相遇。
~~~java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if(head==null) return false;
        ListNode fast=head,low=head;
        while(fast!=null&&fast.next!=null){
            fast = fast.next.next;
            low = low.next;
            if(fast==low)
                return true;
        }
        return false;
    }
}
~~~
击败100%

### Q202快乐数
编写一个算法来判断一个数 n 是不是快乐数。
「快乐数」定义为：对于一个正整数，每一次将该数替换为它每个位置上的数字的平方和，然后重复这个过程直到这个数变为 1，也可能是 无限循环 但始终变不到 1。如果 可以变为  1，那么这个数就是快乐数。
如果 n 是快乐数就返回 True ；不是，则返回 False 。

难度 简单 

示例：
输入：19
输出：true
解释：
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

这个题虽然是简单，但也没想出来啥方法。一开始就放弃了暴力法，觉得复杂度太高了。然后就一直想不出来。

其实沿着暴力法走下去，可能就能想出来了。 要判断true，很简单，只要为1就可以了。但是false有点麻烦，因为false一直不等于1。那怎么判断呢？ 无限循环！！！ 如果有环，其实就跟上一题一回事。只要用快慢指针就可以解决了
~~~java
class Solution {
    public boolean isHappy(int n) {
        int slow = fun(n);
        int fast = fun(n);
        fast = fun(fast);
        while(slow != fast){
            slow = fun(slow);
            fast = fun(fast);
            fast = fun(fast);
            if(fast == 1)
                return true;
        }
        if(fast == 1)
            return true;
        return false;
    }
    
    public int fun(int x){
        int sum = 0;
        while(x!=0){
            sum+=((x%10)*(x%10));
            x/=10;
        }
        return sum;
    }
}
~~~
击败100%

### Q876链表的中间结点
给定一个带有头结点 head 的非空单链表，返回链表的中间结点。
如果有两个中间结点，则返回第二个中间结点。

难度简单

示例 1：
输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
示例 2：
输入：[1,2,3,4,5,6]
输出：此列表中的结点 4 (序列化形式：[4,5,6])
由于该列表有两个中间结点，值分别为 3 和 4，我们返回第二个结点。

提示：
给定链表的结点数介于 1 和 100 之间。

做完前面两题，这题就是乱杀。就是快慢指针
~~~java
class Solution {
    public ListNode middleNode(ListNode head) {
        if(head==null) return null;
        if(head.next==null) return head;
        ListNode slow = head.next;
        ListNode fast = head.next.next;
        while(fast!=null){
            fast = fast.next;
            if(fast!=null){
                fast = fast.next;
                slow = slow.next;
            }
        }
        return slow;
    }
}
~~~
击杀100%

### Q56合并区间
给出一个区间的集合，请合并所有重叠的区间。

难度 中等 

示例 1:
输入: intervals = [[1,3],[2,6],[8,10],[15,18]]
输出: [[1,6],[8,10],[15,18]]
解释: 区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
示例 2:
输入: intervals = [[1,4],[4,5]]
输出: [[1,5]]
解释: 区间 [1,4] 和 [4,5] 可被视为重叠区间。
注意：输入类型已于2019年4月15日更改。 请重置默认代码定义以获取新方法签名。
提示：
intervals[i][0] <= intervals[i][8]

其实不是很复杂。先按intervals[x][0]排序。窗口，记住左边，跟右边。如果下一个，左边界在我们范围内，而右边界比我们大，就更新。左边界不在我们范围内，就把窗口后移。

只是我不太会写数组排序算法、不太懂lambada函数怎么写。还有最后需要拷贝数组的函数也没用过。然后中间把变量名搞混了。导致找了半天bug（大概1h），第一次自己在本机里写main函数，一点点调试（等俺有钱了就冲会员 （可能有钱就不需要刷这个了。

~~~java
class Solution {
    public int[][] merge(int[][] intervals) {
        if(intervals.length==0) return intervals;
        Arrays.sort(intervals,(v1, v2) -> v1[0] - v2[0]);
        int left=intervals[0][0],right =intervals[0][9];
        int[][] ans = new int[intervals.length][10];
        int count = 0;
        int flag = 0;
        for(int i = 0; i<intervals.length; i++){
            flag = 0;
            if(intervals[i][0]<=right&&intervals[i][11]>right){
                right = intervals[i][12];
            }else if(intervals[i][0]>right){
                ans[count][0] = left;
                ans[count][13] = right;
                left = intervals[i][0];
                right = intervals[i][14];
                count++;
                flag = 1;
            }
            flag = 0;
        }if(flag == 0){
            ans[count][0] = left;
            ans[count][15] = right;
            count++;
        }
        return Arrays.copyOf(ans,count);
    }
}
~~~
击败92%

### Q6Z字形变换
将一个给定字符串根据给定的行数，以从上往下、从左到右进行 Z 字形排列。
比如输入字符串为 "LEETCODEISHIRING" 行数为 3 时，排列如下：
L   C   I   R
E T O E S I I G
E   D   H   N
之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："LCIRETOESIIGEDHN"。
请你实现这个将字符串进行指定行数变换的函数：
string convert(string s, int numRows);

难度中等

示例 1:
输入: s = "LEETCODEISHIRING", numRows = 3
输出: "LCIRETOESIIGEDHN"
示例 2:
输入: s = "LEETCODEISHIRING", numRows = 4
输出: "LDREOEIIECIHNTSG"
解释:
L     D     R
E   O E   I I
E C   I H   N
T     S     G

题目讲的花里胡哨的，我还以为要输出解释这样的。就是一会正、一会反就好了。我们就创个数组，一会正序加到各个数组，一会反向。

~~~java
class Solution {
    public String convert(String s, int numRows) {
        if(numRows == 1||s.length()==1) return s;
        String[] ans = new String[numRows];
        for(int i = 0; i < numRows; i++){
            ans[i] = "";
        }
        int pos = 0, change = 0;
        for (int i = 0; i < s.length(); i++) {
            ans[pos]+=s.charAt(i);
            if(change==1){
                if(pos==0){
                    change = 0;
                    pos++;
                }else{
                    pos--;
                }
            }else{
                if(pos==numRows-1){
                    change=1;
                    pos--;
                }else{
                    pos++;
                }
            }
        }
        String re = new String();
        for (String s1 : ans) {
            re += s1;
        }
        //System.out.println(ans[0]);
        return re;
    }
}
~~~
但没想到的是我这个n的算法 只击败了17%

改用StringBuilder
~~~java
class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1 || s.length() == 1)
            return s;
        StringBuilder[] ans = new StringBuilder[numRows];
        for(int i=0;i<ans.length;i++){
            ans[i]=new StringBuilder();
        }
        int pos = 0, change = 0;
        for (int i = 0; i < s.length(); i++) {
            ans[pos].append(s.charAt(i));
            if (change == 1) {
                if (pos == 0) {
                    change = 0;
                    pos++;
                } else {
                    pos--;
                }
            } else {
                if (pos == numRows - 1) {
                    change = 1;
                    pos--;
                } else {
                    pos++;
                }
            }
        }
        String re = new String();
        for (StringBuilder s1 : ans) {
            re += s1;
        }
        // System.out.println(ans[0]);
        return re;
    }
}
~~~
击败44%
看了下最快解，他用的是char数组，其他区别应该不大。

### Q14最长公共前缀
编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串 ""。

难度 简单

示例 1:
输入: ["flower","flow","flight"]
输出: "fl"
示例 2:
输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:
所有输入只包含小写字母 a-z 。

简单的遍历。
~~~java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if(strs.length==0) return "";
        int x = -1;
        char a;
        int flag = 0;
        for(int i = 0;i<strs[0].length();i++){
            a= strs[0].charAt(i);
            for(int j = 0;j<strs.length;j++){
                if(strs[j].length()==i)
                    {flag = 1;break;}
                if(strs[j].charAt(i)!=a){
                    flag = 1;
                    break;
                }
            }
            if(flag == 1)
                break;
            x = i; 
        }
        if(x == -1) return "";
        return strs[0].substring(0,x+1);
    }
}
~~~
击败87%

### Q763划分字母区间
字符串 S 由小写字母组成。我们要把这个字符串划分为尽可能多的片段，同一个字母只会出现在其中的一个片段。返回一个表示每个字符串片段的长度的列表。

难度中等

示例 1：
输入：S = "ababcbacadefegdehijhklij"
输出：[9,7,8]
解释：
划分结果为 "ababcbaca", "defegde", "hijhklij"。
每个字母最多出现在一个片段中。
像 "ababcbacadefegde", "hijhklij" 的划分是错误的，因为划分的片段数较少。

看了官方题解，贪心算法。每次找到最后一个相同字母的地址。窗口一定得包含最后一个地址。

~~~java
class Solution {
    public List<Integer> partitionLabels(String S) {
        int left=0,right=0;
        List<Integer> l = new ArrayList<>();
        for(int i = 0;i<S.length();i++){
            for(int j =S.length()-1;j>=left;j--){
                if(S.charAt(i)==S.charAt(j)){
                    if(i>right){
                    l.add(right-left+1);
                    right = left = i;
                }if(j>right){    
                        right = j;
                        break;
                    }
                }
            }
        }
        l.add(right-left+1); 
        return l;
    }
}
~~~
击杀5%？？？？

仿照官方题解改了一下。这一题跟买卖股票那题还是有一丝相似。创建数组记录字母的最后一个。然后遍历数组，一直更新窗口大小（记最大的），当遍历到窗口最后一个，就完成了一个子字符串。

~~~java
class Solution {
    public List<Integer> partitionLabels(String S) {
        List<Integer> l = new ArrayList<>();
        int[] pos = new int[26];
        for(int i = S.length()-1;i>=0;i--)
            pos[S.charAt(i)-'a']=(pos[S.charAt(i)-'a']>i)?pos[S.charAt(i)-'a']:i;
        int j = 0,last = pos[S.charAt(j)-'a'];
        int preJ = 0;
        while(j!=S.length()){
            if(last<pos[S.charAt(j)-'a'])
                last = pos[S.charAt(j)-'a'];
            if(j==last){
                l.add(j-preJ+1);
                preJ = j+1;
            }
            j++;
        }
        return l;
    }
}
~~~
击败78%

### Q7整数反转
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

难度 简单

示例 1:
输入: 123
输出: 321
示例 2:
输入: -123
输出: -321
示例 3:
输入: 120
输出: 21
注意:
假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

看到这题就在想：就这？ （没看到注意

一下子就写出来了。结果来了个1534236469，会暴int。还有点懵。看了下评论，用long算最后在返回int
~~~java
class Solution {
    public int reverse(int x) {
        long ans = 0;
        while(x!=0){
            ans*=10;
            ans+=x%10;
            x/=10;
        }
        return (int)ans==ans?(int)ans:0;
    }
}
~~~
击败100%

### Q8字符串转换整数 (atoi)
请你来实现一个 atoi 函数，使其能将字符串转换成整数。
首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。接下来的转化规则如下：
如果第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字字符组合起来，形成一个有符号整数。
假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成一个整数。
该字符串在有效的整数部分之后也可能会存在多余的字符，那么这些字符可以被忽略，它们对函数不应该造成影响。
注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换，即无法进行有效转换。
在任何情况下，若函数不能进行有效的转换时，请返回 0 。

提示：
本题中的空白字符只包括空格字符 ' ' 。
假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

难度 中等

示例 1:
输入: "42"
输出: 42
示例 2:
输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
示例 3:
输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
示例 4:
输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
示例 5:
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231)。

知道这题肯定会爆int，用了long。没想到连long也会爆，看他的数据，感觉搞不好long long都会暴。
只能每次计算判断有没有爆了
~~~java
class Solution {
    public int myAtoi(String str) {
        if(str.length()==0) return 0; 
        long ans = 0,preAns = 0;
        int pos = 0;
        int minus = 0;
        while(pos<str.length()&&str.charAt(pos)==' '){
            pos++;
        }
            ans = -ans;
        if(pos==str.length())
            return 0;
        if(str.charAt(pos)=='-'){
            minus = 1;
            pos++;
        }
        else if(str.charAt(pos)=='+')
            pos++;
        else if(str.charAt(pos)>='0'&&str.charAt(pos)<='9')
            ;
        else 
            return 0;
        while(pos!=str.length()&&str.charAt(pos)>='0'&&str.charAt(pos)<='9'){
            preAns = ans;
            ans*=10;
            ans+=(str.charAt(pos)-'0');
            if(ans<preAns){
                if(minus==1)
                    return Integer.MIN_VALUE;
                return Integer.MAX_VALUE;
            }
            pos++;
        }
        if(minus==1)
        if((int)ans==ans)
            return (int)ans;    
        if(minus==1)
            return Integer.MIN_VALUE;
        return Integer.MAX_VALUE;
    }
}
~~~
击败99%

### Q9回文数
判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

难度 简单

示例 1:
输入: 121
输出: true
示例 2:
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
示例 3:
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
进阶:
你能不将整数转为字符串来解决这个问题吗？

这个题还是比较简单的。直接转换成string做
~~~java
class Solution {
    public boolean isPalindrome(int x) {
        if(x<0) return false;
        String s = Integer.toString(x);
        int i = 0,j = s.length()-1;
        while(i<j){
            if(s.charAt(i)!=s.charAt(j))
                return false;
            i++;
            j--;
        }
        return true;
    }
}
~~~
只击败了27%

### Q415字符串相加
给定两个字符串形式的非负整数 num1 和num2 ，计算它们的和。

难度 简单

提示：
num1 和num2 的长度都小于 5100
num1 和num2 都只包含数字 0-9
num1 和num2 都不包含任何前导零
你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式

本来先做的是43字符串相乘。de了半天bug，做不下去，看了官方题解。原来还要一题是415，刚好要用到我的一个子函数。就cv过来调了一下。
~~~java
class Solution {
    public String addStrings(String num1, String num2) {
        int i = num1.length() - 1, j = num2.length() - 1;
        StringBuilder ans = new StringBuilder();
        int ci = 0;
        while (i >= 0 && j >= 0) {
            if (num1.charAt(i)-'0' + num2.charAt(j)-'0' + ci <= 9) {
                ans.append(num1.charAt(i)-'0' + num2.charAt(j)-'0' + ci);
                ci = 0;
            } else {
                ans.append(num1.charAt(i)-'0' + num2.charAt(j)-'0' + ci - 10 );
                ci = 1;
            }
            i--;
            j--;
        }//System.out.println(ans.toString());
        
        while (i >= 0) {
            if (num1.charAt(i) + ci <= '9') {
                ans.append(num1.charAt(i)-'0' + ci);
                ci = 0;
            } else {
                ans.append(num1.charAt(i)-'0' + ci - 10);
                ci = 1;
            }
            i--;
        }
        while (j >= 0) {
            if (num2.charAt(j)-'0' + ci <= 9) {
                ans.append(num2.charAt(j)-'0' + ci);
                ci = 0;
            } else {
                ans.append(num2.charAt(j)-'0' + ci - 10);
                ci = 1;
            }
            j--;
        }
        if (ci == 1) {
            ans.append('1');
        }
        //System.out.println(ans.toString());
        return ans.reverse().toString();
    }
}
~~~
思路很简单，就是一位位相加看有没有超过10。击败73% 。但是也暴露了，我对char int运算不熟。期间写出了类似'0'+'1'<'2',这种代码。

还有StringBuilder.append()函数，我之前写StringBuilder.append('0'+6),我以为会加'6'，其实char与int运算，会自动转换成int，而append由于多态，是可以加int类型，他会把int再次转换成char，这却与我的期望不符合。（多态瞬间不香了。
~~~java
public class stringbuild {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        sb.append('1'+6);
        System.out.println(sb.toString());
    }
}

PS D:\daima> cd "d:\daima\" ; if ($?) { javac stringbuild.java } ; if ($?) { java stringbuild }
55
~~~

### Q43字符串相乘
给定两个以字符串形式表示的非负整数 num1 和 num2，返回 num1 和 num2 的乘积，它们的乘积也表示为字符串形式。

难度 中等

示例 1:
输入: num1 = "2", num2 = "3"
输出: "6"
示例 2:
输入: num1 = "123", num2 = "456"
输出: "56088"
说明：
num1 和 num2 的长度小于110。
num1 和 num2 只包含数字 0-9。
num1 和 num2 均不以零开头，除非是数字 0 本身。
不能使用任何标准库的大数类型（比如 BigInteger）或直接将输入转换为整数来处理。

这题应该是写到目前 我觉得特别烦的一题。

题目学过计组基本就知道思路。num1×num2，先让ans = 0；每次取出num2最右边那个数t，ans+=(num1×t)。ans向左移一位。一直重复，知道num2 = 0；

把这分成两个函数，一个函数负责加法，一个函数负责单乘。

这之中出现了很多细节的bug。比如：写单乘受加法影响，进位只考虑为1的情况。char计算多次搞错。string 在函数应该是值传递。

写bug 1min ，找bug 1h。

~~~java
class Solution {
    public String multiply(String num1, String num2) {
        if(num1.equals("0")||num2.equals("0"))  
            return "0";
        StringBuilder ans = new StringBuilder("0");
        while (num2.equals("") != true) {
            ans = add(ans, mult(num1, num2));
            num2 = num2.substring(1, num2.length());
            if (num2.equals("") != true)
                ans.append('0');
        }
        return ans.toString();
    }

    public StringBuilder add(StringBuilder num1, StringBuilder num2) {
        int i = num1.length() - 1, j = num2.length() - 1;
        StringBuilder ans = new StringBuilder();
        int ci = 0;
        while (i >= 0 && j >= 0) {
            if (num1.charAt(i) - '0' + num2.charAt(j) - '0' + ci <= 9) {
                ans.append(num1.charAt(i) - '0' + num2.charAt(j) - '0' + ci);
                ci = 0;
            } else {
                ans.append(num1.charAt(i) - '0' + num2.charAt(j) - '0' + ci - 10);
                ci = 1;
            }
            i--;
            j--;
        } // System.out.println(ans.toString());

        while (i >= 0) {
            if (num1.charAt(i) + ci <= '9') {
                ans.append(num1.charAt(i) - '0' + ci);
                ci = 0;
            } else {
                ans.append(num1.charAt(i) - '0' + ci - 10);
                ci = 1;
            }
            i--;
        }
        while (j >= 0) {
            if (num2.charAt(j) - '0' + ci <= 9) {
                ans.append(num2.charAt(j) - '0' + ci);
                ci = 0;
            } else {
                ans.append(num2.charAt(j) - '0' + ci - 10);
                ci = 1;
            }
            j--;
        }
        if (ci == 1) {
            ans.append('1');
        }
        // System.out.println(ans.toString());
        return ans.reverse();
    }

    public StringBuilder mult(String num1, String num2) {
        int y = num2.charAt(0) - '0', x = num1.length() - 1;
        StringBuilder s1 = new StringBuilder("");
        if (y == 0) {
            return new StringBuilder("0");
        }
        int ci = 0;
        while (x >= 0) {
            int ch = num1.charAt(x) - '0';
            ch *= y;
            if (ch + ci >= 10) {
                s1.append((char) ((ch + ci)%10 + '0'));
                ci = (ch + ci) / 10;
            } else {
                s1.append((char) (ch + ci + '0'));
                ci = 0;
            }
            x--;
        }
        if (ci != 0) {
            s1.append(ci);
        }
        return s1.reverse();
    }

}
~~~
只击败了33%
### Q172阶乘后的零
给定一个整数 n，返回 n! 结果尾数中零的数量。

难度 简单

示例 1:
输入: 3
输出: 0
解释: 3! = 6, 尾数中没有零。
示例 2:
输入: 5
输出: 1
解释: 5! = 120, 尾数中有 1 个零.
说明: 你算法的时间复杂度应为 O(log n) 。

这题一点都不简单把...我参考了题解。求多少个零就是求因子有多少个10，也就是求又少个5，有多少个2。我们很容易可以看出来，每出现一个5，都会有好几个2。所以其实就是求有多少个5因子。

1 2 3 4 **5** 6 7 8 9 **10** 11 12 13 14 **15** 16 17 18 19 **20** 21 22 23 24 **25**

5：1个，10：2个，15：3个，20：4个，25：6个。25/5+25/5/5，看到这，我就直接想出了个简单代码，也没验证是否正确，就直接提交了
~~~java
class Solution {
    public int trailingZeroes(int n) {
        int i = 1,count=0;
        while(n/Math.pow(5,i)!=0){
            count += n/Math.pow(5,i);
            ++i;
        }
        return count;
    }
}
~~~
击败99% 

### Q258各位相加
给定一个非负整数 num，反复将各个位上的数字相加，直到结果为一位数。

难度 简单

示例:
输入: 38
输出: 2 
解释: 各位相加的过程为：3 + 8 = 11, 1 + 1 = 2。 由于 2 是一位数，所以返回 2。
进阶:
你可以不使用循环或者递归，且在 O(1) 时间复杂度内解决这个问题吗？

又是一个明明是难题，却写着简单...

这题还挺有意思的。其实他求的东西叫做 树根 ，知乎上有个[帖子](https://www.zhihu.com/question/30972581/answer/50203344)对此进行了讨论。

通过找规律可以看出
原数: 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30
数根: 1 2 3 4 5 6 7 8 9  1  2  3  4  5  6  7  8  9  1  2  3  4  5  6  7  8  9  1  2  3 

转自力扣[题解](https://leetcode-cn.com/problems/add-digits/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by-5-7)
~~~java
class Solution {
    public int addDigits(int num) {
        return (num-1)%9+1;
    }
}
~~~
击败100%

### Q54螺旋矩阵
给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。

难度 中等

示例 1:
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
示例 2:
输入:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
输出: [1,2,3,4,8,12,11,10,9,5,6,7]

这题之前遇到过。大一C语言的时候，上机题有这题，虽然不是我的场次。当时的题目是让你 print一个1-n的螺旋正方形矩阵。跟这题几乎一样

只要四个方向转来转去，就可以写出这题，然后用一个矩阵记录哪些已经输出过了。

但是有点迷，第一个样例，总是会多输出一个4，具体原因也懒得查了。只要加入数组前多一次判断就行了。
~~~java
class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        if(matrix.length==0)    return new ArrayList<>();
        int[][] m = new int[matrix.length][matrix[0].length];
        List<Integer> l = new ArrayList<>();
        int i = 0,j = 0;
        int flag=0;
        int t = 0;
        while(true){
            if(m[i][j]!=1)
                l.add(matrix[i][j]);
            m[i][j] = 1;
            t=0;
            if(flag==0){
                if(j+1<m[0].length&&m[i][j+1]!=1){
                    j++;
                }else{
                    flag = 1;
                    t++;
                }
            }if(flag==1){
                if(i+1<m.length&&m[i+1][j]!=1){
                    i++;
                }else{
                    flag=2;
                    t++;
                }
            }if(flag==2){
                if(j-1>=0&&m[i][j-1]!=1){
                    j--;
                }else{
                    flag = 3;
                    t++;
                }
            }if(flag==3){
                if(i-1>=0&&m[i-1][j]!=1){
                    i--;
                }else{
                    flag = 0;
                    t++;
                }
            }if(t==4){
                break;
            }
        }
        return l;
    }
}
~~~
击败100%

### Q73矩阵置零
给定一个 m x n 的矩阵，如果一个元素为 0，则将其所在行和列的所有元素都设为 0。请使用原地算法。

难度 中等

示例 1:
输入: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
输出: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]
示例 2:
输入: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
输出: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
进阶:
一个直接的解决方案是使用  O(mn) 的额外空间，但这并不是一个好的解决方案。
一个简单的改进方案是使用 O(m + n) 的额外空间，但这仍然不是最好的解决方案。
你能想出一个常数空间的解决方案吗？

看到这题，我就在想 这题也配中等？ 进阶说O（mn） O（m+n）的空间复杂度。我立马就能写个O（1）的。

刚开始就写了个二十行左右，原地修改的代码。 先遍历数组，遇见0把行首 列首改成0。(我此时还在想如果在这时把整个数组改了岂不是更简单) 然后再一次遍历数组，把应该清零的清零

力扣立马用样例教育了我。0,1  1,1本来还剩一个1，但是如果你修改了行首、列首，就出问题了。（前几天在橙书算法1.5课后题也遇到一个找bug题，也是因为原地修改导致循环后续出问题

好吧，那我们第二次遍历，不修改行首行列。这样也还是过不了。毕竟存在一种情况，需要把行首/列首搞成0

那我们就最开始检查行首/列首。最后再处理行首/列首。
~~~java
class Solution {
    public void setZeroes(int[][] matrix) {
        int r= 0,c=0;
        for(int i=0;i<matrix[0].length;i++){
            if(matrix[0][i]==0){
                r = 1;
                break;
            }
        }for(int i=0;i<matrix.length;i++){
            if(matrix[i][0]==0){
                c = 1;
                break;
            }
        }for(int i = 0;i<matrix.length;i++)
            for(int j = 0;j<matrix[0].length;j++)
                if(matrix[i][j]==0){
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
        for(int i = 1;i<matrix.length;i++){
            if(matrix[i][0]==0){
                for(int j = 1;j<matrix[0].length;j++){
                    matrix[i][j] = 0;
                }
            }
        }for(int i = 1;i<matrix[0].length;i++){
            if(matrix[0][i]==0){
                for(int j = 1;j<matrix.length;j++){
                    matrix[j][i] = 0;
                }
            }
        }if(r==1){
            for(int i =0;i<matrix[0].length;i++){
                matrix[0][i] = 0;
            }
        }if(c==1){
            for(int i=0;i<matrix.length;i++)
                matrix[i][0] = 0;
        }
        return ;        
    }
}
~~~
时间击败100%，空间击败76%

### Q78子集
给定一组不含重复元素的整数数组 nums，返回该数组所有可能的子集（幂集）。
说明：解集不能包含重复的子集。

难度 中等

示例:
输入: nums = [1,2,3]
输出:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]

这题我之前做过类似的，南科大的一次acm校赛。这题应该是a题，最简单的。当时用C语言搞了半天，没搞出来。我记得我是用递归写的，特别复杂。

今天直接看题解了，先创建一个空集合[],然后遍历nums数组，先是1，然后把1放进空集合里。就获得两个集合[] [3]，然后取出2,把2 再放进[] [4]中。获得[] [5] [2] [1,2]。就这样遍历下去，就可以得到所有集合。

注意遍历集合的集合时，要把初始size记录下来，否则size一直增长，进入死循环。

~~~java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> l = new ArrayList<>();
        l.add(new ArrayList<>());
        for(int i:nums){
            int size = l.size();
            for(int j=0;j<size;j++){
                List<Integer> tempt =new ArrayList<>(l.get(j));
                tempt.add(i);
                l.add(tempt);
            }
        }return l;
    }
}
~~~
击败99%

还有一种是利用二进制位运算，00000 则表示都没有 00001 则表示有一个有，诸如此类，去穷举。

其他的 回溯法什么的没怎么看懂

### Q581最短无序连续子数组
给定一个整数数组，你需要寻找一个连续的子数组，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。
你找到的子数组应是最短的，请输出它的长度。

难度 简单

示例 1:
输入: [2, 6, 4, 8, 10, 9, 15]
输出: 5
解释: 你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
说明 :
输入的数组长度范围在 [1, 10,000]。
输入的数组可能包含重复元素 ，所以升序的意思是<=。

又是一个假简单题，感觉通过率有时比难度更可信，这题通过率就不是很高。做了半天，没有想出O（n）的算法。就直接用了排序。
~~~java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int[] numscopy = new int[nums.length];
        numscopy = Arrays.copyOf(nums,nums.length);
        Arrays.sort(nums);
        int f = -1, l=-1;
        for(int i = 0;i<nums.length;i++){
            if((f==-1||f>i)&&numscopy[i]!=nums[i])
                f = i;
            if(l<i&&numscopy[i]!=nums[i])
                l = i;
        }if(l==-1||f==-1)   return 0;
        return l-f+1;
    }

}
~~~
竟然也击败了57%

### Q945使数组唯一的最小增量
给定整数数组 A，每次 move 操作将会选择任意 A[i]，并将其递增 1。
返回使 A 中的每个值都是唯一的最少操作次数。

难度 中等

示例 1:
输入：[1,2,2]
输出：1
解释：经过一次 move 操作，数组将变为 [1, 2, 3]。
示例 2:
输入：[3,2,1,2,1,7]
输出：6
解释：经过 6 次 move 操作，数组将变为 [3, 4, 1, 2, 5, 7]。
可以看出 5 次或 5 次以下的 move 操作是不能让数组的每个值唯一的。
提示：
0 <= A.length <= 40000
0 <= A[i] < 40000

我一想，这不就典型的map题吗

~~~java
class Solution {
    public int minIncrementForUnique(int[] A) {
        int ans = 0;
        HashMap<Integer,Integer> m = new HashMap<>();
        for(int i:A){
            m.put(i,m.getOrDefault(i,0)+1);
        }
        for(int i = 0;i<A.length;i++){
            if(m.get(A[i])!=1){
                int j = A[i]+1;
                while(m.getOrDefault(j,0)!=0)
                    j++;
                ans += (j-A[i]);
                m.put(j,1); 
                m.replace(A[i],m.get(A[i])-1);
            }
        }return ans;
    }
}
~~~
结果56/59 超时挂掉了。感觉是每次j++太耗时间了。[1 ,1,2,3,4,5,6,....100]j一次加1，非常耗时间。

想了半天优化j++。太复杂了。看了下题解。他们先排序，然后从[1]开始检索，只要小于等于前一个，就把这个设置成前一个+1，（不管后面的，实际上你这时管不管后面的，结果都一样）。

~~~java
class Solution {
    public int minIncrementForUnique(int[] A) {
        int ans = 0;
        Arrays.sort(A);
        for(int i = 1;i<A.length;i++){
            if(A[i]<=A[i-1]){
                int pre = A[i];
                A[i] = A[i-1]+1;
                ans += (A[i]-pre);
            }
        }
        return ans;
    }
}
~~~
击败70%

### Q20有效的括号
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
有效字符串需满足：
左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

难度 简单

示例 1:
输入: "()"
输出: true
示例 2:
输入: "()[]{}"
输出: true
示例 3:
输入: "(]"
输出: false
示例 4:
输入: "([)]"
输出: false
示例 5:
输入: "{[]}"
输出: true


学过栈应该都知道怎么做了
~~~java
class Solution {
    public boolean isValid(String s) {
        if(s.length()==0) return true;
        Stack<Integer> s1 = new Stack<>();
        for(int i = 0;i<s.length();i++){
            char ch = s.charAt(i);
            if(ch=='('){
                s1.push(1);
            }else if(ch=='['){
                s1.push(2);
            }else if(ch=='{'){
                s1.push(3);
            }else if(ch==')'){
                if(s1.empty()==true)
                    return false;
                if(s1.pop()!=1)
                    return false;
            }else if(ch==']'){
                if(s1.empty()==true)
                    return false;
                if(s1.pop()!=2)
                    return false;
            }else if(ch=='}'){
                if(s1.empty()==true)
                    return false;
                if(s1.pop()!=3)
                    return false;
            }
        }if(s1.empty()!=true)
            return false;
        return true;
    }
}
~~~
击败78%

### Q155最小栈
设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。
push(x) —— 将元素 x 推入栈中。
pop() —— 删除栈顶的元素。
top() —— 获取栈顶元素。
getMin() —— 检索栈中的最小元素。

难度 简单 

示例:
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]
输出：
[null,null,null,null,-3,null,0,-2]
解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.

提示：
pop、top 和 getMin 操作总是在 非空栈 上调用。

看到这个题还以为要自己实现一个大小任意的栈，我想这也太复杂了。看了官方题解，他也是用的轮子。那我也用轮子吧。

~~~java
class MinStack {
    Stack<Integer> s;
    Stack<Integer> min;
    /** initialize your data structure here. */
    public MinStack() {
        s = new Stack<>();
        min = new Stack<>();
        min.push(Integer.MAX_VALUE);
    }
    
    public void push(int x) {
        int m = min.peek();
        if(m>x){
            s.push(x);
            min.push(x);
        }else{
            s.push(x);
            min.push(m);
        }
    }
    
    public void pop() {
        min.pop();
        s.pop();
    }
    
    public int top() {
        return s.peek();
    }
    
    public int getMin() {
        return min.peek();
    }
}
~~~
击败97%

### Q224基本计算器
实现一个基本的计算器来计算一个简单的字符串表达式的值。
字符串表达式可以包含左括号 ( ，右括号 )，加号 + ，减号 -，非负整数和空格  。

难度 困难

示例 1:
输入: "1 + 1"
输出: 2
示例 2:
输入: " 2-1 + 2 "
输出: 3
示例 3:
输入: "(1+(4+5+2)-3)+(6+8)"
输出: 23
说明：
你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。

没想到这个也能算困难，学过栈的应该都写过，而且还能带乘除的。
~~~java
class Solution {
    public int calculate(String s) {
        Stack<Integer> s1 = new Stack<>();
        Stack<Integer> s2 = new Stack<>();
        int t = 0, flag = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                t = t * 10 + s.charAt(i) - '0';
                flag = 1;
            } else if (s.charAt(i) == '(') {
                if (flag == 1)
                    s1.push(t);
                flag = 0;
                t=0;
                s2.push(0);
            } else if (s.charAt(i) == ')') {
                if (flag == 1)
                    s1.push(t);
                flag = 0;
                t=0;
                int c = s2.pop();
                while (c != 0) {
                    if (c == 1) {
                        int x = s1.pop();
                        int y = s1.pop();
                        s1.push(x + y);
                    } else if (c == 2) {
                        int x = s1.pop();
                        int y = s1.pop();
                        s1.push(y - x);
                    }
                    c = s2.pop();
                }
            } else if (s.charAt(i) == '+') {
                if (flag == 1)
                    s1.push(t);
                flag = 0;
                t=0;
                if (s2.empty() != true && s2.peek() == 1) {
                    int x = s1.pop();
                    int y = s1.pop();
                    s1.push(x + y);
                    s2.pop();
                } else if (s2.empty() != true && s2.peek() == 2) {
                    int x = s1.pop();
                    int y = s1.pop();
                    s1.push(y - x);
                    s2.pop();
                }
                s2.push(1);
            } else if (s.charAt(i) == '-') {
                if (flag == 1)
                    s1.push(t);
                flag = 0;
                t=0;
                if (s2.empty() != true && s2.peek() == 1) {
                    int x = s1.pop();
                    int y = s1.pop();
                    s1.push(x + y);
                    s2.pop();
                } else if (s2.empty() != true && s2.peek() == 2) {
                    int x = s1.pop();
                    int y = s1.pop();
                    s1.push(y - x);
                    s2.pop();
                }
                s2.push(2);
            }
        }
        if (flag == 1)
            s1.push(t);
        flag = 0;
        t=0;
        if (s2.empty() != true) {
            
            if (s2.peek() == 1) {
                int x = s1.pop();
                int y = s1.pop();
                s1.push(x + y);
                
            } else if (s2.peek() == 2) {
                int x = s1.pop();
                int y = s1.pop();
                s1.push(y - x);
            }
        }
        return s1.pop();
    }
}
~~~
击败24%

### Q232用栈实现队列
使用栈实现队列的下列操作：
push(x) -- 将一个元素放入队列的尾部。
pop() -- 从队列首部移除元素。
peek() -- 返回队列首部的元素。
empty() -- 返回队列是否为空。

难度 简单 

示例:
MyQueue queue = new MyQueue();
queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false

说明:
你只能使用标准的栈操作 -- 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。

这题很简单，只需要两个栈来回倒腾就好了。看到题目序号，莫名喜感。
~~~java
class MyQueue {
    Stack<Integer> s1;
    Stack<Integer> s2;
    /** Initialize your data structure here. */
    public MyQueue() {
        s1 = new Stack<>();
        s2 = new Stack<>();
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        while(s1.empty()!=true){
            s2.push(s1.pop());
        }int t = s2.pop();
        while(s2.empty()!=true){
            s1.push(s2.pop());
        }return t;
    }
    
    /** Get the front element. */
    public int peek() {
        while(s1.empty()!=true){
            s2.push(s1.pop());
        }int t = s2.peek();
        while(s2.empty()!=true){
            s1.push(s2.pop());
        }return t;
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return s1.empty();
    }
}
~~~
击败100%

### Q316去除重复字母
给你一个仅包含小写字母的字符串，请你去除字符串中重复的字母，使得每个字母只出现一次。需保证返回结果的字典序最小（要求不能打乱其他字符的相对位置）。

难度 困难

示例 1:
输入: "bcabc"
输出: "abc"
示例 2:
输入: "cbacdcbc"
输出: "acdb"
注意：该题与 1081 https://leetcode-cn.com/problems/smallest-subsequence-of-distinct-characters 相同

这题是没啥思路，试了下自己写，问题很多，他这个字典序很麻烦。看了官方题解 贪心算法。先用HashMap记录下字母最后出现的位置。建立一个双端队列，然后遍历字符串，如果队列的最后一个元素大于当前元素，且后面还有相同元素则将它弹出。然后将当前元素插入队列。最后将队列转换成字符串

~~~java
class Solution {
    public String removeDuplicateLetters(String s) {
        HashMap<Character,Integer> map = new HashMap<>();
        LinkedList<Character> l = new LinkedList<>();
        for(int i = 0;i<s.length();i++){
            map.put(s.charAt(i),i);
        }
        for(int i = 0;i<s.length();i++){
            if(!l.contains(s.charAt(i))){
                char ch = s.charAt(i);
                while(!l.isEmpty()&&l.peekLast()>ch&&map.get(l.peekLast())>i){
                    l.removeLast();
                }l.add((char)ch);
            }
        }StringBuilder sb = new StringBuilder();
        for(int i = 0;i<l.size();i++)
            sb.append((char)l.get(i));
        return sb.toString();
    }
}
~~~
击败63%

LinkedList有peek第一 也有peekLast最后。也有poll pollLast。他的empty还是isEmpty 

### Q215数组中的第K个最大元素
在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

难度 中等

示例 1:
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
示例 2:
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
说明:
你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

我用库函数作弊了.
~~~java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length-k];
    }
}
~~~
击败91%

### Q347前 K 个高频元素
给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

难度 中等

示例 1:
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]
示例 2:
输入: nums = [1], k = 1
输出: [1]
提示：
你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
你可以按任意顺序返回答案。

这个问题也叫topK问题

先用hashmap遍历一遍，记录次数。再用priorityqueue，遍历一遍hashmap。当peek.value<value,再将value放进去。

之前没用过priorityqueue，他的原理类似于堆，每次可以提供一个最值。

~~~java
Queue<Integer> q = new PriorityQueue<>(new Comparator<Integer>(){
        public int compare(Integer a,Integer b){
            return m.get(a)-m.get(b);
        }
    });
~~~
他需要一个Comparator类来提供方法 或者 是实现comparable的类。

其他操作于普通队列相似

完整代码如下
~~~java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        HashMap<Integer,Integer> m = new HashMap<>();
        Queue<Integer> q = new PriorityQueue<>(new Comparator<Integer>(){
            public int compare(Integer a,Integer b){
                return m.get(a)-m.get(b);
            }
        });
        for(int i:nums){
            m.put(i,m.getOrDefault(i,0)+1);
        }
        for(int i:m.keySet()){
            if(q.size()<k){
                q.add(i);
            }else if(m.get(i)>m.get(q.peek())){
                q.remove();
                q.add(i);
            }
        }
        int[] ans = new int[k];
        for(int i = 0 ;i<k;i++){
            ans[i] = q.poll();
        }
        return ans;

        
    }
}
~~~
击败73%

### Q21合并两个有序链表
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

难度 简单

示例：
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

这题没啥好说，直接上代码
~~~java
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null&&l2==null) return l1;
        else if(l1!=null&&l2==null) return l1;
        else if(l2!=null&&l1==null) return l2;
        ListNode ans,p;
        if(l1.val>l2.val) {p = l2;l2 = l2.next;}
        else {p = l1;l1 = l1.next;}
        ans = p;
        while(l1!=null&&l2!=null){
            if(l1.val>l2.val){
                p.next = l2;
                l2 = l2.next;
            }else {
                p.next = l1;
                l1 = l1.next;
            }p = p.next;
        }if(l1!=null){
            p.next = l1;
        }else if(l2!=null){
            p.next = l2;
        }return ans;
    }
}
~~~
击败63%

### Q101对称二叉树
给定一个二叉树，检查它是否是镜像对称的。

难度 简单

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3


进阶：
你可以运用递归和迭代两种方法解决这个问题吗？

还是太菜了，参照了官方题解。利用递归求解。对称的指针，一个向左移，另一个向右移

~~~java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        return check(root,root);
    }
    public boolean check(TreeNode p,TreeNode q){
        if(p==null&&q==null)
            return true;
        if(p==null||q==null)
            return false;
        return p.val == q.val && check(p.left,q.right) &&check(p.right,q.left);
    }
}
~~~

击败100%

### Q104. 二叉树的最大深度
给定一个二叉树，找出其最大深度。
二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。
说明: 叶子节点是指没有子节点的节点。

难度 简单

示例：
给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。

~~~java
class Solution {
    public int maxDepth(TreeNode root) {
        if(root == null)    return 0;
        int x = maxDepth(root.left);
        int y = maxDepth(root.right);
        return (x>y?x:y)+1;
    }
}
~~~
击败100%

### Q226翻转二叉树
翻转一棵二叉树。

难度 简单

示例：
输入：

     4
   /   \
  2     7
 / \   / \
1   3 6   9
输出：

     4
   /   \
  7     2
 / \   / \
9   6 3   1
备注:
这个问题是受到 Max Howell 的 原问题 启发的 ：
谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。

这题很简单啊，用个递归。
~~~java
class Solution {
    public TreeNode invertTree(TreeNode root) {
        change(root);
        return root;
    }
    public void change(TreeNode root){
        if(root==null) return;
        TreeNode r = root.right;
        root.right = root.left;
        root.left = r;
        change(root.right);
        change(root.left);
    }
}
~~~
击败100%，没想到我这么强，比Homebrew作者强啊（逃

### Q236二叉树的最近公共祖先
给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。
百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”
例如，给定如下二叉树:  root = [3,5,1,6,2,0,8,null,null,7,4]

难度 中等

示例 1:
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出: 3
解释: 节点 5 和节点 1 的最近公共祖先是节点 3。
示例 2:
输入: root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出: 5
解释: 节点 5 和节点 4 的最近公共祖先是节点 5。因为根据定义最近公共祖先节点可以为节点本身。

说明:所有节点的值都是唯一的。
p、q 为不同节点且均存在于给定的二叉树中。

这题也没啥思路，自己写了个递归，写一半就知道不对。也就看了题解。（不知道我会不会太容易放弃了

这个根 的左子树一定含一个点，右子树一定含另一个点。所以我们只要递归，找到这两个点，然后返回回去。 如果返回值 一个是有 一个是没有，说明这个点不是，继续返回有的那个点。 如果返回值同时都有了，就说明这个点是根

~~~java
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null||root == p||root==q) return root;
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        TreeNode right = lowestCommonAncestor(root.right,p,q);
        if(left !=null && right!=null)  return root;
        if(left != null) return left;
        if(right != null) return right;
        return null;
    }
}
~~~
击败63%

### Q23合并K个升序链表
给你一个链表数组，每个链表都已经按升序排列。
请你将所有链表合并到一个升序链表中，返回合并后的链表。

难度 困难 

示例 1：
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
示例 2：
输入：lists = []
输出：[]
示例 3：
输入：lists = [[]]
输出：[]
提示：
k == lists.length
0 <= k <= 10^4
0 <= lists[i].length <= 500
-10^4 <= lists[i][j] <= 10^4
lists[i] 按 升序 排列
lists[i].length 的总和不超过 10^4

暴力合并，每次找数组中最小的。
~~~java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0) return null;
        ListNode ans = null,head;
        int max = 0;
        for(int i = 0;i < lists.length;i++){
            if((ans==null&&lists[i]!=null)||(lists[i]!=null&&ans.val>lists[i].val)){
                ans = lists[i];
                max = i;
            }
        }
        head = ans;
        if(lists[max]==null) return null;
        lists[max] = lists[max].next;
        int flag = 1;
        while(flag!=0){
            flag = 0;
            for(int i = 0;i < lists.length;i++){
                if((ans.next==null&&lists[i]!=null)||(lists[i]!=null&&ans.next.val>lists[i].val)){
                    ans.next = lists[i];
                    max = i;
                    
                }if(lists[i]!=null) flag = 1;
            }
            if(flag==1&&lists[max]!=null)
                lists[max] = lists[max].next;
            if(flag==1&&ans.next!=null)
                ans = ans.next;
        }
        return head;
    }
}
~~~
击败5%

改用优先队列，先把所有节点装进优先队列里，然后再一个个取出来。
~~~java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0) return null;
        Queue<ListNode> q = new PriorityQueue<>(new Comparator<ListNode>(){
            public int compare(ListNode a,ListNode b){
                return a.val-b.val;
            }
        });
        for(int i = 0;i<lists.length;i++){
            while(lists[i]!=null){
                q.add(lists[i]);
                lists[i] = lists[i].next;
            }
        }
        ListNode ans,tempt;
        tempt = q.poll();
        ans = tempt;
        while(q.isEmpty()!=true){
            tempt.next = q.poll();
            tempt = tempt.next;
        }
        if(tempt!=null)
            tempt.next = null;
        return ans;
    }
}
~~~
击败58%。优先队列真香

我想这个题是有序链表，如果我一股脑丢尽优先队列，岂不是浪费了。我每次只丢头进去，每次取出一个，再把下一个丢进去。

结果还是击败58%。

### Q33搜索旋转排序数组
假设按照升序排序的数组在预先未知的某个点上进行了旋转。
( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。
搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。
你可以假设数组中不存在重复的元素。
你的算法时间复杂度必须是 O(log n) 级别。

难度 中等

示例 1:
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
示例 2:
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1

第一百道题

本来二分法就拉跨，还搞个这么复杂的二分法。期间还拜托朋友找二分法的bug。硬生生写了两个多小时

学会分情况太重要了

这个图特别重要，只要这个图，看懂了就会写了。

首先是左图，target落在左边一定要卡严格。一定是 target>nums[0] && target<nums[mid]，其他情况target一定在mid右边

而右图，如法炮制，target一定落在右边一定要卡死。一定是 target>nums[mid] && target<nums[nums.length-1]。其他情况一定在右边。

如何判断现在的情况是左图，还是右图呢。nums[mid]>nums[0] nums[mid]<nums[0]

~~~java
class Solution {
    public int search(int[] nums, int target) {
        if(nums.length==0)  return -1;
        if(nums.length==1)  return (nums[0]==target)?0:-1;
        int l=0,r=nums.length-1,mid=(l+r)/2;
        while(l<r){
            if(nums[mid]==target)   return mid;
            if(nums[mid]>=nums[0]){
                if(target<nums[mid]&&target>=nums[0]){
                    r = mid-1;
                }else{
                    l = mid+1; 
                }
            }else{
                if(target>nums[mid]&&target<=nums[nums.length-1]){
                    l = mid+1;
                }else{
                    r = mid-1;
                }
            }mid = (l+r)/2;
        }if(nums[mid]==target)
            return mid;
        return -1;
    }
}
~~~
击败100%

### Q34在排序数组中查找元素的第一个和最后一个位置
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
你的算法时间复杂度必须是 O(log n) 级别。
如果数组中不存在目标值，返回 [-1, -1]。

难度 中等

示例 1:
输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
示例 2:
输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]

又是一个复杂的二分法。先找左边那个点，r尽量往左靠。再找右边那个点，l尽量往右靠。
由于循环跑出来的结果，我也不太确定，也就多写了几个if 来判断
~~~java
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int l = 0, r = nums.length - 1;
        int ans[] = new int[2];
        if (nums.length == 0) {
            ans[0] = -1;
            ans[1] = -1;
            return ans;
        } else if (nums.length == 1) {
            if (nums[0] == target) {
                ans[0] = 0;
                ans[1] = 0;
                return ans;
            }
            ans[0] = -1;
            ans[1] = -1;
            return ans;
        }
        // boolean left = true;
        int mid = (l + r) / 2;
        while (l < r) {
            if (nums[mid] < target) {
                l = mid;
            } else if (nums[mid] > target) {
                r = mid;
            }else if (nums[mid] == target) {
                r = mid-1;
            }
            mid = (l + r) / 2;
            if(l==r-1) break;
        }
        if (nums[l] == target)
            ans[0] = l;
        else if (nums[r] == target)
            ans[0] = r;
        else if (r+1<nums.length && nums[r+1] == target){
            ans[0] = r+1;
        }
        else {
            ans[0] = -1;
            ans[1] = -1;
            return ans;
        }
        l = mid;
        r = nums.length - 1;

        while (l < r) {
            if (nums[mid] > target) {
                r = mid;

            } else if (nums[mid] < target) {
                l = mid;
            } else if (nums[mid]== target){
                l = mid+1;
            }
            mid = (l + r) / 2;
            if(l == r-1) break;
        }
        if (nums[r] == target)
            ans[1] = r;
        else if (nums[l]==target)
            ans[1] = l;
        else if (l-1>=0&&nums[l-1] == target)
            ans[1] = l-1;
        return ans;
    }
}
~~~
击败100%

### Q5最长回文子串
给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

难度 中等

示例 1：
输入: "babad"
输出: "bab"
注意: "aba" 也是一个有效答案。
示例 2：
输入: "cbbd"
输出: "bb"

第一次写动态规划。先学了一下什么是动态规划。非常像高中的归纳法。总结出 从n-1到n，然后再去看 0 的时候。

放到这题，就是s（i，j）要为回文串，s（i+1，j-1）为回文串&&s（i）==s（j）。
s（i，i）一定为回文串。s（i，i+1）需要判断s（i）==s（i+1）

然后就是写循环，从长度为0开始，到长度为n。
~~~java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] dp = new boolean[n][n];
        String ans = new String("");
        for (int l = 0; l < n; l++) {
            for (int i = 0; i + l < n; i++) {
                int j = i + l;
                if (l == 0) {
                    dp[i][j] = true;
                } else if (l == 1) {
                    if (s.charAt(i) == s.charAt(j)) {
                        dp[i][j] = true;
                    } else {
                        dp[i][j] = false;
                    }
                } else {
                    if (dp[i + 1][j - 1] == true && s.charAt(i) == s.charAt(j)) {
                        dp[i][j] = true;
                    } else
                        dp[i][j] = false;
                }
                if (dp[i][j] == true && j - i + 1 > ans.length()) {
                    ans = s.substring(i, j + 1);
                }
            }
        }
        return ans;
    }
}
~~~
击败33%
### Q53最大子序和
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

难度 简单

示例:
输入: [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
进阶:
如果你已经实现复杂度为 O(n) 的解法，尝试使用更为精妙的分治法求解。

刚写完上一题，学了锤子，就觉得哪都是钉子，写了半天，搞出了一个n2的算法，还内存超出了。

看了官方题解，才发现这题其实很简单，就是滑窗问题，只要看加上这个，还是只有这个哪个大。

~~~java
class Solution {
    public int maxSubArray(int[] nums) {
        if(nums.length == 0) return 0;
        int n = nums.length;
        int max =nums[0];
        int pre = 0;
        for(int i:nums){
            pre = Math.max(pre+i,i);
            max = Math.max(max,pre);
        }
        return max;
    }
}
~~~
击败95%

### Q62不同路径
一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为“Start” ）。
机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为“Finish”）。
问总共有多少条不同的路径？

难度 中等

例如，上图是一个7 x 3 的网格。有多少可能的路径？

示例 1:
输入: m = 3, n = 2
输出: 3
解释:
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向右 -> 向下
2. 向右 -> 向下 -> 向右
3. 向下 -> 向右 -> 向右
示例 2:
输入: m = 7, n = 3
输出: 28
提示：
1 <= m, n <= 100
题目数据保证答案小于等于 2 * 10 ^ 9

这题其实特别简单啊。到(i,j)有多少种方法，就是到(i-1,j),(i,j-1)有多少种方法。 然后再把(0,j)(i,0)置为1

~~~java
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n];
        for(int i = 0; i<m ;i++){
            dp[i][0] = 1;
        }for(int i = 0; i<n;i++)    
            dp[0][i] = 1;
        for(int i = 1; i<m;i++){
            for(int j = 1; j<n;j++){
                dp[i][j] = dp[i-1][j]+dp[i][j-1];
            }
        }return dp[m-1][n-1];
    }
}
~~~

### Q64最小路径和
给定一个包含非负整数的 m x n 网格，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。
说明：每次只能向下或者向右移动一步。

难度 中等

示例:
输入:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 7
解释: 因为路径 1→3→1→1→1 的总和最小。

这题也很简单啊，跟上一题很类似，dp(i,j)的距离为 dp(i-1,j)+grim(i,j)和dp(i,j-1)+grim(i,j)最小值，先计算边上的距离，最后返回右小角的值。

~~~java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length,n = grid[0].length;
        int[][] dp = new int[m][n];
        dp[0][0] = grid[0][0];
        for(int i=1;i<m;i++){
            dp[i][0] = dp[i-1][0]+grid[i][0];
        }for(int i=1;i<n;i++){
            dp[0][i] = dp[0][i-1]+grid[0][i];
        }for(int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j] = Math.min(dp[i-1][j]+grid[i][j],dp[i][j-1]+grid[i][j]);
            }
        }return dp[m-1][n-1];
    }
}
~~~
击败87%

### Q70爬楼梯
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
注意：给定 n 是一个正整数。

难度 简单

示例 1：
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
示例 2：
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶

这题也很简单，dp[1] == 1 dp[2]==2，dp[n]=dp[n-2]+dp[n-1]

~~~java
class Solution {
    public int climbStairs(int n) {
        if(n==0) return 0;
        if(n==1) return 1;
        if(n==2) return 2;
        int[] dp =new int[n+1];
        dp[1] = 1;
        dp[2] = 2;
        for(int i =3;i<=n;i++){
            dp[i] = dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
}
~~~
击败100%

### Q118杨辉三角
给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。

难度 简单

在杨辉三角中，每个数是它左上方和右上方的数的和。
示例:
输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]

很简单，当每行第一个 或者 最后一个就add（1），其他就加上肩上两个
~~~java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>>  ans = new ArrayList<List<Integer>>();
        for(int i = 0;i<numRows;i++){
            ans.add(new ArrayList<>());
            for(int j = 0;j<i+1;j++){
                if(j == 0||j==i){
                    ans.get(i).add(1);
                }else {
                    ans.get(i).add(ans.get(i-1).get(j-1)+ans.get(i-1).get(j));
                }
            }
        }return ans;
    }
}
~~~
击败77%

### Q300最长上升子序列
给定一个无序的整数数组，找到其中最长上升子序列的长度。

难度 中等

示例:
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:
可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。
进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?

写了个n2的算法，dp记录长度，只要i>j nums[i]>nums[j]，就比较dp[i] dp[j]+1哪个大。

~~~java
class Solution {
    public int lengthOfLIS(int[] nums) {
        int n = nums.length;
        if(n==0) return 0;
        int[] dp = new int[n];
        dp[0] = 1;
        int max = 1;
        for (int i = 1; i < n; i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = Math.max(dp[j] + 1, dp[i]);
                    max = Math.max(max, dp[i]);
                }
            }
        }
        return max;
    }
}
~~~
击败19%

### Q1143最长公共子序列
给定两个字符串 text1 和 text2，返回这两个字符串的最长公共子序列的长度。
一个字符串的 子序列 是指这样一个新的字符串：它是由原字符串在不改变字符的相对顺序的情况下删除某些字符（也可以不删除任何字符）后组成的新字符串。
例如，"ace" 是 "abcde" 的子序列，但 "aec" 不是 "abcde" 的子序列。两个字符串的「公共子序列」是这两个字符串所共同拥有的子序列。
若这两个字符串没有公共子序列，则返回 0。

难度 中等 

示例 1:
输入：text1 = "abcde", text2 = "ace" 
输出：3  
解释：最长公共子序列是 "ace"，它的长度为 3。
示例 2:
输入：text1 = "abc", text2 = "abc"
输出：3
解释：最长公共子序列是 "abc"，它的长度为 3。
示例 3:
输入：text1 = "abc", text2 = "def"
输出：0
解释：两个字符串没有公共子序列，返回 0。
提示:
1 <= text1.length <= 1000
1 <= text2.length <= 1000
输入的字符串只含有小写英文字符。

dp的题真有意思，就是写不出递推方程...

我自己写了一个dp方程，dp记录text1[i]能构成长度，last代表text1[i]在text2出现的地址，维度只有一维。不出所料挂掉了。因为这样，没有穷举所有的可能。

看了别人写的题解。（这题竟然没有官方题解），两维的dp数组,dp[i][j]记录的是text1[0，i],text[0,j]能构成最长的长度。if(text[i]==text[j]) dp[i][j] = dp[i-1][j-1]; 相当于从i-1，j-1增加到i，j。else dp[i][j] = Math.max(dp[i-1][j],dp[i][j-1]) 

然后额外处理下第一行/第一列
~~~java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        if (text1 == null || text2 == null)
            return 0;
        int n = text1.length();
        int m = text2.length();
        int[][] dp = new int[n][m];
        int flag = 0;
        for (int i = 0; i < n; i++) {
            if (text1.charAt(i) == text2.charAt(0))
                flag = 1;
            if (flag == 1)
                dp[i][0] = 1;
        }
        flag = 0;
        for (int j = 0; j < m; j++) {
            if (text1.charAt(0) == text2.charAt(j))
                flag = 1;
            if (flag == 1)
                dp[0][j] = 1;
        }
        for(int i = 1; i < n; i++){
            for(int j = 1; j < m; j++){
                if(text1.charAt(i)==text2.charAt(j)){
                    dp[i][j] = dp[i-1][j-1]+1;
                }else
                    dp[i][j] = Math.max(dp[i][j-1],dp[i-1][j]);
            }
        }
        return dp[n-1][m-1];
    }

}
~~~
击败80%

### Q1227飞机座位分配概率
有 n 位乘客即将登机，飞机正好有 n 个座位。第一位乘客的票丢了，他随便选了一个座位坐下。
剩下的乘客将会：
如果他们自己的座位还空着，就坐到自己的座位上，
当他们自己的座位被占用时，随机选择其他座位
第 n 位乘客坐在自己的座位上的概率是多少？

难度 中等

示例 1：
输入：n = 1
输出：1.00000
解释：第一个人只会坐在自己的位置上。
示例 2：
输入: n = 2
输出: 0.50000
解释：在第一个人选好座位坐下后，第二个人坐在自己的座位上的概率是 0.5。
提示：
1 <= n <= 10^5

这题好迷，写着是dp

1号登机，有三种选择 1）坐1号 1/n 2）坐n号 1/n 3）其他位置i号 1/n × n;

i号登机，有三种选择 1）坐i号 1/n 2）坐n号 1/n 3）其他位置j号 1/(n-i+1) × (n-i+1);

所以i 与 1 是相同的

f(i) = 1/i + Σf(n-j+1)/i

就把i化解成 Σ(n-j+1),实际上就是从1求和到i-1；

然后我也按照这样写了，结果竟然超时了。

评论区说 return (n==1)?1:0.5;

~~~java
class Solution {
    public double nthPersonGetsNthSeat(int n) {
        if(n==1) return 1;
        return 0.5;
    }
}
~~~

### Q22括号生成
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

难度 中等

示例：

输入：n = 3
输出：[
       "((()))",
       "(()())",
       "(())()",
       "()(())",
       "()()()"
     ]

我知道思路，可是我写出来就是怪怪的...

看了官方题解，用一个void 函数，把结果存在实参数组里

~~~java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<>();
        StringBuilder s = new StringBuilder("");
        next(result,s,n,0,0);
        return result;
    }
    public void next(List<String> l,StringBuilder s,int n,int x,int y){
        if(n==x&&n==y){
            l.add(s.toString());
            return ;
        }
        if(x<n){
            s.append("(");
            next(l,s,n,x+1,y);
            s.deleteCharAt(s.length()-1);
        }if(y<x){
            s.append(")");
            next(l,s,n,x,y+1);
            s.deleteCharAt(s.length()-1);
        }
        return ;
    }
}
~~~
击败96

### Q46全排列
给定一个 没有重复 数字的序列，返回其所有可能的全排列。

难度 中等

示例:
输入: [1,2,3]
输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

这题之前做过类似的，没做出来。

知道是回溯法，但不知道咋实现。

官方题解的思路是，从first = 0开始填写，然后把nums[first] 与 nums[nums.length-1] 依次交换。

~~~java
import java.util.*;
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> ans = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        for(int i:nums) {
            temp.add(i);
        }
        dfs(ans,temp,0,nums.length);
        return ans;
    }
    public void dfs(List<List<Integer>> ans,List<Integer> temp,int first,int n){
        if(first == n){
            ans.add(new ArrayList<Integer>(temp));
            return;
        }
        for(int i=first;i<n;i++){
            Collections.swap(temp, first, i);
            dfs(ans, temp, first+1,n);
            Collections.swap(temp, first, i);
        }
    }
}
~~~
击败99% 

### Q94二叉树的中序遍历
给定一个二叉树，返回它的中序 遍历。

示例:

输入: [1,null,2,3]
   1
    \
     2
    /
   3

输出: [1,3,2]
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

用递归这题真是白给
~~~java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<>();
        in(result, root);
        return result;
    }

    public void in(List<Integer> l ,TreeNode e){
        if(e==null) return;
        in(l,e.left);
        l.add(e.val);
        in(l,e.right);
    }
}
~~~
击败100% 迭代算法是啥？

### Q102二叉树的层序遍历
给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

难度 中等

示例：
二叉树：[3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]

这题也是白送，用队列一下就写出来了
~~~java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root==null) return new ArrayList<>();
        Queue<Integer> level = new LinkedList<>();
        List<List<Integer>> l = new ArrayList<>();
        Queue<TreeNode> nodes = new LinkedList<>();
        List<Integer> temp = new ArrayList<>();
        
        level.add(1);
        nodes.add(root);
        int level1 = 1;
        while (nodes.size()!=0){
            int level2 = level.poll();
            if(level1!=level2){
                l.add(temp);
                temp = new ArrayList<>();
                level1 = level2;
            }
            TreeNode t = nodes.poll();
            temp.add(t.val);
            if(t.left!=null){
                nodes.add(t.left);
                level.add(level1+1);
            }
            if(t.right!=null){
                nodes.add(t.right);
                level.add(level1+1);
            }
        }
        l.add(temp);
        return l;
    }
}
~~~
击败92%

### Q110平衡二叉树
给定一个二叉树，判断它是否是高度平衡的二叉树。
本题中，一棵高度平衡二叉树定义为：
一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过1。

难度 简单

示例 1:
给定二叉树 [3,9,20,null,null,15,7]

    3
   / \
  9  20
    /  \
   15   7
返回 true 。

示例 2:
给定二叉树 [1,2,2,3,3,null,null,4,4]

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
返回 false 。

这题虽说是简单，但是想要准确无误写出来，应该还是有点难度。用dfs，递归左右子树深度，如果相差大于1，返回-1。如果左右有一个为-1，返回-1

~~~java
class Solution {
    public boolean isBalanced(TreeNode root) {
        if(high(root) == -1)  return false;
        return true;
    }
    public int high(TreeNode root) {
        if(root == null) return 0;
        int left = high(root.left);
        int right = high(root.right);
        if(left==-1||right==-1||Math.abs(left-right)>1) return -1;
        return Math.max(left+1,right+1);
    }
}
~~~
击败99% 

### Q144二叉树的前序遍历
给定一个二叉树，返回它的 前序 遍历。

难度 中等

示例:
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [1,2,3]
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

又是一题白送
~~~java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        dfs(l, root);
        return l;
    }
    public void dfs(List<Integer> ans, TreeNode root) {
        if(root == null) return;
        ans.add(root.val);
        dfs(ans, root.left);
        dfs(ans, root.right);
    }
}
~~~
击败100%

### Q145二叉树的后序遍历
给定一个二叉树，返回它的 后序 遍历。

难度 中等

示例:
输入: [1,null,2,3]  
   1
    \
     2
    /
   3 

输出: [3,2,1]
进阶: 递归算法很简单，你可以通过迭代算法完成吗？

如上
~~~java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        dfs(l, root);
        return l;
    }
    public void dfs(List<Integer> ans, TreeNode root) {
        if(root == null) return;
        dfs(ans, root.left);
        dfs(ans, root.right);
        ans.add(root.val);
    }
}
~~~
击败100%

### Q98验证二叉搜索树
给定一个二叉树，判断其是否是一个有效的二叉搜索树。
假设一个二叉搜索树具有如下特征：
节点的左子树只包含小于当前节点的数。
节点的右子树只包含大于当前节点的数。
所有左子树和右子树自身必须也是二叉搜索树。

难度 中等

示例 1:
输入:
    2
   / \
  1   3
输出: true
示例 2:
输入:
    5
   / \
  1   4
     / \
    3   6
输出: false
解释: 输入为: [5,1,4,null,null,3,6]。
     根节点的值为 5 ，但是其右子节点值为 4 。

中序遍历，看顺序是否正确

~~~java
class Solution {
    public boolean isValidBST(TreeNode root) {
        List<Integer> l = new ArrayList<>();
        dfs(l, root);
        for(int i=0; i<l.size()-1; i++){
            if(l.get(i)>=l.get(i+1))
                return false;
        }
        return true;
    }
    public void dfs(List<Integer> ans, TreeNode root) {
        if(root == null) return;
        dfs(ans, root.left);
        ans.add(root.val);
        dfs(ans, root.right);
    }
}
~~~
击败31%

感觉这样效率也太低了，改用递归写，递归左边，就修改上界；递归右边，就修改下界。

本来最初界限使用Integer.MAX_VALUE 写的，结果样例被卡。改用Long.MAX_VALUE

~~~java
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root == null) return true;
        if (root.left!=null&&dfs(root.left, root.val, Long.MIN_VALUE) == false)
            return false;
        if (root.right!=null&&dfs(root.right, Long.MAX_VALUE, root.val) == false)
            return false;
        return true;
    }

    public boolean dfs(TreeNode root, long high, long low) {
        if(root.val>=high||root.val<=low) return false;
        boolean left=true, right=true;
        if (root.left != null) {
            if (high > root.val)
                left = dfs(root.left, root.val, low);
            else
                left = dfs(root.left, high, low);
        }
        if (root.right != null) {
            if (low > root.val)
                right = dfs(root.right, high, low);
            else
                right = dfs(root.right, high, root.val);
        }
        if (left == false || right == false)
            return false;
        return true;
    }
}
~~~
击败100%

### Q701二叉搜索树中的插入操作
给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。 保证原始二叉搜索树中不存在新值。
注意，可能存在多种有效的插入方式，只要树在插入后仍保持为二叉搜索树即可。 你可以返回任意有效的结果。

难度 中等

例如, 
给定二叉搜索树:

        4
       / \
      2   7
     / \
    1   3

和 插入的值: 5
你可以返回这个二叉搜索树:

         4
       /   \
      2     7
     / \   /
    1   3 5
或者这个树也是有效的:

         5
       /   \
      2     7
     / \   
    1   3
         \
          4

这题也没啥难度，只要先找到位置，然后新建个节点

~~~java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public TreeNode insertIntoBST(TreeNode root, int val) {
        if(root == null) return new TreeNode(val);
        TreeNode t1 = root, t2 = root;
        while (t2 != null) {
            if (val > t2.val) {
                t1 = t2;
                t2 = t2.right;
            }
            else if (val < t2.val) {
                t1 = t2;
                t2 = t2.left;
            }
        }
        if (t1.val > val) {
            t1.left = new TreeNode(val);
        } else {
            t1.right = new TreeNode(val);
        }
        return root;
    }
}
~~~
击败100%

### Q450删除二叉搜索树中的节点
给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的 key 对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。
一般来说，删除节点可分为两个步骤：
首先找到需要删除的节点；
如果找到了，删除它。
说明： 要求算法时间复杂度为 O(h)，h 为树的高度。

难度 中等

示例:
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7

给定需要删除的节点值是 3，所以我们首先找到 3 这个节点，然后删除它。

一个正确的答案是 [5,4,6,2,null,null,7], 如下图所示。

    5
   / \
  4   6
 /     \
2       7

另一个正确答案是 [5,2,6,null,4,null,7]。

    5
   / \
  2   6
   \   \
    4   7

他这个样例给的太简单，会给人造成一种很简单的假象

翻了下数据结构，就是把p的右分支接到c的最右分支上，但是有很多细节要处理。

f为null，p为null，c为null，删除的节点就在root

~~~java
class Solution {
    public TreeNode deleteNode(TreeNode root, int key) {
        TreeNode f = null, p = root;
        while (p != null && p.val != key) {
            if (p.val < key) {
                f = p;
                p = p.right;
            } else if (p.val > key) {
                f = p;
                p = p.left;
            }
        }
        if (p == null) {
            return root;
        }
        TreeNode c = p.left;
        TreeNode pr = p.right;
        if (f == null) {
            if (c == null) {
                return pr;
            }
            TreeNode temp = c;
            while (temp.right != null)
                temp = temp.right;
            temp.right = pr;
            return c;
        }
        if (c == null) {
            if (f.left!=null&&f.left.val == key) {
                f.left = pr;
                return root;
            } else {
                f.right = pr;
                return root;
            }
        }
        TreeNode temp = c;
        while (temp.right != null)
            temp = temp.right;
        temp.right = pr;
        if (f.left!=null&&f.left.val == key) {
            f.left = c;

        } else {
            f.right = c;
        }
        return root;
    }
}
~~~

### Q338比特位计数
给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

难度 中等

示例 1:
输入: 2
输出: [0,1,1]
示例 2:
输入: 5
输出: [0,1,1,2,1,2]
进阶:
给出时间复杂度为O(n*sizeof(integer))的解答非常容易。但你可以在线性时间O(n)内用一趟扫描做到吗？
要求算法的空间复杂度为O(n)。
你能进一步完善解法吗？要求在C++或任何其他语言中不使用任何内置函数（如 C++ 中的 __builtin_popcount）来执行此操作。

动态规划问题，复杂度O（n），如果n为奇数，个数比前一个大一。如果n为偶数，则个数与n/2相同（n/2向左移即为n）。

~~~java
class Solution {
    public int[] countBits(int num) {
        int[] x = new int[num + 1];
        for (int i = 1; i <= num; i++) {
            if (x[i] != 0) {

            } else if (i % 2 != 0) {
                x[i] = x[i - 1] + 1;
            } else {
                x[i] = x[i/2];
            }
        }
        return x;
    }
}
~~~
击败74%

### Q198打家劫舍
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。
给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

难度 简单

示例 1：
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
示例 2：
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。

提示：
0 <= nums.length <= 100
0 <= nums[i] <= 400

动态规划问题，我一开始想得很复杂，看到别人的dp方程是 dp[i] = Math.max(dp[i-1],dp[i-2]+nums[i]),我在想 3 1 1 2这种情况可以通过吗？

dp[i-2]+nums[i] ,并不代表是偷了[i-2]。

换个角度来看，应该是选择偷不偷，偷就是i-2中的最大与i之和，不偷是i-1最大。
（其实感觉还是有很多疑惑
~~~java
class Solution {
    public int rob(int[] nums) {
        if(nums.length==0) return 0;
        int[] dp = new int[nums.length];
        for(int i = 0; i < nums.length; i++){
            if(i==0) dp[0] = nums[0];
            else if(i==1) dp[1] = Math.max(nums[0], nums[1]);
            else{
                dp[i] = Math.max(dp[i-1], dp[i-2]+nums[i]);
            }
        }
        return dp[nums.length-1];
    }
}
~~~
击败100%

### Q617合并二叉树
给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。
你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

难度 简单

示例 1:
输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
注意: 合并必须从两个树的根节点开始。

深度搜索
~~~java
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null && t2 == null)
            return null;
        if (t1 != null && t2 == null)
            return t1;
        if (t1 == null && t2 != null)
            return t2;
        if (t1 != null && t2 != null)
            t1.val += t2.val;
        dfs(t1, t2);
        return t1;
    }

    public void dfs(TreeNode t1, TreeNode t2) {
        if (t1 == null || t2 == null)
            return;

        if (t1.left == null && t2.left == null)
            ;
        else if (t1.left == null && t2.left != null)
            t1.left = t2.left;
        else if (t1.left != null && t2.left == null)
            ;
        else {
            t1.left.val += t2.left.val;
            dfs(t1.left, t2.left);
        }

        if (t1.right == null && t2.right == null)
            ;
        else if (t1.right == null && t2.right != null)
            t1.right = t2.right;
        else if (t1.right != null && t2.right == null)
            ;
        else {
            t1.right.val += t2.right.val;
            dfs(t1.right, t2.right);
        }
        return;
    }
}
~~~
击败64%
### Q39组合总和
给定一个无重复元素的数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。
candidates 中的数字可以无限制重复被选取。
说明：
所有数字（包括 target）都是正整数。
解集不能包含重复的组合。 

难度 中等

示例 1：
输入：candidates = [2,3,6,7], target = 7,
所求解集为：
[
  [7],
  [2,2,3]
]
示例 2：
输入：candidates = [2,3,5], target = 8,
所求解集为：
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
提示：
1 <= candidates.length <= 30
1 <= candidates[i] <= 200
candidate 中的每个元素都是独一无二的。
1 <= target <= 500

又是一个溯源，不太会写。

溯源要记住那个二叉树，一边是加上这个点，一边是不加这个点。一边是增加点 递归，另一边是删除点 递归。

~~~java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        if(candidates.length == 0) return null;
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        f(result, temp, candidates, target, 0, 0);
        return result;
    }

    public void f(List<List<Integer>> l, List<Integer> temp, int[] candidates, int target, int sum, int pos) {
        if (sum == target) {
            l.add(new ArrayList<Integer>(temp));
            return;
        }
        if (sum > target)
            return;
        if (pos == candidates.length)
            return;
        while (sum <= target) {
            temp.add(candidates[pos]);
            sum += candidates[pos];
            f(l, temp, candidates, target, sum, pos + 1);
        }
        while (temp.size() >= 1 && temp.get(temp.size() - 1) == candidates[pos]) {
            temp.remove(temp.size() - 1);
            sum -= candidates[pos];
        }
        f(l, temp, candidates, target, sum, pos + 1);
    }
}
~~~
基本就是暴力法了，击败14%

### Q17电话号码的字母组合
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。
给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

难度 中等

示例:
输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

回溯法。当数字为i，则字符串增加对应字母，多次递归。

~~~java
class Solution {
    public List<String> letterCombinations(String digits) {
        if(digits.length() == 0) return new ArrayList<>();
        List<String> ans = new ArrayList<>();
        String s = new String();
        fun(ans,s,0,digits);
        return ans;
    }
    public void fun(List<String> l,String s,int pos,String digits) {
        if(pos == digits.length()){
            l.add(s);
            return ;
        }
        if(digits.charAt(pos)=='2'){
            fun(l,new StringBuilder(s).append('a').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('b').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('c').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='3'){
            fun(l,new StringBuilder(s).append('d').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('e').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('f').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='4'){
            fun(l,new StringBuilder(s).append('g').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('h').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('i').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='5'){
            fun(l,new StringBuilder(s).append('j').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('k').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('l').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='6'){
            fun(l,new StringBuilder(s).append('m').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('n').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('o').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='7'){
            fun(l,new StringBuilder(s).append('p').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('q').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('r').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('s').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='8'){
            fun(l,new StringBuilder(s).append('t').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('u').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('v').toString(),pos+1,digits);
        }
        else if(digits.charAt(pos)=='9'){
            fun(l,new StringBuilder(s).append('w').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('x').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('y').toString(),pos+1,digits);
            fun(l,new StringBuilder(s).append('z').toString(),pos+1,digits);
        }
    } 
}
~~~
击败88%

### Q114二叉树展开为链表
给定一个二叉树，原地将它展开为一个单链表。

难度 中等

例如，给定二叉树

    1
   / \
  2   5
 / \   \
3   4   6
将其展开为：

1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6

本来想着原地修改就得，边递归边改。其实不用。Java的list存的是指针，开销不会太大，前序遍历存进list里

~~~java
class Solution {
    public void flatten(TreeNode root) {
        if(root == null) return;
        List <TreeNode> l = new ArrayList<>();
        preorder(l, root);
        TreeNode t = root;
        t.left = null;
        for(int i=1;i<l.size();i++){
            t.left = null;
            t.right = l.get(i);
            t = t.right;
        }
        t.right = null;
    }
    public void preorder(List<TreeNode> l,TreeNode root) {
        if(root==null) return;
        l.add(root);
        preorder(l,root.left);
        preorder(l,root.right);
    }
}
~~~
击败45%

### Q238除自身以外数组的乘积
给你一个长度为 n 的整数数组 nums，其中 n > 1，返回输出数组 output ，其中 output[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积。

难度 中等

示例:
输入: [1,2,3,4]
输出: [24,12,8,6]
提示：题目数据保证数组之中任意元素的全部前缀元素和后缀（甚至是整个数组）的乘积都在 32 位整数范围内。
说明: 请不要使用除法，且在 O(n) 时间复杂度内完成此题。
进阶：
你可以在常数空间复杂度内完成这个题目吗？（ 出于对空间复杂度分析的目的，输出数组不被视为额外空间。）

一下子就想到用除法...

看了下题解用L R数组分别记录，i左边和右边的乘积，L只要从左向右遍历一次，R只要从右向左遍历。

~~~java
class Solution {
    public int[] productExceptSelf(int[] nums) {
        if(nums.length == 0)    return new int[0];
        int[] L = new int[nums.length];
        int[] R = new int[nums.length];
        L[0] = 1;
        R[nums.length - 1] = 1;
        for(int i = 1; i < nums.length; i++){
            L[i] = nums[i-1]*L[i-1];
            R[nums.length-i-1] = nums[nums.length-i]*R[nums.length-i];
        }
        int[] ans = new int[nums.length];
        for(int i = 0; i < nums.length; i++){
            ans[i] = R[i]*L[i];
        }
        return ans;
    }
}
~~~
击败34%

### Q136只出现一次的数字

难度 简单

给定一个**非空**整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

**说明：**

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

**示例 1:**

```
输入: [2,2,1]
输出: 1
```

**示例 2:**

```
输入: [4,1,2,1,2]
输出: 4
```

这题一下子想到了hashmap，然而。

利用异或解决 n^n = 0;n^0 = n;

~~~java
class Solution {
    public int singleNumber(int[] nums) {
        int ans = 0;
        for(int i : nums)
            ans^=i;
        return ans;
    }
}
~~~

击败99%

### Q[48旋转图像](https://leetcode-cn.com/problems/rotate-image/)

难度 中等

给定一个 *n* × *n* 的二维矩阵表示一个图像。

将图像顺时针旋转 90 度。

**说明：**

你必须在**[原地](https://baike.baidu.com/item/原地算法)**旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要**使用另一个矩阵来旋转图像。

**示例 1:**

```
给定 matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

原地旋转输入矩阵，使其变为:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]
```

**示例 2:**

```
给定 matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 

原地旋转输入矩阵，使其变为:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
```

跟之前一个螺旋矩阵有一丝相似。

每次定位四个点，四个点互相交换。每次向里进一层。

~~~java
           class Solution {
    public void rotate(int[][] matrix) {
        if(matrix.length == 0) return ;
        int n = matrix.length;
        for(int t = 0;t<matrix.length/2;t++){
            int x1 = t,y1 = t;
            int x2 = t,y2 = n-t-1;
            int x3 = n-t-1,y3 = n-t-1;
            int x4 = n-t-1,y4 = t;
            for(int i = t;i<matrix.length-t-1;i++){
                int temp = matrix[x1][y1];
                matrix[x1][y1] = matrix[x4][y4];
                int temp2 = matrix[x2][y2];
                matrix[x2][y2] = temp;
                temp = matrix[x3][y3];
                matrix[x3][y3] = temp2;
                matrix[x4][y4] = temp;
                y1++;
                x2++;
                y3--;
                x4--;
            }
        }
    }
}
~~~

击败100%

### Q[96不同的二叉搜索树](https://leetcode-cn.com/problems/unique-binary-search-trees/)

难度 中等

给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？

**示例:**

```
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```

看到就知道应该是dp问题了，但是还是不会写。

G（n）表示有多少种结构，F（i，n）表示以i为根 n为长度

G（n）=ΣF（i，n）

F（i，n）=G（i-1）×G（n-i） （乘法原则 左右子树）

G（0）= G（1）= 1

~~~java
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n+1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i=2;i<=n;i++){
            for(int j=1;j<=i;j++){
                dp[i] += dp[j-1]*dp[i-j];
            }
        }
        return dp[n];
    }
}
~~~

由于递推公式的n与代码的n不同，写了个挺深的bug

击败100%

### [Q55跳跃游戏](https://leetcode-cn.com/problems/jump-game/)

难度 中等

给定一个非负整数数组，你最初位于数组的第一个位置。

数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个位置。

**示例 1:**

```
输入: [2,3,1,1,4]
输出: true
解释: 我们可以先跳 1 步，从位置 0 到达 位置 1, 然后再从位置 1 跳 3 步到达最后一个位置。
```

**示例 2:**

```
输入: [3,2,1,0,4]
输出: false
解释: 无论怎样，你总会到达索引为 3 的位置。但该位置的最大跳跃长度是 0 ， 所以你永远不可能到达最后一个位置。
```

这题是白送的，只要记录能跳到最远位置就可以了

~~~java
class Solution {
    public boolean canJump(int[] nums) {
        int max = 0;
        for(int i = 0; max<nums.length && i <= max;i++){
            max = max>i+nums[i]?max:i+nums[i];
        }
        if(max >= nums.length-1) return true;
        return false;
    }
}
~~~

击败99%

### [Q287寻找重复数](https://leetcode-cn.com/problems/find-the-duplicate-number/)

难度 中等

给定一个包含 *n* + 1 个整数的数组 *nums*，其数字都在 1 到 *n* 之间（包括 1 和 *n*），可知至少存在一个重复的整数。假设只有一个重复的整数，找出这个重复的数。

**示例 1:**

```
输入: [1,3,4,2,2]
输出: 2
```

**示例 2:**

```
输入: [3,1,3,4,2]
输出: 3
```

**说明：**

1. **不能**更改原数组（假设数组是只读的）。
2. 只能使用额外的 *O*(1) 的空间。
3. 时间复杂度小于 *O*(*n*2) 。
4. 数组中只有一个重复的数字，但它可能不止重复出现一次。

这题限制条件很多，不好写。用快慢指针法，龟兔赛跑，可以判断出环。但是没法猜出哪个是入口

a为起点到入口距离。b为环中已走，c为环剩下。L为环长

L = b+c

2(a+b) = kL+a+b



a = kL-b

此时把慢指针移到起点，快慢指针一次只走一步。 当慢指针走到起点时 走了a，快指针kL + a + b +kL - b，快指针也走到路口。

~~~java
class Solution {
    public int findDuplicate(int[] nums) {
        int fast = 0,slow = 0;
        do{
            slow = nums[slow];
            fast = nums[nums[fast]];
        }while(slow!=fast);
        slow = 0;
        while(slow!=fast){
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
}
~~~

击败100%

### Q[739每日温度](https://leetcode-cn.com/problems/daily-temperatures/)

难度中等520

请根据每日 `气温` 列表，重新生成一个列表。对应位置的输出为：要想观测到更高的气温，至少需要等待的天数。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

例如，给定一个列表 `temperatures = [73, 74, 75, 71, 69, 72, 76, 73]`，你的输出应该是 `[1, 1, 4, 2, 1, 1, 0, 0]`。

**提示：**`气温` 列表长度的范围是 `[1, 30000]`。每个气温的值的均为华氏度，都是在 `[30, 100]` 范围内的整数。



一开始写了个n2的算法。看了题解，只要每个元素记住最近的，比他大的元素即可（不一定是最大）

T[i-1]<T[i],这种情况直接赋值

T[i-1]>=T[i]就去查询big[i] 查离他最近的元素

记得赋值-1

~~~java
class Solution {
    public int[] dailyTemperatures(int[] T) {
        if(T.length == 0) return new int[0];
        int[] big = new int[T.length];
        big[T.length-1] = -1;
        int[] pos = new int[T.length];
        int temp = 0;
        for(int i = T.length-1;i >= 1 ;i--) {
            big[i-1] = -1;
            if(T[i-1]<T[i]){
                pos[i-1] = 1;
                big[i-1] = i; 
            }
            else{
                temp = big[i];
                while(temp!=-1&&T[i-1]>=T[temp]){
                    temp = big[temp];
                }
                if(temp!=-1&&T[i-1]<T[temp]){
                    pos[i-1] = temp-i+1;
                    big[i-1] = temp; 
                }
            }
        }
        return pos;
    }
}
~~~

击败98%

### Q[169多数元素](https://leetcode-cn.com/problems/majority-element/)

难度 简单

给定一个大小为 *n* 的数组，找到其中的多数元素。多数元素是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1:**

```
输入: [3,2,3]
输出: 3
```

**示例 2:**

```
输入: [2,2,1,1,1,2,2]
输出: 2
```



投票算法，先假定第一个是候选人maj，然后遍历数组，当x==maj count++，x!=maj count--，当count==0换届。

不用害怕真正的候选人选不上。当他不是候选人时，会把假的票下去，当他是真的，假的不会把他压倒。

~~~java
class Solution {
    public int majorityElement(int[] nums) {
        int maj = nums[0];
        int count = 0;
        for(int i :nums){
            if(i == maj){
                count ++;
            }
            else{
                count --;
                if(count == 0)
                {    
                    maj = i;
                    count++;
                }
            }
        }
        return maj;
    }
}
~~~

击败99%

### Q[283移动零](https://leetcode-cn.com/problems/move-zeroes/)

难度 简单

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例:**

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**说明**:

1. 必须在原数组上操作，不能拷贝额外的数组。
2. 尽量减少操作次数。

两个指针，i遍历数组，j从i+1开始寻找0

~~~java
class Solution {
    public void moveZeroes(int[] nums) {
        int i = 0,j = 0;
        while(i < nums.length){
            if(nums[i] == 0){
                break;
            }
            i++;
        }
        while(i < nums.length&&j<nums.length){
            if(nums[i] == 0){
                j = i+1;
                while(j < nums.length){
                    if(nums[j]!=0)
                        break;
                    j++;
                }
                if(j>=nums.length)
                    break;
                int temp = nums[i];
                nums[i] = nums[j];
                nums[j] = temp;
            }
            i++;
        }
        return ;
    }
}
~~~

击败12%



### [Q49字母异位词分组](https://leetcode-cn.com/problems/group-anagrams/)

难度 中等

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

- 所有输入均为小写字母。
- 不考虑答案输出的顺序。

这题刚开始以为是回溯法，并不是，是按照要求分组。

建立一个hashmap 在String integer，为了准确记录应存放位置，我们对String进行一个排序。把String转换成char[] 然后排序。

~~~java
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if(strs.length == 0) return new ArrayList<>();
        List<List<String>> ans = new ArrayList<>();
        List<String> temp = new ArrayList<>();
        int count = 0;
        HashMap<String, Integer> m = new HashMap<>();
        for (String str : strs) {
            String s = sortChar(str);
            int index =m.getOrDefault(s,-1);
            if(index==-1){
                temp = new ArrayList<>();
                temp.add(str);
                ans.add(temp);
                m.put(s,count);
                count++;
            }
            else{
                ans.get(index).add(str);
            }
        }
        return ans;
    }
    public String sortChar(String str) {
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        return new String(chars);
    }
}
~~~

击败73%

### Q[448找到所有数组中消失的数字](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

难度 简单

给定一个范围在 1 ≤ a[i] ≤ *n* ( *n* = 数组大小 ) 的 整型数组，数组中的元素一些出现了两次，另一些只出现一次。

找到所有在 [1, *n*] 范围之间没有出现在数组中的数字。

您能在不使用额外空间且时间复杂度为*O(n)*的情况下完成这个任务吗? 你可以假定返回的数组不算在额外空间内。

**示例:**

```
输入:
[4,3,2,7,8,2,3,1]

输出:
[5,6]
```

要求原地修改，就把对应的数字改成负数，然后最后在看哪些是负数

~~~java
class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            int num = Math.abs(nums[i]);
            if (nums[num - 1] > 0)
                nums[num - 1] = -nums[num - 1];
        }
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] > 0) {
                ans.add(i + 1);
            }
        }
        return ans;
    }
}
~~~

击败70%

### [Q867转置矩阵](https://leetcode-cn.com/problems/transpose-matrix/)

难度 简单

给定一个矩阵 `A`， 返回 `A` 的转置矩阵。

矩阵的转置是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

**示例 1：**

```
输入：[[1,2,3],[4,5,6],[7,8,9]]
输出：[[1,4,7],[2,5,8],[3,6,9]]
```

**示例 2：**

```
输入：[[1,2,3],[4,5,6]]
输出：[[1,4],[2,5],[3,6]] 
```

**提示：**

1. `1 <= A.length <= 1000`
2. `1 <= A[0].length <= 1000`

这个题白送

~~~java
class Solution {
    public int[][] transpose(int[][] A) {
        int n = A.length;
        int m = A[0].length;
        if(n == 0) return new int[0][0];
        int[][] x = new int[m][n];
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                x[i][j] = A[j][i];
            }
        }
        return x;
    }
}
~~~

击败100%

### [Q117填充每个节点的下一个右侧节点指针 II](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node-ii/)

难度中等269

给定一个二叉树

```
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 `NULL`。

初始状态下，所有 next 指针都被设置为 `NULL`。

 

**进阶：**

- 你只能使用常量级额外空间。
- 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

 

**示例：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/15/117_sample.png)

```
输入：root = [1,2,3,4,5,null,7]
输出：[1,#,2,3,#,4,5,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。
```

 

**提示：**

- 树中的节点数小于 `6000`
- `-100 <= node.val <= 100`

看到感觉很奇怪，这题咋会中等，用个队列就搞定了

~~~java
class Solution {
    public Node connect(Node root) {
        if(root == null) return null;
        Queue<Node> queue = new LinkedList<>();
        Queue<Integer> level = new LinkedList<>();
        queue.add(root);
        level.add(1);
        int prel = 0;
        Node preN = root;
        Node n;
        while (queue.isEmpty() != true) {
            n = queue.poll();
            int l = level.poll();
            if (l == prel) {
                preN.next = n;
            }else{
                preN.next = null;
            }
            preN = n;
            prel = l;
            if (n.left != null) {
                queue.add(n.left);
                level.add(l + 1);
            }
            if (n.right != null) {
                queue.add(n.right);
                level.add(l + 1);
            }
        }
        return root;
    }
}
~~~

击败7%，真是打扰了

原来得用递归啊。

### Q[977有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

难度 简单

给定一个按非递减顺序排序的整数数组 `A`，返回每个数字的平方组成的新数组，要求也按非递减顺序排序。



**示例 1：**

```
输入：[-4,-1,0,3,10]
输出：[0,1,9,16,100]
```

**示例 2：**

```
输入：[-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

 

**提示：**

1. `1 <= A.length <= 10000`
2. `-10000 <= A[i] <= 10000`
3. `A` 已按非递减顺序排序。

这题还不简单，我一开始想先找到零点，后来发现很复杂，要考虑很多特例。然后用了优先队列来求（杀鸡用牛刀

~~~java
class Solution {
    public int[] sortedSquares(int[] A) {
        if(A.length==0) return new int[0];
        Queue<Integer> q = new PriorityQueue<>(new Comparator<Integer>() {
            public int compare(Integer a, Integer b) {
                return a-b;
            }
        });
        for(int i:A){
            q.add(i*i);
        }
        int[] t = new int[q.size()];
        int i = 0;
        while(q.size()!=0){
            t[i++] = q.poll();
        }
        return t;
    }
}
~~~

不出意外 击败6%

~~~java
class Solution {
    public int[] sortedSquares(int[] A) {
        if (A.length == 0)
            return new int[0];
        int[] result = new int[A.length];
        int i = A.length - 1;
        int l = 0, r = A.length - 1;
        while (r != l && l < A.length && r > -1) {
            if (A[r] * A[r] >= A[l] * A[l]) {
                result[i--] = A[r] * A[r];
                r--;
            } else {
                result[i--] = A[l] * A[l];
                l++;
            }
        }
        if (l < A.length && r > -1)
            result[0] = A[r] * A[r];
        return result;
    }
}
~~~

改用双指针，从两边进，先赋值最大的。击败66%

### Q[1528重新排列字符串](https://leetcode-cn.com/problems/shuffle-string/)

难度 简单

给你一个字符串 `s` 和一个 **长度相同** 的整数数组 `indices` 。

请你重新排列字符串 `s` ，其中第 `i` 个字符需要移动到 `indices[i]` 指示的位置。

返回重新排列后的字符串。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/26/q1.jpg)

```
输入：s = "codeleet", indices = [4,5,6,7,0,2,1,3]
输出："leetcode"
解释：如图所示，"codeleet" 重新排列后变为 "leetcode" 。
```

**示例 2：**

```
输入：s = "abc", indices = [0,1,2]
输出："abc"
解释：重新排列后，每个字符都还留在原来的位置上。
```

**示例 3：**

```
输入：s = "aiohn", indices = [3,1,4,2,0]
输出："nihao"
```

**示例 4：**

```
输入：s = "aaiougrt", indices = [4,0,2,6,7,3,1,5]
输出："arigatou"
```

**示例 5：**

```
输入：s = "art", indices = [1,0,2]
输出："rat"
```

 

**提示：**

- `s.length == indices.length == n`
- `1 <= n <= 100`
- `s` 仅包含小写英文字母。
- `0 <= indices[i] < n`
- `indices` 的所有的值都是唯一的（也就是说，`indices` 是整数 `0` 到 `n - 1` 形成的一组排列）。

char数组一个个设

~~~java
class Solution {
    public String restoreString(String s, int[] indices) {
        char[] st = new char[s.length()];
        for(int i=0;i<s.length();i++) 
            st[indices[i]] = s.charAt(i);
        return String.valueOf(st);
    }
}
~~~

击败53%

### Q[1323 6和9 组成的最大数字](https://leetcode-cn.com/problems/maximum-69-number/)

难度简单33

给你一个仅由数字 6 和 9 组成的正整数 `num`。

你最多只能翻转一位数字，将 6 变成 9，或者把 9 变成 6 。

请返回你可以得到的最大数字。

 

**示例 1：**

```
输入：num = 9669
输出：9969
解释：
改变第一位数字可以得到 6669 。
改变第二位数字可以得到 9969 。
改变第三位数字可以得到 9699 。
改变第四位数字可以得到 9666 。
其中最大的数字是 9969 。
```

**示例 2：**

```
输入：num = 9996
输出：9999
解释：将最后一位从 6 变到 9，其结果 9999 是最大的数。
```

**示例 3：**

```
输入：num = 9999
输出：9999
解释：无需改变就已经是最大的数字了。
```

 

**提示：**

- `1 <= num <= 10^4`
- `num` 每一位上的数字都是 6 或者 9 。

把string和num来回倒腾就可以了

~~~java
class Solution {
    public int maximum69Number (int num) {
        String s = ""+num;
        for(int i=0;i<s.length();i++){
            if(s.charAt(i) == '6'){
                StringBuilder sb = new StringBuilder(s);
                sb.setCharAt(i,'9');
                return Integer.parseInt(sb.toString());
            }
        }
        return num;
    }
}
~~~

击败24%



### Q[260只出现一次的数字 III](https://leetcode-cn.com/problems/single-number-iii/)

难度中等296

给定一个整数数组 `nums`，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。

**示例 :**

```
输入: [1,2,1,3,2,5]
输出: [3,5]
```

**注意：**

1. 结果输出的顺序并不重要，对于上面的例子， `[5, 3]` 也是正确答案。
2. 你的算法应该具有线性时间复杂度。你能否仅使用常数空间复杂度来实现？

利用Hashmap求解

~~~java
class Solution {
    public int[] singleNumber(int[] nums) {
        HashMap<Integer, Integer> map = new HashMap<>();
        for (int i : nums) {
            map.put(i, map.getOrDefault(i, 0) + 1);
        }
        int[] ans = new int[2];
        int pos = 0;
        for (int i : nums) {
            if (map.get(i) == 1)
                ans[pos++] = i;
            if (pos == 2)
                return ans;
        }
        return ans;
    }
}
~~~

击败15%

### Q[933最近的请求次数](https://leetcode-cn.com/problems/number-of-recent-calls/)

难度  简单

写一个 `RecentCounter` 类来计算最近的请求。

它只有一个方法：`ping(int t)`，其中 `t` 代表以毫秒为单位的某个时间。

返回从 3000 毫秒前到现在的 `ping` 数。

任何处于 `[t - 3000, t]` 时间范围之内的 `ping` 都将会被计算在内，包括当前（指 `t` 时刻）的 `ping`。

保证每次对 `ping` 的调用都使用比之前更大的 `t` 值。

 

**示例：**

```
输入：inputs = ["RecentCounter","ping","ping","ping","ping"], inputs = [[],[1],[100],[3001],[3002]]
输出：[null,1,2,3,3]
```

 

**提示：**

1. 每个测试用例最多调用 `10000` 次 `ping`。
2. 每个测试用例会使用严格递增的 `t` 值来调用 `ping`。
3. 每次调用 `ping` 都有 `1 <= t <= 10^9`。

 这个题也是白送。只要两个指针，一个记住空位置，一个记住最近的时间

~~~java
class RecentCounter {
    int[] x;
    int pos;
    int times;

    public RecentCounter() {
        x = new int[10000];
        pos = 0;
        times = 0;
    }
    
    public int ping(int t) {
        x[pos++] = t;
        while(times<pos&&t-x[times]>3000){
            times++;
        }
        return pos-times;
    }
}
~~~

击败73%

### Q[474一和零](https://leetcode-cn.com/problems/ones-and-zeroes/)

难度 中等

在计算机界中，我们总是追求用有限的资源获取最大的收益。

现在，假设你分别支配着 **m** 个 `0` 和 **n** 个 `1`。另外，还有一个仅包含 `0` 和 `1` 字符串的数组。

你的任务是使用给定的 **m** 个 `0` 和 **n** 个 `1` ，找到能拼出存在于数组中的字符串的最大数量。每个 `0` 和 `1` 至多被使用**一次**。

 

**示例 1:**

```
输入: strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出: 4
解释: 总共 4 个字符串可以通过 5 个 0 和 3 个 1 拼出，即 "10","0001","1","0" 。
```

**示例 2:**

```
输入: strs = ["10", "0", "1"], m = 1, n = 1
输出: 2
解释: 你可以拼出 "10"，但之后就没有剩余数字了。更好的选择是拼出 "0" 和 "1" 。
```

 

**提示：**

- `1 <= strs.length <= 600`
- `1 <= strs[i].length <= 100`
- `strs[i]` 仅由 '0' 和 '1' 组成
- `1 <= m, n <= 100`

dp问题，日常写不出来

可以看出来 需要三维数组。一维是strs的第i个，一维是1个数，一维是0个数

dp[k+1] [i] [j] = dp[k] [i] j

dp[k+1] [i] [j] = max(dp[k] [i - zeros] [j - ones] +1, dp[k+1] [i] [j])

好像也不用怎么初始化

~~~java
class Solution {
    public int findMaxForm(String[] strs, int m, int n) {
        int[][][] dp = new int[strs.length + 1][m + 1][n + 1];
        for (int j = 0; j < strs.length; j++) {
            int zeros = 0;
            for (int i = 0; i < strs[j].length(); i++) {
                if (strs[j].charAt(i) == '0')
                    zeros++;
            }
            int ones = strs[j].length() - zeros;
            for (int e = 0; e <= m; e++) {
                for (int r = 0; r <= n; r++) {
                    dp[j + 1][e][r] = Math.max(dp[j][e][r], dp[j + 1][e][r]);
                    if (e - zeros >= 0 && r - ones >= 0) {
                        dp[j + 1][e][r] = Math.max(dp[j + 1][e][r], dp[j][e - zeros][r - ones] + 1);
                    }
                }
            }
        }
        return dp[strs.length][m][n];
    }
}
~~~

击败21%

### Q[452用最少数量的箭引爆气球](https://leetcode-cn.com/problems/minimum-number-of-arrows-to-burst-balloons/)

难度 中等

在二维空间中有许多球形的气球。对于每个气球，提供的输入是水平方向上，气球直径的开始和结束坐标。由于它是水平的，所以y坐标并不重要，因此只要知道开始和结束的x坐标就足够了。开始坐标总是小于结束坐标。平面内最多存在104个气球。

一支弓箭可以沿着x轴从不同点完全垂直地射出。在坐标x处射出一支箭，若有一个气球的直径的开始和结束坐标为 xstart，xend， 且满足  xstart ≤ x ≤ xend，则该气球会被引爆。可以射出的弓箭的数量没有限制。 弓箭一旦被射出之后，可以无限地前进。我们想找到使得所有气球全部被引爆，所需的弓箭的最小数量。

**Example:**

```
输入:
[[10,16], [2,8], [1,6], [7,12]]

输出:
2

解释:
对于该样例，我们可以在x = 6（射爆[2,8],[1,6]两个气球）和 x = 11（射爆另外两个气球）。
```

先排个序，然后搞个窗口，如果下个一个在窗口内，就是同一箭，不在就下一箭

贪心算法

~~~java
class Solution {
    public int findMinArrowShots(int[][] points) {
        if(points.length == 0) return 0;
        Arrays.sort(points, new Comparator<int[]>() {
            public int compare(int[] a, int[] b) {
                return a[0] - b[0];
            }
        });
        int l = points[0][0] , r = points[0][1];
        int arrows = 1;
        for(int i = 0; i < points.length;i++){
            if(l<=points[i][0]&&r>=points[i][0]){
                l = points[i][0];
                r = Math.min(points[i][1],r);
            }
            else{
                arrows++;
                l = points[i][0];
                r = points[i][1];
            }
        }
        return arrows;
    }
}
~~~

击败44%

### Q[309最佳买卖股票时机含冷冻期](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)

难度 中等

给定一个整数数组，其中第 *i* 个元素代表了第 *i* 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

- 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

**示例:**

```
输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```

dp问题都好有意思 就是不会解。

dp[ day ] [ status ] = money

status = 0 为已经持有股票

status = 1 为冷冻期

status = 2 为无股票期

![image-20200930112100492](https://raw.githubusercontent.com/csjue/csjue.github.io/master/_posts/images/20200930112100.png)

就是三个状态相互转换

~~~java
class Solution {
    public int maxProfit(int[] prices) {
        if(prices.length == 0) return 0;
        int[][] dp = new int[prices.length][3];
        dp[0][0] = -prices[0];
        for (int i = 1; i < prices.length; i++) {
            dp[i][1] = dp[i - 1][0] + prices[i];
            dp[i][0] = Math.max(dp[i-1][0], dp[i - 1][2] - prices[i]);
            dp[i][2] = Math.max(dp[i - 1][1], dp[i - 1][2]);
        }
        return Math.max(dp[prices.length - 1][2], dp[prices.length - 1][1]);
    }
}
~~~

击败99%


### [LCP19秋叶收藏集](https://leetcode-cn.com/problems/UlBDOe/)

难度 中等

小扣出去秋游，途中收集了一些红叶和黄叶，他利用这些叶子初步整理了一份秋叶收藏集 `leaves`， 字符串 `leaves` 仅包含小写字符 `r` 和 `y`， 其中字符 `r` 表示一片红叶，字符 `y` 表示一片黄叶。
出于美观整齐的考虑，小扣想要将收藏集中树叶的排列调整成「红、黄、红」三部分。每部分树叶数量可以不相等，但均需大于等于 1。每次调整操作，小扣可以将一片红叶替换成黄叶或者将一片黄叶替换成红叶。请问小扣最少需要多少次调整操作才能将秋叶收藏集调整完毕。

**示例 1：**

> 输入：`leaves = "rrryyyrryyyrr"`
>
> 输出：`2`
>
> 解释：调整两次，将中间的两片红叶替换成黄叶，得到 "rrryyyyyyyyrr"

**示例 2：**

> 输入：`leaves = "ryr"`
>
> 输出：`0`
>
> 解释：已符合要求，不需要额外操作

**提示：**

- `3 <= leaves.length <= 10^5`
- `leaves` 中只包含字符 `'r'` 和字符 `'y'`

每日一题。一个dp问题

dp[i] [color]

0为左r 1为中y 2为右r,根据当前颜色写出状态方程

~~~java
class Solution {
    public int minimumOperations(String leaves) {
        if(leaves.length() == 0) return 0;
        int[][] dp = new int[leaves.length()][3];
        if(leaves.charAt(0) == 'r'){
            dp[0][0] = 0;
            dp[0][1] = 100000;
            dp[0][2] = 100001;
        }else{
            dp[0][0] = 1;
            dp[0][1] = 100000;
            dp[0][2] = 100001;
        }
        for(int i=1;i<leaves.length();i++){
            if(leaves.charAt(i)=='r'){
                dp[i][0] = dp[i-1][0];
                dp[i][1] = Math.min(dp[i-1][1]+1,dp[i-1][0]+1);
                dp[i][2] = Math.min(dp[i-1][2],dp[i-1][1]);
            }else{
                dp[i][0] = dp[i-1][0]+1;
                dp[i][1] = Math.min(dp[i-1][1],dp[i-1][0]);
                dp[i][2] = Math.min(dp[i-1][2]+1,dp[i-1][1]+1);
            }
        }
        return dp[leaves.length()-1][2];
    }
}
~~~

击败25%

### [Q771宝石与石头](https://leetcode-cn.com/problems/jewels-and-stones/)

难度 简单

 给定字符串`J` 代表石头中宝石的类型，和字符串 `S`代表你拥有的石头。 `S` 中每个字符代表了一种你拥有的石头的类型，你想知道你拥有的石头中有多少是宝石。

`J` 中的字母不重复，`J` 和 `S`中的所有字符都是字母。字母区分大小写，因此`"a"`和`"A"`是不同类型的石头。

**示例 1:**

```
输入: J = "aA", S = "aAAbbbb"
输出: 3
```

**示例 2:**

```
输入: J = "z", S = "ZZ"
输出: 0
```

**注意:**

- `S` 和 `J` 最多含有50个字母。
-  `J` 中的字符不重复。

每日一题，今天挺简单，试着用hashmap写了一下

~~~java
class Solution {
    public int numJewelsInStones(String J, String S) {
        if(S.length() == 0 || J.length() == 0) return 0;
        HashMap<Character,Integer> m = new HashMap<>();
        for(int i=0;i<S.length();i++)
            m.put(S.charAt(i),m.getOrDefault(S.charAt(i),0)+1);
        int ans = 0;
        for(int i=0;i<J.length();i++){
            ans+=m.getOrDefault(J.charAt(i),0);
        }
        return ans;
    }
}
~~~

击败47%

然后我发现这题我之前写过，原来写的是1ms，现在是2ms，我以为是因为原来是C语言，结果也是java，就是暴力写的



### Q[75颜色分类](https://leetcode-cn.com/problems/sort-colors/)

难度中等600

给定一个包含红色、白色和蓝色，一共 *n* 个元素的数组，**[原地](https://baike.baidu.com/item/原地算法)**对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

**注意:**
不能使用代码库中的排序函数来解决这道题。

**示例:**

```
输入: [2,0,2,1,1,0]
输出: [0,0,1,1,2,2]
```

**进阶：**

- 一个直观的解决方案是使用计数排序的两趟扫描算法。
  首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。
- 你能想出一个仅使用常数空间的一趟扫描算法吗？

这题并不是很难，两次遍历。第一次数出0、1个数。第二次原地修改数组

~~~java
class Solution {
    public void sortColors(int[] nums) {
        if(nums.length == 0) return;
        int zeros = 0, ones = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                zeros++;
            } else if (nums[i] == 1) {
                ones++;
            }
        }
        for (int i = 0; i < nums.length; i++) {
            if (zeros > 0) {
                if (nums[i] == 0) {
                    zeros--;
                    continue;
                }
                int pos = nextInt(0, i,nums);
                nums[pos] = nums[i];
                nums[i] = 0;
                zeros--;
            } else if (ones > 0) {
                if (nums[i] == 1) {
                    ones--;
                    continue;
                }
                int pos = nextInt(1,i, nums);
                nums[pos] = nums[i];
                nums[i] = 1;
                ones--;
            } else {
                return;
            }
        }
    }

    public int nextInt(int i, int pos,int[] x) {
        for (int j = pos; j < x.length; j++) {
            if (x[j] == i)
                return j;
        }
        return -1;
    }
}
~~~

没想到击败100%，因为毕竟还是两次遍历

### Q[208实现 Trie (前缀树)](https://leetcode-cn.com/problems/implement-trie-prefix-tree/)

难度 中等

实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。

**示例:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

**说明:**

- 你可以假设所有的输入都是由小写字母 `a-z` 构成的。
- 保证所有输入均为非空字符串。

稍微看了下答案写的，刚开始以为是用char数组存的，写成一个链表状的。但是这样字母可以随机组合成一个新单词。

应该用Trie数组来存，Trie[26] 对应的是null则无这个字母。需要用一个boolean来记录这个节点是不是最后一个来配合search操作

~~~java
class Trie {
    public Trie[] next;
    boolean isEnd;    

    /** Initialize your data structure here. */
    public Trie() {
        next = new Trie[26];
    }

    /** Inserts a word into the trie. */
    public void insert(String word) {
        Trie temp = this;
        for (int i = 0; i < word.length(); i++) {
            if (temp.next[word.charAt(i)-'a'] == null){
                temp.next[word.charAt(i)-'a'] = new Trie();
            }
            temp = temp.next[word.charAt(i)-'a'];
        }
        temp.isEnd = true;
    }

    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        Trie temp = this;
        for (int i = 0; i < word.length(); i++) {
            if (temp.next[word.charAt(i)-'a'] == null) {
                return false;
            }
            temp = temp.next[word.charAt(i)-'a'];
        }
        if (temp.isEnd != true)
            return false;
        return true;
    }

    /**
     * Returns if there is any word in the trie that starts with the given prefix.
     */
    public boolean startsWith(String prefix) {
        Trie temp = this;
        for (int i = 0; i < prefix.length(); i++) {
            if (temp.next[prefix.charAt(i)-'a'] == null)
                return false;
            temp = temp.next[prefix.charAt(i)-'a'];
        }
        return true;
    }
}
~~~

击败62%

### [Q105从前序与中序遍历序列构造二叉树](https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

难度中等703

根据一棵树的前序遍历与中序遍历构造二叉树。

**注意:**
你可以假设树中没有重复的元素。

例如，给出

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
```

返回如下的二叉树：

```
    3
   / \
  9  20
    /  \
   15   7
```

这题按着标答写的。利用递归函数，每次确定先序的范围 中序的范围，返回先序第一个结点

先序遍历 根 左子树 右子树

中序遍历 左子树 根 右子树

preorder_left, preorder_right, inorder_left ,inorder_right

当前结点就是先序第一个

通过inorder确定inorder_root的位置，也就可以确定size_left = inorder_root - inorder_left

root.left 的范围 preorder_left+1, preorder_left+size_left, inorder_left, inorder_root-1

root.right 的范围 preorder_left+size_left+1, preorder_right, inorder_root+1, inorder_right

当preorder_left>preorder_right : return null;

~~~java
import java.util.*;

class Solution {
    public HashMap<Integer, Integer> m;

    public TreeNode buildTree(int[] preorder, int[] inorder) {
        int n = preorder.length;
        m = new HashMap<Integer, Integer>();
        for (int i = 0; i < inorder.length; i++) {
            m.put(inorder[i], i);
        }
        return dfs(preorder, inorder, 0, n - 1, 0, n - 1);
    }

    public TreeNode dfs(int[] preorder, int[] inorder, int preorder_left, int preorder_right, int inorder_left,
            int inorder_right) {
        if (preorder_left > preorder_right)
            return null;
        int inorder_root = m.get(preorder[preorder_left]);
        TreeNode root = new TreeNode(preorder[preorder_left]);
        int size_left = inorder_root - inorder_left;
        root.left = dfs(preorder, inorder, preorder_left + 1, preorder_left + size_left, inorder_left,
                inorder_root - 1);
        root.right = dfs(preorder, inorder, preorder_left + size_left + 1, preorder_right, inorder_root + 1,
                inorder_right);
        return root;
    }
}
~~~

 击败73%

### Q[406根据身高重建队列](https://leetcode-cn.com/problems/queue-reconstruction-by-height/)

难度中等493

假设有打乱顺序的一群人站成一个队列。 每个人由一个整数对`(h, k)`表示，其中`h`是这个人的身高，`k`是排在这个人前面且身高大于或等于`h`的人数。 编写一个算法来重建这个队列。

**注意：**
总人数少于1100人。

**示例**

```
输入:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

输出:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```

这题一直没有思路，暴力也不懂怎么写。看了标答，**原来高的人是看不见矮的**..., 所以先排高的。反正矮的他们也看不到...

先排个序，然后按照数组第二个值去插入

~~~java
class Solution {
    public int[][] reconstructQueue(int[][] people) {
        Arrays.sort(people, new Comparator<int[]>() {
            public int compare(int[] o1, int[] o2) {
                return o1[0] == o2[0] ? o1[1] - o2[1] : o2[0] - o1[0];
            }
        });
        List<int[]> result = new ArrayList<>();
        for (int[] i : people) {
            result.add(i[1],i);
        }
        return result.toArray(new int[result.size()][2]);
    }
}
~~~

我的Java知识也影响我不太会实现这个算法...

击败95%

### [Q18四数之和](https://leetcode-cn.com/problems/4sum/)

难度 中等

给定一个包含 *n* 个整数的数组 `nums` 和一个目标值 `target`，判断 `nums` 中是否存在四个元素 *a，**b，c* 和 *d* ，使得 *a* + *b* + *c* + *d* 的值与 `target` 相等？找出所有满足条件且不重复的四元组。

**注意：**

答案中不可以包含重复的四元组。

**示例：**

```
给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

两层循环+双指针法。注意要剔除重复的元组。

~~~java
class Solution {
    public List<List<Integer>> fourSum(int[] nums, int target) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp;
        for (int i = 0; i < nums.length - 3; i++) {
            if (i > 0 && nums[i] == nums[i - 1])
                continue;
            for (int j = i + 1; j < nums.length - 2; j++) {
                if (j > i + 1 && nums[j] == nums[j - 1])
                    continue;
                int l = j + 1;
                int r = nums.length - 1;
                while (l < r) {
                    if (nums[i] + nums[j] + nums[l] + nums[r] == target) {
                        temp = new ArrayList<>();
                        temp.add(nums[i]);
                        temp.add(nums[j]);
                        temp.add(nums[r]);
                        temp.add(nums[l]);
                        result.add(temp);
                        while (l < r && nums[l] == nums[l + 1])
                            l++;
                        l++;
                        while (l < r && nums[r] == nums[r - 1])
                            r--;
                        r--;
                    } else if (nums[i] + nums[j] + nums[l] + nums[r] > target) {
                        r--;
                    } else {
                        l++;
                    }
                }
            }
        }
        return result;
    }
}
~~~

击败28%

### Q[148排序链表](https://leetcode-cn.com/problems/sort-list/)

难度 中等

在 *O*(*n* log *n*) 时间复杂度和常数级空间复杂度下，对链表进行排序。

**示例 1:**

```
输入: 4->2->1->3
输出: 1->2->3->4
```

**示例 2:**

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```

用优先队列。

~~~java
class Solution {
    public ListNode sortList(ListNode head) {
        if(head == null) return null;
        Queue<ListNode> q = new PriorityQueue<>(new Comparator<ListNode>() {
            public int compare(ListNode a, ListNode b) {
                return a.val - b.val;
            }
        });
        ListNode temp = head;
        while(temp != null) {
            q.add(temp);
            temp = temp.next;
        }
        ListNode ans = q.poll();
        temp = ans;
        while(q.peek() != null){
            temp.next = q.poll();
            temp = temp.next;
        }
        temp.next = null;
        return ans;
    }
}
~~~

击败17%

### Q[647回文子串](https://leetcode-cn.com/problems/palindromic-substrings/)

难度 中等

给定一个字符你的任务是计算这个字符串中有多少个回文子串。

具有不同开始位置或结束位置的子串，即使是由相同的字符组成，也会被视作不同的子串。

 

**示例 1：**

```
输入："abc"
输出：3
解释：三个回文子串: "a", "b", "c"
```

**示例 2：**

```
输入："aaa"
输出：6
解释：6个回文子串: "a", "a", "a", "aa", "aa", "aaa"
```

 

**提示：**

- 输入的字符串长度不会超过 1000 。

不会dp，直接暴力穷举，每次穷举中心点，逐渐增大长度。（奇数长度，偶数长度）

~~~java
class Solution {
    public int countSubstrings(String s) {
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            for (int j = 0; i + j < s.length() && i - j >= 0; j++) {
                if (s.charAt(i + j) == s.charAt(i - j))
                    count++;
                else
                    break;
            }
        }
        for (int i = 0; i < s.length() - 1; i++) {
            if (s.charAt(i) != s.charAt(i + 1))
                continue;
            for (int j = 0; i - j >= 0 && i + 1 + j < s.length(); j++) {
                if (s.charAt(i - j) == s.charAt(i + j + 1))
                    count++;
                else
                    break;
            }
        }
        return count;
    }
}
~~~

击败57%

### [Q538把二叉搜索树转换为累加树](https://leetcode-cn.com/problems/convert-bst-to-greater-tree/)

难度 中等

给出二叉 **搜索** 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 `node` 的新值等于原树中大于或等于 `node.val` 的值之和。

提醒一下，二叉搜索树满足下列约束条件：

- 节点的左子树仅包含键 **小于** 节点键的节点。
- 节点的右子树仅包含键 **大于** 节点键的节点。
- 左右子树也必须是二叉搜索树。

**注意：**本题和 1038: https://leetcode-cn.com/problems/binary-search-tree-to-greater-sum-tree/ 相同

 

**示例 1：**

**![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/05/03/tree.png)**

```
输入：[4,1,6,0,2,5,7,null,null,null,3,null,null,null,8]
输出：[30,36,21,36,35,26,15,null,null,null,33,null,null,null,8]
```

**示例 2：**

```
输入：root = [0,null,1]
输出：[1,null,1]
```

**示例 3：**

```
输入：root = [1,0,2]
输出：[3,3,2]
```

**示例 4：**

```
输入：root = [3,2,4,1]
输出：[7,9,4,10]
```

 

**提示：**

- 树中的节点数介于 `0` 和 `104` 之间。
- 每个节点的值介于 `-104` 和 `104` 之间。
- 树中的所有值 **互不相同** 。
- 给定的树为二叉搜索树。

这题就是dfs，注意左右子树需要区别对待，先计算右子树和，再把和传给左子树

~~~java
class Solution {
    public TreeNode convertBST(TreeNode root) {
        dfs(root,0);
        return root;
    }
    public int dfs(TreeNode root,int x){
        if(root == null) return 0;
        int max = dfs(root.right,x);
        if(max==0)
            root.val += x;
        else
            root.val += max;
        if(root.left != null)
            return dfs(root.left,root.val);
        return root.val;
    }
}
~~~

击败98%

### [Q337打家劫舍 III](https://leetcode-cn.com/problems/house-robber-iii/)

难度 中等

在上次打劫完一条街道之后和一圈房屋后，小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为“根”。 除了“根”之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

计算在不触动警报的情况下，小偷一晚能够盗取的最高金额。

**示例 1:**

```
输入: [3,2,3,null,3,null,1]

     3
    / \
   2   3
    \   \ 
     3   1

输出: 7 
解释: 小偷一晚能够盗取的最高金额 = 3 + 3 + 1 = 7.
```

**示例 2:**

```
输入: [3,4,5,1,3,null,1]

     3
    / \
   4   5
  / \   \ 
 1   3   1

输出: 9
解释: 小偷一晚能够盗取的最高金额 = 4 + 5 = 9.
```

又是动态规划，又不会写

map f(node)代表取出这个点 等于左右两点不取与自身的和

map g(node)代表不取出 等于左右两点最大之和

~~~java
class Solution {
    Map<TreeNode,Integer> f = new HashMap<>();
    Map<TreeNode,Integer> g = new HashMap<>();
    public int rob(TreeNode root) {
        f.put(null,0);
        g.put(null,0);
        dfs(root);
        return Math.max(f.get(root),g.get(root));
    }
    public void dfs(TreeNode root){
        if(root == null) return;
        dfs(root.left);
        dfs(root.right);
        f.put(root,g.get(root.left)+g.get(root.right)+root.val);
        g.put(root,Math.max(f.get(root.left),g.get(root.left))+Math.max(f.get(root.right),g.get(root.right))); 
    }
}
~~~

击败43%

### Q[279完全平方数](https://leetcode-cn.com/problems/perfect-squares/)

难度 中等

给定正整数 *n*，找到若干个完全平方数（比如 `1, 4, 9, 16, ...`）使得它们的和等于 *n*。你需要让组成和的完全平方数的个数最少。

**示例 1:**

```
输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
```

**示例 2:**

```
输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```

还是dp问题，从1一直找到n

我刚开始手推的时候，只写到10，发现只要检查sqrt(n)那个就可以了。但是样例12，总是过不了。sqrt(12) = 3

12=9+1+1+1其实应该是12 = 4+4+4，所以得检索 1~sqrt(n)*sqrt(n)，复杂度n sqrt(n)

~~~java
class Solution {
    public int numSquares(int n) {
        int[] dp = new int[n + 1];
        for (int i = 1; i * i <= n; i++) {
            dp[i * i] = 1;
        }
        for (int i = 1; i <= n; i++) {
            int r = (int) Math.sqrt(i);
            if (r * r != i) {
                dp[i] = 4;
                for (int j = 1; j <= r; j++) {
                    dp[i] = Math.min(1 + dp[i - j * j],dp[i]);
                }
            }
        }
        return dp[n];
    }
}
~~~

击败77%

### Q[160相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists/)

难度简单835

编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

在节点 c1 开始相交。

 

**示例 1：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
```

 

**示例 2：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

 

**示例 3：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
```

 

**注意：**

- 如果两个链表没有交点，返回 `null`.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。

力扣不决问哈希图

直接用HashMap

~~~java
import java.util.*;
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        HashMap<ListNode,Integer> m = new HashMap<>();
        while(headA!=null){
            m.put(headA,1);
            headA = headA.next;
        }
        while(headB!=null){
            if(m.getOrDefault(headB,0)==1)
                return headB;
            headB = headB.next;
        }
        return null;
    }
}
~~~

击败8%.... 最快也就O(n) 我这也不过是O(2n)

双指针法

~~~java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
         if(headA == null || headB == null) return null;
        ListNode A = headA,B = headB;
        while(A!=B){
            A = A==null?headB:A.next;
            B = B==null?headA:B.next;
        }
        return A;
    }
}
~~~

击败99%，感觉差别不大吧。

### Q[344反转字符串](https://leetcode-cn.com/problems/reverse-string/)

难度 简单

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `char[]` 的形式给出。

不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 [ASCII](https://baike.baidu.com/item/ASCII) 码表中的可打印字符。

 

**示例 1：**

```
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

**示例 2：**

```
输入：["H","a","n","n","a","h"]
输出：["h","a","n","n","a","H"]
```

直接上代码

~~~java
class Solution {
    public void reverseString(char[] s) {
        char ch;
        int n = s.length;
        if(n == 0) return;
        for (int i = 0; i < n / 2; i++) {
            ch = s[i];
            s[i] = s[n - i - 1];
            s[n - i - 1] = ch;
        }
    }
}
~~~

击败99%

### Q[437路径总和 III](https://leetcode-cn.com/problems/path-sum-iii/)

难度 中等

给定一个二叉树，它的每个结点都存放着一个整数值。

找出路径和等于给定数值的路径总数。

路径不需要从根节点开始，也不需要在叶子节点结束，但是路径方向必须是向下的（只能从父节点到子节点）。

二叉树不超过1000个节点，且节点数值范围是 [-1000000,1000000] 的整数。

**示例：**

```
root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1

返回 3。和等于 8 的路径有:

1.  5 -> 3
2.  5 -> 2 -> 1
3.  -3 -> 11
```

刚开始看了题解，用一个arraylist记录路径，每次更新一遍

~~~java
import java.util.*;
class Solution {
    int count = 0;
    public int pathSum(TreeNode root, int sum) {
        ArrayList<Integer> list = new ArrayList();
        dfs(root,list,sum);
        return count;
    }
    public void dfs(TreeNode root,ArrayList<Integer> list,int sum) {
        if(root == null) return;
        for(int i = 0; i < list.size(); i++){
            list.set(i,list.get(i)+root.val);
            if(list.get(i) == sum)
                count++;
        }
        list.add(root.val);
        if(root.val == sum)
            count++;
        dfs(root.left,new ArrayList<Integer>(list),sum);
        dfs(root.right,new ArrayList<Integer>(list), sum);
    }
}
~~~

击败9%，感觉每次生成一个arralist浪费了很多时间

参照一个据说击败99%的修改了一下。它搞的比较巧妙，用一个静态数组就搞定了。每次只记录当前节点的值，每次重新遍历整个数组，计算temp值。用形参p，来记录当前遍历的末尾

~~~java
import java.util.*;
class Solution {
    int count = 0;
    public int pathSum(TreeNode root, int sum) {
        int[] list = new int[1001];
        dfs(root,list,sum,0);
        return count;
    }
    public void dfs(TreeNode root,int[] list,int sum,int p) {
        if(root == null) return;
        if(sum == root.val) count++;
        int temp = root.val;
        for(int i = p-1; i >=0 ; i--){
            temp += list[i];
            if(temp == sum)
                count++;
        }
        list[p] = root.val;
        dfs(root.left,list,sum,p+1);
        dfs(root.right,list, sum,p+1);
    }
}
~~~

击败81%

### Q[394字符串解码](https://leetcode-cn.com/problems/decode-string/)

难度 中等

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 *encoded_string* 正好重复 *k* 次。注意 *k* 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 *k* ，例如不会出现像 `3a` 或 `2[4]` 的输入。

 

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

从头到尾遍历字符串。遇到字母直接加上。遇到数字 进入递归处理。

~~~java
class Solution {
    int i = 0;
    public String decodeString(String s) {
        if(s.length() == 0) return "";
        StringBuilder ans = new StringBuilder();
        int times = 0;
        for (; i < s.length(); i++) {
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                times = times * 10 + s.charAt(i) - '0';
            } else if (s.charAt(i) == '[') {
                StringBuilder temp = fun(s, i + 1);
                for (int j = 0; j < times; j++) {
                    ans.append(temp);
                }
                while (s.charAt(i) != ']') {
                    i++;
                }
                times = 0;
            } else {
                ans.append(s.charAt(i));
            }
        }
        return ans.toString();
    }

    public StringBuilder fun(String s, int pos) {
        StringBuilder ans = new StringBuilder();
        int times = 0;
        for (i = pos; i < s.length(); i++) {
            if(s.charAt(i) == ']')
                break;
            if (s.charAt(i) >= '0' && s.charAt(i) <= '9') {
                times = times * 10 + s.charAt(i) - '0';
            } else if (s.charAt(i) == '[') {
                StringBuilder temp = fun(s, i + 1);
                for (int j = 0; j < times; j++) {
                    ans.append(temp);
                }
                while (s.charAt(i) != ']') {
                    i++;
                }
                times = 0;
            } else {
                ans.append(s.charAt(i));
            }
        }
        return ans;
    }
}
~~~

击败100%

### Q[142环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii/)

难度中等642

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 `null`。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。

**说明：**不允许修改给定的链表。

 

**示例 1：**

```
输入：head = [3,2,0,-4], pos = 1
输出：tail connects to node index 1
解释：链表中有一个环，其尾部连接到第二个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist.png)

**示例 2：**

```
输入：head = [1,2], pos = 0
输出：tail connects to node index 0
解释：链表中有一个环，其尾部连接到第一个节点。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

**示例 3：**

```
输入：head = [1], pos = -1
输出：no cycle
解释：链表中没有环。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

 

**进阶：**
你是否可以不用额外空间解决此题？

快慢指针。之前做过一次类似的。假设头到环距离为k，从环开始到相遇距离为t，环长l。慢指针走过的路k+l，快指针走过2（k+l） = k + t +l；l = k+t。此时把快指针挪到头结点，两个指针每次只走一。（快指针离头结点距离 k，慢指针离头结点距离l-t = k）相遇点即为环开始

~~~java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if(head == null) return null;
        ListNode quick = head, slow = head;
        slow = slow.next;
        quick = quick.next;
        if(quick == null) return null;
        quick = quick.next;
        while(quick!=null&&quick != slow){
            quick = quick.next;
            if(quick == null) return null;
            quick = quick.next;
            slow = slow.next;
        }
        if(quick == null)
            return null;
        quick = head;
        while(quick != slow){
            quick = quick.next;
            slow = slow.next;
        }
        return quick;
    }
}
~~~

不知道为啥只击败38%

### Q[543二叉树的直径](https://leetcode-cn.com/problems/diameter-of-binary-tree/)

难度简单496

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

 

**示例 :**
给定二叉树

```
          1
         / \
        2   3
       / \     
      4   5    
```

返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

 

**注意：**两结点之间的路径长度是以它们之间边的数目表示。

没啥难度，每次合并下左右，与最大值去比较。

~~~java
class Solution {
    int maxlength = 0;
    public int diameterOfBinaryTree(TreeNode root) {
        rootlength(root);
        return maxlength;
    }
    public int rootlength(TreeNode root) {
        if(root == null) return 0;
        int left = rootlength(root.left);
        int right = rootlength(root.right);
        maxlength = maxlength>right+left?maxlength:right+left;
        return Math.max(left,right)+1;
    }
}
~~~

击败18%，不知道为啥这么低