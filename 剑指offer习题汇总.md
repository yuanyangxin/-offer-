# **剑指offer汇总**

### 1. 数组中重复的数字

在一个长度为n的数组里的所有数字都在0到n-1的范围内。 数组中某些数字是重复的，但不知道有几个数字是重复的。也不知道每个数字重复几次。请找出数组中任意一个重复的数字。 例如，如果输入长度为7的数组{2,3,1,0,2,5,3}，那么对应的输出是第一个重复的数字2

自己想的：

~~~java
public class Solution 
{
    // Parameters:
    //    numbers:     an array of integers
    //    length:      the length of array numbers
    //    duplication: (Output) the duplicated number in the array number,length of duplication array is 1,so using duplication[0] = ? in implementation;
    //                  Here duplication like pointor in C/C++, duplication[0] equal *duplication in C/C++
    //    这里要特别注意~返回任意重复的一个，赋值duplication[0]
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    public boolean duplicate(int numbers[],int length,int [] duplication) 
    {
        if(numbers == null || numbers.length == 0) return false;
        
        for(int i=0;i<numbers.length;i++)
        {
            for(int j=i+1;j<numbers.length;j++)
            {
                if(numbers[i] == numbers[j])
                {
                    duplication[0]=numbers[i];
                    return true;
                }
            }
        }
        return false;
    }
}
~~~

剑指书的优化：

~~~java
public class Solution {
    // Parameters:
    //    numbers:     an array of integers
    //    length:      the length of array numbers
    //    duplication: (Output) the duplicated number in the array number,length of duplication array is 1,so using duplication[0] = ? in implementation;
    //                  Here duplication like pointor in C/C++, duplication[0] equal *duplication in C/C++
    //    这里要特别注意~返回任意重复的一个，赋值duplication[0]
    // Return value:       true if the input is valid, and there are some duplications in the array number
    //                     otherwise false
    /** 重排数组
      首先比较这个数字m是不是等于i；如果是，扫描下个数字；
      如果不是，那他和下标为m的数比较，如果它和第m个数字相等，找到了重复数字；不相等，就交换两个数字；
    */
   
    public boolean duplicate(int numbers[],int length,int [] duplication) 
    {
        if(numbers == null || length <2) return false;
        for(int i=0;i<length;i++)
        {
            if(numbers[i] == i){
                continue;
            }else if(numbers[numbers[i]] != numbers[i]){
                swap(numbers,i,numbers[i]);
            }else{
                duplication[0] = numbers[i];
                return true;
            } 
        }
        return false;
    }
    
    public void swap(int[] arr, int i ,int j)
    {
        int k = arr[i];
        arr[i] = arr[j];
        arr[j] = k;
    }
}
~~~



### 2. 不修改数组找出重复数字

不修改数组找出重复的数字。在一个长度为n+1的数组中的所有数字都在1～n的范围内，所以数组中至少有一个数字是重复的。在不修改输入数组的情况下找出数组中任意一个重复数字。例如输入长度为8的数组{2, 3, 5, 4, 3, 2, 6, 7}，则对应输出的是2或者3。

~~~JAVA
public static int findDunplicate(int[] arr)
    {
        if(arr == null || arr.length == 0) return -1;
        int[] copyarr = new int[arr.length+1];
        int index = 0;
        for(int i=0;i<arr.length;i++)
        {
            index = arr[i];
            if(copyarr[index] == 0 )
            {
                copyarr[index] = index;
            }else{
                return index;
            }
        }
        return 0;
    }
~~~

剑指的优化：

~~~~java
    public static int findDunplicate(int[] arr)
    {
        if(arr == null || arr.length == 0) return -1;
        int start = 1;
        int end = arr.length -1;
        while(end >= start)
        {
            int mid = start+(end-start)/2;
            int count = countRange(arr,start,mid);
            if(end == start)
            {
                if(count>1)
                {
                    return start;
                }else{
                    break;
                }
            }
            if(count>(mid-start+1))
            {
                end = mid;
            }else{
                start = mid+1;
            }
        }
        return 0;
    }

    public static int countRange(int[] arr, int start, int end)
    {
        if(arr.length==0 || arr == null) return 0;
        int count = 0;
        for(int i=0;i < arr.length;i++)
        {
            if(arr[i] >= start && arr[i] <= end) count++;
        }
        return count;
    }
~~~~

### 3. 二维数组中的查找

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

~~~java
public class Solution {
    // Tine：O(m+n) space:O(1)
    public boolean Find(int target, int [][] array) 
    {
        if(array==null||array[0]==null||array.length == 0|| array[0].length == 0)
            return false;
        int i=0,j = array[0].length-1;
        int m = array.length; //m行
        int n = array[0].length; //n列
        while(i>=0 && i<m && j>=0 && j<n)
        {
            if(array[i][j] == target)
                return true;
            else if(array[i][j] > target)
                j--;
            else
                i++;
        }
        return false;
    }
}
~~~

### 4.替换空格

请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

~~~java
/** 
原来换成char数组拼成字符串的方法就不写了，有漏洞
时间复杂度为 o(n)的解法：
新建一个替换空格后的数组，从新数组的后面开始存字符，碰到空格就从后向前插入 0，2，%
*/

public class Solution {
    public String replaceSpace(StringBuffer str) {
    	String strToString = str.toString();
        if(strToString.isEmpty()) return strToString;
        int countSpace = 0;
        char[] arr = strToString.toCharArray();
        for(int i=0;i<arr.length;i++)
        {
            if(arr[i]==' ') countSpace++;
        }
        char[] newArr = new char[arr.length + countSpace*2];
        int j = newArr.length-1;
        for(int i = arr.length-1; i>=0;i--)
        {
            if(arr[i] != ' ')
            {
                newArr[j] = arr[i];
            }else{
                newArr[j] = '0';
                newArr[--j] = '2';
                newArr[--j] = '%';
            }
            j--;
        }
        return String.valueOf(newArr);
    }
}
~~~

### 5.从尾到头打印链表

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

用栈：

~~~java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*
*/
import java.util.Stack;
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) 
    {
        Stack<Integer> stack = new Stack<Integer>();
        while(listNode != null)
        {
            stack.push(listNode.val);
            listNode = listNode.next;
        }
        ArrayList<Integer> list = new ArrayList<Integer>();
        while(!stack.isEmpty())
        {
            list.add(stack.pop());
        }
        return list;
    }
}
~~~

用递归实现（不建议）

~~~java
/**
*    public class ListNode {
*        int val;
*        ListNode next = null;
*
*        ListNode(int val) {
*            this.val = val;
*        }
*    }
*  用递归实现，每访问一个节点时，先递归输出其后面的节点，再输出自己
*/
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> printListFromTailToHead(ListNode listNode) 
    {
        ArrayList<Integer> list = new ArrayList<Integer>();
        ListNode pNode = listNode;
        if(pNode != null)
        {
            if(pNode.next != null)
            {
                list = printListFromTailToHead(pNode.next);
            }
            list.add(pNode.val);
        }
        return list;
    }
}
~~~

~~~java
import java.util.Stack;

/**
 * Author:Yangxin Yuan
 * Date:2018-10-28 19:00
 * Description:二叉树的三种遍历的递归和非递归实现
 */
public class erchashu
{
    public class TreeNode {
      int val;
      TreeNode left;
      TreeNode right;
      TreeNode(int x) { val = x; }
 }

    //前序遍历 非递归
    public void nonRecOrder(TreeNode head)
    {
        if(head == null)
        {
            return;
        }
        Stack<TreeNode> stack = new Stack<TreeNode>();
        stack.push(head);
        while(!stack.isEmpty())
        {
            TreeNode head = stack.pop();
            System.out.println(head.val+" ");//弹出根节点
            //如果有右节点，先压入栈
            if(head.right != null)
            {
                stack.push(head.right);
            }
            if(head.left != null)
            {
                stack.push(head.left);
            }
        }
    }
    //中序 非递归 左中右
    public void midRecOrder(TreeNode head)
    {
        Stack<TreeNode> stack = new Stack<TreeNode>();
        while(!stack.isEmpty() || head != null)
        {
            if(head != null)
            {
                stack.push(head);//第一次是把根节点压入
                head = head.left; //左边全部压入栈
            }else{
                head = stack.pop();// 左边的全部弹出
                System.out.println(head.val);
                head = head.right;//右边的压入栈
            }
        }
    }

    //后序 非递归 左右中
    public void backRecOrder(TreeNode head)
    {
        Stack<TreeNode> stack1 = new Stack<TreeNode>();
        Stack<TreeNode> stack2 = new Stack<TreeNode>();
        stack1.push(head);
        while(!stack1.isEmpty())
        {
            head = stack1.pop();
            stack2.push(head);//stack2最下面压的根节点
            if(head.left != null)
            {
                stack1.push(head.left);//stack1中在下面，stack2在上面
            }
            if(head.right != null)
            {
                stack1.push(head.right);
            }
        }
        while(!stack2.isEmpty())
        {
            System.out.println(stack2.pop().val);
        }
    }
    
    //三种的递归实现
    public static void OrderRecur(TreeNode head) {
        if (head == null) {
            return;
        }
        System.out.print(head.val + " "); //前序
        OrderRecur(head.left);
       // System.out.print(head.val + " "); //中序
        OrderRecur(head.right);
        //System.out.print(head.val + " "); //后序
    }
}

~~~



### 6.重建二叉树

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

~~~java
/**
 * Definition for binary tree
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
       前序遍历的第一个数是根节点，
       扫描中序遍历，找到根节点；根节点前面是左子树，后面是右子树；
       由此可确定前序遍历中的左右子树的个数
       后面用递归实现
 * }
 */
public class Solution {
      public TreeNode reConstructBinaryTree(int [] pre,int [] in)
    {
        if(pre == null || in ==null||pre.length!= in.length)
        {
            return null;
        }else{
            return reBuild(pre,0,pre.length-1,in,0,in.length-1);
        }
    }

