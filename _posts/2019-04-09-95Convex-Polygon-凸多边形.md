---
title: 95Convex Polygon 凸多边形
layout: post
categories: 数学
excerpt: 
Tags: leetcode
---

Given a list of points that form a polygon when joined sequentially, find if this polygon is convex [(Convex polygon definition)](https://en.wikipedia.org/wiki/Convex_polygon).

**Note:**

1. There are at least 3 and at most 10,000 points.
2. Coordinates are in the range -10,000 to 10,000.
3. You may assume the polygon formed by given points is always a simple polygon[ (Simple polygon definition)](https://en.wikipedia.org/wiki/Simple_polygon). In other words, we ensure that exactly two edges intersect at each vertex, and that edges otherwise **don't intersect each other**.

**Example 1:**

```
[[0,0],[0,1],[1,1],[1,0]]

Answer: True

Explanation:
```

![img](https://leetcode.com/static/images/problemset/polygon_convex.png)

**Example 2:**

```
[[0,0],[0,10],[10,10],[10,0],[5,5]]

Answer: False

Explanation:
```

![img](https://leetcode.com/static/images/problemset/polygon_not_convex.png)

```c++
//这道题让我们让我们判断一个多边形是否为凸多边形，
//凸多边形的性质:所有的顶点角都不大于180度。
//那么我们该如何快速验证这一个特点呢，学过计算机图形学或者是图像处理的课应该对计算法线normal并不陌生吧，计算的curve的法向量是非常重要的手段，一段连续曲线可以离散看成许多离散点组成，而相邻的三个点就是最基本的单位，我们可以算由三个点组成的一小段曲线的法线方向，而凸多边形的每个三个相邻点的法向量方向都应该相同，要么同正，要么同负。
//那么我们只要遍历每个点，然后取出其周围的两个点计算法线方向，然后跟之前的方向对比，如果不一样，直接返回false。这里我们要特别注意法向量为0的情况，如果某一个点的法向量算出来为0，那么正确的pre就会被覆盖为0，后面再遇到相反的法向就无法检测出来，所以我们对计算出来法向量为0的情况直接跳过即可
//对于多边形顶点呈顺时针排列的情况，判断方式刚好相反。该算法的时间复杂度是O(n)，空间复杂度是O(1)。
class Solution {
public:
    bool isConvex(vector<vector<int>>& points) {
        long long n = points.size(), pre = 0, cur = 0;
        for (int i = 0; i < n; ++i) {
            int dx1 = points[(i + 1) % n][0] - points[i][0];
            int dx2 = points[(i + 2) % n][0] - points[i][0];
            int dy1 = points[(i + 1) % n][1] - points[i][1];
            int dy2 = points[(i + 2) % n][1] - points[i][1];
            //v1(dx1,dy1),v2(dx2,dy2)
            //v1 x v2=dx1*dy2-dy1*dx2;
            cur = dx1 * dy2 - dx2 * dy1;
            if (cur != 0) {
                if (cur * pre < 0) return false;
                else pre = cur;
            }
        }
        return true;
    }
};
```

``` c++
//POJ 2007 Scrambled Polygon(点的极角排序)

//给你一个凸多边形的n个点,其中第一个点是(0,0).其他点乱序给出,要你按逆时针顺序输出该凸多边形的所有点.同样第一个点也输出(0,0).

//分析:

//       由于是凸多边形,所以如果以(0,0)点作为起点,其他所有点与(0,0)点构成了一个向量,我们只需要按照向量的叉积排序即可.

//       两个向量A,B(它们有相同的起点(0,0) ). 如果A与B的叉积>0,那么B的另一个端点一定在A的左边. 这样排序之后, 所有点就是按逆时针顺序输出的了.
//如果是顺时针输出，则判断条件为 A与B的叉积<0
//http://poj.org/problem?id=2007
//time:o(nlog(n))
#include<cstdio>
#include<cmath>
#include<algorithm>
using namespace std;
const int maxn=100+10;
 
struct Point
{
    double x,y;
    Point(){}
    Point(double x,double y):x(x),y(y){}
}P[maxn];
typedef Point Vector;
Vector operator-(Point A,Point B)
{
    return Vector(A.x-B.x,A.y-B.y);
}
double Cross(Vector A,Vector B)
{
    return A.x*B.y-A.y*B.x;
}
bool cmp(Point A,Point B)
{
    return Cross(A-P[0],B-P[0])>0;
}
 
int main()
{
    int n=0;
    while(scanf("%lf%lf",&P[n].x,&P[n].y)==2)
    {
        ++n;
    }
    sort(P+1,P+n,cmp);
    for(int i=0;i<n;++i)
        printf("(%.0lf,%.0lf)\n",P[i].x,P[i].y);
    return 0;
}
```



