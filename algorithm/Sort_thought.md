

## 算法思想



### 递归

==两个特点==

1.调用自身

2.结束语句



#### 阶乘：

输入一个正整数n，输出n!的值。其中n!=1*2*3*…*n, 即求阶乘

```java
public int factorial(int n) {
    //临界条件
    if (n < =1) {
        return1;
    }

    // 递推公式
    return n * factorial(n-1)
}
```

#### 跳台阶

一只青蛙可以一次跳 1 级台阶或一次跳 2 级台阶,例如:跳上第 1 级台阶只有一种跳法：直接跳 1 级即可。跳上第 2 级台阶有两种跳法：每次跳 1 级，跳两次；或者一次跳 2 级。问要跳上第 n 级台阶有多少种跳法？

==如果，青蛙第一下跳一阶，则剩下跳法为f(n-1)；如果第一下为2，则剩下跳法为f(n-2)，所以可得f(n)=f(n-1)+f(n-2)==

```java
public int f(int n) {
    if (n == 1) return1;
    if (n == 2) return2;
    return f(n-1) + f(n-2)
}
```



#### 斐波那契数列：

```java
public int f(int n){
	if(n==1 || n==2){
        return 1;
    }else{
   		return f(n-1)+f(n-2);
    }
}


//动态规划解决
public int f(int target){
	if(target == 1){
        return 1;
    }if(target == 2){
        return 2;
    }
    int a = 1;
    int b = 1;
    for(int i=3;i<=target;i++){
    	sum = a + b;
        a = b;
        b = sum;
    }
    
}
```



#### 汉诺塔Hannota：

![image-20211123110402564](Sort_thought.assets/image-20211123110402564.png)

移动n个环到C时，需要先将n-1移动到B；

然后将第n个环移动到C；

以此类推。。。。

==规律为：== 一个环，2的1次方-1； 两个环卫2的2次方-1 步； 三个环为2的3次方-1 步；...n个环为2的n次方-1步。

```java
public static void move(int disk, char M, char N){
    
}

public static void hannoi(int n, char A, char B, char C){
    if(n==1){
        return 1;
    }else{
        return hannoi(n-1,A,C,B)+1+hannoi(n-1,B,A,C);
    }
    
}
```

#### ==进阶==

> 细胞分裂 有一个细胞 每一个小时分裂一次，一次分裂一个子细胞，第三个小时后会死亡。那么n个小时候有多少细胞？

