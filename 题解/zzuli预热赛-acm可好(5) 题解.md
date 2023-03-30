# 1 acm可好玩了
题解:直接输出`acm可好玩了`即可

$code$
```c
#include <stdio.h>
int main()
{
	printf("acm可好玩了\n"); 
}
```
# 2 scz的简单十道题
1. 质数的定义是:当且仅当能被$1$#和自己整除的数叫做质数,那么这里两个数字很大,但是$a\times b$必然不是质数,因为称的话必然能够被$a$和$b$,或者其他的质因子整除等,所以必然是错误的
2. 高中的数学知识:等比数列有两个求和公式:
   1. 当$q=1$的时候:$S_{n}=na_{1} (q=1)$
   2. 当$q\ne 1的时候$$S_{n}=a_{1} \frac{1-q^{n}}{1-q}$ $(q \ne 1)$
   3. 显然是错误的,出题人这种脑残的题都出,哎,智商堪忧
   4. 高中的几个基本放缩,相信大家数学一定懂得的!,显示是正确的
   5. 众所周知,写题要快!,奇函数有一个性质:$f(x)+f(-x)=0$ 那么直接带入$x=1$,可以解得$a=\frac{1}{2}$,并且满足题意,那么显然是正确的
   6.  这里是$\cos$,不是$\sin$,也是高中的基本知识
   7.这里比较迷糊人,出题人也是看小学数学才知道的,根据前面的定义可以知道,这里的小数部分是$-0.66$
   8. 正确的,出题人在百度百科上抄的
   9. 显然是错误的,因为$AC=Accepted$,希望22级新生各个$AC$!
   10. 错误的,但是$scz$很强!

$code$
```c
#include <stdio.h>
int main()
{
	printf("FFFTTFFTFF\n");
}
```
# 3 做scz的狗
这道题直接按照题意模拟就可以了,注意一下细节

$code$
```c
#include<stdio.h>
int main(){
	int n;
	scanf("%d", &n);
	
	char a[14][42];
	
	for(int i = 1; i <= n; i++){
		for(int j = 1; j <= 3 * n + 2; j++){
			a[i][j] = ' ';
		}
	}
	
	// 打印 S
	for(int j = 2; j < n; j++)a[1][j] = a[(n + 1) / 2][j] = a[n][j] = '#';
	for(int i = 2; i < (n + 1) / 2; i++)a[i][1] = '#';
	for(int i = (n + 1) / 2 + 1; i < n; i++)a[i][n] = '#';
	a[2][n] = a[n - 1][1] = '#';
	
	// 打印 C
	for(int j = n + 3; j < 2 * n + 1; j++)a[1][j] = a[n][j] = '#';
	for(int i = 2; i < n; i++)a[i][n + 2] = '#';
	a[2][2 * n + 1] = a[n - 1][2 * n + 1] = '#';
	
	// 打印 Z
	for(int j = 2 * n + 3; j <= 3 * n + 2; j++)a[1][j] = a[n][j] = '#';
	for(int i = 2, j = 3 * n + 1; i < n; i++, j--)a[i][j] = '#';
	
	for(int i = 1; i <= n; i++){
		for(int j = 1; j <= 3 * n + 2; j++){
			putchar(a[i][j]);
		}
		if(i!=n)putchar('\n');
	}
	
	return 0;
}


```
# 4  Lycoris
我拿到最大的糖果,而且让所有人相同,那么应该是让所以人的糖果等于最少的糖果就可以了,我们的糖果数量就是其余的糖果  

$code$
```c
#include<stdio.h>
#include<math.h>


int a_[55];

int main(){
	int t; scanf("%d\n", &t);
	while(t -- ){
		int n; scanf("%d", &n);
		int minn = 1e9, ans = 0;
		for(int i = 0; i < n; i ++ ){
			scanf("%d", &a_[i]);
			if(a_[i] <= minn) minn = a_[i];//minn就是最小的糖果
		}
		for(int i = 0; i < n; i ++ ){
			ans += a_[i] - minn;//这里ans 不会爆long long , 这里算的就是差值
		}
		printf("%d\n", ans);
	}
}
```
## 5 寻找2022级最强oi爷
这里判断字符串,我们可以先判断字符串的大小,如果这里不是等于$4$,那么这里就是错误的,然后我们可以用tolower()函数将所有的字符串转成小写,然后用strcmp()函数进行判断相同,同样,也可以手动转小写,或者判断相同,这里仅是出题人的一种做法  

$code$
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main()
{
    int n;
    char str[15];

    scanf("%d", &n);
    while (n--)
    {
        scanf("%s", str);
        int len = strlen(str);
        if (len != 4)
        {
            printf("NO\n");//直接输出NO,
            continue;//跳过了
        }

        for (int i = 0; i < 4; i++)
            str[i] = tolower(str[i]);//转小写
        
        if (strcmp(str, "oiye") == 0)//如果相同
            printf("YES\n");
        else//不相同的话
            printf("NO\n");
    }
    return 0;
}
```
## 6 fufu吃大米
这里只要统计这个对角线的上面就可以了,用二维数组进行模拟一下就行  

$code$
```c
#include <stdio.h>
#define N 110

int a[N][N];

int main()
{
	int n;
	scanf("%d", &n);
	
	for (int i = 1; i <= n; i++)
		for (int j = 1; j <= n; j++) scanf("%d", &a[i][j]);//读入
	
	int res = 0;
	for (int i = 1; i <= n / 2; i++)
		for (int j = i + 1; j <= n - i; j++) res += a[i][j];//统计上面的格子, 注意细节
	
	printf("%d\n", res);
}
```
# 7 你玩过糖豆人吗
这里就是判断一下二维数组的曼哈顿距离就可以,曼哈顿的距离要小于时间$time$,并且这个数字要等于$number$,然后暴力就行了,
```c
#include <stdio.h>
#include <stdlib.h>