    private TreeNode reBuild(int[] pre, int startPre, int endPre,
                             int[] in, int startIn, int endIn)
    {
        if(startPre > endPre || startIn>endIn) return null;
        int root = pre[startPre];//前序的第一个是中序节点
        int k = locate(root,in,startIn,endIn) - startIn;

        TreeNode newNode = new TreeNode(root);
        newNode.left = reBuild(pre,startPre+1,startPre+k
        ,in,startIn,startIn+k);
        newNode.right = reBuild(pre,startPre+k+1,endPre,in,startIn+k+1,endIn);
        return newNode;
    }

    //找根节点在中序数组中的位置，前面是左子树的中序数组，后面是右子树的中序数组
    private int locate(int root, int[] in,int startIn,int endIn)
    {
        for(int i=startIn;i<=endIn;i++)
        {
            if(root == in[i])
            {
                return i;
            }
        }
        return -1;
    }
}
~~~

### 7.二叉树的下一个结点

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

~~~java
/*
public class TreeLinkNode {
    int val;
    TreeLinkNode left = null;
    TreeLinkNode right = null;
    TreeLinkNode next = null;

    TreeLinkNode(int val) {
        this.val = val;
    }
}
*/
// 中序遍历：
// 1、节点有右子树，下一个节点就是它的右子树中的最左节点
// 2、节点没有右子树且还是父节点的右子节点--->沿着父节点的指针向上遍历，直到找到一个左节点，这个左节点的父节点就是下一个节点
public class Solution {
    public TreeLinkNode GetNext(TreeLinkNode node)
    {
        if(node==null) return null;
        if(node.right!=null){    //如果有右子树，则找右子树的最左节点
            node = node.right;
            while(node.left!=null) node = node.left;
            return node;
        }
        while(node.next!=null){ //没右子树，则找第一个当前节点是父节点左孩子的节点
            if(node.next.left==node) return node.next;
            node = node.next;
        }
        return null;   //退到了根节点仍没找到，则返回null
    }
}
~~~



### 8.用两个栈实现队列

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

~~~java
/**
* push先全部放stack1里面
  pop的时候先把stack1里面的东西都放到pop的stack2中
  push里面所有东西都要倒完
*/
import java.util.Stack;

public class Solution {
    Stack<Integer> stack1 = new Stack<Integer>();
    Stack<Integer> stack2 = new Stack<Integer>();
    
    public void push(int node) 
    {
        stack1.push(node);
    }
    
    public int pop() 
    {
        int a;
        if(stack2.empty())
        {
            while(!stack1.empty())
            {
                stack2.push(stack1.pop());
            }
        }
        return stack2.pop();
    }
}
~~~



### 9.斐波那契数列

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。

n<=39。

教科书解法略

~~~java
public class Solution {
    public int Fibonacci(int n) 
    {
        int[] result = {0,1};
        if(n<2) return result[n];
        int one = 1;
        int two = 0;
        int k=0;
        for(int i=2;i<=n;i++)
        {
            k= one + two;
            two = one;
            one = k;
        }
        return k;
    }
}
~~~

青蛙跳台阶也是这类问题。唯一需要注意的，n=台阶数+1

### 10.旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组{3,4,5,1,2}为{1,2,3,4,5}的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0。

时间复杂度为o(n):

~~~java
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) 
    {
        if(array.length == 0 || array == null)
        {
            return 0;
        }
        int min = -1;
        for(int i=0;i<array.length-1;i++)
        {
            if(array[i]<=array[i+1])
            {
                continue;
            }else{
                min = array[i+1];
                break;
            } 
        }
        return min;
    }
}
~~~

用二分法将时间复杂度降至o(logn)

~~~java
// 用二分法
// 中间元素如果位于前面递增数组，它要大于第一个元素；如果位于后面递增数组，它要小于最后一个元素
//如果min，max，mid三者相等，无法顺序查找，需要遍历
import java.util.ArrayList;
public class Solution {
    public int minNumberInRotateArray(int [] array) 
    {
        if(array.length == 0 || array == null) return -1;
        int min = 0;
        int max = array.length-1;
        int mid = (min+max)/2;
        while(array[min] >= array[max])
        {
            if(max-min == 1)
            {
                mid = max;
                break;
            }
            mid = (min+max)/2;
            //三个值相等时，无法判断，顺序查找
            if(array[min] == array[mid] && array[mid] == array[max])
            {
                return minOrder(array,min,max);
            }
            if(array[mid] >= array[min])
            {
                min = mid;
            }else{
                max = mid;
            }
        }
        return array[mid];
    }
    
    private int minOrder(int[] num,int left,int right)
    {
        int result = num[left];
        for(int i = left+1;i<right;i++)
        {
            if(num[i]<result)
            {
                result = num[i];
            }
        }
        return result;
    }
    
}
~~~

### 11.矩阵中的路径 暂时不会

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。如果一条路径经过了矩阵中的某一个格子，则之后不能再次进入这个格子。 例如 a b c e s f c s a d e e 这样的3 X 4 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。



### 12.机器人的运动范围 暂时不会

地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

 



### 13.剪绳子 暂时不会

给你一根长度为n的绳子，请把绳子剪成m段 (m和n都是整数，n>1并且m>1)每段绳子的长度记为k[0],k[1],...,k[m].请问k[0]*k[1]*...*k[m]可能的最大乘积是多少？

例如，当绳子的长度为8时，我们把它剪成长度分别为2,3,3的三段，此时得到的最大乘积是18.

~~~java
public int maxAfterCutting(int length)
        {
            if(length<2) return 0;
            if(length == 2) return 1;
            if(length == 3) return 2;

            int[] f = new int[length+1];
            f[0] = 0;
            f[1] = 1;
            f[2] = 2;
            f[3] = 3;
            int result = 0;
            for(int i =4;i<=length;i++)
            {
                int max = 0;
                for(int j=1;j<=i/2;j++)
                {
                    int num = f[j]*f[j-1];
                    if(max<num)
                        max = num;
                    f[i] = max;
                }
            }
            result = f[length];
            return result;
        }
~~~

### 14.二进制中1的个数

输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。

~~~java
public class Solution {
    public int NumberOf1(int n) 
    {
        int count = 0;
        while(n != 0){
            count++;
            n = n & (n - 1);
        }
        return count;
    }
}
~~~



### 15.数值的整数次方

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

高效的O(logn)算法：

~~~java
public class Solution {
    public double Power(double base, int exponent) {
        int n = Math.abs(exponent);
        if(n == 0) return 1;
        if(n == 1) return base;
        if(n==1) return 1/base;
        double result = Power(base,n>>1);
        result *= result;
        if((n&1) == 1) result*=base;
        if(exponent<0) result = 1/result;
        return result;
  }
}
~~~

### 16.打印从1到最大的n位数

输入数字n,按顺序打印出从1最大的n位十进制数。比如输入3，则打印出1、2、3一直到最大的3位数即999 

考虑大数问题，用字符串存储数字，用递归实现全排列：

~~~java
/**
 * Author:Yangxin Yuan
 * Date:2018-10-31 18:48
 * Description:<描述>
 */
public class solution {
    public static void Print1ToMaxOfNDigits_2(int n){
        if(n <=0 ) return;
        StringBuffer s = new StringBuffer(n);
        for(int i=0;i<n;i++)
        {
            s.append('0');
        } //先把每一位都设置为0
        for(int i=0;i<10;i++)
        {
            s.setCharAt(0,(char)(i+'0'));
            printRecursively(s,n,0);
        }//递归实现全排列 
    }
    public static void printRecursively(StringBuffer s, int length,int index)
    {
        if(index == length-1)
        {
            PrintNumber(s); //全部排列完就打印出来
            return;
        }
        for(int i=0;i<10;i++)
        {
            s.setCharAt(index+1,(char)(i+'0'));
            printRecursively(s,length,index+1);
        }
    }
    public static void PrintNumber(StringBuffer s) //打印数字，前面的0不打印
    {
        boolean isBegining0 = true;
        for(int i= 0;i<s.length();i++)
        {
            if(isBegining0 && s.charAt(i) != '0')
            {
                isBegining0 = false;
            }
            if(!isBegining0)
            {
                System.out.print(s.charAt(i));
            }
        }
        System.out.println();
    }
    public static void main(String[] args) {
        Print1ToMaxOfNDigits_2(300);
    }
}
~~~

### 17.删除链表的节点

给定单向链表的头指针和一个节点指针，定义一个函数在O(1)时间删除该节点

~~~java
/**
 * Author:Yangxin Yuan
 * Date:2018-10-31 18:48
 * Description:<描述>
 */
public class solution {
    public static class ListNode
    {
        public int data;
        public ListNode next;
        public ListNode(int data, ListNode next)
        {
            this.data = data;
            this.next = next;
        }
    }
    public static ListNode deleteNode(ListNode head, ListNode node)
    {
        if(node == head) //头结点
        {
            return head.next;
        }else if(node.next != null){ //中间节点
            node.data = node.next.data;
            node.next = node.next.next;
            return head;
        }else{ //尾节点
            ListNode temp = head;
            while(temp.next != node)
                temp = temp.next;
            temp.next = null;
            return head;
        }
    }

    public static void main(String[] args) {
        ListNode tail = new ListNode(1,null);
        ListNode c = new ListNode(2,tail);
        ListNode b = new ListNode(3,c);
        ListNode head = new ListNode(4,b);
        ListNode newNode = deleteNode(head,tail);
        while (newNode != null){
            System.out.println(newNode.data);
            newNode = newNode.next;
        }
    }
}
~~~

### 18.删除链表中的重复节点

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。

~~~java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    
 public ListNode deleteDuplication(ListNode pHead) {
        if (pHead == null || pHead.next == null) { // 只有0个或1个结点，则返回
            return pHead;
        }
        if (pHead.val == pHead.next.val) { // 当前结点是重复结点
            ListNode pNode = pHead.next;
            while (pNode != null && pNode.val == pHead.val) {
                // 跳过值与当前结点相同的全部结点,找到第一个与当前结点不同的结点
                pNode = pNode.next;
            }
            return deleteDuplication(pNode); // 从第一个与当前结点不同的结点开始递归
        } else { // 当前结点不是重复结点
            pHead.next = deleteDuplication(pHead.next); // 保留当前结点，从下一个结点开始递归
            return pHead;
        }
    }
}
~~~

