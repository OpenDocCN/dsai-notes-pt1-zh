# 009：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p09 8_K-均值算法笔记本（选修部分）第1部分.zh_en -BV1eu4m1F7oz_p9-

Welcome to our lab here on Ka meanss clustering， our first lab for Cose4。



![](img/d4aff438c72d06a49a0aeac5e43e9970_1.png)

In this course， we're going to learn how to use K means using SK learn。 So throughout this lab。

 we will run a K means algorithm。

![](img/d4aff438c72d06a49a0aeac5e43e9970_3.png)

Understand what parameters are customizable within the algorithm and then know how to use the inertia curve that we discuss in lecture to determine the optimal number of clusters。

Now， a quick overview， K means is one of the most basic clustering algorithms that we'll be working with。

 It relies on finding cluster centers to group data points based on minimizing that sum of squared  errorss between each data point and its cluster center。



![](img/d4aff438c72d06a49a0aeac5e43e9970_5.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_6.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_7.png)

So first things first， we're going to import all the necessary libraries。 We bring in nuumpy， pandas。

 seaborn mattepl lid。 And then now we're bringing in scale k meanss。

 We're going to make blobs and we'll see how this comes into play。

 and we' be very useful for playing around with k means。 And then we'll use shuffle as well。

 and we'll see that later on。😊。

![](img/d4aff438c72d06a49a0aeac5e43e9970_9.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_10.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_11.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_12.png)

We then going to just set a bunch of our parameters for our visualizations。



![](img/d4aff438c72d06a49a0aeac5e43e9970_14.png)

And then， we are going to。

![](img/d4aff438c72d06a49a0aeac5e43e9970_16.png)

Get started with creating our first simple data set here。



![](img/d4aff438c72d06a49a0aeac5e43e9970_18.png)

So in order to do this。We're going to first， create our function。



![](img/d4aff438c72d06a49a0aeac5e43e9970_20.png)

Where we have， and I'll break this down step by step。 We have our color。

 and this is similar to thinking of it as a list。

![](img/d4aff438c72d06a49a0aeac5e43e9970_22.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_23.png)

Where our color equals B， R， G， C， M， K， we can think of it looping through of B for blue。

 R for red and so on。We say alpha equal to 0。5 and that's just going to be how opaque each one of our data points are hopefully you realize at this point that we're going to be creating some type of scatter plot and then the size of each one of our points S equals 20。



![](img/d4aff438c72d06a49a0aeac5e43e9970_25.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_26.png)

We're going to call this PLT dot GcaA， which stands for get current axes and set the aspect equal to equal。

You see here just in quotes， has this string equal。

 And that's just because we're going to be using a circle。

 and we want it's going to be each unit going either in the x direction or the y direction。

 We want them to be equal to one another。 So we see that clean circle。

 You can try erasing this to see what it looks like otherwise。



![](img/d4aff438c72d06a49a0aeac5e43e9970_28.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_29.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_30.png)

We then say， if we have no clusters， so we're not clustering at all。

 then we're just going to create a scatter plot passing in our X。

 We're going to say all rows per column1。

![](img/d4aff438c72d06a49a0aeac5e43e9970_32.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_33.png)

All rows for column 2。 The color is going to be equal to just that first color。 So that be。



![](img/d4aff438c72d06a49a0aeac5e43e9970_35.png)

Our alpha is going to be equal to 0。5， and our size equal to 20。Now， if we have。



![](img/d4aff438c72d06a49a0aeac5e43e9970_37.png)

A number of clusters。 What we want to do is for each one of our different clusters， plot these out。

 And the way that we do this is we call P L T dot scatter。

 And then we say we want the X values for which our K means model came up with the label equal to I。



![](img/d4aff438c72d06a49a0aeac5e43e9970_39.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_40.png)

Whatever that we're looping through the number of clusters here。 So for the first one。

 then the second one and so on。

![](img/d4aff438c72d06a49a0aeac5e43e9970_42.png)

Then we're going to say we want that first column， and then we're going to say for all those equal to I again。

 and we want that second column。 So we get each of the two columns。

 but specifying the rows that are equal to the labels that we came up with。

And then we set it to different colors looping through each one of these colors that we have defined above。

And then we are also going to plot the actual cluster centers so we can see where those lie as well。



![](img/d4aff438c72d06a49a0aeac5e43e9970_44.png)

So we just say cluster I related to the cluster that we are currently on。

 And we say the x coordinate， as well as the y coordinate。

 So saying that first column and second column。 And then again， using that same color。

 and we're going to mark that with an x so that we can differentiate that from our actual data points。

 We're also going to make the size of that larger， we're going to say the size is equal to 100。



![](img/d4aff438c72d06a49a0aeac5e43e9970_46.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_47.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_48.png)

So to see what this looks like。We're going to crate our a here。In order to do that。

 we create this angle， which is just going to be a nupy array or its values between 0 and two times pi。

 and it's going to be 20 equally spaced points。

