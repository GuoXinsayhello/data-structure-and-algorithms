1.Two Sum
--
```java
 int[] nums={2,1,3};
int[] nums2=nums;
Arrays.sort(nums);
for(int i:nums2)
System.out.println(i);
```
发现一个问题，这时输出的是1,2,3，而不是2,1,3，这就是引用拷贝，如果要想深度拷贝的话，需要用clone()方法，如下：
```java
 int[] nums={2,1,3};
 int[] nums2=new int[3];
 nums2=nums.clone();
```

下面这种方法自己写的，击败了99.39%的用户
```java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
    	int[] nums2=new int[3];
    	nums2=nums.clone();
    	Arrays.sort(nums);
    	int le=0;
    	int ri=nums.length-1;
    	int n1=0,n2=0;
    	while(le<ri){
    		if(nums[le]+nums[ri]==target){
    			n1=nums[le];
    			n2=nums[ri];
    			
    			break;
    		}
    		else if(nums[le]+nums[ri]>target){
    			ri--;
    		}
    		else
    			le++;
    	}
    	int fi1=0;
    	int fi2=0;
    	for(int i=0;i<nums.length;i++){
    		if(nums2[i]==n1){
    			fi1=i;
    			break;
    		}	
    	}
    	for(int j=nums.length-1;j>=0;j--){
    		if(nums2[j]==n2){
    			fi2=j;
    			break;
    		}	
    	}
    	return new int[]{fi1,fi2};
	}
}
```
发现其实和服务器的性能有关，这个代码后来提交只能击败82%的用户。<br>
看到得分比较多的是用一个HashMap来做，感觉想法一般。
2.add two numbers
--
发现另外一个问题
```java
	ListNode l1=new ListNode(0);
		ListNode l2=l1.next;//1
		 l2=new ListNode(1);//2
			System.out.println(l1.next.val);
```
例如这段代码会出现空指针错误1处和2处的l2不是同一个。如果打印l2.val会输出1<br>
http://6924918.blog.51cto.com/6914918/1283761 这篇博客对于值传递和引用传递说的比较清楚，一般对于基本类型作为参数传递时，是传递值的拷贝，无论你怎么改变这个拷贝，原值是不会改变的，对象作为参数传递时，是把对象的引用传递过去，如果引用在方法内被改变了，那么原对象也跟着改变。<br>
又比如一个链表1-》2-》3-》4-》5-》6，开头是root,如果输入root2=root2.next；那么打印出root.next.valc会输出2<br>
```java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode c1 = l1;
        ListNode c2 = l2;
        ListNode sentinel = new ListNode(0);
        ListNode d = sentinel;
        int sum = 0;
        while (c1 != null || c2 != null) {
            sum /= 10;
            if (c1 != null) {
                sum += c1.val;
                c1 = c1.next;
            }
            if (c2 != null) {
                sum += c2.val;
                c2 = c2.next;
            }
            d.next = new ListNode(sum % 10);
            d = d.next;
        }
        if (sum / 10 == 1)
            d.next = new ListNode(1);
        return sentinel.next;
    }
}
```
这个方法还是比较简洁明了。
。
3.Longest Substring Without Repeating Characters
--
List有一个subList(int fromindex，int endindex)方法,可以截取一个list。<br>
map的put方法既可以作为insert来用，也可以当做update来用，比如：
```java
Map<Integer,String> map=new HashMap<Integer,String>();
   map.put(1, "a");
   map.put(1, "b");
   System.out.println(map.get(1));
```
此时不会出现编译错误。
```java
public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max=0;
        for (int i=0, j=0; i<s.length(); ++i){
            if (map.containsKey(s.charAt(i))){
                j = Math.max(j,map.get(s.charAt(i))+1);
            }
            map.put(s.charAt(i),i);
            max = Math.max(max,i-j+1);
        }
        return max;
    }
```
这是一个投票较高的做法，下面是自己的做法
```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
    	char[] arr=s.toCharArray();
    	List<Character> set=new ArrayList<Character>();
    	int l=0;
    	for(int i=0;i<s.length();i++){
    		if(!set.contains(arr[i]))
    			set.add(arr[i]);
    		else{
    			if(set.size()>l)
    				l=set.size();
    			set=set.subList(set.indexOf(arr[i])+1,set.size());
    			set.add(arr[i]);
    		}
    			
    	}
    	
    	return Math.max(l, set.size());
    }
    }
```
自己的做法感觉想法差不多，不过用的数据结构不好，另外每次都要用sublist重新截取list，效率比较低。

5.plin
--
```java
public class Solution {
private int lo, maxLen;

public String longestPalindrome(String s) {
	int len = s.length();
	if (len < 2)
		return s;
	
    for (int i = 0; i < len-1; i++) {
     	extendPalindrome(s, i, i);  //assume odd length, try to extend Palindrome as possible
     	extendPalindrome(s, i, i+1); //assume even length.
    }
    return s.substring(lo, lo + maxLen);
}

private void extendPalindrome(String s, int j, int k) {
	while (j >= 0 && k < s.length() && s.charAt(j) == s.charAt(k)) {
		j--;
		k++;
	}
	if (maxLen < k - j - 1) {
		lo = j + 1;
		maxLen = k - j - 1;
	}
}}
```
这个方法的思想就是从开始不断向周围扩展，直到不是回文字符，然后求出最大值。
6.Zig Zag Conversion
--
```java
public String convert(String s, int nRows) {
    char[] c = s.toCharArray();
    int len = c.length;
    StringBuffer[] sb = new StringBuffer[nRows];
    for (int i = 0; i < sb.length; i++) sb[i] = new StringBuffer();
    
    int i = 0;
    while (i < len) {
        for (int idx = 0; idx < nRows && i < len; idx++) // vertically down
            sb[idx].append(c[i++]);
        for (int idx = nRows-2; idx >= 1 && i < len; idx--) // obliquely up
            sb[idx].append(c[i++]);
    }
    for (int idx = 1; idx < sb.length; idx++)
        sb[0].append(sb[idx]);
    return sb[0].toString();
}
```
这个想法就是每一行都建立一个StringBuffer，然后按照ZIGZAG顺序依次写入每个StringBuffer。
7.reverse Integer
--
下面这个方法是自己写的，用的是try catch去捕捉overflow。
```java
public class Solution {
    public int reverse(int x) {
    	String str=String .valueOf(x);
    	StringBuffer sb=new StringBuffer();
    	char[] arr=str.toCharArray();
    	int res=0;
    	if(x>=0){
    		for(int i=arr.length-1;i>=0;i--){
    			sb.append(arr[i]);
    		}
    		try{
    			 res=Integer.parseInt(sb.toString());
    		}
    		catch(NumberFormatException e){
    			return 0;
    		}
    		return res;
    	}
    	else{
    		   sb.append('-');
        		for(int j=arr.length-1;j>0;j--){
        			sb.append(arr[j]);
        		}
        		try{
        			 res=Integer.parseInt(sb.toString());
        		}
        		catch(NumberFormatException e){
        			return 0;
        		}
        		return res;
        	
    	}
    }
    }
```
下面这个方法投票比较高，当出现overflow时，重新计算时结果与之前的不一样，此时抛出0.
```java
public int reverse(int x)
{
    int result = 0;

    while (x != 0)
    {
        int tail = x % 10;
        int newResult = result * 10 + tail;
        if ((newResult - tail) / 10 != result)
        { return 0; }
        result = newResult;
        x = x / 10;
    }

    return result;
}
```
这个方法就是再算一遍，如果overflow，那么结果y和之前的就会不同。
8.
--
```java
public static int myAtoi(String str) {
    if (str.isEmpty()) return 0;
    int sign = 1, base = 0, i = 0;
    while (str.charAt(i) == ' ')
        i++;
    if (str.charAt(i) == '-' || str.charAt(i) == '+')
        sign = str.charAt(i++) == '-' ? -1 : 1;
    while (i < str.length() && str.charAt(i) >= '0' && str.charAt(i) <= '9') {
        if (base > Integer.MAX_VALUE / 10 || (base == Integer.MAX_VALUE / 10 && str.charAt(i) - '0' > 7)) {
            return (sign == 1) ? Integer.MAX_VALUE : Integer.MIN_VALUE;
        }
        base = 10 * base + (str.charAt(i++) - '0');
    }
    return base * sign;
}
```
下面这个做法还是挺靠谱的。
9. Palindrome Number
--
```java
public boolean isPalindrome(int x) {
    if (x<0 || (x!=0 && x%10==0)) return false;
    int rev = 0;
    while (x>rev){
    	rev = rev*10 + x%10;
    	x = x/10;
    }
    return (x==rev || x==rev/10);
}
```
这个做法还是挺好的， 思想就是把整数反过来，不过只反过来一半，这样就不会出现overflow的问题。

10.Regular Expression Matching
--
用的是DP算法，详细解释见https://discuss.leetcode.com/topic/40371/easy-dp-java-solution-with-detailed-explanation
```java
public class Solution {
    public boolean isMatch(String s, String p) {
        if(s == null || p == null) {
            return false;
        }
        boolean[][] state = new boolean[s.length() + 1][p.length() + 1];
        state[0][0] = true;
        // no need to initialize state[i][0] as false
        // initialize state[0][j]
        for (int j = 1; j < state[0].length; j++) {
            if (p.charAt(j - 1) == '*') {
                if (state[0][j - 1] || (j > 1 && state[0][j - 2])) {
                    state[0][j] = true;
                }
            } 
        }
        for (int i = 1; i < state.length; i++) {
            for (int j = 1; j < state[0].length; j++) {
                if (s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '.') {
                    state[i][j] = state[i - 1][j - 1];
                }
                if (p.charAt(j - 1) == '*') {
                    if (s.charAt(i - 1) != p.charAt(j - 2) && p.charAt(j - 2) != '.') {
                        state[i][j] = state[i][j - 2];
                    } else {
                        state[i][j] = state[i - 1][j] || state[i][j - 1] || state[i][j - 2];
                    }
                }
            }
        }
        return state[s.length()][p.length()];
    }
}
```
下面这个方法用的是递归来做的
```java
public boolean isMatch(String s, String p) {
    if (p.isEmpty()) {
        return s.isEmpty();
    }

    if (p.length() == 1 || p.charAt(1) != '*') {
        if (s.isEmpty() || (p.charAt(0) != '.' && p.charAt(0) != s.charAt(0))) {
            return false;
        } else {
            return isMatch(s.substring(1), p.substring(1));
        }
    }
    
    //P.length() >=2
    while (!s.isEmpty() && (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.')) {  
        if (isMatch(s, p.substring(2))) { 
            return true;                     
        }                                    
        s = s.substring(1);
    }

    return isMatch(s, p.substring(2));
}
```

11. Container With Most Water
这道题第一遍的时候没有写出来，第二遍的时候自己写的用的是投票最高的方法，s提交之后居然超时。下面是自己写的方法。
--
```java

    public int maxArea(int[] h) {
        int l=0,r=h.length-1;
        int max=Math.min(h[r], h[l])*(r-l);
        while(l<r){
        	if(h[l]<=h[r]){
        		max=Math.max(max, h[l]*(r-l));
        		l++;
        	}
        	else{
        		max=Math.max(max, h[r]*(r-l));
        		r--;
        	}
        }
        return max;
    }
```
12. Integer to Roman
--
```java
public class Solution {
public String intToRoman(int num) {

    int[] values = {1000,900,500,400,100,90,50,40,10,9,5,4,1};
    String[] strs = {"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
    
    StringBuilder sb = new StringBuilder();
    
    for(int i=0;i<values.length;i++) {
        while(num >= values[i]) {
            num -= values[i];
            sb.append(strs[i]);
        }
    }
    return sb.toString();
}
```
这个做法就是每次都要遍历一遍values。