### 19.正则表达式匹配

请实现一个函数用来匹配包括'.'和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（包含0次）。 在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但是与"aa.a"和"ab*a"均不匹配

~~~java
//1、 字符串和模式中第一个字符匹配，都后移一个字符，继续比较；不匹配返回false
//2、 匹配中的第二个字符是 *：
//     （1）字符串第一个字符和模式第一个字符不匹配，模式后移2个字符
//      (2)字符串第一个字符和模式第一个字符匹配: 
//             a、模式后移2个字符，---》x*被忽略
//             b、字符串后移1个字符，模式后移2个字符 ---》 x*匹配一次
//             c、字符串后移1个字符，模式不变 ---》 x*匹配多次

public class Solution {
    public boolean match(char[] str, char[] pattern)
    {
        if(str == null || pattern == null) 
            return false;
        int i =0;
        int j =0;
        return matchProcess(str, i, pattern ,j); //i，j是指针
    }
    
    private boolean matchProcess(char[] str,int i,char[] pattern,int j)
    {
        if(i== str.length && j ==  pattern.length)  return true;
        if(i != str.length && j ==  pattern.length) return false;
        if(j+1 < pattern.length && pattern[j+1] == '*' )
        {
            if((i != str.length && str[i] == pattern[j]) || (pattern[j] == '.' && i != str.length) ) 
            {
                return matchProcess(str,i+1,pattern,j+2) || //都往后移动,匹配1次
                    matchProcess(str,i+1,pattern,j) || // 匹配多次
                    matchProcess(str,i,pattern,j+2); // 匹配0次
            }else{
                return matchProcess(str,i,pattern,j+2); // *前面的不匹配，忽略
            }
        }
        if((i != str.length && str[i] == pattern[j]) || (pattern[j] == '.' && i != str.length )){
            return matchProcess(str, i+1, pattern, j+1);
        }
        return false;    
    }
}
~~~

### 20.表示数值的字符串

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。例如，字符串"+100","5e2","-123","3.1416"和"-1E-16"都表示数值。 但是"12e","1a3.14","1.2.3","+-5"和"12e+4.3"都不是。

1、书上的分析的做法

~~~java
/**
A[.[B]][e|EC] 或者 .B[e|EC]
A为数值的整数部分，B为小数部分，C为指数部分，A可以省略
A，C可以以+-开头，B不可以
*/
public class Solution {
    private int index = 0;
    public boolean isNumeric(char[] str) 
    {
        if(str.length < 1) 
            return false;
        boolean flag = scanInter(str);
        if(index<str.length && str[index] == '.')
        {
            index++;
            flag = scanUnsignedCharacter(str) || flag; 
        }
        if(index < str.length && (str[index] == 'E' || str[index] == 'e'))
        {
            index++;
            flag = flag && scanInter(str);
        }
        return flag && index == str.length;
        
    }
    private boolean scanInter(char[] str)
    {
        if(index<str.length && (str[index] == '+' || str[index] == '-'))
        {
            index++;
        }
        return scanUnsignedCharacter(str);
    }
    
    private boolean scanUnsignedCharacter(char[] str)
    {
        int start = index;
        while(index < str.length && str[index] >= '0' && str[index] <= '9')
        {
            index++;
        }
        return index > start;
    }
}
~~~

2、正则表达式的做法

~~~java
/**
A[.[B]][e|EC] 或者 .B[e|EC]
A为数值的整数部分，B为小数部分，C为指数部分，A可以省略
A，C可以以+-开头，B不可以
? 0或1次 ； * 任意次（0或n次）; + 一次或者多次
在 Java 中正则表达式中则需要有两个反斜杠才能被解析为其他语言中的转义作用
() 匹配（）里面所有表达式； [] 
*/
public class Solution {
    public boolean isNumeric(char[] str) 
    {
        if(str.length<1) return false;
        String string = String.valueOf(str);
        return string.matches("[\\+\\-]?\\d*(\\.\\d+)?([eE][\\+\\-]?\\d+)?");        
    }
}
~~~

### 21.调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

~~~java
/**
 * 1.要想保证原有次序，则只能顺次移动或相邻交换。
 * 2.i从左向右遍历，找到第一个偶数。
 * 3.j从i+1开始向后找，直到找到第一个奇数。
 * 4.将[i,...,j-1]的元素整体后移一位，最后将找到的奇数放入i位置，然后i++。
 * 5.終止條件：j向後遍歷查找失敗。
 */
public class Solution {
    public void reOrderArray(int [] array) 
    {
        if(array == null || array.length == 0)
        {
            return;
        }
        int i =0,j;
        while(i<array.length)
        {
            while(i<array.length && isEven(array[i]))// i位置是奇数
                i++;
            j=i+1;
            while(j<array.length && !isEven(array[j])) //j是偶数
                j++;
            if(j<array.length)
            {
                int tmp = array[j];
                for(int j2 = j-1;j2 >= i;j2--)
                {
                    array[j2+1] = array[j2];
                }
                array[i++] = tmp;
            }else{
                break;
            }
        }
    }
    private boolean isEven(int n)
    {
        if(n %2 == 1)
        {
            return true;
        }else{
            return false;
        }
    }
}
~~~





### 22.链表中倒数第k个结点

输入一个链表，输出该链表中倒数第k个结点。

~~~java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
    单向列表，无法从尾部回去； 
    n个节点的链表，倒数第k个节点就是n-k+1个节点
    两个指针，第一个先走k-1步；
    从第k步开始，两个指针一起走，第一个走到结尾时，第二个正巧在倒数第k个节点
}*/
public class Solution {
    public ListNode FindKthToTail(ListNode head,int k) 
    {
        if(head == null || k <= 0 ) return null;
        int i;
        int j;
        ListNode p1 = head;
        ListNode p2 = head;
        for( i = 0; i < k-1; i++)
        {
            if(p1.next != null)
            {
                p1 = p1.next;
            }else{
                return null;
            }
        }
        while(p1.next != null)
        {
                p1 = p1.next;
                p2 = p2.next;
        }
        return p2;
    }
}
~~~



### 23.链表中环的入口结点

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

~~~java
/*
 public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}
1、确定是否包含环
    两个指针，一个每次走一步，一个每次走两步，如果两个相遇了就有环，否则没有
2、找到环中节点的个数
    两个相遇的节点一定在环中，记录相遇节点，一边向前一边计数，重回到该结点就是环中节点的个数
3、找到环的节点
    如果链表的环有n个结点，指针1先移动n步，然后两个一起移动，相遇点就是入口节点
*/
public class Solution {
    public ListNode EntryNodeOfLoop(ListNode head)
    {
        ListNode meetingNode = meetingNode(head);
        if(meetingNode == null) return null;
        int nodeInLoop = 1;
        ListNode p1 = meetingNode;
        while(p1.next != meetingNode)
        {
            p1 = p1.next;
            nodeInLoop++;
        }
        p1 = head;
        for(int i=0;i<nodeInLoop;i++)
        {
            p1 = p1.next;
        }
        ListNode p2 = head;
        while(p1 != p2)
        {
            p1 = p1.next;
            p2 = p2.next;
        }
        return p1;
    }  
    
    public ListNode meetingNode(ListNode head)
    {
        if(head == null) return null;
        ListNode slow = head.next;
        if(slow == null) return null;
        ListNode fast = slow.next;
        while(slow != null && fast != null)
        {
            if(slow == fast) return fast;
            slow = slow.next;
            fast = fast.next;
            if(fast != null) 
                fast = fast.next;
        }
        return null;
    }
}
~~~



### 24.反转链表

输入一个链表，反转链表后，输出新链表的表头。

~~~java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
    调整节点时，我们需要定义3个指针，分别指向当前遍历到的节点、它的前后节点
}*/
public class Solution {
    public ListNode ReverseList(ListNode head) 
    {
        if(head == null) return null;
        ListNode pre = null; //当前节点前一个节点
        ListNode next = null; //当前节点后一个节点
        //1->2->3->4->5
        //1<-2<-3 4->5
        while(head != null)
        {
            //做循环，如果当前节点不为空的话，始终执行此循环，此循环的目的就是让当前节点从指向next到指向pre
            //如此就可以做到反转链表的效果
            //先用next保存head的下一个节点的信息，保证单链表不会因为失去head节点的原next节点而就此断裂
            next = head.next;     //保存完next，就可以让head从指向next变成指向pre了，代码如下
            //head指向pre后，就继续依次反转下一个节点
            head.next = pre;
            //让pre，head依次向后移动一个节点，继续下一次的指针反转
            pre = head;
            head = next;
        }
        return pre;
    }
}
~~~



### 25.合并两个排序的链表

输入两个单调递增的链表，输出两个链表合成后的链表，当然我们需要合成后的链表满足单调不减规则。

~~~java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
    首先确定头节点：两个链表中头节点的较小值作为头节点
    递归确定下面的节点 
}*/
public class Solution {
    public ListNode Merge(ListNode list1,ListNode list2) 
    {
        if(list1 == null)
            return list2;
        if(list2 == null)
            return list1;
        ListNode newNode = null;
        if(list1.val < list2.val )
        {
            newNode = list1;
            newNode.next = Merge(list1.next,list2);
        }else{
            newNode = list2;
            newNode.next = Merge(list1,list2.next);
        }
        return newNode;
    }
}
~~~

### 26.树的子结构

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

~~~java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
 遍历A查找B的头节点，判断相同节点下面的结构是否含有和B相同的结构
*/
public class Solution {
    public boolean HasSubtree(TreeNode root1,TreeNode root2) {
        boolean result = false;
        if(root1 != null && root2 != null)
        {
            if(root1.val == root2.val)
            {
                result = secondCheck(root1,root2);
            }
            if(!result)
                result = HasSubtree(root1.left,root2);
            if(!result)
                result = HasSubtree(root1.right,root2);
        }
        return result;
    }
    
