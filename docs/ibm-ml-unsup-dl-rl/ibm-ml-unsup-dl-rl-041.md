# 041：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p41 2_神经元基础.zh_en -BV1eu4m1F7oz_p41-

Now let's zoom in on a single node in the middle of our basic neural network。



![](img/4594984d509fbbdd6c97d4d74510a6fb_1.png)

First， that node will get input values from the previous layer wherever that node lies。



![](img/4594984d509fbbdd6c97d4d74510a6fb_3.png)

Those input values are then going to be combined via weights from each of those different values similar to basic multiple linear regression。



![](img/4594984d509fbbdd6c97d4d74510a6fb_5.png)

Then that combination of weights is going to be transformed。

 similar to how logistic regression transforms a linear combination to squash those values between 0 and 1。



![](img/4594984d509fbbdd6c97d4d74510a6fb_7.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_8.png)

And that transformed value is used as input for the next layer。



![](img/4594984d509fbbdd6c97d4d74510a6fb_10.png)

Now let's add in some variables to paint this process a bit clear。



![](img/4594984d509fbbdd6c97d4d74510a6fb_12.png)

So we can have as our three input values， x1， x2， and x3。

And we'll assume also an intercept term with that value equal to one as we do with multiple regression。

We also have the respective weights for each one of our different values， W1， W2 and W3。

 as well as B， And our model is going to learn each one of these weights as well as the B。



![](img/4594984d509fbbdd6c97d4d74510a6fb_14.png)

And as mentioned， we will multiply each value by its weight。

 as we do with linear regression and end up with some output value Z。



![](img/4594984d509fbbdd6c97d4d74510a6fb_16.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_17.png)

Finally， we're going to use an activation function， like I said。

 similar to logistic regression or even logistic regression。

 to transform that output and use that value as input for the next layer。



![](img/4594984d509fbbdd6c97d4d74510a6fb_19.png)

Now， without this activation function， we are restricted to only linear output or linear combinations of our inputs。

And no matter how many layers deep we go， we are still just working with a linear combination of our features。

 It's going to be this activation function that allows for the great flexibility with respect to how we consider the model outputs。

 given our model inputs using a neural network。😊。

![](img/4594984d509fbbdd6c97d4d74510a6fb_21.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_22.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_23.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_24.png)

Now， some notation that'll be worth getting familiar with as we walk through working with neural networks。



![](img/4594984d509fbbdd6c97d4d74510a6fb_26.png)

We have Z， which is going to be the net input or the linear combination of the inputs prior to activation。

 so essentially the output of just that linear regression。



![](img/4594984d509fbbdd6c97d4d74510a6fb_28.png)

We'll have our bias term or that B that we just saw。

 which is also similar to our bias term within linear regression。



![](img/4594984d509fbbdd6c97d4d74510a6fb_30.png)

We'll have F our activation function， that nonlinear function we use to transform the output of z。

And then we have a， our output layer， or the value once we take F of Z。

 once we transform Z that we ultimately pass through to our next layer。



![](img/4594984d509fbbdd6c97d4d74510a6fb_32.png)

Now， with this syntax in mind， as well as that basic unit that we just walked through that basic neuron。

We'd seen that there is a lot of relation between that neuron。And logistic regression。

 So when we choose F of Z equals 1 over1 plus E to the negative Z。

 where Z is our output of just the linear part of that neuron。



![](img/4594984d509fbbdd6c97d4d74510a6fb_34.png)

We are actually looking at something very similar to logistic regression。



![](img/4594984d509fbbdd6c97d4d74510a6fb_36.png)

And what we have here was Z。Z is just going to be equal to that intercept term plus the sum of each one of the different inputs multiplied by their respective weights。

 which we've expanded out here。

![](img/4594984d509fbbdd6c97d4d74510a6fb_38.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_39.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_40.png)

And our neuron is then simply just a unit of logistic regression。

 where we have the different weights that we learn are just the coefficients for logistic regression。

 The inputs are the different variables that we have here。 and the bias term is that constant term。

 So it all relates back to our basic logistic regression。



![](img/4594984d509fbbdd6c97d4d74510a6fb_42.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_43.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_44.png)

And because logistic regression and our neural network in a way can accomplish the same task if we're trying to accomplish classification。



![](img/4594984d509fbbdd6c97d4d74510a6fb_46.png)

We want to ensure that when we move to neural network that we actually need a more complex model that we don't just need this single unit。

 but we need multiple units and perhaps multiple layers。

 and that's when we do switch over to neural networks。



![](img/4594984d509fbbdd6c97d4d74510a6fb_48.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_49.png)

The trade off being that you may be able to come up with a more complex boundary with neural networks。



![](img/4594984d509fbbdd6c97d4d74510a6fb_51.png)

But you'll lose a lot of the explanatory value that you have with logistic regression。



![](img/4594984d509fbbdd6c97d4d74510a6fb_53.png)

So what we have here is going to be the sigmoid function， which we use for a logistic regression。

 as well as our activation here when we talked about the neuron and the output for the neural network。



![](img/4594984d509fbbdd6c97d4d74510a6fb_55.png)

And what our sigmoid function will do will take that linear combination and create a linear function。

 as we see here， we have linearity， not a straight line here。And squash those values between0 and1。

 which will be useful as we walk through the different steps of our neural network。



![](img/4594984d509fbbdd6c97d4d74510a6fb_57.png)

![](img/4594984d509fbbdd6c97d4d74510a6fb_58.png)