15. 3Sum
--
```java
public List<List<Integer>> threeSum(int[] num) {
    Arrays.sort(num);
    List<List<Integer>> res = new LinkedList<>(); 
    for (int i = 0; i < num.length-2; i++) {
        if (i == 0 || (i > 0 && num[i] != num[i-1])) {
            int lo = i+1, hi = num.length-1, sum = 0 - num[i];
            while (lo < hi) {
                if (num[lo] + num[hi] == sum) {
                    res.add(Arrays.asList(num[i], num[lo], num[hi]));
                    while (lo < hi && num[lo] == num[lo+1]) lo++;
                    while (lo < hi && num[hi] == num[hi-1]) hi--;
                    lo++; hi--;
                } else if (num[lo] + num[hi] < sum) lo++;
                else hi--;
           }
        }
    }
    return res;
}
```
这个做法就是target从0到length-3,把0-target作为求和目标sum，然后low为该目标的o右边一位，hi为最右，不断验证lo+hi是否等于sum，等于的话就x记录下来，然后lo++，hi--，看是否还有其他的组合，如果大于sum就hi--，否则lo++。
```java
List<List<Integer>> lls=new LinkedList<List<Integer>>();
	    lls.add(Arrays.asList(1,2,3));
```
这样就不用new一个新的List\<Integer\>
16 3sum closest
--
自己写的
```java
public class Solution {
    public int threeSumClosest(int[] nums, int target) {
    	int lo=0;
    	int hi=nums.length-1;
    	int gap=Integer.MAX_VALUE;
    	int flag=0;
    	Arrays.sort(nums);
    	outer:for(int m=0;m<nums.length-2;m++){
    		lo=m+1;
    		hi=nums.length-1;
    		
    		while(lo<hi){
    			if(nums[lo]+nums[hi]+nums[m]==target){
    				flag=0;
    				break outer;
    			}
    			else if(nums[lo]+nums[hi]+nums[m]-target<0){
    				if(Math.abs(nums[lo]+nums[hi]+nums[m]-target)<gap){
    					gap=Math.abs(nums[lo]+nums[hi]+nums[m]-target);
    					flag=-1;
    				}
    				while(lo<hi && nums[lo+1]==nums[lo])lo++;
    				lo++;
    			}
    			else{
    				if(Math.abs(nums[lo]+nums[hi]+nums[m]-target)<gap){
    					gap=Math.abs(nums[lo]+nums[hi]+nums[m]-target);
    					flag=1;
    				}
    					
    				while(lo<hi && nums[hi-1]==nums[hi])hi--;
    				hi--;
    			}
    				
    		}
    	}
    	if(flag==0)
    		return target;
    	else if(flag==-1)
    		return target-gap;
    	else
    		return target+gap;
	}
```
17
--
下面这个做法真是服！
```java
public List<String> letterCombinations(String digits) {
    LinkedList<String> ans = new LinkedList<String>();
    String[] mapping = new String[] {"0", "1", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    ans.add("");
    for(int i =0; i<digits.length();i++){
        int x = Character.getNumericValue(digits.charAt(i));
        while(ans.peek().length()==i){
            String t = ans.remove();
            for(char s : mapping[x].toCharArray())
                ans.add(t+s);
        }
    }
    return ans;
}
```
下面是自己的做法
```java
public class Solution {
    public List<String> letterCombinations(String digits) {
    	Map<Character,String> map=new HashMap<Character,String>();
    	map.put('2', "abc");
    	map.put('3', "def");
    	map.put('4', "ghi");
    	map.put('5', "jkl");
    	map.put('6', "mno");
    	map.put('7', "pqrs");
    	map.put('8', "tuv");
    	map.put('9', "wxyz");
    	List<String> res=new ArrayList<String>();
    	if(digits.length()==0)
    		return res;
    	res.add("");
    	for(int i=0;i<digits.length();i++){
    		res=ge(res,digits.charAt(i),map);
    	}
    	return res;
    }
    public List<String> ge(List<String> ls,char ch,Map<Character,String> map){
    	List<String> res_ls=new ArrayList<String>();
    	for(String str:ls){
    		int l=map.get(ch).length();
    		for(int j=0;j<l;j++){
    			res_ls.add(str+map.get(ch).charAt(j));
    		}
    	}
    	return res_ls;
    }
}
```
18 4 sum
--
```java
public class Solution {
    public List<List<Integer>> fourSum(int[] num, int target) {
        ArrayList<List<Integer>> ans = new ArrayList<>();
        if(num.length<4)return ans;
        Arrays.sort(num);
        for(int i=0; i<num.length-3; i++){
            if(i>0&&num[i]==num[i-1])continue;
            for(int j=i+1; j<num.length-2; j++){
                if(j>i+1&&num[j]==num[j-1])continue;
                int low=j+1, high=num.length-1;
                while(low<high){
                    int sum=num[i]+num[j]+num[low]+num[high];
                    if(sum==target){
                        ans.add(Arrays.asList(num[i], num[j], num[low], num[high]));
                        while(low<high&&num[low]==num[low+1])low++;
                        while(low<high&&num[high]==num[high-1])high--;
                        low++;
                        high--;
                    }
                    else if(sum<target)low++;
                    else high--;
                }
            }
        }
        return ans;
    }
}
```
这个是基于3 sum的做法，挺靠谱。

19.
下面这个做法我服，我发现linkedlist套路就是搞速度不一样的指针，或者是出发时间不一样的指针。
```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    
    ListNode start = new ListNode(0);
    ListNode slow = start, fast = start;
    slow.next = head;
    
    //Move fast in front so that the gap between slow and fast becomes n
    for(int i=1; i<=n+1; i++)   {
        fast = fast.next;
    }
    //Move fast to the end, maintaining the gap
    while(fast != null) {
        slow = slow.next;
        fast = fast.next;
    }
    //Skip the desired node
    slow.next = slow.next.next;
    return start.next;
}
```
22. Generate Parentheses
--
这个用的是回溯算法，比较不错。但是回溯算法不是很理解。
```java
public List<String> generateParenthesis(int n) {
        List<String> list = new ArrayList<String>();
        backtrack(list, "", 0, 0, n);
        return list;
    }
    public void backtrack(List<String> list, String str, int open, int close, int max){
        if(str.length() == max*2){
            list.add(str);
            return;
        }
        if(open < max)
            backtrack(list, str+"(", open+1, close, max);
        if(close < open)
            backtrack(list, str+")", open, close+1, max);
    }
```

23.Merge k sorted list
--
```java
public class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        if (lists==null||lists.length==0) return null;
        
        PriorityQueue<ListNode> queue= new PriorityQueue<ListNode>(lists.length,new Comparator<ListNode>(){
            @Override
            public int compare(ListNode o1,ListNode o2){
                if (o1.val<o2.val)
                    return -1;
                else if (o1.val==o2.val)
                    return 0;
                else 
                    return 1;
            }
        });
        
        ListNode dummy = new ListNode(0);
        ListNode tail=dummy;
        
        for (ListNode node:lists)
            if (node!=null)
                queue.add(node);
            
        while (!queue.isEmpty()){
            tail.next=queue.poll();
            tail=tail.next;
            
            if (tail.next!=null)
                queue.add(tail.next);
        }
        return dummy.next;
    }
}
```
下面这个用的是递归的方法
```java
public static ListNode mergeKLists(ListNode[] lists){
    return partion(lists,0,lists.length-1);
}

public static ListNode partion(ListNode[] lists,int s,int e){
    if(s==e)  return lists[s];
    if(s<e){
        int q=(s+e)/2;
        ListNode l1=partion(lists,s,q);
        ListNode l2=partion(lists,q+1,e);
        return merge(l1,l2);
    }else
        return null;
}

//This function is from Merge Two Sorted Lists.
public static ListNode merge(ListNode l1,ListNode l2){
    if(l1==null) return l2;
    if(l2==null) return l1;
    if(l1.val<l2.val){
        l1.next=merge(l1.next,l2);
        return l1;
    }else{
        l2.next=merge(l1,l2.next);
        return l2;
    }
}
```
这个方法用的是优先队列的方法，我按照两两合并的方法总是超时。
24.swap nodes
--
这个用的是recursion的方法，
```java
public class Solution {
    public ListNode swapPairs(ListNode head) {
        if ((head == null)||(head.next == null))
            return head;
        ListNode n = head.next;
        head.next = swapPairs(head.next.next);
        n.next = head;
        return n;
    }
}
```
25
--
下面这个方法用的是递归，挺不错的，其实这个方法也揭示了如何把一个链翻转过来。基本思想就是构造一个temp保存head的next，然后构造一个cur，让head指向cur，然后cur，head，temp不断右移。
```java
public ListNode reverseKGroup(ListNode head, int k) {
    ListNode curr = head;
    int count = 0;
    while (curr != null && count != k) { // find the k+1 node
        curr = curr.next;
        count++;
    }
    if (count == k) { // if k+1 node is found
        curr = reverseKGroup(curr, k); // reverse list with k+1 node as head
        // head - head-pointer to direct part, 
        // curr - head-pointer to reversed part;
        while (count-- > 0) { // reverse current k-group: 
            ListNode tmp = head.next; // tmp - next head in direct part
            head.next = curr; // preappending "direct" head to the reversed list 
            curr = head; // move head of reversed part to a new node
            head = tmp; // move "direct" head to the next node in direct part
        }
        head = curr;
    }
    return head;
}
```

26.Remove Duplicates from Sorted Array
--
```java
class Solution {
    public:
    int removeDuplicates(int A[], int n) {
        if(n < 2) return n;
        int id = 1;
        for(int i = 1; i < n; ++i) 
            if(A[i] != A[i-1]) A[id++] = A[i];
        return id;
    }
};
```
相当不错的方法。

