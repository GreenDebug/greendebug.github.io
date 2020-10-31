# 算法algorithms橙书 摘录

本文算法用Java实现 持续更新。

linearithmic:NlogN linear:N

## 并查集 union-find
多个集合，将两个元素合并到一个集合（union），看两个元素是否在同一集合（find）

只需要一个数组记录每个元素所属集合号。

三种算法

Quick-find 这种算法每一次union都需要将一个集合中所有元素的记号改成另一个集合。所以他查找特别方便

Quick-union 这种算法union 特别方便，只要把一个元素的记号改成另一个。就形成一种类似树的结构。而find就比较麻烦，需要找到元素记号的根。

Weighted quick-union 这个算法在Quick-union上修改，因为Quick-union每次记号随便改的，这样树可能会特别长，不利于find。所以我们每次都把小的树插到大的树上。
三种算法的实现
~~~java
public class UF {
    private int[] id;
    private int size;
    private int count;
    private int[] sz;

    public UF(int N) {
        count = N;
        size = N;
        id = new int[N];
        for (int i = 0; i < size; i++) {
            id[i] = i;
        }
        sz = new int[N];
        for(int i = 0; i < size; i++) {
            sz[i] = 1;
        }
    }

    public int count() {
        return count;
    }

    public boolean connected(int p, int q) {
        return find(p) == find(q);
    }
    
    public int find(int p) {
        return id[p];
    }

    public void union(int p, int q) {
        int pID = find(p);
        int qID = find(q);
        if (pID == qID) {
            return;
        }
        for (int i = 0; i < size; i++) {
            if (id[i] == pID)
                id[i] = qID;
        }
        count--;
    }

    /**上面是Quick-find
     * 下面是Quick-union
     **/
    /*
     private find(int p){
         while(p!=id[p]) p = id[p];
         return p;
     }

    public void union(int p, int q) {
        int pRoot = find(p);
        int qRoot = find(q);
        if(pRoot == qRoot) return;
        id[pRoot] = qRoot;

        count--;
    }
    */
    //下面是weightedquick
    /*
    public int find(int p){
        while(p!=id[p]) p = id[p];
        return p;
    }
    public void union(int p,int q){
        int i =find(p);
        int j = find(q);
        if(i==j) return;
        if(sz[i]<sz[j]){
            id[i] = j;
            sz[j]+=sz[i];
        }else{
            id[j] = i;sz[i] +=sz[j];
        }count--;
    }
*/

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int N = in.nextInt();
        UF uf = new UF(N);
        while (!in.hasNext()) {
            int p = in.nextInt();
            int q = in.nextInt();
            if (uf.connected(p, q)) {
                continue;
            }
            uf.union(p, q);
            System.out.println(p + " " + q);
        }
        System.out.println(uf.count() + " components");
    }
}
~~~

做了下相关的大作业
![tupian](https://raw.githubusercontent.com/csjue/csjue.github.io/master/_posts/images/20201009224232.jpeg)

看着挺高端的，其实就是 一开始全是黑格。然后每次随机加一个蓝格，看有没有一条从上到下的路，如果没有则继续加点。

但是真正实现起来，还是有些工作量的。刚开始也没想到跟并查集有啥特别的关系。反而用迷宫来解可能还要简单一点。（题目还要求实现一些api，我感觉没啥用，而且还要求数组从1-N，搞得特别别扭

我只能用个 函数 把这个数组映射成一个一维数组。然后每次加一个点，则用 并查集 把 四周蓝点 union一下。 看有没有路，则看第一行与最后一行是否存在在一个集合的点。

大概写了一下午，262行。期间遇到好几个bug，程序一直运行不出来。

题目还要求 用Quick-find和Weighted quick-union比较。我觉得后者应该要牛逼一点。结果前者比后者快了大概7s
~~~bash
 Weighted quick-union
PS D:\daima\algorithms_homework> javac PercolationStats.java
PS D:\daima\algorithms_homework> java PercolationStats 200 100
Example values after creating PercolationStats(200,100)
mean()                                           0.5936469999999998
stddev()                                         0.010319878385862388
confidenceLo()                                   0.5916243038363708
confidenceHi()                                   0.5956696961636289
time                                             32793
 Quick-find
PS D:\daima\algorithms_homework> javac PercolationStats.java  
PS D:\daima\algorithms_homework> java PercolationStats 200 100
Example values after creating PercolationStats(200,100)
mean()                                           0.59072075
stddev()                                         0.009570728901423619
confidenceLo()                                   0.588844887135321
confidenceHi()                                   0.592596612864679
time                                             25510
~~~


## java 内存分析

| 内置类型 | 大小 bytes |
| -------- | ---------- |
| boolean  | 1          |
| char     | 2          |
| int      | 4          |
| float    | 4          |
| double   | 8          |
| long     | 8          |

object 一般有16B overhead  大小为8B的倍数

Integer 24B 16(overhead)+4(int)+4(padding)

Date 32B 16(over)+3×4(3 int)+4(padding)

Reference (可以理解成pointer 强reference即为class 类似cpp智能指针) 8B (64bit machine)

Node 40B = 16(over) + 2×8(reference item, next Node) + 8(extra overhead)

Stack Integer 32+64N; 

32 = 16(over) + 8(reference) +4(int) +4(padding)

64 = 40(Node) + 24(Integer)

Array header 24B= 16(over) + 4(int length) +4(padding)

Array int 24+4N

Array object Date 24+8N(reference)+32N(Date)

Array two-dimensional二维 M×N double 24B+8M(row's reference)+24M(row's header)+8MN(double)

Array two-dimensional二维 M×N Object 24+32M+8MN

String 64+2N = 40+(24+2N) = 8(reference) + 3×4(3 int offset length hash) + 16(over) +4(padding) +(24+2N) (char array)

subString 40B reuse char[] 

## 时间复杂度

tilde 保留系数

## 排序

### 快排

内循环短：优于mergesort   shell sort，它们需要移动元素在内循环

更少的比较

average1.29NlgN

best NlgN