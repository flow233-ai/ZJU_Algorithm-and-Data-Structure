# CS专题——浙大数据结构刷题（一）
最近开始跟着浙江大学的MOOC一步步巩固算法与数据结构的基础，之前也用C++实现过很多数据结构和算法，大家可以去看我的另外一篇文章。纸上得来终觉浅，绝知此事要躬行。所以结合MOOC，我还在使用PTA上面对应的浙大数据结构习题集进行刷题。
# 最大子列和问题
链接：[链接戳此处](https://pintia.cn/problem-sets/1497448825169559552/problems/1497448917745405952)
这题整体上来看难度不大，但是作为引入算法与数据结构以及时间复杂度来说非常值得一试，总共有四种方法，第一种是三层遍历，第二种是两层遍历，第三种是二分法算子序列中的子列和，最后一种是在线处理的方法，下面我采取“在线处理”的方法。
代码如下：
```c
#include<stdio.h>
#define Maxsize 100000

int main(){
    int K;
    scanf("%d",&K);
    //直接用在线处理的方法解决，时间复杂度是O(N);
    int a[Maxsize];
    int thissum,maxsum;
    thissum = maxsum = 0;
    for(int i=0;i<K;i++){
        scanf("%d",&a[i]);
        thissum += a[i];
        //比较当前值的和与当前最大和
        if(thissum > maxsum)
            maxsum = thissum;
        //如果当前子列和为负，就直接归零，重启子序列
        if(thissum <= 0)
            thissum = 0;
        //printf("%d",thissum);
    }
    printf("%d",maxsum);
    return 0;
}
```
