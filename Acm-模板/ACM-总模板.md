[TOC]



# 起点

## 宏定义

```c++
#include <bits/stdc++.h>
using namespace std;
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0    //仅在linux上可以使用
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

#define eps 1e-8
#define int128 __int128
#define gcd(a,b) __gcd(a,b)
#define lcm(a,b) a/gcd(a,b)*b
#define lowbit(x) (x&-x)
#define all(x) x.begin(), x.end()
#define debug(x...) do { cout<< #x <<" -> "; re_debug(x); } while (0)
void re_debug() { cout<<'\n'; }
template<class T, class... Ts> void re_debug(const T& arg,const Ts&... args) { cout<<arg<<" "; re_debug(args...); }
int test=1;
void cut(){cout<<"test:"<<' '<<test++<<'\n';}
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
    return (0^0);
}
```

## __int128

```c++
//这里是因为头文件中#define了,请注意,不要开IOS
int128 read(){
    int128 x = 0, f = 1;
    char ch = getchar();
    while(ch < '0' || ch > '9'){
        if(ch == '-')
            f = -1;
        ch = getchar();
    }
    while(ch >= '0' && ch <= '9'){
        x = x * 10 + ch - '0';
        ch = getchar();
    }
    return x * f;
}
void print(int128 x){
    if(x < 0){
        putchar('-');
        x = -x;
    }
    if(x > 9)
        print(x / 10);
    putchar(x % 10 + '0');
}
```

## Stringstream

```c++
//1 不要开IOS
//2 记得要cin.get()
    string s;
    cin.get();
    getline(cin,s);
    stringstream ssin(s);
    string t;
    while(ssin>>t)
    {
        cout<<t<<'\n';
    }
```



## O2/O3 优化

```c++
#pragma GCC optimize(2)
#pragma GCC optimize(3)
```

## unordered_map的使用以及相关重载

```c++
struct cmp
{
	long long operator()(pair<int,int> x) const
	{
		return 1ll*x.first*5221417+x.second;
	}
};
unordered_map<pair<int,int>,int,cmp> umap;
```



# 数学

## 质数

### 试除法判断质数

```c++
bool is_prime(int n)
{
    if(n<2) return false;
    for(int i=2;i<=n/i;i++)
    {
        if(n%i==0) return false;
    }
    return true;
}
```

### 分解质因数

```c++
void divide(int n)
{
    for(int i=2;i<=n/i;i++)
    {
        if(n%i==0)
        {
            int s=0;
            while(n%i==0)
            {
                n/=i;
                s++;
            }
            cout<<i<<' '<<s<<'\n';//输出质数
            
        }
    }
    if(n>1) cout<<n<<' '<<1<<'\n';//最后一个
    cout<<'\n';
}
```





### 线性筛

> 时间复杂度:$O(n)$

```c++
const int N=1000010;
int primes[N];
bool st[N];
int cnt=0;
void get_primes(int n)
{
    for(int i=2;i<=n;i++)
    {
        if(!st[i]) primes[cnt++]=i;
        for(int j=0;primes[j]<=n/i;j++)
        {
            st[primes[j]*i]=true;
            if(i%primes[j]==0) break;
        }
        
    }
}
```

## 约数

### 试除法求约数

```c++
void get_divisors(int n)
{
    vector<int> q;
    for(int i=1;i<=n/i;i++)
    {
        if(n%i==0)
        {
            q.push_back(i);
            if(n/i!=i) q.push_back(n/i);
        }
    }
    sort(q.begin(),q.end());
    for(auto t:q) cout<<t<<' ';
} 
```

### 最大公约数

```c++
int gcd(int a,int b)
{
    return b?gcd(b,a%b):a;
}
```

## 欧拉函数

### 欧拉函数

