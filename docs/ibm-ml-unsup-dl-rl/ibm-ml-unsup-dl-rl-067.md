# 067：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p67 28_keras笔记本（选修部分）第3部分.zh_en -BV1eu4m1F7oz_p67-

Let's close out this video with exercise2 over here。

 so we're going to build another model and this time we're going to have two hidden layers each with six nodes。



![](img/f5b529235ab6fc50e9a13f842d7f1941_1.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_2.png)

And we're going to use the relu activation function for each one of those different hidden layers。

 which is generally going to be best practice and then sigmoid for that final layer as we're trying to output values between 0 and1。

 and if you recall for relu， you're not going to necessarily get values between0 and1。



![](img/f5b529235ab6fc50e9a13f842d7f1941_4.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_5.png)

We're then going to use a learning rate of 0。003 and we're going to train for 1500 epos。

 so that's going to take some time as we saw as we did a000 epochs。

 so I will do that pause and continue as we go through this。



![](img/f5b529235ab6fc50e9a13f842d7f1941_7.png)

And then as we did before， we're going to graph the trajectory of the loss functions as well as the accuracy on both the train and test set。

 and then we'll plot that ROC curve for these different predictions。



![](img/f5b529235ab6fc50e9a13f842d7f1941_9.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_10.png)

So we're going to initialize our model。

![](img/f5b529235ab6fc50e9a13f842d7f1941_12.png)

As a sequential model， so we just call sequential。And then we're just going to add on each one of our different layers。

 So we have our first hidden layer， and that's going to be a dense layer with six nodes。

 so fully connected。We pass in for that first hidden layer， the actual input shape。

 and that's going to be eight。And for the activation this time， rather than saying sigmoid。

 all we have to do is change that string to relo。So you that for the first layer。

 then we can do that again for the second hidden layer。

 so we just add on again model do add dense and this time we don't need the input shape and we set the activation equal to relo once again。



![](img/f5b529235ab6fc50e9a13f842d7f1941_14.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_15.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_16.png)

And then to get to that final output， we recall that we only want one node as we're just trying to predict one or0 for each one of the different values。



![](img/f5b529235ab6fc50e9a13f842d7f1941_18.png)

And we therefore have a dense layer fully connected to just one node。

And we want that activation at that final layer to be equal to sigmoid。

As we want some value again between0 and 1。We're then going to compile our model and that's the next step。

 and there we're going to pass in our optimizer as well as our loss function。

 as well as the different metrics we want to track。



![](img/f5b529235ab6fc50e9a13f842d7f1941_20.png)

So of saying we want stochastic gradient descent， feel free on your own to try atom as well as RMS prop which we imported earlier。

 as well as playing around with different learning rates perhaps。



![](img/f5b529235ab6fc50e9a13f842d7f1941_22.png)

Then we're going to still use binary cross entropy as we're still trying to figure out a binary zero or one value。

And then we're going to also track accuracy as one of the metrics。

We're going to save our history as run his 2， and we get that output from calling Model 2 dot fit on our X train and our Y train。

 and then again we can specify here what we want our holdout set to be so that we can track that throughout。



![](img/f5b529235ab6fc50e9a13f842d7f1941_24.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_25.png)

And with that， we also， when we fit our model need to specify how many times we want to run through our data set。

 so how many epochs we want， and we set in the exercise prompt that we want 1。

500 run throughs of the entire data set。

![](img/f5b529235ab6fc50e9a13f842d7f1941_27.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_28.png)

So I'm going to run this。

![](img/f5b529235ab6fc50e9a13f842d7f1941_30.png)

And you see， we start to get each one of the different epochs。

 and we see that the accuracy increases， the loss decreases。

 and at least in the beginning the validation loss should continue to decrease。



![](img/f5b529235ab6fc50e9a13f842d7f1941_32.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_33.png)

So I'm going to pause here and we'll come back once it's run through those 1500 different epos。



