# 085：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p85 46_迁移学习笔记本（选修部分）.zh_en -BV1eu4m1F7oz_p85-

Welcome to our demo notebook here on transfer learning。 in this exercise。

 we're going to be using the well known Mnes digit data set。😊。

Which is just going to be a bunch of handwritten digits between 0 and9。Along with their labels。

 so if there's a five written down， then that's labeled as a  five。

0 written down that's labeled as a0。And we're going to use this data set to illustrate the power and the concepts behind transfer learning。

 So we're going to train a convolutional neural net on just the digits between 5 and 9。

And after that， we're going to train just the last layer of the network on the digits0 through4 and see how well the features learned from5 through 9。

 those earlier features before that final layer are going to be able to help classifying0 through4。

So we're going to import the necessary libraries， you should be familiar with all these from before。

 the only ones that are new is we're importing the MminIS data， which is available in Tensorflow。

cars。 datasets。

![](img/ab5245e489ee604bd516a84803cfae41_1.png)

And then we'll see how this is used later on， but we're also importing the back end from Cars。

 and we're importing that as K。We're then going to pull out this now function。

 And the reason for that is just we want to get the actual timing。

 and we can use a magic function within these Jupyter notebooks to get the timing as well。

 And we've done that before。 But generally， that tries to compute a confidence interval。

 And it has to do more than one loop through all the data。 And it may take some time。

 So we're just going to lose the aspect of having a confidence interval。

 but be able to do it a bit more quickly。😊。

![](img/ab5245e489ee604bd516a84803cfae41_3.png)

We're then going to set some of the parameters。So we're going to have the same batch size。

 same number of classes， same number of epochs each time。 And those are 1，28，5， and 5 respectively。

We're also going to set the pixel numbers for the number of rows and the number of columns in regards to the pixels of our image。

 and that's going to be 28 by 28。We're going to set our filters。

 which is going to be the depth of each one of our next layers using our convolutional neural net。

We're then also going to set the pool size and that'll just be the square so it'll be two by two in regards to our max pooling as well as the kernel size。

 and that'll be three by three as we create those kernels as well。



![](img/ab5245e489ee604bd516a84803cfae41_5.png)

Now we're bringing out that K that we mentioned earlier， which is just the back end。



![](img/ab5245e489ee604bd516a84803cfae41_7.png)

And we're saying four images， depending on your backend， when you pulled in this data set。

 it'll either have the number of channels of your image or that depth of your image first or last。

So if you think about this as RGB， then there would be a depth of three。

So if the depth was first or the channels were first。Then using RGB， it would be 3 by 28 by 28。

This is just the gray scale， so there's only a depth of one， so it's 1 by 28 by 28。

 and that's the dimensions of your image。Now， if it's not channels first， but channels last。

 then it'll be 28 by 28 by one。 And this is just to ensure， no matter your back ends。

 that you're going to be producing the same results as we are here。



![](img/ab5245e489ee604bd516a84803cfae41_9.png)

![](img/ab5245e489ee604bd516a84803cfae41_10.png)

Now we're going to create a function。In order to actually run our model in the same aspect。

 So we're actually going to be pulling in a model that will set up the framework before actually passing it through this function that we have here。

We'll have our train set， which will be our both our X train and our Y train。

And then so it'll be a tuL， and then we'll have our test set。

 which will be X test and Y test so also a TL， as well as the number of classes。

 so those are going to be the parameters that we pass through this train model function。Now。

 recalling that。This train that we pass in is going to be both X train and Y train。

To define x strain， we say we want the first value from that tuple。

So that's going to be x train and not Y train， and we're going to reshape that。

So that it has the same number of rows， so if you think about pulling out that x train and calling dot shape zero。

 that's just going to give you how many examples you have。



![](img/ab5245e489ee604bd516a84803cfae41_12.png)

Plus， the input shape， and this is the input shape that we defined up here。Which will either be。

