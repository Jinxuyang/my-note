---
title: 算法竞赛入门经典笔记
date: 2020-03-23 15:06:21
tags:
	- 算法
categories: 
	- 学习笔记
---

[TOC]

## 数组

### 程序3-1 逆序输出

```c
#include<stdio.h>
#define maxn 105 
int a[maxn];
int main()
{
    int x, n = 0;
    while(scanf("%d", &x) == 1)
        a[n++] = x;//读取，先赋值a[n]＝x，然后执行n＝n＋1
    for(int i = n-1; i >= 1; i--)
    	printf("%d ", a[i]);//逆序输出	
    printf("%d\n", a[0]);//最后一个输出后跟换行
    return 0;
}

```

<!--more-->

**数组声明在main函数外可以开的更大**

数组不可以直接b=a，但可以使用memcpy(b,a,sizeof(int)*k),这个函数中的int需要根据数据类型不同，进行改变。这个函数包含在string.h中

### 程序3-2 开灯问题

>开灯问题。有n盏灯，编号为1～n。第1个人把所有灯打开，第2个人按下所有编号为2的倍数的开关（这些灯将被关掉），第3个人按下所有编号为3的倍数的开关（其中关掉灯
>将被打开，开着的灯将被关闭），依此类推。一共有k个人，问最后有哪些灯开着？输入n和k，输出开着的灯的编号。k≤n≤1000。
>
>样例输入：
>7 3
>样例输出：
>1 5 6 7

模拟题

```c
#include<stdio.h>
#include<string.h>
#define maxn 1010
int a[maxn];
int main()
{
    int n, k, first = 1;
    memset(a, 0, sizeof(a));//初始化数组
    scanf("%d%d", &n, &k);//n盏灯，k个人
    for(int i = 1; i <= k; i++)
    	for(int j = 1; j <= n; j++)
    		if(j % i == 0) a[j] = !a[j];//j%i==0可以计算出i的倍数，符合条件就改变灯的状态
    for(int i = 1; i <= n; i++)//循环输出
    	if(a[i]) { //灯开着就输出
            if(first) first = 0; //第一个进行特殊化处理，放在第一个的原因是，最后一个不好找
            else printf(" "); 
            printf("%d", i);
        }
    printf("\n");
    return 0;
}
```

>蛇形填数。在n×n方阵里填入1，2，…，n×n，要求填成蛇形。例如，n＝4时方阵为：
>10 11 12 1
>9   16  13  2
>8 15 14   3
>7 6 5       4

```c
#include<stdio.h>
#include<string.h>
#define maxn 20
int a[maxn][maxn];
int main()
{
    int n, x, y, tot = 0;
    scanf("%d", &n);//读方阵边长
    memset(a, 0, sizeof(a));
    tot = a[x=0][y=n-1] = 1;
    /*
    上面这句其实就是
    x = 0;
    y = n-1;
    a[x][y] = 1;  
    tot = 1;
    */
    while(tot < n*n){ //向四个方向移动，先判断是否到达边界，是否已经来过，再移动，再赋值，
        while(x+1<n && !a[x+1][y]) a[++x][y] = ++tot;
        while(y-1>=0 && !a[x][y-1]) a[x][--y] = ++tot;
        while(x-1>=0 && !a[x-1][y]) a[--x][y] = ++tot;
        while(y+1<n && !a[x][y+1]) a[x][++y] = ++tot;
    }
    for(x = 0; x < n; x++){
        for(y = 0; y < n; y++) printf("%3d", a[x][y]);
        printf("\n");
    }
    return 0;
}

```

**在很多情况下，最好是在做一件事之前检查是不是可以做，而不要做完再后悔。因为“悔棋”往往比较麻烦。**

**细心的读者也许会发现这里的一个“潜在bug”：如果越界，x+1会等a[x+1][y]将访问非法内存！幸运的是，这样的担心是不必要的。“&&”是短路运算符（还记得我们在哪里提到过吗？）。如果x+1<n为假，将不会计算“!a[x+1][y]”，也就不会越界了。**