> 欧拉函数的定义:  
> $1 \sim N$  中与  $N$  互质的数的个数被称为欧拉函数，记为  $\phi(N)$  。
>
> 若在算数基本定理中，  $N=p_{1}^{a_{1}} p_{2}^{a_>{2}} \cdots p_{m}^{a_{m}}$  ，则:  
> $\phi(N)=N \times \frac{p_{1}-1}{p_{1}} \times \frac{p_{2}-1}{p_{2}} \times \ldots \times \frac{p_{m}-1}{p_{m}}$  
> 注意: $\phi(1)=1$

```c++
int get_phi(int n)
{
    int ans=n;
    for(int i=2;i<=n/i;i++)
    {
        if(n%i==0)
        {
            ans=ans/i*(i-1);
            while(n%i==0) n/=i;
        }
    }
    if(n>1) ans=ans/n*(n-1);
    return ans;
}
```

### 欧拉筛求欧拉函数

```c++
int primes[N];
int phi[N];
bool st[N];
int cnt;
void solve()
{
    int n;
    scanf("%lld",&n);
    phi[1]=1;
    for(int i=2;i<=n;i++)  
    {
        if(!st[i])
        {
            primes[cnt++]=i;
            phi[i]=i-1;
        }
        for(int j=0;primes[j]<=n/i;j++)
        {
            st[primes[j]*i]=true;
            if(i%primes[j]==0)
            {
                phi[primes[j]*i]=phi[i]*(primes[j]);
                break;
            }
            phi[primes[j]*i]=phi[i]*(primes[j]-1);
        }
        
    }
   int ans=0;
   for(int i=1;i<=n;i++) ans+=phi[i];
   cout<<ans<<'\n';
}
```

## 逆元

>要求 $ax\equiv 1(\bmod m)$ $m为质数$$ x\equiv1(\bmod m ) b \text {存在乘法逆元的充要条件是 } b \text { 与模数 } m \text { 互质。当模数 } m \text { 为质数时， } b^{m-2} \text { 即为 } b \text { 的乘法逆元 }$

### 快速幂求逆元

```c++
int qmi(int a,int b,int mod)
{
    int ans=1;
    while(b)
    {
        if(b&1) ans=1ll*ans*a%mod;
        a=1ll*a*a%mod;
        b>>=1;
    }
    return ans;
}
void solve()
{
    int a,p;
    cin>>a>>p;
    if(a%p==0) cout<<"impossible"<<'\n';
    else
    {
        cout<<qmi(a,p-2,p)<<'\n';
    }
}
```

### 扩展欧几里得算法求逆元

```c++
int exgcd(int a,int b,int &x,int &y)
{
    if(!b)
    {
        x=1,y=0;
        return a;
    }
    int d=exgcd(b,a%b,y,x);
    y-=a/b*x;
    return d;
}
void solve()
{
    int a,p,x,y;
    cin>>a>>p;
    int d=exgcd(a,p,x,y);
    if(d==1) cout<<(x+p)%p<<endl;//保证x是正数
    else puts("impossible");
}
```

## 组合数

### 求组合数1

> 适用于数据量小的求法(也可以暴力求)
>
> 时间复杂度:$O(n^2)$

```c++
const int N=1010;
const int mod=1e9+7;
int C[N][N];
void solve()
{
    for(int i=0;i<N;i++)
        for(int j=0;j<=i;j++)
            if(!j) C[i][j]=1;
            else C[i][j]=(C[i-1][j],C[i-1][j-1])%mod;
}
```

### 求组合数2 (用逆元求)

> 时间复杂度:$O( log n)$

```c++
const int N=100010;
const int mod=1e9+7;
int fact[N];
int infact[N];
int qmi(int a,int b,int mod)
{
    int ans=1;
    while(b)
    {
        if(b&1) ans=1ll*ans*a%mod;
        a=1ll*a*a%mod;
        b>>=1;
    }
    return ans;
}
void init()
{
    fact[0]=1;
    infact[0]=1;
    for(int i=1;i<N-1;i++)
    {
        fact[i]=fact[i-1]*i%mod;
        infact[i]=infact[i-1]*qmi(i,mod-2,mod)%mod;
    }
}
void solve()
{
    int a,b;
    cin>>a>>b;
    cout<<fact[a]%mod*infact[b]%mod*infact[a-b]%mod<<'\n';
}
```