The channels and image rows and image columns or the image rows， image columns。

 and then the channels。 And this just ensures that we have those in the right ordering。

 So I'm going to actually run the next cell to make this a bit clearer。

 So this is going to initiate all of our values。 And we think about our X train。



![](img/ab5245e489ee604bd516a84803cfae41_14.png)

![](img/ab5245e489ee604bd516a84803cfae41_15.png)

This is going to be。

![](img/ab5245e489ee604bd516a84803cfae41_17.png)

Our first value in our twople， and then if I call a shape。That first value。

Is going to be the number of examples。 So we say we just want 60，000 plus that input shape。

 so rather than being 60，000 by 28 by 28， it's going to be 60，000 by 28 by 28 by one。Or 60。

000 by  one by 28 by 28， depending which one of these is true。

We're then going to do the same for X test。We then going to ensure that we're working only with float values。

 and then we will make sure all those values are between 0 and 1。And that's， again。

 just by dividing by 255。 These our pixels will all be between 0 and 255。

We'll then print out the shape so that we'll be able to confirm that the shapes are as we expected。



![](img/ab5245e489ee604bd516a84803cfae41_19.png)

And then we can see how many train samples we have， how many test samples we have。Or then。

 because we are using。

![](img/ab5245e489ee604bd516a84803cfae41_21.png)

Classification of values between either0 and 4 or5 through9。

 we actually going to have to create those categorical variables as we've done before。

 create those different classes， doing something along the lines of one hot encoding。

 so I do that for our train set as well as our test set。

And then we call model that compile whatever model we pass in， we will compile it using this loss。

 We're using a different optimizer。 I wouldn't worry too much about this being different than what you've seen before。

 It's fairly similar to the math or R M S prop。 that portion of your atom， if you recall， as well。

But I wouldn't worry too much about it。 We use this specifically so that it wouldn't train quite as fast as something like R M S Pro or Adam actually would。

 After you run this， you can try switching this for atom or R M S prop and see that it actually gets to those optimal values much quicker。

We're then going to also track the metrics of accuracy。 We call T equals now to get the timing。

 We're then going to fit our model and fitting our model is going to be what takes the most time on our x train。

 our Y train， that batch size that we specified earlier。 the number of epochs。 we specified earlier。

 verbose equals one， that's the defaults。 If you set verbose equal to 0 rather than showing the steps throughout each epoch that we've seen every time we've run those deep neural nets。

😊，Those just would not show up。 so that would keep all those extra lines from showing up。

 I generally find those useful， but if you see that they're taking up a lot of room on your screen。

 feel free to fetch set verbose equal to 0。 And then we have that validation set to see how we're doing on our holdout set throughout。

And then to figure out how long it took， we call now again and we subtract that T that we initialize here before fitting our model。

We can then call model do evaluate on X test and Y test in order to get the scoring。

 and that will give us both our error and our accuracy for that model that we ran。



![](img/ab5245e489ee604bd516a84803cfae41_23.png)

So， here， we have initialized。

![](img/ab5245e489ee604bd516a84803cfae41_25.png)

Our train model。We're then going to get the data that we need。 So we're loading。

All of our data calling MN。 low data， which will give us the X train and Y train tuple as well as the X test and Y test tuple。

We're then going to separate out less than5。And greater than 5。So that we have。

X train such as such that our outcome variables are all less than five。

Or are x trains such that they're all greater than or equal to5。



![](img/ab5245e489ee604bd516a84803cfae41_27.png)

![](img/ab5245e489ee604bd516a84803cfae41_28.png)

We're then going to define the feature layers。 And these are going to be those earlier layers that we hope to transfer on our new problem。

 And we're going to freeze these layers during the fine tuning process。

So these are going to be those layers that we freeze if you think back to what transfer learning is and how it works。



![](img/ab5245e489ee604bd516a84803cfae41_30.png)

And we're going to set these all to a list。

![](img/ab5245e489ee604bd516a84803cfae41_32.png)

