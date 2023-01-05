

# 动态规划（闫氏DP）更新中

## 闫氏DP法核心

**从集合的角度考虑动态规划问题**![动态规划模板.jpg](./动态规划(闫氏DP).assets/45680_2b2ec6f209-动态规划模板.jpg)

**划分原则**

- 不重复（数量不可重复，但max和min允许重复）
- 不遗漏

动态规划比暴搜效率更高更优

**二维坐标系**

![算法坐标系.jpg](./动态规划(闫氏DP).assets/45680_336d63e609-算法坐标系.jpg)

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
//	while (cin>>x>>y>>k,x||y||k) w[x][y]=k;
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

## 背包模型

## 状态机模型

## 状态压缩DP

## 区间DP

## 树形DP

## 数位DP

## 单调队列优化DP

## 斜率优化DP