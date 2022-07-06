天空一声巨响，迪宝闪亮登场。哈哈哈数据结构刷题又来了。实践是检验真理的唯一标准，最近感觉到自己的代码功底越来越深了，看来多写代码确实很有帮助。这次又带来最新的四道题目，来跟我一起去看看把。~~（顺便提一下，现在我在PTA上浙大《数据结构》这个题库已经排名1/2976了哦！！！）~~
# 排序1 排序
这次我们学习了很多高级的排序算法，主要有以下几种：

   - 快速排序
   - 表排序
   - 物理排序
   - 桶排序/基数排序

下面我将尝试用这几种方式对排序1问题进行不同的尝试：
## 表排序
表排序的话其实就是为每个交换项增加了一个关键字，交换方法还是沿用基本排序算法的。
## 物理排序
物理排序的话其实在下面Sort with Swap的题目中已有涉及，这种方法在数据范围和数组长度相同时可以几乎达到线性复杂度，这里就不再写了。
## 桶排序
桶排序或者基数排序的话是用一定的适用场景的（就是在适用场景下才能发挥出桶排序/基数排序的威力，其他情况当然也能用），主要就是用空间换时间，这里也不太适合使用基数排序。
## 快速排序
下面重点来讲讲这道题的快速排序。快速排序一共分为以下几个步骤：

   - 选主元（左中右取中位数）
   - 子集划分（对撞指针）
   - cutoff阈值（规模较小时直接使用简单排序算法）

快速排序一共用两种实现方式：

   - 直接调用库函数
   - 直接手撕一遍