    private boolean secondCheck(TreeNode root1,TreeNode root2)
    {
        if(root2 == null)
            return true;
        if(root1 == null)
            return false;
        if(root1.val != root2.val)
            return false;
        return secondCheck(root1.left,root2.left) &&
            secondCheck(root1.right,root2.right);
    }
}
~~~

### 27.二叉树的镜像

操作给定的二叉树，将其变换为源二叉树的镜像。

```
             8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11  8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11
    	镜像二叉树
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11 9 7  5
```

~~~java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
遍历树的每个节点，如果遍历到的节点有子节点，就交换这两个节点（只有一个节点的也要交换）
*/
public class Solution {
    public void Mirror(TreeNode root) 
    {
        if(root == null) return;
        
        TreeNode newNode = root.left;
        root.left = root.  8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7 9 11right;
        root.right = newNode;
        
        if(root.left!=null)
            Mirror(root.left); 
        if(root.right!=null)
            Mirror(root.right); 
    }
}
~~~

### 28.对称的二叉树

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。

~~~java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
比较二叉树的前序遍历序列和对称前序遍历序列来判断二叉树是否对称
}
*/
public class Solution {
    boolean isSymmetrical(TreeNode pRoot)
    {
        if(pRoot == null) return true;
        return isSymmetrical(pRoot,pRoot);
    }
    
    private boolean isSymmetrical(TreeNode p1, TreeNode p2)
    {
        if(p1 == null && p2 == null) 
            return true;
        if(p1==null || p2 == null)
            return false;
        if(p1.val != p2.val)
            return false;
        return isSymmetrical(p1.left,p2.right) && isSymmetrical(p1.right,p2.left);
    }
}
~~~

### 29.顺时针打印矩阵 

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字

~~~java
/**
* 设计宏观调度能力，不局限局部位置的变化
* 先打印一个框,再确定循环
*/
public class Solution {
    public static void main(String[] args)
    {
        int[][] m = {{1,2,3,10},{4,5,6,11},{7,8,9,12}};
        printMatrix(m);
    }

    public static void printMatrix(int [][] matrix)
    {
        int r1 = 0;
        int c1 = 0;
        int r2 = matrix.length-1;
        int c2 = matrix[0].length-1;
        while(r1<=r2 && c1 <= c2)
        {
            printBox(matrix,r1++,c1++,r2--,c2--);
        }

    }

    //打印从(r1,c1)到(r2,c2)的框
    private static void printBox(int[][] m, int r1, int c1, int r2, int c2)
    {
        if(r1 == r2)
        {
            for(int i=c1;i<=c2;i++)
            {
                System.out.print(m[r1][i]+" ");
            }
        }else if(c1 == c2){
            for(int j=r1;j<=r2;j++)
            {
                System.out.print(m[j][c1]+" ");
            }
        }else{
            int curRow = r1;
            int curCol = c1;
            while(curCol<c2)
            {
                System.out.print(m[r1][curCol++]+" ");
            }
            while(curRow<r2)
            {
                System.out.print(m[curRow++][c2]+" ");
            }
            while(curCol > c1)
            {
                System.out.print(m[r2][curCol--]+" ");
            }
            while(curRow > r1)
            {
                System.out.print(m[curRow--][c1]+" ");
            }
        }

    }
}
~~~



### 30.包含min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数(时间复杂度应为O(1) )

~~~java
import java.util.Stack;

public class Solution {
    Stack<Integer> dataStack = new Stack<Integer>();
    Stack<Integer> minStack = new Stack<Integer>();
    
    public void push(int node) 
    {
        dataStack.push(node);
        if(minStack.isEmpty() || node < minStack.peek())
        {
            minStack.push(node);
        }else{
            minStack.push(minStack.peek());
        }
    }
    
    public void pop() {
        minStack.pop();
        dataStack.pop();
    }
    
    public int top() {
        return dataStack.peek();
    }
    
    public int min() {
        return minStack.peek();
    }
}
~~~

### 31.栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

~~~java
// 借用辅助栈，遍历压栈顺序；判断栈顶元素 == 出栈顺序的第一个元素？
// 相等栈顶元素出栈，出栈顺序后移1位
// 不相等继续遍历压栈序列
import java.util.*;

public class Solution {
    public boolean IsPopOrder(int [] pushA,int [] popA) 
    {
        if(pushA.length==0 || popA.length == 0)
            return false;
        Stack<Integer> s = new Stack<Integer>();
        int popIndex = 0;
        for(int i=0;i<pushA.length;i++)
        {
            s.push(pushA[i]);
            while(!s.empty() && s.peek() == popA[popIndex])
            {
                s.pop();
                popIndex++;
            }
        }
        return s.empty();
    }
}
~~~

### 32.从上往下打印二叉树 广度优先遍历

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

~~~java
// 新建一个队列，把根结点放入队列
// 以后每次先弹出一个节点，并把该结点的左右节点放入队列
//queue 增加元素 queue.offer(); 
//返回第一个元素，并在队列中删除  queue.poll()
//返回第一个元素 queue.peek()
import java.util.*;
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }

}
*/
public class Solution {
    public ArrayList<Integer> PrintFromTopToBottom(TreeNode root)
    {
        ArrayList<Integer> as = new ArrayList<Integer>();
        Queue<TreeNode> que = new LinkedList<TreeNode>();
        if(root == null) return as;
        que.offer(root);
        while(!que.isEmpty())
        {
            TreeNode addNode = que.poll();
            as.add(addNode.val);
            if(addNode.left != null)
                que.offer(addNode.left);
            if(addNode.right != null)
                que.offer(addNode.right);
        }
        return as;
    }
}
~~~

### 33.按之字形顺序打印二叉树

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

~~~java
import java.util.*;
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
ArrayList<ArrayList<Integer>> Print(TreeNode pRoot) {
        int layer = 1;
        //s1存奇数层节点
        Stack<TreeNode> s1 = new Stack<TreeNode>();
        s1.push(pRoot);
        //s2存偶数层节点
        Stack<TreeNode> s2 = new Stack<TreeNode>();
         
        ArrayList<ArrayList<Integer>> list = new ArrayList<ArrayList<Integer>>();
         
        while (!s1.empty() || !s2.empty()) 
        {
            if (layer%2 != 0) {
                ArrayList<Integer> temp = new ArrayList<Integer>();
                while (!s1.empty()) {
                    TreeNode node = s1.pop();
                    if(node != null) {
                        temp.add(node.val);
                        System.out.print(node.val + " ");
                        s2.push(node.left);
                        s2.push(node.right);
                    }
                }
                if (!temp.isEmpty()) {
                    list.add(temp);
                    layer++;
                    System.out.println();
                }
            } else {
                ArrayList<Integer> temp = new ArrayList<Integer>();
                while (!s2.empty()) {
                    TreeNode node = s2.pop();
                    if(node != null) {
                        temp.add(node.val);
                        System.out.print(node.val + " ");
                        s1.push(node.right);
                        s1.push(node.left);
                    }
                }
                if (!temp.isEmpty()) {
                    list.add(temp);
                    layer++;
                    System.out.println();
                }
            } 
        }
        return list;
    }
}
~~~

### 34.二叉搜索树的后序遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

~~~java
/**
二叉查找树（Binary Search Tree），（又：二叉搜索树，二叉排序树）它或者是一棵空树，
或者是具有下列性质的二叉树： 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值； 
若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值； 它的左、右子树也分别为二叉排序树。
左子树<=根<=右子树
*/
public class Solution {
    public boolean VerifySquenceOfBST(int [] sequence) 
    {
        if(sequence.length == 0)
            return false;
        if(sequence.length == 1)
            return true;
        return judge(sequence,0,sequence.length-1);
    }
    
    public boolean judge(int[] a,int start,int end)
    {
        if(start>=end)
            return true;
        int i = start;
        while(a[i] < a[end])
        {
            i++;
        }
        for(int j=i;j<end;j++)
        {
            if(a[j]<a[end])
            {
                return false;
            }
        }
        return judge(a,start,i-1) && judge(a,i,end-1);
    }
}
~~~

### 35.二叉树中和为某一值的路径

输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

            10
    	   /  \
    	  5   12
    	 / \   
    	4  7 
    和为22的路径有两条：10->12;  10->5->7;
~~~java
import java.util.ArrayList;
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) 
    {
        ArrayList<ArrayList<Integer>> paths = new ArrayList<ArrayList<Integer>>();
        if(root == null) return paths;
        find(paths,new ArrayList<Integer>(),root,target);
        return paths;
    }
    public void find(ArrayList<ArrayList<Integer>> paths,ArrayList<Integer> path,TreeNode root,int target)
    {
        path.add(root.val);
        if(root.left == null && root.right == null)
        {
            if(target == root.val)
            {
                paths.add(path);
            }
            return;
        }
        ArrayList<Integer> path2 = new ArrayList<>();
        path2.addAll(path);
        if(root.left != null)
            find(paths,path,root.left,target-root.val);
        if(root.right != null)
            find(paths,path2,root.right,target-root.val);
    }
    
}
~~~

### 36.复杂链表的复制

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

