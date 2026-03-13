# 053：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p53 14_梯度下降笔记本（选修部分）第2部分.zh_en -BV1eu4m1F7oz_p53-

Welcome back for the second part of this demo on gradient Descent。As we mentioned in the last video。

 when we are working with other problems such as working with neural networks。

 we are not going to have this analytical solution that we just found。So instead。

 we're going to have to move towards that optimal value using gradient descent。

So in order to see this in practice。

![](img/ee5bd56e52e50fa24173b055cddeefb7_1.png)

We're going to actually pick a learning rate， as well as a number of iterations。

Run the code and plot the trajectory as we move towards that grading descent。

 If you recall in lecture， that means that we're going to pick how big each one of our step sizes are going to be and then see step by step as we move closer to the optimal value。

 how we're actually moving towards each one of the different thetas that we're trying to predict。

And then using that we'll find some examples where the learning rate is too high， too low。

 or just right。

![](img/ee5bd56e52e50fa24173b055cddeefb7_3.png)

So we're going to start off。With a learning rate of 1 times 10 to the negative 3， so 0。001。

And we're going to say we want 10000 iterations， so 10000 different steps。

 and we're going to initialize with the value of 3，3，3。



![](img/ee5bd56e52e50fa24173b055cddeefb7_5.png)

So in order to actually perform gradient descent。We're going to pass in a learning rate。

 which we defined above the number of iterations and the theta initial， all defined up here above。

So the initialization steps will be we'll set our theta originally equal to that theta initial。

 which is at this point 333。We're then going to set the theta path at first equal to just a bunch of zeros in the shape of the number of iterations plus one。

So if we doing 10000 iterations， there'll be 10000 in1 rows， and each one will be three columns。

 So it'll say for each one of our different values， which are B B， Theta1 and theta 2。

 how we're moving closer and closer to each one of those steps through each one of the different iterations。

And then we're setting that first value， so theta 0 for all the columns equal to that theta initial。



![](img/ee5bd56e52e50fa24173b055cddeefb7_7.png)

And then just to start off， we're going to set the loss vector equal to NP do zeros。

 and we'll see the loss as we move through each one of the steps to see if we continue to minimize that loss and we'll do that for every single iteration。



![](img/ee5bd56e52e50fa24173b055cddeefb7_9.png)

We're then going to do this main gradient descent loop。

 which is going to be what we discuss in lecture in regards to starting at the initial point。

 finding the gradients， and then using that gradient to move closer and closer towards our optimal value。

So we're going to set our prediction equal to the dot product of our different thetas times our x matrix。

 so if we think about that as taking our entire x matrix and if we take the dot product of the transpose of theta and the transpose of the X matrix。

 then all we're doing is multiplying in this case， initializing with 33，3。

 three times the first value for all them three times a second and three times a third。

 adding those all together and getting our first prediction for each one of our different y values。

Our loss vector， which we defined up here， we will then say that first loss will be equal to the sum of the square of what we predicted minus the actual values minus what we predicted。

So we'll get the mean squared to error。Or just the squared error。And then our gradient vector。

 which we didn't go through in lecture。 But just to know what the gradient actually looks like as we're taking those partial derivatives。

 what will look like is going to be。That。Prediction， that error on the prediction。 So y minus y pre。

 take that in the dot product of X mat。 So that's actually going to be equal to that gradient vector。

 That's how we can come up with that gradient vector。

 And it will be of the size that we need in regards to。Subtracting， or in this case。

 this will actually be the negative of it。 So we're actually later on going to add it on。

 So we see here that we add on that gradient vector。

But that's actually going to be equal to the negative of our gradient vector。

We divide that by the number of observations that we have and we'll use that in order to move a step closer towards our actual theta。

 So at first our theta is 333。 We got that gradient vector and we set 333 plus that learning rate multiplied by that gradient vector。

 which should also be of the same shape as that 333， So our row vector with three values。

And then we say that theta I plus 1 is equal to that theta that we just found， so we reset the theta。

 and then we go back through the for loop using that new theta and coming up with our new prediction。

 our new error， our new gradients， and then our new theta values。

And then we're going to return after we go through that entire for loop， the entire theta path。

 as we go through each one of the different iterations， as well as the full loss vector。

 How far off are we from our actual solution， as we move down the line。

 And if we recall that loss vector is just going to be the sum squared error。



![](img/ee5bd56e52e50fa24173b055cddeefb7_11.png)

So we have our gradient descent function。And then we're going to actually plot this out。

 I'm going to quickly walk through this， I'm not going to go through every single line of code。

 but I do want you to get some intuition as to what we're plotting here。



![](img/ee5bd56e52e50fa24173b055cddeefb7_13.png)

So we have our true coefficients。Which are just equal to the B。

 the theta 1 and theta 2 that we defined earlier。And then we say plot I J。

 And what we're doing here is we're plotting。Two of these different values。

 so either B versus theta 1 or b versus theta 2 or theta 1 versus theta 2。

 those are going to be each of our three plots。So plot Ij。

 we're going to plot the actual true coefficients， so the true coefficient of。

 if we say I and J are equal to B and theta 1。Then we'll say we want the0 with value。

 which will be B and the first value， so J would be theta 1。

So we're just going to plot the actual values and then mark those as the true coefficient。

 We're then going to plot the theta path。And we're going to plot that path。 Again。

 we're only using two of the dimensions at a time。 And let's say， again。

 we're working with B and theta 1。Then we plot the path of if we sets I equal to0。

 then we want for the theta path only that first column， which will be the different values for B。

