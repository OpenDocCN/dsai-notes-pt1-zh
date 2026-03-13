# 116：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p116 77_RL笔记本（选修部分）第3部分.zh_en -BV1eu4m1F7oz_p116-

In this video， we're going to go about actually predicting what the next step should be。

So in order to do that， we're actually going to model using those observations that we just built。

 using that data frame that we just built。So we're going to create somewhat of a supervised learning problem now。

And we're going to import random forests in our extra trees reggressor。

 and if we recall extra treesre reggressor is just going to be a more randomized version of random force where the splits are going to be random。

So we're going to set our extra cheese reggresser。To our model， set the number of estimators。

We're then going to set what our target value is going to be。So we need to create a Y variable。

Given the information that we have。And the way that we're going to do that is we're going to await some of the different rewards that we had within our data frame。

So we had。The actual reward at each step。And recall that's only going to be available really at that final step if we did end up reaching that reward。

We'll do 0。1 times the decay reward and recall that that's going to more heavily weight those actions that are going to lead closer to that final reward。

 and then we're going to put the most weight to the total reward。

 whether or not any step along the way led to us ultimately getting reward within that observation within that episode。

And then our x variable， our x data that we care about is just going to be the observations and the action taken at that observation。

 So given the state that we're at。What action did we take and deciding given that action。

 whether or not that led to higher or lower reward？So we fit that。



![](img/48dc51cc564f14dee265d737e3ae4d64_1.png)

Given our X and Y。

![](img/48dc51cc564f14dee265d737e3ae4d64_3.png)

And that gives us steps to take along the way。And it gives us an outcome of given the X values that went in。

 what is going to be the predicted y。And then ultimately， if we think about it， the goal will be。

Take the observation that we're currently at。Pass each one of the optional actions that we have。

And then return which one of those actions resulted in our model， giving the highest output。

 So let's see that here。

![](img/48dc51cc564f14dee265d737e3ae4d64_5.png)

So we have our model here we're going to use random forcegresser。

 we're using that same Y and that same X that we just discussed， and then we're fitting our model。



![](img/48dc51cc564f14dee265d737e3ae4d64_7.png)

![](img/48dc51cc564f14dee265d737e3ae4d64_8.png)

We're then going to do 500 different episodes this time just to see the results。



![](img/48dc51cc564f14dee265d737e3ae4d64_10.png)

We don't need to worry about this right now。

![](img/48dc51cc564f14dee265d737e3ae4d64_12.png)

And then we're going to save our life memory as we did before。

 and all of these steps are very similar to before we have our initial observation for our episode。

We're not done yet， so our done is equal to false until we actually get to done。

 and our total reward is zero， and we have this blank episode memory for this specific episode。Now。

 here's our first major difference。Our predictions are going to take all the old observation。

As well as I for I in range 4， recall that we only have four different steps and if we look at。

 say some old observation。

![](img/48dc51cc564f14dee265d737e3ae4d64_14.png)

We can see usually that we'll say here that's at five。

And we can actually take the model that we fit above。 Well， actually， let's just do this first。

We can see what our。Data is， we we're saying。Passing in this tuple and recall that this tuple should relate to the observation in the action。

 so we're at observation 5， we can either take step  zero。Or action zero， take action 1。

 take action 2 or take action 3 when we're at observation5。



![](img/48dc51cc564f14dee265d737e3ae4d64_16.png)

We then。Call our model and call predict for each one of these different tuples。And when we do that。

 we have that model that we did above， we can actually call model dot predict here then。



![](img/48dc51cc564f14dee265d737e3ae4d64_18.png)

This is， again， referencing this model above， not the model that we have initiated down here。



![](img/48dc51cc564f14dee265d737e3ae4d64_20.png)

But you see that this outputs four different values where that maximum value should be the next step。

 because that's predicting the most amount of reward possible。



![](img/48dc51cc564f14dee265d737e3ae4d64_22.png)

And that's why we callmp。org max on the output of what we add here。Pta Arg Max。And that will tell us。

Which action would lead to the highest possible reward。



![](img/48dc51cc564f14dee265d737e3ae4d64_24.png)

And then we take that new action， so now our new action is going to be decided by that Arcm。



![](img/48dc51cc564f14dee265d737e3ae4d64_26.png)

And then as we go along， we keep adding on the reward。

 we app on the actual observation that we were at， the action we took。

 the reward and the episode that we're at。

![](img/48dc51cc564f14dee265d737e3ae4d64_28.png)

![](img/48dc51cc564f14dee265d737e3ae4d64_29.png)

![](img/48dc51cc564f14dee265d737e3ae4d64_30.png)

And we reay the old observation and run through this loop until we hit that done。Equal to true。

 right， this over here being equal to true。

![](img/48dc51cc564f14dee265d737e3ae4d64_32.png)

We're then going to incorporate that the total reward into our episode memory。

 so once this is done running。We're going to say， if we hit any reward。

 then we're setting that equal to 1 or 0， generally speaking， as we discussed。

 if we landed in a hole， we'd end up with a total reward of 0。

 If we were able to get to that end point， then we'd end up with a total reward of one。



![](img/48dc51cc564f14dee265d737e3ae4d64_34.png)

And then we add that on to our life memory。

![](img/48dc51cc564f14dee265d737e3ae4d64_36.png)

And then we have our new data frame with this new life memory。

 and then we can look at the mean value of our episode。

 given the fact that we're now taking more educated steps along the way。



![](img/48dc51cc564f14dee265d737e3ae4d64_38.png)

So I'm going to run this。And as we'll take just a second to run。

 we're going through 500 different iterations now we have a model that has to fit first and also predict along every single step along the way。

So we're going to pause the video as it's taking just a second to run。



![](img/48dc51cc564f14dee265d737e3ae4d64_40.png)

And then here we see the results that we ended up getting。And that may have taken just a bit of time。

 We see that it got here up to 62。4。 So much better in terms of how often it was able to get to that end goal。

 I'm not promising theyd be this high。 That's going to be somewhat random。

 There is a bit of a sarcastic process there。 But still。

 we see how much we are able to improve once we implemented that reward system。

Now that closes out our video here。In the last video in regards to reinforcement learning。

 we're going to introduce one last environment。And see how we can work through that environment as well。

 And hopefully， as you start to go through reinforcement learning on your own and play around in the open AI gym。

 you'll be ready to hit the ground running。 All right， I'll see you there。😊。



![](img/48dc51cc564f14dee265d737e3ae4d64_42.png)

![](img/48dc51cc564f14dee265d737e3ae4d64_43.png)