~~~java
/*
public class RandomListNode {
    int label;
    RandomListNode next = null;
    RandomListNode random = null;

    RandomListNode(int label) {
        this.label = label;
    }
}
*1、遍历链表，复制每个结点，如复制结点A得到A1，将结点A1插到结点A后面；
*2、重新遍历链表，复制老结点的随机指针给新结点，如A1.random = A.random.next;
*3、拆分链表，将链表拆分为原链表和复制后的链表
*/
public class Solution {
    public RandomListNode Clone(RandomListNode pHead)
    {
        if(pHead == null) return null;
        RandomListNode currentNode = pHead;
        //1、复制每个结点，如复制结点A得到A1，将结点A1插到结点A后面；
        while(currentNode != null)
        {
            RandomListNode cloneNode = new RandomListNode(currentNode.label);
            RandomListNode nextNode = currentNode.next;
            currentNode.next = cloneNode;
            cloneNode.next = nextNode;
            currentNode = nextNode;
        }
        currentNode = pHead;
        
        //2、重新遍历链表，复制老结点的随机指针给新结点，如A1.random = A.random.next;
        while(currentNode != null)
        {
            currentNode.next.random = currentNode.random==null?null:currentNode.random.next;
            currentNode = currentNode.next.next;
        }
        //3、拆分链表，将链表拆分为原链表和复制后的链表
        currentNode = pHead;
        RandomListNode pCloneHead = pHead.next;
        while(currentNode != null)
        {
            RandomListNode cloneNode = currentNode.next;
            currentNode.next = cloneNode.next;
            cloneNode.next = cloneNode.next==null?null:cloneNode.next.next;
            currentNode = currentNode.next;
        }
        return pCloneHead;
    }
}
~~~

### 37.二叉搜索树与双向链表

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

~~~java
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
原先指向左节点的指针调整为链表中指向前一个节点的指针
原先指向右节点的指针调整为链表中指向最后一个节点的指针
根节点将和左子树的最大值和右子树的最小值连起来
*/
public class Solution {
    public TreeNode Convert(TreeNode root) 
    {
        if(root == null)
            return null;
        if(root.left == null && root.right == null)
            return root;
        //左子树构造成双链表，并返回链表头节点
        TreeNode left = Convert(root.left);
        TreeNode p = left;
        //定位到左子树双链表最后一个节点
        while(p != null && p.right != null)
        {
            p = p.right;
        }
        //左子树链表不为空，将当前root追加到左子树链表
        if(left != null)
        {
            p.right = root;
            root.left = p;
        }
        //右子树构造为双链表，返回链表头节点
        TreeNode right = Convert(root.right);
        //如果右子树链表不为空，将该链表追加到root节点之后
        if(right != null)
        {
            right.left = root;
            root.right = right;
        }
        return left != null?left:root;
    }
}
~~~

### 38.序列化二叉树

请实现两个函数，分别用来序列化和反序列化二叉树

~~~java
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public int index = -1;
    String Serialize(TreeNode root)
    {
        StringBuffer sb = new StringBuffer();
        if(root == null)
        {
            sb.append("#,");
            return sb.toString();
        }
        sb.append(root.val+",");
        sb.append(Serialize(root.left));
        sb.append(Serialize(root.right));
        return sb.toString();
    }
    TreeNode Deserialize(String str) 
    {
        index++;
        int len = str.length();
        if(index >= len)
        {
            return null;
        }
        String[] strr = str.split(",");
        TreeNode node = null;
        if(!strr[index].equals("#"))
        {
            node = new TreeNode(Integer.valueOf(strr[index]));
            node.left = Deserialize(str);
            node.right = Deserialize(str);
        }
       return node;
    }
}
~~~

### 39.字符串的排列

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

~~~java
import java.util.*;
/**
固定第一个字符，递归取得首位后面的各种字符串组合；
再把第一个字符与后面每一个字符交换，并同样递归获得首位后面的字符串组合； 
递归的出口，就是只剩一个字符的时候，递归的循环过程，就是从每个子串的第二个字符开始依次与第一个字符交换，然后继续处理子串。

* 假如有重复值呢？
* *由于全排列就是从第一个数字起，每个数分别与它后面的数字交换，我们先尝试加个这样的判断——如果一个数与后面的数字相同那么这两个数就不交换了。
* 例如abb，第一个数与后面两个数交换得bab，bba。然后abb中第二个数和第三个数相同，就不用交换了。
* 但是对bab，第二个数和第三个数不 同，则需要交换，得到bba。
* 由于这里的bba和开始第一个数与第三个数交换的结果相同了，因此这个方法不行。
    
* 换种思维，对abb，第一个数a与第二个数b交换得到bab，然后考虑第一个数与第三个数交换，此时由于第三个数等于第二个数，
* 所以第一个数就不再用与第三个数交换了。再考虑bab，它的第二个数与第三个数交换可以解决bba。此时全排列生成完毕！
  */
import java.util.ArrayList;
public class Solution {
    public ArrayList<String> Permutation(String str) 
    {
       ArrayList<String> list = new ArrayList<String>();
       if(str != null && str.length()>0)
       {
           Iteration(str.toCharArray(),0,list);
           Collections.sort(list);
       }
       return list;
    }
    
    public void Iteration(char[] chars, int i, ArrayList<String> list)
    {
        if(i == chars.length-1 )
        {
            list.add(String.valueOf(chars));
        }else{
            Set<Character> charSet = new HashSet<Character>();
            for(int j=i;j<chars.length;j++)
            {
                if(j==i || !charSet.contains(chars[j]))
                {
                    charSet.add(chars[j]);
                    swap(chars,i,j);
                    Iteration(chars,i+1,list);
                    swap(chars,i,j);
                }
            }
        }
    }
    
    private void swap(char[] cs,int i,int j)
    {
        char tmp = cs[i];
        cs[i] = cs[j];
        cs[j] = tmp;
    }
}
~~~



### 40.数组中出现次数超过一半的数字 

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。

~~~java
/**
基于快速排序，时间复杂度为o(n)的算法
排序后数组中间的数字一定是出现次数超过数组长度一半的数字
*/
public class Solution {
    private int Partition(int[] arr, int low,int high)
    {
        int pivot = arr[low];
        while(low< high)
        {
            while(low<high && arr[high]>= pivot)
                high--;
            arr[low] = arr[high];
            while(low<high && arr[low]<=pivot)
                ++low;
            arr[high] = arr[low];
        }
        arr[low] = pivot;
        return low;
    }
    
    public int MoreThanHalfNum_Solution(int [] array) {
        if(array == null || array.length == 0) return 0;
        if(array.length == 1 || array.length == 2) return array[0];
        int middle = array.length/2;
        int start = 0;
        int end = array.length-1;
        int index = Partition(array,start,end);
        while(index != middle)
        {
            if(index>middle)
            {
                end = index-1;
                index = Partition(array,start,end);
            }else{
                start = index+1;
                index = Partition(array,start,end);
            }
        }
        int result = array[middle];
        if(!CheckMoreThanHalf(array,array.length,result))
        {
            result = 0;
        }
        return result;
    }
    private boolean CheckMoreThanHalf(int[] array,int length,int number)
    {
        int count = 0;
        for(int i=0;i<length;i++)
        {
            if(array[i] == number)
            {
                count++;
            }
        }
        boolean flag = true;
        if(2*count<=length)
        {
            flag = false;
        }
        return flag;
    }
}
~~~



### 41.最小的K个数

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

~~~java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) 
    {
        ArrayList<Integer> result = new ArrayList<Integer>();
        int length = input.length;
        if(k>length || k==0)
        {
            return result;
        }
        Queue<Integer> maxHeap = new PriorityQueue<>(k, Collections.reverseOrder());
        for(int i=0;i<length;i++)
        {
            if(maxHeap.size() != k)
            {
                maxHeap.offer(input[i]);
            }else if(maxHeap.peek()>input[i]){
                Integer temp = maxHeap.poll();
                temp = null;
                maxHeap.offer(input[i]);
            }
        }
        for(Integer integer:maxHeap)
        {
            result.add(integer);
        }
        return result;
    }
}
~~~

### 42.数据流中的中位数


如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。我们使用Insert()方法读取数据流，使用GetMedian()方法获取当前读取数据的中位数。

~~~java
import java.util.*;
public class Solution {
    private PriorityQueue<Integer> smaller = new PriorityQueue<>(Collections.reverseOrder());
    private PriorityQueue<Integer> larger = new PriorityQueue<>();
    public void Insert(Integer num) 
    {
        smaller.add(num);
        larger.add(smaller.poll());
        if(larger.size() > smaller.size())
        {
            smaller.add(larger.poll());
        }
    }

    public Double GetMedian() 
    {
        if(smaller.size() == larger.size())
        {
            return ( (double)smaller.peek() + (double)larger.peek() )/2;
        }else{
            return (double)smaller.peek();
        }
        
    }
}
~~~

### 43.连续子数组的最大和

HZ偶尔会拿些专业问题来忽悠那些非计算机专业的同学。今天测试组开完会后,他又发话了:在古老的一维模式识别中,常常需要计算连续子向量的最大和,当向量全为正数的时候,问题很好解决。但是,如果向量中包含负数,是否应该包含某个负数,并期望旁边的正数会弥补它呢？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。给一个数组，返回它的最大连续子序列的和，你会不会被他忽悠住？(子向量的长度至少是1)

~~~java
public class Solution {
    public int FindGreatestSumOfSubArray(int[] array) 
    {
        if(array == null || array.length <0 )
            return -1;
        int nCurSum = 0;
        int nGreatestSum = 0x80000000;
        for(int i=0;i<array.length;i++)
        {
            if(nCurSum <= 0)
                nCurSum = array[i];
            else
                nCurSum += array[i];
            if(nCurSum > nGreatestSum)
                nGreatestSum =  nCurSum;
        }
        return nGreatestSum;
    }
}
~~~



### 44.整数中1出现的次数（从1到n整数中1出现的次数）

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。

暴力解：

~~~java
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) 
    {
        if(n<1)
            return 0;
        int count =0;
        for(int i=1;i<=n;i++)
        {
            int num =i;
            while(num!=0)
            {
                if(num%10 == 1)
                    count++;
                num = num/10;
            }
        }
        return count;
    }
}
~~~

数学解法：

```java
//数学解法求,分别别每个个位，百位...上1的个数
public class Solution {
    public int NumberOf1Between1AndN_Solution(int n) 
    {
        if(n<1)
            return 0;
        long count = 0, factor=1;
        while(n/factor != 0)
        {
            long c = (n/factor)%10;
            long high = n/(10*factor);
            long low = n%factor;
            if(c==0)
            {
                count += high*factor;
            }else if(c == 1)
            {
                count += high*factor + low +1;
            }else{
                count += (high+1)*factor;
            }
            factor = factor*10;
        }
        return (int)count;
    }
}
```



