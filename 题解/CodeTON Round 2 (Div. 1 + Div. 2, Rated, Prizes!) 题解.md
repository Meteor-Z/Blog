# CodeTON Round 2 (Div. 1 + Div. 2, Rated, Prizes!) 题解
## A-Two 0-1 Sequences
>题意:有两个字符串$a和b$,都是$01$字符串,可以进行一下操作看是否可以将$a$变成$b$,设$a_1$和$a_2$ 表示的是字符串$a$的第一个字母和字母
>>1. 在满足可以操作的前提下,将$a_2$变成$max(a_1,a_2)$,并将$a_1$删去
>>2. 在满足可以操作的前提下,将$a_2$变成$min(a_1,a_2)$,并将$a_1$删去,

思路: 删去的时候肯定不能让字符串$a$的大小&lt;字符串$b$的大小,并且得知后面的字符串一定要相同,否则不能满足题意,但是字符串$b$的第一个字母是可以和$字符串a$与之对应的第一个字母不相同的,因为可以通过操作$1$和$2$进行变化,将如果缺少$1$ 那么可以用前面的操作将$a$中的$1$挪到$b$中对应的位置上

code:
```c++
#include <bits/stdc++.h>
using namespace std;
void solve()
{
    int aa,bb;
    cin>>aa>>bb;
    string a,b;
    cin>>a>>b;
    if(aa<bb)//如果大小小的话,直接NO
    {
        cout<<"NO"<<'\n';
        return;
    }
    if(aa==bb)
    {
        if(a==b) cout<<"YES"<<'\n';//特殊判断
        else cout<<"NO"<<'\n';
    }else
    {
        for(int i=aa-1,j=bb-1;j>=1;i--,j--)//判断后面的单词是否一样
        {
            if(a[i]!=b[j])
            {
                cout<<"NO"<<'\n';
                return;
            }
        }
        // debug(a.size()-b.size()+1);
        for(int i=0;i<a.size()-b.size()+1;i++)//判断前面的单词是否有与之对应的1或者0就可以了
        {
            if(a[i]==b[0])
            {
                cout<<"YES"<<'\n';
                return;
            }
        }
        cout<<"NO"<<'\n';
    }
}
int main()
{
    int T=1;
    cin>>T;
    while(T--) solve();
    return 0^0;
}
```
## B-Luke is a Foodie
>题意:一串数字,只能挨着吃,当且仅当$|a_i-x| \le k$ 才可以吃掉,$x$当前自己的分数, $a_i$是数字,$k$是临界值,开头可以自己设置任意值,但是之后每一次改变自己的$x$都会导致$ans$++,问$ans$的最小值是多少