30. Substring with Concatenation of All Words  
--
下面这个做法的基本思想就是用一个hashmap保存L中的关键词，记录它们出现的次数，然后遍历给的String，不断如果有关键词出现，就从map中删除，如果最后mapw为空，说明删除完了，就是包含，就添加到结果当中。
```java
public static List<Integer> findSubstring(String S, String[] L) {
    List<Integer> res = new ArrayList<Integer>();
    if (S == null || L == null || L.length == 0) return res;
    int len = L[0].length(); // length of each word
    
    Map<String, Integer> map = new HashMap<String, Integer>(); // map for L
    for (String w : L) map.put(w, map.containsKey(w) ? map.get(w) + 1 : 1);
    
    for (int i = 0; i <= S.length() - len * L.length; i++) {
        Map<String, Integer> copy = new HashMap<String, Integer>(map);
        for (int j = 0; j < L.length; j++) { // checkc if match
            String str = S.substring(i + j*len, i + j*len + len); // next word
            if (copy.containsKey(str)) { // is in remaining words
                int count = copy.get(str);
                if (count == 1) copy.remove(str);
                else copy.put(str, count - 1);
                if (copy.isEmpty()) { // matches
                    res.add(i);
                    break;
                }
            } else break; // not in L
        }
    }
    return res;
}
```
31. Next Permutation
--
函数调用这个swap函数不能交换两个int的值，因为基本类型按照值传递，不会改变原有的数值。
```java
private void swap(int i,int j){
    	int temp=0;
    	temp=i;
    	i=j;
    	j=temp;
    }
```
但是下面这个函数swap2就可以：
```java
private void swap2(int[] nums,int i,int j){
	    	int temp=0;
	    	temp=nums[i];
	    	nums[i]=nums[j];
	    	nums[j]=temp;
	    }
```
32. Longest Valid Parentheses
--
下面这个方法比较巧妙
```java
public class Solution {
    public int longestValidParentheses(String s) {
        LinkedList<Integer> stack = new LinkedList<>();//1处
        int result = 0;
        stack.push(-1);
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == ')' && stack.size() > 1 && s.charAt(stack.peek()) == '(') {
                stack.pop();
                result = Math.max(result, i - stack.peek());
            } else {
                stack.push(i);
            }
        }
        return result;
    }
}
```
注意1处newh之后的LinkedList居然不用写类型。
33. Search in Rotated Sorted Array
--
```java
class Solution {
public:
    int search(int A[], int n, int target) {
        int lo=0,hi=n-1;
        // find the index of the smallest value using binary search.
        // Loop will terminate since mid < hi, and lo or hi will shrink by at least 1.
        // Proof by contradiction that mid < hi: if mid==hi, then lo==hi and loop would have been terminated.
        while(lo<hi){
            int mid=(lo+hi)/2;
            if(A[mid]>A[hi]) lo=mid+1;
            else hi=mid;
        }
        // lo==hi is the index of the smallest value and also the number of places rotated.
        int rot=lo;
        lo=0;hi=n-1;
        // The usual binary search and accounting for rotation.
        while(lo<=hi){
            int mid=(lo+hi)/2;
            int realmid=(mid+rot)%n;
            if(A[realmid]==target)return realmid;
            if(A[realmid]<target)lo=mid+1;
            else hi=mid-1;
        }
        return -1;
    }
};
```
注意这个题里面二分搜索与搜索pivot同时出现了，注意两者的区别，二分搜索while的条件是l<=r,而且都是mid+1或者mid-1.而搜索pivot的话while条件是lo\<hi,hi=mid而不是mid-1.。而且注意这种搜索pivot的方法不能处理重复的数据，比如数据如果是1,1,1,0,1这样的例子就不行了。
34. Search for a Range
--
具体解释参见https://discuss.leetcode.com/topic/5891/clean-iterative-solution-with-two-binary-searches-with-explanation
```java
vector<int> searchRange(int A[], int n, int target) {
    int i = 0, j = n - 1;
    vector<int> ret(2, -1);
    // Search for the left one
    while (i < j)
    {
        int mid = (i + j) /2;
        if (A[mid] < target) i = mid + 1;
        else j = mid;
    }
    if (A[i]!=target) return ret;
    else ret[0] = i;
    
    // Search for the right one
    j = n-1;  // We don't have to set i to 0 the second time.
    while (i < j)
    {
        int mid = (i + j) /2 + 1;	// Make mid biased to the right
        if (A[mid] > target) j = mid - 1;  
        else i = mid;				// So that this won't make the search range stuck.
    }
    ret[1] = j;
    return ret; 
}
```
36.Valid Sudoku
--
```java
public boolean isValidSudoku(char[][] board) {
    for(int i = 0; i<9; i++){
        HashSet<Character> rows = new HashSet<Character>();
        HashSet<Character> columns = new HashSet<Character>();
        HashSet<Character> cube = new HashSet<Character>();
        for (int j = 0; j < 9;j++){
            if(board[i][j]!='.' && !rows.add(board[i][j]))
                return false;
            if(board[j][i]!='.' && !columns.add(board[j][i]))
                return false;
            int RowIndex = 3*(i/3);
            int ColIndex = 3*(i%3);
            if(board[RowIndex + j/3][ColIndex + j%3]!='.' && !cube.add(board[RowIndex + j/3][ColIndex + j%3]))
                return false;
        }
    }
    return true;
}
```
37
--
这道题目用的是backtracking算法，其实感觉就是暴力破解一种方法不行就倒回去再试另一种方法。
```java
public class Solution {
    public void solveSudoku(char[][] board) {
        if(board == null || board.length == 0)
            return;
        solve(board);
    }
    public boolean solve(char[][] board){
        for(int i = 0; i < board.length; i++){
            for(int j = 0; j < board[0].length; j++){
                if(board[i][j] == '.'){
                    for(char c = '1'; c <= '9'; c++){//trial. Try 1 through 9
                        if(isValid(board, i, j, c)){
                            board[i][j] = c; //Put c for this cell
                            
                            if(solve(board))
                                return true; //If it's the solution return true
                            else
                                board[i][j] = '.'; //Otherwise go back
                        }
                    }
                    
                    return false;
                }
            }
        }
        return true;
    }
    private boolean isValid(char[][] board, int row, int col, char c){
        for(int i = 0; i < 9; i++) {
            if(board[i][col] != '.' && board[i][col] == c) return false; //check row
            if(board[row][i] != '.' && board[row][i] == c) return false; //check column
            if(board[3 * (row / 3) + i / 3][ 3 * (col / 3) + i % 3] != '.' && 
board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == c) return false; //check 3*3 block
        }
        return true;
    }
}
```
38.
--
下面这种方法用的是recursive算法
```java
public class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
    	Arrays.sort(candidates);
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        getResult(result, new ArrayList<Integer>(), candidates, target, 0);
        
        return result;
    }
    
    private void getResult(List<List<Integer>> result, List<Integer> cur, int candidates[], int target, int start){
    	if(target > 0){
    		for(int i = start; i < candidates.length && target >= candidates[i]; i++){
    			cur.add(candidates[i]);
    			getResult(result, cur, candidates, target - candidates[i], i);
    			cur.remove(cur.size() - 1);
    		}//for
    	}//if
    	else if(target == 0 ){
    		result.add(new ArrayList<Integer>(cur));
    	}//else if
    }
}
```
41
--
下面这个做法还是挺好的，就是把正确的数放在正确的位置。
```java
public class Solution {
    public int firstMissingPositive(int[] A) {
        int i = 0;
        while(i < A.length){
            if(A[i] == i+1 || A[i] <= 0 || A[i] > A.length) i++;
            else if(A[A[i]-1] != A[i]) swap(A, i, A[i]-1);
            else i++;
        }
        i = 0;
        while(i < A.length && A[i] == i+1) i++;
        return i+1;
    }
    
    private void swap(int[] A, int i, int j){
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
}
```
43.Multiply Strings 
--
```java
public String multiply(String num1, String num2) {
    int m = num1.length(), n = num2.length();
    int[] pos = new int[m + n];
   
    for(int i = m - 1; i >= 0; i--) {
        for(int j = n - 1; j >= 0; j--) {
            int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0'); 
            int p1 = i + j, p2 = i + j + 1;
            int sum = mul + pos[p2];

            pos[p1] += sum / 10;
            pos[p2] = (sum) % 10;
        }
    }  
    
    StringBuilder sb = new StringBuilder();
    for(int p : pos) if(!(sb.length() == 0 && p == 0)) sb.append(p);
    return sb.length() == 0 ? "0" : sb.toString();
}
```
44.
--
比较清晰易懂。
```java
boolean comparison(String str, String pattern) {
        int s = 0, p = 0, match = 0, starIdx = -1;            
        while (s < str.length()){
            // advancing both pointers
            if (p < pattern.length()  && (pattern.charAt(p) == '?' || str.charAt(s) == pattern.charAt(p))){
                s++;
                p++;
            }
            // * found, only advancing pattern pointer
            else if (p < pattern.length() && pattern.charAt(p) == '*'){
                starIdx = p;
                match = s;
                p++;
            }
           // last pattern pointer was *, advancing string pointer
            else if (starIdx != -1){
                p = starIdx + 1;
                match++;
                s = match;
            }
           //current pattern pointer is not star, last patter pointer was not *
          //characters do not match
            else return false;
        }
        
        //check for remaining characters in pattern
        while (p < pattern.length() && pattern.charAt(p) == '*')
            p++;
        
        return p == pattern.length();
}
```

45
--
```java
public int jump(int[] A) {
int step_count = 0;
int last_jump_max = 0;
int current_jump_max = 0;
for(int i=0; i<A.length-1; i++) {
    current_jump_max = Math.max(current_jump_max, i+A[i]);
    if( i == last_jump_max ) {
        step_count++;
        last_jump_max = current_jump_max;
    } 
}
return step_count;
}
```
这个做法的想法就是看1步，2步，3步。。。最多能走多远。
下面这个做法是BFS
```java
 int jump(int A[], int n) {
	 if(n<2)return 0;
	 int level=0,currentMax=0,i=0,nextMax=0;

	 while(currentMax-i+1>0){		//nodes count of current level>0
		 level++;
		 for(;i<=currentMax;i++){	//traverse current level , and update the max reach of next level
			nextMax=max(nextMax,A[i]+i);
			if(nextMax>=n-1)return level;   // if last element is in level+1,  then the min jump=level 
		 }
		 currentMax=nextMax;
	 }
	 return 0;
 }
 ```
  也就是把具有相同步数能够达到的node看做同一个level。
 47
 --
 这个方法效率应该比较高，但是理解不了
 ```java
 public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(nums==null || nums.length==0) return res;
        boolean[] used = new boolean[nums.length];
        List<Integer> list = new ArrayList<Integer>();
        Arrays.sort(nums);
        dfs(nums, used, list, res);
        return res;
    }

    public void dfs(int[] nums, boolean[] used, List<Integer> list, List<List<Integer>> res){
        if(list.size()==nums.length){
            res.add(new ArrayList<Integer>(list));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(used[i]) continue;
            if(i>0 &&nums[i-1]==nums[i] && !used[i-1]) continue;
            used[i]=true;
            list.add(nums[i]);
            dfs(nums,used,list,res);
            used[i]=false;
            list.remove(list.size()-1);
        }
    }
}
```

48
--
https://discuss.leetcode.com/topic/6796/a-common-method-to-rotate-the-image 这个方法讲的是对于旋转的一个一般的办法，感觉和rotate string的思想非常像，比如1234567——》3217654——》4567123
```c++
/*
 * clockwise rotate
 * first reverse up to down, then swap the symmetry 
 * 1 2 3     7 8 9     7 4 1
 * 4 5 6  => 4 5 6  => 8 5 2
 * 7 8 9     1 2 3     9 6 3
*/
void rotate(vector<vector<int> > &matrix) {
    reverse(matrix.begin(), matrix.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}

/*
 * anticlockwise rotate
 * first reverse left to right, then swap the symmetry
 * 1 2 3     3 2 1     3 6 9
 * 4 5 6  => 6 5 4  => 2 5 8
 * 7 8 9     9 8 7     1 4 7
*/
void anti_rotate(vector<vector<int> > &matrix) {
    for (auto vi : matrix) reverse(vi.begin(), vi.end());
    for (int i = 0; i < matrix.size(); ++i) {
        for (int j = i + 1; j < matrix[i].size(); ++j)
            swap(matrix[i][j], matrix[j][i]);
    }
}
```
49. Group Anagrams
--
 ```
 public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs == null || strs.length == 0) return new ArrayList<List<String>>();
        Map<String, List<String>> map = new HashMap<String, List<String>>();
        Arrays.sort(strs);
        for (String s : strs) {
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String keyStr = String.valueOf(ca);
            if (!map.containsKey(keyStr)) map.put(keyStr, new ArrayList<String>());
            map.get(keyStr).add(s);
        }
        return new ArrayList<List<String>>(map.values());
    }
}
```

50.Pow(x,n)
https://discuss.leetcode.com/topic/21837/5-different-choices-when-talk-with-interviewers 这个网址集结了多种实现的方法，现列出其中一种
```java
double myPow(double x, int n) {
    if(n<0) return 1/x * myPow(1/x, -(n+1));
    if(n==0) return 1;
    if(n==2) return x*x;
    if(n%2==0) return myPow( myPow(x, n/2), 2);
    else return x*myPow( myPow(x, n/2), 2);
}
```
主要用的是递归的想法

51.N-queens
--
这个方法用的是DFS以及 backtracking算法，比较厉害！ 这道题的借鉴意义就是首先判断过某个点的两条对角线上是否有其他的Q，只需要 i+j==x+y || i+y==j+x即可。此外，backtracking算法在写的时候要在末尾把之前的操作去掉，比如此题就体现在下面这句。
```java
 board[i][colIndex] = 'Q';
 dfs(board, colIndex + 1, res);
  board[i][colIndex] = '.';//这句
```

