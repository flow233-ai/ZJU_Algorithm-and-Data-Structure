最近开始跟着浙江大学的MOOC一步步巩固算法与数据结构的基础，之前也用C++实现过很多数据结构和算法，大家可以去看我的另外一篇文章。纸上得来终觉浅，绝知此事要躬行。所以结合MOOC，我还在使用PTA上面对应的浙大数据结构习题集进行刷题。
# 最大子列和问题
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1497448917745405952](https://pintia.cn/problem-sets/1497448825169559552/problems/1497448917745405952)
这题整体上来看难度不大，但是作为引入算法与数据结构以及时间复杂度来说非常值得一试，总共有四种方法，第一种是三层遍历，第二种是两层遍历，第三种是二分法算子序列中的子列和，最后一种是在线处理的方法，下面我采取“在线处理”的方法，时间复杂度为$O(N)$。
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
# Maximum Subsequence Sum
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1497448917745405954](https://pintia.cn/problem-sets/1497448825169559552/problems/1497448917745405954)
这题就是上一题赤裸裸的PLUS版，就是多了：需要我们记录头部和尾部还有需要考虑很多极端情况，总之就是根据极端情况定义一些相应的变量就行了。
代码如下：
```c
#include<stdio.h>
#define Maxsize 100000

int main(){
    //这题就是上一题的PLUS版本
    //需要我们记录头部和尾部
    //同样大小的和要找头下标最小的子列
    //且需要考虑很多极端情况
    int K;
    scanf("%d",&K);
    int a[Maxsize];
    int thissum,maxsum,maxfirst,maxlast,thisfirst,thislast;
    thissum = maxsum = 0;
    maxfirst = maxlast = 0;
    thisfirst = thislast = 0;
    int flag = 0;
    int flag2 = 0;
    int flag3 = 0;
    int start,end;
    for(int i=0;i<K;i++){
        scanf("%d",&a[i]);
        if(a[i] > 0)
            flag = 1;
        else if(a[i] == 0)
            flag2 = 1;
        if(flag3 == 0){
            start = i;
            thisfirst = a[i];
            flag3 = 1;
        }
        thislast = a[i];
        end = i;
        thissum += a[i];
        //length++;
        if(thissum > maxsum){
            maxsum = thissum;
            maxfirst = thisfirst;
            maxlast = thislast;
        }
        if(thissum < 0){
            thissum = 0;
            flag3 = 0;
        }
        //printf("%d",thissum);
    }
    if(flag == 1)
        printf("%d %d %d",maxsum,maxfirst,maxlast);
    else{
        if(flag2 == 1)
            printf("%d %d %d",maxsum,0,0);
        else
            printf("%d %d %d",maxsum,a[0],a[K-1]);
    }
    return 0;
}
```
# 一元多项式加法与乘法
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1500420206048747520](https://pintia.cn/problem-sets/1497448825169559552/problems/1500420206048747520)
这道题主要是考察使用结构体数组或者链表来表示多项式，跟上一题一样，要考虑一些极端情况（比如同类项合并与抵消等等）。这里的多项式乘法我选择采用了**相乘+冒泡排序+同类项合并**的方法，不知道有没有童鞋有更快的实现方法。
代码如下：
```c
#include<stdio.h>
#include<stdlib.h>
# define Maxisize 1001

struct node {
    int xishu;
    int zhishu;
};
typedef struct node part;

int main() {
    int k1, k2;
    part array1[Maxisize];
    part array2[Maxisize];
    part array4[100*Maxisize];
    part array5[100*Maxisize];

    //读取两个多项式并用结构体数组表示
    scanf("%d",&k1);
    //array1 = (part*)malloc(k1 * sizeof(part));
    for (int i = 0; i < k1; i++) {
        scanf("%d %d", &array1[i].xishu, &array1[i].zhishu);
        if(array1[i].xishu > 1000)
            array1[i].xishu = 1000;
        else if(array1[i].xishu < -1000)
            array1[i].xishu = -1000;
        if(array1[i].zhishu > 1000)
            array1[i].zhishu = 1000;
        else if(array1[i].zhishu < -1000)
            array1[i].zhishu = -1000;
        //printf("%d %d",array1[i].xishu,array1[i].zhishu);
        //printf("   ");
    }
    //printf("\n");
    scanf("%d",&k2);
    //array2 = (part*)malloc(k2 * sizeof(part));
    for (int i = 0; i < k2; i++) {
        scanf("%d %d", &array2[i].xishu, &array2[i].zhishu);
        if(array2[i].xishu > 1000)
            array2[i].xishu = 1000;
        else if(array2[i].xishu < -1000)
            array2[i].xishu = -1000;
        if(array2[i].zhishu > 1000)
            array2[i].zhishu = 1000;
        else if(array2[i].zhishu < -1000)
            array2[i].zhishu = -1000;
        //printf("%d %d",array2[i].xishu,array2[i].zhishu);
        //printf("   ");
    }

    //两个多项式相乘
    int end4;
    end4 = 0;
    //array4 = (part*)malloc(k1*k2*sizeof(part));
    for (int i = 0; i < k1; i++) {
        for (int j = 0; j < k2; j++) {
            array4[end4].xishu = array1[i].xishu * array2[j].xishu;
            array4[end4].zhishu = array1[i].zhishu + array2[j].zhishu;
            end4++;
        }
    }
   
    //冒泡排序
    int flag;
    int tempxishu, tempzhishu;
    while (1) {
        flag = 0;
        for (int i = 0; i < end4 - 1; i++) {
            for (int j = i; j < end4 - 1; j++) {
                if (array4[j].zhishu < array4[j + 1].zhishu) {
                    tempxishu = array4[j].xishu;
                    tempzhishu = array4[j].zhishu;
                    array4[j].xishu = array4[j + 1].xishu;
                    array4[j].zhishu = array4[j + 1].zhishu;
                    array4[j + 1].xishu = tempxishu;
                    array4[j + 1].zhishu = tempzhishu;
                    if (flag == 0)
                        flag = 1;
                }
            }
        }
        if (flag == 0)
            break;
    }
    //printf("%d ", end4);
    //for (int i = 0; i < end4; i++) {
    //    printf("%d %d", array4[i].xishu, array4[i].zhishu);
    //    printf("   ");
    //}
    //printf("\n");
    
    //将相同系数的项合并
    int end5 = 0;
    int k4 = 0;
    //array5 = (part*)malloc(k1*k2*sizeof(part));
    while (k4 != k1 * k2) {
        int tempxs = array4[k4].xishu;
        int tempzs = array4[k4].zhishu;
        while (array4[k4].zhishu == tempzs) {
            k4++;
            if (array4[k4].zhishu == tempzs) {
                tempxs += array4[k4].xishu;
            }
        }
        if(tempxs != 0){
            array5[end5].xishu = tempxs;
            array5[end5].zhishu = tempzs;
            end5++;
        }
    }
    //printf("%d ", end5);
    if(end5 == 0){
        printf("0 0");
    }
    else{
        for (int i = 0; i < end5; i++) {
            if(i!=0)
                printf(" ");
            printf("%d %d", array5[i].xishu, array5[i].zhishu);
        }
    }
    printf("\n");

    //两个多项式相加
    int start1, start2;
    start1 = start2 = 0;
    int end3 = 0;
    part array3[2*Maxisize];
    //array3 = (part*)malloc((k1+k2)*sizeof(part));
    while (start1 != k1 && start2 != k2) {
        if (array1[start1].zhishu > array2[start2].zhishu) {
            if(array1[start1].xishu != 0){
                array3[end3].xishu = array1[start1].xishu;
                array3[end3].zhishu = array1[start1].zhishu;
                end3++;
            }
            start1++;
        }
        else if (array1[start1].zhishu < array2[start2].zhishu) {
            if(array2[start2].xishu != 0){
                array3[end3].xishu = array2[start2].xishu;
                array3[end3].zhishu = array2[start2].zhishu;
                end3++;
            }
            start2++;
        }
        else {
            if((array1[start1].xishu + array2[start2].xishu) != 0){
                array3[end3].xishu = array1[start1].xishu + array2[start2].xishu;
                array3[end3].zhishu = array1[start1].zhishu;
                end3++;
            }
            start1++;
            start2++;
        }
    }
    for (; start1 < k1; start1++) {
        if(array1[start1].xishu != 0){
            array3[end3].xishu = array1[start1].xishu;
            array3[end3].zhishu = array1[start1].zhishu;
            end3++;
        }
    }
    for (; start2 < k2; start2++) {
        if(array2[start2].xishu != 0){
            array3[end3].xishu = array2[start2].xishu;
            array3[end3].zhishu = array2[start2].zhishu;
            end3++;
        }
    }
    //printf("%d ", end3);
    if(end3 == 0){
        printf("0 0");
    }
    else{
        for (int i = 0; i < end3; i++) {
            if(i!=0)
                printf(" ");
            printf("%d %d", array3[i].xishu, array3[i].zhishu);
        }
    }
    printf("\n");
    return 0;
}
```
# Reversing Linked List
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1500420206048747521](https://pintia.cn/problem-sets/1497448825169559552/problems/1500420206048747521)
这道题真的是把我折磨的不行，用了两种方法，还是没有AC。两种方法分别是链表和用结构体数组表示的抽象的“链表”（这题给了我特别大的影响就是在这里，就是用数组也可以表示链表，这点在后面树的表示中也有用到），不知道为神马运行超时，研究ing。。
## 链表法
链表法代码如下：
```cpp
#include<iostream>
#include<vector>
#include<string>
using namespace std;

class Node
{
    private:
    int value;
    string address;
    string nextaddress;
    Node* next;
    public:
    Node()
    {
        value = 0;
        next = NULL;
    }
    int Getvalue() { return this->value; }
    Node* Getnext() { return this->next; }
    string Getaddress(){return this->address;}
    string Getnextaddress(){return this->nextaddress;}
    void Setvalue(int num) { value = num; }
    void Setnext(Node* a) { next = a; }
    void Setaddress(string a){ address = a;}
    void Setnextaddress(string a){ nextaddress = a;}
};

class Nodearray
{
    private:
    Node* head;
    Node* tail;
    public:
    Nodearray()
    {
        head = NULL;
        tail = NULL;
    }
    void Addnum(string address,int num,string nextaddress)
    {
        if (head == NULL)
        {
            head = new Node;
            head->Setvalue(num);
            head->Setaddress(address);
            head->Setnextaddress(nextaddress);
            head->Setnext(NULL);
            tail = head;
        }
        else
        {
            tail->Setnext(new Node);
            (tail->Getnext())->Setvalue(num);
            (tail->Getnext())->Setaddress(address);
            (tail->Getnext())->Setnextaddress(nextaddress);
            (tail->Getnext())->Setnext(NULL);
            tail = tail->Getnext();
        }
    }
    void Display()
    {
        Node* p = head;
        if (head == NULL) return;
        else
        {
            while (p != NULL)
            {
                cout << p->Getaddress() << " ";
                cout << p->Getvalue() << " ";
                cout << p->Getnextaddress();
                p = p->Getnext();
                cout<<endl;
            }
        }
        //cout << endl;
    }
    void Getnum(int pos)
    {
        int count = 1;
        Node* p = head;
        while (p != NULL)
        {
            if (count == pos)
            {
                cout << "找到了！ ";
                cout << p->Getvalue() << endl;
                return;
            }
            p = p->Getnext();
            count++;
        }
        cout << "没找到！" << endl;
    }
    void Getaddr(int pos)
    {
        int count = 1;
        Node* p = head;
        while (p != NULL)
        {
            if (count == pos)
            {
                cout << "找到了！ ";
                cout << p->Getaddress() << endl;
                return;
            }
            p = p->Getnext();
            count++;
        }
        cout << "没找到！" << endl;
    }
    void Getnextaddr(int pos)
    {
        int count = 1;
        Node* p = head;
        while (p != NULL)
        {
            if (count == pos)
            {
                cout << "找到了！ ";
                cout << p->Getnextaddress() << endl;
                return;
            }
            p = p->Getnext();
            count++;
        }
        cout << "没找到！" << endl;
    }
    void Delnum(int num)
    {
        Node* p1 = head;
        Node* p2 = head;
        if (head == NULL)
        {
            cout << "这是空列表" << endl;
            return;
        }
        else
        {
            if (head->Getvalue() == num)
            {
                head = head->Getnext();
                return;
            }
            else
            {
                p2 = p2->Getnext();
            }
            while (p2 != NULL)
            {
                if (p2->Getvalue() == num)
                {
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

int main(){
    //读入输入部分
    string start;
    cin>>start;
    int length;
    cin>>length;
    if(length > 1e5)
        length = 1e5;
    int K;
    cin>>K;
    if(K > length)
        K = length;
    int *array2;
    string *array1,*array3;
    array1 = new string[length];
    array2 = new int[length];
    array3 = new string[length];
    for(int i=0;i<length;i++){
        cin>>array1[i]>>array2[i]>>array3[i];
    }
    
    int *array2_2;
    string *array1_2,*array3_2;
    array1_2 = new string[length];
    array2_2 = new int[length];
    array3_2 = new string[length];
    
    //先写正常顺序
    int k=0;
    string standard = start;
    //这么写当N很大时要找很久，太低效
    while(k!=length){
        for(int i=0;i<length;i++){
            if(array1[i] == standard){
                array1_2[k] = array1[i];
                array2_2[k] = array2[i];
                array3_2[k] = array3[i];
                standard = array3[i];
                break;
            }
        }
        k++;
    }
    //for(int i=0;i<length;i++){
    //    cout<<array1_2[i]<<" "<<array2_2[i]<<" "<<array3_2[i]<<endl;
    //}
    if(K == length){
        array3_2[0] = array3_2[length-1];
        for(int i=K-1;i>0;i--)
            array3_2[i] = array1_2[i-1]; 
    }
    else if(K == 1){
        for(int i=K-1;i>0;i--)
            array3_2[i] = array1_2[i-1];
        array3_2[0] = array1_2[K];
    }
    else if(length % K == 0){
        int group = length/K;
        for(int j = group;j>0;j--){
            if(j != 1)
                array3_2[(j-1)*K - 1] = array3_2[j*K - 2];
            array3_2[(j-1)*K] = array3_2[j*K - 1];
            for(int i=K-1;i>0;i--)
                array3_2[(j-1)*K+i] = array1_2[(j-1)*K+i-1];
        }
    }
    else{
        int group = length/K;
        //int yushu = length%K;
        for(int j = group;j>0;j--){
            if(j != 1)
                array3_2[(j-1)*K - 1] = array3_2[j*K - 2];
            array3_2[(j-1)*K] = array3_2[j*K - 1];
            for(int i=K-1;i>0;i--)
                array3_2[(j-1)*K+i] = array1_2[(j-1)*K+i-1];
        }
    }
    
    //存储成与K相关的Reversed链表形式
    Nodearray a;
    if(K==1){
        for(int i=0;i<length;i++){
            a.Addnum(array1_2[i],array2_2[i],array3_2[i]);
        }
    }
    else if(K==length){
        for(int i=0;i<length;i++){
            a.Addnum(array1_2[length-i-1],array2_2[length-i-1],array3_2[length-i-1]);
        }
    }
    else if(length % K == 0){
        int group2 = length/K;
        for(int i = 0;i < group2;i++){
            for(int j = K;j > 0;j--){
                a.Addnum(array1_2[i*K+j-1],array2_2[i*K+j-1],array3_2[i*K+j-1]);
            }
        }
    }
    else{
        int group2 = length/K;
        for(int i = 0;i < group2;i++){
            for(int j = K;j > 0;j--){
                a.Addnum(array1_2[i*K+j-1],array2_2[i*K+j-1],array3_2[i*K+j-1]);
            }
        }
        for(int i=K*group2;i<length;i++){
            a.Addnum(array1_2[i],array2_2[i],array3_2[i]);
        }
    }
    a.Display();
    return 0;
}
```
## 结构体数组法
结构体数组法代码如下：
```cpp
#include<iostream>
#include<vector>
#include<string>
#include<cstdlib>
#define Maxsize 100000
using namespace std;

class Node{
public:
    string thisaddr;
    int value;
    string nextaddr;
};

Node array[Maxsize];
int findaddr(int count,string start){
    int tempstart;
    tempstart = stoi(start);
    int temp = 0;
    int temp2 = tempstart;
    while(temp != count){
        temp2 = stoi(array[temp2].nextaddr);
        temp++;
    }
    return temp2;
}

int findlen(string start){
    int count = 0;
    int output2 = stoi(start);
    while(true){
        //cout<<array[output2].thisaddr<<" "<<array[output2].value<<" "<<array[output2].nextaddr<<endl;
        if(array[output2].nextaddr != "-1"){
            output2 = stoi(array[output2].nextaddr);
            count++;
        }
        else{
            count++;
            break;
        }
        //cout<<output2<<endl;
    }
    return count;
    
}

int main(){
    //读入输入部分
    string start;
    cin>>start;
    //cout<<start<<endl;
    int length;
    cin>>length;
    if(length > Maxsize)
        length = Maxsize;
    int K;
    cin>>K;
    if(K > length)
        K = length;
    
    
    string temp,temp2;
    int index = 0;
    //cout<<start<<endl;
    for(int i=0;i<length;i++){
        cin>>temp;
        index = stoi(temp);
        array[index].thisaddr = temp;
        cin>>array[index].value;
        cin>>array[index].nextaddr;
        //if(K == 1){
        //    cout<<array[index].thisaddr<<" "<<array[index].value<<" "<<array[index].nextaddr<<endl;
        //}
    }
    
    //根据链表长度判断有无多余节点
    //并更改长度和K的数值
    int len = findlen(start);
    length = len;
    if(K > length)
        K = length;

    //进行链表的逆转
    int tempstart = 0;
    int tempstart2 = 0;
    int truestart = 0;
    int a = 0;
    if(K == 1){
        int output1;
        output1 = stoi(start);
        //cout<<output1;
        int count = 0;
        while(count != length){
            cout<<array[output1].thisaddr<<" "<<array[output1].value<<" "<<array[output1].nextaddr<<endl;
            output1 = stoi(array[output1].nextaddr);
            count++;
        }
        return 0;
    }
    else{
        int counts,temps2,news,olds,variable;
        int group = length/K;
        for(int i = 0;i<group;i++){
            tempstart = findaddr(i,start);
            //cout<<tempstart<<endl;
            news = tempstart;
            olds = findaddr(i+1,start);
            //cout<<olds<<endl;
            temps2 = findaddr(i+2,start);
            //cout<<temps2<<endl;
            variable = findaddr(i+K-1,start);
            if(i != 0){
                array[tempstart2].nextaddr = array[a].thisaddr;
                //cout<<array[tempstart2].nextaddr<<endl;
                //cout<<"a";
            }
            array[tempstart].nextaddr = array[variable].nextaddr;
            if(i == 0)
                truestart = variable;
            counts = 0;
            while(counts != K-1){
                array[olds].nextaddr = array[news].thisaddr;
                news = olds;
                olds = temps2;
                if(array[temps2].nextaddr != "-1")
                    temps2 = stoi(array[temps2].nextaddr);
                counts++;
            }
            if(i != (group-1)){
                a = findaddr(i+K,start);
                tempstart2 = tempstart;
                //cout<<tempstart2<<endl;
                //cout<<a<<endl;
            }
        }
    }
    //结果输出
    int output2 = truestart;
    //cout<<truestart;
    int count = 0;
    while(count != length){
        cout<<array[output2].thisaddr<<" "<<array[output2].value<<" "<<array[output2].nextaddr<<endl;
        output2 = stoi(array[output2].nextaddr);
        count++;
        //cout<<output2<<endl;
    }
    return 0;
    //cout<<output2<<endl;
    
}
```
# Pop Sequence
链接：[https://pintia.cn/problem-sets/1497448825169559552/problems/1500420206048747522](https://pintia.cn/problem-sets/1497448825169559552/problems/1500420206048747522)
这道题大概花了我40分钟就AC了，难度真的很小（毕竟是堆栈，堆栈在C++中可以直接用vector容器轻松实现），关键是要想清楚判定序列能出现与否的方法。就是先push到指定序列，再pop，直到不能pop之后再进行一个判断，如果判断已经无法满足指定输出序列了，就直接跳出，say no，否则继续push到指定序列的下一个数字，周而复始。
代码如下：
```cpp
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<vector>

using namespace std;

int main(){
    int M,N,K;
    cin>>M>>N>>K;
    int *array;
    array = new int[N];
    vector<int> stack;
    for(int i=0;i<K;i++){
        //清空上一轮的存储结果
        for(int j=0;j<N;j++)
            cin>>array[j];
        stack.clear();
        int poppointer = 0;
        int pushpointer = 1;
        int flag = 1;
        //开始执行
        while(true){
            //push阶段，只要两个不一样且堆栈没爆我就push
            if(pushpointer!=(array[poppointer]+1) && stack.size() < M){
                stack.push_back(pushpointer++);
            }
            //如果发现要pop的值已经被压在栈里且不是最后一个元素，就直接NO
            else if(pushpointer > array[poppointer]){
                flag = 0;
                break;
            }
            //如果发现堆栈满了，且没法pop元素，直接NO
            if(stack.size() == M && pushpointer!=(array[poppointer]+1)){
                flag = 0;
                break;
            }
            //pop阶段，只要队堆栈顶符合pop队列，就一直pop，知道没法pop为止
            while(stack.back() == array[poppointer]){
                stack.pop_back();
                if(poppointer!=N-1)
                    poppointer++;
            }
            //如果所有程序全部走完，说明可以满足给定的pop序列，返回YES
            if(pushpointer == N+1 && poppointer == N-1){
                break;
            }
        }
        if(flag == 0)
            cout<<"NO"<<endl;
        else
            cout<<"YES"<<endl;
    }
    return 0;
}

```
# 总结
这一次的刷题练习完成的题目并不多，但是从这短短几道题目中我发现了一个之前一直被自己忽视的关键点：写算法不仅只是实现就好了，更要注重代码的规范、代码的鲁棒性以及注释等等，这些东西对于一个好的程序员来说必不可少，这也才是代码部分的魅力所在。所以以后再写代码时，我会注意：一是灵感的迸发和大脑的思考，二是养成规范的代码风格和代码习惯。
