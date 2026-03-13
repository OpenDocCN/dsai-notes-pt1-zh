# 016：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p16 15_维数灾难笔记本第3部分.zh_en -BV1eu4m1F7oz_p16-

Now， we discussed how we moved from two dimensions up to three dimensions。

 And we saw that when we moved from2 to three dimensions。

We saw how many more values are more than one unit away from that mean value of each one of our different covariates。

Again， working between negative one and one。 And we see that before it was at 21 per cent in two dimensions and then just adding on one more covariate with the same range from negative one to 1 and the same idea that it's going to be standardized with the standard deviation of  one and a mean of 0。

 We saw that 48 per cent light outside。Now， what we want to see from there if we can generalize up to higher dimensions。

 Now， obviously， we won't be able to plot in higher dimensions。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_1.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_2.png)

But we can start to get an intuitive sense if the idea is。

 if it's within one unit away from that mean。That would mean that we are working within the ball。

 within the sphere， whatever you want to call it。 And then outside of that， still using that range。

 the similar range for each one of our different covariates。 If it's outside of that ball。

 then we would say that it was outside of that standard deviation。

 And we'd say that's a bit of an outlier using the same sized covariates。So in order to do that。

 what we're going to start with is。Here we have a random sample calling N do random do sample is just going to pull from a uniform distribution。

 random points from 0 to 1。We're saying that we once the size to be 5 rows and two columns。

 So we're going to have two dimensional points。We're then going to get the norm。Again。

 this is just the distance from 0，0。The norm is going to be that Euclidean distance from 0，0。

So the Euclidean distance is just going to be that value squared because we're moving from 0，0。

 So we square that value and then take the square root of that value squared。

 and we're calling dot sum and we're sum one just because we're going to be passing in an array。

 And we want to get that sum for each one of our individual points。

So we're getting that Euclidean distance for each one of our points。

And then we want to determine using that norm whether or not we are one unit away from that mean or if we're greater than one unit away。

 And no matter the dimensional space， that's going to be the way that we determine whether or not we are within the ball within that sphere or not。

So we're going to say。This in the ball will just say。

Is that value using our norm that we just defined within the ball or not within the ball。

And it will return either a true or false value。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_4.png)

So just to see an example of this， we're going to use that sample data， we're going to say 4 x。

 Y and zip， and we're going to zip together both the norm value so we can see what the norm value output is for each one of these sample data points。

 and then we can say whether or not that's in the ball。

 and we should see anything above one being outside the ball。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_6.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_7.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_8.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_9.png)

So we run this。 And first， we printed out our sample data that we randomly generated with all the points being between 0 and 1。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_11.png)

And we see that all of these were actually within the circle。

And here were working in two dimensional space that was a bit lucky， you see if I run this again。

 that two of them happened to be outside of the circle。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_13.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_14.png)

Now， how would we generalize this beyond two dimensions。

 so we saw we could do three dimensions bywinu do it to any number of dimensions。

So the way that we're going to do that。Is we're going to create this function called what percent of the n cube is in the N ball。

 So in the n dimensional cube is in the n dimensional ball。We pass in the number of dimensions。

We can also pass in our different sample sizes here。 we're going to use 10000。

 So we're going to generate 10000 random points。We're then going to create a random sample again。

 those will be values between 0 and 1。Using the shape of 10000 different rows。

 all with the dimensions defined by the number of dimensions we pass into this function。

So originally， we just did two dimensions， as we saw in our samples here。 Now。

 we're going to move that up to 3，4，5 dimensions。 And you can also imagine this again。

 Think that each one of these different rows。Contains our first covariate then our second covariate。

 And when we add more dimensions， all we're doing it is is adding on more features。

Adding on more dimensions。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_16.png)

So what we're going to do is we're going to call in the ball。For these 10000 different values。

And then we're going to call dot mean。 So if you think about it。

 this will be outputting either true or false for each one of these 10000 values。

True or false can be used as one and 0 with true being  one false being 0。 If we take the averageage。

 we can see what percentage actually falls within the ball。

 That's how this dot mean will work for us。And then we're saying for iteration in range 100 so that we get 100 different examples of these 10000 points to ensure that we converge on what something close to what the actual solution would be in regards to generalizing to these higher dimensions。

 So we end up with 100 different values for the average amount that lies within the ball versus outside the ball。

 And then we take the mean of those values。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_18.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_19.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_20.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_21.png)

And that will give us the percentage of the N cube that's in the end ball。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_23.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_24.png)

We're then going to call for dimensions ranging from 2 up till 15。 So not including 15。

 So up till 14。 those are going to be the different dimensions that we're going to test。And then。

 our data is going to be。For each of these， we want to pull out what percentage is in the ball。

 So we're just going to map in。These different dimensions into are what percent of the N cube is in the N ball function。

