天空一声巨响，迪宝闪亮登场。哈哈哈数据结构刷题又来了。实践是检验真理的唯一标准，最近感觉到自己的代码功底越来越深了，看来多写代码确实很有帮助。这次又带来最新的三道题目，来跟我一起去看看把。~~（顺便提一下，现在我在PTA上浙大《数据结构》这个题库已经排名1/2883了哦！！！）~~
# 排序1 排序
这道题主要是用来巩固各种排序算法的代码基础，基本的排序算法有如下几种：

   - 冒泡排序
   - 插入排序
   - 堆排序/选择排序
   - 希尔排序
   - 归并排序
   - ~~拓扑排序~~

下面我想用各种排序算法完成此题，然后对比下不同排序算法的：**1）代码量、2）时间和空间复杂度以及3）各种排序算法在不同数据集上的适用性**。
## 冒泡排序
冒泡排序代码如下：
```cpp
#include<iostream>

using namespace std;
int N,tempN;
long int *H;
int flag = 1;

void Swap(long int &a, long int &b);

int main(){
    cin >> N;
    H = new long int[N];
    for(int i = 0;i < N;i++)
        cin >> H[i];
    while(flag == 1){
        flag = 0;
        for(int i = 0;i < N - 1;i++){
            for(int j = i;j < N - 1;j++){
                if(H[j] > H[j+1]){
                    Swap(H[j],H[j+1]);
                    if(flag == 0)
                        flag = 1;
                }
            }
        }
    }
    cout<<H[0];
    for(int i = 1;i < N;i++)
        cout<<' '<<H[i];
    return 0;
}

void Swap(long int &a, long int &b){
    long int temp1;
    temp1 = a;
    a = b;
    b = temp1;
}
```
## 选择排序
选择排序代码如下：
```cpp
#include<iostream>

using namespace std;
int N;
long int *H;
long int temp;

int main(){
    cin >> N;
    H = new long int[N];
    for(int i = 0;i < N;i++){
        cin >> temp;
        if(i == 0){
            H[i] = temp;
            continue;
        }
        for(int j = 0;j < i;j++){
            if(temp < H[j]){
                for(int k = i;k > j ;k--)
                    H[k] = H[k-1];
                H[j] = temp;
                break;
            }
            else if(j == i-1)
                H[i] = temp;
        }
    }
    cout<<H[0];
    for(int i = 1;i < N;i++)
        cout<<' '<<H[i];
    return 0;
}
```
## 堆排序/选择排序
堆排序代码如下：
```cpp
#include<iostream>

using namespace std;
int N,tempN;
long int *H;

void Swap(long int &a, long int &b);
void CreateHeap(int index);
void DeleteHeap(int index);

int main(){
    cin >> N;
    tempN = N;
    H =  new long int[N+1];
    for(int i = 1;i <= N;i++){
        cin >> H[i];
        CreateHeap(i);
    }
    tempN = N;
    int count = 0;
    while(tempN != 0){
        if(count == 0)
            count++;
        else
            cout<<' ';
        cout<<H[1];
        DeleteHeap(1);
    }
    return 0;
}

void Swap(long int &a, long int &b){
    long int temp1;
    temp1 = a;
    a = b;
    b = temp1;
}

void CreateHeap(int index){
    //建堆，插入新元素后调整成堆
    int temp = index;
    for(;temp != 1;temp = temp/2){
        if(H[temp] < H[temp/2])
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
        tempN--;
        return;
    }
    if(index == 1){
        H[1] = H[Ms];
        tempN--;
    }
    //从两个子节点中找出比现在这个元素小的，交换位置并迭代函数
    //下面考虑了两种节点的多种情况（可能为空）
    if(2*index <= tempN && 2*index+1 <= tempN){
        if(H[2*index] <= H[2*index+1] && H[2*index] < H[index]){
            Swap(H[index],H[2*index]);
            DeleteHeap(2*index);
        }
        else if(H[2*index] >= H[2*index+1] && H[2*index+1] < H[index]){
            Swap(H[index],H[2*index+1]);
            DeleteHeap(2*index+1);
        }
        else
            return;
    }
    else if(2*index <= tempN && 2*index+1 > tempN){
        if(H[2*index] < H[index]){
            Swap(H[index],H[2*index]);
            DeleteHeap(2*index);
        }
        else
            return;
    }
    else
        return;
}
```
## 希尔排序
希尔排序代码如下所示：
```cpp
#include<iostream>
#include<cmath>

using namespace std;
int N,D;
long int *H;
long int temp;

int main(){
    cin >> N;
    H = new long int[N];
    int count = 1;
    while((int)pow(2,count) - 1 < N)
        count++;
    count--;
    for(int i = 0;i < N;i++)
        cin >> H[i];
    for(int i = count;i > 0;i--){
        D = (int)pow(2,i) - 1;
        for(int j = 0;j < D;j++){
            for(int k = j;k < N;k += D){
                if(k == j)
                    continue;
                temp = H[k];
                for(int p = j;p < k;p += D){
                    if(temp < H[p]){
                        for(int s = k;s > p;s -= D)
                            H[s] = H[s-D];
                        H[p] = temp;
                        break;
                    }
                }
            }
        }
    }
    cout<<H[0];
    for(int i = 1;i < N;i++)
        cout<<' '<<H[i];
    return 0;
}

```
## 归并排序
归并排序代码如下所示：
```cpp
#include<iostream>

using namespace std;
int N;
int length = 1;
long int *H,*tempH;
long int temp;

void Mergesort(long int *a,long int*b);
void Merge(int start,int end,long int *a,long int*b);

int main(){
    cin >> N;
    H = new long int[N];
    tempH = new long int[N];
    for(int i = 0;i < N;i++)
        cin >> H[i];
    while(length <= N){
        Mergesort(H,tempH);
        length *= 2;
        Mergesort(tempH,H);
        length *= 2;
    }
    cout<<H[0];
    for(int i = 1;i < N;i++)
        cout<<' '<<H[i];
    return 0;
}

void Mergesort(long int *a,long int*b){
    int i = 0;
    for(;i < N - 2 * length;i += 2 * length)
        Merge(i,i+2*length-1,a,b);
    Merge(i,N-1,a,b);
    //cout<<b[0];
    //for(int i = 1;i < N;i++)
    //    cout<<' '<<b[i];
    //cout<<endl;
}

void Merge(int start,int end,long int *a,long int*b){
    if(end - start + 1 <= length){
        for(int i = start;i <= end;i++)
            b[i] = a[i];
        return;
    }
    int leftend = start + length -1;
    int l = start;
    int r = start + length;
    while(l <= leftend && r <= end){
        if(a[l] < a[r])
            b[start++] = a[l++];
        else
            b[start++] = a[r++];
    }
    while(l <= leftend)
        b[start++] = a[l++];
    while(r <= end)
        b[start++] = a[r++];
}
```
# 不同排序算法的对比
仅仅会写代码怎么行！真正的程序员还要懂得分析，那下面就让我们来分析分析。
### 代码量
下面是五种排序方法各自的代码量：

