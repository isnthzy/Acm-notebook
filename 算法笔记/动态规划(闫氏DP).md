[TOC]

------

# 动态规划（闫氏DP）更新中

## 闫氏DP法核心

**从集合的角度考虑动态规划问题**![动态规划模板.jpg](./动态规划(闫氏DP).assets/45680_2b2ec6f209-动态规划模板.jpg)

**划分原则**

- 不重复（数量不可重复，但max和min允许重复）
- 不遗漏

动态规划比暴搜效率更高更优

**二维坐标系**

算法中坐标系向右为y轴，向下为x轴

## 数字三角形模型

二维图

max:[AcWing 1015. 摘花生 - AcWing](https://www.acwing.com/activity/content/problem/content/1256/)

min:[AcWing 1018. 最低通行费 - AcWing](https://www.acwing.com/activity/content/problem/content/1257/)

DP求min的时候要注意边界问题

```cpp
for(int i=1;i<=n;i++){
	for(int j=1;j<=m;j++){
        //求min的时候要特判边界
        //if(i==1&&j==1) f[i][j]=w[i][j];
        //if(i==1) f[i][j]=f[i][j-1]+w[i][j];
        //else if(j==1) f[i][j]=f[i-1][j]+w[i][j];  
        //else{
            f[i][j]=(min)max(f[i-1][j],f[i][j-1])+w[i][j];
        //}
    }
}
```





从一个点到另一个点，并且这两个点上的路径不重合，路径上有不同的值，求最大路径值

[AcWing 275. 传纸条 - AcWing](https://www.acwing.com/activity/content/problem/content/1286/)

[AcWing 1027. 方格取数 - AcWing](https://www.acwing.com/activity/content/problem/content/1258/)

```cpp
f[k][i1][i2]
//k,i1,i2,k==i1+j1==i2+j2,f[k][i1][j1]=从1,1点到[i1,j1],[i2,j2]的最大路径和 
```

方格取数板子分析

题意：在$n×n$网格选两条从左上角到右下角的路径，使得二者之和最大（路径相交的格子只计一次）(即走过的路值会变为0)

传纸条与方格取数相同

证明过程[AcWing 275. 证明传纸条为何可以使用方格取数的代码 - AcWing](https://www.acwing.com/solution/content/12389/)

结论，最优解经过的点一定不会重合

板子:

```cpp
const int N=14;
int w[N][N],f[N*2][N][N];
int main(){
	int n;
	cin>>n;
	int x,y,k;
	while(cin>>x>>y>>k){
		if(x==0&&y==0&&k==0) break;
		w[x][y]=k;
	}
//while (cin>>x>>y>>k,x||y||k) w[x][y]=k;
//k,i1,i2,k==i1+j1==i2+j2,f[k][i1][j1]=从1,1点到[i1,j1],[i2,j2]的最大路径和 
	for(int k=2;k<=n*2;k++){
		for(int i1=1;i1<=n;i1++){
			for(int i2=1;i2<=n;i2++){
				int j1=k-i1,j2=k-i2;
				if(j1>=1&&j1<=n&&j2>=1&&j2<=n){//边界条件
					int t=w[i1][j1];
					if(i1!=i2) t+=w[i2][j2]; //如果点不重合 ，就把两点和加起来
					int &x=f[k][i1][i2];
					x=max(x,f[k-1][i1-1][i2-1]+t);
					x=max(x,f[k-1][i1-1][i2]+t);
					x=max(x,f[k-1][i1][i2-1]+t);
					x=max(x,f[k-1][i1][i2]+t);
					//保留最大属性
				}
			}
		} 
	}
	cout<<f[n*2][n][n]<<endl;
	return 0;
}

```



## 最长上升子序列模型

```cpp
//求长度就把带星号的行h[i]换成1
int res=0; //res代表最大长度或价值
for(int i=1;i<=n;i++){
*	f[i]=h[i]; //求价值
	for(int j=1;j<i;j++){
		if(h[i]>h[j]){
*			f[i]=max(f[i],f[j]+h[i]);
		}
	}
	res=max(res,f[i]); 
} //从左到右的最长上升或最大上升值
```



## 背包模型

**背包问题要注意状态转移时 $j$ 的状态转移过程是正序还是逆序**

| 正序           | 逆序             |
| -------------- | ---------------- |
| 01背包二维     | 01背包一维       |
| 完全背包 all   | 多重背包二分优化 |
| 多重背包暴力法 | 分组背包 all     |



### 01背包

每件物品最多只用一次

![DP-01背包.jpg](./动态规划(闫氏DP).assets/45680_59e05a6607-DP-01背包.jpg)



二维表示

```cpp
int n,m; //n物品数量，m背包容量
int v[N],w[N]//v[N]代表体积 w[N]代表价值
int f[N][N] //状态
//f[i][j] i表示第i件物品，j表示体积
for(int i=1;i<=n;i++){
    for(int j=0;j<=m;j++){
        f[i][j]=f[i-1][j] //选i的情况下一定存在
        if(j>=v[i]) f[i][j]=max(f[i][j],f[i-1][j-v[i]]+w[i]);
	}
}
cout<<f[n][m]<<endl;
```



转化为一维（建议画图理解）

```cpp
int n,m; //n物品数量，m背包容量
int v[N],w[N]//v[N]代表体积 w[N]代表价值
int f[N] //状态
for(int i=1;i<=n;i++){
    for(int j=m;j>=v[i];j--)
        f[j]=max(f[j],f[j-v[i]]+w[i]);
	}
}
cout<<f[m]<<endl;
```



### 完全背包

每件物品有无限个

![DP-完全背包.jpg](./动态规划(闫氏DP).assets/45680_6064c89007-DP-完全背包.jpg)

**未优化:**

```cpp
int n,m; //n物品数量，m背包容量
int v[N],w[N]//v[N]代表体积 w[N]代表价值
int f[N][M];    // f[i][j]表示在考虑前i个物品后，背包容量为j条件下的最大价值
for (int i=1;i<=n;i++)
    for (int j=1;j<=m;j++)
        for (int k=0;k*v[i]<=j;k++)
            f[i][j]=max(f[i][j],f[i-1][j-k*v[i]]+k*w[i]);
```

优化过程：

```cpp
f[i,j]  =max(f[i-1,j],f[i-1,j-v]+w,f[i-1,j-2*v]+2*w,f[i-1,j-3*v]+3*w, .....)
f[i,j-v]=max(         f[i-1,j-v]  ,f[i-1,j-2*v]+  w,f[i-1,j-3*v]+2*w, .....)
由上两式，可得出如下递推关系： f[i][j]=max(f[i,j-v]+w , f[i-1,j])                      
```

**优化后:**

二维表示:

```cpp
int n,m; //n物品数量，m背包容量
int v[N],w[N]//v[N]代表体积 w[N]代表价值
int f[N][N] //状态
//f[i][j] i表示第i件物品，j表示体积
for(int i=1;i<=n;i++){
    for(int j=0;j<=m;j++){
        f[i][j]=f[i-1][j] //选i的情况下一定存在
        if(j>=v[i]) f[i][j]=max(f[i][j],f[i][j-v[i]]+w[i]);
	}
}
cout<<f[n][m]<<endl;
```



转化为一维

```cpp
int n,m; //n物品数量，m背包容量
int v[N],w[N]//v[N]代表体积 w[N]代表价值
int f[N] //状态
//f[i][j] i表示第i件物品，j表示体积
for(int i=1;i<=n;i++){
    for(int j=v[i];j<=m;j++){ //与01背包问题的区别是更新方式
        f[j]=max(f[j],f[j-v[i]]+w[i]);
	}
}
cout<<f[n][m]<<endl;
```



### 多重背包问题

每个物品数量不同

**未优化**：

```cpp
int n,m; //n物品数量，m背包容量
int v[N],w[N],s[i]//v[N]代表体积 w[N]代表价值,s[i]代表数量
int f[N][M];    // f[i][j]表示在考虑前i个物品后，背包容量为j条件下的最大价值
for (int i=1;i<=n;i++)
    for (int j=1;j<=m;j++)
        for (int k=0;k<=s[i]&&k*v[i]<=j;k++)
            f[i][j]=max(f[i][j],f[i-1][j-k*v[i]]+k*w[i]);
```

**优化后**:

贪心后--多个物品看作一个物品，使用01背包求解
时间复杂度 $O(nmlogs)$
二进制优化后：

```cpp
int n,m; //n物品数量，m背包容量
int v[N],w[N]; //v[N]代表体积 w[N]代表价值
int f[M]; //状态
cin>>n>>m;
int cnt=0;
for(int i=1;i<=n;i++){
    int a,b,s;
    cin>>a>>b>>s; 
	// 读入物品个数时顺便打包
    int k=1;      // 当前包裹大小
*   while(k<=s){
        cnt++;            // 实际物品种数
        v[cnt]=a*k;
        w[cnt]=b*k;
        s-=k;
        k*=2;             // 倍增包裹大小
    }
    if(s>0){
        // 不足的单独放一个，即C
        cnt++ ;
        v[cnt]=a*s;
        w[cnt]=b*s;
*   } //从*开始到结尾操作意思为二进制枚举
      //原本总数为s*n个
      //枚举后优化为n=n*logs个
}
n=cnt;        // 更新物品种数

// 转换成01背包问题
for(int i=1;i<=n;i++)
    for(int j=m;j>=v[i];j--)
        f[j]=max(f[j],f[j-v[i]]+w[i]);
cout<<f[m]<<endl;
```



### 分组背包问题

分n组，每一组有若干个物品，每组最多选一个物品

实际上是带有约束的01背包问题

![image-20230106192723175](./动态规划(闫氏DP).assets/image-20230106192723175.png)

题目描述：

有 N 组物品和一个容量是 V的背包。

每组物品有若干个，同一组内的物品最多只能选一个。
每件物品的体积是 $Vij$，价值是 $Wij$，其中 i 是组号，j 是组内编号。 求最大价值
输入格式第一行有两个整数 N，V，用空格隔开，分别表示物品组数和背包容量。
接下来有 N 组数据：

- 每组数据第一行有一个整数 $Si$，表示第 i 个物品组的物品数量；
- 每组数据接下来有 $Si$ 行，每行有两个整数 $Wij,Wij$用空格隔开，分别表示第 i 个物品组的第 j 个物品的体积和价值；

```cpp
int n;              // 物品总数
int m;              // 背包容量
int v[N][S];         // 重量 
int w[N][S];         // 价值
int s[N];           // 各组物品种数
int f[N];
cin>>n>>m;
// 读入数据
 for(int i=1;i<=n;i++)
 {
     cin>>s[i];
     for(int j=1;j<=s[i];j++) cin>>v[i][j]>>w[i][j]; 
 }

// 处理数据
for(int i=1;i<=n;i++){
    for(int j=m;j>=1;j--){
        for(int k=1;k<=s[i];k++){
            if(v[i][k]<=j){
                f[j]=max(f[j],f[j-v[i][k]]+w[i][k]);
            }
        }
    }
}    
cout<<f[m]<< endl;
```



## 状态机模型

## 状态压缩DP

## 区间DP

## 树形DP

## 数位DP

## 单调队列优化DP

## 斜率优化DP