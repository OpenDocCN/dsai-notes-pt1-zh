# 054：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p54 15_梯度下降笔记本（选修部分）第3部分.zh_en -BV1eu4m1F7oz_p54-

Welcome back to our notebook here on gradient Descent in this video we're going to close out by discussing stochastic gradient descent。

 so rather than averaging the gradients across the entire data set before taking any steps。

 we're now going to take a step for every single data point as we discussed in lecture。Now。

 our exercise here is going to be to run the stochastic grading descent that we have this function。

 and then also modify the code so that we can randomly reorder which one of the different data points it picks at each iteration and I'll walk through that in just a second。



![](img/2547dd3c2a664ad13ff385fcaac2b334_1.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_2.png)

So first for dochastic gradient descents， much of the function will look similar to our gradient descent function。

 will set theta equal to this initial theta。

![](img/2547dd3c2a664ad13ff385fcaac2b334_4.png)

We will have our zeros which are going to be the same size as not just here the number of iterations。

 but also the number of observations， So here the idea is that the number of iterations will be how many times are we going to go through the full data set？

But we're updating at every single data point。So by the time we run through the full data set and our data has 100 values。

 we all have made 100 updates， so we have 100 iterations。

If our number of beerations is equal to 100 and our number of observations equal to 100。

 then we'll be making 10，000 updates， but only running through the entire data set 100 times。



![](img/2547dd3c2a664ad13ff385fcaac2b334_6.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_7.png)

And this will become more and more familiar as when we work with our deep learning models later on in this course。

 this will be something called EpochX， and the epochs will be the number of times you run through the full data set。

 and then you will also define the batchge size as we talked about mini batch gradient descent。

 where we will actually find that right balance of the batch size as well as how many times we want to run through the entire dataset set。



![](img/2547dd3c2a664ad13ff385fcaac2b334_9.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_10.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_11.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_12.png)

So we're going to have zeros for the theta path， that's going to be the size of the number of iterations。

Times the number of observations within our pass through matrix。

We're then going to set theta initial equal to the first value and our loss vector again is equal to the zeros for the number of iterations times the number of observations。



![](img/2547dd3c2a664ad13ff385fcaac2b334_14.png)

Then for the main stochastic gradient descent loop。



![](img/2547dd3c2a664ad13ff385fcaac2b334_16.png)

We're saying4 I in range number of iterations。 But now we're also saying4 J in range the number of our observations。

 So 4 I， and then 4 J， and that will allow for。Every single value within every single for loop。

 So we're do 100 iterations and for each each iteration。

 we're going to go through each one of the different observations。

 and that's going to be your value for J。And then we're seeing to get that gradient vector。

 So up until the gradient vector， things are the same for that gradient vector。

We're going to say the value for J。Versus the value of j that was predicted for that specific row。

 And then we're going to take the x matrix， but only the J row in order to get that dot product to get our gradient vector just for that single value。

And then with that new gradient vector， we'll use that to update our theta values at each one of the different steps to get each one of our new thetas。



![](img/2547dd3c2a664ad13ff385fcaac2b334_18.png)

And then we update the theta path， according to the counts。

 and our count is being added by one Each time we run through either the A and the J loop。

 That's why we have this count variable to keep adding on， rather than just。



![](img/2547dd3c2a664ad13ff385fcaac2b334_20.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_21.png)

If we were looking at the number of observations zero to 100， or just the number of iterations。

 which is also0 to 100， the count will go from 0 through that 10。

000 as it' will have to go through both of these four loops。



![](img/2547dd3c2a664ad13ff385fcaac2b334_23.png)

And then we'll just return， as we did before， the theta path， as well as the loss factor。

So here we set the learning rate to1 e to the negative4。

 the number of iterations here again is just 100， but that means we're making 10，000 steps。

 but we're only running through the full data set 100 times。

And are they initials going to be this 333 again。So we can call stochastic gradient descent。

 get our path as well as our loss vector， and use that same plot all function that we defined above。

 and plot out that theta path， the loss vector， and then the appropriate labels for the learning rates。

 the number of iterations， and the initial theta。

![](img/2547dd3c2a664ad13ff385fcaac2b334_25.png)

So he'd run this。

![](img/2547dd3c2a664ad13ff385fcaac2b334_27.png)

And we can see the path that was taken and if we look at this bottom left graph。

 that'll be your first clear observation of the amount that it swerves back and forth rather than creating a straight path towards where we're trying to aim as we did with the normal gradienting descent。

 so it's a bit more random。

![](img/2547dd3c2a664ad13ff385fcaac2b334_29.png)

Now， something to note is if you are doing something like stochastic gradient descent or mini batch gradient descents。



![](img/2547dd3c2a664ad13ff385fcaac2b334_31.png)

Each one of our updates as we go through each one of the values。

 if we do that for loop with that set ordering， the update for iteration 20 will be dependent on the update from iteration 19 and the update from。

Iteration 18， so on and so forth， so be biased according to the ordering of our actual data frame。

So rather than do that， we're going to make this a bit more random and at each time set R J equal to a random integer。



![](img/2547dd3c2a664ad13ff385fcaac2b334_33.png)

Need a NP。The dot random dot random ins。Some value the size of the number of observations。

 so we can actually say zero through nu observations。



![](img/2547dd3c2a664ad13ff385fcaac2b334_35.png)

And we run this and now rather than that ordering， we have a bit more randomness。

 and we can see it's a bit more squiggly， but that ensures that it doesn't have like we saw before a clear pattern going back and forth and it's not dependent on the ordering of the data frame。



![](img/2547dd3c2a664ad13ff385fcaac2b334_37.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_38.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_39.png)

Now we can play around with this。As we see here， we can increase the number of iterations and I'll run this because this will take a second and now again we're making 10。

000 times 100 updates along the way， so using srcchastic gradient descent hopefully we will be able to get to that solution。

 but in general， using srcchastic gradient descent as mentioned during lecture will allow you to speed things up but at the same time it may not exactly get to the right solution as it will be bouncing around in order to get to that ultimate solution。



![](img/2547dd3c2a664ad13ff385fcaac2b334_41.png)

That may have taken just a second to run， but as we see here。

 once we increase the number of iterations， that may have been even too many iterations we didn't have to go as long as we did We see that we end up。



![](img/2547dd3c2a664ad13ff385fcaac2b334_43.png)

At that final point， at the finish that we'd hoped for with thee。

Theta 2 equal to5 if we look at the bottom left and theta 1 equal to2。



![](img/2547dd3c2a664ad13ff385fcaac2b334_45.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_46.png)

And we see that we have this sharp decrease right at the beginning at the number of iterations and very very slight amount of improvement along the way。

 But we do know where we are aiming towards。 Now， something to note here， as I said。

 we may not have needed to go as many iterations， as we get towards the bottom of that slope。

 If we imagine that concave curve that we discussed during lecture。😊。



![](img/2547dd3c2a664ad13ff385fcaac2b334_48.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_49.png)

The gradient is going to get smaller and smaller as it approaches that optimum value。

 so the updates will be smaller and smaller as we get closer and closer to that optimum value。

 So that's going to ensure that if we have a small enough learning rate that we don't overshoot it。

 It just stays within that right direction and keeps within that optimal minimal point within that concave curve。

Now， that closes out our section here on gradient descent。 And with that。

 we will get back to lecture and discuss further working with our neural networks。 Allright。

 I'll see you there。

![](img/2547dd3c2a664ad13ff385fcaac2b334_51.png)

![](img/2547dd3c2a664ad13ff385fcaac2b334_52.png)