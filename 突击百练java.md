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

# [21] 合并两个有序链表
```
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if(l1==null && l2==null){
            return null;
        }
        if(l1==null || l2==null){
            if(l1==null)
                return l2;
            if(l2==null)
                return l1;
        }
        List<Integer> list = new ArrayList<>();
        while(l1!=null){
            list.add(l1.val);
            l1=l1.next;
        }
        while(l2!=null){
            list.add(l2.val);
            l2=l2.next;
        }
        Collections.sort(list); //排序
        ListNode root=null;
        ListNode cur = null;
        for(Integer i: list){
            if(root==null){
                root = new ListNode(i);
                cur = root;
            }
            else{
                ListNode tmp = new ListNode(i);
                cur.next = tmp;
                cur = tmp;
            }
        }
        return root;
    }
}
```

# [22]括号生成
1,static变量无法在leetcode刷题时保证正确性。会覆盖之前的结果。实际上leetcode肯定是不断运行此函数，所以static变量需要在函数之初进行初始化。

2,回溯实际上也是dfs，dfs的过程中需要存储中间结果，且可以回退状态。这道题实际上就是dfs，下面的解并没有回退状态。只是记录了不合法的状态是右括号多于左括号。或者左括号数量太多。

```
class Solution {
    //static变量无法在leetcode刷题时保证正确性
    public static int _num = 0;
    public static List<String> _res = new ArrayList<>(); 
    public List<String> generateParenthesis(int n) {
        _num = n;
        _res.clear();//clear
        dfs(0,0,"");
        return _res;
    }
    public void dfs(int left,int right,String path){
        if(right>left){
            return;
        }
        if(left>_num){
            return;
        }
        if(path.length()==_num*2 && left == right){
            _res.add(path);
            return;
        }
        else if(path.length()>=_num*2){
            return;
        }
        dfs(left+1,right,path+"(");
        dfs(left,right+1,path+")");
    }
}
```

# [31] 下一个排序
1,掌握reverse的实现办法。

2，Comparator只能对Integer生效而不是int。所以这里不能考虑Comparator。

3，这道题的核心关键是从后往前找到第一个顺序对。假如a[i]小于a[j] ，证明a[j]到数组的最后都是逆序。

这个时候将a[i]和后面的逆序数组中仅仅比a[i]大一点点的数字交换。交换后后序从a[j]到最后依然是逆序数组，这个时候进行reverse操作将后面的数组变成顺序数组即可。

4，特殊情况：全顺序数组，这个其实一上来就是顺序对，直接交换即可。全逆序数组，也就是找不到顺序对，那么直接reverse操作即可。

```
class Solution {
    public void nextPermutation(int[] nums) {
        int pre=nums.length-2;
        int cur=nums.length-1;
        //find a[pre]<a[cur]
        while(pre>=0 && nums[pre]>=nums[cur]){
            pre--;
            cur--;
        }
        if(pre>=0){ //find
            //从pre后面找一个合适的数字和pre进行交换，刚好比pre大一点点即可
            int k = nums.length - 1;
            while(nums[k]<=nums[pre]){
                k--;
            }
            //swap k,pre
            int tmp = nums[k];
            nums[k] = nums[pre];
            nums[pre] = tmp;
        }
        //交换后的后面的内容进行顺序排序 交换后必然是逆序 reverse cur-->end
        int i=cur;
        int j=nums.length-1;
        while(i<j){
            int tmp = nums[i];
            nums[i] = nums[j];
            nums[j] = tmp;            
            i++;
            j--;
        }
    }
}
```


# [33] 搜索旋转数组

这道题也是做了很多次，每做一次就忘记一次。

二分查找的过程在于准确的扔下一半的数据。

对于旋转排序数组，切一刀，肯定一半是有序的。（如何判断数组是有序的成为了关键，给出的数组是有序数组只不过被切分了）

在这两个前提下，我们优先找有序的一半并且在这一半里看看是不是target在里面。如果不在那只能去找无序的另一半（可以肯定就在另一半里了）。

