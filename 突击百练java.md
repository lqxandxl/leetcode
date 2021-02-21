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