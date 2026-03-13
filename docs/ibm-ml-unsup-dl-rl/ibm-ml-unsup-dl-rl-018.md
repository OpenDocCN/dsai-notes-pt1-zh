# 018：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p18 17_层次聚合聚类.zh_en -BV1eu4m1F7oz_p18-

Now let's talk about our next clustering algorithm， hierarchical aglomative clustering。

With hierarchical aglomative clustering， we'll try to continuously split out and merge new clusters successively until we reach a level of emergence。

😊，Now， let's see how hierarchical aglom clustering actually works。



![](img/806b73729bb29eea4c9c51744fde20a0_1.png)

So here we're using the same example as before， and we're going to try and come up with our different clusters。



![](img/806b73729bb29eea4c9c51744fde20a0_3.png)

For hierarchical gllomerated clustering， we start off by looking at the points and identifying the pair。

 which has the minimal distance。

![](img/806b73729bb29eea4c9c51744fde20a0_5.png)

![](img/806b73729bb29eea4c9c51744fde20a0_6.png)

So notice here again that the distance becomes a very important factor in the success of our clustering algorithm。

 so we need to keep into account which distance metrics we're actually using。



![](img/806b73729bb29eea4c9c51744fde20a0_8.png)

![](img/806b73729bb29eea4c9c51744fde20a0_9.png)

We see that these two points that we have in green are the closest。

 so we color code them here to highlight that these are going to be our first pair。



![](img/806b73729bb29eea4c9c51744fde20a0_11.png)

![](img/806b73729bb29eea4c9c51744fde20a0_12.png)

And then we continue to do this again， looking for the next closest pair of points。



![](img/806b73729bb29eea4c9c51744fde20a0_14.png)

And the next closest pair。And we can keep doing this。



![](img/806b73729bb29eea4c9c51744fde20a0_16.png)

But the next closest pair can actually be a pair of clusters。

 So we might have two different clusters or a cluster and a point that are going to be closest to one another。

 It doesn't necessarily just have to be two different points。



![](img/806b73729bb29eea4c9c51744fde20a0_18.png)

![](img/806b73729bb29eea4c9c51744fde20a0_19.png)

![](img/806b73729bb29eea4c9c51744fde20a0_20.png)

Now， how we define the distance from a cluster to a point or from a cluster to another cluster will depend on our linkage criterion。



![](img/806b73729bb29eea4c9c51744fde20a0_22.png)

![](img/806b73729bb29eea4c9c51744fde20a0_23.png)

And we'll expand on this a bit later。 But for the distance to a particular cluster。

 maybe it's going to be the average of points in a given cluster and the distance to that average of given points。

 Or maybe it's the minimal distance between all points in a given cluster to that point。

 So just taking the minimum distance。

![](img/806b73729bb29eea4c9c51744fde20a0_25.png)

![](img/806b73729bb29eea4c9c51744fde20a0_26.png)

![](img/806b73729bb29eea4c9c51744fde20a0_27.png)

And if it's a pair of clusters， if we do find it's a cluster that is the closest point。

 then we can go ahead and merge them into their own cluster。



![](img/806b73729bb29eea4c9c51744fde20a0_29.png)

![](img/806b73729bb29eea4c9c51744fde20a0_30.png)

![](img/806b73729bb29eea4c9c51744fde20a0_31.png)

So we see that again， we don't have that。 but here we move one more step， and we see that the。



![](img/806b73729bb29eea4c9c51744fde20a0_33.png)

Blue and the green that we had merged together into the two greens。



![](img/806b73729bb29eea4c9c51744fde20a0_35.png)

And we can continue to see that we can create more and more clusters， and we keep going。

 creating each one of our pairs， moving forward。😊。

![](img/806b73729bb29eea4c9c51744fde20a0_37.png)

![](img/806b73729bb29eea4c9c51744fde20a0_38.png)

And now we see some of them merging together， merging further together。

 We have those red dots all creating their own cluster。

 And now the number of clusters will start to reduce。



![](img/806b73729bb29eea4c9c51744fde20a0_40.png)

![](img/806b73729bb29eea4c9c51744fde20a0_41.png)

![](img/806b73729bb29eea4c9c51744fde20a0_42.png)

As we keep moving forward。Each one of them， combining together。



![](img/806b73729bb29eea4c9c51744fde20a0_44.png)

And at this point， looking here were at six different clusters。



![](img/806b73729bb29eea4c9c51744fde20a0_46.png)

We can run again， and we get down to five clusters as we continue to find the closest linkage。



![](img/806b73729bb29eea4c9c51744fde20a0_48.png)

Now we're down to four。

![](img/806b73729bb29eea4c9c51744fde20a0_50.png)

And again， as we continue to move up that ladder， we can continue to merge these different clusters together。

 And then we're at three different clusters at two different clusters。

 And if we were to continue this， we can end up with one large cluster。😊。



![](img/806b73729bb29eea4c9c51744fde20a0_52.png)

![](img/806b73729bb29eea4c9c51744fde20a0_53.png)

![](img/806b73729bb29eea4c9c51744fde20a0_54.png)

So this means that if we allow this to continue， eventually， we don't have clusters。

 so we have to come up with some type of stopping criteria when we're using a glloorative clustering。



![](img/806b73729bb29eea4c9c51744fde20a0_56.png)

![](img/806b73729bb29eea4c9c51744fde20a0_57.png)

![](img/806b73729bb29eea4c9c51744fde20a0_58.png)