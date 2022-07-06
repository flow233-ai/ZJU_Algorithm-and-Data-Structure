走过路过不要错过，浙大《数据结构》PTA刷题又来了，这次还是带来了五道题，目前我AC了其中三道题，还有两道题还差一点没有AC，所以后续可能会持续更新，请大家持续关注。
# 两个有序链表序列的合并
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1500420206044553216](https://pintia.cn/problem-sets/1497448825169559552/problems/1500420206044553216)
这题其实难度不大，就是链表中常用的一些操作，关键是审题一定要审清楚，像我就是没有看到“每个链表有头结点”这个信息卡了好久，审题非常重要。
代码如下：
```c
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;
typedef struct Node *PtrToNode;
struct Node {
    ElementType Data;
    PtrToNode   Next;
};
typedef PtrToNode List;

List Read(); /* 细节在此不表 */
void Print( List L ); /* 细节在此不表；空链表将输出NULL */

List Merge( List L1, List L2 );

int main()
{
    List L1, L2, L;
    L1 = Read();
    L2 = Read();
    L = Merge(L1, L2);
    Print(L);
    Print(L1);
    Print(L2);
    return 0;
}

/* 你的代码将被嵌在这里 */
List Merge(List L1, List L2 ){
    PtrToNode ans = (PtrToNode)malloc(sizeof(PtrToNode));
    PtrToNode temp = ans;
    PtrToNode p1 = L1;
    PtrToNode p2 = L2;
    while(p1 != NULL && p2 != NULL){
        if(p1->Data <= p2->Data){
            temp->Data = p1->Data;
            p1 = p1->Next;
        }
        else{
            temp->Data = p2->Data;
            p2 = p2->Next;
        }
        temp->Next = (PtrToNode)malloc(sizeof(struct Node));
        temp = temp->Next;
    }
    while(p1 != NULL){
        temp->Data = p1->Data;
        p1 = p1->Next;
        if(p1 != NULL){
            temp->Next = (PtrToNode)malloc(sizeof(struct Node));
            temp = temp->Next;
        }
    }
    while(p2 != NULL){
        temp->Data = p2->Data;
        p2 = p2->Next;
        if(p2 != NULL){
            temp->Next = (PtrToNode)malloc(sizeof(struct Node));
            temp = temp->Next;
        }
    }
    L1->Next = NULL;
    L2->Next = NULL;
    ans = ans->Next;
    return ans;
}
```
# Root of AVL Tree
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1505905647748263938](https://pintia.cn/problem-sets/1497448825169559552/problems/1505905647748263938)
这题就是考AVL树的四种旋转方式了，就是旋转那里要仔细想想，四种情况的结点赋值情况比较复杂，这里的树可以采用数组的方式实现（毕竟数组可以用来实现链表嘛。。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<vector>
#include<cmath>
#include<algorithm>
#define Maxsize 20

using namespace std;

class Node{
    public:
    int value;
    int lastnode;
    int leftnode;
    int rightnode;
    int h;
    Node(){
        lastnode = -1;
        leftnode = -1;
        rightnode = -1;
        h = 0; //定义为左树减右树的高度
    }
};

Node a[Maxsize];
int N;
int Calh(int node);
int Calheight(int node);


int main(){
    int temp,tempindex,root;
    cin >> N;
    tempindex = root = 0;
    //读入数据并建树
    for(int p = 0;p < N;p++){
        cin >> temp;
        //往现有树中添加新的节点
        a[p].value = temp;
        if(p != 0){
            tempindex = root;
            while(true){
                if(temp > a[tempindex].value){
                    if(a[tempindex].rightnode == -1){
                        a[tempindex].rightnode = p;
                        a[tempindex].h--;
                        a[p].lastnode = tempindex;
                        break;
                    }
                    else{
                        tempindex = a[tempindex].rightnode;
                    }
                }
                else{
                    if(a[tempindex].leftnode == -1){
                        a[tempindex].leftnode = p;
                        a[tempindex].h++;
                        a[p].lastnode = tempindex;
                        break;
                    }
                    else{
                        tempindex = a[tempindex].leftnode;
                    }
                }
            }
            //在每一轮添加节点后都重新计算每个节点的高度
            for(int s = 0;s < p + 1;s++)
                a[s].h = Calh(s);
            //for(int i = 0;i < p + 1;i++)
            //    cout<<a[i].lastnode<<" "<<a[i].value<<" "<<a[i].leftnode<<" "<<a[i].rightnode<<" "<<a[i].h<<endl;
            //cout<<endl;
            
            //根据重新计算得到的高度进行旋转
            for(int s = p;s > -1;s--){
                //发现出现不平衡，旋转成AVL树
                if(a[s].h == 2 || a[s].h == -2){
                    //看下有没有更低的旋转节点
                    if(a[s].leftnode != -1 && a[a[s].leftnode].h == -2)
                        s = a[s].leftnode;
                    else if(a[s].rightnode != -1 && a[a[s].rightnode].h == -2)
                        s = a[s].rightnode;
                    //cout<<s<<" ";
                    //LL旋转
                    if(a[s].value > a[p].value && a[a[s].leftnode].value > a[p].value){
                        cout<<"LL"<<endl;
                        int temprightnode;
                        temprightnode = a[a[s].leftnode].rightnode;
                        a[a[s].leftnode].rightnode = s;
                        a[a[s].leftnode].lastnode = a[s].lastnode;
                        a[s].lastnode = a[s].leftnode;
                        a[s].leftnode =  temprightnode;
                        if(a[a[s].lastnode].lastnode != -1){
                            if(a[a[a[s].lastnode].lastnode].leftnode == s)
                                a[a[a[s].lastnode].lastnode].leftnode = a[s].lastnode;
                            else
                                a[a[a[s].lastnode].lastnode].rightnode = a[s].lastnode;
                        }
                    }
                    //RR旋转
                    else if(a[s].value < a[p].value && a[a[s].rightnode].value < a[p].value){
                        //cout<<"RR"<<endl;
                        int templeftnode;
                        templeftnode = a[a[s].rightnode].leftnode;
                        a[a[s].rightnode].leftnode = s;
                        a[a[s].rightnode].lastnode = a[s].lastnode;
                        a[s].lastnode = a[s].rightnode;
                        a[s].rightnode =  templeftnode;
                        if(a[a[s].lastnode].lastnode != -1){
                            if(a[a[a[s].lastnode].lastnode].rightnode == s)
                                a[a[a[s].lastnode].lastnode].rightnode = a[s].lastnode;
                            else
                                a[a[a[s].lastnode].lastnode].leftnode = a[s].lastnode;
                        }
                    }
                    //LR旋转
                    else if(a[s].value > a[p].value && a[a[s].leftnode].value < a[p].value){
                        //cout<<"LR"<<endl;
                        int templastone,f,n;
                        n = a[a[s].leftnode].rightnode;
                        f = 0;
                        templastone = -1;
                        //判断LR旋转的方式
                        if(a[a[a[s].leftnode].rightnode].leftnode != -1){
                            templastone = a[a[a[s].leftnode].rightnode].leftnode;
                            f = 1;
                        }
                        else if(a[a[a[s].leftnode].rightnode].rightnode != -1){
                            templastone = a[a[a[s].leftnode].rightnode].rightnode;
                            f = 2;
                        }
                        a[a[a[s].leftnode].rightnode].leftnode = a[s].leftnode;
                        a[a[a[s].leftnode].rightnode].rightnode = s;
                        a[a[a[s].leftnode].rightnode].lastnode = a[s].lastnode;
                        if(a[a[a[s].leftnode].rightnode].lastnode != -1){
                            if(a[a[a[a[s].leftnode].rightnode].lastnode].leftnode == s)
                                a[a[a[a[s].leftnode].rightnode].lastnode].leftnode = a[a[s].leftnode].rightnode;
                            else
                                a[a[a[a[s].leftnode].rightnode].lastnode].rightnode = a[a[s].leftnode].rightnode;
                        }
                        a[a[s].leftnode].lastnode = a[a[s].leftnode].rightnode;
                        if(f == 0 || f == 1)
                            a[a[s].leftnode].rightnode = templastone;
                        a[s].lastnode = n;
                        if(f == 0 || f == 2)
                            a[s].leftnode = templastone;
                    }
                    //RL旋转
                    else{
                        //cout<<"RL"<<endl;
                        int templastone,f,n;
                        n = a[a[s].rightnode].leftnode;
                        f = 0;
                        templastone = -1;
                        //判断LR旋转的方式
                        if(a[a[a[s].rightnode].leftnode].rightnode != -1){
                            templastone = a[a[a[s].rightnode].leftnode].rightnode;
                            f = 1;
                        }
                        else if(a[a[a[s].rightnode].leftnode].leftnode != -1){
                            templastone = a[a[a[s].rightnode].leftnode].leftnode;
                            f = 2;
                        }
                        a[a[a[s].rightnode].leftnode].rightnode = a[s].rightnode;
                        a[a[a[s].rightnode].leftnode].leftnode = s;
                        a[a[a[s].rightnode].leftnode].lastnode = a[s].lastnode;
                        if(a[a[a[s].rightnode].leftnode].lastnode != -1){
                            if(a[a[a[a[s].rightnode].leftnode].lastnode].rightnode == s)
                                a[a[a[a[s].rightnode].leftnode].lastnode].rightnode = a[a[s].rightnode].leftnode;
                            else
                                a[a[a[a[s].rightnode].leftnode].lastnode].leftnode = a[a[s].rightnode].leftnode;
                        }
                        a[a[s].rightnode].lastnode = a[a[s].rightnode].leftnode;
                        if(f == 0 || f == 1)
                            a[a[s].rightnode].leftnode = templastone;
                        a[s].lastnode = n;
                        if(f == 0 || f == 2)
                            a[s].rightnode = templastone;
                    }
                    break;
                }
            }
            //重新计算根节点
            for(int s = 0;s < p + 1;s++){
                if(a[s].lastnode == -1){
                    root = s;
                    break;
                }
            }
            //cout<<root<<endl;
            for(int i = 0;i < p + 1;i++)
                cout<<a[i].lastnode<<" "<<a[i].value<<" "<<a[i].leftnode<<" "<<a[i].rightnode<<" "<<a[i].h<<endl;
            cout<<endl;
        }
    }
    //查看树中每个节点的相关成员的信息
    //for(int i = 0;i < N;i++)
    //    cout<<a[i].lastnode<<" "<<a[i].value<<" "<<a[i].leftnode<<" "<<a[i].rightnode<<" "<<a[i].h<<endl;
    cout<<a[root].value;
    return 0;
}

//计算一个节点的h，用于判断是否超出+-2
int Calh(int node){
    if(a[node].leftnode == -1 && a[node].rightnode == -1)
        return 0;
    else if(a[node].leftnode != -1 && a[node].rightnode == -1)
        return Calheight(a[node].leftnode);
    else if(a[node].leftnode == -1 && a[node].rightnode != -1)
        return - Calheight(a[node].rightnode);
    else
        return (Calheight(a[node].leftnode) - Calheight(a[node].rightnode));
}

//计算一个节点的高度
int Calheight(int node){
    if(a[node].leftnode == -1 && a[node].rightnode == -1)
        return 1;
    else if(a[node].leftnode != -1 && a[node].rightnode == -1)
        return (Calheight(a[node].leftnode) + 1);
    else if(a[node].leftnode == -1 && a[node].rightnode != -1)
        return (Calheight(a[node].rightnode) + 1);
    else
        return (max(Calheight(a[node].leftnode),Calheight(a[node].rightnode)) + 1);
}
```
# Complete Binary Search Tree
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1505905647748263939](https://pintia.cn/problem-sets/1497448825169559552/problems/1505905647748263939)
这道题目是让你根据一个序列构建唯一的完全二叉搜索树，关键是要想清楚如何通过序列构建完全二叉搜索树——其实关键就是每次序列最“中间”的那个点都要被选为根节点，然后根据这个根节点将这个序列再分为两个子序列，然后继续对子序列进行相同的操作，持续递归下去——毕竟树的本质就是递归嘛。这题不难。花了50分钟就AC了。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<vector>
#include<string>
#include <algorithm>
#define Maxsize 1000

using namespace std;

int N;
int a[Maxsize];
int s[Maxsize];
int tempstart[Maxsize];
int e[Maxsize];
int tempend[Maxsize];
vector<int> stack;
vector<int> stack2;

void Maopao(int* series,int length);
int pow(int dishu,int zhishu);
int Findlayer(int length);
int Findroot(int start,int end,int layer);
int Findindex(int starts,int ends,int layer);
int Countnum(int* series,int length,int num);

int main(){
    int temproot,tempindex;
    //数据读取
    cin >> N;
    for(int i = 0;i < N;i ++)
        cin >> a[i];
    //冒泡排序
    Maopao(a,N);
    //for(int i = 0;i < N;i++)
    //    cout<<" "<<a[i];
    //cout << endl;
    //递归寻找根节点
    int layer = Findlayer(N);
    s[0] = 0;
    e[0] = N - 1;
    //cout<<layer<<endl;
    for(int i = layer;i > 0;i--){
        for(int j = 0;j < pow(2,layer-i);j++){
            //if (i == 1 && j == pow(2,layer-1)-1)
            //    cout<<s[j]<<" "<<e[j]<<endl;
            temproot = Findroot(s[j],e[j],i);
            tempindex = Findindex(s[j],e[j],i);
            tempstart[2*j] = s[j];
            tempend[2*j] = tempindex - 1;
            tempstart[2*j+1] = tempindex + 1;
            tempend[2*j+1] = e[j];
            //if(i == 1)
            //    cout<<temproot<<endl;
            if(count(stack.begin(),stack.end(),temproot) != Countnum(a,N,temproot))
                stack.push_back(temproot);
        }
        //转移start和end
        for(int j = 0;j < pow(2,layer-i+1);j++){
            s[j] = tempstart[j];
            e[j] = tempend[j];
        }
    }
    //将堆栈里的元素一一转移到另一个堆栈里
    int tempnum;
    while(stack.size() != 0){
        tempnum = stack.back();
        stack2.push_back(tempnum);
        stack.pop_back();
    }
    //将堆栈里的元素一一输出
    int count = 0;
    while(stack2.size() != 0){
        if(count == 0){
            count++;
            cout<<stack2.back();
            stack2.pop_back();
        }
        else{
            cout<<" "<<stack2.back();
            stack2.pop_back();
        }
    }
    return 0;
}

//冒泡排序
void Maopao(int* series,int length)
{
    bool finish = false;
    int temp = 0;
    while (finish != true)
    {
        finish = true;
        for (int i = 0; i < length-1; i++)
        {
            for (int j = i; j < length-1; j++)
            {
                if (series[j] > series[j+1])
                {
                    temp = series[j];
                    series[j] = series[j + 1];
                    series[j + 1] = temp;
                    finish = false;
                }
            }
        }
    }
}

//计算序列包含的层数
int Findlayer(int length){
    int layernum = 1;
    //计算层数
    while(true){
        if((pow(2,layernum) - 1) >= N){
            break;
        }
        else
            layernum ++;
    }
    return layernum;
}

//递归寻找每一步的根节点
int Findroot(int starts,int ends,int layer){
    int root;
    if(starts == ends)
        return a[starts];
    int temp = ends - starts + 1 - pow(2,layer-1) + 1;
    if(temp <= pow(2,layer-2))
        root = a[starts + pow(2,layer-2) - 1 + temp];
    else
        root = a[starts + pow(2,layer-1) - 1];
    return root;
}

//递归寻找每一步的根节点的下标索引
int Findindex(int starts,int ends,int layer){
    int root;
    if(starts == ends)
        return starts;
    int temp = ends - starts + 1 - pow(2,layer-1) + 1;
    if(temp <= pow(2,layer-2))
        root = starts + pow(2,layer-2) - 1 + temp;
    else
        root = starts + pow(2,layer-1) - 1;
    return root;
}

//pow次方
int pow(int dishu,int zhishu){
    int di = 1;
    if(zhishu == 0)
        return 1;
    else{
        for(int i=0;i<zhishu;i++)
            di *= dishu;
    }
    return di;
}

//计算在一个序列中一个数字出现的次数
int Countnum(int* series,int length,int num){
    int count = 0;
    for(int i = 0;i < length;i++){
        if(series[i] == num)
            count ++;
    }
    return count;
}
```
# 堆中的路径
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1508006239265828864](https://pintia.cn/problem-sets/1497448825169559552/problems/1508006239265828864)
主要考察建堆的过程（这里是最小堆），也就是插入一个新结点后怎么调整原来的堆使之成为一个新的堆。代码量才四五十行，不难，依旧是四十分钟左右就AC的题目。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#define Maxsize 10000

using namespace std;

int N,M;
int H[Maxsize];

void Swap(int &a,int &b){
    int temp;
    temp = a;
    a = b;
    b = temp;
}

int main(){
    cin >> N >> M;
    //读入数据
    for(int i = 1;i < (N + 1);i++){
        int temp = i;
        cin >> H[i];
        if(i != 1){
            //建堆，插入新元素后调整成堆
            for(;temp != 1;temp = temp/2){
                if(H[temp] < H[temp/2])
                    Swap(H[temp],H[temp/2]);
                else
                    break;
            }
        }
    }
    //for(int i = 1; i < (N+1);i++)
    //    cout << H[i] << endl;
    //输出H[i]到根节点的路径
    int tempindex,count;
    for(int i = 0;i < M;i++){
        count = 0;
        cin >> tempindex;
        for(;tempindex != 0;tempindex /= 2){
            if(count == 0){
                cout << H[tempindex];
                count ++;
            }
            else
                cout << " " << H[tempindex];
        }
        cout << endl;
    }
    return 0;
}
```
这道题目只是考察了堆的插入与建立，并没有考虑堆的删除，所以我自己写了个堆的删除，堆的删除会写之后，堆排序（堆排序yyds）很容易就可以实现啦：
```cpp
#include<iostream>
#include<cstdio>
#define Maxsize 10000

using namespace std;

int N,M,temp;
int H[Maxsize] = {0};

void Swap(int &a,int &b){
    int temps;
    temps = a;
    a = b;
    b = temps;
}

void DeleteHeap(int index);
void CreateHeap();

int main(){
    cin >> N >> M;
    //读入数据,堆从数组下标为1开始
    for(int i = 1;i < (N + 1);i++){
        temp = i;
        cin >> H[i];
        if(i != 1)
            CreateHeap();
    }
    for(int i = 1; i < (N+1);i++)
        cout << H[i] << endl;
    cout<<endl;
    while(N != 0){
        DeleteHeap(1);
        N--;
    }
    return 0;
}

void CreateHeap(){
    //建堆，插入新元素后调整成堆
    for(;temp != 1;temp = temp/2){
        if(H[temp] < H[temp/2])
            Swap(H[temp],H[temp/2]);
        else
            break;
    }
}

void DeleteHeap(int index){
    if(N == 0)
        return;
    else if(N == 1){
        cout<<H[N];
        return;
    }
    if(index == 1){
        cout<<H[index];
        H[index] = H[N];
        H[N] = 0;
    }
    if(H[2*index] != 0 && H[2*index+1] != 0){
        if(H[2*index] < H[2*index+1] && H[2*index] < H[index]){
            Swap(H[index],H[2*index]);
            DeleteHeap(2*index);
        }
        else if(H[2*index] > H[2*index+1] && H[2*index+1] < H[index]){
            Swap(H[index],H[2*index+1]);
            DeleteHeap(2*index+1);
        }
        else
            return;
    }
    else if(H[2*index] != 0 && H[2*index+1] == 0){
        if(H[2*index] < H[index]){
            Swap(H[index],H[2*index]);
            DeleteHeap(2*index);
        }
        else
            return;
    }
    else if(H[2*index] == 0 && H[2*index+1] != 0){
        if(H[2*index+1] < H[index]){
            Swap(H[index],H[2*index+1]);
            DeleteHeap(2*index+1);
        }
        else
            return;
    }
    else
        return;
}
```
# File Transfer
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1508006239265828865](https://pintia.cn/problem-sets/1497448825169559552/problems/1508006239265828865)
考察并查集的并和查的操作，并查集本身不是一个很难的内容，这里主要考虑到并运算的过程中要考虑：1）小的集合并到大的集合中；2）在寻找根结点的同时要进行路径压缩。这两个操作的目的都是**防止树变得很高而导致搜索根结点效率很差**。总体难度不大。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#define Maxsize 10000

using namespace std;

int N,count;
int a[Maxsize];

int Findparent(int index);
void Union(int index1, int index2);
void Check(int index1, int index2);

int main(){
    cin >> N;
    count = N;
    //初始化
    for(int i = 0;i < N;i++)
        a[i] = -1;
    string temp = " ";
    int tempnum1, tempnum2;
    //读入指令
    while(true){
        cin >> temp;
        if(temp == "S")
            break;
        cin >> tempnum1 >> tempnum2;
        if(temp == "I"){
            //并集合
            if(count != 1)
                Union(tempnum1,tempnum2);
        }
        else{
            //确认两个节点是否连接
            if(count != 1)
                Check(tempnum1,tempnum2);
            else
                cout<<"yes"<<endl;
        }
    }
    //看网络的连接情况
    if(count == 1)
        cout<<"The network is connected.";
    else
        cout<<"There are "<< count <<" components.";
    cout<<endl;
    //for(int i = 0;i < N;i++)
    //    cout<<a[i]<<endl;
    //return 0;
}

//找父亲节点
int Findparent(int index){
    int temp = index;
    while(a[temp] >= 0){
        temp = a[temp];
    }
    //路径压缩
    if(index != temp)
        a[index] = temp;
    return temp;
}

//并，需要用到路径压缩和根据集合大小union
void Union(int index1, int index2){
    int root1 = Findparent(index1);
    int root2 = Findparent(index2);
    //小的集合并到大的集合中
    if(a[root1] >= a[root2]){
        int temp = a[root1];
        a[root1] = root2;
        a[root2] += temp;
        count--;
    }
    else{
        int temp = a[root2];
        a[root2] = root1;
        a[root1] += temp;
        count--;
    }
}

//查，查找两个节点
void Check(int index1, int index2){
    //if(a[index1] == index2 || a[index2] == index1 || index1 == index2){
    //    cout<<"yes"<<endl;
    //    return;
    //}
    int root1 = Findparent(index1);
    int root2 = Findparent(index2);
    if(root1 == root2)
        cout<<"yes"<<endl;
    else
        cout<<"no"<<endl;
}
```
# 总结
纸上得来终觉浅，绝知此事要躬行。通过这次的五道题我又有了新的收获：1）当代码出现bug或者答案不对时，要积极地去改代码，观测一些变量，不要焦虑地将一个错误的答案反复提交；2）debug和改代码的时间绝对要比你写代码或者有思路的时间长的多；3）如果答案不对的话，一定要审题！看下答案的输出格式什么的是否正确！审题非常非常关键，做算法题不是要做对，而是要把答案变成题目想让你呈现的样子而已。
欢迎对ECE/CS/AI感兴趣的小伙伴关注我，如果你对我的内容有什么建议的话，或者你也对算法和数据结构感兴趣的话，可以单独找我讨论，也欢迎在评论区留下你的声音。
