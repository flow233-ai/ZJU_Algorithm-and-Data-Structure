终于终于更上进度了，这一期又会带来最新的五道题目，终于更上姥姥更新的进度了，最近做题目越来越有感觉了，今天一共做了四个小时多的PTA，直接AC两道题+有一道题只差一分满分了（话说我的舍友都在刷字节跳动面试题还有Leetcode了，真的牛人）。话不多说，跟随我去看看本期的五道题把。
# 二叉搜索树的操作集
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1505905647748263936](https://pintia.cn/problem-sets/1497448825169559552/problems/1505905647748263936)
这题的难点主要在于树的删除操作，在写代码之前一定要想清楚树的删除操作过程是怎么实现的，想通了就好写多了。如果想通了还是实现不了，说明不是理解数据结构不到位，而是不会写代码（这种的话还是推荐把C++基础打好再来把）。友情提醒，**在树的问题中经常要做结点的判断，提前判断结点是否为NULL,否则就会出现访问错误**。
代码如下：
```cpp
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;
typedef struct TNode *Position;
typedef Position BinTree;
struct TNode{
    ElementType Data;
    BinTree Left;
    BinTree Right;
};

void PreorderTraversal( BinTree BT ); /* 先序遍历，由裁判实现，细节不表 */
void InorderTraversal( BinTree BT );  /* 中序遍历，由裁判实现，细节不表 */

BinTree Insert( BinTree BST, ElementType X );
BinTree Delete( BinTree BST, ElementType X );
Position Find( BinTree BST, ElementType X );
Position FindMin( BinTree BST );
Position FindMax( BinTree BST );

int main()
{
    BinTree BST, MinP, MaxP, Tmp;
    ElementType X;
    int N, i;

    BST = NULL;
    scanf("%d", &N);
    for ( i=0; i<N; i++ ) {
        scanf("%d", &X);
        BST = Insert(BST, X);
    }
    printf("Preorder:"); PreorderTraversal(BST); printf("\n");
    MinP = FindMin(BST);
    MaxP = FindMax(BST);
    scanf("%d", &N);
    for( i=0; i<N; i++ ) {
        scanf("%d", &X);
        Tmp = Find(BST, X);
        if (Tmp == NULL) printf("%d is not found\n", X);
        else {
            printf("%d is found\n", Tmp->Data);
            if (Tmp==MinP) printf("%d is the smallest key\n", Tmp->Data);
            if (Tmp==MaxP) printf("%d is the largest key\n", Tmp->Data);
        }
    }
    scanf("%d", &N);
    for( i=0; i<N; i++ ) {
        scanf("%d", &X);
        BST = Delete(BST, X);
    }
    printf("Inorder:"); InorderTraversal(BST); printf("\n");

    return 0;
}
/* 你的代码将被嵌在这里 */
Position FindMax2( BinTree BST );
Position FindMin2( BinTree BST );
BinTree Insert( BinTree BST, ElementType X ){
    if(BST == NULL){
        BST = (BinTree)malloc(sizeof(struct TNode));
        BST->Data = X;
        BST->Left = NULL;
        BST->Right = NULL;
        return BST;
    }
    BinTree temp = BST;
    while(1){
        if(X >= temp->Data){
            if(temp->Right != NULL)
                temp = temp->Right;
            else{
                temp->Right = (BinTree)malloc(sizeof(struct TNode));
                temp = temp->Right;
                temp->Data = X;
                temp->Left = NULL;
                temp->Right = NULL;
                break;
            }
        }
        else{
            if(temp->Left != NULL)
                temp = temp->Left;
            else{
                temp->Left = (BinTree)malloc(sizeof(struct TNode));
                temp = temp->Left;
                temp->Data = X;
                temp->Left = NULL;
                temp->Right = NULL;
                break;
            }
        }
    }
    return BST;
}
Position Find( BinTree BST, ElementType X ){
    BinTree temp = BST;
    while(1){
        if(temp == NULL)
            return NULL;
        if(temp->Data == X)
            return temp;
        else{
            if(X > temp->Data){
                if(temp->Right != NULL)
                    temp = temp->Right;
                else
                    return NULL;
            }
            else{
                if(temp->Left != NULL)
                    temp = temp->Left;
                else
                    return NULL;
            }
        }
    }
}
Position Find2( BinTree BST, ElementType X ){
    BinTree temp = BST;
    BinTree temp2 = NULL;
    while(1){
        if(temp == NULL)
            return NULL;
        if(temp->Data == X)
            return temp2;
        else{
            if(X > temp->Data){
                if(temp->Right != NULL){
                    temp2 = temp;
                    temp = temp->Right;
                }
                else
                    return NULL;
            }
            else{
                if(temp->Left != NULL){
                    temp2 = temp;
                    temp = temp->Left;
                }
                else
                    return NULL;
            }
        }
    }
}
BinTree Delete( BinTree BST, ElementType X ){
    BinTree temp = Find(BST,X);
    if(temp == NULL){
        printf("Not Found\n");
        return BST;
    }
    BinTree temp2 = Find2(BST,X);
    if(temp->Left == NULL && temp->Right == NULL){
        if(temp2 == NULL){
            BST = NULL;
        }
        else if(temp2->Left == temp){
            temp2->Left = NULL;
        }
        else{
            temp2->Right = NULL;
        }
        //printf("Found\n");
        return BST;
    }
    else if(temp->Left != NULL && temp->Right == NULL){
        if(temp2 == NULL){
            BST = temp->Left;
        }
        else if(temp2->Left == temp){
            temp2->Left = temp->Left;
        }
        else{
            temp2->Right = temp->Left;
        }
        //printf("Found\n");
        return BST;
    }
    else if(temp->Left == NULL && temp->Right != NULL){
        if(temp2 == NULL){
            BST = temp->Right;
        }
        else if(temp2->Left == temp){
            temp2->Left = temp->Right;
        }
        else{
            temp2->Right = temp->Right;
        }
        //printf("Found\n");
        return BST;
    }
    else{
        if((temp->Right)->Left == NULL){
            if(temp2 == NULL){
                BinTree t1 = BST->Left;
                BST = BST->Right;
                BST->Left = t1;
                //printf("Found\n");
                return BST;
            }
            else{
                if(temp2->Left == temp){
                    temp2->Left = temp->Right;
                    (temp->Right)->Left = temp->Left;
                }
                else{
                    temp2->Right = temp->Right;
                    (temp->Right)->Left = temp->Left;
                }
            }
        }
        else if(((temp->Right)->Left)->Left == NULL && ((temp->Right)->Left)->Right == NULL){
            if(temp2 == NULL){
                BinTree t5 = BST->Left;
                BinTree t6 = BST->Right;
                BST = (temp->Right)->Left;
                BST->Left = t5;
                BST->Right = t6;
                (BST->Right)->Left = NULL;
            }
            else{
                
            }
        }
        else if(((temp->Right)->Left)->Left != NULL){
            BinTree t3 = FindMin((temp->Right)->Left);
            BinTree t4 = FindMin2((temp->Right)->Left);
            if(temp2 == NULL){
                BinTree t5 = BST->Left;
                BinTree t6 = BST->Right;
                BST = t3;
                BST->Left = t5;
                BST->Right = t6;
                if(t4 == NULL){
                    (BST->Right)->Left = NULL;
                }
                else{
                    if(t4->Left == t3)
                        t4->Left = NULL;
                    else
                        t4->Right = NULL;
                }
            }
            else{
                
            }
        }
        else{
            BinTree t3 = FindMax((temp->Right)->Left);
            BinTree t4 = FindMax2((temp->Right)->Left);
            if(temp2 == NULL){
                BinTree t5 = BST->Left;
                BinTree t6 = BST->Right;
                BST = t3;
                BST->Left = t5;
                BST->Right = t6;
                if(t4 == NULL){
                    (BST->Right)->Left = NULL;
                }
                else{
                    if(t4->Left == t3)
                        t4->Left = NULL;
                    else
                        t4->Right = NULL;
                }
            }
            else{
                
            }
        }
        //printf("Found\n");
        //PreorderTraversal(BST);
        return BST;
    }
}
Position FindMin( BinTree BST ){
    BinTree temp = BST;
    if(temp == NULL)
        return BST;
    while(1){
        if(temp->Left != NULL)
            temp = temp->Left;
        else{
            return temp;
        }
    }
}
Position FindMin2( BinTree BST ){
    BinTree temp = BST;
    BinTree temp2 = NULL;
    if(temp == NULL)
        return NULL;
    while(1){
        if(temp->Left != NULL){
            temp2 = temp;
            temp = temp->Left;
        }
        else{
            return temp2;
        }
    }
}
Position FindMax2( BinTree BST ){
    BinTree temp = BST;
    BinTree temp2 = NULL;
    if(temp == NULL)
        return NULL;
    while(1){
        if(temp->Right != NULL){
            temp2 = temp;
            temp = temp->Right;
        }
        else{
            return temp2;
        }
    }
}
Position FindMax( BinTree BST ){
    BinTree temp = BST;
    if(temp == NULL)
        return BST;
    while(1){
        if(temp->Right != NULL)
            temp = temp->Right;
        else{
            return temp;
        }
    }
}
```
# Huffman Codes问题
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1508006239265828866](https://pintia.cn/problem-sets/1497448825169559552/problems/1508006239265828866)
这一题乍一看题目特别长（没错，当时唬住我了。。。这道题可以被分解为两个步骤：1）计算由哈夫曼树得到的最优编码的总长度；2）判断同学提交的编码是否符合“编码长度最优”和“无前缀问题”两个方面。理解了这一点就去写代码去吧！
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#define Maxsize 126
#define Maxsize2 1000

using namespace std;
class Node{
public:
    int leftnode;
    int rightnode;
    string c;
    int f;
    int h;
    Node(){
        leftnode = -1;
        rightnode = -1;
        c = " ";
        h = -1;
    }
};

int N,M;
int count = 0;
int extra = 0;
int length = 0;
Node a[Maxsize];
Node b[Maxsize];
string code[Maxsize2][Maxsize];

void Findmin();
void Findmin2();
void Qianxu(int index,int layer);

int main(){
    //读取数据
    cin >> N;
    for(int i = 0;i < N;i++){
        cin >> a[i].c;
        cin >> a[i].f;
        b[i].c = a[i].c;
        b[i].f = a[i].f;
    }
    while(count != (2*N - 2)){
    //通过建立霍夫曼树来判断最优编码的总长度
        Findmin();
        Findmin2();
        a[N+extra].leftnode = count;
        a[N+extra].rightnode = count+1;
        a[N+extra].f = a[count].f + a[count+1].f;
        extra ++;
        count += 2;
        //for(int i = 0;i < (N + extra);i++)
        //    cout<<a[i].f<<" ";
        //cout<<endl;
    }
    Qianxu(N+extra-1,0);
    for(int i = 0;i < N + extra;i++){
        //cout<<a[i].c<<" "<<a[i].leftnode<<" "<<a[i].rightnode<<" "<<a[i].f<<" "<<a[i].h<<endl; 
        if(a[i].c != " ")
            length += a[i].f * a[i].h;
    }
    //cout<<length<<endl;
    //得到最优编码的长度
    //判断下列编码符不符合最优码的要求
    cin >> M;
    string s1,s2;
    int templen;
    int flag2;
    for(int i = 0;i < M;i++){
        templen = 0;
        for(int j = 0;j < N;j++){
            cin >> s1;
            cin >> code[i][j];
            //cout<<code[i][j].size()<<" "<<b[j].f<<endl;
            templen += code[i][j].size() * b[j].f;
        }
        //此编码长度大于最优编码长度
        if(templen > length){
            //cout<<templen<<endl;
            cout<<"No"<<endl;
            continue;
        }
        //检查前缀问题
        flag2 = 0;
        for(int j = 0;j < N;j++){
            for(int k = 0;k < N;k++){
                if(k != j){
                    if(code[i][j].size() > code[i][k].size())
                        continue;
                    else if(code[i][j] == code[i][k].substr(0,code[i][j].size())){
                        cout<<"No"<<endl;
                        flag2 = 1;
                        break;
                    }
                }
            }
            if(flag2 == 1)
                break;
            if(j == N-1)
                cout<<"Yes"<<endl;
        }
    }
    return 0;
}

void Findmin(){
    int maxindex = -1;
    for(int i = count;i < (N + extra);i++){
        if(maxindex == -1)
            maxindex = i;
        else{
            if(a[i].f < a[maxindex].f)
                maxindex = i;
        }
    }
    if(maxindex != count){
        int temp,templeft,tempright;
        string temp2;
        temp = a[maxindex].f;
        temp2 = a[maxindex].c;
        templeft = a[maxindex].leftnode;
        tempright = a[maxindex].rightnode;
        a[maxindex].f = a[count].f;
        a[maxindex].c = a[count].c;
        a[maxindex].leftnode = a[count].leftnode;
        a[maxindex].rightnode = a[count].rightnode;
        a[count].f = temp;
        a[count].c = temp2;
        a[count].leftnode = templeft;
        a[count].rightnode = tempright;
    }
}

void Findmin2(){
    int maxindex = -1;
    for(int i = count+1;i < (N + extra);i++){
        if(maxindex == -1)
            maxindex = i;
        else{
            if(a[i].f < a[maxindex].f)
                maxindex = i;
        }
    }
    if(maxindex != count+1){
        int temp,templeft,tempright;
        string temp2;
        temp = a[maxindex].f;
        temp2 = a[maxindex].c;
        templeft = a[maxindex].leftnode;
        tempright = a[maxindex].rightnode;
        a[maxindex].f = a[count+1].f;
        a[maxindex].c = a[count+1].c;
        a[maxindex].leftnode = a[count+1].leftnode;
        a[maxindex].rightnode = a[count+1].rightnode;
        a[count+1].f = temp;
        a[count+1].c = temp2;
        a[count+1].leftnode = templeft;
        a[count+1].rightnode = tempright;
    }
}

void Qianxu(int index,int layer){
    if(a[index].leftnode == -1 && a[index].rightnode == -1){
        a[index].h = layer;
        return;
    }
    if(a[index].leftnode != -1)
        Qianxu(a[index].leftnode,layer+1);
    if(a[index].rightnode != -1)
        Qianxu(a[index].rightnode,layer+1);
}

```
# 列出图的连通集
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1510878544933298176](https://pintia.cn/problem-sets/1497448825169559552/problems/1510878544933298176)
最简单的题目，没有之一，30分钟一次性AC，没什么多说的，无非就是深度优先搜索和广度优先搜索嘛。。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<queue>
#include<string.h>
#define Maxsize 10

using namespace std;

int N,E;
int a[Maxsize][Maxsize];
int visited[Maxsize] = {0};

void DFS(int index);
void BFS(int index);

int main(){
    
    //读入数据
    cin >> N >> E;
    int temp1,temp2;
    for(int i = 0;i < E;i++){
        cin >> temp1 >> temp2;
        a[temp1][temp2] = 1;
        a[temp2][temp1] = 1;
    }
    //DFS
    for(int i = 0;i < N;i++){
        if(!visited[i]){
            cout<<"{";
            DFS(i);
            cout<<" }"<<endl;
        }
    }
    //BFS
    for(int i = 0;i < N;i++){
        if(visited[i]){
            cout<<"{";
            BFS(i);
            cout<<" }"<<endl;
        }
    }
    return 0;
}

void DFS(int index){
    visited[index] = 1;
    cout<<" "<<index;
    for(int i = 0;i < N;i++){
        if(a[index][i] != 0 && !visited[i])
            DFS(i);
    }
}

void BFS(int index){
    queue<int> q;
    int temp;
    q.push(index);
    visited[index] = 0;
    while(q.size() != 0){
        temp = q.front();
        q.pop();
        cout<<" "<<temp;
        for(int i = 0;i < N;i++){
            if(a[temp][i] != 0 && visited[i]){
                q.push(i);
                visited[i] = 0;
            }
        }
    }
}
```
# Saving James Bond
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1510878544933298177](https://pintia.cn/problem-sets/1497448825169559552/problems/1510878544933298177)
这一题如果审题正确，感觉20分钟可以AC的，直接将上一题的DFS拿来用稍微改改就好了（BTW，程序员也确实都有自己的一套代码库），问题是：**我审题没审清楚**，第一跳与后面几跳的循环不能放在一起写（至于为什么，可以去看看题目。其他没什么。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<queue>
#include<cmath>
#include<string.h>
#define Maxsize 101

using namespace std;

int N,flag;
float D;
int a[Maxsize][Maxsize];
float x[Maxsize],y[Maxsize];
int escaped[Maxsize] = {0};
int visited[Maxsize] = {0};

int checkdist(float x1,float y1,float x2,float y2);
void DFS(int index);

int main(){
    flag = 0;
    memset(a,0,sizeof(a));
    //读入N和D还有每个点
    cin >> N >> D;
    x[0] = y[0] = 0;
    for(int i = 1;i < N + 1;i++)
        cin >> x[i] >> y[i];
    //构建邻接矩阵
    for(int i = 0;i < N;i++){
        for(int j = i+1;j < N + 1;j++){
            if(i == 0){
                if(sqrt(x[j]*x[j]+y[j]*y[j]) <= 7.5 + D)
                    a[i][j] = a[j][i] = 1;
            }
            else{
                if(checkdist(x[i],y[i],x[j],y[j]) == 1)
                    a[i][j] = a[j][i] = 1;
            }
        }
    }
    //判断每个点是否能跳出
    for(int i = 0;i < N + 1;i++){
        if(i == 0){
            if(7.5 + D >= 50)
                escaped[i] = 1;
        }
        else{
            if(abs(x[i]-50)<=D||abs(x[i]+50)<=D||abs(y[i]-50)<=D||abs(y[i]+50)<=D)
                escaped[i] = 1;
        }
    }
    //开始从原点开始尝试跳出
    DFS(0);
    //输出结果
    if(flag == 0)
        cout<<"No"<<endl;
    else
        cout<<"Yes"<<endl;
    return 0;
}

int checkdist(float x1,float y1,float x2,float y2){
    float temp = sqrt((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2));
    if(temp <= D)
        return 1;
    else
        return 0;
}

void DFS(int index){
    if(flag == 1)
        return;
    visited[index] = 1;
    if(escaped[index] == 1){
        flag = 1;
        return;
    }
    for(int i = 0;i < N+1;i++){
        if(a[index][i] != 0 && !visited[i])
            DFS(i);
    }
}

```
# 六度空间问题
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1510878544933298178](https://pintia.cn/problem-sets/1497448825169559552/problems/1510878544933298178)
这道题是解决了一个很有名的命题——六度空间问题，牵扯到的知识点是计算一个点到另一个点的距离。我心想，如果一个一个点套用计算最短距离的函数，可能太慢了，**有没有什么全局更新的方法呢？答案是有，这里我用到了通信网中学过的一个算法**，轻松实现了计算所有点到一个固定结点的最短路径。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include <bits/stdc++.h>
#include<cmath>
#include<string.h>
#define Maxsize 1001
#define INIT  9999

using namespace std;

int N,M;
int a[Maxsize][Maxsize];
int vis[Maxsize][Maxsize];
int dis[Maxsize][Maxsize];
int min1 = 99999999;
int u = 0;

int check(int index1,int index2);
void dijkstra(int input);

int main(){
    //构建邻接矩阵
    cin >> N >> M;
    int index1,index2;
    //初始化
    for (int i = 0; i < N; i++)
    {
        for (int j = 0; j < N; j++)
        {
            if (i == j)
                a[i][j] = 0;
            else
                a[i][j] = 99999999;
        }
    }
    //读入数据
    for(int i = 0;i < M;i++){
        cin >> index1 >> index2;
        a[index1-1][index2-1] = 1;
        a[index2-1][index1-1] = 1;
    }
    //计算每个结点的六度结点百分比
    for(int j = 0;j < N;j++){
        for (int i = 0; i < N; i++)
            dis[j][i] = a[j][i];
        vis[j][j] = 1;
        //Dijkstra最短路算法求最短路径
        dijkstra(j);
        //for (int i = 0; i < N; i++)
        //    cout << dis[j][i];
        //cout<<endl;
        double temp = 0;
        for(int i = 0;i < N;i++){
            if(dis[j][i] <= 6)
                temp++;
        }
        //输出结果
        temp = temp/N * 100;
        cout << (j+1) << ": " << fixed << setprecision(2) << temp << "%"<< endl;
    }
    return 0;
}

void dijkstra(int input)
{
    for (int i = 0; i < N - 1; i++)
    {
        min1 = 99999999;
        // 寻找权值最小的点u
        for (int j = 0; j < N; j++)
        {
            if (vis[input][j] == 0 && dis[input][j] < min1)
            {
                min1 = dis[input][j];
                u = j;
            }
        }

        vis[input][u] = 1;

        for (int v = 0; v < N; v++)
        {
            // 对于每个u可达的v来说
            if (a[u][v] < 99999999)
            {
                // 如果当前的dis[v]不满足三角形不等式，那么进行松弛操作
                if (dis[input][v] > dis[input][u] + a[u][v])
                {
                    dis[input][v] = dis[input][u] + a[u][v];
                }
            }
        }
    }
}

```
# 总结
好啦好啦，又到了本期的总结时间了，通过这次的八道题我收获到了：1）**审题审题再审题**，多说多少遍都不为过；2）**如果发生了段错误的话，在链表类问题中就是出现了访问NULL结点的情况**，在数组类问题中有可能是数组越界、数组开太大了或者递归太多导致栈爆了等问题；3）如果运行超时，在浙大的PTA中大概率是哪个循环没有跳出来，如果是LeetCode超时的话有可能就是时间复杂度太大了；4）思路很重要，想清楚思路还有数据存放的方式以及算法再动手。
欢迎对ECE/CS/AI感兴趣的小伙伴关注我，如果你对我的内容有什么建议的话，或者你也对算法和数据结构感兴趣的话，可以单独找我讨论，也欢迎在评论区留下你的声音。
