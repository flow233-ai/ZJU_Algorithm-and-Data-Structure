天空一声巨响，迪宝闪亮登场。哈哈哈数据结构刷题又来了。实践是检验真理的唯一标准，最近感觉到自己的代码功底越来越深了，看来多写代码确实很有帮助。这次又带来最新的六道题目，来跟我一起去看看把。~~（顺便提一下，现在我在PTA上浙大《数据结构》这个题库已经排名6/2701了哦！！！）~~
# 哈利波特的考试
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1513011844732821504](https://pintia.cn/problem-sets/1497448825169559552/problems/1513011844732821504)
接下来三道题都是关于图中的最短路算法的，这道题采用的原则和最近统计信号处理中的“最大代价最小化准则”很相似，55分钟AC了。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#define Maxsize 101

using namespace std;

int N,M;
int min1 = 99999999;
int u = 0;
int minindex,minlen;
int a[Maxsize][Maxsize];
int dis[Maxsize];
int vis[Maxsize];

void dijkstra(int input);
int Checkmax();

int main(){
    //读取数据
    minindex = minlen = 0;
    cin >> N >> M;
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
    int index1,index2,val;
    for(int p = 0;p < M;p++){
        cin >> index1 >> index2 >> val;
        a[index1-1][index2-1] = a[index2-1][index1-1] = val;
    }
    int tempindex,templen;
    //求各个点的最短距离
    for(int i = 0;i < N;i++){
        for (int k = 0;k < N;k++)
            vis[k] = 0;
        for (int j = 0; j < N; j++)
            dis[j] = a[i][j];
        vis[i] = 1;
        //采用Dijkstra计算最短路径
        dijkstra(i);
        //最大路径最小化
        tempindex = Checkmax();
        templen = dis[tempindex];
        if(minlen == 0){
            minindex = i;
            minlen = templen;
        }
        else if(templen < minlen && minlen != 0){
            minindex = i;
            minlen = templen;
        }
    }
    //输出结果
    if(minlen == 99999999)
        cout<<"0"<<endl;
    else{
        cout<<(minindex+1)<<" "<<minlen<<endl;
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
            if (vis[j] == 0 && dis[j] < min1)
            {
                min1 = dis[j];
                u = j;
            }
        }
        vis[u] = 1;
        for (int v = 0; v < N; v++)
        {
            // 对于每个u可达的v来说
            if (a[u][v] < 99999999)
            {
                // 如果当前的dis[v]不满足三角形不等式，那么进行松弛操作
                if (dis[v] > dis[u] + a[u][v])
                {
                    dis[v] = dis[u] + a[u][v];
                }
            }
        }
    }
}

int Checkmax(){
    int temp = -1;
    for(int i = 0;i < N;i++){
        if(temp == -1)
            temp = i;
        else if(dis[i] > dis[temp])
            temp = i;
    }
    return temp;
}

