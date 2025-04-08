---
title: "CF919D"
description: "solution"
pubDate: "04 08 2025"
heroImage: "/blog-placeholder-3.jpg"
---

## CF919D

### Problem

给你一个有向图，每个点有一个小写字母权值，定义一条路径的权值是这条路径出现次数最多的字母的个数，求出这个图权值最大权值的路径的权值。

### Solution

这个题很容易想到深搜，但是发现 $n\le 300000$，考虑动态规划，但是因为 $n\le 300000$，所以把我们的第二维空间卡的很死，而这道题又跟字符有关，那么很容易想到 $dp_{i,j}( 0\le j \le 25)$ 表示在第 $i$ 个位置字符为 $j$ 的答案。

考虑转移，很好想。如果当前的字符与 $j$ 相同，那么自然可以加一，否则不变，那么对于一条边：
$$
dp_{v,j}=max(dp_{v,j},dp_{u,j}+(s[v]==j))
$$
答案即为最大的一个，按照拓扑排序的顺序更新即可。

### Code

短且易读的代码。

~~~cpp
#include<bits/stdc++.h>
using namespace std;
using LL=long long;
const LL N=3e5+5;
LL n,m,f[N][30],in[N],ans,cnt,x,y;
vector<LL> g[N];
string s;
queue<LL> q;
int main(){
    cin>>n>>m;
    cin>>s;
    while(m--){
        cin>>x>>y;
        g[x].emplace_back(y);
        in[y]++;
    }
    s=' '+s;
    for(int i=1;i<=n;i++){
        if(!in[i]){
            q.push(i);
            f[i][s[i]-'a']=1;
        }
    }
    while(!q.empty()){
        LL to=q.front();q.pop();
        for(auto i : g[to]){
            for(int j=0;j<26;j++) f[i][j] = max(f[i][j],f[to][j] + LL(bool(s[i]-'a'==j))),ans=max(ans,f[i][j]);
            if(!(--in[i])){
                q.push(i);
            }
        }
        cnt++;
    }
    cout<<(cnt<n ? -1 : ans);
    return 0; 
}
~~~
