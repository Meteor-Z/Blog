# [AtCoder Beginner Contest 262](https://atcoder.jp/contests/abc262/)
## [A - World Cup](https://atcoder.jp/contests/abc262/tasks/abc262_a)
题解:循环判断即可
```c++
#include <bits/stdc++.h>
using namespace std;
void solve()
{
    int n;
    cin>>n;
    for(int i=n;;i++)
    {
        if(i%4==2)
        {
            cout<<i<<'\n';
            return;
        }
    }
}
int main()
{
    int T=1;
    while(T--) solve();
    return 0^0;
}
```
## [B - Triangle (Easier)](https://atcoder.jp/contests/abc262/tasks/abc262_b)
题意: 给定$n$点,$m$条边,如果$a,b,c$相连,那么$ans++$,求$ans$

题解:观察到$n$ $\le$ $100$ 可以直接暴力循环判断,然后直接搞
```c++
#include <bits/stdc++.h>
using namespace std;
const double PI=acos(-1.0);
typedef pair<int,int> PII;
void solve()
{
    int n,m;
    cin>>n>>m;
    map<PII,int> mp;
    for(int i=0;i<m;i++)
    {
        int a,b;
        cin>>a>>b;
        mp[{a,b}]++;//模拟连边
        mp[{b,a}]++;
    }
    int ans=0;
    for(int i=1;i<=n;i++)
    {
        for(int j=i+1;j<=n;j++)
        {
            for(int k=j+1;k<=n;k++)
            {
                if(mp[{i,j}]&&mp[{i,k}]&&mp[{j,k}])//表示连接了
                {
                    ans++;
                }
            }
        }
    }
    cout<<ans<<'\n';
}
int main()
{
    int T=1;
 //   cin>>T;
    while(T--) solve();
    return 0^0;
}
```
## [C - Min Max Pair](https://atcoder.jp/contests/abc262/tasks/abc262_c)
>题意:给定$n$个数字,并且数字范围是$1$到$n$,然后如果满足以下等式那么就$ans$++,求$ans$的个数:
1. $1 \le i<j \le N$
2. $min(a_i,a_j)=i$
3. $max(a_i,a_j)=j$

题解:这里有两种情况要考虑.
1. 当$a_i=i$的时候,这时候另一个就是$a_j=j$,这里可以发现所以的$a_i=i$两两之间都可以进行相应的排列组合,,比并且这里就是组合数$C_{n}^{2}$(就是两两组合)
2. 当$a_i!=i$的时候,那么他肯定是这样的$a_i=j$和$a_j=i$这两个组合,那么就可以直接找,然后进判断.

code
```c++
#include <bits/stdc++.h>
using namespace std;
const double PI=acos(-1.0);
typedef long long ll;
void solve()
{
    int n;
    cin>>n;
    vector<int> q(n+1);
    for(int i=1;i<=n;i++) cin>>q[i];
    ll ans=0;
    vector<int> st(n+1,true);
    for(int i=1;i<=n;i++)
    {
        if(q[i]!=i)
        {
            if(q[q[i]]==i&&st[i]&&st[q[i]])
            {
                ans++;//这个统计的是第二种情况,
                st[i]=false;//防止重复出现
                st[q[i]]=false;
            }
        }
    }
    // cout<<ans<<'\n';
    ll temp=0;
    for(int i=1;i<=n;i++) temp+=(q[i]==i);//第一种情况
    // cout<<temp<<'\n';
    ans+=(ll)(temp*(temp-1)/2);//组合数C_{n}^{2}
    cout<<ans<<'\n';
}
int main()
{
    int T=1;
    //cin>>T;
    while(T--) solve();
    return 0^0;
}
```
## [D - I Hate Non-integer Number](https://atcoder.jp/contests/abc262/tasks/abc262_d)
>题意:给定$n$个数字,对于$n$个数字组成的所有的非空子集,他的子集元素所组成的算数平均数是整数的子集个数有多少个? 答案对于$998244353$进行取模

题解: 显示要考虑$DP$,但是比赛的时候并不会,还是赛后会的...

设$f[i][j][k][b]$表示前$i$个数字,选$j$个,$模k$的情况下等于b的方案数
   1.  $f[i+1][j+1][k][(b+x)\% k]+=f[i][j][k][b]%mod$;选$x$这个数字
   2.  $f[i+1][j][k][b]+=f[i][j][k][b]$;不选$x$这个数字
   
code
```c++
#include <bits/stdc++.h>
using namespace std;
const int N=101;
const int mod=998244353;;
ll f[N][N][N][N];
//表示的是前i个数字,选j个,在模k的情况下等于b的方案数
void solve()
{
    int n;
    cin>>n;
    for(int i=0;i<=n;i++) f[0][0][i][0]=1;
    for(int i=0;i<n;i++)
    {
        int x;
        cin>>x;
        for(int j=0;j<=i;j++)
        {
            for(int k=1;k<=n;k++)
            {
                for(int b=0;b<=n;b++)
                {
                    f[i+1][j+1][k][(b+x)%k]=(f[i+1][j+1][k][(b+x)%k]+f[i][j][k][b])%mod;//选这个数字
                    f[i+1][j][k][b]=(f[i+1][j][k][b]+f[i][j][k][b])%mod;//不选这个数字
                }
            }
        }
    }
    ll ans=0;
    for(int i=1;i<=n;i++) ans=(ans+f[n][i][i][0])%mod;
    cout<<ans<<'\n';
}
int main()
{
    int T=1;

    while(T--) solve();
    return 0^0;
}
```