### 卢卡斯定理

> 适用情况:数字较大,但是模数较小
>
> 公式:$C_{a}^{b}(\text { lucas }) \equiv C_{\frac{a}{p}}^{\frac{b}{p}}(\text { lucas }) C_{a \bmod p}^{b \bmod p}(\bmod p)$

```c++
int qmi(int a,int b,int mod)
{
    int ans=1;
    while(b)
    {
        if(b&1) ans=1ll*ans*a%mod;
        a=1ll*a*a%mod;
        b>>=1;
    }
    return ans;
}
int C(int a,int b,int mod)
{
    if(b>a) return 0;
    int ans=1;
    for(int i=1,j=a;i<=b;i++,j--)
    {
        ans=ans*j%mod;
        ans=ans%mod*qmi(i,mod-2,mod)%mod;
    }
    return ans;
}
int lucas(int a,int b,int mod)
{
    if(a<mod&b<mod) return C(a,b,mod);
    else return C(a%mod,b%mod,mod)*lucas(a/mod,b/mod,mod)%mod;
}
void solve()
{
    cin>>a>>b>>mod;
    cout<<lucas(a,b,mod)<<'\n';
}
```

## 欧拉函数

>$a^{\varphi(n)} \equiv 1(\bmod m)$  
>如果说若$m$,$a$为正整数，且$m$和 $a$ 互质 那么就成立,当$m$为质数的时候,就是小费马定理

## FFT

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <cmath>
using namespace std;
const int N = 300010;
const double PI = acos(-1);
int n, m;
struct Complex
{
    double x, y;
    Complex operator+ (const Complex& t) const
    {
        return {x + t.x, y + t.y};
    }
    Complex operator- (const Complex& t) const
    {
        return {x - t.x, y - t.y};
    }
    Complex operator* (const Complex& t) const
    {
        return {x * t.x - y * t.y, x * t.y + y * t.x};
    }
}a[N], b[N];
int rev[N], bit, tot;

void fft(Complex a[], int inv)
{
    for (int i = 0; i < tot; i ++ )
        if (i < rev[i])
            swap(a[i], a[rev[i]]);
    for (int mid = 1; mid < tot; mid <<= 1)
    {
        auto w1 = Complex({cos(PI / mid), inv * sin(PI / mid)});
        for (int i = 0; i < tot; i += mid * 2)
        {
            auto wk = Complex({1, 0});
            for (int j = 0; j < mid; j ++, wk = wk * w1)
            {
                auto x = a[i + j], y = wk * a[i + j + mid];
                a[i + j] = x + y, a[i + j + mid] = x - y;
            }
        }
    }
}