```java
public class Solution {
    public List<List<String>> solveNQueens(int n) {
        char[][] board = new char[n][n];
        for(int i = 0; i < n; i++)
            for(int j = 0; j < n; j++)
                board[i][j] = '.';
        List<List<String>> res = new ArrayList<List<String>>();
        dfs(board, 0, res);
        return res;
    }
    
    private void dfs(char[][] board, int colIndex, List<List<String>> res) {
        if(colIndex == board.length) {
            res.add(construct(board));
            return;
        }
        
        for(int i = 0; i < board.length; i++) {
            if(validate(board, i, colIndex)) {
                board[i][colIndex] = 'Q';
                dfs(board, colIndex + 1, res);
                board[i][colIndex] = '.';
            }
        }
    }
    
    private boolean validate(char[][] board, int x, int y) {
        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < y; j++) {
                if(board[i][j] == 'Q' && (x + j == y + i || x + y == i + j || x == i))
                    return false;
            }
        }
        
        return true;
    }
    
    private List<String> construct(char[][] board) {
        List<String> res = new LinkedList<String>();
        for(int i = 0; i < board.length; i++) {
            String s = new String(board[i]);
            res.add(s);
        }
        return res;
    }
}
```
52.N-queens II
--
这个解法用的仍然是DFS，不过并没有真正放置queen，而是用了三个set共同判断放置的queen是否有效。
```java
private final Set<Integer> occupiedCols = new HashSet<Integer>();
private final Set<Integer> occupiedDiag1s = new HashSet<Integer>();
private final Set<Integer> occupiedDiag2s = new HashSet<Integer>();
public int totalNQueens(int n) {
    return totalNQueensHelper(0, 0, n);
}

private int totalNQueensHelper(int row, int count, int n) {
    for (int col = 0; col < n; col++) {
        if (occupiedCols.contains(col))
            continue;
        int diag1 = row - col;
        if (occupiedDiag1s.contains(diag1))
            continue;
        int diag2 = row + col;
        if (occupiedDiag2s.contains(diag2))
            continue;
        // we can now place a queen here
        if (row == n-1)
            count++;
        else {
            occupiedCols.add(col);
            occupiedDiag1s.add(diag1);
            occupiedDiag2s.add(diag2);
            count = totalNQueensHelper(row+1, count, n);
            // recover
            occupiedCols.remove(col);
            occupiedDiag1s.remove(diag1);
            occupiedDiag2s.remove(diag2);
        }
    }
    
    return count;
}
```
54. Spiral Matrix
--
```java
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> res = new ArrayList<Integer>();
        if (matrix.length == 0) {
            return res;
        }
        int rowBegin = 0;
        int rowEnd = matrix.length-1;
        int colBegin = 0;
        int colEnd = matrix[0].length - 1;
        while (rowBegin <= rowEnd && colBegin <= colEnd) {
            // Traverse Right
            for (int j = colBegin; j <= colEnd; j ++) {
                res.add(matrix[rowBegin][j]);
            }
            rowBegin++;
            // Traverse Down
            for (int j = rowBegin; j <= rowEnd; j ++) {
                res.add(matrix[j][colEnd]);
            }
            colEnd--;
            if (rowBegin <= rowEnd) {
                // Traverse Left
                for (int j = colEnd; j >= colBegin; j --) {
                    res.add(matrix[rowEnd][j]);
                }
            }
            rowEnd--;
            if (colBegin <= colEnd) {
                // Traver Up
                for (int j = rowEnd; j >= rowBegin; j --) {
                    res.add(matrix[j][colBegin]);
                }
            }
            colBegin ++;
        }
        return res;
    }
}
```
55. Jump Game
--
下面这个方法是自己写的，自己用了几个例子测试还可以，用的可以说是DFS算法吧，然而stackoverflow了。
```java
public class Solution {
    public boolean canJump(int[] nums) {
    	int l=nums.length;
    	List<Integer> trace =new LinkedList<Integer>();
    	if(l<=1)
    		return true;
    	ge(nums,l-2,l-1,trace);
    	
    	if(trace.size()==0 || trace.get(trace.size()-1)!=0)
    		return false;
    	else
    		return true;
    }
    private void ge(int[] a ,int start,int end ,List<Integer> trace){
    	if(start<0)
    		return ;
    	if(start+a[start]>=end){
    		trace.add(start);
    		end=start;
    		if(start==0)
    			return ;
    		ge(a,start-1,end,trace);
    		
    	}
    	else{
    		ge(a,start-1,end,trace);
    	}	
    }
}
```
下面这个做法简洁优美真的很不错，大致意思就是如果某一点是可达的，那么该点之前的所有点都是可达的
```java
public boolean canJump(int[] nums) {
    int reachable = 0;
    for (int i=0; i<nums.length; ++i) {
        if (i > reachable) return false;
        reachable = Math.max(reachable, i + nums[i]);
    }
    return true;
}
```
60
--
https://discuss.leetcode.com/topic/17348/explain-like-i-m-five-java-solution-in-o-n 这个做法挺不错，就是要分析挺多。
```java
public class Solution {
public String getPermutation(int n, int k) {
    int pos = 0;
    List<Integer> numbers = new ArrayList<>();
    int[] factorial = new int[n+1];
    StringBuilder sb = new StringBuilder();
    
    // create an array of factorial lookup
    int sum = 1;
    factorial[0] = 1;
    for(int i=1; i<=n; i++){
        sum *= i;
        factorial[i] = sum;
    }
    // factorial[] = {1, 1, 2, 6, 24, ... n!}
    
    // create a list of numbers to get indices
    for(int i=1; i<=n; i++){
        numbers.add(i);
    }
    // numbers = {1, 2, 3, 4}
    
    k--;
    
    for(int i = 1; i <= n; i++){
        int index = k/factorial[n-i];
        sb.append(String.valueOf(numbers.get(index)));
        numbers.remove(index);
        k-=index*factorial[n-i];
    }
    
    return String.valueOf(sb);
}
}
```
下面这个做法也挺好的
61. Rotate List
--
下面这个做法是自己写的，击败了80%+的人 ，不过仍然 需要计算链表的长度。
```java
public class Solution {
    public ListNode rotateRight(ListNode head, int k) {
    	ListNode pivot=head;
    	ListNode dummy=head;
    	ListNode oldpre=new ListNode(0);
    	ListNode pre=new ListNode(0);
    	oldpre.next=head;
    	pre.next=head;
    	int count=0,num=0;
    	if(head==null || head.next==null || k==0)
    		return head;
    	while(head!=null){
    		count++;
    		head=head.next;
    		oldpre=oldpre.next;
    	}
    	k=k%count;
    	if(k==0)
    		return dummy;
    	while(num<count-k){
    		pivot=pivot.next;
    		pre=pre.next;
    		num++;
    	}
    	oldpre.next=dummy;
    	pre.next=null;
    	return pivot;
        
    }  
}
```
下面这个 做法应该比较好，不用计算链表的长度，只是用了两个指针，一个先出发，一个后出发，这个想法还是挺不错的
具体可以看一下http://www.tuicool.com/articles/BVnqQn 这个网站

66.Plus One
--
```java
public int[] plusOne(int[] digits) {
        
    int n = digits.length;
    for(int i=n-1; i>=0; i--) {
        if(digits[i] < 9) {
            digits[i]++;
            return digits;
        }
        
        digits[i] = 0;
    }
    
    int[] newNumber = new int [n+1];
    newNumber[0] = 1;
    
    return newNumber;
}
```
67
--
注意一点，字符0对应的数字是48，一个字符和一个数字相加得到的是其对应的数，比如a='0',则a+1会等于49

69.Sqrt（x）
--下面这种方法用的是二分搜索
```java
public int sqrt(int x) {
    if (x == 0)
        return 0;
    int left = 1, right = Integer.MAX_VALUE;
    while (true) {
        int mid = left + (right - left)/2;
        if (mid > x/mid) {
            right = mid - 1;
        } else {
            if (mid + 1 > x/(mid + 1))
                return mid;
            left = mid + 1;
        }
    }
}
```
下面这种方法使用牛顿法做的
```java
    long r = x;
    while (r*r > x)
        r = (r + x/r) / 2;
    return (int) r;
```

71,simple path
--
感觉自己第一次做的时候没有搞清楚题目的意思，/..表示回到上一级目录，/.表示当前目录，
```java
public String simplifyPath(String path) {
    Deque<String> stack = new LinkedList<>();
    Set<String> skip = new HashSet<>(Arrays.asList("..",".",""));
    for (String dir : path.split("/")) {
        if (dir.equals("..") && !stack.isEmpty()) stack.pop();
        else if (!skip.contains(dir)) stack.push(dir);
    }
    String res = "";
    for (String dir : stack) res = "/" + dir + res;
    return res.isEmpty() ? "/" : res;
}
```

72.Edit Distance
--
Levenshtein 距离，又称编辑距离，指的是两个字符串之间，由一个转换成另一个所需的最少编辑操作次数。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。http://www.stanford.edu/class/cs124/lec/med.pdf 这个PDF讲的非常详细，解决这种问题一般采用动态规划算法，但是这个pdf里面有点问题，当x[i]!=y[j]的时候，为啥d(i,j)=d(i-1,j-1)+2？网友评论说是认为替换的cost更大一点，2是cost，而不是步数，如果要求步数的话要加1。下面是动态规划算法的表达。
```java
public class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        
        int[][] cost = new int[m + 1][n + 1];
        for(int i = 0; i <= m; i++)
            cost[i][0] = i;
        for(int i = 1; i <= n; i++)
            cost[0][i] = i;
        
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(word1.charAt(i) == word2.charAt(j))
                    cost[i + 1][j + 1] = cost[i][j];
                else {
                    int a = cost[i][j];
                    int b = cost[i][j + 1];
                    int c = cost[i + 1][j];
                    cost[i + 1][j + 1] = a < b ? (a < c ? a : c) : (b < c ? b : c);
                    cost[i + 1][j + 1]++;
                }
            }
        }
        return cost[m][n];
    }
}
```

73.
--
下面这种想法的思想就是每当遇到0的就把0置在这个0位置的行开头和列开头，然后第二次遍历的时候凡是遇到0开头的就把该行或者该列置为0.
```java
void setZeroes(vector<vector<int> > &matrix) {
    int col0 = 1, rows = matrix.size(), cols = matrix[0].size();

    for (int i = 0; i < rows; i++) {
        if (matrix[i][0] == 0) col0 = 0;
        for (int j = 1; j < cols; j++)
            if (matrix[i][j] == 0)
                matrix[i][0] = matrix[0][j] = 0;
    }

    for (int i = rows - 1; i >= 0; i--) {
        for (int j = cols - 1; j >= 1; j--)
            if (matrix[i][0] == 0 || matrix[0][j] == 0)
                matrix[i][j] = 0;
        if (col0 == 0) matrix[i][0] = 0;
    }
}
```

74
---
下面这个做法是把一个矩阵当做一个向量来对待。
```java
class Solution {
public:
    bool searchMatrix(vector<vector<int> > &matrix, int target) {
        int n = matrix.size();
        int m = matrix[0].size();
        int l = 0, r = m * n - 1;
        while (l != r){
            int mid = (l + r - 1) >> 1;
            if (matrix[mid / m][mid % m] < target)
                l = mid + 1;
            else 
                r = mid;
        }
        return matrix[r / m][r % m] == target;
    }
};
```
下面这个做法是自己写的，就是用了两次的二分法
```java
public class Solution {
    public boolean searchMatrix(int[][] ma, int ta) {
    	int lx=0,rx=ma.length-1,index1=0,index2=0,lx2=0,rx2=ma[0].length-1;
    	boolean flag=false;
    	while(lx<=rx)
    	{
    		 index1=(rx+lx)/2;
    			if(ta>ma[index1][0])
    			{
    				lx=index1+1;
    			}
    			else if(ta<ma[index1][0])
    			{
    				rx=index1-1;
    			}
    			else
    			{
    				flag=true;
    				break;
    			}
    		
    	}
    	index1=(lx+rx)/2;
    	if(flag==false)
    	{
    		while(lx2<=rx2)
    		{
    			index2=(lx2+rx2)/2;
    				if(ta>ma[index1][index2])
    					lx2=index2+1;
    				else if(ta<ma[index1][index2])
    					rx2=index2-1;
    				else
    				{
    					flag=true;
    					break;
    				}
    		}
    	}
    	return flag; 
    }
}
```

75.sort colors
--
这个做法就是把0交换到左边，把2交换到右边
```java
    class Solution {
    public:
        void sortColors(int A[], int n) {
            int second=n-1, zero=0;
            for (int i=0; i<=second; i++) {
                while (A[i]==2 && i<second) swap(A[i], A[second--]);
                while (A[i]==0 && i>zero) swap(A[i], A[zero++]);
            }
        }
    };
```