所以算法的思路就是： 先确定有序的这一段，有序的里面可以快速判断有没有target，从而快速知道target在我们划分的哪一段里。这是保证正确性的关键。

```
class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]==target){
                return mid;
            }
            else if(nums[left]<=nums[mid]){ //左边有序
                if(target>=nums[left] && target<=nums[mid]){
                    right = mid-1; //如果target在有序的里面就在有序的里面去找
                }
                else{
                    left = mid+1;//不在那肯定在另一半里
                }
            }
            else if(nums[mid]<=nums[right]){ //右边有序
                if(target>=nums[mid] && target<=nums[right]){
                    left = mid+1;
                }
                else{
                    right = mid-1;
                }
            }
        }

        return -1;

    }
}
```


# [34] 查找一个元素的区间范围 (二分查找经典题目)

1,首先要确定是否存在该数字。

2，其次是找到上边界和下边界。

二分查找的一些思路：

首先是循环不变量的两种形态。

```left<=right```这种形态，代表了left可以==right，搜索的区间可以搜到right，因此是左闭右闭的搜索。那么移动的时候就要mid=left+1和mid=right-1。 退出循环时left>right。

```left<right```这种形态代表了left==right时就要退出循环，代表了左闭右开的区间，因为left永远不能等于right，从而判断right位置的值。因此移动的时候left=mid+1，right=mid（此处需要记忆）。

```
class Solution {
    public int[] searchRange(int[] nums, int target) {
        int left = 0;
        int right = nums.length-1;
        int find = 0;
        //先确定是否存在
        while(left<=right){ //[]
            int mid = left + (right-left)/2;
            if(nums[mid]==target){
                find=1;
                break;
            }
            else if(nums[mid]<target){
                left = mid+1;
            }
            else{
                right = mid-1;
            }
        }
        if(find==0){
            return new int[]{-1,-1};
        }
        //找上边界
        left=0;
        right=nums.length-1;
        int up=-1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]==target){
                left++; //为了找到上边界 left不断递增 （如何证明right一定<=数字的上边界以防止left++提前退出导致漏解？）
            }
            else if(nums[mid]<target){
                left = mid+1;
            }
            else{
                right = mid-1;
            }
        }
        up = left;
        //找下边界
        left=0;
        right=nums.length-1;
        int down=-1;
        while(left<=right){
            int mid = left + (right-left)/2;
            if(nums[mid]==target){
                right--; //为了找到下边界 right不断递减
            }
            else if(nums[mid]<target){
                left = mid+1;
            }
            else{
                right = mid-1;
            }
        }
        down = right;  
        return new int[]{down+1,up-1};      
    }
}
```

# [39] 组合总和
不允许结果重复，意味着递归时需要知道自己的起点。
递归的同时也需要缓存中间结果。

```
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        dfs(target,new ArrayList<Integer>(),0,res,candidates);
        return res;

    }
    public void dfs(int target,ArrayList<Integer> path,int start,List<List<Integer>> res,int[] candidates){
        if(target==0){
            res.add((List<Integer>)path.clone()); //只有ArrayList有clone方法，clone是浅拷贝，也就是Integer还是那个Integer
            return;
        }
        if(target<0)
            return;
        for(int i=start;i<candidates.length;++i){
            ArrayList<Integer> tmp =(ArrayList<Integer>)path.clone(); //clone返回时Object，需要对引用进行类型转化。
            tmp.add(candidates[i]);
            dfs(target-candidates[i],tmp,i,res,candidates); //传入为i是最关键的一步
        }
    }
}
```


# [42] 接雨水 （数组、双指针）

思路是从左往右扫描一次，从右往左扫描一次。

定理一： 能接的雨水取决于左侧出现过的最大值，右侧出现过的最大值，两个值的最小值 - 自己本身的高度 就是自己能承载的雨水。

