# 022：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p22 21_可视化DBSCAN.zh_en -BV1eu4m1F7oz_p22-

![](img/497c0acda7c4462cb2f949aec4b46030_0.png)

So as promised， let's start to visualize how DB scan actually works。



![](img/497c0acda7c4462cb2f949aec4b46030_2.png)

And as we have in our past clustering algorithms， we're going to start with this two dimensional data set and we're going to come up with clusters depending on the visits and the recency and how far away each point is from one another。

So we start at a random point here we have this point in pink。

And then we look at the radius epsilon around that point and we'll have to define that epsilon and here we define it as 1。

75。And we look， we create that 1。75 epsilon， and we look around。And we see。

 is there enough points given our N clue within that circle to start a cluster。

 And we see that there are four points。 again， we include that point itself。

 even though even with that point， we get up to 5。 So we now have our first cluster。

 So every point within that epsilon is going to be part of our first cluster。



![](img/497c0acda7c4462cb2f949aec4b46030_4.png)

![](img/497c0acda7c4462cb2f949aec4b46030_5.png)

![](img/497c0acda7c4462cb2f949aec4b46030_6.png)

![](img/497c0acda7c4462cb2f949aec4b46030_7.png)

And then we process each new point in the same way。

So we move on to our next point here and anything within that radius。

 within that epsilon radius gets included as part of that cluster。



![](img/497c0acda7c4462cb2f949aec4b46030_9.png)

![](img/497c0acda7c4462cb2f949aec4b46030_10.png)

And we keep moving along。And then here we see that this point， while it is part of the cluster。

 because it's near one of the core points。 as we looped through。

 we saw that the point to the right of this that it is part of within that Epsilon radius was a core point with four points。

 This one is not a core point。But it is density reachable， so it will be part of our cluster。

 but we will highlight that this particular point is going to be a border point。

 a density reachable point and not one of our core points。



![](img/497c0acda7c4462cb2f949aec4b46030_12.png)

So we elite that as part of a lighter pink。And we keep going down this chain， adding on points。

 according to those that fall within epsilon。

![](img/497c0acda7c4462cb2f949aec4b46030_14.png)

Keep running through and we see these are all core points because they all have at least four points including themselves in there。

And we move along and then this point only has three。 So this one again。

 is going to be a border point， but it is near one of the core points。

 so it will count as part of the cluster still。

![](img/497c0acda7c4462cb2f949aec4b46030_16.png)

![](img/497c0acda7c4462cb2f949aec4b46030_17.png)

But we see we highlight that in light pink， and we see we can keep moving along。



![](img/497c0acda7c4462cb2f949aec4b46030_19.png)

And eventually， we have。

![](img/497c0acda7c4462cb2f949aec4b46030_21.png)

All of our points within the cluster， we s to search along all the points。

 And then if there are no neighbors left， we will randomly try a new。

 unvisited point to potentially start a brand new cluster。😊。



![](img/497c0acda7c4462cb2f949aec4b46030_23.png)

And when we do that。Here we start with the blue， we we to check。

 is this going to be a core point once again？

![](img/497c0acda7c4462cb2f949aec4b46030_25.png)

So we check again with an epsilon of this new random point that we sought out。



![](img/497c0acda7c4462cb2f949aec4b46030_27.png)

We see that it is a core point， now we have started our new cluster。

Now this point again is going to be that density reachable point。

 but it will still be part of the cluster because it's near another point that is a core point。



![](img/497c0acda7c4462cb2f949aec4b46030_29.png)

And we can continue to move along to build out our cluster here。



![](img/497c0acda7c4462cb2f949aec4b46030_31.png)

And you see again， we have a density reachable point， we've had a couple so far。

 but all those are near core points， so they still are going to be。



![](img/497c0acda7c4462cb2f949aec4b46030_33.png)

Part of our cluster。And then， we see here。That we have with n clue equal to equal to 4。

 We only have three within this cluster。 So this is going to be a density reachable point。

 but not a core point。

![](img/497c0acda7c4462cb2f949aec4b46030_35.png)

And then when we move over to this point over here， we see that the only one within that radius。

Is going to be that density reachable point。 So there's no core points within this radius。



![](img/497c0acda7c4462cb2f949aec4b46030_37.png)

So if theres no points within this radius that are not core points， then this becomes a noise point。

 It becomes an outlier。 So this isn't part of either of our two clusters and is labeled as an outlier point。

 which is why we haven't marked it here in gray。

![](img/497c0acda7c4462cb2f949aec4b46030_39.png)

![](img/497c0acda7c4462cb2f949aec4b46030_40.png)

![](img/497c0acda7c4462cb2f949aec4b46030_41.png)

Now， I want you to take a moment。 and given that DB scan method that we just walked through。

 notice which points tended to be the core points as we have them labeled in a darker hue。



![](img/497c0acda7c4462cb2f949aec4b46030_43.png)

Which ones were those density reachable points， which are still part of our cluster。

 but don't have the number of points that make it a core point， given our end clue。



![](img/497c0acda7c4462cb2f949aec4b46030_45.png)