And that will output for each one of these different values in the range。

 What percentage actually lies within the cube， the circle， whatever it is。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_26.png)

嗯。You see here that we also include 2 and 3。 So we'll also be able to check compared to what we saw before。

 whether or not we have close approximations of what the actual values are。

 given the calculations that we had in regards to the actual。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_28.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_29.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_30.png)

Formulas of a sphere versus a cube and a circle versus a square。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_32.png)

So we say4 dim and percent。 So're just getting。Say start with2 and then the input for two for that data。

 We're going to map those two together and get the dimension， as well as the percent within the ball。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_34.png)

So we see that 78 per cent fall within the ball at first， which 78。5， which makes sense。

 given that we saw before that 21 per cent was outside of the ball。

 Sam with 52 per cent being in the ball for three dimensions。 We saw 48 per cent above。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_36.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_37.png)

And we see how that drops off quite dramatically as we keep increasing the number of dimensions。

 So more and more of our values， as we add on these more fee， these features。

 all with similar ranges and similar standard distributions。

 We see how many of them tend to be outliers。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_39.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_40.png)

And we can plot this finally getting a simple plot， calling PLT do plot。

 We're going to get our x label， our Y label， and just our title。

 and all we're doing is our dimensions。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_42.png)

Versus the data， which is the percentage of。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_44.png)

The amount that falls within the ball versus not。 And we can see how it steeply drops off as we add on more and more dimensions。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_46.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_47.png)

So just do double check。Our understanding， we see that this is dropping off quite dramatically。

 We're also going to measure the distance from the center of our cube to its nearest point。

 So you can see out of all those points that we have。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_49.png)

But here we're going to generate rather than 10000， just 1000 points。

We can see how many of those or out of those thousand0 points， which one's closest to the center。

 and hopefully we will see I will。Give you a little bit of a spoiler。

 We will see that that closest point will be farther and farther away as we increase the number of dimensions。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_51.png)

So this is just a bit more evidence to that same point。So we're going to pass in the dimension。

 We're going to pass in our sample size here being 100。 We're setting the default equal to 1000。

Were going to， again。Call a random sample this time， rather than 0 to 1， will subtract 0。5。

 So it's centered at 00。And then it'll be from negative 0。5 up till 0。5。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_53.png)

And then we will return the min of the norm of each one of these points。 Again。

 the norm is the distance from 0 in either direction。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_55.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_56.png)

And then in order to estimate the closest， given that dimension。

We can use that getmin distance that we just defined that will give us that minimum distance using the norm of each one of those points。

We're going to do that 100 times over。 So in the same fashion that we just did to ensure that we have a large enough sample。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_58.png)

And then we're going to return not just the average of that data。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_60.png)

But the minimum of those minimums。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_62.png)

As well as the maximum of those minimums so that we can get a bit of a range of the values in regards to how far away they are from the origin。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_64.png)

So we're going to calculate this from values ranging from 2 to 100。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_66.png)

We're then going to map those dims into that estimate closest function that we just defined above。

And we can print this out。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_68.png)

And this will take just a second to run。 And then afterwards。

 we'll also be able to plot using that same functionality that we just discussed。 So we see here。

 four dimension 6， V。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_70.png)

Average value was 0。22。 The minimum of those minimum values was 0。1。

 and the maximum of those minimum values， given that 100 different iterations of this was 0。3。

So we're going to plot those dimensions， as well as the mind data， all of the rows first column。

And then this PL T dot fill between。We're going to use that in order to plot both our min and max。

 So we'll have the range of the average values， and then we'll also be able to fill between the min and max values so we can see a bit more clearly what the range was as we increase the number of dimensions。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_72.png)

So the menes data， if you recall， is going to output three different values。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_74.png)

0 is the mean。The first column is going to be， or the second first in Python is going to be the minimum。

 and the second is going to be the max。 And I're saying alpha equal to 05 because it's going to fill between the two values。

 And we want to also see that line in between。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_76.png)

So you run this。And we can see as we increase the number of dimensions。

 how far that minimum point is from the origin， as well as that good of range that we are able to get using that fill between as well。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_78.png)

So that closes out this video。 And it gave us an opportunity to look at how we can expand up into higher dimensions。

😊。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_80.png)

With all this in mind。In the next video， we will begin to show you the effects of working with high dimensional data when you are actually trying to use your different classification algorithms that we introduced in the last course。



![](img/966b22b6f5ab13ee64957b6ca55e10e3_82.png)

![](img/966b22b6f5ab13ee64957b6ca55e10e3_83.png)

All right， I'll see you there。

![](img/966b22b6f5ab13ee64957b6ca55e10e3_85.png)