int main()
{
    scanf("%d%d", &n, &m);
    for (int i = 0; i <= n; i ++ ) scanf("%lf", &a[i].x);
    for (int i = 0; i <= m; i ++ ) scanf("%lf", &b[i].x);
    while ((1 << bit) < n + m + 1) bit ++;
    tot = 1 << bit;
    for (int i = 0; i < tot; i ++ )
        rev[i] = (rev[i >> 1] >> 1) | ((i & 1) << (bit - 1));
    fft(a, 1), fft(b, 1);
    for (int i = 0; i < tot; i ++ ) a[i] = a[i] * b[i];
    fft(a, -1);
    for (int i = 0; i <= n + m; i ++ )
        printf("%d ", (int)(a[i].x / tot + 0.5));

    return 0;
}
```

## 欧拉筛求积性函数

> f($a\times b$)=f(a)$\times$f(b) 这样的函数叫做积性函数(gcd(a,b)==1)

```c++
typedef long long ll;
const int N=1e7+10;
int idx=0;
int cnt[N];//一个数字的最小质因子出现的次数
int primes[N];
bool st[N];
ll f[N];
void solve()
{
    int n;
    cin>>n;
    cout<<f[n]<<'\n';
}
ll calc_f(int n,int cnt)//用于这个primes的计算,对于每一个积性函数是不一样的
{
    return cnt+1;
}
void get_primes(int n)
{
    f[1]=1;//一般都要f[1]初始化
    for(int i=2;i<=n;i++)
    {
        if(!st[i])
        {
            primes[idx++]=i;
            f[i]=2;//就两个因子
            cnt[i]=1;//注意这里也要加上,i这个最小质因子出现的次数是1
        }
        for(int j=0;primes[j]<=n/i;j++)
        {
          st[primes[j]*i]=true;
          if(i%primes[j]==0)
          {
              cnt[primes[j]*i]=cnt[i]+1;
           f[primes[j]*i]=f[i]/calc_f(primes[j],cnt[i])*calc_f(primes[j],cnt[i]+1);//这里是除去这个数字然后再*上
              break;
          }
          cnt[primes[j]*i]=1;//这里表示就是出现了一次
          f[primes[j]*i]=f[i]*calc_f(primes[j],1);//出现了一次,所以这里也要加上
        }
   
        
    }
}
```



# 字符串

## KMP

### 求next数组

```c++
const int N=100010;
char s[N];
int ne[N];
void getnext() //使用数组读入字符串 
{
    //s是子串
    int n;//字符串长度
    cin>>n>>s+1;//下标要从1开始读入 
    for(int i=2;i<=n;i++)
    {
        ne[i]=ne[i-1];
        while(ne[i]&&s[i]!=s[ne[i]+1]) ne[i]=ne[ne[i]];
        ne[i]+=s[i]==s[ne[i]+1];
    }   
}
```

### KMP匹配

```c++
//p是子串,s是模式串 
for(int i=1,j=0;i<=m;i++)
{
    while(j&&s[i]!=p[j+1]) j=ne[j];
    if(s[i]==p[j+1]) j++;
    if(j==n)
    {
        j=ne[j];//如果从0 开始不重复,那么应该设置为j=0
        //这里进行相关的操作
    }
}
```

### 求最小循环节

```c++
//在ne数组上进行求解
for(int i=2;i<=n;i++)
{
    int t=i-ne[i];
    if(i%t==0&&ne[i])
    {
        cout<<i<<' '<<i/t<<'\n';
    }
}
```

## Trie树

```c++
const int N=2000010*2;
int tr[N][2];//得看对应的情况
int has[N];
int idx=0;
void insert(int x,int add)//add是增量,一般是1/0,表示的是增加和删除
{
    int p=0;
    for(int i=31;i>=0;i--)
    {
        int u=(x>>i)&1;
        if(!tr[p][u]) tr[p][u]=++idx;
        p=tr[p][u];
        has[p]+=add;//表示的是有没有这个枝条
    }
    
}
int query(int x)
{
    int ans=0;
    int p=0;
    for(int i=31;i>=0;i--)
    {
        int u=(x>>i)&1;
        if(has[tr[p][!u]])
        {
            ans+=(1<<i);
            p=tr[p][!u];
        }else if(has[tr[p][u]])//有可能这个枝条不存在,如果没有就直接返回就行了
        {
            p=tr[p][u];
        }else return ans;
    }
    return ans;
}
```

## Manachar算法(求最长回文串长度)

> 时间复杂度: $O(n)$

1. `p数组`不需要`memset`
2. 从0开始,字符串`abca`会变成`$#a$b#c#a#^`