![](img/f5b529235ab6fc50e9a13f842d7f1941_35.png)

So now we have run through our 1，500 epochs， as we see here， and just a quick reminder。

 if we call run his2， which we defined when we set it equal to the fit output。



![](img/f5b529235ab6fc50e9a13f842d7f1941_37.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_38.png)

We have our dictionary of keys with our different keys， which is the loss， the accuracy。

 the validation loss， and the validation accuracy。Now last time we didn't do this but here we're actually going to plot the accuracy as well。

 so we're going to create subplots and in that first subplot so we call PLT dot figure and then on that figure we add on the subplot and we're going to say that it's a one by two subplot and we want to look at the first one。



![](img/f5b529235ab6fc50e9a13f842d7f1941_40.png)

And on that axe。On that bounding box， we're going to plot the loss。



![](img/f5b529235ab6fc50e9a13f842d7f1941_42.png)

As well as the validation loss in red and blue， respectively。And then in the next subplot。

 so we call add subplot and we say we want that second subplot。



![](img/f5b529235ab6fc50e9a13f842d7f1941_44.png)

We're going to call the accuracy， as well as that validation accuracy in red and blue as well。

 And we're going to include a legend on each。 So you won't have to remember which one was which。



![](img/f5b529235ab6fc50e9a13f842d7f1941_46.png)

So you run this and here we see。

![](img/f5b529235ab6fc50e9a13f842d7f1941_48.png)

That's on that training set obviously continued to go down。

 but on that validation set going through 1500 epochs and those two layers。

 we definitely overfit our data set as we see that the loss。

 the validation loss actually starts to increase around that 800 epochs point。

 we kind of see a bit of an inflection。

![](img/f5b529235ab6fc50e9a13f842d7f1941_50.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_51.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_52.png)

AndThen we can see that accuracy jumping up and down as it tests throughout。

 and we see that that fairly increase throughout as we fit our model closer and closer。

 and then again， we see a bit of a decrease around。



![](img/f5b529235ab6fc50e9a13f842d7f1941_54.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_55.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_56.png)

800 to 1000 epochs。 And that's not going to correlate perfectly。

 This is just according to our loss function。 and then our overall accuracy when we check it。

 And we see that kind of jumps up and down。 But really plateaus definitely pass out 1000 epoch mark。



![](img/f5b529235ab6fc50e9a13f842d7f1941_58.png)

Then we can use what we did before in order to predict both the classes as well as the probabilities。

 check for our accuracy as well as our ROC AU score once we get those outputs using the predicted class as well as the predicted probabilities。

 then we can also plot our ROC using that function that we defined earlier。

So you run this and we see a bit higher of accuracy， a bit higher of ROC AU。

 and then we see the curve as well。 Now again， there's a bit of randomization so we won't necessarily get the exact same answer and maybe there's not that much improvement and especially with the amount of time it took to train this model there probably wasn't enough improvement from our random forest so keep that in mind that sometimes it's not always going to be the best solution there is this talk within the data science community where you just throw neural nets at everything and how that's not best practice。

 I want to ensure as you watch this video that you keep that in mind as well but they will be very powerful throughout and that's why we're learning it here。



![](img/f5b529235ab6fc50e9a13f842d7f1941_60.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_61.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_62.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_63.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_64.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_65.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_66.png)

That closes out our video here with the introduction to Carras。

 feel free to play around with this model that we have here。 You can add on more layers。

 increase the amount of nodes， decrease the amount of nodes。 change the activation functions。

 change your optimizers。 You can play around with each of these and see how the model runs。

 I would say don't do each one for 1500 epochs。 You're probably overfitting， and it'll take too long。

 but it is worth playing around getting familiar with what you can play around with within this Cars framework。

 All right， I'll see you back at lecture。😊。

![](img/f5b529235ab6fc50e9a13f842d7f1941_68.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_69.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_70.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_71.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_72.png)

![](img/f5b529235ab6fc50e9a13f842d7f1941_73.png)