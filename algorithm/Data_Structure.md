



### 栈

存放方式：LIFO，先进后出







### 搜索 

线性搜索；

二分搜索；(要求序列有序)





#### 线性搜索

也叫顺序查找(linear search),按照顺序从头到尾逐一搜索。





#### 二叉搜索树（二叉树）

​	时间复杂度 O(h)  h为深度

所以跟树的结构有关：

![image-20211128194939557](Data_Structure.assets/image-20211128194939557.png)

所以要平衡二叉树：对于任意一个子结点|Height(LeftTree) - Height(RightTree)| <=1

[Tree](Tree.md)

==优化点==

1.找出mid，该索引为mid =（left + right）/ 2，但是这样写有可能溢出（==超出int的范围==），所以我们需要改进一下写成mid = left +（right - left）/ 2 或者  left + ((right - left ) >> 1)  两者作用是一样的，都是为了找到两指针的中间索引，使用位运算的速度更快。那么此时的 mid = 0 + (8-0) / 2 = 4



##### 递归写法

![图片](https://mmbiz.qpic.cn/mmbiz_png/ClAkUIOhotV55pxribicAdnmrLb0lthhrVy3Hm24rlNNOReYX9ZXBkHUWaKg79R3yJtQ5SVaQzmCOGNQhxRYeX1Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)







##### LeetCode34.在排序数组中查找元素的第一个和最后一个位置

> 给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。
>
> 如果数组中不存在目标值 target，返回 [-1, -1]。

示例 1：

> 输入：nums = [5,7,7,8,8,10], target = 8 输出：[3,4]

示例 2：

> 输入：nums = [5,7,7,8,8,10], target = 6 输出：[-1,-1]

示例 3：

> 输入：nums = [], target = 0 输出：[-1,-1]

例题中使用的是单独分别求上下界的形式，

```java
public class LeetCode34 {
    static int[] f(int[] a,int left,int right,int target){
        int l1,l2;
        int mid=0;
        while(left<=right){
            mid = left + ((right - left)>>1);
            //分别求出中街店的左右点
            if(a[mid] == target){
                l1=mid;
                l2=mid;
                while(a[l1-1]==target)
                    l1--;
                while(a[l2+1]==target)
                    l2++;
                return new int[]{l1,l2};
            }else if(a[mid]>target){
                right = mid-1;
            }else if(a[mid]<target){
                left = mid+1;
            }
        }

        return new int[]{-1, -1};
    }

    public static void main(String[] args) {
        int[] arr = { 5,7,7,8,8,10};
        System.out.println(Arrays.toString(f(arr,0,arr.length-1,6)));

    }
}
```



##### LeetCode35.搜索插入位置

> 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
>
> 你可以假设数组中无重复元素。

示例 1:

> 输入: [1,3,5,6], 5 输出: 2

示例 2:

> 输入: [1,3,5,6], 2 输出: 1

示例 3:

> 输入: [1,3,5,6], 7 输出: 4

示例 4:

> 输入: [1,3,5,6], 0 输出: 0

实际上跟前面的二分搜索是一样的，只不过当查询不到的时候我们要返回left；

```java
public class LeetCode35 {
    static int f(int[] a,int left,int right,int target){
        int mid=0;
        while(left<=right){
            mid = left + ((right - left)>>1);
            if(a[mid] == target){
                return mid;
            }else if(a[mid]>target){
                right = mid-1;
            }else if(a[mid]<target){
                left = mid+1;
            }
        }

        return left;
    }

    public static void main(String[] args) {
        int[] arr = {1,3,5,6};
        System.out.println(f(arr,0,arr.length-1,2));

    }
}
```









##### 详解：

​	https://mp.weixin.qq.com/s/1ExIav9uK4bvVnnf4t0H2Q