And then for that second column， the different values for theta 1。 if we set J here equal to  one。

And then， we can。

![](img/ee5bd56e52e50fa24173b055cddeefb7_15.png)

Say what the initial value is by calling theta path0 I and J and label that as the start。

And also to note here， we are taking each one of the steps that we take from the start to the end and using the dashed lines。

 as well as a marker for each step of the triangle。 And then finally。

 we're going to say negative one and I and negative one and J for thetapaths。

In order to get the final value and we'll label that to finish。



![](img/ee5bd56e52e50fa24173b055cddeefb7_17.png)

![](img/ee5bd56e52e50fa24173b055cddeefb7_18.png)

And then that's just going to be a subset when we call Cla O。



![](img/ee5bd56e52e50fa24173b055cddeefb7_20.png)

What we're doing is we're just taking each one of the different combinations of axes that we can。

 so we'll have b versus theta 1， B versus theta 2 and theta 1 versus theta 2。



![](img/ee5bd56e52e50fa24173b055cddeefb7_22.png)

And as we see here， we're calling plot I J 0，1。And then plot I J 0，2， and then plot I J 1，2。

 on each one of our different axes。 And then on top of that。

 we're also going to plot our loss vector to see how we reduce the actual loss function as we iterate and move closer and closer to the true values。

With that， we get our gradient descent to output both our theta path and our loss vector。

We can then pass that into our plot all function that we defined with our learning rate。

 our number of iterations and our theta。

![](img/ee5bd56e52e50fa24173b055cddeefb7_24.png)

We run this。

![](img/ee5bd56e52e50fa24173b055cddeefb7_26.png)

First， we have to obviously。Run what we have here， so we run that。

 and this will plot out the actual steps that we take。



![](img/ee5bd56e52e50fa24173b055cddeefb7_28.png)

![](img/ee5bd56e52e50fa24173b055cddeefb7_29.png)

Here we see the start and we see each one of these triangles and are moving closer and closer this axis here on the we look at the top left plot。

 we see the x axis is theta 0， the y axis is theta 1， and we want theta 0 to be moving towards 1。5。

 that's our b， and we want the theta 1 to be moving towards 2。

And then if we look at the actual values， it looks like theta zero stopped at around 1。9。

Theta1 also started stop at around 1。9。 and then if we look at the top right graph。

 we see theta 0 is still the x axis so also still at 1。9， but now the y axis is theta 2。

 and that's actually pretty close to 5 already。

![](img/ee5bd56e52e50fa24173b055cddeefb7_31.png)

And we can see each step it takes along the way。 And then here we can see theta 1 versus theta 2。

 which started as we suppose all these are starting at 3，3。

And then it should have moved towards that two and five that we'd like。And in the bottom right graph。

We see the number of iterations and we see pretty quickly once it gets to， it looks like maybe 100。

200 iterations drops off to getting a very low error， and that it's slowly。

 very gradually continues to minimize that error， but not quite as much。

 which is probably why that learning rate， Why we haven't gotten all the way to our optimal values。

 and we have the gap between some of our end points are actual true values and the endpoints that we got using gradient descent。



![](img/ee5bd56e52e50fa24173b055cddeefb7_33.png)

Now I quickly want to show you what this looks like。If we were to decrease the number of iterations。



![](img/ee5bd56e52e50fa24173b055cddeefb7_35.png)

So if we decrease this to， say， 100。

![](img/ee5bd56e52e50fa24173b055cddeefb7_37.png)

And we run all this。We see that it stops a lot earlier along the way。

 The gradient descent didn't get to quite finish。

![](img/ee5bd56e52e50fa24173b055cddeefb7_39.png)

![](img/ee5bd56e52e50fa24173b055cddeefb7_40.png)

Now if we keep this at 10，000。And we decrease the learning rate， which allows for larger steps。



![](img/ee5bd56e52e50fa24173b055cddeefb7_42.png)

We can see here that it actually reached the optimal solution。



![](img/ee5bd56e52e50fa24173b055cddeefb7_44.png)

So。Decreasing the learning rate allowed for those larger steps。

 and we were actually able to get to those optimal solutions。



![](img/ee5bd56e52e50fa24173b055cddeefb7_46.png)

Finally， I want to show you what will happen if you set that learning rate too large。



![](img/ee5bd56e52e50fa24173b055cddeefb7_48.png)

Now if we run this。Maybe a little bit difficult to see if you see the top right corner of this top left or top left corner of the top left plot。

 we see that we're talking about massive massive numbers。 So it's 0。2 times 10 to the 305。



![](img/ee5bd56e52e50fa24173b055cddeefb7_50.png)

![](img/ee5bd56e52e50fa24173b055cddeefb7_51.png)

![](img/ee5bd56e52e50fa24173b055cddeefb7_52.png)

We have missed our value by a long shot。 We totally overbl the optimal value。

 And if we look at the actual error rate， we see as we increase the number of iterations。



![](img/ee5bd56e52e50fa24173b055cddeefb7_54.png)

The error shoots way， way， way up as we completely miss the optimal value。

So that closes out our video here working with vanilla gradient descent in the next video we will go through the same steps and briefly walk through how you can do it using stochastic gradient descent。



![](img/ee5bd56e52e50fa24173b055cddeefb7_56.png)

![](img/ee5bd56e52e50fa24173b055cddeefb7_57.png)