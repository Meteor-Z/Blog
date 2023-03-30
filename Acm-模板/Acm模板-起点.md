# 宏定义

``` c++
#include <bits/stdc++.h>
using namespace std;
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
#define eps 1e-8
#define gcd(a,b) __gcd(a,b)
#define lcm(a,b) a/gcd(a,b)*b
#define lowbit(x) (x&-x)
#define all(x) x.begin(), x.end()
#define debug(x...) do { cout<< #x <<" -> "; re_debug(x); } while (0)
void re_debug() { cout<<'\n'; }
template<class T, class... Ts> void re_debug(const T& arg,const Ts&... args) { cout<<arg<<" "; re_debug(args...); }
void cut(){ cout<<'\n'<<"------------"<<'\n';}
typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
const int INF=0x3f3f3f3f;
const ll LNF=0x3f3f3f3f3f3f3f3fll;
const double PI=acos(-1.0);
void solve()
{

}
int main()
{
    IOS;
    int T=1;
    cin>>T;
    while(T--) solve();
    return 0^0;
}
```
# 解绑

```c++
    //注意: 解绑之后不能使用getchar(),scanf(),printf等
    #define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
    IOS;
```
# 离散化

```c++
     //unique 将重复元素放在后面+1的位置然后到原来的数组后面然后后面全部抹掉 
     //先排序,再去重
    vector<int> q(n);
    sort(q.begin(),q.end());
    q.erase(unique(q.begin(),q.end()),q.end());
```

# 查询最大/最小值

```c++
    //返回的是地址,减去首地址,就是下标,时间复杂度O(n)
    vector<int> q;
    for(int i=1;i<100;i++) q.push_back(i);
    int maxn=max_element(q.begin(),q.end())-q.begin();
    int minn=min_element(q.begin(),q.end())-q.begin();
    cout<<q[maxn]<<' '<<q[minn]<<'\n';
```

# __int128

```c++
    //仅在linux上可以使用
    __int128 read()
    {
        __int128 x=0,f=1;
        char ch=getchar();
        while(!isdigit(ch)&&ch!='-')ch=getchar();
        if(ch=='-')f=-1;
        while(isdigit(ch))x=x*10+ch-'0',ch=getchar();
        return f*x;
    }

    void print(__int128 x)
    {
        if(x<0)putchar('-'),x=-x;
        if(x>9)print(x/10); 
        putchar(x%10+'0');
    }
```

# 统计1的个数

```c++
    //输出一个数字二进制中有多少个1
    //常数非常小
    int n=2;
    cout<<__builtin_popcount(n)<<'\n';
```

# 随机排列

```c++
    //先srand()一下,然后rand_shuffle()
    vector<int> q;
    for(int i=0;i<10;i++) q.push_back(i);
    srand(time(NULL));
    random_shuffle(q.begin(),q.end());
```

# Sringstream

```c++
    //过滤其中很多的回车, <题目: 估值一亿的AI核心代码>
    //请将同步流开启,否则不能读入,可以读很多,都能读
    //以字符串读入,但是可以读入数字等
    //ios::sync_with_stdio(false);
    //cin.tie(nullptr);
    //记住:要吞掉前面的空格
    string s;
    getline(cin,s);//可以使用cin.get() 代替,这样可以不取消同步流
    stringstream ssin(s);
    string t;
    vector<string> q;
    while(ssin>>t)
    {
        q.push_back(t);
    }
```

# 判断升序与降序

```c++
     vector<int> q;
    for(int i=100;i>=1;i--) q.push_back(i);
    if(is_sorted(q.begin(),q.end())) cout<<"判断升序"<<'\n';
    if(is_sorted(q.begin(),q.end(),greater<int>())) cout<<"判断降序"<<'\n';
```
# 旋转
```c++
    vector<int> q;//不太懂,建议手写
    for(int i=1;i<=10;i++) q.push_back(i);
    //三个参数: 开始,哪一个参数放在前面,终止 然后循环
    rotate(q.begin(),q.begin()+2,q.end());
    
```
# 大小写转换
```c++
    string s;
    cin>>s;
    for(int i=0;i<s.size();i++)
    {
        s[i]=char(toupper(s[i]));
        //或者
        //s[i]=char(tolower(s[i]));
    }
    cout<<s<<'\n';
```
# O2/O3优化
```c++
    #pragma GCC optimize(2)//建议只开O2就行了
    #pragma GCC optimize(3,"Ofase","inline")
```
# unordered_map的使用以及相关重载
```c++
  struct cmp
  {
	long long operator()(pair<int,int> x)const
    {
		return 1ll*x.first*521417+x.second;//return的就是相关重载,可以尽量写复杂点,
	}
  };

  unordered_map<pair<int,int>,int,cmp> umap;
```