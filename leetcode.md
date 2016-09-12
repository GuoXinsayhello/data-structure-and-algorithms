
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
116. Populating Next Right Pointers in Each Node
--
```java
public void connect(TreeLinkNode root) {
   while (root != null) {
       TreeLinkNode p = root;
       while (p != null && p.left != null) {
           p.left.next = p.right;
           p.right.next = p.next == null ? null : p.next.left;
           p = p.next;
       }
       root = root.left;
   }
}
```
117.Populating Next Right Pointers in Each Node II
--
基本思想就是一层一层地迭代，然后每一次层从左到右。
```java
public class Solution {
    
    //based on level order traversal
    public void connect(TreeLinkNode root) {

        TreeLinkNode head = null; //head of the next level
        TreeLinkNode prev = null; //the leading node on the next level
        TreeLinkNode cur = root;  //current node of current level

        while (cur != null) {
            
            while (cur != null) { //iterate on the current level
                //left child
                if (cur.left != null) {
                    if (prev != null) {
                        prev.next = cur.left;
                    } else {
                        head = cur.left;
                    }
                    prev = cur.left;
                }
                //right child
                if (cur.right != null) {
                    if (prev != null) {
                        prev.next = cur.right;
                    } else {
                        head = cur.right;
                    }
                    prev = cur.right;
                }
                //move to next node
                cur = cur.next;
            }
            
            //move to next level
            cur = head;
            head = null;
            prev = null;
        }
        
    }
}
```

118.
--
```java
ls.add(1);
fils.add(ls);
ls.add(1);
fils.add(ls);
return fils;
```
这样写会出现<br>
1 1<br>
1 1<br>
的奇怪结果
下面这个答案挺好的
```java
public class Solution {
public List<List<Integer>> generate(int numRows)
{
	List<List<Integer>> allrows = new ArrayList<List<Integer>>();
	ArrayList<Integer> row = new ArrayList<Integer>();
	for(int i=0;i<numRows;i++)
	{
		row.add(0, 1);
		for(int j=1;j<row.size()-1;j++)
			row.set(j, row.get(j)+row.get(j+1));
		allrows.add(new ArrayList<Integer>(row));
	}
	return allrows;
	
}
}
```
基本思想就是在首先在第0位插入1，然后该位的值是原来该位的数加上后面一位的数。
120. Triangle
--
```java
public int minimumTotal(List<List<Integer>> triangle) {
    int[] A = new int[triangle.size()+1];
    for(int i=triangle.size()-1;i>=0;i--){
        for(int j=0;j<triangle.get(i).size();j++){
            A[j] = Math.min(A[j],A[j+1])+triangle.get(i).get(j);
        }
    }
    return A[0];
}
```
感觉这个想法很厉害啊，自己刚开始是想能否用动态规划去做，这个类似于倒序的动态规划。<br>
其实现在想想，正序的也可以，只不过写程序的时候每行开头和末尾填入的值没有选择性，写起来比较麻烦。
121.Best Time to Buy and Sell Stock I
--
https://discuss.leetcode.com/topic/19853/kadane-s-algorithm-since-no-one-has-mentioned-about-this-so-far-in-case-if-interviewer-twists-the-input
这个做法用的是Kadane's algorithm的方法，也可以用来解决最大子串问题。
```java
public int maxProfit(int[] prices) {
        int maxCur = 0, maxSoFar = 0;
        for(int i = 1; i < prices.length; i++) {
            maxCur = Math.max(0, maxCur += prices[i] - prices[i-1]);
            maxSoFar = Math.max(maxCur, maxSoFar);
        }
        return maxSoFar;
    }
```
这个做法的含义就是找到在每一天提交的最大收益，然后再比较一共prices.length个收益的最大值。
122.Best Time to Buy and Sell Stock II
--
这道题改变之后，解法比上一题简单了。
```java
public class Solution {
public int maxProfit(int[] prices) {
    int total = 0;
    for (int i=0; i< prices.length-1; i++) {
        if (prices[i+1]>prices[i]) total += prices[i+1]-prices[i];
    }
    
    return total;
}
```