```c++
const int N=1000010;
char a[N],b[N*2];//a是原来的串,然后b是后来进行扩充的串,
int p[N*2];//包括自身的最长回文串半径
int n;//回文串长度 最终会变成b的回文串长度
void init()
{
    int k=0;
    b[k++]='$';
    b[k++]='#';
    for(int i=0;i<n;i++) b[k++]=a[i],b[k++]='#';
    b[k++]='^';
    n=k;
}
void manacher()
{
    int mr=0,mid;
    for(int i=1;i<n;i++)
    {
        if(i<mr) p[i]=min(p[mid*2-i],mr-i);
        else p[i]=1;
        while(b[i-p[i]]==b[i+p[i]]) p[i]++;
        if(i+p[i]>mr)
        {
            mr=i+p[i];
            mid=i;
        }
    }
}
void solve()
{
    cin>>a;//读入字符串
    n=strlen(a);
    init();
    manacher();
}

```

## 字符串哈希

1. 下标从0开始

```c++
struct Hash
{
	int size;
	static constexpr int base1=20023;
	static constexpr int base2=20011;
	static constexpr ll mod1=2000000011;
	static constexpr ll mod2=3000000019;
	vector<array<ll, 2>> hash, pow_base;
	Hash(){}
	Hash(const string& s)
	{
		size = s.size();
		hash.resize(size);
		pow_base.resize(size);
		pow_base[0][0] = pow_base[0][1] = 1;
		hash[0][0] = hash[0][1] = s[0];
		for(int i = 1; i < size; i++){
			hash[i][0] = (hash[i - 1][0] * base1 + s[i]) % mod1;
			hash[i][1] = (hash[i - 1][1] * base2 + s[i]) % mod2;
			pow_base[i][0] = pow_base[i - 1][0]*base1%mod1;
			pow_base[i][1] = pow_base[i - 1][1]*base2%mod2;
		}
	}
	array<ll, 2> operator[](const array<int,2>& range)const{
		int l=range[0],r=range[1];
		if(l==0) return hash[r];
		auto single_hash = [&](bool flag){
			const ll mod=!flag?mod1:mod2;//注意顺序前面有!
			return (hash[r][flag]-hash[l-1][flag]*pow_base[r-l+1][flag]%mod+mod)%mod;
		};
		return { single_hash(0),single_hash(1)};
	}
};
```

# 图论

## Dijkstra求最短路

```c++
#include <bits/stdc++.h>
using namespace std;
const int N=200010;
typedef pair<int,int> PII;
int dist[N];
bool st[N];
int e[N],ne[N],w[N],h[N],idx;
void Dijkstra()
{
	priority_queue<PII,vector<PII>,greater<PII>> q;
	q.push({0,1});
	dist[1]=0;
	while(q.size())
	{
		auto v=q.top().second;
		q.pop();
		if(st[v]) continue;
		st[v]=true;
		for(int i=h[v];i!=-1;i=ne[i])
		{
			int j=e[i];
			if(dist[j]>dist[v]+w[i])
			{
				dist[j]=dist[v]+w[i];
				q.push({dist[j],j});
			}
		}
		
	}
	
}
```

