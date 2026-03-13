# 078：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p78 39_彩色图像的卷积.zh_en -BV1eu4m1F7oz_p78-

Now， to bring this all home when you're working with images， generally speaking。

 most of our images will not just be on the gray scale， but rather have color。



![](img/a1b311aaf018b13db63ddc0d69ce4490_1.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_2.png)

And for a color image should be represented numerically。It will be。 It will have to be  three。

 generally speaking， most common is three，2 dimensional arrays， all stacked one on top of the other。

 As we see here， where each one of these two dimensional arrays represents either the red scale。

 the green scale or the blue scale respectively。

![](img/a1b311aaf018b13db63ddc0d69ce4490_4.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_5.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_6.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_7.png)

Now to move our kernels to three dimensions。

![](img/a1b311aaf018b13db63ddc0d69ce4490_9.png)

Rather than using the convolution operation， using just this kernel that's three by three。



![](img/a1b311aaf018b13db63ddc0d69ce4490_11.png)

We're going to use convolutions on a filter， filters the term once we move up to three dimensions。

 which may be three by three by3， so it's going to be three，3 by three kernels all stacked together。



![](img/a1b311aaf018b13db63ddc0d69ce4490_13.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_14.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_15.png)

So that instead of having nine multiplications added together to get our one output。

 we had the sum of 27 multiplications。

![](img/a1b311aaf018b13db63ddc0d69ce4490_17.png)

9ine and you can think about the filters that we learned where we'll have nine for each one of these different dimensions。

 so9 for red，9 for green and9 for blue， and we get we multiply those respectively to each one of their different components within that input image to get our one centered output。

 so we're adding together 27 different multiplications。



![](img/a1b311aaf018b13db63ddc0d69ce4490_19.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_20.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_21.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_22.png)

![](img/a1b311aaf018b13db63ddc0d69ce4490_23.png)

So once we use that filter， we end up with just a。

![](img/a1b311aaf018b13db63ddc0d69ce4490_25.png)

We'll go back to two dimensional output rather than these three dimensions。



![](img/a1b311aaf018b13db63ddc0d69ce4490_27.png)

Now， something that you may have noticed as we went through this idea of working with convolutions。



![](img/a1b311aaf018b13db63ddc0d69ce4490_29.png)

Is that when we work with these centered values and we're trying to output centered values？



![](img/a1b311aaf018b13db63ddc0d69ce4490_31.png)

The edges of our image and the corners of our image tend to get somewhat overlooked。



![](img/a1b311aaf018b13db63ddc0d69ce4490_33.png)

So in the next video， we're going to address this problem and introduce the concept of padding。



![](img/a1b311aaf018b13db63ddc0d69ce4490_35.png)

All right， I'll see you there。

![](img/a1b311aaf018b13db63ddc0d69ce4490_37.png)