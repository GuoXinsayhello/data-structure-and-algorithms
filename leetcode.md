
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
作者仅仅用了简单的中序遍历，简洁优雅
https://discuss.leetcode.com/topic/3988/no-fancy-algorithm-just-simple-and-powerful-in-order-traversal
高效。