## 字符数组





### 程序3-4　竖式问题

> 竖式问题。找出所有形如abc*de（三位数乘以两位数）的算式，使得在完整的竖式中，所有数字都属于一个特定的数字集合。输入数字集合（相邻数字之间没有空格），输出所有竖式。每个竖式前应有编号，之后应有一个空行。最后输出解的总数。具体格式见样例输出(为了便于观察，竖式中的空格改用小数点显示，但所写程序中应该输出空格，而非小数点）。
> 样例输入：
> 2357
> 样例输出：
> <1>
> ..775
>
> X..33
>
> .2325
>
> 2325.
>
> 25575
> The number of solutions = 1

伪代码

```c
char s[20];
int count = 0;
scanf("%s", s);
for (int abc = 111; abc <= 999; abc++)
	for (int de = 11; de <= 99; de++)
		if ("abc*de"是个合法的竖式)
		{
			printf("<%d>\n", count);
			打印abc*de的竖式和其后的空行
			count++;
		}
printf("The number of solutions = %d\n", count);

```



```c
#include<stdio.h>
#include<string.h>
int main()
{
	int count = 0;
	char s[20], buf[99];
	scanf("%s", s);
	for (int abc = 111; abc <= 999; abc++)
		for (int de = 11; de <= 99; de++)
		{
			int x = abc * (de % 10), y = abc * (de / 10), z = abc * de;//模拟竖式计算过程
			sprintf(buf, "%d%d%d%d%d", abc, de, x, y, z);//把这个竖式 的所有字符放入buf中
			int ok = 1;
			for (int i = 0; i < strlen(buf); i++)//遍历buf的所有字符
				if (strchr(s, buf[i]) == NULL) ok = 0;//如果buf里有一个字符在s中找不到，就不符合要求，ok=0不输出
			if (ok)
			{
				printf("<%d>\n", ++count);
				printf("%5d\nX%4d\n-----\n%5d\n%4d\n-----\n%5d\n\n", abc, de, x, y, z);
			}
		}
	printf("The number of solutions = %d\n", count);
	return 0;
}

```

#### strchr():

​	原型：

​		char *strchr(const char *str, int c)

​	参数：

​		str--要被检索的C字符串

​		c--在str中要搜索的字符

​	返回值：

​		该函数返回在字符串 str 中第一次出现字符 c 的位置，如果未找到该字符则返回 NULL。

#### sprintf()：

​	将格式化的数据写入字符串	

​	原型：

​		 int sprintf(char *str, char * format [, argument, ...]);

​	参数：		

​		str为要写入的字符串；format为格式化字符串，与printf()函数相同；argument为变量

​	返回值：

​			成功则返回参数str 字符串长度，失败则返回-1

#### strlen()：

​	返回字符串实际长度

#### scanf("%s", s)

​	使用该函数读取字符串，当遇到空格或TAB时，不会将他们读入

## 竞赛题目选讲



### 例题3-2　WERTYU（WERTYU, UVa10082）

> 把手放在键盘上时，稍不注意就会往右错一位。这样，输入Q会变成输入W，输入J会变成输入K等。键盘如图3-2所示。
> 输入一个错位后敲出的字符串（所有字母均大写），输出打字员本来想打出的句子。输入保证合法，即一定是错位之后的字符串。例如输入中不会出现大写字A。
> 样例输入：
> O S, GOMR YPFSU/
> 样例输出：
> I AM FINE TODAY.

```c
#include<stdio.h>
char s[] = "`1234567890-=QWERTYUIOP[]\\ASDFGHJKL;'ZXCVBNM,./";//常量数组，真好！！！
int main() {
	int i, c;
	while ((c = getchar()) != EOF) {
		for (i = 1; s[i] && s[i] != c; i++); //找错位之后的字符在常量表中的位置
		if (s[i]) putchar(s[i - 1]); //如果找到，则输出它的前一个字符
		else putchar(c);
	}
	return 0;
}

```

**主要是常量数组的使用**

> 输入一个字符串，判断它是否为回文串以及镜像串。输入字符串保证不含数字0。所谓回文串，就是反转以后和原串相同，如abba和madam。所有镜像串，就是左右镜像之后和原串相同，如2S和3AIAE。注意，并不是每个字符在镜像之后都能得到一个合法字符。在本题中，每个字符的镜像如图3-3所示（空白项表示该字符镜像后不能得到一个合法字符）。
> ![8qbomF.png](https://s1.ax1x.com/2020/03/24/8qbomF.png)
> 输入的每行包含一个字符串（保证只有上述字符。不含空白字符），判断它是否为回文
> 串和镜像串（共4种组合）。每组数据之后输出一个空行。
> 样例输入：
> NOTAPALINDROME
> ISAPALINILAPASI
> 2A3MEAS
> ATOYOTA
> 样例输出：
> NOTAPALINDROME -- is not a palindrome.
> ISAPALINILAPASI -- is a regular palindrome.
> 2A3MEAS -- is a mirrored string.
> ATOYOTA -- is a mirrored palindrome.

```c
#include<stdio.h>
#include<string.h>
#include<ctype.h>
const char* rev = "A   3  HIL JM O   2TUVWXY51SE Z  8 ";
const char* msg[] = {"not a palindrome", "a regular palindrome", "a mirrored string",
char r(char ch) {
	if (isalpha(ch)) return rev[ch - 'A'];//如果ch是个字母，就返回ch-'A'(这个的意思就是在rev中的序号)
	return rev[ch - '0' + 25];
}
int main() {
	char s[30];
	while (scanf("%s", s) == 1) {
		int len = strlen(s);//读取字符串实际长度
		int p = 1, m = 1;
		for (int i = 0; i < (len + 1) / 2; i++) {//只要一半符合要求即可确定，因为回文串和镜像串从中间分开，两头都存在一定的关系，因此i < (len + i) / 2
			if (s[i] != s[len - 1 - i]) p = 0; //不是回文串,len-1-i实现倒序
			if (r(s[i]) != s[len - 1 - i]) m = 0; //不是镜像串
		}
		printf("%s -- is %s.\n\n", s, msg[m * 2 + p]);
	}
	return 0;
}

```

**头文件ctype.h中定义的isalpha，isdigit，isorint可以用来判断字符的属性，toupper，tolower可以用来转换大小写**

### 例题3-4　猜数字游戏的提示（Master-Mind Hints, UVa 340）

>实现一个经典"猜数字"游戏。给定答案序列和用户猜的序列，统计有多少数字位置正确（A），有多少数字在两个序列都出现过但位置不对（B）。
>输入包含多组数据。每组输入第一行为序列长度n，第二行是答案序列，接下来是若干猜测序列。猜测序列全0时该组数据结束。n=0时输入结束。
>
>样例输入：
>4
>1 3 5 5
>1 1 2 3
>4 3 3 5
>6 5 5 1
>6 1 3 5
>1 3 5 5
>0 0 0 0
>10
>1 2 2 2 4 5 6 6 6 9
>1 2 3 4 5 6 7 8 9 1
>1 1 2 2 3 3 4 4 5 5
>1 2 1 3 1 5 1 6 1 9
>1 2 2 5 5 5 6 6 6 7
>0 0 0 0 0 0 0 0 0 0
>
>样例输出：
>Game 1:
>(1,1)
>(2,0)
>(1,2)
>(1,2)
>(4,0)
>Game 2:
>(2,4)
>(3,2)
>(5,0)
>(7,0)
>
>

```c
#include<stdio.h>
#define maxn 1010
int main() {
	int n, a[maxn], b[maxn];
	int kase = 0;
	while (scanf("%d", &n) == 1 && n) { //n=0时输入结束
		printf("Game %d:\n", ++kase);
		for (int i = 0; i < n; i++) scanf("%d", &a[i]);//读入答案
		for (;;) {
			int A = 0, B = 0;
			for (int i = 0; i < n; i++) {
				scanf("%d", &b[i]);//读入用户猜的序列
				if (a[i] == b[i]) A++; //相同位置，且相同的用A来记
			}
			if (b[0] == 0) break; //正常的猜测序列不会有0，所以只判断第一个数是否为0即可
			for (int d = 1; d <= 9; d++) {
				int c1 = 0, c2 = 0; //统计数字d在答案序列和猜测序列中各出现多少次
				for (int i = 0; i < n; i++) {
					if (a[i] == d) c1++;
					if (b[i] == d) c2++;
				}
				if (c1 < c2) B += c1; else B += c2;
			}//刘老师真是nb，这里把答案数组和用户数组的每一位拿出来和1-9分别比较，统计每一个数字出现的次数，然后输出两个计数器中值小的一个（一旦小的都大于等于1，就说明两个数列中都含有这个数）
            1355
            1123
			printf(" (%d,%d)\n", A, B - A);//B-A总数-位置相同的，就是。。。
		}
	}
	return 0;
}

```

### 例题3-5　生成元（Digit Generator, ACM/ICPC Seoul 2005, UVa1583）

> 如果x加上x的各个数字之和得到y，就说x是y的生成元。给出n（1≤n≤100000），求最小生成元。无解输出0。例如，n=216，121，2005时的解分别为198，0，1979。

**为了节省空间，提升效率，可以一次性把100000内所有数字的生成元计算出来，然后输出时直接调用**

**一个数的生成元可能有多个**



```c
#include<stdio.h>
#include<string.h>
#define maxn 100005
int ans[maxn];
int main() {
	int T, n;
	memset(ans, 0, sizeof(ans));//初始化
	for (int m = 1; m < maxn; m++) {//
		int x = m, y = m;
		while (x > 0) { y += x % 10; x /= 10; }//把x拆分，各项相加
		if (ans[y] == 0 || m < ans[y]) ans[y] = m;//把这个数的生成元存到数组里，下标为数字，值为生成元
        //有点难理解，可以让m=198自行尝试一下，就会发现其实这个程序是倒着来的，是已知某个数的生成元，然后再计算出这个数
        //一个数的生成元是不唯一的，因此需要ans[y] == 0 || m < ans[y]进行限制
	}
	scanf("%d", &T);
	while (T--) {
		scanf("%d", &n);

		printf("%d\n", ans[n]);
	}
	return 0;
}

```

### 例题3-6　环状序列（Circular Sequence, ACM/ICPC Seoul 2004, UVa1584）

> 长度为n的环状串有n种表示法，分别为从某个位置开始顺时针得到。例如，图3-4的环状串有10种表示：
>
> [![8juuRJ.png](https://s1.ax1x.com/2020/03/25/8juuRJ.png)](https://imgchr.com/i/8juuRJ)
>
> CGAGTCAGCT，GAGTCAGCTC，AGTCAGCTCG等。在这些表示法中，字典序最小的称为"最小表示"。
> 输入一个长度为n（n≤100）的环状DNA串（只包含A、C、G、T这4种字符）的一种表示法，你的任务是输出该环状串的最小表示。例如，CTCC的最小表示是CCCT，CGAGTCAGCT的最小表示为AGCTCGAGTC。

**我理解能力不行，基本上每个题都要看原题才看得懂（可以上vj上看）**

**字典序（lexicographical order）：就是字符串在字典中的排序。abc比bcd小，hi比his小，1，2，4，7比1，2，5小**

```c
#include<stdio.h>
#include<string.h>
#define maxn 105
//环状串s的表示法p是否比表示法q的字典序小
int less(const char* s, int i, int ans) {
	int n = strlen(s);
	for (int j = 0; j < n; i++)
		if (s[(i + j) % n] != s[(ans + j) % n])
            1 0/2 1/3 2
			return s[(i + j) % n] < s[(ans + j) % n];
	return 0; //相等
}
int main() {
	int T;
	char s[maxn];
	scanf("%d", &T);
	while (T--) {
		scanf("%s", s);
		int ans = 0;
		int n = strlen(s);
		for (int i = 1; i < n; i++)
			if (less(s, i, ans)) ans = i;
		for (int i = 0; i < n; i++)
			putchar(s[(i + ans) % n]);
		putchar('\n');
	}
	return 0;
}

```

## 习题

### 习题3-1　得分（Score, ACM/ICPC Seoul 2005, UVa1585）

题目地址：https://vjudge.net/problem/UVA-1585

```c
#include<stdio.h>
int main(int argc, char const *argv[])
{
	int T;
	char x;
	scanf("%d", &T);
	while (T--) {
		int i = 0, res = 0, temp = 1;
		while ((x = getchar()) != '\n') {
			if (x = 'O') {
				res += temp;
				temp++;
			}
			else {
				temp = 1;
			}
		}
	}
	printf("%d\n", res);
	return 0;
}
```

### 习题3-2　分子量（Molar Mass, ACM/ICPC Seoul 2007, UVa1586）

**又臭又长的烂代码，但是至少可以安慰自己这个代码好理解把，哈哈哈**

```c
#include<stdio.h>
#include<ctype.h>
#include<string.h>
const float ex[] = {67, 12.01, 72, 1.008, 79, 16.00, 78, 14.01} ;
int main(int argc, char const *argv[])
{
	int T;
	scanf("%d", &T);
	while (T--) {
		char s[100];
		scanf("%s", s);
		int len = strlen(s);
		double res = 0.0;
		for (int i = 0; i < len; i++) {
			if (isalpha(s[i])) {
				if (!isalpha(s[i + 1]) && i + 1 < len) {
					int n = 1;
					while (!isalpha(s[i + n]) && i + n < len) {
						n++;
					}
					int temp1 = 1, temp2 = 0;
					for (int j = i + n - 1; j > i; j--) {
						temp2 += (s[j] - 48) * temp1;
						temp1 *= 10;
					}

					for (int j = 0; j < 8; j++) {
						if (s[i] == ex[j]) {
							res += ex[j + 1] * temp2;
							break;
						}
					}
				}
				else {
					for (int j = 0; j < 8; j++) {
						if (s[i] == ex[j]) {
							res += ex[j + 1];
							break;
						}
					}
				}
			}
		}
		printf("%.3lf\n", res);
	}
	return 0;
}
```

**这题考察的应该是ctype.h里的函数的应用，isalpha，isdigit，当时没想到isdigit这个函数，所以写的比较复杂**

**在vj的评论区看到了29行的代码，牛皮**

### 习题3-3　数数字（Digit Counting , ACM/ICPC Danang 2007, UVa1225）

```c
#include <stdio.h>
char x[10010];
int main(int argc, char const *argv[])
{
	int T;
	scanf("%d", &T);
	while (T--) {
		for (int i = 0; i <= 10005; i++) {
			x[i] = i;
		}
		int d;
		scanf("%d", &d);
		int q = 0, w = 0, e = 0, r = 0, t = 0, y = 0, u = 0, j = 0, o = 0, p = 0;
		for (int i = 1; i <= d; i++) {
			int temp = i;
			while (temp) {
				switch (temp % 10) {
				case 0: q++; break;
				case 1: w++; break;
				case 2: e++; break;
				case 3: r++; break;
				case 4: t++; break;
				case 5: y++; break;
				case 6: u++; break;
				case 7: j++; break;
				case 8: o++; break;
				case 9: p++; break;
				}
				temp /= 10;
			}
		}
		printf("%d %d %d %d %d %d %d %d %d %d\n", q, w, e, r, t, y, u, j, o, p);
	}
	return 0;
}
```

### 习题3-4　周期串（Periodic Strings, UVa455）

搞我心态。。

![GCYz7V.png](https://s1.ax1x.com/2020/03/27/GCYz7V.png)

..到网上随便找了个oj，贡献了一发WA，改了改代码，再到vj上就就提交上去了，哈哈哈vj这是为了我好！！！

```c
#include <stdio.h>
#include <string.h>
int main(int argc, char const *argv[])
{
	int T;
	scanf("%d", &T);
	while (T--) {
		char s[100];
		scanf("%s", s);
		int len = strlen(s), count = 1;
		while (count <= len) {
			int ok = 1;
			for (int j = 0; j < len ; j++) {
				if (s[j] != s[j % count] || len % count != 0) {
					ok = 0;
					break;
				}
			}
			if (ok) {
				printf("%d\n", count);
				break;
			}
			count++;
		}
	}
	return 0;
}
```

**这个代码在vj上没过，在台湾那个oj上过了，不得不说vj真是严格**

**调整了一下格式总算AC了**

```c
#include <stdio.h>
#include <string.h>
int main(int argc, char const *argv[])
{
	int T;
	scanf("%d", &T);
	while (T--) {
		char s[100];
		scanf("%s", s);
		int len = strlen(s), count = 1;
		while (count <= len) {
			int ok = 1;
			for (int j = 0; j < len ; j++) {
				if (s[j] != s[j % count] || len % count != 0) {
					ok = 0;
					break;
				}
			}
			if (ok) {
				if (T) {
					printf("%d\n\n", count);
				}
				else printf("%d\n", count);
				break;
			}
			count++;
		}
	}
	return 0;
}
```





### 习题3-5　谜题（Puzzle, ACM/ICPC World Finals 1993, UVa227）

**这个代码没AC，格式有问题**

**写了一中午，在vj上获得两发persentation error之后脑子炸裂决定放弃**



```c
#include <stdio.h>
#include<string.h>
int main(int argc, char const *argv[])
{
	int count = 0;
	int tag = 1;
	while (1) {
		char f[5][5],temp;
		int x,y; 
		count++;
		for (int i = 0; i < 5; i++) {
			for (int j = 0; j < 5; j++) {
				if((temp = getchar()) != '\n'){
					if (temp == 'Z') return 0;
					f[i][j] = temp;	
					if (f[i][j] == ' ') {
						x = i;
						y = j;
					}
				}
				else j--; 
			}
		}
		char op = '0';
		int ok = 1;
		while ((op = getchar()) != '0') {
			switch (op) {
			case 'A':
				if (x-1 >= 0 ) {f[x][y] = f[x-1][y]; f[x-1][y] = ' '; x = x - 1;}
				else ok = 0;
				break;

			case 'B':
				if (x+1 < 5 ) {f[x][y] = f[x+1][y]; f[x+1][y] = ' '; x = x + 1;}
				else ok = 0;
				break;

			case 'L':
				if (y-1 >= 0) {f[x][y] = f[x][y-1]; f[x][y-1] = ' '; y = y - 1;}
				else ok = 0;
				break;
			case 'R':
				if (y + 1 < 5) {f[x][y] = f[x][y+1]; f[x][y+1] = ' '; y = y + 1;}
				else ok = 0;
				break;
			default: break;
			}
		}
		int isfil = 1, isfic = 1;
		
		if (ok) {
			if(tag){
				printf("Puzzle #%d:\n", count);
				tag = 0;
			}
			else printf("\nPuzzle #%d:\n", count);
			
			for (int i = 0; i < 5; i++) {
				if(!isfil) isfic = 1;
				else isfil = 0;
				for (int j = 0; j < 5; j++) {
					if (isfic) {
						printf("%c", f[i][j]);
						isfic = 0;
					}
					else printf(" %c", f[i][j]);
				}
				printf("\n");
			}
		}
		else{
			if(tag){
				printf("Puzzle #%d:\n", count);
				printf("This puzzle has no final configuration.\n\n");
				tag = 0;
			}
			else {
				printf("\nPuzzle #%d:\n", count);
				printf("This puzzle has no final configuration.\n\n");
				
			}
			
		} 
	}
	return 0;
}
```

## 函数和递归

### 例题4-3　救济金发放（The Dole Queue, UVa 133）

> n(n<20)个人站成一圈，逆时针编号为1～n。有两个官员，A从1开始逆时针数，B从n开始顺时针数。在每一轮中，官员A数k个就停下来，官员B数m个就停下来（注意有可能两个官员停在同一个人上）。接下来被官员选中的人（1个或者2个）离开队伍。
> 输入n，k，m输出每轮里被选中的人的编号（如果有两个人，先输出被A选中的）。例如，n=10，k=4，m=3，输出为4 8, 9 5, 3 1, 2 6, 10, 7。注意：输出的每个数应当恰好占3列。

```c
#include<stdio.h>
#define maxn 25
int n, k, m, a[maxn];
//逆时针走t步，步长是d（-1表示顺时针走），返回新位置
int go(int p, int d, int t) { 
	while(t--) {//走t布
		do { 
            p = (p+d+n-1) % n + 1; //循环数组
        } while(a[p] == 0); //走到下一个非0数字，是零的说明那个人都已经走了
	}
return p;
}
int main() {
    while (scanf("%d%d%d", &n, &k, &m) == 3 && n) {
        for (int i = 1; i <= n; i++) a[i] = i;//循环，初始化数组
        int left = n; //还剩下的人数
        int p1 = n, p2 = 1;
        while (left) {
            p1 = go(p1, 1, k);//从p1开始，逆时针遍历数组
            p2 = go(p2, -1, m);//顺时针
            printf("%3d", p1); //输出函数的返回值
            left--;//剩余人数-1
            if (p2 != p1) { //如果不是同一人
                printf("%3d", p2); //再打印第二个人
                left--; //人数-1
            }
            a[p1] = a[p2] = 0;//赋为0，表示两人已离开，不参与下次循环，遇到就直接跳过
            if (left) printf(",");//最后一个不打印，
        }
        printf("\n");
    }
    return 0;
}

```



### 例题4-4　信息解码（Message Decoding, ACM/ICPC World Finals 1991, UVa 213）

## C++和STL

### 引用(与指针类似比指针弱)

直接再参数名之前加上一个&，再函数内修改参数的值，也会修改函数的实参

```c++
#include<iostream>
using namespace std;
void swap2(int& a, int& b) {//直接在参数名之前加一个"&"，即可
    int t = a; a = b; b = t;
}
int main() {
    int a = 3, b = 4;
    swap2(a, b);
    cout << a << " " << b << "\n";
    return 0;
}

```

### 字符串

C++中提供了一个string类型使处理字符数组更加简便，cin/cout可以直接读写string，还可以像整数一样相加

> 输入数据的每行包含若干个（至少一个）以空格隔开的整数，输出
> 每行中所有整数之和。如果只能使用字符与字符数组，一般有两种方案：一是使用getchar( )边读边算，代码较短，但容易写错，并且相对较难理解(5)；二是每次读取一行，然后再扫描该行的字符，同时计算结果。如果使用C＋＋，代码可以很简单。

```C++
#include<iostream>
#include<string>
#include<sstream>
using namespace std;
int main() {
    string line;
    while (getline(cin, line)) {
        int sum = 0, x;
        stringstream ss(line);
        while (ss >> x) sum += x;
        cout << sum << "\n";
    }
    return 0;
}
```

string类在string头文件中，而stringstream在sstream头文件中

### STL



## 一个总结 

1. 对于需要输入多组数据的题目，当第一组数据处理完之后，一定要及时初始化各种变量，因此，定义变量时，尽量把变量定义在需要用的地方的附近。
2. 对于一些需要特定格式的题目，例如需要使用空格隔开字母，但最后一个字母后不能有空格，如果直到一共可以输出多少个，既可以做一个判断，最后一个字母后不接空格，如果不知道要输出多少个，可以把空格放在每个字母之前，对第一个进行特殊化