And then which point we have labeled as outlier？

![](img/497c0acda7c4462cb2f949aec4b46030_47.png)

Now that we understand how the DB scan algorithm works。

 let's discuss some strengths and weaknesses of working with the DB scan algorithm。



![](img/497c0acda7c4462cb2f949aec4b46030_49.png)

![](img/497c0acda7c4462cb2f949aec4b46030_50.png)

So as we saw with the DV scan algorithm， we not need to specify the number of clusters as DV scan will automatically determine the clusters dependent on how close points are from one another。



![](img/497c0acda7c4462cb2f949aec4b46030_52.png)

![](img/497c0acda7c4462cb2f949aec4b46030_53.png)

It also allows for noise and will not automatically determine that outliers are part of a particular cluster。



![](img/497c0acda7c4462cb2f949aec4b46030_55.png)

They'll also do a strong job of handling arbitrary shapes。

 as it's going to be searching out points that are within epsilon distance of one another and will stop whenever a gap occurs。

 no matter what that boundary shape between the clusters are。



![](img/497c0acda7c4462cb2f949aec4b46030_57.png)

![](img/497c0acda7c4462cb2f949aec4b46030_58.png)

![](img/497c0acda7c4462cb2f949aec4b46030_59.png)

Now some weaknesses。

![](img/497c0acda7c4462cb2f949aec4b46030_61.png)

It's going to require two parameters， which means we need to search over more possible values to find that optimal solution。



![](img/497c0acda7c4462cb2f949aec4b46030_63.png)

Also， those hyperparameter can be very difficult to fine tune in higher dimensional space。



![](img/497c0acda7c4462cb2f949aec4b46030_65.png)

And then finally will not do well with clusters of different density。

 so even if we have two clear groups， if for one group the points are about five units away from one another and the other is one unit away depending on our distance metric。



![](img/497c0acda7c4462cb2f949aec4b46030_67.png)

![](img/497c0acda7c4462cb2f949aec4b46030_68.png)

![](img/497c0acda7c4462cb2f949aec4b46030_69.png)

Depending on that distance between our two clusters that are on average five units away or one unit away。

 it may be difficult to determine the differentiation between those two clusters。



![](img/497c0acda7c4462cb2f949aec4b46030_71.png)

![](img/497c0acda7c4462cb2f949aec4b46030_72.png)

![](img/497c0acda7c4462cb2f949aec4b46030_73.png)

Now let's walk through how the DB scan algorithm can actually be used using Python。

 so first things first we import the class containing our clustering method。

 so from SKLn dot cluster we import DB scan。

![](img/497c0acda7c4462cb2f949aec4b46030_75.png)

We then create an instance of that class and pass in the necessary hyper parameters。 Here。

 we're setting epsilon equal to 3 and the min samples equal to 2。

 So that's that n clue that we've been talking of。 And epsilon is the epsilon we've been talking of。

 that distance from every single point in order to include it as a core point or within the cluster。



![](img/497c0acda7c4462cb2f949aec4b46030_77.png)

![](img/497c0acda7c4462cb2f949aec4b46030_78.png)

We're then going to Fibit instance on the data。So just calling Db。fit。



![](img/497c0acda7c4462cb2f949aec4b46030_80.png)

And then we can't call DB。predict because of the way that the algorithm actually works。

 if you recall it's defining the points iteratively by scanning through each one of the different points within that data。

 so it's just creating clusters within that fitted data， you can't call predict with the DB scan。



![](img/497c0acda7c4462cb2f949aec4b46030_82.png)

If you wanted to fit on a larger data set， then you just include it in that fit。

 and then you can come up with the different clusters。So we get our Db dot labels。



![](img/497c0acda7c4462cb2f949aec4b46030_84.png)

And just to note， for those labels， we're going to have class zero， class1。

 and if there's going to be an outlier， any outlier。

 as we saw can happen with the AB scan will be labeled negative one。



![](img/497c0acda7c4462cb2f949aec4b46030_86.png)

Now let's recap what we learned here in this section。In this section。

 we discuss the DB scan algorithm and how it will come up with its own clusters dependent on which points are within a certain distance of the other points。



![](img/497c0acda7c4462cb2f949aec4b46030_88.png)

We then discuss the inputs and their importance， especially that of the epsilon and N clue chosen。

 as well as the outputs and understanding the difference between a core point。

 a density reachable point， and just outliers or noise。



![](img/497c0acda7c4462cb2f949aec4b46030_90.png)

![](img/497c0acda7c4462cb2f949aec4b46030_91.png)

![](img/497c0acda7c4462cb2f949aec4b46030_92.png)

And finally， we discussed some of the algorithms strengths and weaknesses such as it being able to better determine clusters of arbitrary shapes。

 but perhaps having difficulty determining clusters that may have different densities。

Now this closes out our discussion on DB scan and in the next video we'll introduce our final clustering algorithm。

 the mean shift clustering All right， I'll see you there。



![](img/497c0acda7c4462cb2f949aec4b46030_94.png)

![](img/497c0acda7c4462cb2f949aec4b46030_95.png)