![图片](https://mmbiz.qpic.cn/mmbiz_png/OyweysCSeLWvDS0Xny7l5kj0Nj4znUDibK3VJALJuo8IxWjDvPuAWxvO5dbBaLn01GcDnIqiaKXbSWRvwmyEM1uQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

如图所示，从第四个小时开始，我们可以得到一个方程f(n) =  fa(n)  + fb(n)  + fc(n) ；a代表第一个小时的细胞，b为第二个小时的细胞，c为第三个小时的细胞；

即可递推方程式为：

fa(n)  = fa(n-1) + fb(n-1) + fc(n-1) ， 当 n =1时，fa(1) = 1

fb(n) = fa(n -1) , 当n=1时，为0；

fc(n)  = fb(n-1) , 当n=1 或2 时，为0；

```java
public int allCells(int n) {
    return aCell(n) + bCell(n) + cCell(n);
}

/**
 * 第 n 小时 a 状态的细胞数
 */
public int aCell(int n) {
    if(n==1){
        return1;
    }else{
        return aCell(n-1)+bCell(n-1)+cCell(n-1);
    }
}

/**
 * 第 n 小时 b 状态的细胞数
 */
public int bCell(int n) {
    if(n==1){
        return0;
    }else{
        return aCell(n-1);
    }
}

/**
 * 第 n 小时 c 状态的细胞数
 */
public int cCell(int n) {
    if(n==1 || n==2){
        return0;
    }else{
        return bCell(n-1);
    }
}
```







### 动态规划：

#### 1.求数组间隔数最大和

![image-20211123175258019](Sort_thought.assets/image-20211123175258019.png)

![image-20211123175343790](Sort_thought.assets/image-20211123175343790.png)



```java
递归：2的n次方，复杂度

public static int rec_opt(int[] arr,int i){
        if(i==0){
            return arr[0];
        }else if(i==1){
            return Math.max(arr[0],arr[1]);
        }else{
            return Math.max(rec_opt(arr,i-1),arr[i]+rec_opt(arr,i-2));
        }
}

动态规划：
    public static int opt(int[] arr){
        int[] opt =new int[arr.length];
        opt[0] = arr[0];
        opt[1] = Math.max(arr[0],arr[1]);
        for (int i=2;i<arr.length;i++) {
            opt[i] = Math.max(opt[i-1],arr[i]+opt[i-2]);
        }
        return opt[opt.length-1];
    }
```



#### 2.数之和为9

https://www.bilibili.com/video/BV12W411v7rd?spm_id_from=333.999.0.0

![image-20211123212327380](Sort_thought.assets/image-20211123212327380.png)

如图数组，判断是否能有数之和为9

```java
递归
public static boolean rec_sum(int[] arr,int s , int i){
    if(s==0){
        return true;
    }else if(i==0){
        return arr[i]==s;
    }else if(arr[i] > s){
        return rec_sum(arr,s,i-1);
    }else{
        reurn rec_sum(arr,s,i-1) || rec_sum(arr,s-arr[i],i-1);
    }
    
}


动态规划
public static boolean sum(int[] arr,int S){
        boolean[][] subset = new boolean[arr.length][S+1];
    //初始化判断数组
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < S+1; j++) {
                if(j==0){
                    subset[i][j]=true;
                }if(i==0){
                    subset[i][j]=false;
                }
            }
        }
        subset[0][arr[0]]=true;
    
        for (int i = 1; i < subset.length; i++) {
            for (int j = 1; j < subset[0].length; j++) {
                if(arr[i] > S){
                    subset[i][j]=subset[i-1][j];
                }else{
                    if(j-arr[i]>0) {
                        subset[i][j] = subset[i - 1][j] || subset[i - 1][j - arr[i]];
                    }else{
                        subset[i][j] = subset[i - 1][j];
                    }
                }
            }
        }
        return subset[subset.length-1][subset[0].length-1];
    }


```



#### 3.连续子数组最大和

> 给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

状态转移方程为dp[i]: 以第i个结尾的最大序列和，即该点必选，则方程为；

`dp[0] = arr[0]`

`dp[i]=max(dp[i-1]+a[i],a[i])`

依此可以得出代码位：

```java
public int maxSubArray(int[] nums) {
    int dp[]=new int[nums.length];
    int max=nums[0];
    dp[0]=nums[0];
    for(int i=1;i<nums.length;i++)
    {
      dp[i]=Math.max(dp[i-1]+nums[i],nums[i]);
      if(dp[i]>max)
        max=dp[i];
    }
    return max;
}
```



#### 4.最长递增子序列 longest increasing subsquence

<!--子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。-->

dp[i]表示以i结尾的时最长的递增子序列。

j<i,所以状态转移方程为

如果nums[i]>nums[j],则dp[i] = （dp[j]+ 1，max）；

否则dp[i]=max。

```java
public int f(int[] nums){
    int dp[] = new int[nums.length];
    int maxLen = 1;
    for(int i=1; i<nums.length; i++){
        int max = 0;
        for(int j=0;j<i;j++){
        	if(nums[j]<nums[i]&&dp[j]>max){
            	max=dp[j];
            }
        }
        dp[i] = max+1;
        if(maxLen < dp[i])
            maxLen = dp[i];
    }
    return maxLen;
}
```





#### 5.最长公共子序列、最长公共子串

==最长公共子序列==

> 两个字符串匹配，我们设立二维`dp[][]`数组，`dp[i][j]`表示text1中第i个结尾，text2中第j个结尾的**最长公共子串的长度**。
>
> 如果`text1[i]==text2[j]`,因为两个元素都在最末尾的位置，**所以一定可以匹配成功**，换句话说，这个位置的邻居dp值不可能大于他（最多相等）。所以这个时候就是`dp[i][j]`=`dp[i-1][j-1]` +`1`;
>
> 如果不相等，则直接挪用dp [i-1] [j] 或 dp[i] [j-1] 

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        char ch1[]=text1.toCharArray();
        char ch2[]=text2.toCharArray();
        int dp[][]=new int[ch1.length+1][ch2.length+1];
        for(int i=0;i<ch1.length;i++)
        {
            for(int j=0;j<ch2.length;j++)
            {
                if(ch1[i]==ch2[j])
                {
                    dp[i+1][j+1]=dp[i][j]+1;
                }
                else
                    dp[i+1][j+1]=Math.max(dp[i][j+1],dp[i+1][j]);

            }
        }
        return dp[ch1.length][ch2.length];
    }
}
```



==最长公共子串==

如何分析呢? 和上面最长公共子序列的分析方式相似，要进行动态规划匹配，并且逻辑上处理更简单，只要当前i,j不匹配那么dp值就为0，如果可以匹配那么就变成`dp[i-1][j-1] + 1`







### 分治算法

> 将父问题分解为子问题同等方式求解，这和递归的概念很吻合，所以在分治算法通常以递归的方式实现(当然也有非递归的实现方式)。分治算法的描述从字面上也很容易理解，分、治其实还有个合并的过程：
>
> - 分(Divide)：递归解决较小的问题(到终止层或者可以解决的时候停下)
> - 治(Conquer)：递归求解，如果问题够小直接求解。
> - 合并(Combine)：将子问题的解构建父类问题