## spfa求最短路
>时间复杂度:$O(nlogn)$
```c++
const int N=200010;
const int INF=0x3f3f3f3f;
int n,m;
int e[N],ne[N],w[N],h[N],idx;
int dist[N];
bool st[N];
void spfa()
{
	memset(dist,INF,sizeof(dist));
	queue<int> q;
	q.push(1);
	dist[1]=0;
	st[1]=true;
	while(q.size())
	{
		auto t=q.front();
		q.pop();
		st[t]=false;
		for(int i=h[t];i!=-1;i=ne[i])
		{
			int j=e[i];
			if(dist[j]>dist[t]+w[i])
			{
				dist[j]=dist[t]+w[i];
				if(!st[j])
				{
					st[j]=true;
					q.push(j);
				}
			}
		}
	}
}
```
## floyd求最短路
>时间复杂度:$O(n)$
```c++
const int N=1010;
const int INF=0x3f3f3f3f;
int g[N][N];
int n,m,k;
void init()
{
	memset(g,INF,sizeof(g));
	for(int i=1;i<=n;i++) g[i][i]=0;
}
void floyd()
{
	for(int k=1;k<=n;k++)
		for(int i=1;i<=n;i++)
			for(int j=1;j<=n;j++)
				g[i][j]=min(g[i][j],g[i][k]+g[k][j]);
	
}
```
## prim算法求最小生成树
```c++
const int N=510;
bool st[N];
int g[N][N];
int dist[N];
int n,m;
void init()
{
	memset(dist,0x3f,sizeof dist);
	memset(g,0x3f,sizeof g);
}
void prim()
{
	
	int ans=0;
	for(int i=0;i<n;i++)
	{
		int t=-1;
		for(int j=1;j<=n;j++)
		{
			if(!st[j]&&(t==-1||dist[t]>dist[j])) t=j;
		}
		
		if(i&&dist[t]==0x3f3f3f3f)
		{
			cout<<"impossible"<<endl;
			return;
		}
		st[t]=true;
		if(i) ans+=dist[t];
		for(int j=1;j<=n;j++) dist[j]=min(dist[j],g[t][j]);
		
	}
	cout<<ans<<endl;
	return;
	
}
```
## Kruskal 求最小生成树
```c++
const int N=2e5+10;
int n,m;
int p[N];
struct Node
{
    int a,b,w;
    bool operator <(const Node &b) const
    {
        return w<b.w;
    }
}q[N];
int find(int x)
{
    if(x!=p[x]) p[x]=find(p[x]);
    return p[x];
}
void Kruskal()
{
    int cnt=0;
    int ans=0;
    for(int i=1;i<=n;i++) p[i]=i;
    sort(q,q+m);
    for(int i=0;i<m;i++)
    {
        int a=q[i].a;
        int b=q[i].b;
        int w=q[i].w;
        int pa=find(a);
        int pb=find(b);
        if(pa!=pb)
        {
            p[pa]=pb;
            ans+=w;
            cnt++;
        }
    }
    if(cnt>=n-1) cout<<ans<<'\n';
    else cout<<"impossible"<<'\n';
}
```

# 计算几何