定理二： 从左边开始扫描的时候，可以确定左边的最大值，但是不能确定右边的最大值。

从左扫描一遍，从右扫描一遍，且记录下左侧or右侧最大值-自己的高度。那么综合来看就满足了定理一，可以推断出能接的雨水。

能否想起来定理一是解这道题的关键所在。
```
class Solution {
    public int trap(int[] height) {
        int nums_len = height.length;
        int [] left_nums = new int[nums_len];
        int [] right_nums = new int[nums_len];
        //left
        int left_max = 0;
        for(int i=0;i<nums_len;++i){
            if(height[i]>left_max){
                left_max = height[i];
                left_nums[i]=0;
            }
            else{
                left_nums[i]=left_max - height[i];
            }
        }
        //right
        int right_max =0;
        for(int i=nums_len-1;i>=0;--i){
            if(height[i]>right_max){
                right_max = height[i];
                right_nums[i]=0;
            }
            else{
                right_nums[i]=right_max - height[i];
            }
        }
        int res=0;
        for(int i=0;i<nums_len;++i){
            res += Math.min(left_nums[i],right_nums[i]);
        }
        return res;
    }
}
```


# [46] 全排列 （经典回溯问题）

回溯问题一定要画图。做到心中有图。dfs时需要保存路径，保存剩余内容（知道自己遍历到哪里了）。

dfs还原现场需要保存原先的值。

```

class Solution {
    public static List<List<Integer>> res = new ArrayList<>(); //static的使用
    public List<List<Integer>> permute(int[] nums) {
        res.clear();
        ArrayList<Integer> remain = new ArrayList<>();
        for(int num:nums){
            remain.add(num);
        }
        ArrayList<Integer> path = new ArrayList<>();
        dfs(remain,path);
        return res;
    }
    public void dfs(ArrayList<Integer> remain,ArrayList<Integer> path){
        if(remain.size()==0){
            res.add( (ArrayList)(path.clone()) ); //clone的使用
            return;
        }
        for(int i=0;i<remain.size();++i){
            int tmp = remain.get(i);
            path.add(tmp);
            remain.remove(i);
            dfs(remain,path);
            remain.add(i,tmp); //还原现场
            path.remove(path.size()-1);
        }
    }
}

```

# [48] 旋转图像

首先使用矩阵转置，其次列列对称交换。 其实就是矩阵找规律的题目。多用用转置和交换等等。

```
class Solution {
    public void rotate(int[][] matrix) {
        int row = matrix.length;
        if(row==0){
            return;
        }
        int col = matrix[0].length;
        //转置
        //(i,j) <--> (j,i)
        for(int i=0;i<row;++i){
            for(int j=i;j<col;++j){
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = tmp;
            }
        }
        //对称列交换
        for(int j=0;j<col/2;++j){
            for(int i=0;i<row;++i){
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[i][col-j-1];
                matrix[i][col-j-1] = tmp;
            }
        }
    }
}
```

# [49] 字母异位词分组

将具备相同字母的字符串进行分组。需要使用字典。key为排序后字符串，value为下属字符串。
```
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        Map<String,List<String> > dict = new HashMap<>();
        List<List<String>> res = new ArrayList<>();
        for(String s:strs){
            String key = keyOfWord(s);
            if(dict.containsKey(key)){
                List<String> list = dict.get(key);
                list.add(s);
            }
            else{
                List<String> list = new ArrayList<>();
                list.add(s);
                dict.put(key,list);
            }
        }
        //iterator map
        for(Map.Entry<String,List<String>> entry:dict.entrySet()){
            res.add(entry.getValue());
        }
        return res;
    }
    public String keyOfWord(String str){
        String key = null;
        char[] array = str.toCharArray(); //String to char[]
        Arrays.sort(array);
        key = String.valueOf(array); //char[] to String
        return key;
    }
}
```

# [53] 最大子序和

dp数组存储每个阶段的最大子序和。令人困惑的点在于，这个最大子序和dp[i]是否一定要包含nums[i]。

