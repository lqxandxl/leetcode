# [130] 被围绕的区域
```
给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。

找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

示例:

X X X X
X O O X
X X O X
X O X X
运行你的函数后，矩阵变为：

X X X X
X X X X
X X X X
X O X X
解释:

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。
```


dfs做法，注意row和col分别代表什么。
从四个边开始向内伸展，只要是O就伸展，临时改为Z。

最后只要是Z的都是外边界的O不用管，还原即可。里面的O变化为X。

```
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        row=-1
        col=-1
        def dfs(board,x,y):
            nonlocal row,col
            if x>=0 and x<col and y>=0 and y<row:
                if board[y][x] == 'O':
                    board[y][x]='Z'
                    dfs(board,x+1,y)
                    dfs(board,x-1,y)
                    dfs(board,x,y+1)
                    dfs(board,x,y-1)
            else:
                return
        if board == None:
            return
        row = len(board)
        if row==0:
            return
        col = len(board[0])
        for index in range(0,col):
            dfs(board,index,0)
            dfs(board,index,row-1)
        for index in range(0,row):
            dfs(board,0,index)
            dfs(board,col-1,index)
        for i in range(0,row):
            for j in range(0,col):
                if board[i][j]=='Z':
                    board[i][j]='O'
                elif board[i][j]=='O':
                    board[i][j]='X'
        return
```


## bfs做法

list就能当队列使用。bfs的关键词在于队列。

```
class Solution:
    def solve(self, board: List[List[str]]) -> None:
        """
        Do not return anything, modify board in-place instead.
        """
        row=-1
        col=-1
        if board == None:
            return
        row = len(board)
        if row==0:
            return
        col = len(board[0])
        q=[]
        for index in range(0,col):
            q.append([0,index])
            q.append([row-1,index])
        for index in range(0,row):
            q.append([index,0])
            q.append([index,col-1])
        while len(q)!=0:
            l=list(q[0])
            q.pop(0)
            if l[0]>=0 and l[0]<=row-1 and l[1]>=0 and l[1]<=col-1:
                if board[l[0]][l[1]]=='O':
                    board[l[0]][l[1]]='Z'
                    q.append([l[0]-1,l[1]])
                    q.append([l[0]+1,l[1]])
                    q.append([l[0],l[1]+1])
                    q.append([l[0],l[1]-1])
        for i in range(0,row):
            for j in range(0,col):
                if board[i][j]=='Z':
                    board[i][j]='O'
                elif board[i][j]=='O':
                    board[i][j]='X'
        return
```


# [102] 二叉树的层序遍历
```
给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

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

```

二叉树的层序遍历，使用list充当队列。
因为输出的关系，需要存储每一个结点所在的层数，方便输出到最后结果。

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        q = []
        if root == None:
            return []
        q.append([root,1])
        res=[]
        while len(q)!=0:
            node = q[0]
            if node[1]>len(res):
                res.append([node[0].val])
            else:
                res[node[1]-1].append(node[0].val)
            if node[0].left!=None:
                q.append([node[0].left,node[1]+1])
            if node[0].right!=None:
                q.append([node[0].right,node[1]+1])
            q.pop(0)
        return res
```