int a[11][11];
int main() {
    int n;
    scanf("%d",&n);
    for(int i=1;i<=n;++i) {
        for(int j=1;j<=n;++j) {
            scanf("%d",&a[i][j]);//读二维数组
        }
    }
    int x,y,t,num;
    scanf("%d %d %d %d",&x,&y,&t,&num);
    for(int i=1;i<=n;++i) {
        for(int j=1;j<=n;++j) {//遍历判断
            if(a[i][j]==num&&abs(x-i)+abs(y-j)<=t) {
                printf("Yes\n");
                return 0;//如果找到,直接退出
            }
        }
    }
    printf("No\n");
    return 0;
}
```
# 8 hygg的训练计划
## 解法1:  
考虑差分,对于$[a,b]$区间,我们可以直接差分,表示当前用过此区间,然后在统计的时候如果差分数组大于$0$的就是可以用的,时间复杂度是$O(n)$,即$O(能过)$
```c
#include <stdio.h>

int time[1000010];

int main () {
	int q;
	scanf("%d", &q);
	for (int i = 1; i <= q; i ++ ) {
		int l, r;
		scanf("%d%d", &l, &r);
		time[l] ++ , time[r + 1] -- ;//差分
	}
	
	int cnt = 0;
	for (int i = 1; i <= 1e6; i ++ ) {
		time[i] += time[i - 1];//边做前缀和边统计
		if (time[i] > 0) cnt ++ ;	
	}
	printf("%d\n", cnt);
}
```
如果不了解的话,可以学习一下`前缀和`和`差分`,
## 解法2
对于每一次区间合并,我们可以用并查集来算,一个并查集的时间复杂接近于$O(1)$,综合的时间复杂度是$O(n)$
每一次合并的时候是l到r,那么我们就遍历l,r然后将这边的数据合并到r+1这个区间连,虽然都要遍历l,r,但是总长度是$n$,所以最大的时间复杂度还是$O(n)$  
ZZULIOJ好像不欢迎C语言,C语言TLE了
```c
#include <stdio.h>
#define N 200010
int p[N];
int m;
int find(int x)
{
	if(x!=p[x]) p[x]=find(p[x]);
	return p[x];
}
int main()
{
	scanf("%d",&m);
	for(int i=1;i<=N;i++) p[i]=i;
	int ans=0;
	for(int i=0;i<m;i++)
	{
		int l,r;
		scanf("%d%d",&l,&r);
		for(int i=find(l);i<=r;i=find(i+1))
		{
			ans++;
			p[i]=find(i+1);
		}
	}
	printf("%d",ans);
	return 0;
}
```
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
const int N=2000010;
int n,m;
int p[N];
int find(int x)
{
	if(x!=p[x]) p[x]=find(p[x]);
	return p[x];
}
void solve()
{
	cin>>m;
	for(int i=1;i<=N;i++) p[i]=i;
	int ans=0;
	while(m--)
	{
		int l,r;
		cin>>l>>r;
		for(int i=find(l);i<=r;i=find(i+1))
		{
			ans++;
			p[i]=find(i+1);
		}
	}
	cout<<ans<<'\n';
	
	
}
int main()
{
	IOS;
	int T=1;
	//	cin>>T;
	while(T--) solve();
	return (0^0);
}
```
# 9  lzc和zyb的生日

这里就是为了判断一下闰年就行了,注意:闰年的判断跟平常的不是一样的,比如$1900$就不是闰年,可以上网查一下,zzulioj上也有一道判断闰年的题目:[zzulioj i love 闰年!](http://acm.zzuli.edu.cn/problem.php?id=1028)
```c
#include <stdio.h>

int is_leap (int x) {
	if ((x % 4 == 0 && x % 100 != 0) || (x % 400 == 0)) {//判断闰年的方式
		return 1;
	}
	else return 0;
}

int main () {
	int year, month, day;
	scanf("%d%d%d", &year, &month, &day);
	if (month > 2) {//这里要判断一下在2月前还是后,因为有可能是365天,也有可能是366天
		if (is_leap(year + 1)) puts("366");
		else puts("365");
	}
	else {
		if ((is_leap(year))) puts("366");
		else puts("365");
	}
}
```
# 10 scz的异世界探索之旅
这道题也有很多解法,这里用写题解的人的一种解法,根据题意可以知道,$G$和$B$是认为是相同的,那么,对于一列来说,如果说有一个是$R$,那么另一个必然一定是$R$,$B$和$G$我们可以不用管,我们可以扫一遍,然后判断就可以了
```c
#include <stdio.h>
#define N 1010
char g[3][N];
int main()
{
	int t;
	scanf("%d",&t);
	while(t--)
	{
		int n;
		scanf("%d",&n);
		for(int i=1;i<=2;i++)
		{
			scanf("%s",g[i]+1);//读入字符串//注意 这样就是从下标1开始往后读了
		}
		int is_true=1;
		for(int i=1;i<=n;i++)
		{
			if(g[1][i]=='R')
			{
				if(g[2][i]!='R')
				{
					is_true=0;
				}
			}
			if(g[2][i]=='R')
			{
				if(g[1][i]!='R')
				{
					is_true=0;
				}
			}
		}
		if(is_true) printf("YES\n");
		else printf("NO\n");
		
	}
}
```