下面我就来手撕一遍快速排序算法把！
此题代码如下：
```cpp
#include<iostream>
#include<cmath>

using namespace std;
long int N;
long int *a;

void Median(int l,int r);
int Subset(int l,int r);
void Swap(long int&a,long int&b);
void HSort(long int *H,int n);
void Quicksort(int l,int r);

int main(){
    cin >> N;
    a = new long int[N];
    for(int i = 0;i < N;i++)
        cin >> a[i];
    Quicksort(0,N-1);
    cout<<a[0];
    for(int i = 1;i < N;i++)
        cout << ' ' << a[i];
    return 0;
}

void Swap(long int&a,long int&b){
    long int temp = a;
    a = b;
    b = temp;
}

void Quicksort(int l,int r){
    if(l >= r)
        return;
    else if(r-l == 1){
        if(a[l] > a[r])
            Swap(a[l],a[r]);
        return;
    }
    //如果小于阈值就直接希尔排序
    if(r-l <= 10){
        HSort(a+l,r-l+1);
        return;
    }
    Median(l,r); //选主元
    int med = Subset(l+1,r-1); //子集划分
    Quicksort(l,med-1); //递归排序左子集
    Quicksort(med+1,r); //递归排序右子集
}

void Median(int l,int r){
    int c = (l+r) / 2;
    if(a[l] > a[c])
        Swap(a[l],a[c]);
    if(a[c] > a[r])
        Swap(a[r],a[c]);
    if(a[l] > a[c])
        Swap(a[l],a[c]);
    Swap(a[c],a[r-1]);
}

int Subset(int l,int r){
    int z1 = l;
    int z2 = r-1;
    int flag1 = 0;
    while(z1 < r && z2 < r){
        if(flag1 == 0){
            if(a[z1] > a[r])
                flag1 = 1;
            else
                z1++;
        }
        else{
            if(a[z2] < a[r]){
                if(z1 <= z2){
                    Swap(a[z1],a[z2]);
                    flag1 = 0;
                }
                else
                    break;
            }
            else
                z2--;
        }
    }
    Swap(a[z1],a[r]);
    return z1;
}

void HSort(long int *H,int n){
    int count = 1;
    while((int)pow(2,count) - 1 < n)
        count++;
    count--;
    for(int i = count;i > 0;i--){
        int D = (int)pow(2,i) - 1;
        for(int j = 0;j < D;j++){
            for(int k = j;k < n;k += D){
                if(k == j)
                    continue;
                long int t = H[k];
                for(int p = j;p < k;p += D){
                    if(t < H[p]){
                        for(int s = k;s > p;s -= D){
                            H[s] = H[s-D];
                        }
                        H[p] = t;
                        break;
                    }
                }
            }
        }
    }
}

```
阈值为10的结果如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2131041/1651652777819-3bf755e3-c58d-453b-a603-b7b24d0c7f6c.png#clientId=u710f2fe9-881f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=697&id=u35cbe7d6&margin=%5Bobject%20Object%5D&name=image.png&originHeight=871&originWidth=1164&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72796&status=done&style=none&taskId=u8c90195b-6d0a-46fb-b31f-80bc1a2140c&title=&width=931.2)
阈值为30的结果如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2131041/1651652830781-220f0dc2-8db5-418a-b140-a3b07bb3e118.png#clientId=u710f2fe9-881f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=685&id=u9fea74ec&margin=%5Bobject%20Object%5D&name=image.png&originHeight=856&originWidth=1140&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71945&status=done&style=none&taskId=ubc3f7770-5e2d-4c83-8313-f6116faa483&title=&width=912)
阈值为70的结果如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2131041/1651652911995-ee254d2c-62f7-49e9-842c-069fcb1935a7.png#clientId=u710f2fe9-881f-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=682&id=u3248e837&margin=%5Bobject%20Object%5D&name=image.png&originHeight=852&originWidth=1135&originalType=binary&ratio=1&rotation=0&showTitle=false&size=71229&status=done&style=none&taskId=u91a4be7f-f0ee-4754-9059-4e9a232829a&title=&width=908)
可以发现，当阈值为30时，排序时间复杂度最小，但是这个也跟问题规模有关，当问题规模相对较小时，快速排序比较慢，这时候可以选择简单排序算法如希尔排序等。**所以快速排序的方法可以归纳为**：分而治之+对每个分好的子集使用简单排序算法。
# 统计工龄
这道题挺简单的，没什么难点。
此题代码如下：
```cpp
#include<iostream>

using namespace std;
long int N;
long int *a;

int main(){
    int temp;
    cin >> N;
    a = new long int[51];
    for(long int i = 0;i < N;i++){
        cin >> temp;
        a[temp]++;
    }
    for(int i = 0;i < 51;i++){
        if(a[i] != 0)
            cout<<i<<':'<<a[i]<<endl;
    }
    return 0;
}
```
# PAT Judge
这道题非常锻炼代码能力。主要的思路就是根据几个指标进行**桶排序**，或者叫**基数排序**，而且要采用“**次位优先”**的方法，还使用到了**链表**和**哈希表**的相关知识和操作，可以说出的很不错。
此题代码如下：
```cpp
#include<iostream>
#include<string>

using namespace std;
int N,K;
long int M;
int *p;

class Node2{
    public:
    string name;
    int ttr;
    int grade;
    int s[6];
    Node2(){
        ttr = 0;name = "";
        grade = 0;
        for(int i = 1;i <= K;i++)
            s[i] = -2;
    }
    Node2 &operator=(const Node2& n){
        if(this == &n)
            return *this;
        else{
            this->name = n.name;
            this->ttr = n.ttr;
            this->grade = n.grade;
            for(int i = 1;i < 6;i++)
                this->s[i] = n.s[i];
            return *this;
        }
    }
    bool operator==(const Node2& n){
        if(this->name != n.name || this->ttr != n.ttr || this->grade != n.grade)
            return false;
        for(int i = 1;i < 6;i++){
            if(this->s[i] != n.s[i])
                return false;
        }
        return true;
    }
};

class Node{
    private:
    Node2 value;
    Node* next;
    public:
    Node(){ next = NULL; }
    Node2 Getvalue() { return this->value; }
    Node* Getnext() { return this->next; }
    void Setvalue(Node2 num) { value = num; }
    void Setnext(Node* a) { next = a; }
};

class Nodearray{
    public:
    Node* head;
    Node* tail;
    Nodearray(){
        head = NULL;
        tail = NULL;
    }
    void Addnum(Node2 num){
        if (head == NULL){
            head = new Node;
            head->Setvalue(num);
            head->Setnext(NULL);
            tail = head;
        }
        else{
            tail->Setnext(new Node);
            (tail->Getnext())->Setvalue(num);
            (tail->Getnext())->Setnext(NULL);
            tail = tail->Getnext();
        }
    }
    void Delnum(Node2 num){
        Node* p1 = head;
        Node* p2 = head;
        if (head == NULL){
            cout << "这是空列表" << endl;
            return;
        }
        else{
            if (head->Getvalue() == num){
                head = head->Getnext();
                return;
            }
            else
                p2 = p2->Getnext();
            while (p2 != NULL){
                if (p2->Getvalue() == num){
                    p1->Setnext(p2->Getnext());
                    cout << "删除成功" << endl;
                    return;
                }
                p1 = p1->Getnext();
                p2 = p2->Getnext();
            }
        }
    }
};

Node2 *a;
Nodearray *b,*c,*d;

int main(){
    cin >> N >> K >> M;
    a = new Node2[N+1];
    p = new int[K+1];
    int mf = 0;
    for(int i = 1;i <= K;i++){
        cin >> p[i];
        mf += p[i];
    }
    string temp;int k,g;
    for(int m = 0;m < M;m++){
        cin >> temp >> k >> g;
        if(a[stoi(temp)].name == "")
            a[stoi(temp)].name = temp;
        if(g > a[stoi(temp)].s[k]){
            if(a[stoi(temp)].s[k] >= 0)
                a[stoi(temp)].grade += g - a[stoi(temp)].s[k];
            else if(g >= 0)
                a[stoi(temp)].grade += g;
            a[stoi(temp)].s[k] = g;
            if(g == p[k])
                a[stoi(temp)].ttr++;
        }
    }
    //按照总分排序
    b = new Nodearray[K+1];
    c = new Nodearray[mf+1];
    int flag;
    for(int i = 1;i <= N;i++){
        if(a[i].name == "")
            continue;
        if(a[i].grade == 0){
            flag = 0;
            for(int j = 1;j <= K;j++){
                if(a[i].s[j] >= 0)
                    break;
                if(j == K)
                    flag = 1;
            }
            if(flag == 1)
                continue;
        }
        b[a[i].ttr].Addnum(a[i]);
    }
    //按照完全对题数排序
    Node *p;
    for(int i = K;i > -1;i--){
        p = b[i].head;
        while(p != NULL){
            c[(p->Getvalue()).grade].Addnum(p->Getvalue());
            p = p->Getnext();
        }
    }
    //按照ID排序
    Node *p2;
    int count = 1;int lastcount;int lastg = -1;
    for(int i = mf;i > -1;i--){
        p2 = c[i].head;
        while(p2 != NULL){
            if((p2->Getvalue()).grade != lastg){
                cout<<count<<' ';
                lastcount = count;
            }
            else
                cout<<lastcount<<' ';
            lastg = (p2->Getvalue()).grade;
            cout<<(p2->Getvalue()).name<<' '<<(p2->Getvalue()).grade;
            for(int j = 1;j <= K;j++){  
                if((p2->Getvalue()).s[j] >= 0)
                    cout<<' '<<(p2->Getvalue()).s[j];
                else if((p2->Getvalue()).s[j] == -1)
                    cout<<" 0";
                else
                    cout<<" -";
            }
            cout<<endl;
            count++;
            p2 = p2->Getnext();
        }
    }
    return 0;
}
```
# Sort with Swap
这道题就是赤裸裸的**物理排序**了，先想清楚答案是怎么计算得到的，再考虑一些特殊情况（比如一开始0就在索引为0的位置等），这道题帮助大家巩固**表排序**和**物理排序**。
此题代码如下：
```cpp
#include<iostream>

using namespace std;
long int N;
long int *a;
long int *vis;

int main(){
    cin >> N;
    if(N == 1){
        cout<<'0';
        return 0;
    }
    a = new long int[N];
    vis = new long int[N];
    for(int i = 0;i < N;i++)
        vis[i] = 0;
    for(int i = 0;i < N;i++)
        cin >> a[i];
    int count = N-2;
    if(a[0] == 0)
        count += 2;
    for(int i = 0;i < N;i++){
        if(vis[i] == 0){
            if(a[i] == i){
                vis[i] = 1;
                count -= 1;
                continue;
            }
            int temp = i;
            while(a[temp] != i){
                vis[temp] = 1;
                temp = a[temp];
            }
            vis[temp] = 1;
            count++;
        }
    }
    cout<<count;
    return 0;
}
```
# 总结
这次的刷题记录是排序专场，我们学习到了几种高级排序方法，各有千秋，强烈建议所有排序大家都手写一次，这样子以后用到的时候直接CV就行（这也是为自己的代码库增加新代码的方式嘛！）。通过这次刷题，我又获得了新的体悟：1）**你现在写的代码有可能可以为将来节省不少时间**，之后再遇到直接CV即可；2）在实现算法之前多想想，**不要只是急于实现，刷题到这个地步时也要想想如何高效实现，包括时间空间复杂度和代码量等等**。
欢迎对ECE/CS/AI感兴趣的小伙伴关注我，如果你对我的内容有什么建议的话，或者你也对算法和数据结构感兴趣的话，可以单独找我讨论，也欢迎在评论区留下你的声音。