### 45.把数组排成最小的数

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

~~~java
// 是大数问题
// 自定义一个比较大小的函数，比较两个字符串s1, s2大小的时候，先将它们拼接起来，比较s1+s2,和s2+s1那个大，如果s1+s2大，那说明s2应该放前面，所以按这个规则，s2就应该排在s1前面
import java.util.ArrayList;

public class Solution {
    public String PrintMinNumber(int [] numbers) {
        String str = "";
        for(int i=0;i<numbers.length;i++)
        {
            for(int j=i+1;j<numbers.length;j++)
            {
                int a = Integer.valueOf(numbers[i]+""+numbers[j]);
                int b = Integer.valueOf(numbers[j]+""+numbers[i]);
                if(a>b)
                {
                    int t = numbers[i];
                    numbers[i] = numbers[j];
                    numbers[j] = t;
                }
            }
        }
        for(int i=0;i<numbers.length;i++)
        {
            str+=String.valueOf(numbers[i]);
        }
        return str;
    }
}
~~~

## 动态规划

### 46.把数字翻译成字符串

给定一个数字，按照如下规则翻译成字符串：0翻译成“a”，1翻译成“b”...25翻译成“z”。一个数字有多种翻译可能，例如12258一共有5种，分别是bccfi，bwfi，bczi，mcfi，mzi。实现一个函数，用来计算一个数字有多少种不同的翻译方法。

~~~java
/**
 * 动态规划 f(r)表示从r开始到最右端所组成的数字能够翻译成字符串的种数
 * 对于长度为n的数字，f(n)=0,f(n-1)=1,求f(0)。
 * 递推公式为 f(r-2) = f(r-1)+g(r-2,r-1)*f(r)；
 * 其中，如果r-2，r-1能够翻译成字符，则g(r-2,r-1)=1，否则为0。
 * 因此，对于12258：
 * f(5) = 0
 * f(4) = 1
 * f(3) = f(4)+0 = 1
 * f(2) = f(3)+f(4) = 2
 * f(1) = f(2)+f(3) = 3
 * f(0) = f(1)+f(2) = 5
 * */
public class Solution
{
    public static int getTranslationCount(int number)
    {
        if(number < 0) return 0;
        if(number == 1) return 1;
        return getTranslationCount(Integer.toString(number));
    }

    public static int getTranslationCount(String number)
    {
        int f1 = 0, f2 = 1, g = 0;
        int temp;
        for(int i=number.length()-2;i>=0;i--)
        {
            if(Integer.parseInt(number.charAt(i)+""+number.charAt(i+1))<26)
                g = 1;
            else
                g = 0;
            temp = f2;
            f2 = f2 + g*f1;
            f1 = temp;
        }
        return f2;
    }
}
~~~



### 47.礼物的最大值

在一个m*n的棋盘的每一个格都放有一个礼物，每个礼物都有一定价值（大于0）。从左上角开始拿礼物，每次向右或向下移动一格，直到右下角结束。给定一个棋盘，求拿到礼物的最大价值。

~~~java
/**
 * 动态规划
 * 使用二维辅助数组
 * f(i,j)表示到达坐标为(i,j)的格子时能拿到的礼物总和的最大值
 * f(i,j) = max(f(i-1,j), f(i,j-1)) + gift[i,j]
 * */
public class Solution
{
    public static int getMax(int[][] arr)
    {
        if(arr.length == 0 || arr[0].length == 0)
            return -1;
        int rows = arr.length;
        int cols = arr[0].length;

        int[][] maxValue = new int[rows][cols];

        for(int i=0;i<rows;i++)
        {
            for(int j=0;j<cols;j++)
            {
                int left = 0;
                int up = 0;
                if(i>0)
                {
                    up = maxValue[i-1][j];
                }
                if(j>0)
                {
                    left = maxValue[i][j-1];
                }
                maxValue[i][j] = Math.max(up,left)+arr[i][j];
            }
        }
        return maxValue[rows-1][cols-1];
    }

    public static void main(String[] args) {
        int[][] arr = { { 1, 10, 3, 8 }, { 12, 2, 9, 6 }, { 5, 7, 4, 11 },
                { 3, 7, 16, 6 } };
        System.out.println(getMax(arr));
    }
}
~~~

### 48.最长不含重复字符的子字符串

从给定的字符串中找到最长不含重复字符的子字符串，返回该子字符串的长度。假设字符中只有‘a-z’.例如“asadsfgdf”的最长子字符串是“adsfg ”应该返回 5。

~~~java
/**
 * 动态规划
 * 设到第i个字符的最长不重复子字符串长度为f(i)
 * (1) 如果第i个字符之前没出现过; f(i) = f(i-1)+1;
 * (2) 出现过分为以下情况
 *     a. 第i个字符和上次出现的位置的距离d 小于或等于 目前不重复子字符串f(i-1)时，
 *         f(i)=d,意味着第i个字符出现两次所夹的字符中没有重复的字符
 *     b. 当d大于f(i-1)的时候，说明第i个字符上一次出现在最长子字符串之前，f(i)=f(i-1)+1
 * */
public class Solution
{
    public static int longestSubString(String str)
    {
        if(str == null) return 0;
        if(str.length() == 1 && str.charAt(0) >= 'a' && str.charAt(0) <= 'z')
            return 1;
        int maxLength = 0;
        int currentLength = 0;
        int[] position = new int[26];
        for(int i=0;i<position.length;i++)
        {
            position[i] = -1;
        }
        for(int i=0;i<str.length();i++)
        {
            if(!(str.charAt(i) >= 'a' && str.charAt(i) <= 'z'))
            {
                return 0;
            }
            int prevIndex = position[str.charAt(i)-'a'];
            position[str.charAt(i)-'a'] = i;
            if(prevIndex < 0 || i-prevIndex>currentLength)
                currentLength++;
            else
                currentLength = i-prevIndex;
            if(currentLength > maxLength)
                maxLength = currentLength;

        }
        return maxLength;
    }

    public static void main(String[] args) {
        String str = "sdsadasdsfcds";
        System.out.println(longestSubString(str));
    }

}
~~~

### 49.丑数

把只包含质因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含质因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。

~~~java
import java.util.*;
// 创建一个数组，
// 创建三个队列，丑数乘以2，3，5分别存入三个队列
// 令x为三个队列的最小值，将x添加至数组的尾部
// 若x存在于：
//     Q2：将 x * 2、x * 3、x*5 分别放入Q2 Q3 Q5，从Q2中移除x
//     Q3：将 x * 3、x*5 分别放入Q3 Q5，从Q3中移除x
//     Q5：将 x * 5放入Q5，从Q5中移除x
public class Solution {
    public int GetUglyNumber_Solution(int index) 
    {
       if(index == 0) return 0;
        int n =1, ugly = 1,min;
        Queue<Integer> q2 = new LinkedList<Integer>();
        Queue<Integer> q3 = new LinkedList<Integer>();
        Queue<Integer> q5 = new LinkedList<Integer>();
        q2.add(2);
        q3.add(3);
        q5.add(5);
        while(n != index)
        {
            ugly = Math.min(q2.peek(),Math.min(q3.peek(),q5.peek()));
            if(ugly == q2.peek())
            {
                q2.add(ugly*2);
                q3.add(ugly*3);
                q5.add(ugly*5);
                q2.poll();
            }
            if(ugly == q3.peek())
            {
                q3.add(ugly*3);
                q5.add(ugly*5);
                q3.poll();
            }
            if(ugly == q5.peek())
            {
                q5.add(ugly*5);
                q5.poll();
            }
            n++;
        }
        return ugly;
    }
}
~~~

### 50.第一个只出现一次的字符

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

~~~java
// 从头开始扫描每个字符出现的次数，并存入hash表中
// 第二次扫描的时候就可以知道每个字符出现的次数
import java.util.*;
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        HashMap<Character,Integer> map = new HashMap<Character,Integer>();
        for(int i=0;i<str.length();i++)
        {
            if(map.containsKey(str.charAt(i)))
            {
                int time = map.get(str.charAt(i));
                map.put(str.charAt(i),++time);
            }else{
                map.put(str.charAt(i),1);
            }
        }
        for(int j = 0;j<str.length();j++)
        {
            char c = str.charAt(j);
            if(map.get(c) == 1)
            {
                return j;
            }
        }
        return -1;
    }
}
~~~

### 51.字符流中第一个不重复的字符

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

~~~java
// 定义一个数据容器来保存字符在字符流中的位置
// 当一个字符第一次出现，把它在字符流中的位置保存到数据容器
// 当字符再次出现，字符重复，可以将其对应位置值设为负数，抛弃
// 再用Arrayist记录输入流，每次返回第一个出现一次的字符都在Arrayist中找，有序的，直接找key的话会乱掉
import java.util.*;
public class Solution {
    HashMap<Character,Integer> map = new HashMap();
    ArrayList<Character> list = new ArrayList<Character>();
    //Insert one char from stringstream
    public void Insert(char ch)
    {
        if(map.containsKey(ch))
            map.put(ch,-10);
        else
            map.put(ch,1);
        list.add(ch);
    }
  //return the first appearence once char in current stringstream
    public char FirstAppearingOnce()
    {
        char res = '#';
        for(char key:list)
        {
            if(map.get(key) == 1)
            {
                res = key;
                break;
            }
        }
        return res;
    }
}
~~~





### 52.数组中的逆序对 不会啊

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

~~~java
/*归并排序的改进，把数据分成前后两个数组(递归分到每个数组仅有一个数据项)，
合并数组，合并时，出现前面的数组值array[i]大于后面数组值array[j]时；则前面
数组array[i]~array[mid]都是大于array[j]的，count += mid+1 - i
参考剑指Offer，但是感觉剑指Offer归并过程少了一步拷贝过程。
还有就是测试用例输出结果比较大，对每次返回的count mod(1000000007)求余
*/
 
