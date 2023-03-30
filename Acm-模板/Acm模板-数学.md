# 质数 
## 试除法判断质数 
> 时间复杂度 $0(\sqrt n)$
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
## 分解质因数
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
## 线性筛
> 时间复杂度: $O(n)$
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
# 约数 
## 试除法求约数
> 时间复杂度 : $O(\sqrt{n})$
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
## 最大公约数
> 时间复杂度 $O(logn)$
```c++
int gcd(int a,int b)
{
    return b?gcd(b,a%b):a;
}
```
# 欧拉函数
## 欧拉函数
> 欧拉函数的定义:  
>   $1 \sim N$  中与  $N$  互质的数的个数被称为欧拉函数，记为  $\phi(N)$  。
> 
>若在算数基本定理中，  $N=p_{1}^{a_{1}} p_{2}^{a_>{2}} \cdots p_{m}^{a_{m}}$  ，则:  
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
## 筛法求欧拉函数
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
# 逆元
> 要求 $ax\equiv 1(\bmod m)$ $m为质数$ 

> $x\equiv1(\bmod m ) b \text {存在乘法逆元的充要条件是 } b \text { 与模数 } m \text { 互质。当模数 } m \text { 为质数时， } b^{m-2} \text { 即为 } b \text { 的乘法逆元 }$
 ## 快速幂求逆元
```c++
#define int long long
int qmi(int a,int b,int mod)
{
    int ans=1;
    while(b)
    {
        if(b&1) ans=ans*a%mod;
        a=a*a%mod;
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
## 扩展欧几里得算法算求逆元
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
# 组合数
## 求组合数1
> 适用于数据量少的求法(也可以直接暴力求)  
> 时间复杂度: $n^{2}$
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
## 求组合数2 (使用逆元进行求)
>时间复杂度: $log(n)$
```c++
#define int long long
const int N=100010;
const int mod=1e9+7;
int fact[N];
int infact[N];
int qmi(int a,int b,int mod)
{
    int ans=1;
    while(b)
    {
        if(b&1) ans=ans*a%mod;
        a=a*a%mod;
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
## 卢卡斯定理
> 使用情况: 数字较大,但是模数较小  
> 公式:$C_{a}^{b}(\text { lucas }) \equiv C_{\frac{a}{p}}^{\frac{b}{p}}(\text { lucas }) C_{a \bmod p}^{b \bmod p}(\bmod p)$
```c++
#define int long long
int qmi(int a,int b,int mod)
{
    int ans=1;
    while(b)
    {
        if(b&1) ans=ans*a%mod;
        a=a*a%mod;
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
        ans=ans*qmi(i,mod-2,mod)%mod;
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

$ka \equiv kb \pmod{m}$ 
如果说$k$与$m$互质,那么就免费除以$k$
# 欧拉定理
$a^{\varphi(n)} \equiv 1(\bmod m)$  
如果说若$m$,$a$为正整数，且$m$和 $a$ 互质 那么就成立,当$m$为质数的时候,就是小费马定理