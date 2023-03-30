# 今天我要ak!!
按照题意直接输出就好了
```c
#include <stdio.h>
int main()
{
    printf("今天我要AK\n");
}
```
# 小A的方程题
给出了三个方程式,只要按照题意,直接模拟带入就行,跟高中的函数套函数差不多,模拟题,注意要`保留小数点后三位`,对于小数来说,请使用`double`来提高精确值
```c
#include <stdio.h>
#include <math.h>

int main () {
	double x;
	scanf("%lf",&x);
	double f = 2 * x + 3;
	double g = f * f + 2 * f + 6;
	double h = sqrt(g + sqrt(f));
	printf("%.3lf\n", h);
}
```
# $a+b \ge c?$
这里考察的是进制转换,高中数学应该学过,对于一个非$十$进制的数字转换成$10$进制的数字,我们应该用每一位的权重来乘以每一位的数字,然后进行换算,如果不会进制转换,可以参考一下链接  
[进制转换](https://www.bilibili.com/video/BV1sQ4y1r7PQ?spm_id_from=333.337.search-card.all.click&vd_source=f1c89669d341702064db968ba68bdc30)
题解中的`temp[i]-'0'` 是将一个字符串数字变成一个`number`的数字,$4-3=1$可以是$4$这个数字$3$这个数字的距离是$1$,那么-'0'就相当于到'0'
这个字符串的距离,然后就发现,'1'就可以转化成$1$了,因为他到'0'的距离就是$1$
```c
#include <stdio.h>
#include <string.h>
#define N 100010
long long c;
char a[N];
char b[N];
//使用long long 防止爆int
long long get(char temp[],int len)
{
    long long ans=0;
    for(int i=0;i<len;i++)
    {
        ans=ans*6+(temp[i]-'0');//模拟权重进行运算
    }
    return ans;
    // 比如 123 是六进制,那么十进制应该是1*6^2+  2*6^1+3*6^0 从1 开始算,其实就是for循环,
    //然后这样for循环这样乘就可以了
    
    
}
int main()
{
    scanf("%s%s",a,b);
    scanf("%lld",&c);
    int len1=strlen(a);
    int len2=strlen(b);
    long long temp=get(a,len1)+get(b,len2);//分别传入长度和字符串,进行求解
    if(temp>=c) printf("YES\n");
    else printf("NO\n");
}
```
# 九九乘法表
模拟,按照题意的直接模拟就可以了,注意这里'\\t'的使用
```c
#include <stdio.h>
int main () {
    for (int i = 1; i <= 9; i ++ ) {
        for (int j = 1; j <= i; j ++ ) {
            printf("%d * %d = %d  ", i, j, i * j);
        }
        puts("");
    }
}
```
# Arcaea Problem
判断是什么三角形,高中数学我��学过,当$a^2+b^2=c^2$的时候,就是直角三角形,那么也学过$a^2+b^2>c^2$是钝角三角形,反之,是锐角三角形
```c
#include <stdio.h>
#include <math.h>

int main () {
	int a, b, c;//读入
	scanf("%d%d%d", &a, &b, &c);
	int tmp;
	if (a < b) {//这里主要是进行排序,abc要非递增的顺序进行排序
		tmp = a;
		a = b;
		b = tmp;
	}
	if (a < c) {
		tmp = a;
		a = c;
		c = tmp;
	}
	
	int res1 = a * a, res2 = c * c + b * b;
	if (res1 > res2) {
		puts("FUTURE");
	}
	else if (res1 < res2) {
		puts("PAST");
	}
	else puts("PRESENT");
}
```
# good number
其实就是求一个数字$n$的全部约数,然后进行判断就可以了  
但是这里需要一些数学知识,对于一个数字$n$,可以直接枚举$1$到$n$进行判断是否是$n$的约数,但是我们注意到当$a \times b=n$的时候,枚举到$a$的时候,$b$就是$\frac{n}{a}$那么这个数字也是$n$的约数,同时注意到如果$a \times a=n$的时候,$a$最大的数字就是$\sqrt{n}$ 所以说枚举到$\sqrt{n}$就可以了,这个就是$i \times i<=n$ 的约束条件,但是$i \times i$在$i$很大的时候就会爆$int$,所以这里我们把$i$移动到$n$这一旁边,于是就是$i \times i \leq n$ $\to$  $i \leq \frac{n}{i}$,,同时这里$\sqrt{n}$其实是一个约数,所以这里还要帕判断一下,$i \ne \frac{n}{i}$
```c
int main() 
{
    int n;
    scanf("%d", &n);
    int sum = 0;
    for (int i = 1; i <= n / i; i++) 
    {
        if (n % i == 0) 
        {
            if (i < n) sum += i;
            if (i != n / i && n / i < n) sum += n / i;
        }
    }
    if(sum==n) printf("yes\n");
    else printf("no\n")
}
```
# 简单求和问题
某位学长出的题,本来还想卡卡long long ,但是后来并没有卡,这里直接开long long 一相加就可以了
```c
#include <stdio.h>
int main()
{
    int n;
    scanf("%d",&n);
    long long ans=0;
    for(int i=0;i<2*n;i++)
    {
        long long x;
        scanf("%lld",&x);
        ans+=x;
    }
    printf("%lld\n",ans);
}
```
# scz的简单函数题
题解如图  
  ![acm可好玩了(6)题解图片](https://c2.im5i.com/2022/09/11/5XvsZ.png)
```c
#include <stdio.h>
#define N 1000010
long long f[N];//数值很大,要开long long
int main()
{
   int n;
   scanf("%d",&n);
   f[0]=0;
   f[1]=1;
   for(int i=2;i<=n;i++)
   {
       if(i%2==0) f[i]=f[i/2];
       else f[i]=f[(i-1)/2]+f[i-2];
   }
   long long ans=0;
   for(int i=1;i<=n;i++) ans+=f[i];
   printf("%lld\n",ans);
}
```
# llw和mez的不烫手山芋
小型博弈的问题,但是这个可以通过举例来模拟出来,这里假设就是$A和B$一起玩博弈,如果说  
> $n=1$的时候,就是A赢,
> $n==2$的时候,A赢
> $n==3$的时候,B赢,因为A不管拿1或者2个,B都可以直接拿完

这里发现当一个数字能够变成$3$的时候,这个时候不管取$1$还是$2$,我们都可以赢,那么如果我们是4,是否我们也是赢了? 以为我们拿$1$让他变成$3$这个状态同理,$5$的时候,我们就可以拿2 还是让让变成$3$这个状态,但是我们我们是$6$的时候,对方都可以变化使我们变成$3$这个状态,然后结果就反过来了,于是我们可以推出:对于$n \mod 3==0$,A赢,反着,B赢 注意这里题目要求反过来了,反着就可以了.
```c
#include<stdio.h>
int main()
{
    int n;
    scanf("%d",&n);
    if(n%3==0)
        printf("llwloser");
    else 
        printf("llwyyds");
}
```
# hjl在稻妻的奇妙冒险
模拟题,注意点细节就可以了~~
```c
#include<stdio.h>
int main()
{
    int a,b,c,d;
    scanf("%d%d%d%d",&a,&b,&c,&d);
    if(a==b&&b==c&&c==d)
    {
        printf("yes");
        return 0;
    }
    else
    {
        int aa,bb,cc,dd;

        //攻击第一个方块
        aa=a,bb=b,cc=c,dd=d;
        aa++;
        if(aa>3) aa-=3;
        bb++;
        if(bb>3) bb-=3;
        if(aa==bb&&bb==cc&&cc==dd)
        {
            printf("yes");
            return 0;
        }

        //2
        aa=a,bb=b,cc=c,dd=d;
        aa++;
        if(aa>3) aa-=3;
        bb++;
        if(bb>3) bb-=3;
        cc++;
        if(cc>3) cc-=3;
        if(aa==bb&&bb==cc&&cc==dd)
        {
            printf("yes");
            return 0;
        }

        //3
        aa=a,bb=b,cc=c,dd=d;
        dd++;
        if(dd>3) dd-=3;
        bb++;
        if(bb>3) bb-=3;
        cc++;
        if(cc>3) cc-=3;
        if(aa==bb&&bb==cc&&cc==dd)
        {
            printf("yes");
            return 0;
        }

        //4
        aa=a,bb=b,cc=c,dd=d;
        dd++;
        if(dd>3) dd-=3;
        cc++;
        if(cc>3) cc-=3;
        if(aa==bb&&bb==cc&&cc==dd)
        {
            printf("yes");
            return 0;
        }
    }
    printf("no");
}
```
