
94. Binary Tree Inorder Traversal  
--
也就是二叉树的中序遍历，作者用了一个stack来记录，还是比较厉害的
```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();

    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode cur = root;

    while(cur!=null || !stack.empty()){
        while(cur!=null){
            stack.add(cur);
            cur = cur.left;
        }
        cur = stack.pop();
        list.add(cur.val);
        cur = cur.right;
    }

    return list;
}
```
95.Unique Binary Search Trees II
--
```java
public class Solution {
    public List<TreeNode> generateTrees(int n) {
        
        return genTrees(1,n);
    }
        
    public List<TreeNode> genTrees (int start, int end)
    {

        List<TreeNode> list = new ArrayList<TreeNode>();

        if(start>end)
        {
            list.add(null);
            return list;
        }
        
        if(start == end){
            list.add(new TreeNode(start));
            return list;
        }
        
        List<TreeNode> left,right;
        for(int i=start;i<=end;i++)
        {
            
            left = genTrees(start, i-1);
            right = genTrees(i+1,end);
            
            for(TreeNode lnode: left)
            {
                for(TreeNode rnode: right)
                {
                    TreeNode root = new TreeNode(i);
                    root.left = lnode;
                    root.right = rnode;
                    list.add(root);
                }
            }
                
        }
        
        return list;
    }
}
```