|  | 代码量（行数） |
| --- | --- |
| 冒泡排序 | 38 |
| 选择排序 | 32 |
| 堆排序 | 87 |
| 希尔排序 | 41 |
| 归并排序 | 60 |

不难看出，越复杂的方法需要的代码量也越多，但是于此同时时间复杂度也越好。但是在这里我们有个极大的例外——希尔排序，已经有研究表明，希尔排序大多数情况下是这五种排序方法最好的之一，基本和堆排序不相上下，尤其是使用Sedgewick增量序列的希尔排序，效果出奇的好。
### 时间复杂度
时间复杂度这里就直接甩结论把：

|  | 最坏时间复杂度 |
| --- | --- |
| 冒泡排序 | $O(N^2)$ |
| 选择排序 | $O(N^2)$ |
| 堆排序 | $O(NlogN)$ |
| 希尔排序 | $O(NlogN)$ |
| 归并排序 | $O(NlogN)$ |

### 空间复杂度
空间复杂度的分析较为复杂，因为每种排序方法自己可能也有多种实现方式，例如说归并排序就有递归和非递归两种方法，多种方法的空间复杂度各不相同，所以很难进行分析。这里就把每种排序算法的运行图放一下把：
冒泡排序：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2131041/1651065054162-b02ae523-c506-4468-bfbc-c8b9fc51c545.png#clientId=u43a0b7f3-815d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=689&id=u5d41f859&margin=%5Bobject%20Object%5D&name=image.png&originHeight=861&originWidth=1147&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72870&status=done&style=none&taskId=uedf23b27-23ce-4dbe-901c-147d2e1baa9&title=&width=917.6)
选择排序：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2131041/1651065101937-4b170723-fe66-4895-b5f3-e9bfc9383239.png#clientId=u43a0b7f3-815d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=690&id=u087cfb58&margin=%5Bobject%20Object%5D&name=image.png&originHeight=862&originWidth=1163&originalType=binary&ratio=1&rotation=0&showTitle=false&size=74848&status=done&style=none&taskId=u57552b96-f567-4d80-8ee9-4fa62161005&title=&width=930.4)
堆排序：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2131041/1651065144600-e76e3069-54d8-477b-9e0d-b43181092098.png#clientId=u43a0b7f3-815d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=682&id=ub54b57f1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=853&originWidth=1152&originalType=binary&ratio=1&rotation=0&showTitle=false&size=73113&status=done&style=none&taskId=u943dce40-921c-4ef2-8f35-b86323aa167&title=&width=921.6)
希尔排序：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2131041/1651065211636-945c0d52-3681-46ee-af8e-2ecc40052d1a.png#clientId=u43a0b7f3-815d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=682&id=u7f37bf30&margin=%5Bobject%20Object%5D&name=image.png&originHeight=852&originWidth=1162&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75399&status=done&style=none&taskId=u6d0570d9-811c-4f47-a571-7b0e53bff58&title=&width=929.6)
归并排序：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/2131041/1651064932813-f5cbac4d-3bdb-4478-a5cf-8747954f4d0a.png#clientId=u43a0b7f3-815d-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=683&id=u15a24ded&margin=%5Bobject%20Object%5D&name=image.png&originHeight=854&originWidth=1154&originalType=binary&ratio=1&rotation=0&showTitle=false&size=75765&status=done&style=none&taskId=ucfb69559-f171-4bcc-b37f-37295eacf01&title=&width=923.2)
### 适用场景
先来说说冒泡排序和选择排序，这两个排序算法确实比较低效，但是他们的好处是没有用到很多额外的存储空间；再来看看堆排序，堆排序是个非常好用的算法，而且有堆这种数据结构作为基础，堆排序的不好之一是需要比较多的额外空间（尤其没有处理好的时候）；希尔排序是个不错的方法，效率仅次于堆排序，而且不需要多余的额外存储空间，代码量也比较少，代码较好理解；而归并排序的话常常用于外排序，内排序不经常用到。
# Insert or Merge
这道题主要是需要大家发现插入排序和归并排序的特点，我推荐：因为是做二元判决，所以可以判断是不是插入排序，这个比较容易判断，如果不是插入的话就是归并，然后用上一题写的排序代码再进行一轮就行了。我大概是55分钟AC了，最后一些边界测试需要小心。
此题代码如下：
```cpp
#include<iostream>

using namespace std;
int N;
int *a,*b;
int length;

int checkd();
void Mergesort(int *a,int*b);
void Merge(int start,int end,int *a,int*b);

int main(){
    cin >> N;
    a = new int[N];
    b = new int[N];
    int pos1,pos2,temp;
    //插入排序中前面是有序的，后面是相同的
    pos1 = pos2 = -1;
    for(int i = 0;i < N;i++)
        cin >> a[i];
    for(int i = 0;i < N;i++){
        cin >> b[i];
        if(i != 0 && b[i] < b[i-1] && pos1 == -1)
            pos1 = i - 1;
        if(b[i] == a[i] && pos2 == -1 && pos1 != -1)
            pos2 = i;
        if(pos2 != -1 && b[i] != a[i])
            pos2 = -1;
    }
    //cout<<pos1<<" "<<pos2<<endl;
    if(pos1 + 1 == pos2){
        cout<<"Insertion Sort"<<endl;
        //再进行一轮插入排序
        temp = b[pos2];
        for(int i = 0;i < pos2;i++){
            if(temp < b[i]){
                for(int j = pos2;j > i;j--)
                    b[j] = b[j-1];
                b[i] = temp;
                break;
            }
        }
        cout<<b[0];
        for(int i = 1;i < N;i++)
            cout<<' '<<b[i];
    }
    else{
        cout<<"Merge Sort"<<endl;
        //先判断进行到第几轮
        int D = checkd();
        int *tempb;
        tempb = new int[N];
        length = D;
        //cout<<length<<endl;
        //然后再进行一轮归并排序
        Mergesort(b,tempb);
        cout<<tempb[0];
        for(int i = 1;i < N;i++)
            cout<<' '<<tempb[i];
    }
    return 0;
}

//判断归并排序的当前增量D
int checkd(){
    int gap = -1;
    int tempgap = 0;
    int i = 0;
    for(;i < N;i++){
        if(tempgap == 0){
            tempgap++;
            //cout<<gap<<endl;
            continue;
        }
        if(b[i] >= b[i-1])
            tempgap++;
        else{
            if(gap == -1)
                gap = tempgap;
            else if(tempgap < gap)
                gap = tempgap;
            tempgap = 1;
        }
        //cout<<gap<<endl;
    }
    return gap;
}

//归并排序
void Mergesort(int *a,int*b){
    int i = 0;
    for(;i < N - 2 * length;i += 2 * length)
        Merge(i,i+2*length-1,a,b);
    Merge(i,N-1,a,b);
    //cout<<b[0];
    //for(int i = 1;i < N;i++)
    //    cout<<' '<<b[i];
    //cout<<endl;
}

//两个有序子列的归并
void Merge(int start,int end,int *input1,int*input2){
    if(end - start + 1 <= length){
        for(int i = start;i <= end;i++)
            input2[i] = input1[i];
        return;
    }
    int leftend = start + length -1;
    int l = start;
    int r = start + length;
    while(l <= leftend && r <= end){
        if(input1[l] < input1[r])
            input2[start++] = input1[l++];
        else
            input2[start++] = input1[r++];
    }
    while(l <= leftend)
        input2[start++] = input1[l++];
    while(r <= end)
        input2[start++] = input1[r++];
}
```
# Insertion or Heap Sort
这道题基本是上一题的翻版，而且更加简单，判断出是堆排序后**直接再进行一次删除最大堆的堆顶元素+调整堆**即可，23分钟左右一次就AC。
此题代码如下：
```cpp
#include<iostream>

using namespace std;
int N,num,tempN;
int *a,*b,*H;

int checknum();
void Swap(int &a, int &b);
void CreateHeap(int index);
void DeleteHeap(int index);

int main(){
    cin >> N;
    tempN = N;
    a = new int[N];
    b = new int[N];
    H = new int[N+1];
    int pos1,pos2,temp;
    pos1 = pos2 = -1;
    for(int i = 0;i < N;i++)
        cin >> a[i];
    for(int i = 0;i < N;i++){
        cin >> b[i];
        if(i != 0 && b[i] < b[i-1] && pos1 == -1)
            pos1 = i - 1;
        if(b[i] == a[i] && pos2 == -1 && pos1 != -1)
            pos2 = i;
        if(pos2 != -1 && b[i] != a[i])
            pos2 = -1;
    }
    //cout<<pos1<<" "<<pos2<<endl;
    if(pos1 + 1 == pos2){
        cout<<"Insertion Sort"<<endl;
        temp = b[pos2];
        for(int i = 0;i < pos2;i++){
            if(temp < b[i]){
                for(int j = pos2;j > i;j--)
                    b[j] = b[j-1];
                b[i] = temp;
                break;
            }
        }
        cout<<b[0];
        for(int i = 1;i < N;i++)
            cout<<' '<<b[i];
    }
    else{
        cout<<"Heap Sort"<<endl;
        num = checknum();
        tempN = N - num;
        for(int i = 1;i <= tempN;i++)
            H[i] = b[i-1];
        DeleteHeap(1);
        cout<<H[1];
        for(int i = 2;i <= tempN;i++)
            cout<<' '<<H[i];
        cout<<' '<<b[0];
        for(int i = N - num;i < N;i++)
            cout<<' '<<b[i];
    }
    return 0;
}

int checknum(){
    int i = 1;
    for(;i < N;i++){
        if(b[i] > b[0])
            break;
    }
    return N - i;
}

void Swap(int &a, int &b){
    int temp1;
    temp1 = a;
    a = b;
    b = temp1;
}

void CreateHeap(int index){
    //建堆，插入新元素后调整成堆
    int temp = index;
    for(;temp != 1;temp = temp/2){
        if(H[temp] > H[temp/2])
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
        tempN--;
        return;
    }
    if(index == 1){
        H[1] = H[Ms];
        tempN--;
    }
    //从两个子节点中找出比现在这个元素小的，交换位置并迭代函数
    //下面考虑了两种节点的多种情况（可能为空）
    if(2*index <= tempN && 2*index+1 <= tempN){
        if(H[2*index] >= H[2*index+1] && H[2*index] > H[index]){
            Swap(H[index],H[2*index]);
            DeleteHeap(2*index);
        }
        else if(H[2*index] <= H[2*index+1] && H[2*index+1] > H[index]){
            Swap(H[index],H[2*index+1]);
            DeleteHeap(2*index+1);
        }
        else
            return;
    }
    else if(2*index <= tempN && 2*index+1 > tempN){
        if(H[2*index] > H[index]){
            Swap(H[index],H[2*index]);
            DeleteHeap(2*index);
        }
        else
            return;
    }
    else
        return;
}

```
# 总结
这次的刷题记录是排序专场，我们学习到了五种排序方法，五种方法各有千秋，强烈建议五种排序方法大家都手写一次，这样子以后用到的时候直接CV就行（这也是为自己的代码库增加新代码的方式嘛！）。通过这次刷题，我又获得了新的体悟：1）**你现在写的代码有可能可以为将来节省不少时间**，之后再遇到直接CV即可；2）在实现算法之前多想想，**不要只是急于实现，刷题到这个地步时也要想想如何高效实现，包括时间空间复杂度和代码量等等**。
欢迎对ECE/CS/AI感兴趣的小伙伴关注我，如果你对我的内容有什么建议的话，或者你也对算法和数据结构感兴趣的话，可以单独找我讨论，也欢迎在评论区留下你的声音。
