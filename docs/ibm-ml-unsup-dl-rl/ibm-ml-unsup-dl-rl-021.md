# 021：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p21 20_DBSCAN算法.zh_en -BV1eu4m1F7oz_p21-

Our next unsupervised learning algorithm we are going to cover is densely based spatial clustering of applications with noise。

And the noise part is going to be important， as this is one of the few approaches that truly clusters our data rather than partitioning it。

And it will help us find outliers rather than putting them all into different clusters。

And we'll see how in just a bit。Now， let's cover the learning goals for this section。

 In this section， we're going to discuss how this D B scan clustering algorithm actually works in finding our different clusters。



![](img/00338866e885790a9f373fc7f3679bb6_1.png)

We will also discuss the input arguments and their importance for determining our clusters。

 as well as discussing the outputs of our DB scan algorithm。



![](img/00338866e885790a9f373fc7f3679bb6_3.png)

And finally， we'll close out by discussing the strengths and weaknesses of working with the DV scan algorithm。



![](img/00338866e885790a9f373fc7f3679bb6_5.png)

So let's start here with a quick introduction to what D B scan is。 As we mentioned。

 a key part of this clustering algorithm is that it truly finds clusters of data rather than just partitioning our data and thus works better when we have noise in our data set。

We know that outliers will show up in most of our data sets。

 and in reality we should be able to create our clusters and say that these outlier points do not belong to any of these clusters。



![](img/00338866e885790a9f373fc7f3679bb6_7.png)

Now， the basics of how D B scan works is that we are working under the assumption that points in a cluster should be a certain distance from one another within a certain neighborhood。

 So we would randomly select points from these higher density regions and slowly expand our clusters。

 And as we expand， we only include points that are at a certain distance from the points that have iterly already been included within that cluster。

 Given that distance that we're using from point to point。

And the algorithm ends when no more points are of a certain distance from the clusters already identified。

 And thus all points will have been classified as either belonging to a particular cluster or otherwise。

 they would be noise。 Now， this is all high level。 And in just a few slides will make sure to visualize how this actually works in practice。



![](img/00338866e885790a9f373fc7f3679bb6_9.png)

![](img/00338866e885790a9f373fc7f3679bb6_10.png)

![](img/00338866e885790a9f373fc7f3679bb6_11.png)

![](img/00338866e885790a9f373fc7f3679bb6_12.png)

Before we get to those visualizations， though， let's talk about the inputs for D B scan。

 as these inputs will be of utmost importance to getting our clusters identified correctly。So first。

 as we've seen repeatedly with all our clustering algorithms。

 we have to define the distance metric used to define our similarity between our different points。



![](img/00338866e885790a9f373fc7f3679bb6_14.png)

Then we have to define the epsilon。As we mentioned。

 we are starting at random points and then using those points we determining if other points are within a certain distance。

 And if they are， they become part of the cluster。Now this minimum distance between the points。

Is going to be considered part of the same cluster if it's within a certain epsilon range。

 So that's going to be our epsilon。 how far away a point needs to be to be considered part of that cluster。

N clue， or often seen as min samples， which is actually the argument used for S K learn。



![](img/00338866e885790a9f373fc7f3679bb6_16.png)

And this argument， this input， will be the minimum amount of points for a particular point to be considered a core point of a cluster。



![](img/00338866e885790a9f373fc7f3679bb6_18.png)

![](img/00338866e885790a9f373fc7f3679bb6_19.png)

And core points are going to be defined by this N clue argument。



![](img/00338866e885790a9f373fc7f3679bb6_21.png)

And they're going to be defined as those points that have at least N clue neighbors。

 including itself。

![](img/00338866e885790a9f373fc7f3679bb6_23.png)

So if we set n clue equal to 3， that means that that point has at least two other neighbors that are within that epsilon distance。

A non core point can still be a part of the cluster if it's in the neighborhood of that core point。

But to understand this， let's dive a bit deeper into the different classification of points given our DB scan model。



![](img/00338866e885790a9f373fc7f3679bb6_25.png)

![](img/00338866e885790a9f373fc7f3679bb6_26.png)

So there are three possible labels for any given point。



![](img/00338866e885790a9f373fc7f3679bb6_28.png)

First， we have our core point， which we just defined as any point that has more than N clue neighbors。

And all clusters will require at least one core point。



![](img/00338866e885790a9f373fc7f3679bb6_30.png)

We then have density reachable or border points。And these will be points that can be reached by a core point。



![](img/00338866e885790a9f373fc7f3679bb6_32.png)

But may have fewer than end clue neighbours itself。



![](img/00338866e885790a9f373fc7f3679bb6_34.png)

These will still be a part of the cluster as long as they are in the Epsilon neighborhood of a core point。



![](img/00338866e885790a9f373fc7f3679bb6_36.png)

And then finally， we have noise and noise is going to be a point that is not part of any cluster。



![](img/00338866e885790a9f373fc7f3679bb6_38.png)

And that would be one that has no core points in that Epsilon neighborhood of the point。



![](img/00338866e885790a9f373fc7f3679bb6_40.png)

So if we have n clue equal to 4 and three points are within epsilon and no others are near by。

None of these three are going to be core points， unless thus they are all going to be identified as noise and again。

 will visualize this a bit more clearly in the videos to come。



![](img/00338866e885790a9f373fc7f3679bb6_42.png)

And with those possible labels， for any point， we identify clusters as the connected core and density reachable points within our data set。



![](img/00338866e885790a9f373fc7f3679bb6_44.png)

So that closes out this video and in the next video we're going to turn to that visualization。

 I keep promising that we're going to see to clearly understand how the DB scan algorithm works。

 Allright， I'll see you there。

![](img/00338866e885790a9f373fc7f3679bb6_46.png)

![](img/00338866e885790a9f373fc7f3679bb6_47.png)

![](img/00338866e885790a9f373fc7f3679bb6_48.png)