```
# Saving James Bond（2）
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1513011844732821505](https://pintia.cn/problem-sets/1497448825169559552/problems/1513011844732821505)
这道题一开始没有什么思路，卡了好久，一开始是想在递归中加入标识layer来代表递归层数，后来发现不太正确；就另寻他法，打算把路径进行动态存储，效果还不错，得了29/30分，花了两个小时的时间。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<cmath>
#include<string.h>
#define Maxsize 101

using namespace std;

int N,flag;
int minipath = -1;
int nowpath = 0;
int count = 0;
double startfirst = 0;
double minstartfirst = 0;
float D;
int a[Maxsize][Maxsize];
int path[Maxsize]; //存放路径的
int minpath[Maxsize]; //存放逃脱路径的
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
    //去掉在岸上和岛上的鳄鱼
    int p = 1;
    while(p != (N + 1)){
        if((sqrt(x[p]*x[p]+y[p]*y[p]) <= 7.5)||(x[p]<=-50||x[p]>=50||y[p]<=-50||y[p]>=50)){
            for(int i = p;i < N;i++){
                x[i] = x[i + 1];
                y[i] = y[i + 1];
            }
            N --;
        }
        else{
            p ++;
        }
    }
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
        cout<<"0"<<endl;
    else{
        cout<<minipath<<endl;
        for(int i = 1;i < minipath;i++)
            cout<<x[minpath[i]]<<" "<<y[minpath[i]]<<endl;
    }
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
    //记录当前路径
    path[nowpath] = index;
    visited[index] = 1;
    //如果能上岸
    if(escaped[index] == 1){
        //判断是否需要更新最短路径的相关情况
        if((minipath == -1) || ((nowpath + 1) < minipath) ||((nowpath + 1) == minipath && startfirst <= minstartfirst)){
            for(int i = 0;i < (nowpath + 1);i++)
                minpath[i] = path[i];
            if(minipath == -1 ||(nowpath + 1) < minipath)
                minipath = nowpath + 1;
            flag = 1;
            //cout<<"1"<<" "<<x[index]<<" "<<y[index]<<endl;
            minstartfirst = startfirst;
        }
        nowpath --;
        return;
    }
    //遍历相邻没访问过的节点
    for(int i = 0;i < N+1;i++){
        //
        if(index == 0){
            for(int j = 0;j < N+1;j++){
                if(a[index][j] == 0 && j != 0)
                    visited[j] = 0;
            }
        }
        if(a[index][i] != 0 && !visited[i]){
            if(index == 0){
                //cout<<i<<endl;
                nowpath = 0;
                startfirst = sqrt(x[0]*x[i]+y[0]*y[i]);
            }
            nowpath ++;
            DFS(i);
        }
    }
    nowpath --;
}

```
# 旅游规划问题
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1513011844732821506](https://pintia.cn/problem-sets/1497448825169559552/problems/1513011844732821506)
这道题真的没什么，迪杰斯特拉算法改进版，20分钟AC。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#define Maxsize 500
#define Bignum 9999

using namespace std;

int N,M,S,D;
int min1;
int a[Maxsize][Maxsize];
int b[Maxsize][Maxsize];
int vis[Maxsize];
int dis[Maxsize];
int fee[Maxsize];
int u = 0;

void Dij(int input);

int main(){
    //数据读取
    cin >> N >> M >> S >> D;
    for(int i = 0;i < N;i++){
        for(int j = 0;j < N;j++){
            if(i == j)
                a[i][j] = 0;
            else
                a[i][j] = Bignum;
        }
    }
    int index1,index2,temp1,temp2;
    for(int k = 0;k < M;k++){
        cin >> index1 >> index2 >> temp1 >> temp2;
        a[index1][index2] = a[index2][index1] = temp1;
        b[index1][index2] = b[index2][index1] = temp2;
    }
    //最短路算法
    for (int p = 0;p < N;p++)
        vis[p] = 0;
    for (int j = 0; j < N; j++){
        dis[j] = a[S][j];
        fee[j] = b[S][j];
    }
    vis[S] = 1;
    Dij(S);
    //输出结果
    cout<<dis[D]<<" "<<fee[D]<<endl;
    return 0;
}

void Dij(int input){
    for (int i = 0; i < N - 1; i++){
        min1 = Bignum;
        // 寻找权值最小的点u
        for (int j = 0; j < N; j++){
            if (vis[j] == 0 && dis[j] < min1){
                min1 = dis[j];
                u = j;
            }
        }
        vis[u] = 1;
        for (int v = 0; v < N; v++){
            // 对于每个u可达的v来说
            if (a[u][v] < Bignum){
                // 如果当前的dis[v]不满足三角形不等式，那么进行松弛操作
                if (dis[v] > dis[u] + a[u][v]){
                    dis[v] = dis[u] + a[u][v];
                    fee[v] = fee[u] + b[u][v];
                }
                else if((dis[v] == (dis[u] + a[u][v]))&&(fee[v] > fee[u] + b[u][v])){
                    fee[v] = fee[u] + b[u][v];
                }
            }
        }
    }
}

```
# 公路村村通
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1516033032308813824](https://pintia.cn/problem-sets/1497448825169559552/problems/1516033032308813824)
这题真的是我目前为止做起来最灵活的一道题目，一道题用到了**Kruscal最小生成树算法+最小堆的堆排序+并查集及其操作**，真的这道题做完的时候有一种将学过的知识都融会贯通的感觉，看来以后也要经常把自己之前学过的算法or数据结构多拿来用用。最让我惊讶的是，自己竟然一个小时就AC了这道题，真的进步也太大了把！
代码如下：
```cpp
#include<iostream>
#define Maxsize 3003
#define num 9999

using namespace std;

//定义一条边的三个元素，起点终点和费用（权值）
class Node{
public:
    int ind1;
    int ind2;
    int fee;
    Node(){ fee = -1;}
};

int N,M,index1,index2,f,tempN,count;
//N，M：与题目里面意思相同
//index1,index2,f：暂存变量
//tempN：存储数组b的长度
//count：树里面包含的节点个数
//start
int a[Maxsize][Maxsize];
int *b;
int cost = 0;
Node H[Maxsize];
Node temp;

void Swap(Node &a, Node &b);
void CreateHeap(int index);
void DeleteHeap(int index);

int Findparent(int index);
void Union(int index1, int index2);
bool Check(int index1, int index2);

int main(){
    //读取数据并建立邻接矩阵和最小堆
    cin >> N >> M;
    b = new int[N+1];
    for(int i = 1;i<= N;i++)
        b[i] = -1;
    for(int i = 1;i <= N;i++){
        for(int j = 1;j <= N;j++)
            a[i][j] = a[j][i] = num;
    }
    for(int i = 1;i <= M;i++){
        cin >> index1 >> index2 >> f;
        a[index1][index2] = f;
        H[i].ind1 = index1;
        H[i].ind2 = index2;
        H[i].fee = f;
        CreateHeap(i);
    }
    tempN = M;
    count = 0;
    while(count < N - 1 && tempN != 0){
        temp = H[1];
        tempN--;
        //cout<<H[1].fee<<endl;
        DeleteHeap(1); //用最小堆+贪心找到每次的最小值
        if(count == 0){
            cost += a[temp.ind1][temp.ind2];
            Union(temp.ind1,temp.ind2); 
            continue;
        }
        //用并查集来确定现有路径加入树会不会构成回路
        if(Check(temp.ind1,temp.ind2) == false){
            cost += a[temp.ind1][temp.ind2];
            Union(temp.ind1,temp.ind2);
        }
        else
            continue;
    }
    //for(int i = 1;i <= N;i++)
    //    cout<<b[i]<<endl;
    if(count != N-1)
        cout<<"-1";
    else
        cout<<cost;
    return 0;
}

void Swap(Node &a, Node &b){
    int temp1,temp2,temp3;
    temp1 = a.ind1;
    temp2 = a.ind2;
    temp3 = a.fee;
    a.ind1 = b.ind1;
    a.ind2 = b.ind2;
    a.fee = b.fee;
    b.ind1 = temp1;
    b.ind2 = temp2;
    b.fee = temp3;
}

void CreateHeap(int index){
    //建堆，插入新元素后调整成堆
    int temp = index;
    for(;temp != 1;temp = temp/2){
        if(H[temp].fee < H[temp/2].fee)
            Swap(H[temp],H[temp/2]);
        else
            break;
    }
}

//堆的删除，用来实现堆排序
void DeleteHeap(int index){
    int Ms = tempN;
    if(Ms == 0)
        return;
    else if(Ms == 1){
        return;
    }
    if(index == 1){
        H[1].ind1 = H[Ms].ind1;
        H[1].ind2 = H[Ms].ind2;
        H[1].fee = H[Ms].fee;
        H[Ms].fee = -1;
    }
    //从两个子节点中找出比现在这个元素小的，交换位置并迭代函数
    //下面考虑了两种节点的多种情况（可能为空）
    if(2*index <= tempN && 2*index+1 <= tempN && H[2*index].fee != -1 && H[2*index+1].fee != -1){
        if(H[2*index].fee <= H[2*index+1].fee && H[2*index].fee < H[index].fee){
            Swap(H[index],H[2*index]);
            DeleteHeap(2*index);
        }
        else if(H[2*index].fee >= H[2*index+1].fee && H[2*index+1].fee < H[index].fee){
            Swap(H[index],H[2*index+1]);
            DeleteHeap(2*index+1);
        }
        else
            return;
    }
    else if(2*index <= tempN && 2*index+1 <= tempN && H[2*index].fee != -1 && H[2*index+1].fee == -1){
        if(H[2*index].fee < H[index].fee){
            Swap(H[index],H[2*index]);
            DeleteHeap(2*index);
        }
        else
            return;
    }
    else if(2*index <= tempN && 2*index+1 <= tempN && H[2*index].fee == -1 && H[2*index+1].fee != -1){
        if(H[2*index+1].fee < H[index].fee){
            Swap(H[index],H[2*index+1]);
            DeleteHeap(2*index+1);
        }
        else
            return;
    }
    else
        return;
}

//找父亲节点
int Findparent(int index){
    int temp = index;
    while(b[temp] >= 0){
        temp = b[temp];
    }
    //路径压缩
    if(index != temp)
        b[index] = temp;
    return temp;
}

//并，需要用到路径压缩和根据集合大小union
void Union(int index1, int index2){
    int root1 = Findparent(index1);
    int root2 = Findparent(index2);
    //小的集合并到大的集合中
    if(b[root1] >= b[root2]){
        int temp = b[root1];
        b[root1] = root2;
        b[root2] += temp;
        count++;
    }
    else{
        int temp = b[root2];
        b[root2] = root1;
        b[root1] += temp;
        count++;
    }
}

//查，查找两个节点
bool Check(int index1, int index2){
    int root1 = Findparent(index1);
    int root2 = Findparent(index2);
    if(root1 == root2)
        return true;
    else
        return false;
}
```
# How Long Does It Take
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1516033032308813825](https://pintia.cn/problem-sets/1497448825169559552/problems/1516033032308813825)
竟然40分钟就AC了！！！是关于AOE、关键路径问题的一道题。其实这道题只要把拓扑排序的代码稍微改一改就行了，后面应该还要升级版的关键路径问题，可以期待一下。
代码如下：
```cpp
#include<iostream>
#include<queue>
#define Maxsize 100

using namespace std;
int N,M;
int a[Maxsize][Maxsize];
int *Early;
int *indegree;
int count = 0;

int main(){
    //数据读取部分
    cin >> N >> M;
    indegree = new int[N];
    Early = new int[N];
    for(int i = 0;i < N;i++){
        indegree[i] = 0;
        Early[i] = -1;
    }
    //建立邻接矩阵和入度数组部分
    int index1,index2,lasting;
    for(int i = 0;i < N;i++)
        for(int j = 0;j < N;j++)
            a[i][j] = a[j][i] = -1;
    for(int s = 0;s < M;s++){
        cin >> index1 >> index2 >> lasting;
        a[index1][index2] = lasting;
        indegree[index2] ++;
    }
    queue<int> q;
    int tempind;
    for(int i = 0;i < N;i++){
        if(indegree[i] == 0){
            Early[i] = 0;
            q.push(i);
            count++;
        }
    }
    //不断将入度为0的点放进queue容器并弹出
    //不断更新入度和每个点的最早完成时间
    while(count != N && q.size() != 0){
        tempind = q.front();
        for(int j = 0;j < N;j++){
            if(a[tempind][j] != -1){
                indegree[j] --;
                if(indegree[j] == 0){
                    q.push(j);
                    count++;
                }
                if(indegree[j] >= 0 && Early[tempind] + a[tempind][j] > Early[j])
                    Early[j] = Early[tempind] + a[tempind][j];
            }
        }
        q.pop();
    }
    if(count != N){
        cout<<"Impossible";
        return 0;
    }
    //for(int i = 0;i < N;i++)
    //    cout<<Early[i]<<" ";
    //cout<<endl;
    int maxnum = Early[0];
    for(int i = 0;i < N;i++){
        if(Early[i] > maxnum)
            maxnum = Early[i];
    }
    cout<<maxnum;
    return 0;
}
```
# 关键活动
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1516033032308813826](https://pintia.cn/problem-sets/1497448825169559552/problems/1516033032308813826)
这题像是上一题的加强版，我直接在上一题代码的基础上改的，直接30分钟AC。不是很难，核心步骤为：建立邻接矩阵和入出度数组+利用queue正向得到工期最早完成的时间+将正向的步骤反过来反推每个checkpoint的最晚完成时间+根据关键活动的定义输出结果。
代码如下：
```cpp
#include<iostream>
#include<queue>
#define Maxsize 100
#define num 9999

using namespace std;
int N,M;
int a[Maxsize][Maxsize];
int *Early,*Late;
int *indegree,*outdegree;
int count = 0;

int main(){
    cin >> N >> M;
    indegree = new int[N];
    outdegree = new int[N];
    Early = new int[N];
    Late = new int[N];
    for(int i = 0;i < N;i++){
        indegree[i] = 0;
        outdegree[i] = 0;
        Early[i] = -1;
        Late[i] = num;
    }
    int index1,index2,lasting;
    for(int i = 0;i < N;i++)
        for(int j = 0;j < N;j++)
            a[i][j] = a[j][i] = -1;
    for(int s = 0;s < M;s++){
        cin >> index1 >> index2 >> lasting;
        a[index1-1][index2-1] = lasting;
        indegree[index2-1] ++;
        outdegree[index1-1] ++;
    }
    queue<int> q;
    int tempind;
    for(int i = 0;i < N;i++){
        if(indegree[i] == 0){
            Early[i] = 0;
            q.push(i);
            count++;
        }
    }
    while(count != N && q.size() != 0){
        tempind = q.front();
        for(int j = 0;j < N;j++){
            if(a[tempind][j] != -1){
                indegree[j] --;
                if(indegree[j] == 0){
                    q.push(j);
                    count++;
                }
                if(indegree[j] >= 0 && Early[tempind] + a[tempind][j] > Early[j])
                    Early[j] = Early[tempind] + a[tempind][j];
            }
        }
        q.pop();
    }
    if(count != N){
        cout<<"0";
        return 0;
    }
    //for(int i = 0;i < N;i++)
    //    cout<<Early[i]<<" ";
    //cout<<endl;
    int maxnum = 0;
    for(int i = 0;i < N;i++){
        if(Early[i] > Early[maxnum])
            maxnum = i;
    }
    cout<<Early[maxnum]<<endl;
    
    queue<int> q2;
    count = 0;
    for(int i = 0;i < N;i++){
        if(outdegree[i] == 0){
            Late[i] = Early[maxnum];
            q2.push(i);
            count++;
        }
    }
    while(count != N && q2.size() != 0){
        tempind = q2.front();
        for(int j = 0;j < N;j++){
            if(a[j][tempind] != -1){
                outdegree[j] --;
                if(outdegree[j] == 0){
                    q2.push(j);
                    count++;
                }
                if(outdegree[j] >= 0 && Late[tempind] - a[j][tempind] < Late[j])
                    Late[j] = Late[tempind] - a[j][tempind];
            }
        }
        q2.pop();
    }
    //for(int i = 0;i < N;i++)
    //    cout<<Early[i]<<" "<<Late[i]<<endl;
    
    for(int i = 0;i < N;i++){
        for(int j = N-1;j > -1;j--){
            if(i != j && a[i][j] != -1 && ((Late[j] - Early[i] - a[i][j]) == 0))
                cout<<(i+1)<<"->"<<(j+1)<<endl;
        }
    }
    return 0;
}
```
# 总结
纸上得来终觉浅，绝知此事要躬行。通过这次的六道题，我又有新的体悟：1）**多使用新学的、或者曾经掌握过的算法or数据结构，不要固步自封**；比如一个排序问题，不要每次都用冒泡暴力那一套，多用点新的排序算法如桶排序、堆排序或者插入排序等等，这样子才能掌握灵活运用各种算法和数据结构的奥秘，也能提升自己码代码的能力；2）多思考怎样高效Debug，Debug是很重要的，这次几道题我都完成得比较顺利就是因为Debug比较快，不然又要出现“写代码五分钟，Debug两小时”的无语场景。
欢迎对ECE/CS/AI感兴趣的小伙伴关注我，如果你对我的内容有什么建议的话，或者你也对算法和数据结构感兴趣的话，可以单独找我讨论，也欢迎在评论区留下你的声音。