76
--
这道题目的想法就是用start，end两个指针来决定一个窗口，首先移动end使该窗口满足能够覆盖所有的字符串，然后移动start使窗口最小。https://discuss.leetcode.com/topic/30941/here-is-a-10-line-template-that-can-solve-most-substring-problems 这个网址总结了这种题目的一般做法。
```java
public String minWindow(String s, String t) {
    HashMap<Character,Integer> map = new HashMap();
    for(char c : s.toCharArray())
        map.put(c,0);
    for(char c : t.toCharArray())
    {
        if(map.containsKey(c))
            map.put(c,map.get(c)+1);
        else
            return "";
    }
    
    int start =0, end=0, minStart=0,minLen = Integer.MAX_VALUE, counter = t.length();
    while(end < s.length())
    {
        char c1 = s.charAt(end);
        if(map.get(c1) > 0)
            counter--;
        map.put(c1,map.get(c1)-1);
        
        end++;
        
        while(counter == 0)
        {
            if(minLen > end-start)
            {
                minLen = end-start;
                minStart = start;
            }
            
            char c2 = s.charAt(start);
            map.put(c2, map.get(c2)+1);
            
            if(map.get(c2) > 0)
                counter++;
            
            start++;
        }
    }
    return minLen == Integer.MAX_VALUE ? "" : s.substring(minStart,minStart+minLen);
}
```

77
--
主要用到C(n,k)=C(n-1,k)+C(n-1,k-1);
```java
public class Solution {
    public List<List<Integer>> combine(int n, int k) {
        if (k == n || k == 0) {
            List<Integer> row = new LinkedList<>();
            for (int i = 1; i <= k; ++i) {
                row.add(i);
            }
            return new LinkedList<>(Arrays.asList(row));
        }
        List<List<Integer>> result = this.combine(n - 1, k - 1);
        result.forEach(e -> e.add(n));
        result.addAll(this.combine(n - 1, k));
        return result;
    }
}
```
下面这个是backtracking算法，

```java
  public static List<List<Integer>> combine(int n, int k) {
		List<List<Integer>> combs = new ArrayList<List<Integer>>();
		combine(combs, new ArrayList<Integer>(), 1, n, k);
		return combs;
	}
	public static void combine(List<List<Integer>> combs, List<Integer> comb, int start, int n, int k) {
		if(k==0) {
			combs.add(new ArrayList<Integer>(comb));
			return;
		}
		for(int i=start;i<=n;i++) {
			comb.add(i);
			combine(combs, comb, i+1, n, k-1);
			comb.remove(comb.size()-1);
		}
	}
```

79 word search
--
下面这个做法就是DFS，感觉很经典。首先是与256进行异或，对访问过的位置进行标记，然后就是用了 三个||运算符。
```java
public boolean exist(char[][] board, String word) {
    char[] w = word.toCharArray();
    for (int y=0; y<board.length; y++) {
    	for (int x=0; x<board[y].length; x++) {
    		if (exist(board, y, x, w, 0)) return true;
    	}
    }
    return false;
}

private boolean exist(char[][] board, int y, int x, char[] word, int i) {
	if (i == word.length) return true;
	if (y<0 || x<0 || y == board.length || x == board[y].length) return false;
	if (board[y][x] != word[i]) return false;
	board[y][x] ^= 256;
	boolean exist = exist(board, y, x+1, word, i+1)
		|| exist(board, y, x-1, word, i+1)
		|| exist(board, y+1, x, word, i+1)
		|| exist(board, y-1, x, word, i+1);
	board[y][x] ^= 256;
	return exist;
}
```

80
--
```java
public class Solution {
    public int removeDuplicates(int[] nums) {
    	int index=2;
    	int n=nums.length;
    	if(n<=2)
    		return n;
    	for(int i=2;i<n;i++){
    		if((nums[i]>nums[index-2])){  //1处
    			nums[index++]=nums[i];
    		}
    	}
    	return index;
        
    }
}
```
注意1处的index不能写成i，因为下面的一句把nums改变了，所以写成i比较的是变化后的nums，会出现nums[i]==nums[i-2]的结果
上面这种方法是根据下面这种别人的方法写出来的
```java
public int removeDuplicates(int[] nums) {
    int i = 0;
    for (int n : nums)
        if (i < 2 || n > nums[i-2])
            nums[i++] = n;
    return i;
}
```

81.
--
```c++
class Solution {
public:
  bool search(int A[], int n, int target) {
    int lo =0, hi = n-1;
    int mid = 0;
    while(lo<hi){
          mid=(lo+hi)/2;
          if(A[mid]==target) return true;
          if(A[mid]>A[hi]){
              if(A[mid]>target && A[lo] <= target) hi = mid;
              else lo = mid + 1;
          }else if(A[mid] < A[hi]){
              if(A[mid]<target && A[hi] >= target) lo = mid + 1;
              else hi = mid;
          }else{
              hi--;
          }
          
    }
    return A[lo] == target ? true : false;
  }
};
```
因为数组当中存在着重复的元素，所以可能出现A[mid]==A[hi]的情况

82.Remove Duplicates from Sorted List II
--
```java
public ListNode deleteDuplicates(ListNode head) {
        if(head==null) return null;
        ListNode FakeHead=new ListNode(0);
        FakeHead.next=head;
        ListNode pre=FakeHead;
        ListNode cur=head;
        while(cur!=null){
            while(cur.next!=null&&cur.val==cur.next.val){
                cur=cur.next;
            }
            if(pre.next==cur){
                pre=pre.next;
            }
            else{
                pre.next=cur.next;
            }
            cur=cur.next;
        }
        return FakeHead.next;
    }
```

84
--
下面这个算法来自http://www.geeksforgeeks.org/largest-rectangle-under-histogram/ 基本想法就是对于每个bar，向左延伸找到第一个比它小的，向右延伸找到第一个比它小的，然后计算长方形大小。用了栈来存储信息，感觉比较高效。
```java
public class Solution {
    public int largestRectangleArea(int[] height) {
        int len = height.length;
        Stack<Integer> s = new Stack<Integer>();
        int maxArea = 0;
        for(int i = 0; i <= len; i++){
            int h = (i == len ? 0 : height[i]);
            if(s.isEmpty() || h >= height[s.peek()]){
                s.push(i);
            }else{
                int tp = s.pop();
                maxArea = Math.max(maxArea, height[tp] * (s.isEmpty() ? i : i - 1 - s.peek()));
                i--;
            }
        }
        return maxArea;
    }
}
```

85.Maximal Rectangle
--
下面这个做法用的是DP算法，就是定出左边界和右边界，以及连续的高度
```java
class Solution {public:
int maximalRectangle(vector<vector<char> > &matrix) {
    if(matrix.empty()) return 0;
    const int m = matrix.size();
    const int n = matrix[0].size();
    int left[n], right[n], height[n];
    fill_n(left,n,0); fill_n(right,n,n); fill_n(height,n,0);
    int maxA = 0;
    for(int i=0; i<m; i++) {
        int cur_left=0, cur_right=n; 
        for(int j=0; j<n; j++) { // compute height (can do this from either side)
            if(matrix[i][j]=='1') height[j]++; 
            else height[j]=0;
        }
        for(int j=0; j<n; j++) { // compute left (from left to right)
            if(matrix[i][j]=='1') left[j]=max(left[j],cur_left);
            else {left[j]=0; cur_left=j+1;}
        }
        // compute right (from right to left)
        for(int j=n-1; j>=0; j--) {
            if(matrix[i][j]=='1') right[j]=min(right[j],cur_right);
            else {right[j]=n; cur_right=j;}    
        }
        // compute the area of rectangle (can do this from either side)
        for(int j=0; j<n; j++)
            maxA = max(maxA,(right[j]-left[j])*height[j]);
    }
    return maxA;
}
```
87.scramble String
--
下面的做法真是简洁优美，而且1处的代码也给出了如何判断两个单词是由相同的字母构成的
```java
public class Solution {
    public boolean isScramble(String s1, String s2) {
        if (s1.equals(s2)) return true; 
        
        int[] letters = new int[26];
        for (int i=0; i<s1.length(); i++) {   //1
            letters[s1.charAt(i)-'a']++;
            letters[s2.charAt(i)-'a']--;
        }
        for (int i=0; i<26; i++) if (letters[i]!=0) return false;
    
        for (int i=1; i<s1.length(); i++) {
            if (isScramble(s1.substring(0,i), s2.substring(0,i)) 
             && isScramble(s1.substring(i), s2.substring(i))) return true;
            if (isScramble(s1.substring(0,i), s2.substring(s2.length()-i)) 
             && isScramble(s1.substring(i), s2.substring(0,s2.length()-i))) return true;
        }
        return false;
    }
}
```

88
--
下面这个做法还是挺好的
```c++
class Solution {
public:
    void merge(int A[], int m, int B[], int n) {
        int i=m-1;
		int j=n-1;
		int k = m+n-1;
		while(i >=0 && j>=0)
		{
			if(A[i] > B[j])
				A[k--] = A[i--];
			else
				A[k--] = B[j--];
		}
		while(j>=0)
			A[k--] = B[j--];
    }
};
```

89.Gray Code
--
下面这种做法是自己写的，用的就是递归的策略，求n的graycode，就在n-1的格雷码前加0或者1，不过这个做法超时了（TLE）！
```java
public class Solution {
    public List<Integer> grayCode(int n) {
    	List<Integer> res=new LinkedList<>();
    	if(n==0){
    	    res.add(0);
    	    return res;
    	}
    		
    	List<String> strres=ge(n);
    	for(String str:strres)
    		res.add(Integer.parseInt(str,2));
    	return res;
    }
    private List<String> ge(int n){
    	List<String> temp=new LinkedList<>();
    	if(n==1){
    		temp.add("0");
    		temp.add("1");
    		return temp;
    	}
    	else{
    		for(String str:ge(n-1)){
    			StringBuffer sb0=new StringBuffer(str);
    			sb0.insert(0, "0");
    			temp.add(sb0.toString());
    		}
    		int l=ge(n-1).size();
    		for(int i=l-1;i>=0;i--){
    			StringBuffer sb1=new StringBuffer(ge(n-1).get(i));
    			sb1.insert(0, "1");
    			temp.add(sb1.toString());
    		}
    		return temp;
    	}
    }
}
```
下面这个做法是别人写的，太简洁了
```java
public List<Integer> grayCode(int n) {
    List<Integer> result = new LinkedList<>();
    for (int i = 0; i < 1<<n; i++) result.add(i ^ i>>1);
    return result;
}
```
The idea is simple. G(i) = i^ (i/2).



91.Decode ways
--
下面的做法是自己写的，居然超时了，感觉逻辑并没有错误，可能效率太低，用的是递归。
```java
public class Solution {
    public int numDecodings(String s) {
    	if(s.length()==0)
    		return 0;
    	else if(s.startsWith("0"))
    		return 0;
    	else if(s.length()==1){
    		if(!s.equals("0"))
    			return 1;
    		else
    			return 0;
    	}
    		
        else if(s.length()==2){
    		int temps=Integer.parseInt(s);
    		if(temps<=26 && temps!=10 && temps!=20)
    			return 2;
    		else if(temps%10==0 && temps!=10 && temps!=20)
    			return 0;
    		else
    			return 1;
    			 
    	}
    	else{
    		int temp=Integer.parseInt(s.substring(0,2));
    		if(temp<=26 && temp!=20 && temp !=10)
    			return numDecodings(s.substring(2))+numDecodings(s.substring(1));
    		else if(temp==20 || temp==10)
    			return numDecodings(s.substring(2));
    		else
    			return numDecodings(s.substring(1));
    			
    	}
        
    }
}
```
下面这个做法是别人的，用的DP算法，基本思想就是从字符串的末尾开始向前