题解:考虑贪心:吃的是一个范围,所以说我们可以找到一个值,使他能够横穿而过,这个时候就是最优解,然后再判断当前是否可以横穿而过,如果不能,那么就重新写这个范围,如果能,贼可以缩小这个范围 ![示意图](https://c2.im5i.com/2022/08/01/bKPuL.jpg)

红线的就是能走的范围,如果不能,就只有两种情况,可以直接哦判断,然后设置范围!![bKKgS.jpg](https://c2.im5i.com/2022/08/01/bKKgS.jpg)

code
```c++
#include <bits/stdc++.h>
void solve()
{
    int n,k;
    cin>>n>>k;
    int ans=0;
    vector<ll> q(n);
    
    for(int i=0;i<n;i++) cin>>q[i];
    ll maxn=q[0]+k;//一开始的最大值
    ll minn=q[0]-k;//一开始的最小值
    for(int i=1;i<n;i++)
    {
        if(q[i]-k>maxn||q[i]+k<minn)
        {
            ans++;//如果不在这个范围内,就重新设置范围,并且ans++
            maxn=q[i]+k;
            minn=q[i]-k;
        }else
        {
            maxn=min(maxn,q[i]+k);//缩小范围
            minn=max(minn,q[i]-k);
        }
    }
    cout<<ans<<'\n';
}
int main()
{
    int T=1;
    cin>>T;
    while(T--) solve();
    return 0^0;
}
```
## C-Virus
>题意:$n$个村庄首尾相连,然后有初始化$k$个点是一开始感染的,然后每一天就会向外边感染其他没有未被感染的村庄(必须是相邻的),可以在每一天的开始保护一个村庄永远不会感染,然后可以隔绝其他的村庄感染,然后问你最后感染的最小值


可以考虑贪心.对于一个间隔,我们可以直接保护最左端点和最右端点,然后中间的端点就不会感染,这样我们能保护的村庄就是最优的.可以先对没有感染的排序一下,从大到小贪心的做一下

code
```c++
#include <bits/stdc++.h>
using namespace std;
bool cmp(int a,int b)
{
    return a>b;
}
void solve()
{
    int n,k;
    cin>>n>>k;
    vector<ll> q(k);
    for(int i=0;i<k;i++) cin>>q[i];
    sort(all(q));
    vector<ll> s;
    for(int i=1;i<k;i++)
    {
        if(q[i]-q[i-1]-1>0) s.push_back(q[i]-q[i-1]-1);//如果间隔是大于0的,那么就加入
    }
    if(n-q[k-1]+q[0]-1>0) s.push_back(n-q[k-1]+q[0]-1);//加入收尾相连的那个
    sort(all(s),cmp);
    if(s.size()==0)//特判
    {
        cout<<n<<'\n';
        return;
    }
    ll ans=0;//表示没有被污染的点
    ll day=0;
    for(int i=0;i<s.size();i++)
    {
       s[i]-=day;//减去以前累计的
       if(s[i]>0) ans++;
       else if(s[i]<=0) break;
        s[i]--;
       day+=4;//相当于两天,+4就是这个间隔污染了4个
       if(s[i]>=1) ans+=(s[i]-1);//因为上面加了一个了,而且这一天最多污染1个,所以这样判断
       
        
        
    }
    cout<<n-ans<<'\n';
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
## [D-Magical Array](https://codeforces.com/contest/1704/problem/D)
>题意:初始化有$n$个数组,对于其中$n-1$个数字进行操作$1$,对于一个特殊的数组进行操作$2$;

操作1:选择$(i,j)$ 然后可以使得$a[i-1]+=1,a[i]-=1,a[j]-=1,a[j+1]+=1$.  
操作2:选择$i,j$,然后使得$a[i-1]+=1,a[i]-=1,a[j]-=1,a[j+2]+=1$  
然后问你是哪一个特殊的数组,并且进行了几次操作?

题解:我们对于这些数字进行前缀和,但是这里是进行两次前缀和,这样就能表现出一定的差异,是因为操作$1$和操作$2$会虽然第一次对于前缀和没有一定的改变,但是他 操作2会对$a[i+2]+=1$,这时候相当与对于他的前缀和进行一定的右移,并且可以对于第二次前缀和一定的影响,所以说进行两次前缀和,然后搞差距,那个差距就是进行操作2的不同数量,然后那个不同的是多少

code
```c++
#include <bits/stdc++.h>
using namespace std;
void solve()
{
    int n,m;
    cin>>n>>m;
    vector<vector<ll>> s(n+1,vector<ll>(m+1,0));
    for(int i=1;i<=n;i++)
        for(int j=1;j<=m;j++) cin>>s[i][j];
    for(int k=0;k<2;k++)
    {
        for(int i=1;i<=n;i++)
                for(int j=1;j<=m;j++)
                    s[i][j]+=s[i][j-1];//进行两次的前缀和
    }
    vector<int> p(n+1);
    for(int i=1;i<=n;i++) p[i]=i;
    for(int i=1;i<=100;i++)//暴力循环判断100便,因为要找到1一个不同,所以随机判断前两个是相同的话,就是���的,这里随机了,事件复杂度不是很大,感觉近乎O(1)(
    {
        random_shuffle(p.begin()+1,p.end());
        int a=p[1];
        int b=p[2];
        if(s[a][m]==s[b][m])
        {
            for(int i=1;i<=n;i++)
            {
                if(s[i][m]!=s[a][m])
                {
                    cout<<i<<' '<<s[a][m]-s[i][m]<<'\n';
                    return;
                }
            }
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