public class Solution {
    public int InversePairs(int [] array) {
        if(array==null||array.length==0)
        {
            return 0;
        }
        int[] copy = new int[array.length];
        for(int i=0;i<array.length;i++) //复制数组
        {
            copy[i] = array[i];
        }
        int count = InversePairsCore(array,copy,0,array.length-1);
        return count;
         
    }
    private int InversePairsCore(int[] array,int[] copy,int low,int high)
    {
        if(low==high)
        {
            return 0;
        }
        int mid = (low+high)>>1;
        int leftCount = InversePairsCore(array,copy,low,mid)%1000000007;
        int rightCount = InversePairsCore(array,copy,mid+1,high)%1000000007;
        int count = 0;
        int i=mid;
        int j=high;
        int locCopy = high;
        while(i>=low&&j>mid)
        {
            if(array[i]>array[j])
            {
                count += j-mid;
                copy[locCopy--] = array[i--];
                if(count>=1000000007)//数值过大求余
                {
                    count%=1000000007;
                }
            }
            else
            {
                copy[locCopy--] = array[j--];
            }
        }
        for(;i>=low;i--)
        {
            copy[locCopy--]=array[i];
        }
        for(;j>mid;j--)
        {
            copy[locCopy--]=array[j];
        }
        for(int s=low;s<=high;s++)
        {
            array[s] = copy[s];
        }
        return (leftCount+rightCount+count)%1000000007;
    }
}
~~~

### 53.两个链表的第一个公共结点

输入两个链表，找出它们的第一个公共结点。

解法1 暴力求解

~~~java
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) 
    {
        if(pHead1 == null || pHead2 == null)
            return null;
        ListNode current1 = pHead1;
        ListNode current2 = pHead2;
        ListNode resultNode = null;
        while(current1 != null)
        {
            current2 = pHead2;
            while(current2 != null)
            {
                if(current1 != current2)
                {
                    current2 = current2.next;
                }else{
                    resultNode = current1;
                    break;
                }
            }
            if(resultNode != null)
                break;
            current1= current1.next;
        }
        return resultNode;
    }
}
~~~

方法2 根据链表的结构来确定解法

~~~java
//两个链表只可能是Y型结构
// 我们想同时遍历达到两个栈的尾节点
// 我们可以首先遍历两个链表得到他们的长度，可以得到长链表比短链表长多少；
// 第二次遍历时，在长链表上先走若干步，接着同时在两个链表上遍历，找到的第一个相同的节点就是第一个公共节点
/*
public class ListNode {
    int val;
    ListNode next = null;

    ListNode(int val) {
        this.val = val;
    }
}*/
public class Solution {
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) 
    {
        ListNode current1 = pHead1;
        ListNode current2 = pHead2;
        if(pHead1 == null || pHead2 == null)
            return null;
        int length1 = getLength(current1);
        int length2 = getLength(current2);
        if(length1 >= length2 )
        {
            int len = length1 - length2;
            while(len >0){
                current1 = current1.next;
                len--;
            }
        }else{
            int len = length2 - length1;
            while(len >0){
                current2 = current2.next;
                len--;
            }
        }
        while(current1 != current2)
        {
            current1 = current1.next;
            current2 = current2.next;
        }
        return current1 == null ? null:current1;
    }
    
    public static int getLength(ListNode pHead)
    {
        int length = 0;
        ListNode current = pHead;
        while(current != null)
        {
            length++;
            current = current.next;
        }
        return length;
    }
}
~~~



### 54.数字在排序数组中出现的次数

统计一个数字在排序数组中出现的次数。

~~~java
// 用二分法分别查找第一个和最后一个k出现的位置
// 找第一个k的过程：
//     二分找到k；如果k前面的数字不是k，则此时就是k出现的第一个位置；如果是k，那么第一个k在其前面
public class Solution {
    public int GetNumberOfK(int [] array , int k) 
    {
        if(array.length == 0) return 0;
       int number = 0;
        if(array != null && array.length >0)
        {
            int first = getFirst(array,k,0,array.length-1);
            int end = getEnd(array,k,0,array.length-1);
            if(first != -1 && end != -1)
                number = end - first+1;
        }
        return number;
    }
    
    public int getFirst(int[] array,int k, int start,int end)
    {
        if(start > end)
            return -1;
        int mid = (start+end)/2;
        int midData = array[mid];
        if(midData == k)
        {
            if((mid >0 && array[mid-1]!=k) || mid == 0)
                return mid;
            else
                end = mid -1;
        }else if(midData > k){
            end = mid -1;
        }else if(midData < k){
            start = mid+1;
        }
        return getFirst(array,k,start,end);
    }
    
    public int getEnd(int[] array,int k, int start,int end)
    {
        if(start > end)
            return -1;
        int mid = (start+end)/2;
        int midData = array[mid];
        if(midData == k)
        {
            if((mid < array.length-1 && array[mid+1]!=k) || mid == array.length-1)
                return mid;
            else
                start = mid +1;
        }else if(midData > k){
            end = mid -1;
        }else if(midData < k){
            start = mid+1;
        }
        return getEnd(array,k,start,end);
    }
}
~~~



### 55.0~n-1中缺失的数字

一个长度为 n-1 的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围 0~n-1 之内。在范围 0~n-1 内的 n 个数字中有且只有一个数字不在该数组中，请找出这个数字。

~~~java
/**
 * 使用二分法找出第一个数字不等于下标的数字，其下标就是问题的解
 * 如果中间数字等于下标，目标在右边；
 * 如果中间数字和下标不相等 且 其前面的数字和下标也不想等，目标在左边
 * 如果中间数字和下标不相等 且 其前面的数字和下标想等，get
 * */
public class Solution
{
    public static int getMissingNumber(int[] numbers)
    {
        if(numbers.length == 0 || numbers == null)
            return -1;
        int left = 0;
        int right = numbers.length-1;

        while(left <= right)
        {
            int mid = (left+right)/2;
            if(mid == numbers[mid])
            {
                left = mid+1;
            }else if(mid == 0 || (mid != numbers[mid] && (mid-1) == numbers[mid-1])) //注意以下第一个数字就不满足的情况，mid == 0必须写在前面，不然下标越界
            {
                return mid;
            }else
            {
                right = mid -1;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] numbers= {1,2};
        System.out.println(getMissingNumber(numbers));
    }
}

~~~



### 56.数组中数值和下标相等

假设一个单调递增的数组里的每个元素都是整数并且是唯一的。请编程实现一个函数，找出数组中任意一个数值等于其下标的元素。例如，在数组{-3，-1,1,3,5}中，数字3和它的下标相等。

~~~java
/**
 * 使用二分法找出数组中任意一个数等于下标的元素
 * 数组值 numbers[mid] , 下标 mid
 * 如果numbers[mid]  == mid，get
 * numbers[mid] > mid,因为数组递增，找左边
 * numbers[mid] < mid,因为数组递增，找右边
 * */
public class Solution
{
    public static int findEqualNUmber(int[] numbers)
    {
        if(numbers.length == 0 || numbers == null)
            return -1;
        int left = 0;
        int right = numbers.length-1;
        while(left <= right)
        {
            int mid = (left+right)/2;
            if(numbers[mid] > mid)
            {
                right = mid-1;
            }else if(numbers[mid] < mid)
            {
                left = mid + 1;
            }else{
                return mid;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {-3,-1,1,3,5};
        System.out.println(findEqualNUmber(arr));

    }
}

~~~





### 57.二叉搜索树的第k个结点

给定一棵二叉搜索树，请找出其中的第k小的结点。例如， （5，3，7，2，4，6，8）    中，按结点数值大小顺序第三小结点的值为4。

~~~java
// 中序遍历二叉搜索树得到的遍历序列的数值就是递增的
/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
import java.util.*;
public class Solution {
    TreeNode KthNode(TreeNode root, int k)
    {
        if(root == null || k ==0)
            return null;
        Stack<TreeNode> stack = new Stack<TreeNode>();
        int count =0;
        while(!stack.isEmpty() || root != null)
        {
            if(root != null)
            {
                stack.push(root);
                root = root.left;
            }else{
                root = stack.pop();
                count++;
                if(count == k)
                    return root;
                root = root.right;
            }
        }
        return null;
    }
}
~~~



### 58.二叉树的深度

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

~~~java
// 如果根节点只有左子树没有右子树，树的深度为左子树深度加1 （右子树同理）
// 如果左右子树都有，树的深度为左右子树深度的较大值加1
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;
    }
}
*/
public class Solution {
    public int TreeDepth(TreeNode root)
    {
        if(root == null)
            return 0;
        int left = TreeDepth(root.left);
        int right = TreeDepth(root.right);
        return (left > right)?(left+1):(right+1);
    }
}
~~~



### 59.平衡二叉树

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

~~~java
// 用后序遍历二叉树的每一个节点
// 遍历时记录节点的深度，某一节点的深度等于它到叶节点的路径的长度
public class Solution {
    private boolean isBalanced = true;
    public boolean IsBalanced_Solution(TreeNode root) 
    {
        getDepth(root);
        return isBalanced;
    }
    public int getDepth(TreeNode root)
    {
        if(root == null)
            return 0;
        int left = getDepth(root.left);
        int right = getDepth(root.right);
        if(Math.abs(left-right)>1)
        {
            isBalanced = false;
        }
        return right > left?right+1:left+1;
    }
}
~~~



### 60.数组中只出现一次的两个数字

一个整型数组里除了两个数字之外，其他的数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度O(n)，空间复杂度O(1)。

~~~java
/**
 * 数组中只出现一次的数字可以用异或解决
 * 从头到尾异或数组中的每个数字得到两个只出现一次的数字的异或结果
 * 异或结果中至少有一位为1，找到结果中第一个为1的位置，以第n位是不是1把原数组分为两个子数组
 * 于是就可以得到两个数组，每个数组中包含一个只出现一次的数字
 * */
public class Solution
{
    public static int[] findTwoNumbers(int[] numbers)
    {
        int result = 0;
        for(int i=0;i<numbers.length;i++)
        {
            result ^= numbers[i];
        }
        int indexOf1 = findFirstBit(result);
        int[] res = new int[]{0,0};
        if(indexOf1 < 0 )
            return res;
        for(int i=0;i<numbers.length;i++)
        {
            if((numbers[i] & indexOf1) == 0)
                res[0] ^= numbers[i];
            else
                res[1] ^= numbers[i];
        }
        return res;
    }