So features layers are equal to this list and it's going to have this convolutional layer with the filters and kernels that we specified earlier。

 that activation， then another convolutional layer。

 with another activation that max pulling some dropout。

 and then it's going to flatten our convolutional layer into a one dimensional array。

And we pass this into lists and we'll see later on that sequential model can actually take in a list of those features。

 so it has that add functionality。But also， if needed。

 you can pass in all those layers as a list into your sequential function。

And then we're going to have the layers that were actually going to be fine tuning。

 and that's going to be this dense layer， another activation layer。



![](img/ab5245e489ee604bd516a84803cfae41_34.png)

Some drop out and then another dense layer to get it down to the number of classes。

 plus that soft max function。So I'll run each of these。

And then we're setting our model equal to sequential again， as mentioned。

 we can just pass in that list of the different layers。



![](img/ab5245e489ee604bd516a84803cfae41_36.png)

Now we have our model， we can look at the summary。

![](img/ab5245e489ee604bd516a84803cfae41_38.png)

And we see here， in the summary。That we have each one of those layers that we specified before in regards to our different feature layers。

 as well as those classification layers leading all the way to the ends of our soft max function。



![](img/ab5245e489ee604bd516a84803cfae41_40.png)

![](img/ab5245e489ee604bd516a84803cfae41_41.png)

And then we see that we have a total number of parameterss of 600，165 that we're going to the train。



![](img/ab5245e489ee604bd516a84803cfae41_43.png)

Now that we have our model， we can call that function that we created earlier。Using that model。

Then our training set。Then our test set and then our number of classes， which is still equal to five。

So we're going to start running this and this will take。Maybe呃。Two minutes。

 something along those lines， So I'm going to pause here， and once it's done running。

 we'll come back and look at these results and discuss these results。



![](img/ab5245e489ee604bd516a84803cfae41_45.png)

So we see here that that took about three minutes to train， three minutes and 12 seconds here。

 we can also look at the improvement in the accuracy step by step， going from 0。22 to 0。3，0。

38 and so on on regards to our training set and you look at the validation set as well。

 but you see that its slowly getting that accuracy up at each step。

 and probably can continue to improve。Now， our goal here with transfer learning is going to be to freeze certain layers and only train on those later layers。

So CARS allows layers to be frozen during the training process。



![](img/ab5245e489ee604bd516a84803cfae41_47.png)

In order to do what we just said， that is， some layers would have their weights updated during the train process while others will remain pros。

 and they won't be updated。 And this is going to be that core part of transfer learning。

You also want to note that a lot of the training time。

 and we mention this in lecture is going to be spent back propagating the gradients back to that first layer。

 Therefore， if we only need to compute the gradients for a small number of layers。

 the training time should speed up。 should speed up at least a bit， hopefully quite a bit。



![](img/ab5245e489ee604bd516a84803cfae41_49.png)

So in order to freeze the layers， we just set each one of those layers。

 which are going to be for each one of the layers in our feature layers that we defined earlier as those that we will ultimately freeze for each one of those。

 we just set L doc trainable equal to false。And that will freeze the training where it is and won' allow for further training。



![](img/ab5245e489ee604bd516a84803cfae41_51.png)

Now when we look at the model dot summary。

![](img/ab5245e489ee604bd516a84803cfae41_53.png)

We see that our total number of parameters is 609000。

But the trainable parameters is going to be less at 600，165。

And that's going to be due to the fact that we are freezing these upper layers in place。



![](img/ab5245e489ee604bd516a84803cfae41_55.png)

Now we do still have to train a lot， but again， those being later layers。

 they'll be able to update a bit quicker than those earlier layers。



![](img/ab5245e489ee604bd516a84803cfae41_57.png)

So now we can call train model。

![](img/ab5245e489ee604bd516a84803cfae41_59.png)