![](img/d4aff438c72d06a49a0aeac5e43e9970_50.png)

And we're saying we don't want the end point。 So it's going to be up to。

 but not including two times pi。

![](img/d4aff438c72d06a49a0aeac5e43e9970_52.png)

We're then going to append。Two different values together to create our x。

 to create our X within our x， our first feature and our second feature。So each of our two axes。

 where the first one is going to be the cosine of our angle。



![](img/d4aff438c72d06a49a0aeac5e43e9970_54.png)

And the second one's going to be the sign of our angle。



![](img/d4aff438c72d06a49a0aeac5e43e9970_56.png)

And this0 is just to say that we want to append these across the zero axis so that we have them。



![](img/d4aff438c72d06a49a0aeac5e43e9970_58.png)

1 alongside the other。 And then we transpose this so that we have two different columns。



![](img/d4aff438c72d06a49a0aeac5e43e9970_60.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_61.png)

So I'm going to show you quickly。 First， I will run this。



![](img/d4aff438c72d06a49a0aeac5e43e9970_63.png)

So and we display the cluster， and we see our perfect circle here。

And just to take a quick look what X looks like。

![](img/d4aff438c72d06a49a0aeac5e43e9970_65.png)

X is just going to be these two different columns with one of them being the cosine of the angle and the other one being the signine of that angle。



![](img/d4aff438c72d06a49a0aeac5e43e9970_67.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_68.png)

And now we have0 clusters because our default above was setting the number of clusters equal to 0。

 There is no K and yet， but we'll introduce K means models。 So all we're doing is plotting the x。



![](img/d4aff438c72d06a49a0aeac5e43e9970_70.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_71.png)

Now we're going to group this data into two clusters to see what it looks like。

And we use two different random states to initialize the algorithm。

And to see how we come up with different results， depending on how we initialize the algorithm。



![](img/d4aff438c72d06a49a0aeac5e43e9970_73.png)

So we set number of clusters equal to 2。We say K means。

 and we set the number of clusters equal to that number of clusters。 We set random state equal to 10。

 And we're saying we only want to initialize once。

![](img/d4aff438c72d06a49a0aeac5e43e9970_75.png)

Generally， speaking， Kas couldn't initialize a number of times and then just choose the one with the best inertia。

 Here， we're just saying choose only one time just to see the differences between two different random states。

 even though again， the defaults。 if we look here， will be to use the initialization of Kas plus plus。

 So that will ensure that it's more likely to choose far away points。

 but it will still choose different points。 So it is going to be important to either initialize a number of times。



![](img/d4aff438c72d06a49a0aeac5e43e9970_77.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_78.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_79.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_80.png)

Or to if well， I guess either way you're going to initialize a number of times， if you do more times。

 check those inertias and choose which one is best on your own。



![](img/d4aff438c72d06a49a0aeac5e43e9970_82.png)

So we call a K means on those hyperparameters that we've passed。



![](img/d4aff438c72d06a49a0aeac5e43e9970_84.png)

We call K M dot fit on x。 So now we have our Kmings model fit。

And then we can using that K means that we fit， be able to display the cluster using that function that we defined earlier。



![](img/d4aff438c72d06a49a0aeac5e43e9970_86.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_87.png)

And you can see using that K M that we came up with。

 it has an attribute to give us the different labels。As well as the different cluster centers。



![](img/d4aff438c72d06a49a0aeac5e43e9970_89.png)

So that's now available and we can create this scatter plot。So， we run this。And we see。

Are two different groupings。 And again， these groupings。

 because of the way that we created this data， it could really fall anywhere。

 There are no natural groupings。 as is why it's likely to fall in many different places。

 Given the way that we are running this。 That's why it really will not converge necessarily in the same spot。

So we see that here and we have the x marking where those centroids actually lie that should be the average of all the red dots。

 the blue X is going to be the average of all the blue dots。

 and we see how it classifies each one of those two classes。

Now setting the random state equal to 20 here。

![](img/d4aff438c72d06a49a0aeac5e43e9970_91.png)

We can see that it comes up with a very different clustering。



![](img/d4aff438c72d06a49a0aeac5e43e9970_93.png)

And if we think about it， coming back to lecture。Why are these clusters different when we run the K means twice？

And this should be obvious as we talk through it quite thoroughly as I went through each one of these different graphs。

 But it's because the starting points of the cluster centers have an impact on where these final clusters actually lie。

And again， these also are going to be clusters that don't actually probably exist。

 given how equally space each one of these points are。

 So it's very highly likely that each one of these different clusters will come up in a different place。



![](img/d4aff438c72d06a49a0aeac5e43e9970_95.png)

So I'm going to pause here。And we will continue to figure out the optimum number of clusters and how we'd actually do that using Python code。

 All right， I'll see in a bit。

![](img/d4aff438c72d06a49a0aeac5e43e9970_97.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_98.png)

![](img/d4aff438c72d06a49a0aeac5e43e9970_99.png)