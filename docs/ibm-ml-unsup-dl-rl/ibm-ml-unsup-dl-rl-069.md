# 069：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p69 30_深度学习的正则化技术.zh_en -BV1eu4m1F7oz_p69-

Now， we've discussed the importance of regularization and the trade off between bias and variance when working with different algorithms in prior courses。

In this current set of videos， we'll discuss different regularization techniques for deep learning specifically and why it's of utmost importance when creating these more complex models。

 So in this section， we're going to cover regularization techniques for deep learning and why it's so important for deep learning specifically。

😊。

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_1.png)

We'll touch on dropout as well as early stopping， which are different regularization techniques specific to neural nets。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_3.png)

And with that， we'll also discuss different approaches to optimization when working through our neural nets and why those are important。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_5.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_6.png)

So to start off。Technically， a deep neural net is a neural net that has two or more hidden layers and often many more。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_8.png)

So a neural network becomes a deep neural network once we move beyond two layers。

 the importance of regularization when it comes to deep neural nets。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_10.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_11.png)

Is that with more and more layers， we can learn more and more complex models。

 and those complex models can often nearly perfectly fit to our training data。

 but due to its complexity will often overfit to that training data and not generalize well to that new data。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_13.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_14.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_15.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_16.png)

Now， with that in mind， let's quickly remind ourselves what regularization is。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_18.png)

Regularization is any modification we make to a learning algorithm that's intended to reduce its generalization error。

 but not necessarily its training error。

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_20.png)

So again， we're not optimizing to training error。But rather tweaking our algorithm to optimize her generalization and usually at the price of additional training air。

So for neural nets， there are several means of regularization available to us。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_22.png)

Adding some regularization penalty in our actual cost function is an option。

 and this would be similar to what we saw with Lassso or Rige， for example。

 where more and higher weights will be penalized within our actual cost function。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_24.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_25.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_26.png)

We have something called drop out， where we'll randomly lose certain neurons in our network to ensure no model is over reliant on any particular neuron or any particular path。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_28.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_29.png)

Talk about early stopping， or will just be the idea of stopping gradient descent short so that's not perfectly fit to the training set。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_31.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_32.png)

And then with that， to some degree， those ideas of storcchastic and mini batch gradient descent that we discuss in prior videos may ensure that we don't perfectly fit to our training set。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_34.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_35.png)

And therefore may generalize a bit better than full batch grading descent。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_37.png)

Now， starting with that first option that we had listed there。

 we can choose a loss function that will penalize our model for having higher weights。 And again。

 this will be similar to Rir Russian if we're trying to predict a numerical value in that we can take something like our mean squared air。

😊。

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_39.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_40.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_41.png)

And add on a regularization term that penalizes the weight squared。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_43.png)

And if we are trying to predict a categorical variable。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_45.png)

Rather than a numericalmer value variable， this will be very similar in that we will be simply adjusting our categorical loss function that we have to penalize again those larger weights。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_47.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_48.png)

Now let's move to a method that will be more specific to working with neural networks。

 namely dropout。

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_50.png)

Now， with dropout， we'll be randomly removing a subset of the neurons for each batch。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_52.png)

So a common problem that neural nets faced in earlier implementations was that although there are all these different pathways available when optimizing these complex neural nets。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_54.png)

Often they would become over reliant on particular pathways。

While not much learning was done on other pathways through other nodes。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_56.png)

So creating a deeper or denser network， adding on more layers or adding on more nodes would tend to not add that much value to our neural net。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_58.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_59.png)

By adding in dropout。Our neural networks will not be able to over relyly on any individual pathway。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_61.png)

And thus， it'll become more robust to overfitting to that particular pathway。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_63.png)

And finally， since we're randomly dropping out neurons and learning on just a subset。

 we then will have to rescale those weights of the neurons at the end to reflect the percentage of the time that that neuron was active during training versus not active。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_65.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_66.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_67.png)

So we can think of this image that we have here as our standard fully connected feed forward neural network that we are familiar with at this point。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_69.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_70.png)

With drop out at each iteration， we end up with something like we see here to the right。

 where we drop a predetermined percentage of the nodes at each one of our layers so that our model is forced to learn iterations with different possible pathways through to our solution。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_72.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_73.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_74.png)

So what exactly do we mean also when we say that we are rescaling our neurons after training our model？

If you think about it when we are training our model。

 a node is going to have a probability P as we see here to the left of not being present。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_76.png)

At any particular iteration， and thus， the remaining weights are going to be scaled up to make up for that。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_78.png)

Thus when we actually get to testing time and all the weights are present。

We need to ensure that we appropriately rescale our weights。

 and that's why we have P times W at the end。

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_80.png)

When all these weights are always present。

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_82.png)

Now， another heuristic that was available to us that we discussed earlier is idea of early stopping。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_84.png)

And this just refers to choosing some rules at which to stop training our model to ensure that we don't overfit。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_86.png)

Now an example of this would be to start by checking the loss on a validation set。

 so not the training set， but rather some holdout set that validation set at every 10 epochs。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_88.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_89.png)

And if the validation loss at that next step，10 epochs later is higher than it was at the previous step。

 then we would stop training。

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_91.png)

So that's the idea of early stopping。

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_93.png)

Now that closes out this section on common regularization techniques that are available to us。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_95.png)

In the next videos， we'll discuss another important part of tuning your neural net。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_97.png)

Namely what type of optimizer you're going to use All right， I'll see you there。



![](img/88ca3d3a46cbc424b87612fdc6c73ad9_99.png)

![](img/88ca3d3a46cbc424b87612fdc6c73ad9_100.png)