96.Unique Binary Search Trees
--
https://discuss.leetcode.com/topic/8398/dp-solution-in-6-lines-with-explanation-f-i-n-g-i-1-g-n-i/2
```java
public int numTrees(int n) {
    int [] G = new int[n+1];
    G[0] = G[1] = 1;
    
    for(int i=2; i<=n; ++i) {
    	for(int j=1; j<=i; ++j) {
    		G[i] += G[j-1] * G[i-j];
    	}
    }

    return G[n];
}
```
97. Interleaving String
--
```java
public boolean isInterleave(String s1, String s2, String s3) {
    if ((s1.length()+s2.length())!=s3.length()) return false;
    boolean[][] matrix = new boolean[s2.length()+1][s1.length()+1];
    matrix[0][0] = true;
    for (int i = 1; i < matrix[0].length; i++){
        matrix[0][i] = matrix[0][i-1]&&(s1.charAt(i-1)==s3.charAt(i-1));
    }
    for (int i = 1; i < matrix.length; i++){
        matrix[i][0] = matrix[i-1][0]&&(s2.charAt(i-1)==s3.charAt(i-1));
    }
    for (int i = 1; i < matrix.length; i++){
        for (int j = 1; j < matrix[0].length; j++){
            matrix[i][j] = (matrix[i-1][j]&&(s2.charAt(i-1)==s3.charAt(i+j-1)))
                    || (matrix[i][j-1]&&(s1.charAt(j-1)==s3.charAt(i+j-1)));   //1处
        }
    }
    return matrix[s2.length()][s1.length()];
}
```
用的是DP算法，特别要注意`1处`的代码，切勿写成
```java
if(path[h][k-1]+path[h-1][k]!=0 && (s1.substring(k,k+1).equals(s3.substring(k+h-1,k+h))
    						      || s2.substring(h,h+1).equals(s3.substring(k+h-1,k+h))))
    						path[h][k]=1;
```
这个写法的错误在与会对字符串重复使用，也就是用过的字符会再用一次。
98. Validate Binary Search Tree  
--
这道题注意二叉搜索树与二叉树的区别，二叉搜索树要求树左边所有点的值都要小于根的值，右边所有点的值都要大于根的值。
```java
public class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    public boolean isValidBST(TreeNode root, long minVal, long maxVal) {
        if (root == null) return true;
        if (root.val >= maxVal || root.val <= minVal) return false;
        return isValidBST(root.left, minVal, root.val) && isValidBST(root.right, root.val, maxVal);
    }
}
```
下面这种解法用的是中序遍历
```java
public boolean isValidBST(TreeNode root) {
   if (root == null) return true;
   Stack<TreeNode> stack = new Stack<>();
   TreeNode pre = null;
   while (root != null || !stack.isEmpty()) {
      while (root != null) {
         stack.push(root);
         root = root.left;
      }
      root = stack.pop();
      if(pre != null && root.val <= pre.val) return false;
      pre = root;
      root = root.right;
   }
   return true;
}
```
99. Recover Binary Search Tree 
--
作者仅仅用了简单的中序遍历，简洁优雅
https://discuss.leetcode.com/topic/3988/no-fancy-algorithm-just-simple-and-powerful-in-order-traversal
高效。
100.same tree
--
自己写的方法，用的是递归的思想，
```java
public class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
    	if(p==null && q==null)
    		return true;
    	if(p==null && q!=null)
    		return false;
    	if(p!=null && q==null)
    		return false;
    	else 
    	{
    		if(p.val==q.val)
    		return isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
    		else
    			return false;
    	}
	        
	    }
}
```
下面这个方法也是自己写的，用的中序遍历，仅仅击败1%的人，不太理想
```java
public class Solution {
    public boolean isSameTree(TreeNode p, TreeNode q) {
    	TreeNode curp=p;
    	TreeNode curq=q;
    	Stack<TreeNode> stp=new Stack<TreeNode>();
    	Stack<TreeNode> stq=new Stack<TreeNode>();
    	boolean flag=true;
    	if(p==null && q==null)
    		return true;
    	outer:while( (curp!=null || !stp.isEmpty())   && (curq!=null || !stq.isEmpty()) )
    	{
    		while(curp!=null || curq!=null)
    		{
    		  if(curp!=null && curq!=null)
    		  {
    			if(curp.val==curq.val)
    			{
    				stp.push(curp);
    				stq.push(curq);
    				curp=curp.left;
    				curq=curq.left;
    			}
    			else
    			{
    				flag=false;
    				break outer;
    			}
    		  }
    			else
    			{
    				flag=false;
    				break outer;
    			}
    		}
    		curp=stp.pop();
    		curq=stq.pop();
    		if(curq.val!=curp.val)
    		{
    			flag=false;
    			break outer;
    		}
    		else
    		{
    			curp=curp.right;
    			curq=curq.right;
    		}
    	}
    	if(flag==false)
    		return false;
    	else
    	{
    		if(curp==null && stp.isEmpty() && curq==null && stq.isEmpty())
    			return true;
    		else
    			return false;
    	}  
	    }
}
```
101.Symmetric Tree
--
下面这个做法是得分比较高的做法，作者用了递归方法，非常简洁
```java
public boolean isSymmetric(TreeNode root) {
    return root==null || isSymmetricHelp(root.left, root.right);
}

private boolean isSymmetricHelp(TreeNode left, TreeNode right){
    if(left==null || right==null)
        return left==right;
    if(left.val!=right.val)
        return false;
    return isSymmetricHelp(left.left, right.right) && isSymmetricHelp(left.right, right.left);
}
```
下面的是自己的做法，用的是中序遍历，分别遍历左边和右边，左边是以左根右的顺序，右边是以右根左的顺序遍历的。
```java
public class Solution {
    public boolean isSymmetric(TreeNode root) {
    	if(root==null)
    		return true;
    	if(root.left==null && root.right==null)
    		return true;
    	if(root.left==null || root.right==null)
    		return false;
    	else
    	{
    		boolean flag=true;
    		Stack<TreeNode> stl=new Stack<TreeNode>();
    		Stack<TreeNode> str=new Stack<TreeNode>();
    		TreeNode curl=root.left;
    		TreeNode curr=root.right;
    		stl.push(curl);
    		str.push(curr);
    		outer:while(!stl.isEmpty() && !stl.isEmpty())
    		{
    			while(curl!=null || curr!=null)
    			{
    				if(curl!=null && curr!=null)
    				{
    					if(curl.val!=curr.val)
    					{
    						flag=false;
    						break outer;
    					}
    					else
    					{
    						str.push(curr);
    						stl.push(curl);
    						curr=curr.right;
    						curl=curl.left;
    					}
    				}
    				else
    				{
    					flag=false;
    					break outer;
    				}
    			}
    			curl=stl.pop();
    			curr=str.pop();
    			curl=curl.right;
    			curr=curr.left;
    		}
    		if(flag==false)
    			return false;
    		else
    			return true;
    	}
    }
    }
```

