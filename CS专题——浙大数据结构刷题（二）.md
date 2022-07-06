这是浙江大学《数据结构》刷题记录的第二篇，越发觉得编程是一件很有意思的事情。而且对于EE/CS/AI专业的学生，多刷算法题和数据结构题真的对于无论找工还是培养工程代码能力都大有裨益。下面就具体论述下这次的五道数据结构题目。
# 二分查找
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1497448917745405953](https://pintia.cn/problem-sets/1497448825169559552/problems/1497448917745405953)
这题就是最最原始的二分查找，之后刷很多算法题都有可能用到这个算法，因为其复杂度只有$O(logn)$。但是要注意的一点是，**二分查找需要数组或者序列是排好序的有序数列**。
代码如下：
```c
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10
#define NotFound 0
typedef int ElementType;

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};

List ReadInput(); /* 裁判实现，细节不表。元素从下标1开始存储 */
Position BinarySearch( List L, ElementType X );

int main()
{
    List L;
    ElementType X;
    Position P;
    
    L = ReadInput();
    scanf("%d", &X);
    P = BinarySearch( L, X );
    printf("%d\n", P);
    
    return 0;
}

/* 你的代码将被嵌在这里 */
Position BinarySearch( List L, ElementType X ){
    Position end = L->Last;
    Position start = 1;
    Position temp;
    Position i = NotFound;
    while(1){
        if(end==start){
            if(X==L->Data[end])
                return end;
            else
                break;
        }
        if((end-start+1)%2==0){
            temp = (end-start+1)/2 + start - 1;
            if(X==L->Data[temp])
                return temp;
            else if(X==L->Data[temp+1])
                return temp+1;
            else if(X<L->Data[temp])
                end = temp;
            else
                start = temp + 1;
        }
        else{
            temp = (start+end)/2;
            if(X==L->Data[temp])
                return temp;
            else if(X<L->Data[temp])
                end = temp - 1;
            else
                start = temp + 1;
        }
    }
    return i;
}
```
# 树的同构问题
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1503334324290916352](https://pintia.cn/problem-sets/1497448825169559552/problems/1503334324290916352)
其实在解决树的很多问题我们都需要用到一个很重要的性质——那就是树的递归效应，即树的子树仍旧是一棵树，所以在树的许多算法中也会用到递归的算法，在程序中经常体现为函数的递归调用，当然在递归时要经常考虑是否还能继续递归，即还有没有左节点或者右节点（节点可能为空，那就不需要继续递归了）。
代码如下：
```cpp
#include<iostream>
#include<cstdlib>
#include<cstdio>
#define Maxsize 10

using namespace std;

class Node{
    public:
    char value;
    int left;
    int right;
    Node(){
        value = 'a';
        left = -1;
        right = -1;
    }
};

Node tree[Maxsize];
int check[Maxsize] = {0};
Node tree2[Maxsize];
int check2[Maxsize] = {0};
void Issame(int root,int root2);
int Isthesame = 1;

int main(){
    int N,root;
    char tempchar;
    //读入数据并建树1
    cin>>N;
    for(int i = 0;i < N;i++){
        cin >> tree[i].value;
        cin >> tempchar;
        if(tempchar != '-'){
            tree[i].left = int(tempchar) - 48;
            check[(int(tempchar) - 48)] = 1;
        }
        else
            tree[i].left = -1;
        cin >> tempchar;
        if(tempchar != '-'){
            tree[i].right = int(tempchar) - 48;
            check[(int(tempchar) - 48)] = 1;
        }
        else
            tree[i].right = -1;
    }
    //寻找根节点
    for(int i = 0;i < N;i++)
    {
        if(check[i] == 0){
            root = i;
            break;
        }
    }
    //for(int i = 0;i < N;i++)
    //    cout<<tree[i].value<<" "<<tree[i].left<<" "<<tree[i].right<<endl;
    
    //读入数据并建树2
    int N2,root2;
    cin>>N2;
    for(int i = 0;i < N2;i++){
        cin >> tree2[i].value;
        cin >> tempchar;
        if(tempchar != '-'){
            tree2[i].left = int(tempchar) - 48;
            check2[(int(tempchar) - 48)] = 1;
        }
        else
            tree2[i].left = -1;
        cin >> tempchar;
        if(tempchar != '-'){
            tree2[i].right = int(tempchar) - 48;
            check2[(int(tempchar) - 48)] = 1;
        }
        else
            tree2[i].right = -1;
    }
    for(int i = 0;i < N2;i++)
    {
        if(check2[i] == 0){
            root2 = i;
            break;
        }
    }
    //for(int i = 0;i < N2;i++)
    //    cout<<tree2[i].value<<" "<<tree2[i].left<<" "<<tree2[i].right<<endl;
    //cout<<root<<endl;
    //cout<<root2<<endl;
    
    //判断两棵树是否同构
    if((N == 0 && N2 != 0)||(N != 0 && N2 == 0)){
        cout<<"No"<<endl;
        return 0;
    }
    else if(N == 0 && N2 == 0){
        cout<<"Yes"<<endl;
        return 0;
    }
    else
        Issame(root,root2);
    if(Isthesame == 1)
        cout<<"Yes"<<endl;
    else
        cout<<"No"<<endl;
    return 0;
}

void Issame(int root,int root2){
    //cout<<"1"<<endl;
    //考虑不同情况，采用递归的方法
    if(Isthesame == 0)
        return;
    if(tree[root].value != tree2[root2].value){
        Isthesame = 0;
        return;
    }
    else if((tree[root].left + tree[root].right == -2)&&(tree2[root2].left + tree2[root2].right == -2)){
        return;
    }
    else if((tree[root].left >= 0 && tree[root].right == -1)&&(tree2[root2].left >= 0 && tree2[root2].right == -1)){
        Issame(tree[root].left,tree2[root2].left);
    }
    else if((tree[root].left >= 0 && tree[root].right == -1)&&(tree2[root2].left == -1 && tree2[root2].right >= 0)){
        Issame(tree[root].left,tree2[root2].right);
    }
    else if((tree[root].left == -1 && tree[root].right >= 0)&&(tree2[root2].left >= 0 && tree2[root2].right == -1)){
        Issame(tree[root].right,tree2[root2].left);
    }
    else if((tree[root].left == -1 && tree[root].right >= 0)&&(tree2[root2].left == -1 && tree2[root2].right >= 0)){
        Issame(tree[root].right,tree2[root2].right);
    }
    else{
        if(tree[tree[root].left].value == tree2[tree2[root2].left].value){
            Issame(tree[root].left,tree2[root2].left);
            if(Isthesame != 0)
                Issame(tree[root].right,tree2[root2].right);
        }
        else{
            Issame(tree[root].left,tree2[root2].right);
            if(Isthesame != 0)
                Issame(tree[root].right,tree2[root2].left);
        }
    }
}
```
# List Leaves问题
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1503334324290916353](https://pintia.cn/problem-sets/1497448825169559552/problems/1503334324290916353)
这题本质上就是使用**层序遍历+判断叶节点**，从而将叶节点一个个的输出。当谈到层序遍历时，我们需要使用队列queue来实现层序遍历，这里我**使用了两个堆栈来代替队列**执行一样的功能。
代码如下：
```cpp
#include<iostream>
#include<cstdlib>
#include<cstdio>
#include<vector>
#define Maxsize 10

using namespace std;

class Node{
    public:
    int left;
    int right;
    Node(){
        left = -1;
        right = -1;
    }
};

Node tree[Maxsize];
int check[Maxsize] = {0};
int count = 0;

int main(){
    int N,root;
    char tempchar;
    //读入数据并建树1
    cin>>N;
    for(int i = 0;i < N;i++){
        cin >> tempchar;
        if(tempchar != '-'){
            tree[i].left = int(tempchar) - 48;
            check[(int(tempchar) - 48)] = 1;
        }
        else
            tree[i].left = -1;
        cin >> tempchar;
        if(tempchar != '-'){
            tree[i].right = int(tempchar) - 48;
            check[(int(tempchar) - 48)] = 1;
        }
        else
            tree[i].right = -1;
    }
    //寻找根节点
    for(int i = 0;i < N;i++)
    {
        if(check[i] == 0){
            root = i;
            break;
        }
    }
    //for(int i = 0;i < N;i++)
    //    cout<<i<<" "<<tree[i].left<<" "<<tree[i].right<<endl;
    //cout<<root<<endl;
    vector<int> stack1,stack2;
    int tempindex;
    //cout<<root;
    if(N == 0)
        return 0;
    else if(N == 1){
        cout<<root;
        return 0;
    }
    while(true){
        //pop阶段
        //下面使用两个stack来完成一个queue的功能
        if(stack1.size() != 0){
            auto begin = stack1.begin();
            auto end = stack1.end();
            while(end != begin){
                stack2.push_back(*(end-1));
                --end;
            }
            stack1.clear();
            tempindex = stack2.back();
            if(tree[tempindex].left == -1 && tree[tempindex].right == -1){
                if(count != 0){
                    cout<<" ";
                }
                cout<<tempindex;
                count++;
            }
            stack2.pop_back();
        }
        else{
            tempindex = root;
        }
        
        //push阶段
        if(stack2.size() != 0){
            auto begin2 = stack2.begin();
            auto end2 = stack2.end();
            while(end2 != begin2){
                stack1.push_back(*(end2-1));
                --end2;
            }
            stack2.clear();
        }
        
        if(tree[tempindex].left != -1){
            stack1.push_back(tree[tempindex].left);
            //cout<<tree[tempindex].left;
        }
        if(tree[tempindex].right != -1){
            stack1.push_back(tree[tempindex].right);
            //cout<<tree[tempindex].right;
        }
        
        //判断是否全部pop出
        if(stack1.size() == 0)
            break;
    }
    return 0;
}
```
# Tree Traversals Again
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1503334324290916354](https://pintia.cn/problem-sets/1497448825169559552/problems/1503334324290916354)
好难，研究ing。。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<vector>
#include<string>
#define Maxsize 30

using namespace std;

class Node{
    public:
    int lastnode;
    int leftnode;
    int rightnode;
    int flag;
    Node(){
        leftnode = -1;
        rightnode = -1;
        flag = 0;
    }
};

Node array[Maxsize];

void Postorder(int index);

int main(){
    string No;
    int N;
    int count;
    count = 0;
    getline(cin, No, '\n');
    N = stoi(No);
    string temp,temp2;
    int num,root;
    int nowindex = 0;
    //根据operations of stack建树
    for(int p = 0;p < 2*N;p++){
        getline(cin, temp, '\n');
        if(array[root].flag == 1 && array[root].leftnode != -1 && array[root].rightnode != -1)
            goto f;
        //cout<<temp<<endl;
        //根节点及根节点左边
        if(p == 0){
            num = (int)(temp.back()) - 48;
            //cout<<num<<endl;
            root = num;
            count++;
            nowindex = num;
            array[nowindex].lastnode = -1;
            //cout<<"PUSH"<<endl;
        }
        else{
            if(temp.length() > 3){
                //cout<<"PUSH"<<endl;
                num = (int)(temp.back()) - 48;
                //cout<<num<<endl;
                if(array[nowindex].leftnode == -1 && array[nowindex].flag == 0)
                    array[nowindex].leftnode = num;
                else
                    array[nowindex].rightnode = num;
                array[num].lastnode = nowindex;
                nowindex = num;
                count++;
            }
            else{
                //cout<<"POP"<<endl;
                if(array[nowindex].leftnode == -1){
                    array[nowindex].flag = 1;
                    nowindex = array[nowindex].lastnode;
                    while(array[nowindex].flag == 1)
                        nowindex = array[nowindex].lastnode;
                    
                }
                else{
                    array[nowindex].flag = 1;
                }
            }
        }
        continue;
        //根节点右边
        f:if(temp.length() > 3){
            num = (int)(temp.back()) - 48;
            //cout<<num<<endl;
            if(array[nowindex].leftnode == -1 && array[nowindex].flag == 0)
                array[nowindex].leftnode = num;
            else
                array[nowindex].rightnode = num;
            array[num].lastnode = nowindex;
            nowindex = num;
            count++;
        }
        else{
            
        }
        //cin>>temp;
        
        //for(int i = 0;i < count; i++)
        //    cout<<array[i].lastnode<<" "<<array[i].leftnode<<" "<<array[i].rightnode<<" "<<array[i].flag<<endl;
        //cout<<endl;
    }
    //for(int i = 0;i < count; i++)
    //    cout<<array[i].lastnode<<" "<<array[i].leftnode<<" "<<array[i].rightnode<<" "<<array[i].flag<<endl;
    
    //根据树进行后序遍历并输出
    Postorder(root);
    return 0;
}

int counts = 0;

void Postorder(int index){
    if(array[index].leftnode != -1)
        Postorder(array[index].leftnode);
    if(array[index].rightnode != -1)
        Postorder(array[index].rightnode);
    if(counts == 0){
        cout<<index;
        counts++;
    }
    else{
        cout<<" "<<index;
    }
}
```
# 同一棵二叉搜索树
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1505905647748263937](https://pintia.cn/problem-sets/1497448825169559552/problems/1505905647748263937)
这题也是花了40分钟左右就AC的一道题，真的不难，还是利用到上面所说的“树的递归效应”将树根据根节点一分为二，然后将同样的操作作用于子树，递归判断子树，子树的子树等等。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<vector>
#define Maxsize 10

using namespace std;

int flag = 1;
void Compare(int* a, int* b, int length1, int length2);

int main(){
    int N,L;
    int temps,tempnum;
    int arr_o[Maxsize];
    int arr_c[Maxsize];
    while(true){
        cin>>N;
        if(N == 0)
            return 0;
        cin>>L;
        //建立原始序列
        for(int i = 0;i < N;i++){
            cin >> tempnum;
            arr_o[i] = tempnum;
        }
        //读入与原始序列比较的L个序列
        for(int p = 0;p < L;p++){
            for(int i = 0;i < N;i++){
                cin >> temps;
                arr_c[i] = temps;
                if(temps == 0)
                    return 0;
            }
            flag = 1;
            //将两个序列进行比较
            Compare(arr_o,arr_c,N,N);
            if(flag == 0)
                cout<<"No"<<endl;
            else
                cout<<"Yes"<<endl;
        }
    }
    return 0;
}

void Compare(int* a, int* b, int length1, int length2){
    if(flag == 0)
        return;
    //长度不等直接返回No
    if(length1 != length2){
        //cout<<"a"<<endl;
        flag = 0;
        return;
    }
    //序列都为空直接返回
    if(length1 == 0 && length2 == 0){
        return;
    }
    else if(length1 <= 2){
        for(int i = 0;i < length1;i++){
            //根节点不一样直接No
            if(a[i] != b[i]){
                //cout<<"b"<<endl;
                flag = 0;
                return;
            }
        }
        return;
    }
    else{
        //如果根节点一样就直接比较根节点两边（大于和小于）的子序列，递归比较
        int tempa[Maxsize],tempb[Maxsize],tempa_2[Maxsize],tempb_2[Maxsize];
        int templen1,templen2,templen1_2,templen2_2;
        templen1 = templen2 = templen1_2 = templen2_2 = 0;
        for(int i = 0;i < length1;i++){
            if(i == 0){
                if(a[0] != b[0]){
                    //cout<<"c"<<endl;
                    flag = 0;
                    return;
                }
            }
            else{
                if(a[i] > a[0])
                    tempa_2[templen1_2++] = a[i];
                else
                    tempa[templen1++] = a[i];
                if(b[i] > b[0])
                    tempb_2[templen2_2++] = b[i];
                else
                    tempb[templen2++] = b[i];
            }
        }
        //递归比较左右子序列
        Compare(tempa,tempb,templen1,templen2);
        Compare(tempa_2,tempb_2,templen1_2,templen2_2);
        return;
    }
}
```
# 总结
从这次的五道题中，我又有新的收获，就是：1）关于代码的调试，可以多采用cout来观察想观察的变量来判断程序地bug在哪个地方；2）抓住问题的本质，多总结归纳，比如说树的递归效应，这在很多树的相关问题中都有用到；3）注释一定要及时补充好，不然真的过段时间非常容易忘记；4）**写代码的时候多注意边界测试**，比如i=0或者1，或者某个指针为空的情况等等。
欢迎对ECE/CS/AI感兴趣的小伙伴关注我，如果你对我的内容有什么建议的话，或者你也对算法和数据结构感兴趣的话，可以单独找我讨论，也欢迎在评论区留下你的声音。