124.Binary Tree Maximum Path Sum
--
这道题
```
public class Solution {
    int maxValue;
    
    public int maxPathSum(TreeNode root) {
        maxValue = Integer.MIN_VALUE;
        maxPathDown(root);
        return maxValue;
    }
    
    private int maxPathDown(TreeNode node) {
        if (node == null) return 0;
        int left = Math.max(0, maxPathDown(node.left));
        int right = Math.max(0, maxPathDown(node.right));
        maxValue = Math.max(maxValue, left + right + node.val);
        return Math.max(left, right) + node.val;
    }
}
```
下面的代码是自己写的，但是是`错误的`，请注意！！！！！！！！！
```java
public class Solution {
    public int maxPathSum(TreeNode root) { 
        return maxPathDown(root)+Math.min(maxPathDown(root.left), maxPathDown(root.right));
    }
    
    private int maxPathDown(TreeNode node) {
        if (node == null) return 0;
        int left = Math.max(0, maxPathDown(node.left));
        int right = Math.max(0, maxPathDown(node.right));
        return Math.max(left, right) + node.val;
    }
```
这个错误写法把maxValue去掉了，但是这种方法无法通过【2，-1】这个输入。就是说输入为负值的时候得到的是错误的结果。

125
--
有时候a=a+b不能等同于a+=b。比如:<br>
```java
char a='m';
a+=32;
```
这是可以的，但是<br>
```java
char a='m';
a=a+32;
```
这就不允许了。

126.
--
```java
public int ladderLength(String beginWord, String endWord, Set<String> wordDict) {
    Set<String> reached = new HashSet<String>();
    reached.add(beginWord);
    wordDict.add(endWord);
	int distance = 1;
	while(!reached.contains(endWord)) {
		Set<String> toAdd = new HashSet<String>();
		for(String each : reached) {
			for (int i = 0; i < each.length(); i++) {
                char[] chars = each.toCharArray();
                for (char ch = 'a'; ch <= 'z'; ch++) {
                	chars[i] = ch;
                    String word = new String(chars);
                    if(wordDict.contains(word)) {
                    	toAdd.add(word);
                    	wordDict.remove(word);
                    }
                }
			}
		}
		distance ++;
		if(toAdd.size() == 0) return 0;
		reached = toAdd;
	}
	return distance;
}
```
这是https://discuss.leetcode.com/topic/20965/java-solution-using-dijkstra-s-algorithm-with-explanation 的做法（附有解释）
这个做法的基本思想就是有一个reached的set，表示可以到达的单词，然后遍历set中的所有word，对于每个单词都改变每个字母，从a改变到z，然后看字典中是否有改变后的单词，如果有就把这个单词添加到reached的set。
个人觉得为什么要从a到z，有些尝试是根本不必要的，比如修改为一个字典中所有单词都没有的字母，这样的尝试没用，或者可以首先把字典中的所有单词的字母提取出来，自定义一个字母表，然后改变就改变为这些字母。
```java
Set<String> set=new HashSet<String>();
	   set.add("abc");
	   set.add("cde");
	   List<Character> pdict=new ArrayList<Character>();
	   for(String s:set){
		   for(char c:s.toCharArray()){
		   if(!pdict.contains(c)){
			   pdict.add(c);
		   }
		   }  
	   }
```
127
--
java中Arrays.sort的实现原理，对于基本的（primitive）类型采用的是快速排序，而对于Object类型，有两种，一种是归并排序，另外一种采用的是插入与归并结合的排序TimSort。http://www.jianshu.com/p/d083332c3c29 这个网址说的比较好。
128
--
下面这个做法太厉害了，具体参见https://discuss.leetcode.com/topic/6148/my-really-simple-java-o-n-solution-accepted
```java
public int longestConsecutive(int[] num) {
    int res = 0;
    HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    for (int n : num) {
        if (!map.containsKey(n)) {
            int left = (map.containsKey(n - 1)) ? map.get(n - 1) : 0;
            int right = (map.containsKey(n + 1)) ? map.get(n + 1) : 0;
            // sum: length of the sequence n is in
            int sum = left + right + 1;
            map.put(n, sum);
            
            // keep track of the max length 
            res = Math.max(res, sum);
            
            // extend the length to the boundary(s)
            // of the sequence
            // will do nothing if n has no neighbors
            map.put(n - left, sum);
            map.put(n + right, sum);
        }
        else {
            // duplicates
            continue;
        }
    }
    return res;
}
```
