# 065：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p65 26_keras笔记本（选修部分）第1部分.zh_en -BV1eu4m1F7oz_p65-

All right， welcome to our notebook here that will introduce Karas， this one's a bit of a lab。

 so hopefully you're able to walk through some of this on your own。

The goal here is going to be to use CARS to build and train neural networks。

 We're going to be using the UCI PiMma diabetes dataset。

 which will just allow us to predict whether or not a certain person has diabetes based on the attributes that we have here so we have nine different features within our data。

 one of them is the outcome variable we're trying to predict so're working with eight features。

 if you recall when we need to pass in that input that input here would be equal to 8。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_1.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_2.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_3.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_4.png)

We're also going to start off。By having a random forest in order to just get a baseline value for what the actual accuracy should be around。

 And then hopefully we'll try and improve on that。 And we may see as we go through this。

 that deep learning may not always be the answer。 And we'll see how much longer it also takes as well。

 But often， obviously will。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_6.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_7.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_8.png)

Be the answer sometimes， so don't say just because here it's not answer that we should never use neural nets。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_10.png)

So the first step is going to be import here in this first cell are going to be libraries that we're already familiar with。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_12.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_13.png)

And then in this second cell here。You see that we're importing。 And again。

 I mentioned in the lecture that Tensorflowlow has now incorporated the Kara syntax in order to get that specifically from Tensorflow。

 we import from Tensorflow dot cars rather than saying from Kas。

 So from Tensorflow do Cars dot models。 we import the sequential function and from Tensorflow docars do layers。

 we import denses。 And we're just going to be working here， I believe， with SgD， we'll see later on。

 But from Tensorflow docars dot optimizers。 we're going to import a couple of options。

 And I would say feel free to try working with some of these other options that we may not use。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_15.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_16.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_17.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_18.png)

We're then going to import our actual file that we're going to be working with and that's going to be this diabetes data frame。

 we're going to name each one of the columns as specified here。

 so we just have a list equal to these names and we're going to use those names specifically when we read in our CSV file。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_20.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_21.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_22.png)

We can then get the shape and。As anticipated， there's going to be nine columns。

 one of them being that outcome， whether or not they have diabetes， and there's 768 rows。

 so this isn't a huge data set。Generally speaking， deep learning will work better when you have a larger data set。

We're then going to set x equal to diabetes df do IO。

 and you see we're taking all of the rows in just that last column。

 everything besides that last column and then for the y variable we're just taking that last column has diabetes。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_24.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_25.png)

We're then going to use that train test split with a 75 to 25% split。

 something that we should already be familiar with so that we get our X train， our X test。

 our Y train and our Y test， and then we can train on our train and test on our test sets。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_27.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_28.png)

We pull out the mean value for y and1 minus y here。 again， those are just going to be zeros or ones。

 so it's going to provide some value between0 and1， and itll show us what proportion of our data set。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_30.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_31.png)

Is already going to be a positive value。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_33.png)

So because we see that 35% of the patients have diabetes whereas 65% do not。

 we can get an accuracy of 65% by just predicting that nobody has diabetes。

 so we've talked about classification and how we need to be careful when working with something like accuracy so we'll look at accuracy but with that we'll also look at the ROC AUC metric as well。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_35.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_36.png)

So we're going to get our baseline using random forest。

 so we're going to train a random forest using 200 trees。

 so our number of estimators that we see here is going to be equal to 200。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_38.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_39.png)

So RF model and we initiate our model random force classifier， this is a classification problem。

 and then we fixed that to our training set， our X train and our Y train。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_41.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_42.png)

Now our models fit。And we're going to want to predict the actual values。

 we're also going to want to predict the probability outputs for each one of our values。

 for each one of our different rows， and we're going to do that so that we can plot our area under the curve。

 that ROC curve in the next cell。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_44.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_45.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_46.png)

So we call RF model dot predictdict on our excess as well as RF model dot predictic Praba in order to get the different probabilities。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_48.png)

And then we can get what our actual accuracy is by passing in the Y tests and the predicted value。

 and then we can get the ROC AUC score by passing in the Y test and the predicted probabilities。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_50.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_51.png)

And here we say that we that will output the predicted probabilities for each one of the classes we want just for the positive class。

 which is why we say one here。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_53.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_54.png)

So we see that our accuracy is about 77。6 better than just predicting not for everything。

 right that was 65%。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_56.png)

And our ROC AUC is going to be 83。6。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_58.png)

We're then going to create this function to plot our ROC curve and we'll use this later on so we create our function so that we can use it again later on。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_60.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_61.png)

We get our false positive rate and our true positive rate。

 as well as the threshold that we're using by calling R OC curve on the Y tests and our predicted values。

😊。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_63.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_64.png)

And that's just going to be whatever we pass in as our YP and we should pass in those probabilities。

 not actual predictions。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_66.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_67.png)

As well as the actual model that we'll be using。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_69.png)

We're then going to or the model name of what we're using so that we can specify this as random forest versus our neural nets。

 which we'll use later。We're then going to just initiate our figure and our axis。

 And then we're going to app plot our false positive rate versus our true positive rate with a black line。

😊，And then we're going to plot just a straight line。

 which would show us if we were to just predict randomly about how well we would do So we can see that area over that line as well。

 And that's going to be a dashed line。 That's going to be 。5 rather than that full line width of one。

 So we'll see a smaller line there。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_71.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_72.png)

Keeping our grid， and then we can set our title as well as our x limit。

 so from essentially zero to 1 on our x and Y axes。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_74.png)

Then once that function has been created， we just call plot R O C on our Y test。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_76.png)

And our wide pre probabilities， again， specifying that we just want the positive values。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_78.png)

And then we're saying that this is a random force for Ar。



![](img/c989c3970ba6f5f7cc0b31125e4668ff_80.png)

And we see here the ROC curve for RF on that piMma diabetes problem。

 and we see it does better than random prediction， and perhaps we can do a little bit better。

 it's not a perfect prediction as we have that ROC AU of 0。836， where1 is perfect。

So that's going to close out our baseline values so you can remember these values as well as this graph that we have here of 77。

6 and 83。6。 And with that， we are now ready to build out our first neural net model。 All right。

 I'll see you in the next video。

![](img/c989c3970ba6f5f7cc0b31125e4668ff_82.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_83.png)

![](img/c989c3970ba6f5f7cc0b31125e4668ff_84.png)