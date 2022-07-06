天空一声巨响，迪宝闪亮登场。哈哈哈数据结构刷题又来了。实践是检验真理的唯一标准，最近感觉到自己的代码功底越来越深了，看来多写代码确实很有帮助。这次又带来最新的五道题目，来跟我一起去看看把。~~（顺便提一下，现在我在PTA上浙大《数据结构》这个题库已经排名1/3105了哦！！！）~~
# 电话聊天狂人
这道题我就是使用了**哈希函数+分离链接法**，然后第一次还超时了，后来将哈希函数改了后，降低了装填因子，就AC了。整体难度不大。
此题代码如下：
```cpp
#include<iostream>
#include<string>

using namespace std;

class Node{
    public:
    string s;
    long int value;
    Node* next;
    Node(){ next = NULL;value = 0; }
    long int Getvalue() { return this->value; }
    Node* Getnext() { return this->next; }
    void Setvalue(long int num) { value = num; }
    void Setnext(Node* a) { next = a; }
};

class Nodearray{
    public:
    Node* head;
    Node* tail;
    int total;
    Nodearray(){
        head = NULL;
        tail = NULL;
        total = 0;
    }
    void Addnum(string num){
        if (head == NULL){
            head = new Node;
            head->s = num;
            head->Setvalue(1);
            head->Setnext(NULL);
            tail = head;
        }
        else{
            Node* p = this->head;
            Node* tempp = p;
            while(p != NULL){
                if(num > p->s){
                    tempp = p;
                    p = p->Getnext();
                }
                else{
                    Node* temp;
                    temp = new Node;
                    temp->s = num;
                    temp->Setvalue(1);
                    if(p == this->head){
                        temp->Setnext(p);
                        this->head = temp;
                        return;
                    }else{
                        temp->Setnext(p);
                        tempp->Setnext(temp);
                        return;
                    }
                }
            }
            tempp->Setnext(new Node);
            tempp = tempp->Getnext();
            tempp->s = num;
            tempp->Setvalue(1);
            tempp->Setnext(NULL);
            return;
        }
    }
    bool Findnum(string num){
        Node* p1 = head;
        Node* p2 = head;
        if (head == NULL){
            return false;
        }
        else{
            if (head->s == num){
                head->value ++;
                return true;
            }
            else
                p2 = p2->Getnext();
            while (p2 != NULL){
                if (p2->s == num){
                    p2->value ++;
                    return true;
                }
                p1 = p1->Getnext();
                p2 = p2->Getnext();
            }
        }
        return false;
    }
};


int N,index;
string s,s2;
Nodearray a[8020];
Node* temp;
long int maxlen,maxnum;

int Hash(string ss);

int main(){
    cin >> N;
    for(int i = 0;i < N;i++){
        cin >> s;
        index = Hash(s);
        if(a[index].Findnum(s) == false)
            a[index].Addnum(s);
        cin >> s;
        index = Hash(s);
        if(a[index].Findnum(s) == false)
            a[index].Addnum(s);
    }
    maxnum = -1;
    maxlen = 0;
    for(int i = 0;i < 8020;i++){
        if(a[i].head == NULL)
            continue;
        temp = a[i].head;
        while(temp != NULL){
            if(maxnum == -1){
                s2 = temp->s;
                maxlen = 1;
                maxnum = temp->Getvalue();
            }
            else if(temp->Getvalue() > maxnum){
                s2 = temp->s;
                maxlen = 1;
                maxnum = temp->Getvalue();
            }
            else if(temp->Getvalue() == maxnum){
                maxlen ++;
                if(temp->s < s2)
                    s2 = temp->s;
            }
            temp = temp->Getnext();
        }
    }
    cout<<s2<<' '<<maxnum;
    if(maxlen != 1)
        cout<<' '<<maxlen;
    return 0;
}

int Hash(string ss){
    int num = 0;
    for(int i = 0;i < ss.size();i++)
        num += ((int)ss[i] - 48)*((int)ss[i] - 48)*((int)ss[i] - 48);
    return num;
}
```
# Hashing
代码如下：
```cpp
#include <iostream>
#include <cmath>
using namespace std;
const int maxn = 100005;
typedef long long ll;
int TableSize;
bool a[maxn];
bool isPrime(int m) { 
    if(m <= 1) return false; 
    int k = (int)sqrt(m);
    for(int i = 2; i <= k; ++i) {
        if(m % i == 0) return false;
    }
    return true;
}
int NextPrime(int m) {
    if(m % 2 == 0 || m == 1)
        m++;
    while(!isPrime(m)) m += 2;
    return m;
}
int Hash(int x) {
    return x % TableSize;
}
void Insert(int x) {
    int p = Hash(x);
    if(!a[p]) { //该位置没有元素
        a[p] = true;
        cout << p;
    } else {
        int newp = p;
        int i;
        for(i = 1; i <= TableSize; ++i) {
            newp = (p+i*i) % TableSize;
            if(!a[newp]) {
                a[newp] = true;
                cout << newp;
                return;
            }
        }
        cout << '-';
    }
}
int main() {
    int M, N, x;
    cin >> M >> N;
    TableSize = NextPrime(M);
    for(int i = 0; i < N; ++i) {
        cin >> x;
        Insert(x);
        if(i == N-1) cout << endl;
        else cout << ' ';
    }
    return 0;
}


```
# QQ账号的登录
依旧是哈希函数+分离链接法解决冲突，40分钟AC。
代码如下：
```cpp
#include<iostream>
#include<string>

using namespace std;

class Node{
    public:
    string s;
    long int value;
    Node* next;
    Node(){ next = NULL;value = 0; }
    long int Getvalue() { return this->value; }
    Node* Getnext() { return this->next; }
    void Setvalue(long int num) { value = num; }
    void Setnext(Node* a) { next = a; }
};

class Nodearray{
    public:
    Node* head;
    Node* tail;
    int total;
    Nodearray(){
        head = NULL;
        tail = NULL;
        total = 0;
    }
    void Addnum(long int num,string pa){
        if (head == NULL){
            head = new Node;
            head->Setvalue(num);
            head->s = pa;
            head->Setnext(NULL);
            tail = head;
        }
        else{
            Node* p = this->head;
            Node* tempp = p;
            while(p != NULL){
                if(num > p->Getvalue()){
                    tempp = p;
                    p = p->Getnext();
                }
                else{
                    Node* temp;
                    temp = new Node;
                    temp->s = pa;
                    temp->Setvalue(num);
                    if(p == this->head){
                        temp->Setnext(p);
                        this->head = temp;
                        return;
                    }else{
                        temp->Setnext(p);
                        tempp->Setnext(temp);
                        return;
                    }
                }
            }
            tempp->Setnext(new Node);
            tempp = tempp->Getnext();
            tempp->s = pa;
            tempp->Setvalue(num);
            tempp->Setnext(NULL);
            return;
        }
    }
    bool Findnum(long int num){
        Node* p1 = head;
        Node* p2 = head;
        if (head == NULL){
            return false;
        }
        else{
            if (head->Getvalue() == num){
                return true;
            }
            else
                p2 = p2->Getnext();
            while (p2 != NULL){
                if (p2->Getvalue() == num){
                    return true;
                }
                p1 = p1->Getnext();
                p2 = p2->Getnext();
            }
        }
        return false;
    }
    void Checknum(long int num,string pa){
        Node* p1 = head;
        Node* p2 = head;
        if (head == NULL)
            return;
        else{
            if (head->Getvalue() == num){
                if(head->s == pa)
                    cout<<"Login: OK"<<endl;
                else
                    cout<<"ERROR: Wrong PW"<<endl;
                return;
            }
            else
                p2 = p2->Getnext();
            while (p2 != NULL){
                if (p2->Getvalue() == num){
                    if(p2->s == pa)
                        cout<<"Login: OK"<<endl;
                    else
                        cout<<"ERROR: Wrong PW"<<endl;
                    return;
                }
                p1 = p1->Getnext();
                p2 = p2->Getnext();
            }
        }
        return;
    }
};

int N,index;
char c;
long int t;
string temp2,temp3;
Nodearray a[8020];

int Hash(string ss);

int main(){
    cin >> N;
    for(int n = 0;n < N;n++){
        cin >> c >> t >> temp3;
        temp2 = to_string(t);
        index = Hash(temp2);
        if(c == 'N'){
            if(a[index].Findnum(t) == true)
                cout<<"ERROR: Exist"<<endl;
            else{
                a[index].Addnum(t,temp3);
                cout<<"New: OK"<<endl;
            }
        }
        else{
            if(a[index].Findnum(t) == false)
                cout<<"ERROR: Not Exist"<<endl;
            else
                a[index].Checknum(t,temp3);
        }
    }
    return 0;
}

int Hash(string ss){
    int num = 0;
    for(int i = 0;i < ss.size();i++)
        num += ((int)ss[i] - 48)*((int)ss[i] - 48)*((int)ss[i] - 48);
    return num;
}
```
# Hashing-Hard Version
这道题需要动点脑子，想清楚过程和规则后再下手。这里我先使用了希尔排序进行排序，再用查表的方式+某种判断算法来判断出下一个填入的数字是什么。
代码如下：
```cpp
#include<iostream>
#include<cmath>

using namespace std;
int N,start,index;
int total = 0;
int *a,*b;
int c[100000];

void HSort1(int *H,int n);

int main(){
    cin >> N;
    a = new int[N];
    b = new int[N];
    for(int i = 0;i < N;i++){
        cin >> a[i];
        b[i] = a[i];
    }
    HSort1(b,N);
    for(int i = 0;i < N;i++){
        if(b[i] != -1){
            start = i;
            break;
        }
    }
    int flag;
    while(total != N - start + 1){
        flag = 0;
        for(int i = start;i < N;i++){
            if(c[b[i]] == 1)
                continue;
            index = b[i] % N;
            if(a[index] == b[i]){
                c[b[i]] = 1;
                if(total == 0)
                    cout<<b[i];
                else
                    cout<<' '<<b[i];
                break;
            }
            for(int j = index;j < index + N;j++){
                if(c[a[j % N]] != 1 && a[j % N] != b[i])
                    break;
                if(a[j % N] == b[i]){
                    c[b[i]] = 1;
                    if(total == 0)
                        cout<<b[i];
                    else
                        cout<<' '<<b[i];
                    flag = 1;
                    break;
                }
            }
            if(flag == 1)
                break;
        }
        total++;
    }
    return 0;
}

void HSort1(int *H,int n){
    int count = 1;
    while((int)pow(2,count) - 1 < n)
        count++;
    count--;
    int D;
    for(int i = count;i > 0;i--){
        D = (int)pow(2,i) - 1;
        for(int j = 0;j < D;j++){
            for(int k = j;k < n;k += D){
                if(k == j)
                    continue;
                int t1 = H[k];
                for(int p = j;p < k;p += D){
                    if(t1 < H[p]){
                        for(int s = k;s > p;s -= D)
                            H[s] = H[s-D];
                        H[p] = t1;
                        break;
                    }
                }
            }
        }
    }
}
```
# KMP串的模式匹配
代码如下：
```cpp
#include <iostream>
#include <string>
#include <cstring>
#include <algorithm>
using namespace std;
typedef long long ll;
#define div 1000000007
const int maxn = 1000005;
const int inf  = 0x3f3f3f;
int N,M;
int nxt[maxn];
void getnxt(string t) {
    int len = t.length();
    int i = 0, j = -1; 
    nxt[0] = -1;
    while (i < len) {
        if (j == -1 || t[i] == t[j]) {
            i++, j++;
            if (t[i] == t[j])
                nxt[i] = nxt[j]; // next数组优化
            else
                nxt[i] = j;
        } else
            j = nxt[j];
    }
}

int KMP(string s, string t) {//s为文本串，t为模式串(短的那个)
    getnxt(t);
    int len1 = s.length();
    int len2 = t.length();
    int i = 0, j = 0, ans = 0;
    while (i < len1) {
        if (j == -1 || s[i] == t[j]) {
            i++, j++;
            if (j >= len2) {
                return i-j;
            }
        } else
            j = nxt[j];
    }
    return -1;
}
int main(){
    ios::sync_with_stdio(false);
    string String, Pattern;
    cin >> String;
    cin >> N;
    for(int i = 0; i < N; ++i) {
        memset(nxt, 0, sizeof(nxt));
        cin >> Pattern;
        int k = KMP(String, Pattern);
        if(k == -1) cout << "Not Found" << endl;
        else cout << String.substr(k) << endl;
    }
    return 0;
}


```
# 总结
这次是散列查找和KMP的专场。散列查找的问题或者说散列查找、哈希表本身不是很难用代码实现的查找算法，但是哈希算法却是一个非常重要的查找方法，值得我们细细品味。
欢迎对ECE/CS/AI感兴趣的小伙伴关注我，如果你对我的内容有什么建议的话，或者你也对算法和数据结构感兴趣的话，可以单独找我讨论，也欢迎在评论区留下你的声音。