dp数组存储了每个阶段的结果，为了方便理解，应该规定dp必须存储当前位置的数字。1...i-1的dp为-10，i为-9 or -11。dp[i]应该是包含nums[i]的。

负数不给后面的计算提供借鉴。
```

class Solution {
    public int maxSubArray(int[] nums) {
        int nums_len = nums.length;
        int[] dp = new int[nums_len];
        int max = 0x80000000;
        for(int i =0;i<nums.length;++i){
            if(i==0){
                dp[i] = nums[i];
            }
            else{
                if(dp[i-1]>0){
                    dp[i] = dp[i-1] + nums[i];
                }
                else{
                    dp[i] = Math.max(nums[i],dp[i-1]); //这里直接等于nums[i]也可以通过编译
                }
            }
            if(dp[i]>max){
                max = dp[i];
            }
        }
        return max;
    }
}

```

# [55] 跳跃游戏

dp数组确定每一个位置能否到达终点。这样可以避免很多子问题的重复计算，避免超时。

dp[index] = true ; 表示从index的位置是可以到达终点的。

```
class Solution {
    static int[] _nums;
    static int[] _dp;
    public boolean canJump(int[] nums) {
        _nums = nums;
        _dp = new int[nums.length];
        return dfs(0);
    }
    public boolean dfs(int index){
        if(index >= _nums.length-1){
            return true;
        }
        if(_dp[index]!=0){ //利用缓存避免多余的递归
            if(_dp[index]==1){
                return true;
            }
            else{
                return false;
            }
        }
        if(_nums[index]==0){ //为0代表无法到达终点
            _dp[index]=-1;
            return false;
        }
        int step = _nums[index];
        boolean res=false;
        for(int i=1;i<=step;++i){
            res = res|dfs(index+i); //单|避免截断
        }
        if(res==true)
            _dp[index]=1;
        else
            _dp[index]=-1;
        return res;
    }
}
```

# [56] 合并区间

合并区间是一道很锻炼人的题目，其中涉及了二维数组的排序，二维数组和List之间的转化。
List在迭代过程中的删除之后的变化等等。

思路就是合并一次后重新遍历整个数组，如果有重复就再次合并，直到没有重复为止。

```
class Solution {
    public int[][] merge(int[][] intervals) {
        int count=0;
        int[][] res=null;
        Arrays.sort(intervals,new Comparator<int[]>(){
            @Override //考场上不写这个也行
            public int compare(int[] o1,int []o2){
                if(o1[0]==o2[0]) return o1[1]-o2[1];
                return o1[0]-o2[0];
            }
        });
        List<List<Integer>> cur = new ArrayList<>();
        for(int i=0;i<intervals.length;++i){
            List<Integer> tmp = new ArrayList<>();
            for(int j=0;j<intervals[0].length;++j){
                tmp.add(intervals[i][j]);
            }
            cur.add(tmp);
        }
        do{
            count=0;
            int row = cur.size();
            if(row==0){
                break;
            }
            int col = cur.get(0).size();
            if(col==0){
                break;
            }
            for(int i=0;i<row-1;++i){
                List<Integer> r1 = cur.get(i);
                List<Integer> r2 = cur.get(i+1);
                if(r1.get(1)>=r2.get(0)){
                    Integer i1 = r1.get(0);
                    Integer i2 = Math.max(r2.get(1),r1.get(1)); //不要漏掉情况 比如后面的区间完全在第一个区间内部
                    List<Integer> l2 = new ArrayList<>();
                    l2.add(i1);
                    l2.add(i2);
                    cur.remove(i);
                    cur.remove(i);//后面元素前移
                    cur.add(i,l2);
                    count++;
                    break;
                }
            }
        }while(count!=0);
        res = new int[cur.size()][2];
        for(int i=0;i<cur.size();++i){
            res[i][0] = cur.get(i).get(0);
            res[i][1] = cur.get(i).get(1);
        }
        return res;
    }
}
```

