---
title: match human pose
layout: post
categories: object detection
excerpt: 
tags: 实习
---
### **matching human poses**

*we have the 2D joint points of two poses:*

![image](https://ws1.sinaimg.cn/large/006tNbRwly1fwbaiqp50hj31460i0h19.jpg)

So we have 2 **sets of matching points** (each consisting of 18 2D coordinates);
1. The model X (which needs to be mimicked) -left hand side 

2. The input Y (needs to be checked on matching grade) -right hand side

   Essentially this comes down to checking if the two poses (described by their 18 2D coordinates) are similar.

 [**affine transformation**](https://en.wikipedia.org/wiki/Affine_transformation):
$$
\vec{y}=f(\vec x)=A\vec x+\vec b
$$
![image](https://ws4.sinaimg.cn/large/006tNbRwly1fwbc1piramj30w00ozdpo.jpg)

 **first step**: find the transformation matrix.

 **next step** is to transform the second pose onto the model. Now we can compare the **image of the second** **pose** and the model with each other. If all their **corresponding points are adjacent**, we can conclude the two poses match. From the moment a distance exceeds a certain threshold, they’re different.

 [distance metrics](https://numerics.mathdotnet.com/distance.html)

#### The Thresholds

We will base our decision (different or similar shape) on two parameters: **the distance between two corresponding points and the rotation introduced by the affine transformation.**

**1. Rotation**

Consider a person performing handstand. This is not the same pose as another person just standing straight. Although, in some cases, it’s perfectly possible a descent affine transformation is found. When you examine the transformation matrix a bit closer, you will find rotation angle around 180°.

![image](https://ws2.sinaimg.cn/large/006tNbRwly1fwbc9vrl08j312w0goai7.jpg)

The above example illustrates the case a decent transformation is found even though this is not what we humans understand under “the same pose”. 
Fortunately the rotation angle of the torso (arms) is around 175°, so well above the accepted value.

**2. Distance**

As distance metric the **Euclidean distance** is used (there are other options, as there are many [distance metrics](https://numerics.mathdotnet.com/distance.html)). We limit the maximum Euclidean distance between two corresponding points by a determined threshold. This means, from the moment one distance exceeds a certain threshold, the whole pose does not match. Note we don’t take the sum of all euclidean distances. This is because we want to focus on **outliers (maximum values)**. From the moment one body-part is not well transformed, we can conclude the global pose doesn’t match.

算法思路：

1.feacture scaling：
$$
X=\frac{X-min(X)}{
    max(X)-min(X)}
$$
2.affine transformation(仿射变换)

3.thresholds:

```
if(最大的欧几里得距离>threshold)
{
	计算并调整角度
}
else{
	match
}
```