```c++
#include <bits/stdc++.h>
using namespace std;
#define IOS ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
#define eps 1e-8
#define int128 __int128
#define gcd(a,b) __gcd(a,b)
#define lcm(a,b) a/gcd(a,b)*b
#define lowbit(x) (x&-x)
#define all(x) x.begin(), x.end()
#define debug(x...) do { cout<< #x <<" -> "; re_debug(x); } while (0)
void re_debug() { cout<<'\n'; }
template<class T, class... Ts> void re_debug(const T& arg,const Ts&... args) { cout<<arg<<" "; re_debug(args...); }
int test=1;
void cut(){cout<<"test:"<<' '<<test++<<'\n';}
typedef long long ll;
typedef unsigned long long ull;
typedef pair<int,int> PII;
const int INF=0x3f3f3f3f;
const ll LNF=0x3f3f3f3f3f3f3f3fll;
const double PI=acos(-1.0);
int sign(double x)//符号函数
{
	if(abs(x)<eps) return 0;//算是0
	if(x<0) return -1;
	return 1;
}
struct Point
{
	double x,y;
	Point operator +(const Point &b) const
	{
		return Point{x+b.x,y+b.y};
	}
	Point operator -(const Point &b) const
	{
		return Point{x-b.x,y-b.y};
	}
	Point operator *(const double &k) const
	{
		return Point{x*k,y*k};	
	}
	Point operator /(const double &k) const
	{
		return Point{x/k,y/k};
	}
	bool operator ==(const Point &b) const
	{
		return sign(x-b.x)==0&&sign(y-b.y)==0;
	}
	bool operator <(const Point &b) const
	{
		return x < b.x || (x == b.x && y < b.y); 
	}
	
};


int cmp(double x,double y)//比较函数
{
	if(abs(x-y)<eps) return 0;
	if(x<y) return -1;
	return 1;
}
double dot(Point a,Point b)//点乘
{
	return a.x*b.x+a.y*b.y;
}
double cross(Point a,Point b)//外积:表示向量A,B形成的平行四边形面积
{
	return a.x*b.y-a.y*b.x;
}
double get_lenth(Point a)//求模长 用的是向量
{
	return sqrt(dot(a,a));
}
double get_lenth(Point a,Point b)
{
	return sqrt((a.x-b.x)*(a.x-b.x)+(a.y-b.y)*(a.y-b.y));
}
double get_angle(Point a,Point b)//返回的是弧度
{
	return acos(dot(a,b)/get_lenth(a)/get_lenth(b));
}
double get_area(Point a,Point b,Point c)//返回三点构成的平行四边形的有向面积
{
	return cross(b-a,c-a);
}
Point rotate(Point a,double angle)//向量A顺指针旋转C的角度
{
	return Point{a.x*cos(angle)+a.y*sin(angle),-a.x*sin(angle)+a.y*cos(angle)};
}
Point get_line_intersection(Point p,Point v,Point q,Point w)//两个直线相交的点
{
	//两个直线是p+tv和q+tw
	Point u=p-q;
	double t=cross(w,u)/cross(v,w);
	return p+v*t;
}
double distance_to_line(Point a,Point b,Point p)//点p到直线ab的距离
{
	Point v1=b-a,v2=p-a;
	return abs(cross(v1,v2)/get_lenth(a,b));
}
double distance_to_segment(Point a,Point b,Point p)//点p到线段ab的距离
{
	if(a==b) return get_lenth(p-a);
	Point v1=b-a,v2=p-a,v3=p-b;
	if(sign(dot(v1,v2))<0) return get_lenth(v2);
	if(sign(dot(v1,v3))>0) return get_lenth(v3);
	return distance_to_line(a,b,p);
}
Point get_line_projection(Point a,Point b,Point p)//点p在向量ab的投影的坐标
{
	Point v=b-a;
	return a+v*(dot(v,p-a)/dot(v,v));
}
bool is_on_segment(Point a,Point b,Point p)//点p是否在线段ab上
{
	return sign(cross(p-a,p-b))==0&&sign(dot(p-a,p-b))<=0;
}
bool is_segment_intersection(Point a1,Point a2,Point b1,Point b2)//线段a和b是否相交
{
	/*
	  double c1 = cross(a2 - a1, b1 - a1), c2 = cross(a2 - a1, b2 - a1);
	  double c3 = cross(b2 - b1, a2 - b1), c4 = cross(b2 - b1, a1 - b1);
	  return sign(c1) * sign(c2) <= 0 && sign(c3) * sign(c4) <= 0;

	 */
	
	/* 
	 a1    b2 
	   \   /
		\ /
	    /\
    b1 /  \ a2
	
	*/
	double c1=cross(a2-a1,b1-a1),c2=cross(a2-a1,b2-a1);
	double c3=cross(b2-b1,a2-b1),c4=cross(b2-b1,a1-b1);
	return sign(c1)*sin(c2)<=0&&sign(c3)*sign(c4)<=0;
}
double get_triangle_area(Point a,Point b,Point c)//得到三个点围城的三角形面积 
{
	//海伦公式 p=(a+b+c)/2 S=sqrt((p-a)*(p-b)*(p-c)));
	double len_a=get_lenth(a-b);
	double len_b=get_lenth(a-c);
	double len_c=get_lenth(b-c);
	double p=(len_a+len_b+len_c)/2;
	return sqrt(p*(p-len_a)*(p-len_b)*(p-len_c));
}
double polygon_area(Point p[],int n)//求多边形面积
{
	double ans=0;
	for(int i=1;i+1<n;i++)
	{
		ans+=cross(p[i]-p[0],p[i+1]-p[i]);
	}
	return ans/2;
}
void solve()
{
	cout<<get_triangle_area({0,0},{1,1},{1,1})<<'\n';
}
int main()
{
	IOS;
	int T=1;
	//	cin>>T;
	while(T--) solve();
	return 0^0;
}
```