104. Maximum Depth of Binary Tree
--
下面这种做法真的被惊艳到了！
```java
public int maxDepth(TreeNode root) {
        if(root==null){
            return 0;
        }
        return 1+Math.max(maxDepth(root.left),maxDepth(root.right));
    }
```
105.Construct Binary Tree from Preorder and Inorder Traversal
--
这道题的解法就是根据先序遍历找到根，然后再中序遍历中根左边就是左子树，右边就是右子树，然后递归。
https://discuss.leetcode.com/topic/3695/my-accepted-java-solution/2
```java
public TreeNode buildTree(int[] preorder, int[] inorder) {
    return helper(0, 0, inorder.length - 1, preorder, inorder);
}

public TreeNode helper(int preStart, int inStart, int inEnd, int[] preorder, int[] inorder) {
    if (preStart > preorder.length - 1 || inStart > inEnd) {
        return null;
    }
    TreeNode root = new TreeNode(preorder[preStart]);
    int inIndex = 0; // Index of current root in inorder
    for (int i = inStart; i <= inEnd; i++) {
        if (inorder[i] == root.val) {
            inIndex = i;
        }
    }
    root.left = helper(preStart + 1, inStart, inIndex - 1, preorder, inorder);
    root.right = helper(preStart + inIndex - inStart + 1, inIndex + 1, inEnd, preorder, inorder);
    return root;
}
```
108.Convert Sorted Array to Binary Search Tree
--
```java
public TreeNode sortedArrayToBST(int[] num) {
    if (num.length == 0) {
        return null;
    }
    TreeNode head = helper(num, 0, num.length - 1);
    return head;
}

public TreeNode helper(int[] num, int low, int high) {
    if (low > high) { // Done
        return null;
    }
    int mid = (low + high) / 2;
    TreeNode node = new TreeNode(num[mid]);
    node.left = helper(num, low, mid - 1);
    node.right = helper(num, mid + 1, high);
    return node;
}
```
109. Convert Sorted List to Binary Search Tree  
--
下面这种解法的思想就是找到链表的中间位置（具体做法就是设置一个fast和low的ListNode，让fast以两倍的速度，而slow以一倍的速度遍历，这样slow最终停止的位置就是中间位置）
```java
public TreeNode sortedListToBST(ListNode head) {
            // base case
            if (head == null) return null;
    
            ListNode slow = head;
            ListNode fast = head;
            ListNode prev = null;
            // find the median node in the linked list, after executing this loop
            // fast will pointing to the last node, while slow is the median node.
            while (fast.next != null) {
                fast = fast.next;
                if (fast.next == null) {
                    break;
                }
                fast = fast.next;
                prev = slow;
                slow = slow.next;
            }
            if (prev != null) prev.next = null;
            else head = null;
    
            TreeNode root = new TreeNode(slow.val);
            root.left = sortedListToBST(head);
            root.right = sortedListToBST(slow.next);
    
            return root;
        }
```
112.Path Sum
--
下面这种做法是递归方法
```java
public class Solution {
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null) return false;
    
        if(root.left == null && root.right == null && sum - root.val == 0) return true;
    
        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
    }
}
```
113. Path Sum II
```java
public List<List<Integer>> pathSum(TreeNode root, int sum) {
    List<List<Integer>>ret = new ArrayList<List<Integer>>(); 
    List<Integer> cur = new ArrayList<Integer>(); 
    pathSum(root, sum, cur, ret);
    return ret;
}

public void pathSum(TreeNode root, int sum, List<Integer>cur, List<List<Integer>>ret){
    if (root == null){
        return; 
    }
    cur.add(root.val);
    if (root.left == null && root.right == null && root.val == sum){
        ret.add(new ArrayList(cur));
    }else{
        pathSum(root.left, sum - root.val, cur, ret);
        pathSum(root.right, sum - root.val, cur, ret);
    }
    cur.remove(cur.size()-1);
}
```

114.Flatten Binary Tree to Linked List  
--
```java
  while(x>1)
	   {
		   System.out.println("fuck");
		   if(x>0)
		   {
			   x--;
			   continue;
		   }
		   System.out.println("a");
	   }
```
这段程序会输出两个fuck，也就是说遇到continue，会直接跳到while（x>1），后面的程序不会执行下去。
`递归的调用顺序`<br>
```java
public static void main(String[] args)
	{
		int x=1;
		up(x);
		
        }
	private static void up(int i)
	{
		System.out.println(i);
		if(i<4)
			up(i+1);
		System.out.println(i);
	}
```
这段程序会输出12344321.具体可以参见http://wenku.baidu.com/link?url=E-iZXdptlqUtUp0UOCFU7weyox1nCKl6d5NOlyKpGAJLAxKWfVVdVXRKn1Vv_Ma_pdGjsxNR-XK-ffPG1gkMt0UbNl5VsmGB-vEo8lBxX6K 

```java
private TreeNode prev = null;

public void flatten(TreeNode root) {
    if (root == null)
        return;
    flatten(root.right);
    flatten(root.left);
    root.right = prev;
    root.left = null;
    prev = root;
}
```
115.distinct subsequences
--
下面这个做法用的动态规划，真大神也！具体分析参见 https://discuss.leetcode.com/topic/9488/easy-to-understand-dp-in-java/2
```java
public int numDistinct(String S, String T) {
    // array creation
    int[][] mem = new int[T.length()+1][S.length()+1];

    // filling the first row: with 1s
    for(int j=0; j<=S.length(); j++) {
        mem[0][j] = 1;
    }
    
    // the first column is 0 by default in every other rows but the first, which we need.
    
    for(int i=0; i<T.length(); i++) {
        for(int j=0; j<S.length(); j++) {
            if(T.charAt(i) == S.charAt(j)) {
                mem[i+1][j+1] = mem[i][j] + mem[i+1][j];
            } else {
                mem[i+1][j+1] = mem[i+1][j];
            }
        }
    }
    
    return mem[T.length()][S.length()];
}
```
