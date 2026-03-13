# IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 -BV1eu4m1F7oz_p94-

Welcome back now in this exercise we're just going to play around with some of the parameters。

 show you some of the parameters that you can play around with。And then。On your own。

 as mentioned earlier， I would suggest you try playing around as well with these max features。

 the max length， as well as something that we won't do the hidden dimension for our R and N。

 I believe we're also going to work with the word embedding dimension and we'll see the performance for each as we move along。

So。

![](img/f11f466a4aaf6b9495c5afc275de31d5_1.png)

First thing that we have is we set the max features equal to 20，000。

 which is the same as what we had before。And then the other thing that we have is that we're setting the max length。

 so recall that we cap off our sequences or our sentences at a certain length and then pad them accordingly as well。

Here， rather than the 30 that we had before， we're going to pad or truncate at 80。



![](img/f11f466a4aaf6b9495c5afc275de31d5_3.png)

Everything else that we have here stays the same。The same holds for our H dimension。

 as well as our wordenbeddings， as well as the setup of our model， our R N N model。



![](img/f11f466a4aaf6b9495c5afc275de31d5_5.png)

So we're just going to run this， we're not going to walk through that again。



![](img/f11f466a4aaf6b9495c5afc275de31d5_7.png)

Again， we're going to use RMS prop and again the loss will be binary cross entropy。

 the metric that we will track is going to be accuracy along with that loss will automatically be tracked。

We run this and then again， finally we will fit on our training set using our X train and Y train and then having that hold out validation data of X test and Y test。



![](img/f11f466a4aaf6b9495c5afc275de31d5_9.png)

So we run this and again， this may take some time to run and the only difference again that we have with this sample versus before is that we're setting the max length where we will truncate our sequences up here at 80。

So I'm going to pause here and we'll come back once this training is done。



![](img/f11f466a4aaf6b9495c5afc275de31d5_11.png)

So that will actually take quite a bit of time to run we're not also going to run the accuracy results as we did before。

 but you can actually see those accuracy results here on the validation set and we see that it went up to 0。

842 compared to what we had before which was 0。7846 and that matches that evaluate output over here So we see we're able to increase our accuracy。



![](img/f11f466a4aaf6b9495c5afc275de31d5_13.png)

![](img/f11f466a4aaf6b9495c5afc275de31d5_14.png)

![](img/f11f466a4aaf6b9495c5afc275de31d5_15.png)

![](img/f11f466a4aaf6b9495c5afc275de31d5_16.png)

Now we want to see again， we're going to play around with just one more parameter here。



![](img/f11f466a4aaf6b9495c5afc275de31d5_18.png)

So we have， again， this time， instead of。

![](img/f11f466a4aaf6b9495c5afc275de31d5_20.png)

20，000 features， which is the amount of actual words we're going to use using the most common words。

 we bring that down to 5，000， keeping that max length up at 80。



![](img/f11f466a4aaf6b9495c5afc275de31d5_22.png)

And then for our word embedding dimension， recall that that's going to take those integer values that we're starting off with。

And convert them into x dimensional vectors here， 20 dimensional prior they were at 50 dimensional vectors。

 So of shrinking that down as well。 So we're changing two features actually here。



![](img/f11f466a4aaf6b9495c5afc275de31d5_24.png)

So you run this to get our new Xtrain and X test。

![](img/f11f466a4aaf6b9495c5afc275de31d5_26.png)

We get our new R&N model， everything else yet the same。



![](img/f11f466a4aaf6b9495c5afc275de31d5_28.png)

And then after that， again， we will use the compile with the same loss function， the same optimizer。

 tracking the same metrics。And we're going to call fit again here。



![](img/f11f466a4aaf6b9495c5afc275de31d5_30.png)

And then after that， this is going to run。 and this will take some time as well。

 So all these will take a bit of time and。It's part of the process when we're doing this deep learning。



![](img/f11f466a4aaf6b9495c5afc275de31d5_32.png)

But here we see it's running through that first epoch。

 and then we'll do that again for another 10 epochs here。

The goal being that if the accuracy on our holdoutet is continuing to improve。

 we should probably run it for more epochs。 And we did see that that was the case up here。

 If We look at the validation accuracy。 We see that it continued to go up。



![](img/f11f466a4aaf6b9495c5afc275de31d5_34.png)

After each epoch， so we probably could have continued to run that and get even higher accuracy。



![](img/f11f466a4aaf6b9495c5afc275de31d5_36.png)

So we're actually going to do that here。 we'll see after 10 epochs how well we were able to perform。

 and then after that， I'm just going to run this now。

It'll run for another 10 epoch and we'll see how much that accuracy can actually improve。

 so I'm going to pause it here and we will get back once we are done having both these items。

 which will be quite a bit of time。

![](img/f11f466a4aaf6b9495c5afc275de31d5_38.png)

![](img/f11f466a4aaf6b9495c5afc275de31d5_39.png)

Now， looking at our results here are going through now 20 epochs， 10 on the first run。

 another 10 on the next run， and that second 10， of course。

 as it was in our last notebook will pick up where we left off so here we see that we had a actually a 0。

8479。

![](img/f11f466a4aaf6b9495c5afc275de31d5_41.png)

![](img/f11f466a4aaf6b9495c5afc275de31d5_42.png)

On the training set， and we see that continues to go up as that loss continues to go down。



![](img/f11f466a4aaf6b9495c5afc275de31d5_44.png)

And we see towards the ends that we get that validation accuracy of about 0。84， that is。



![](img/f11f466a4aaf6b9495c5afc275de31d5_46.png)

![](img/f11f466a4aaf6b9495c5afc275de31d5_47.png)

Around equal to what we were able to accomplish in just 10 epochs。



![](img/f11f466a4aaf6b9495c5afc275de31d5_49.png)

Using the word embeddings with 50 dimensions， max features are 20000 and the max length of 80。



![](img/f11f466a4aaf6b9495c5afc275de31d5_51.png)

I'd say again， feel free to play around with these different parameters。

 see if you can improve the model， but we are also going to in the next lecture start to discuss a more powerful recurrent neural net structure with more long term memory。

 specifically LSTMs。

![](img/f11f466a4aaf6b9495c5afc275de31d5_53.png)

So I'll see you back in lecture where we will pick up with long short term memory models。 All right。

 I'll see you there。

![](img/f11f466a4aaf6b9495c5afc275de31d5_55.png)

![](img/f11f466a4aaf6b9495c5afc275de31d5_56.png)