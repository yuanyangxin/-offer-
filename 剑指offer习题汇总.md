# **剑指offer习题汇总**

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
    public boolean Find(int target, int [][] array) 
    {
        int row = array.length;
        int col = array[0].length;
        if(row == 0 || col == 0 || array == null) return false;
        int i = 0;// 初始行号
        int j = col -1; //初始列号
        while(i<row && j<col && i>=0 && j>=0)
        {
            if(array[i][j] > target){
                j--;
            }else if(array[i][j] < target){
                i++;
            }else{
                return true;
            }
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
        root.left = root.right;
        root.right = newNode;
        
        if(root.left!=null)
            Mirror(root.left); 
        if(root.right!=null)
            Mirror(root.right); 
    }
}
~~~





























































































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

