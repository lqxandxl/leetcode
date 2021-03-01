# [1] 两数之和 //数组
```

class Solution {
    public int[] twoSum(int[] nums, int target) {
        if(nums==null || nums.length<=1){
            return null;//error input
        }
        int[] res = new int[2];
        Map<Integer,Integer> indexMap = new HashMap<>(); 
        int index=0;
        for(int num:nums){//jdk 5 later is ok
            indexMap.put(num,index);
            index++;
        }
        index=0;
        for(int num:nums){
            if(indexMap.containsKey(target-num)==true && indexMap.get(target-num)!=index){ //可以是相等的数字但不能是重复的数字
                res[0] = Math.min(indexMap.get(target-num),index);
                res[1] = Math.max(indexMap.get(target-num),index);
            }
            index++;
        }
        return res;
    }
}

```

# [2] 两数相加 //链表
要考虑情况：
1，双方都走到尽头后，如果进位有1，应该多一个结点。如果进位是0，则不应该新增结点。

2，其中一方走尽，要考虑另一方继续走。

3，都走到尽头，进位是1，那么需要新增一个结点，该结点值为1。
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode root = new ListNode(0);
        ListNode res = root;
        int up=0;
        while(l1!=null && l2!=null){
            if(res==root){
                //first
                if(l1.val+l2.val>=10){
                    root.val = l1.val+l2.val-10;
                    up=1;
                }
                else{
                    root.val = l1.val+l2.val;
                    up=0;
                }
                ListNode tmp = new ListNode();
                root.next = tmp;
                if(l1.next==null && l2.next==null && up==0){ //截断，否则新增多余结点，下同
                    root.next=null;
                }
                root = tmp;
            }
            else{ //不是第一次相加则需要考虑之前的进位
                if(l1.val+l2.val+up>=10){
                    root.val = l1.val+l2.val-10 + up;
                    up=1;
                }
                else{
                    root.val = l1.val+l2.val + up;
                    up=0;
                }
                ListNode tmp = new ListNode();
                root.next = tmp;
                if(l1.next==null && l2.next==null && up==0){
                    root.next=null;
                }
                root = tmp;                
            }
            l1=l1.next;
            l2=l2.next;
        }
        ListNode cur=null;
        if(l1!=null){
            cur=l1;
        }
        else if (l2!=null){
            cur=l2;
        }
        while(cur!=null){ //一方走到尽头需要继续判断另一方
            if(res==root){
                //first
                if(cur.val>=10){
                    root.val = cur.val-10;
                    up=1;
                }
                else{
                    root.val = cur.val;
                    up=0;
                }
                ListNode tmp = new ListNode();
                root.next = tmp;
                if(cur.next==null && up==0){
                    root.next=null;
                }
                root = tmp;
            }
            else{
                if(cur.val+up>=10){
                    root.val = cur.val-10 + up;
                    up=1;
                }
                else{
                    root.val = cur.val + up;
                    up=0;
                }
                ListNode tmp = new ListNode();
                root.next = tmp;
                if(cur.next==null && up==0){
                    root.next=null;
                }
                root = tmp;                
            }       
            cur=cur.next;     
        }
        if(up==1 && root!=null){
            root.val=1; //都走到尽头，进位是1，证明需要需要多一个结点，该结点数值是1
        }
        return res;
    }
}
```

# [3] 最长无重复子串 //字符串
```
class Solution {
    public int lengthOfLongestSubstring(String s) {
        Map<Character,Integer> hashMap = new HashMap<>(); //包装类进入容器
        if(s==null){
            return 0;
        }
        if(s.length()<=1){
            return s.length();
        }
        int begin=0;
        int end=1;  
        int res=1;
        hashMap.put(s.charAt(begin),begin);
        while(begin<end && end<s.length()){
            Character cur_c = s.charAt(end);
            if(hashMap.containsKey(cur_c)){
                begin = hashMap.get(cur_c)+1<begin?begin:hashMap.get(cur_c)+1; //这一步很关键 滑动之后 map中的一部分数据就失效了，落在了窗口外面，不能拿来更新左窗口值
                hashMap.put(cur_c,end);
            }
            else{
                hashMap.put(cur_c,end);
            } 
            if(end-begin+1>res){
                res = end-begin+1;
            }
            end++;
        }      

        return res;
    }
}
```

# [4] 寻找两个正序数组的中位数
```
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        List<Integer> list = new ArrayList<>();
        for(int num:nums1){
            list.add(num);
        }
        for(int num:nums2){
            list.add(num);
        }
        Collections.sort(list); //基本排序
        System.out.println(list);
        if(list.size()%2==0){
            return (float)(list.get(list.size()/2)+list.get((list.size()/2) - 1))/2; //避免取整
        }
        else{
            return list.get(list.size()/2);
        }
    }
}
```

# [5] 最长回文子串
使用中心开花法以及注意奇偶两种情况分别考虑。
```
class Solution {
    public String longestPalindrome(String s) {
            int s_len = s.length();
            String res_s="";
            for(int i=0;i<s_len;++i){
                if(judge1(s,i).length()>res_s.length()){
                    res_s = judge1(s,i);
                }
            }
            //偶数
            for(int i=0;i<s_len;++i){
                if(judge2(s,i,i+1).length()>res_s.length()){
                    res_s = judge2(s,i,i+1);
                }
            }
            return res_s;
    }
    public String judge1(String s,int index){
        int res=0;
        int s_len = s.length();
        int i=0;
        String res_s="";
        while(index-i>=0 && index+i<=s_len-1){
            if(s.charAt(index-i) == s.charAt(index+i)){
                res++;
                res_s = s.substring(index-i,index+i+1);
            }
            else{
                break;
            }
            i++;
        }
        return res_s;
    }
    public String judge2(String s,int index1,int index2){
        int res=0;
        int s_len = s.length();
        int i=0;
        String res_s="";
        while(index1-i>=0 && index2+i<=s_len-1){
            if(s.charAt(index1-i) == s.charAt(index2+i)){
                res++;
                res_s = s.substring(index1-i,index2+i+1);
            }
            else{
                break;
            }
            i++;
        }
        return res_s;
    }
}
```

# [10] 正则表达式 （难，暂略）

# [11] 盛最多的水

移动短边就行。有数学性的证明。

```
class Solution {
    public int maxArea(int[] height) {
        if(height==null){
            return 0;
        }
        int nums_len = height.length;
        int begin=0;
        int end = nums_len-1;
        int res=0;
        while(begin<end){
            if((end-begin)*Math.min(height[end],height[begin]) > res){
                res = (end-begin)*Math.min(height[end],height[begin]) ;
            }
            if(height[begin]>height[end]){
                end--;
            }
            else{
                begin++;
            }
        }
        return res;
    }
}
```



# [15] 三数之和

特殊点在于不允许出现重复结果。所以需要对数组进行排序。
三指针做这一道题时，后二指针的移动也要避免重复结果，可以引入一个while循环来解决。

```
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        if(nums==null){
            return null;
        }
        int nums_len = nums.length;
        Arrays.sort(nums);
        List<List<Integer>> res=new ArrayList<>();
        for(int i=0;i<nums_len;++i){
            int begin = i+1;
            int end = nums_len-1;
            if(i>=1 && nums[i]==nums[i-1]){
                continue; //避免重复
            }
            while(begin>=0 && end < nums_len && begin<end){
                if(nums[i]+nums[begin]+nums[end]==0 ){
                    List<Integer> tmp = new ArrayList<Integer>();
                    tmp.add(nums[i]);
                    tmp.add(nums[begin]);
                    tmp.add(nums[end]);
                    res.add(tmp);
                    while(begin+1<nums_len && nums[begin]==nums[begin+1]){
                        begin++; //避免重复
                    }
                    begin++;
                    end--;
                }
                else if(nums[i]+nums[begin]+nums[end]<0){
                    begin++;
                }
                else{
                    end--;
                }
            }
        }
        return res;
    }
}
```

# [17] 电话号码组合

利用字典和递归思想去做这道题，尽量简化操作，想明白递归的目的是什么，每次递归只解决一个字母的问题。

```
class Solution {
    public static HashMap<String,String> _hashMap = new HashMap<>();
    static{ //static初始化
        _hashMap.put("2","abc");
        _hashMap.put("3","def");
        _hashMap.put("4","ghi");
        _hashMap.put("5","jkl");
        _hashMap.put("6","mno");
        _hashMap.put("7","pqrs");
        _hashMap.put("8","tuv");
        _hashMap.put("9","wxyz");  
    }
    public List<String> letterCombinations(String digits) {                                      
        if(digits==null||digits.length()==0){
            return new ArrayList<String>();
        }
        List<String> result = letterCombinations(digits.substring(1)); //子串
        List<String> self_list = new ArrayList<>();
        String value = _hashMap.get(String.valueOf(digits.charAt(0))); //char to string
        for(char c:value.toCharArray()){
            self_list.add(String.valueOf(c));
        }
        if(result.size()>0){ //should change the self_list
            List<String> swap_list = new ArrayList();
            for(int i=0;i<self_list.size();++i){
                for(int j=0;j<result.size();++j){
                    String tmp = self_list.get(i) +
                        result.get(j);
                    swap_list.add(tmp);
                }
            }
            self_list = swap_list;
        }  

        return self_list;

    }
}
```

# [19] 删除倒数第n个结点
只遍历一遍的话需要双指针距离为n，当快指针指向最后一个结点，证明慢指针指向了正确位置。

但作为笔试题能过就行的要求来说。遍历两次，使用容器存储结点更容易。

```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        List<ListNode> list = new ArrayList<>();
        ListNode cur = head;
        while(cur!=null){
            list.add(cur);
            cur=cur.next;
        }
        int del_index = list.size() - n; //计算出要删的结点 然后判断前面的结点存不存在 后面的结点存不存在 然后执行删除操作（变换指针指向）
        if(del_index<0 || del_index>=list.size()){
            return head;
        }
        int pre_index = del_index-1;
        int after_index = del_index+1;
        ListNode pre_node = null;
        ListNode after_node = null;
        if(pre_index>=0){
            pre_node=list.get(pre_index);
        }
        if(after_index<list.size()){
            after_node=list.get(after_index);
        }
        if(pre_node==null && after_node==null){
            return null;
        }
        if(pre_node==null){
            return after_node;
        }
        if(after_node==null){
            pre_node.next = null;
            return head;
        }
        pre_node.next = after_node;
        return head;
    }
}

```

# [20] 有效的括号

利用栈的思想做题，方便记忆则使用List去模拟Stack的行为。

```
class Solution {
    public boolean isValid(String s) {
        List<Character> stack = new ArrayList<>();
        for(Character c:s.toCharArray()){
            if(stack.size()==0){
                stack.add(c);
            }
            else{
                Character top = stack.get(stack.size()-1);
                if((top=='['&&c==']')||(top=='('&&c==')')||(top=='{'&&c=='}')){
                    stack.remove(stack.size()-1);
                }
                else{
                    stack.add(c);
                }
            }
        }
        if(stack.size()==0){
            return true;
        }
        return false;
        
    }
}
```