# [62] 不同路径

基本的动态规划。

```
class Solution {
    public int uniquePaths(int m, int n) {
        int[][] dp = new int[m][n]; //二维数组的创建方式
        for(int i=0;i<m;++i){
            for(int j=0;j<n;++j){
                if(i==0|j==0){
                    dp[i][j]=1;
                }
                else{
                    dp[i][j] = dp[i-1][j]+dp[i][j-1];
                }
            }
        }
        return dp[m-1][n-1];
    }
}
```

# [64] 最小路径和

也是一道常规动态规划题。

```
class Solution {
    public int minPathSum(int[][] grid) {
        int row = grid.length;
        int col = grid[0].length;
        int [][] dp = new int[row][col];
        for(int i=0;i<row;++i){
            for(int j=0;j<col;++j){
                if(i==0|j==0){
                    if(i==0&&j==0){
                        dp[i][j]=grid[i][j];
                    }
                    else{
                        if(i==0){
                            dp[i][j] = dp[i][j-1]+grid[i][j];
                        }
                        else{
                            dp[i][j] = dp[i-1][j]+grid[i][j];
                        }
                    }
                }
                else{
                    dp[i][j] = Math.min(dp[i-1][j],dp[i][j-1])+grid[i][j];
                }
            }
        }
        return dp[row-1][col-1];
    }
}
```

# [70] 爬楼梯

经典dp题,都是从简单问题开始考虑的。

```

class Solution {
    public int climbStairs(int n) {
        if(n<=2){
            return n;
        }
        int [] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 2;
        for(int i=3;i<=n;++i){
            dp[i] = dp[i-1]+dp[i-2];
        }
        return dp[n];
    }
}

```

# [75] 颜色分类

数组排序问题，使用快排。

```
class Solution {
    public void sortColors(int[] nums) {
        quickSort(nums,0,nums.length-1);
    }
    public void quickSort(int[] nums,int begin,int end){ //需要死记硬背 需要先想起来快排的基本函数形式
        if(begin>end){
            return;
        }
        int low = begin;
        int high = end;
        int mid_value = nums[low];
        while(low<high){
            while(mid_value<=nums[high]&&low<high){ //不断进行筛选 把比mid大的值放右边，小的放左边
                high--;
            }
            nums[low] = nums[high];

            while(nums[low]<=mid_value&&low<high){
                low++;
            }
            nums[high] = nums[low];
        }
        //low==high时退出循环
        nums[low] = mid_value;
        quickSort(nums,begin,low-1);
        quickSort(nums,low+1,end);
    }
}
```


# [78] 子集
经典回溯问题，心中有图，且利用模板即可。
```
class Solution {
    public static List<List<Integer>> res;
    public List<List<Integer>> subsets(int[] nums) {
        res=new ArrayList<>();
        dfs(nums,0,new ArrayList<>());
        return res;
    }
    public void dfs(int[] nums,int start,ArrayList<Integer> path){
        res.add((ArrayList<Integer>)path.clone());
        for(int i=start;i<nums.length;++i){
            path.add(nums[i]);
            dfs(nums,i+1,path);
            path.remove(path.size()-1);
        }
    }
}
```

# [79] 单词搜索

这道题从每一个矩阵的点开始出发，开始探索。

坑的第一点就是，不能走回头路，不能重复，所以在dfs的时候要知道自己从哪里来以避免走回头路。

坑的第二点就是，为了避免走到重复的点，需要记录自己走过了哪些点。

坑的第三点就是，由于记录了走过哪些点，当尝试一条路失败以后，会破环现场环境，所以当这条路失败的时候应该还原现场，仿佛自己没走过。

坑的第四点就是，传入的word字符串是引用，应该尽量避免不同尝试后对word的修改对结果产生不好的影响。简而言之就是注意传参。

