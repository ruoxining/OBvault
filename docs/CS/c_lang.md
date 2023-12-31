# C语法

## 指针

数组名是一个指针常量
指向指针的数组（二级指针）

```cpp
int a = 10;
int *p = &a;
int **p = &p;
```

指针数组与数组指针

```cpp
char ccolor[][] = {"red", "blue", "yellow"};
```

是一个二维数组，每一行都是一个一维数组，即ccolor+i是一个指针，指向第i行的一维数组，ccolor+i类型是“长度为7的一维数组”的指针。ccolor[i]是一维数组

## 指针进阶

类型名* 指针变量名

指针的值或地址值

```cpp
double a, x[10];
double *p1, *p2;
```

p1和p2都是指针：p1的基类型是double，p2的基类型是double*，此时都还没有确定地址值，需要赋值。

```cpp
// 读入一个字符串
char str[100], *s;
scanf("%s", s);

// 读入一个整数
int a, *p;
scanf("%d", p);

// 引用指针
int c, a = 2, *p;
c = 3 + *p;
```

指针的加法对地址值的影响

地址值的增量 = sizeof(基类型)

| 定义 | 基类型 | p当前指向地址 | p+1 |
| --- | --- | --- | --- |
| char \*p | char (1 byte) | 20 | 21 |
| short \*p | short (2 bytes)  | 40 | 42 |
| long \*p | long (4 bytes) | 80 | 84 |

理解运算符[]

a[3]等价于3[a]等价于*(3+a)

二维数组与行指针

- 它的每个元素都是一维数组
- a也是一个指针，它的基类型是“长度为3的int指针”
- \*(a+i) = a\[i]
- \*(a\[i] + j) = a\[i]\[j]

## 约瑟夫环 - 公式法（递推公式）

### 问题描述

N个人围成一圈，第一个人从1开始报数，报M的将被杀掉，下一个人从1开始报，最后剩下一个，求最后的胜利者。

### 普通解法

用链表模拟这个过程，N个人看作N个节点，节点1指向节点2，节点2指向节点3，节点N指向节点1.

每报到M，就删除这个数。缺点是要模拟整个游戏过程，时间复杂度就高达O(nm)。

### 公式法

递推公式

$$
f(N, M) = (f(N-1, M)+M)\%N
$$

其中f(M, N) 表示 N 个人报数，每报到 M 时杀掉这个人，最后胜利者的编号。

怎样推导该公式。

f(11, 3)

问题1: 假设知道11个人时胜利者的下标为6，下一轮10个人时胜利者的下标为：第一轮删掉编号为3的人后，所有人都往前移动了3位，胜利者也往前移动了3位，所以下标由6变3.

问题2: 假设已知10个人时，胜利者下标为3，那下一轮11个人时胜利者的下标是？

可以看作上个过程的逆过程，大家都往后移动3位，所以f(11, 3) = f(10, 3) + 3。不过有可能数组会越界，所以最后模当前人数的个数，f(11, 3) = (f(10, 3) + 3) %11

问题3: 现在改成人数为N，报到M时把人杀掉，数组是怎样移动的？每杀掉一个人，下一个人成为头，相当于把数组向前移动M位。若已知N-1个人时，胜利者的下标位置f(N-1, M)，则N个人时，就是往后移动M，考虑到数组越界，要模N，即f(N, M) = (f(N-1, M) + M) % N。

```cpp
int cir(int n, int m)
{
	int p = 0;
	for (int i=2; i<=n; i++)
	{
		p=(p+m)%i;
	}
	return p+1;
}
```