This time on the values less than 5。 So if we recall going back up。

 we originally trained on the values greater than 5。 We froze those layers。

 and now we want to see our model that we again， just froze those first few layers on and are only allowing for training on those final layers。



![](img/ab5245e489ee604bd516a84803cfae41_61.png)

![](img/ab5245e489ee604bd516a84803cfae41_62.png)

![](img/ab5245e489ee604bd516a84803cfae41_63.png)

![](img/ab5245e489ee604bd516a84803cfae41_64.png)

4 values less than 5。 Sorry， I run this。 And again， this will take some time。

 but hopefully faster than the last one did。And we will come back as soon as is done training and touch on our results。



![](img/ab5245e489ee604bd516a84803cfae41_66.png)

Now， looking at the results that we have here， we see that the total training time came down by a full minute when we're only training five epochs。

 that's quite a bit。And also we're seeing that towards the ends。

We were getting higher overall accuracies。Both in our training set as well as in our validation set。

So we see how this power of transfer learning allowed us to save time while gaining higher accuracy in that shorter amount of time。



![](img/ab5245e489ee604bd516a84803cfae41_68.png)

Now， just to close out， we want to flip these two steps， so rather than。



![](img/ab5245e489ee604bd516a84803cfae41_70.png)

Doing the ordering of。First on training on our greater than five and then freezing layers。

 we're going to train on our less than five， then freeze our layers。

And then run our final model on greater than five。

![](img/ab5245e489ee604bd516a84803cfae41_72.png)

So in order to do that。We're going to reset our feature layers。

 They' are going to be the same values。 But now we want to set retrain them。

 set the right values trainable after training them on a different data set and then doing the same thing for our classification layers。

 leaving those as trainable。And then we set that up equal to Model 2。

 so we have sequential with our new layers， which are going to be the same layers as before。

 just not trained yet。

![](img/ab5245e489ee604bd516a84803cfae41_74.png)

We look at the summary and it should be the same steps。

 except now our total parameters and our training trainable parameters should remain the same。

We can then call our train model function。This time， starting off with the less than five values。

And we run this。And this will take some time to run。

 so we'll see you as soon as this is done running。

![](img/ab5245e489ee604bd516a84803cfae41_76.png)

Now we can see now that it's done running。 we can see the different accuracy numbers。

 as well as the validation accuracy。 We can go through the same steps of freezing those traable layers。



![](img/ab5245e489ee604bd516a84803cfae41_78.png)

![](img/ab5245e489ee604bd516a84803cfae41_79.png)

We look at the summary again。We see that same number of total parameters versus the number of trainable parameters。

And then we can again。Set our train our model， call that train model function。

 and then pass in our new model Model 2 with those layers frozen and try to get that greater than5 accuracy。

We run this and again， this will take a bit to run shorter than the last one。

 I'm going to pause it here and then we will look at the results once it's complete。



![](img/ab5245e489ee604bd516a84803cfae41_81.png)

So as we see here now that the results are completed， we were able to reduce the training time。

 But once we flipped which one we were performing first。

 we didn't quite get the same accuracy results that we had before with a little bit less accuracy。

 a little bit less accuracy on that validation set。

 and that can happen Transfer learning is a bit of an art takes a bit more of understanding where we can fine tune how deep we can fine tune Another thing that we can keep in mind is the fact that each epoch is moving a lot faster。

😊。

![](img/ab5245e489ee604bd516a84803cfae41_83.png)

And we are getting continuous improvement on accuracy。

 so we couldn even add on an extra epoch or two， get that improved accuracy while doing it in less time than just running it from scratch as well。

Feel free to play around with。Training different layers going deeper back。

 seeing how we were able to work with holding certain parts constant and so on。

That closes out our notebook here on Transfer Learn。

 and I look forward to seeing you back in lecture。 Allright， I'll see you there。😊。



![](img/ab5245e489ee604bd516a84803cfae41_85.png)

![](img/ab5245e489ee604bd516a84803cfae41_86.png)