```java
public class Solution {
    public int numDecodings(String s) {
        int n = s.length();
        if (n == 0) return 0;
        
        int[] memo = new int[n+1];
        memo[n]  = 1;
        memo[n-1] = s.charAt(n-1) != '0' ? 1 : 0;
        
        for (int i = n - 2; i >= 0; i--)
            if (s.charAt(i) == '0') continue;
            else memo[i] = (Integer.parseInt(s.substring(i,i+2))<=26) ? memo[i+1]+memo[i+2] : memo[i+1];
        
        return memo[0];
    }
}
```
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

其实上面的做法麻烦了，下面这个非常简单，而且能够实现预期的效果。
```java
public class Append 
{
	List<TreeNode> tree=new LinkedList<>();
	public List ge(TreeNode root){
		if(root==null)
			return tree;
		ge(root.left);
		    tree.add(root);
		ge(root.right);
		return tree;	
 }
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
这个写法的错误在与会对字符串重复使用，也就是用过的字符会再用一次。<br>
下面的做法是自己在12月20日又写的，自己测试一下都通过了，但是超时了
```java
public class Solution {
    public boolean isInterleave(String s1, String s2, String s3) {
    	int l1=s1.length();
    	int l2=s2.length();
    	int l3=s3.length();
    	if(l1+l2!=l3)
    		return false;
    	if(l3==0)
    		return true;
    	boolean valid=(   (l1>0 && s1.substring(0,1).equals(  s3.substring(0,1)  ) && 
    			  isInterleave(s1.substring(1),s2,s3.substring(1) )    ) || 
    			(l2>0 && s2.substring(0,1).equals(  s3.substring(0,1)  ) && 
    	    		isInterleave(s1,s2.substring(1),s3.substring(1))  ) );
    	return valid;
        
    }
```
下面的做法是别人的DFS算法
```java
public boolean isInterleave(String s1, String s2, String s3) {
    char[] c1 = s1.toCharArray(), c2 = s2.toCharArray(), c3 = s3.toCharArray();
	int m = s1.length(), n = s2.length();
	if(m + n != s3.length()) return false;
	return dfs(c1, c2, c3, 0, 0, 0, new boolean[m + 1][n + 1]);
}

public boolean dfs(char[] c1, char[] c2, char[] c3, int i, int j, int k, boolean[][] invalid) {
	if(invalid[i][j]) return false;
	if(k == c3.length) return true;
	boolean valid = 
	    i < c1.length && c1[i] == c3[k] && dfs(c1, c2, c3, i + 1, j, k + 1, invalid) || 
        j < c2.length && c2[j] == c3[k] && dfs(c1, c2, c3, i, j + 1, k + 1, invalid);
	if(!valid) invalid[i][j] = true;
    return valid;
}
```
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
102.Binary Tree Level Order Traversal
下面这个方法用了一个队列
```java
public class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        List<List<Integer>> wrapList = new LinkedList<List<Integer>>();
        
        if(root == null) return wrapList;
        