    public static int findFirstBit(int num)
    {
        if(num < 0)
            return -1;
        int indexOf1 = 1;
        while(num != 0){
            if((num & 1) == 1)
                return indexOf1;
            else{
                num = num>>1;
                indexOf1 *=2;
            }
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {1,2,3,3,4,4,1,7};
        int[] res = findTwoNumbers(arr);
        System.out.println(res[0]+"  "+res[1]);
    }
}

~~~

### 61.数组中唯一只出现一次的数字（其余出现三次）

~~~java
/**
 * 把数组中所有数字的二进制表示的每一位都加起来
 * 如果某一位的和能被3整除，那么那个只出现1次的数字的二进制表示的对应位为0，否则是1
 * */
public class Solution
{
    public static int findOnceNumbers(int[] numbers)
    {
        if(numbers == null || numbers.length <=0)
        {
            return -1;
        }
        int[] bitSum = new int[32];
        //初始化数组全为0
        for(int i=0;i<bitSum.length;i++)
            bitSum[i] = 0;



        for(int i=0;i<numbers.length;i++)
        {
            int bitMask = 1;
            for(int j =31;j>=0;j--)
            {
                int bit = numbers[i] & bitMask;
                if(bit != 0)
                    bitSum[j] += 1;
                bitMask = bitMask <<1;
            }
        }

        int result =0;
        for(int i=0;i<32;i++)
        {
            result = result<<1;
            result += bitSum[i]%3;
        }
        return result;
    }

    public static void main(String[] args) {
        int[] a = {9};
        System.out.println(findOnceNumbers(a));
    }
}

~~~



### 62.和为S的两个数字

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

~~~java
// 在数组中选择两个数字，如果和小于s，可以考虑选择较小数字后面的数字
// 如果和大于s，可以选择较大数字前面的数字
// ---> 采用左右夹逼的方法求出来
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) 
    {
       ArrayList<Integer> res = new ArrayList<Integer>();
        if(array == null || array.length <2)
            return res;
        int i=0,j=array.length-1;
        while(i<j)
        {
            if(array[i] + array[j] == sum)
            {
                res.add(array[i]);
                res.add(array[j]);
                return res;
            }else if(array[i] + array[j] > sum){
                j--;
            }else{
                i++;
            }
        }
        return res;
    }
}
~~~



### 63.和为S的连续正数序列

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck!

~~~java
// 用两个数small和big分别表示最小和最大值，small-->1,big-->2
// 如果和大于s，增大small的值
// 如果和小于s，增大big的值
// 序列至少要两个数字，一直增加到small到(s+1)/2为止
import java.util.ArrayList;
public class Solution {
    ArrayList<ArrayList<Integer> > res = new ArrayList<ArrayList<Integer> >();
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
       if(sum<3)
           return res;
        int small = 1;
        int big = 2;
        int mid =(1+sum)/2;
        int curSum = small+big;
        
        while(small<mid)
        {
            if(curSum == sum)
                addResult(small,big);
            
            while(curSum>sum && small<mid)
            {
                curSum -=small;
                small++;
                if(curSum == sum)
                    addResult(small,big);
            }
            big++;
            curSum += big;
        }
        return res;
    }
    
    public void addResult(int small,int big)
    {
        ArrayList<Integer> subRes = new ArrayList<Integer>();
        for(int i=small;i<=big;i++)
        {
             subRes.add(i);
        }
        res.add(subRes);
    }
}
~~~





### 64.翻转单词顺序列

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

方法1. 翻转两次的方法

~~~
public class Solution {
    public String ReverseSentence(String str) 
    {
        if(str==null||str.length()==0||str.trim().length()==0)
            return str;
        String str1 = reverseWord(str);
        String[] strarr = str1.split(" ");
        for(int i=0;i<strarr.length;i++)
        {
            strarr[i] = reverseWord(strarr[i]);
        }
        String str2="";
        for(int i=0;i<strarr.length;i++)
        {
            str2 += strarr[i]+" ";
        }
        return str2.trim();
    }
    
    public String reverseWord(String str)
    {
        char[] res = str.toCharArray();
        int begin = 0;
        int end = res.length-1;
        while(begin<end)
        {
            char temp = res[begin];
            res[begin] = res[end];
            res[end] = temp;
            begin++;
            end--;
        }
        String result= "";
        for(int i=0;i<res.length;i++)
        {
            result += res[i];
        }
        return result;
    }
        
}
~~~

方法2 借用辅助数组 

~~~java
public class Solution {
    public String ReverseSentence(String str) 
    {
        if(str==null||str.length()==0||str.trim().length()==0)
            return str;
        StringBuffer sb = new StringBuffer();
        String[] s = str.trim().split(" ");
        for(int i=s.length-1;i>=0;i--)
        {
            if(s[i] != "")
                sb.append(s[i]);
            if(i >= 1)
                sb.append(" ");
        }
        return sb.toString();
    }
}
~~~





### 65.左旋转字符串

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

~~~java
public class Solution {
    public String LeftRotateString(String str,int n)
    {
        if(str == null || str.length() ==0 ||str.trim().length()==0)
            return str;
        StringBuffer sb =new StringBuffer();
        char[] arr = str.toCharArray();
        for(int i = n; i < arr.length ; i++)
        {
            sb.append(arr[i]);
        }
        for(int i=0;i<n;i++)
        {
            sb.append(arr[i]);
        }
        return sb.toString();
    }
}
~~~



### 66.滑动窗口的最大值 

给定一个数组和滑动窗口的大小，找出所有滑动窗口里数值的最大值。例如，如果输入数组{2,3,4,2,6,2,5,1}及滑动窗口的大小3，那么一共存在6个滑动窗口，他们的最大值分别为{4,4,6,6,6,5}； 针对数组{2,3,4,2,6,2,5,1}的滑动窗口有以下6个： {[2,3,4],2,6,2,5,1}， {2,[3,4,2],6,2,5,1}， {2,3,[4,2,6],2,5,1}， {2,3,4,[2,6,2],5,1}， {2,3,4,2,[6,2,5],1}， {2,3,4,2,6,[2,5,1]}。

~~~java
// 把有可能成为最大值的存入双端队列
// 当窗口滑动一次：（1） 判断当前最大值是否过期
//              （2） 新增加的值从队尾开始比较，把所有比它小的值丢掉
import java.util.*;
public class Solution {
    public ArrayList<Integer> maxInWindows(int [] num, int size)
    {
        ArrayList<Integer> res = new ArrayList<>();
        if(size == 0) return res;
        int begin;
        ArrayDeque<Integer> q = new ArrayDeque<>();
        for(int i=0;i<num.length;i++)
        {
            begin = i-size+1;
            if(q.isEmpty())  //peek是获取，poll是删除，add是在队列最后增加
                q.add(i);
            else if(begin > q.peekFirst())
                q.pollFirst();
            
            while((!q.isEmpty()) && num[q.peekLast()]<=num[i])
                q.pollLast();
            q.add(i);
            if(begin >= 0)
                res.add(num[q.peekFirst()]);
        }
        return res;
    }
}
~~~



### 

定义一个队列并实现函数max得到队列里的最大值。要求max，pushBack，popFront的时间复杂度都是o(1)。

~~~

~~~





### 68.n个骰子的点数 不会

把 n 个骰子扔在地上，所有骰子朝上一面的点数之和为 s。输入 n，打印出 s 的所有可能的值出现的概率。















































# 算法基础班的课后习题

### 1.进制转换

  给定一个十进制数M，以及需要转换的进制数N。将十进制数M转化为N进制数 

~~~java
/** 
1.十进制数为负数时，这个时候就需要将十进制数转换成一个整数进行转换，只不过在最后输出的时候在结果加上一个负号就好了
2.输出的值不止是阿拉伯数字，还有A-F，可以使用一个数组保存，对余数进行判断就好了；
3.辗转相除得到的结果最后输出需要反序，因为计算的结果是反的；（我是保存在字符串的情况）
*/
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
 
public class Main {
    //进制转换
    public static void main(String[] args) throws IOException {
        //输入一行两个数，第一个是M，第二个是N
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] nums = br.readLine().split(" ");
        int m = Integer.valueOf(nums[0]);
        int n = Integer.valueOf(nums[1]);
        StringBuffer sb = new StringBuffer();
        char[] arr = {'A','B','C','D','E','F'};
        int temp = 0;
        boolean fs = false;
        if(m < 0){
            fs = true;
            m = -m;
        }
        //辗转相除法
        while(m != 0){
            temp = m%n;
            if(temp > 9)
                sb.append(arr[temp-9-1]);
            else
                sb.append(temp);
            m = m/n;
        }
        if(fs)
            sb.append("-");
        System.out.println(sb.reverse().toString());
    }
}
~~~

### 2.末尾0的个数

输入一个正整数n,求n!(即阶乘)末尾有多少个0？ 比如: n = 10; n! = 3628800,所以答案为2 

  ~~~~java
import java.math.BigInteger;
import java.util.Scanner;
public class Main{
    public static void main(String []args) {
        Scanner sc = new Scanner(System.in);
        int n=sc.nextInt();
        BigInteger result = new BigInteger("1");
        for(int i=n;i>0;i--){
            BigInteger num=new BigInteger(String.valueOf(i));
            result=result.multiply(num);
        }
        StringBuffer sb=new StringBuffer(String.valueOf(result));
        sb.reverse();
        int count=0;
        while(sb.substring(count,sb.length()).startsWith("0")){
            count++;
        }
        System.out.println(count);
    }
}

  ~~~~