```
class Solution {
    public static int[][] record = null;
    public boolean exist(char[][] board, String word) {
        int row = board.length;
        int col = board[0].length;
        for(int i=0;i<row;++i){
            for(int j=0;j<col;++j){
                record = new int[row][col];
                if(exist(board,i,j,word,0)){
                    return true;
                }
            }
        }        
        return false;
    }
    public boolean exist(char[][] board,int x,int y,String word,int direct){
        int row = board.length;
        int col = board[0].length;
        if(x<0||y<0||x>=row||y>=col){
            return false;
        }
        if(word.length()==0){
            return false;
        }
        if(record[x][y]==1){
            return false; //have use it
        }
        //System.out.println(String.valueOf(x)+":"+String.valueOf(y));
        if(word.length()>=1){
            if(board[x][y] == word.charAt(0)){
                record[x][y] = 1;
                if(word.length()>1){
                    //4 direction but no reback
                    String new_word = null;
                    boolean res;
                    if(direct==0){
                        new_word=word.substring(1);
                        res = exist(board,x-1,y,new_word,1)|exist(board,x+1,y,new_word,2)
                    |exist(board,x,y-1,new_word,3)|exist(board,x,y+1,new_word,4);
                    }
                    else if(direct==1){//from down
                        new_word=word.substring(1);
                        res = exist(board,x-1,y,new_word,1)
                    |exist(board,x,y-1,new_word,3)|exist(board,x,y+1,new_word,4);
                    }
                    else if(direct==2){//from  up
                        new_word=word.substring(1);
                            res = exist(board,x+1,y,new_word,2)
                        |exist(board,x,y-1,new_word,3)|exist(board,x,y+1,new_word,4);
                    }
                    else if(direct==3){//from right
                        new_word=word.substring(1);
                        res = exist(board,x-1,y,new_word,1)|exist(board,x+1,y,new_word,2)
                    |exist(board,x,y-1,new_word,3);                       
                    }
                    else{
                        new_word=word.substring(1);
                        res = exist(board,x-1,y,new_word,1)|exist(board,x+1,y,new_word,2)
                    |exist(board,x,y+1,new_word,4);
                    }
                    if(res==false){
                        record[x][y] = 0;
                    }
                    return res;
                }
                return true;
            }
            else{
                record[x][y]=0;
                return false;
            }
        }
        return false;
    }
}
```

# [94] 二叉树的中序遍历

dfs解决遍历问题。

```
class Solution {
    public static List<Integer> _res;
    public List<Integer> inorderTraversal(TreeNode root) {
        _res = new ArrayList<>();
        dfs(root);
        return _res;
    }
    public void dfs(TreeNode root){
        if(root==null){
            return;
        }
        dfs(root.left);
        _res.add(root.val);
        dfs(root.right);
    }
}
```

# [96] 不同的二叉搜索树
经典的dp问题，最终结果是由左子树和右子树形态的累加之和。

递推公式见代码。

```
class Solution {
    public int numTrees(int n) {
        if(n<=1){
            return n;
        }
        //dp[n] = dp[l] * dp[n-1-l] l=0...n-1
        int[] dp = new int[n+1];
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 2;
        for(int i=3;i<=n;++i){
            for(int j=0;j<=i-1;++j){
                if(dp[j]==0|dp[i-1-j]==0){
                    dp[i]+=dp[j]+dp[i-1-j];
                }
                else{
                    dp[i]+=dp[j]*dp[i-1-j];
                }
            }
        }
        return dp[n];
    }
}
```


# [98] 验证二叉搜索树

中序遍历即可。

```
class Solution {
    public static ArrayList<Integer> _list;
    public boolean isValidBST(TreeNode root) {
        _list = new ArrayList<>();
        dfs(root);
        for(int i=1;i<_list.size();++i){
            if(_list.get(i) <= _list.get(i-1)){
                return false;
            }
        }
        return true;
    }
    public void dfs(TreeNode root){
        if(root==null){
            return;
        }
        dfs(root.left);
        int tmp = root.val;
        _list.add(tmp);
        dfs(root.right);
    }
}
```