        queue.offer(root);
        while(!queue.isEmpty()){
            int levelNum = queue.size();
            List<Integer> subList = new LinkedList<Integer>();
            for(int i=0; i<levelNum; i++) {
                if(queue.peek().left != null) queue.offer(queue.peek().left);
                if(queue.peek().right != null) queue.offer(queue.peek().right);
                subList.add(queue.poll().val);
            }
            wrapList.add(subList);
        }
        return wrapList;
    }
}
```
103.Binary Tree Zigzag Level Order Traversal
```java
public class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) 
    {
        List<List<Integer>> sol = new ArrayList<>();
        travel(root, sol, 0);
        return sol;
    }
    
    private void travel(TreeNode curr, List<List<Integer>> sol, int level)
    {
        if(curr == null) return;
        
        if(sol.size() <= level)
        {
            List<Integer> newLevel = new LinkedList<>();
            sol.add(newLevel);
        }
        
        List<Integer> collection  = sol.get(level);
        if(level % 2 == 0) collection.add(curr.val);
        else collection.add(0, curr.val);
        
        travel(curr.left, sol, level + 1);
        travel(curr.right, sol, level + 1);
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
129.Sum Root to Leaf Numbers
--
下面是递归的方法做的
```java
public int sumNumbers(TreeNode root) {
	return sum(root, 0);
}

public int sum(TreeNode n, int s){
	if (n == null) return 0;
	if (n.right == null && n.left == null) return s*10 + n.val;
	return sum(n.left, s*10 + n.val) + sum(n.right, s*10 + n.val);
}
```
130.
--
https://discuss.leetcode.com/topic/17224/a-really-simple-and-readable-c-solution-only-cost-12ms
这个是用C++写的一个DFS算法。
下面这个做法是用DFS写的一个深度优先搜索算法，
```java
public void solve(char[][] board) {
	if (board.length == 0 || board[0].length == 0)
		return;
	if (board.length < 2 || board[0].length < 2)
		return;
	int m = board.length, n = board[0].length;
	//Any 'O' connected to a boundary can't be turned to 'X', so ...
	//Start from first and last column, turn 'O' to '*'.
	for (int i = 0; i < m; i++) {
		if (board[i][0] == 'O')
			boundaryDFS(board, i, 0);
		if (board[i][n-1] == 'O')
			boundaryDFS(board, i, n-1);	
	}
	//Start from first and last row, turn '0' to '*'
	for (int j = 0; j < n; j++) {
		if (board[0][j] == 'O')
			boundaryDFS(board, 0, j);
		if (board[m-1][j] == 'O')
			boundaryDFS(board, m-1, j);	
	}
	//post-prcessing, turn 'O' to 'X', '*' back to 'O', keep 'X' intact.
	for (int i = 0; i < m; i++) {
		for (int j = 0; j < n; j++) {
			if (board[i][j] == 'O')
				board[i][j] = 'X';
			else if (board[i][j] == '*')
				board[i][j] = 'O';
		}
	}
}
//Use DFS algo to turn internal however boundary-connected 'O' to '*';
private void boundaryDFS(char[][] board, int i, int j) {
	if (i < 0 || i > board.length - 1 || j <0 || j > board[0].length - 1)
		return;
	if (board[i][j] == 'O')
		board[i][j] = '*';
	if (i > 1 && board[i-1][j] == 'O')
		boundaryDFS(board, i-1, j);
	if (i < board.length - 2 && board[i+1][j] == 'O')
		boundaryDFS(board, i+1, j);
	if (j > 1 && board[i][j-1] == 'O')
		boundaryDFS(board, i, j-1);
	if (j < board[i].length - 2 && board[i][j+1] == 'O' )
		boundaryDFS(board, i, j+1);
}
```
这道题目也有用union find方法，具体参见https://discuss.leetcode.com/topic/1944/solve-it-using-union-find

这道题也可以用BFS算法，如下：
```java
public class Solution {
    public void solve(char[][] board) {
        if (board.length == 0) return;
        
        int rowN = board.length;
        int colN = board[0].length;
        Queue<Point> queue = new LinkedList<Point>();
       
       //get all 'O's on the edge first
        for (int r = 0; r< rowN; r++){
        	if (board[r][0] == 'O') {
        		board[r][0] = '+';
                queue.add(new Point(r, 0));
        	}
        	if (board[r][colN-1] == 'O') {
        		board[r][colN-1] = '+';
                queue.add(new Point(r, colN-1));
        	}
        	}
        
        for (int c = 0; c< colN; c++){
        	if (board[0][c] == 'O') {
        		board[0][c] = '+';
                queue.add(new Point(0, c));
        	}
        	if (board[rowN-1][c] == 'O') {
        		board[rowN-1][c] = '+';
                queue.add(new Point(rowN-1, c));
        	}
        	}
        

        //BFS for the 'O's, and mark it as '+'
        while (!queue.isEmpty()){
        	Point p = queue.poll();
        	int row = p.x;
        	int col = p.y;
        	if (row - 1 >= 0 && board[row-1][col] == 'O') {board[row-1][col] = '+'; queue.add(new Point(row-1, col));}
        	if (row + 1 < rowN && board[row+1][col] == 'O') {board[row+1][col] = '+'; queue.add(new Point(row+1, col));}
        	if (col - 1 >= 0 && board[row][col - 1] == 'O') {board[row][col-1] = '+'; queue.add(new Point(row, col-1));}
            if (col + 1 < colN && board[row][col + 1] == 'O') {board[row][col+1] = '+'; queue.add(new Point(row, col+1));}        	      
        }
        

        //turn all '+' to 'O', and 'O' to 'X'
        for (int i = 0; i<rowN; i++){
        	for (int j=0; j<colN; j++){
        		if (board[i][j] == 'O') board[i][j] = 'X';
        		if (board[i][j] == '+') board[i][j] = 'O';
        	}
        }
       
        
    }
}
```
BFS算法需要一个queue，也就是队列，需要入队和出队。而DFS需要递归。

131. Palindrome Partitioning
--
```java
public class Solution {
    public List<List<String>> partition(String s) {
        List<List<String>> res=new ArrayList<List<String>>();
        if(s.length()==0)return res;
        recur(res,new ArrayList<String>(),s);
        return res;
    }
    
    public void recur(List<List<String>> res,List<String> temp, String s){
        if(s.length()==0){
            res.add(new ArrayList<String>(temp));
            return;
        }
        for(int i=0;i<s.length();i++){
            if(isPalin(s.substring(0,i+1))){
                temp.add(s.substring(0,i+1));
                recur(res,temp,s.substring(i+1));
                temp.remove(temp.size()-1); //1处
            }
        }
    }
    
    public boolean isPalin(String s){
        for(int i=0;i<s.length()/2;i++){
            if(s.charAt(i)!=s.charAt(s.length()-1-i))return false;
        }
        return true;
    }
}
```
就是很迷惑1处的代码是什么意思。为什么要去掉。
132
--
```java
public int minCut(String s) {
    char[] c = s.toCharArray();
    int n = c.length;
    int[] cut = new int[n];
    boolean[][] pal = new boolean[n][n];
    
    for(int i = 0; i < n; i++) {
        int min = i;
        for(int j = 0; j <= i; j++) {
            if(c[j] == c[i] && (j + 1 > i - 1 || pal[j + 1][i - 1])) {
                pal[j][i] = true;  
                min = j == 0 ? 0 : Math.min(min, cut[j - 1] + 1);
            }
        }
        cut[i] = min;
    }
    return cut[n - 1];
}
```
这道题用的动态规划，挺好的，可以看看
133.clone graph
--
```java
public class Solution {
    private HashMap<Integer, UndirectedGraphNode> map = new HashMap<>();
    public UndirectedGraphNode cloneGraph(UndirectedGraphNode node) {
        return clone(node);
    }

    private UndirectedGraphNode clone(UndirectedGraphNode node) {
        if (node == null) return null;
        
        if (map.containsKey(node.label)) {
            return map.get(node.label);
        }
        UndirectedGraphNode clone = new UndirectedGraphNode(node.label);
        map.put(clone.label, clone);
        for (UndirectedGraphNode neighbor : node.neighbors) {
            clone.neighbors.add(clone(neighbor));
        }
        return clone;
    }
}
```
134.gas station
--
```java
class Solution {
public:
    int canCompleteCircuit(vector<int> &gas, vector<int> &cost) {

       int start = gas.size()-1;
       int end = 0;
       int sum = gas[start] - cost[start];
       while (start > end) {
          if (sum >= 0) {
             sum += gas[end] - cost[end];
             ++end;
          }
          else {
             --start;
             sum += gas[start] - cost[start];
          }
       }
       return sum >= 0 ? start : -1;
    }
};
```
135.candy
--
```c++
int candy(vector<int> &ratings)
 {
	 int size=ratings.size();
	 if(size<=1)
		 return size;
	 vector<int> num(size,1);
	 for (int i = 1; i < size; i++)
	 {
		 if(ratings[i]>ratings[i-1])
			 num[i]=num[i-1]+1;
	 }
	 for (int i= size-1; i>0 ; i--)
	 {
		 if(ratings[i-1]>ratings[i])
			 num[i-1]=max(num[i]+1,num[i-1]);
	 }
	 int result=0;
	 for (int i = 0; i < size; i++)
	 {
		 result+=num[i];
		// cout<<num[i]<<" ";
	 }
	 return result;
 }
```
139.word break
--
用的是动态规划的算法
```java
public class Solution {
    public boolean wordBreak(String s, Set<String> dict) {
        
        boolean[] f = new boolean[s.length() + 1];
        f[0] = true;
        //Second DP
        for(int i=1; i <= s.length(); i++){
            for(int j=0; j < i; j++){
                if(f[j] && dict.contains(s.substring(j, i))){
                    f[i] = true;
                    break;
                }
            }
        }
        
        return f[s.length()];
    }
}
```

140.word breakII
--
这道题用的是DFS的方法
```java
public List<String> wordBreak(String s, Set<String> wordDict) {
    return DFS(s, wordDict, new HashMap<String, LinkedList<String>>());
}       

// DFS function returns an array including all substrings derived from s.
List<String> DFS(String s, Set<String> wordDict, HashMap<String, LinkedList<String>>map) {
    if (map.containsKey(s)) 
        return map.get(s);
        
    LinkedList<String>res = new LinkedList<String>();     
    if (s.length() == 0) {
        res.add("");
        return res;
    }               
    for (String word : wordDict) {
        if (s.startsWith(word)) {
            List<String>sublist = DFS(s.substring(word.length()), wordDict, map);
            for (String sub : sublist) 
                res.add(word + (sub.isEmpty() ? "" : " ") + sub);               
        }
    }       
    map.put(s, res);
    return res;
}
```
142.Linked List Cycle II
--
```java
public class Solution {
            public ListNode detectCycle(ListNode head) {
                ListNode slow = head;
                ListNode fast = head;
                while (fast!=null && fast.next!=null){
                    fast = fast.next.next;
                    slow = slow.next;
                    if (fast == slow){
                        ListNode slow2 = head; 
                        while (slow2 != slow){
                            slow = slow.next;
                            slow2 = slow2.next;
                        }
                        return slow;
                    }
                }
                return null;
            }
        }
```
佩服地五体投地！！详细解释见https://discuss.leetcode.com/topic/19367/java-o-1-space-solution-with-detailed-explanation
143.reorder list
--
```java
public void reorderList(ListNode head) {
            if(head==null||head.next==null) return;
            
            //Find the middle of the list
            ListNode p1=head;
            ListNode p2=head;
            while(p2.next!=null&&p2.next.next!=null){ 
                p1=p1.next;
                p2=p2.next.next;
            }
            
            //Reverse the half after middle  1->2->3->4->5->6 to 1->2->3->6->5->4
            ListNode preMiddle=p1;
            ListNode preCurrent=p1.next;
            while(preCurrent.next!=null){
                ListNode current=preCurrent.next;
                preCurrent.next=current.next;
                current.next=preMiddle.next;
                preMiddle.next=current;
            }
            
            //Start reorder one by one  1->2->3->6->5->4 to 1->6->2->5->3->4
            p1=head;
            p2=preMiddle.next;
            while(p1!=preMiddle){
                preMiddle.next=p2.next;
                p2.next=p1.next;
                p1.next=p2;
                p1=p2.next;
                p2=preMiddle.next;
            }
        }
```
145.Binary Tree Postorder Traversal
--
`PRE ORDER TRAVERSE`
```java
public List<Integer> preorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode p = root;
    while(!stack.isEmpty() || p != null) {
        if(p != null) {
            stack.push(p);
            result.add(p.val);  // Add before going to children
            p = p.left;
        } else {
            TreeNode node = stack.pop();
            p = node.right;   
        }
    }
    return result;
}
```
`IN ORDER TRAVERSE`
```java
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> result = new ArrayList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode p = root;
    while(!stack.isEmpty() || p != null) {
        if(p != null) {
            stack.push(p);
            p = p.left;
        } else {
            TreeNode node = stack.pop();
            result.add(node.val);  // Add after all left children
            p = node.right;   
        }
    }
    return result;
}
```
`POST ORDER TRAVERSE`
```java
public List<Integer> postorderTraversal(TreeNode root) {
    LinkedList<Integer> result = new LinkedList<>();
    Deque<TreeNode> stack = new ArrayDeque<>();
    TreeNode p = root;
    while(!stack.isEmpty() || p != null) {
        if(p != null) {
            stack.push(p);
            result.addFirst(p.val);  // Reverse the process of preorder
            p = p.right;             // Reverse the process of preorder
        } else {
            TreeNode node = stack.pop();
            p = node.left;           // Reverse the process of preorder
        }
    }
    return result;
}
```
https://discuss.leetcode.com/topic/30632/preorder-inorder-and-postorder-iteratively-summarization/2 这个作者把先序、中序、后序遍历都写了出来，用的是相同的结构，值得借鉴。

147.insertion sort list
--
```java
public ListNode insertionSortList(ListNode head) {
		if( head == null ){
			return head;
		}
		
		ListNode helper = new ListNode(0); //new starter of the sorted list
		ListNode cur = head; //the node will be inserted
		ListNode pre = helper; //insert node between pre and pre.next
		ListNode next = null; //the next node will be inserted
		//not the end of input list
		while( cur != null ){
			next = cur.next;
			//find the right place to insert
			while( pre.next != null && pre.next.val < cur.val ){
				pre = pre.next;
			}
			//insert between pre and pre.next
			cur.next = pre.next;
			pre.next = cur;
			pre = helper;
			cur = next;
		}
		
		return helper.next;
	}
```
148 sort list
--
```java
public class Solution {
    
    //merge two sorted list, return result head
    public ListNode merge(ListNode h1, ListNode h2){
        if(h1 == null){
            return h2;
        }
        if(h2 == null){
            return h1;
        }
        
        if(h1.val < h2.val){
            h1.next = merge(h1.next, h2);
            return h1;
        }
        else{
            h2.next = merge(h1, h2.next);
            return h2;
        }
        
    }
    
    public ListNode sortList(ListNode head) {
        //bottom case
        if(head == null){
            return head;
        }
        if(head.next == null){
            return head;
        }
        
        //p1 move 1 step every time, p2 move 2 step every time, pre record node before p1
        ListNode p1 = head;
        ListNode p2 = head;
        ListNode pre = head;
        
        while(p2 != null && p2.next != null){
            pre = p1;
            p1 = p1.next;
            p2 = p2.next.next;
        }
        //change pre next to null, make two sub list(head to pre, p1 to p2)
        pre.next = null;
        
        //handle those two sub list
        ListNode h1 = sortList(head);
        ListNode h2 = sortList(p1);
        
        return merge(h1, h2);
        
    }
    
}
```
作者用的类似于merge算法来解决，比较好懂。
150
--
这道题用的是stack，记得之前见过这样的题目，为什么就想不到呢
```java
public class Solution {
    public int evalRPN(String[] tokens) {
        int a,b;
		Stack<Integer> S = new Stack<Integer>();
		for (String s : tokens) {
			if(s.equals("+")) {
				S.add(S.pop()+S.pop());
			}
			else if(s.equals("/")) {
				b = S.pop();
				a = S.pop();
				S.add(a / b);
			}
			else if(s.equals("*")) {
				S.add(S.pop() * S.pop());
			}
			else if(s.equals("-")) {
				b = S.pop();
				a = S.pop();
				S.add(a - b);
			}
			else {
				S.add(Integer.parseInt(s));
			}
		}	
		return S.pop();
	}
}
```
152.Maximum Product Subarray
--
```java
int maxProduct(int A[], int n) {
    // store the result that is the max we have found so far
    int r = A[0];

    // imax/imin stores the max/min product of
    // subarray that ends with the current number A[i]
    for (int i = 1, imax = r, imin = r; i < n; i++) {
        // multiplied by a negative makes big number smaller, small number bigger
        // so we redefine the extremums by swapping them
        if (A[i] < 0)
            swap(imax, imin);

        // max/min product for the current number is either the current number itself
        // or the max/min by the previous number times the current one
        imax = max(A[i], imax * A[i]);
        imin = min(A[i], imin * A[i]);

        // the newly computed max value is a candidate for our global result
        r = max(r, imax);
    }
    return r;
}
```
153.
--
下面用的是二分搜索法，参见https://discuss.leetcode.com/topic/6468/my-pretty-simple-code-to-solve-it/2
虽然下面这个方法用于重复的元素，不重复的也可以。
```java
public class Solution {
 public  int findMin(int[] num) {
        int lo = 0;
        int hi = num.length - 1;
        int mid = 0;
        
        while(lo < hi) {
            mid = lo + (hi - lo) / 2;
            
            if (num[mid] > num[hi]) {
                lo = mid + 1;
            }
            else if (num[mid] < num[hi]) {
                hi = mid;
            }
            else { // when num[mid] and num[hi] are same
                hi--;
            }
        }
        return num[lo];
    }
```
154.
--
有人问了这样一个问题
a simple question: why mid = lo + (hi - lo) / 2 rather than mid = (hi + lo) / 2 ?
而有人回答是这样的：This is a famous bug in binary search. if the size of array are too large, equal or larger than the upper bound of int type, hi + lo may cause an overflow and become a negative number. It's ok to write (hi + lo) / 2 here, leetcode will not give you a very large array to test. But we'd better know this. For a detailed information or history of this bug, you could search "binary search bug" on google.<br>
具体代码见上题

160.
--
我自己的想法和下面这个差不多，为什么通过只通过了38/42个例子
自己写的
```java
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    	ListNode firstA=headA;
    	ListNode firstB=headB;
    	ListNode sec=new ListNode(0);
    	while(firstA!=null && firstB!=null){
    		firstA=firstA.next;
    		firstB=firstB.next;
    	}
    	if(firstA==null && firstB==null){
    		while(headA!=null){
    			if(headA==headB){
    				return headA;
    			}
    			headA=headA.next;
    			headB=headB.next;
    		}
    		return null;
    	}
    	else if(firstA==null){
    		sec=headB;
    	while(firstB!=null){
    		firstB=firstB.next;
    		sec=sec.next;
    	}
    	while(headA.next!=null){
    		if(headA==sec)
    			return headA;
    		headA=headA.next;
    		sec=sec.next;
    	}
    	return null;
    	}
    	else {
    		sec=headA;
    	while(firstA!=null){
    		firstA=firstA.next;
    		sec=sec.next;
    	}
    	while(headB.next!=null){
    		if(headB==sec)
    			return headB;
    		headB=headB.next;
    		sec=sec.next;
    	}
    	return null;
    	}	
    }
```
 别人的
```java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int lenA = length(headA), lenB = length(headB);
    // move headA and headB to the same start point
    while (lenA > lenB) {
        headA = headA.next;
        lenA--;
    }
    while (lenA < lenB) {
        headB = headB.next;
        lenB--;
    }
    // find the intersection until end
    while (headA != headB) {
        headA = headA.next;
        headB = headB.next;
    }
    return headA;
}

private int length(ListNode node) {
    int length = 0;
    while (node != null) {
        node = node.next;
        length++;
    }
    return length;
}
```
162.Find Peak Element 
--
```java
class Solution {
public:
    int findPeakElement(const vector<int> &num) 
    {
        int low = 0;
        int high = num.size()-1;
        
        while(low < high)
        {
            int mid1 = (low+high)/2;
            int mid2 = mid1+1;
            if(num[mid1] < num[mid2])
                low = mid2;
            else
                high = mid1;
        }
        return low;
    }
};
```
我自己就是很普通的从左到右遍历，上述方法用的是二分法搜索。
165. Compare Version Numbers
--
1、如果用“.”作为分隔的话,必须是如下写法,String.split("\\."),这样才能正确的分隔开,不能用String.split(".");

2、如果用“|”作为分隔的话,必须是如下写法,String.split("\\|"),这样才能正确的分隔开,不能用String.split("|");

“.”和“|”都是转义字符,必须得加"\\";
```java
public int compareVersion(String version1, String version2) {
    String[] levels1 = version1.split("\\.");
    String[] levels2 = version2.split("\\.");
    
    int length = Math.max(levels1.length, levels2.length);
    for (int i=0; i<length; i++) {
    	Integer v1 = i < levels1.length ? Integer.parseInt(levels1[i]) : 0;
    	Integer v2 = i < levels2.length ? Integer.parseInt(levels2[i]) : 0;
    	int compare = v1.compareTo(v2);
    	if (compare != 0) {
    		return compare;
    	}
    }
    
    return 0;
}
```
169. Majority Element
--
```java
public class Solution {
    public int majorityElement(int[] nums) {
        Arrays.sort(nums);
	  int len = nums.length;
	  return nums[len/2];
    }
}
```
这个做法挺精妙的。<br>
当然下面这个是Boyer–Moore majority vote algorithm的做法
```java
public class Solution {
    public int majorityElement(int[] num) {

        int major=num[0], count = 1;
        for(int i=1; i<num.length;i++){
            if(count==0){
                count++;
                major=num[i];
            }else if(major==num[i]){
                count++;
            }else count--;
            
        }
        return major;
    }
}
```这个做法的问题就是有局限性，如果当大多数的数目小于n/2时就会随便输出一个结果。

172.Factorial Trailing Zeroes
--
```java
    return n == 0 ? 0 : n / 5 + trailingZeroes(n / 5);
```
这个做法就是输出n到1中5的因子个数，不错！
173.Binary Search Tree Iterator
--
这个方法好精妙
```java
public class BSTIterator {
    private Stack<TreeNode> stack = new Stack<TreeNode>();
    
    public BSTIterator(TreeNode root) {
        pushAll(root);
    }

    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !stack.isEmpty();
    }

    /** @return the next smallest number */
    public int next() {
        TreeNode tmpNode = stack.pop();
        pushAll(tmpNode.right);
        return tmpNode.val;
    }
    
    private void pushAll(TreeNode node) {
        for (; node != null; stack.push(node), node = node.left);
    }
}
```
179.Largest Number
用了比较器
--
```java
public class Solution {
     public String largestNumber(int[] num) {
		if(num == null || num.length == 0)
		    return "";
		
		// Convert int array to String array, so we can sort later on
		String[] s_num = new String[num.length];
		for(int i = 0; i < num.length; i++)
		    s_num[i] = String.valueOf(num[i]);
			
		// Comparator to decide which string should come first in concatenation
		Comparator<String> comp = new Comparator<String>(){
		    @Override
		    public int compare(String str1, String str2){
		        String s1 = str1 + str2;
			String s2 = str2 + str1;
			return s2.compareTo(s1); // reverse order here, so we can do append() later
		    }
	        };
		
		Arrays.sort(s_num, comp);
                // An extreme edge case by lc, say you have only a bunch of 0 in your int array
                if(s_num[0].charAt(0) == '0')
                    return "0";
            
		StringBuilder sb = new StringBuilder();
		for(String s: s_num)
	            sb.append(s);
		
		return sb.toString();
		
	}
}
```
187. Repeated DNA Sequences
--
```java
public List<String> findRepeatedDnaSequences(String s) {
    Set seen = new HashSet(), repeated = new HashSet();
    for (int i = 0; i + 9 < s.length(); i++) {
        String ten = s.substring(i, i + 10);
        if (!seen.add(ten))
            repeated.add(ten);
    }
    return new ArrayList(repeated);
}
```
189.Rotate Array
--
```java
public class Solution {
    public void rotate(int[] nums, int k) {
        if(k==0)
        return ;
        int l=nums.length;
        if(k>l)
        k=k%l;
        int [] newnum=new int[l];
        for(int i=0;i<k;i++){
            newnum[i]=nums[l-k+i];
        }
        for(int j=k;j<l;j++){
            newnum[j]=nums[j-k];
        }
        for(int t=0;t<l;t++){  //1
        	nums[t]=newnum[t];//1
        }//1
   
    }
}
```
上面这种 做法是正确的，但是如果要将1处的代码该为nums=newnum，那么就是错的，这就是实参和形参的问题，形参变了，但是实际的参数nums并没有发生变化，所以输出的是没有改变的结果。

下面是别人的做法
比如  1,2,3,4,5,6  k=2，首先变成654321，然后前k个逆序，后边的逆序，变成561234 ，so cool
```java
public void rotate(int[] nums, int k) {
    k %= nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
}

public void reverse(int[] nums, int start, int end) {
    while (start < end) {
        int temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
}
```
190
--
```java
public int reverseBits(int n) {
    int result = 0;
    for (int i = 0; i < 32; i++) {
        result += n & 1;
        n >>>= 1;   // CATCH: must do unsigned shift
        if (i < 31) // CATCH: for last digit, don't shift!
            result <<= 1;
    }
    return result;
}
```
192.
--
这道题和上题的思想类似都是用的按位运算，比起用现成的把数转换成二进制字符串要好多了
```java
public static int hammingWeight(int n) {
	int ones = 0;
    	while(n!=0) {
    		ones = ones + (n & 1);
    		n = n>>>1;
    	}
    	return ones;
}
```
198.House Robber
--
https://discuss.leetcode.com/topic/11082/java-o-n-solution-space-o-1/8 解释的还是比较清楚
```java
public static int rob(int[] nums) 
	{
		int ifRobbedPrevious = 0; 	// max monney can get if rob current house
	    int ifDidntRobPrevious = 0; // max money can get if not rob current house
	    
	    // We go through all the values, we maintain two counts, 1) if we rob this cell, 2) if we didn't rob this cell
	    for(int i=0; i < nums.length; i++) 
	    {
	    	// If we rob current cell, previous cell shouldn't be robbed. So, add the current value to previous one.
	        int currRobbed = ifDidntRobPrevious + nums[i];
	        
	        // If we don't rob current cell, then the count should be max of the previous cell robbed and not robbed
	        int currNotRobbed = Math.max(ifDidntRobPrevious, ifRobbedPrevious); 
	        
	        // Update values for the next round
	        ifDidntRobPrevious  = currNotRobbed;
	        ifRobbedPrevious = currRobbed;
	    }
	    
	    return Math.max(ifRobbedPrevious, ifDidntRobPrevious);
	}
```
199. Binary Tree Right Side View  
--
```java
public class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<Integer>();
        rightView(root, result, 0);
        return result;
    }
    
    public void rightView(TreeNode curr, List<Integer> result, int currDepth){
        if(curr == null){
            return;
        }
        if(currDepth == result.size()){
            result.add(curr.val);
        }
        
        rightView(curr.right, result, currDepth + 1);
        rightView(curr.left, result, currDepth + 1);
        
    }
}
```
200.number of islands
--
深度优先算法，很不错啊；
```java
public class Solution {

private int n;
private int m;

public int numIslands(char[][] grid) {
    int count = 0;
    n = grid.length;
    if (n == 0) return 0;
    m = grid[0].length;
    for (int i = 0; i < n; i++){
        for (int j = 0; j < m; j++)
            if (grid[i][j] == '1') {
                DFSMarking(grid, i, j);
                ++count;
            }
    }    
    return count;
}

private void DFSMarking(char[][] grid, int i, int j) {
    if (i < 0 || j < 0 || i >= n || j >= m || grid[i][j] != '1') return;
    grid[i][j] = '0';
    DFSMarking(grid, i + 1, j);
    DFSMarking(grid, i - 1, j);
    DFSMarking(grid, i, j + 1);
    DFSMarking(grid, i, j - 1);
}
}
```
201.Bitwise AND of Numbers Range
```java
public class Solution {
    public int rangeBitwiseAnd(int m, int n) {
        if(m == 0){
            return 0;
        }
        int moveFactor = 1;
        while(m != n){
            m >>= 1;
            n >>= 1;
            moveFactor <<= 1;
        }
        return m * moveFactor;
    }
}
```
202.
--
`Floyd判圈算法(Floyd Cycle Detection Algorithm)`，又称`龟兔赛跑算法(Tortoise and Hare Algorithm)`。该算法由美国科学家罗伯特·弗洛伊德发明，是一个可以在有限状态机、迭代函数或者链表上判断是否存在环，求出该环的起点与长度的算法。[1]
如果有限状态机、迭代函数或者链表上存在环，那么在某个环上以不同速度前进的2个指针必定会在某个时刻相遇。同时显然地，如果从同一个起点(即使这个起点不在某个环上)同时开始以不同速度前进的2个指针最终相遇，那么可以判定存在一个环，且可以求出2者相遇处所在的环的起点与长度。
```c
int digitSquareSum(int n) {
    int sum = 0, tmp;
    while (n) {
        tmp = n % 10;
        sum += tmp * tmp;
        n /= 10;
    }
    return sum;
}

bool isHappy(int n) {
    int slow, fast;
    slow = fast = n;
    do {
        slow = digitSquareSum(slow);
        fast = digitSquareSum(fast);
        fast = digitSquareSum(fast);
    } while(slow != fast);
    if (slow == 1) return 1;
    else return 0;
}
```
这个用的居然是Floyd cycle detection algorithm，好厉害！
```java
public boolean isHappy(int n) {
    Set<Integer> inLoop = new HashSet<Integer>();
    int squareSum,remain;
	while (inLoop.add(n)) {
		squareSum = 0;
		while (n > 0) {
		    remain = n%10;
			squareSum += remain*remain;
			n /= 10;
		}
		if (squareSum == 1)
			return true;
		else
			n = squareSum;
	}
	return false;
}
```
下面这种方法用的是hash set方法。
203
--
```java
public class Solution {
    public int countPrimes(int n) {
        boolean[] notPrime = new boolean[n];
        int count = 0;
        for (int i = 2; i < n; i++) {
            if (notPrime[i] == false) {
                count++;
                for (int j = 2; i*j < n; j++) {
                    notPrime[i*j] = true;
                }
            }
        }
        
        return count;
    }
}
```
这个做法相当于利用y以前的结果，把n以内某数倍数的设为不是质数，不用再算了。
205. Isomorphic Strings
--
插入一个map需要用到put方法，得到对应的key的value要用到map.get(key)方法，判断是否包括某key要用到map.containsKey(key),判断是否包括某value要用到map.containsValue(value);
```java
String s1="abc";
int[] m=new int[256];
m[97]=89;
System.out.println(m[s1.charAt(0)]);
```
数组居然还有这样的用法，
```java
public class Solution {
    public boolean isIsomorphic(String s1, String s2) {
        int[] m = new int[512];
        for (int i = 0; i < s1.length(); i++) {
            if (m[s1.charAt(i)] != m[s2.charAt(i)+256]) return false;
            m[s1.charAt(i)] = m[s2.charAt(i)+256] = i+1;
        }
        return true;
    }
}
```
这个方法就是建立一个长度为512的数组，因为ASCII码的长度为256，然后前边部分是s1的映射，后半部分是s2的映射，然后看两个映射是否相同。
206.reverse linked list
--
```java
public ListNode reverseList(ListNode head) {
    /* iterative solution */
    ListNode newHead = null;
    while (head != null) {
        ListNode next = head.next;
        head.next = newHead;
        newHead = head;
        head = next;
    }
    return newHead;
}
```
207.
--
queue的方法：offer ，添加一个元素并返回true，如果队列已满，则返回false
```java
public boolean canFinish(int numCourses, int[][] prerequisites) {
    int[][] matrix = new int[numCourses][numCourses]; // i -> j
    int[] indegree = new int[numCourses];
    
    for (int i=0; i<prerequisites.length; i++) {
        int ready = prerequisites[i][0];
        int pre = prerequisites[i][1];
        if (matrix[pre][ready] == 0)
            indegree[ready]++; //duplicate case
        matrix[pre][ready] = 1;
    }
    
    int count = 0;
    Queue<Integer> queue = new LinkedList();
    for (int i=0; i<indegree.length; i++) {
        if (indegree[i] == 0) queue.offer(i);
    }
    while (!queue.isEmpty()) {
        int course = queue.poll();
        count++;
        for (int i=0; i<numCourses; i++) {
            if (matrix[course][i] != 0) {
                if (--indegree[i] == 0)
                    queue.offer(i);
            }
        }
    }
    return count == numCourses;
}
```
这个方法好像是According to the Wiki about what Topological sorting is (https://en.wikipedia.org/wiki/Topological_sorting)
and the Kahn's algorithm as shown below，这个方法。没怎么看懂。
