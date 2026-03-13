# 017：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p17 16_维数灾难笔记本第4部分.zh_en -BV1eu4m1F7oz_p17-

Welcome back to the final video for this notebook。In this final video。

 we're going to show how dimensionality， how high dimensionality can end up affecting model performance。



![](img/b002437c2fa3ec18f3a613ce171a9e15_1.png)

And with that， I want to quickly touch on again how we can fight the curse of dimensionality。



![](img/b002437c2fa3ec18f3a613ce171a9e15_3.png)

And two different methods that should immediately come to mind as we discuss them in the intro to this course are going to be feature selection。

 where you would use domain knowledge to reduce the number of features。

 given the ones that you think are already informative。



![](img/b002437c2fa3ec18f3a613ce171a9e15_5.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_6.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_7.png)

As well as feature extraction and with feature extraction。

 you're going to use dimensionary reduction techniques such as PCA。

Which we'll learn later on within this course to transform our raw data into lower dimensionality data that will preserve hopefully the majority of our variability in that original data。

 And again， well touch on this later on in the course。



![](img/b002437c2fa3ec18f3a613ce171a9e15_9.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_10.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_11.png)

So here we're going to show creating play data sets how high dimensionality will end up affecting our model performance。



![](img/b002437c2fa3ec18f3a613ce171a9e15_13.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_14.png)

In order to do so， we're going to import。Many libraries here we're doing a classification problem。

 so we need our train test split， we're going to need our standard scalar。

 and then something new that we haven't seen yet is we're going to use this make classification function。

 which is available in Scalalar do data sets， and that's just going to create a toy dataset set with a certain amount of classes。

 and I'll show you this in practice in just a second。

 and then we're going to use our decision tree classifier to ultimately predict the class。



![](img/b002437c2fa3ec18f3a613ce171a9e15_16.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_17.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_18.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_19.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_20.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_21.png)

So first thing that we're going to do is create our classification data set in order to do so。

 we're using this make classification。

![](img/b002437c2fa3ec18f3a613ce171a9e15_23.png)

Function that I just introduced a second ago， and I'm going to show you a bit about how these arguments work。

 So I'm going to create a cell above。

![](img/b002437c2fa3ec18f3a613ce171a9e15_25.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_26.png)

And of course， first we're going to have to import that library。

 but're then going to use this to create our X and y。 So now we have our x。



![](img/b002437c2fa3ec18f3a613ce171a9e15_28.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_29.png)

And our x is going to be this two dimensional data set。

 the default is that there are going to be 100 samples。 you see here that it has 100 samples。

 so if we were to run x dot shape。

![](img/b002437c2fa3ec18f3a613ce171a9e15_31.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_32.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_33.png)

We would see that we have 100 rows and two features。



![](img/b002437c2fa3ec18f3a613ce171a9e15_35.png)

Those two features are going to be decided because we said that we want the number of features equal to two。

We're saying that the number of features that are redundants that don't give any extra information are going to be equal to 0。

 If you imagine， often we will have redundant features， such as when we discussed。

 if you're talking about age versus whether or not they're a senior。

 there will be a bit of redundancy built in。

![](img/b002437c2fa3ec18f3a613ce171a9e15_37.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_38.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_39.png)

The number of informative features will be the rest of that。

 so we're saying all of our features will be informative here。



![](img/b002437c2fa3ec18f3a613ce171a9e15_41.png)

And then the number of class clusters per class will allow us to spread out that data in a way。 Now。

 I'm going to plot。Each one of our different classes。



![](img/b002437c2fa3ec18f3a613ce171a9e15_43.png)

Along with that X， we have which class each one of those values belong to。



![](img/b002437c2fa3ec18f3a613ce171a9e15_45.png)

In order to look at both of those， we're going to use a scatter plot， and we're going to scatter。



![](img/b002437c2fa3ec18f3a613ce171a9e15_47.png)

Are x such that y equals 1。And that'll be。On X， we want first， our first feature。

 Then we're going to once our second feature。And then I'm going to use this again。



![](img/b002437c2fa3ec18f3a613ce171a9e15_49.png)

To create another scatter plot。That's going to be。Our y equal to 0。



![](img/b002437c2fa3ec18f3a613ce171a9e15_51.png)

And everything else the same。So here you see our two different classes。

 they're differentiated fairly clearly。And just to show you how different things work。

 if we were to say the number of clusters is equal to one。

 so we don't have separate clusters within our different classes。



![](img/b002437c2fa3ec18f3a613ce171a9e15_53.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_54.png)

Then you see that they're very clearly separated， so adding on this extra cluster allowed them to be a little bit closer together as there were going to be separate clusters for that class。

Also， to go along with that， if instead of having both of our features being informative。

If we only had。

![](img/b002437c2fa3ec18f3a613ce171a9e15_56.png)

One of our features as informative。Then we would see that the other one is redundant in everything along one axis。

 So we don't create this separation， And there's no use in one of those features。

 One of those features don't essentially add any extra value or combined。

 they don't add any extra value。

![](img/b002437c2fa3ec18f3a613ce171a9e15_58.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_59.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_60.png)

So that's how the make classification works。 We saw that original plot of what our data actually looks like when we're working with two features。



![](img/b002437c2fa3ec18f3a613ce171a9e15_62.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_63.png)

We're then going to add on that a bit of noise。So we're just going to use our random state here。

 We're setting a random state equal to 2 with that object， we can call range dot uniform。

 and we're going to be adding on two times a bunch of random values of the same shape as x。

 So we're adding 2 x， something the same size as x。

 So it'll add to each one of the individual data points within our 100 by two array。



![](img/b002437c2fa3ec18f3a613ce171a9e15_65.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_66.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_67.png)

And we're going to add on values that are between0 and 2。

 So the default for range dot uniform will be values between 0 and1。



![](img/b002437c2fa3ec18f3a613ce171a9e15_69.png)

And we're going to multiply that by two， soll be values between0 and 2。

We're then going to scale our data so that it is all between0 ends。 Well。

 so that the standard deviation， the mean will be0 and the standard deviation will be 1。

 and now that we have our data resetting x to the standard scalar version of itself。

 we can split it into our x train， x test， Y train and Y test。



![](img/b002437c2fa3ec18f3a613ce171a9e15_71.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_72.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_73.png)

So we have our toy data set。

![](img/b002437c2fa3ec18f3a613ce171a9e15_75.png)

We can then use our decision tree classifier。And run that on X train and Y train and see what our score is for our X test and Y test。



![](img/b002437c2fa3ec18f3a613ce171a9e15_77.png)

And we see that our score from this two feature classifier is 0。875。



![](img/b002437c2fa3ec18f3a613ce171a9e15_79.png)

Now we're going to run all the same steps and what's important to note here is that the number of features is obviously going to be going up 100 fold。

 but with that， we're also ensuring that each one of those different features are informative。



![](img/b002437c2fa3ec18f3a613ce171a9e15_81.png)

So we're not allowing for redundant features here， so we still have all of our features being informative。



![](img/b002437c2fa3ec18f3a613ce171a9e15_83.png)

We are going to run through the same steps。 Otherwise everything else is the same。 We are going to。



![](img/b002437c2fa3ec18f3a613ce171a9e15_85.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_86.png)

Said our range。Again， setting that random state， adding on that extra noise two times the uniform value。

 run through the steps of setting up the training set and the test set。



![](img/b002437c2fa3ec18f3a613ce171a9e15_88.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_89.png)

Then we're going to use again our decision tree classifier on our standard scale data。

 Check our score on our test set after fitting on our train set。

And we see that our score goes all the way down to 0。425。



![](img/b002437c2fa3ec18f3a613ce171a9e15_91.png)

So we see that adding on additional features， even if they're informative。

 end up leading to worse model performance。

![](img/b002437c2fa3ec18f3a613ce171a9e15_93.png)

Due to the fact that it will very heavily increase the amount that it will overfi to each one of these features。



![](img/b002437c2fa3ec18f3a613ce171a9e15_95.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_96.png)

And something to note along with this， is that， as we mentioned during the lectures， you should。

 if you are going to have more features， try to also have more rows of data。

 So if we had enough rows of data， maybe we can counteract this problem。 But generally。

 if you' are going to have a certain amount of rows。 The less features you have。

 the more informative each of those features can be， less likely you will be to overfit。



![](img/b002437c2fa3ec18f3a613ce171a9e15_98.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_99.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_100.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_101.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_102.png)

We're then going to， rather than just looking at 2 and 200 loop through values between 50 and 4000 and run through each one of these same steps。



![](img/b002437c2fa3ec18f3a613ce171a9e15_104.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_105.png)

So all the steps are going to be the same。 We're calling for nu and NP dot L space starting at 50。

 That's our increments up till 4000， counting by 50。

 And we're just going to continuously pass in that numb for a number of features。



![](img/b002437c2fa3ec18f3a613ce171a9e15_107.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_108.png)

As well as。By setting number of redundant equal to zero by default， all of them will be informative。



![](img/b002437c2fa3ec18f3a613ce171a9e15_110.png)

And then everything else is the same。 We can get each one of our different scores as those are going to be appended on to this empty list。



![](img/b002437c2fa3ec18f3a613ce171a9e15_112.png)

We run this and this will take just a second to run， and then we can plot that as well。



![](img/b002437c2fa3ec18f3a613ce171a9e15_114.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_115.png)

Just looking across each one of the numbers of different features and seeing the classification and accuracy as we increase the number of features。

Now， by chance， some of these can be a bit more accurate。

 but adding features in general can very much lead to reductions in accuracy， not all the time。

 but it very easily can。

![](img/b002437c2fa3ec18f3a613ce171a9e15_117.png)

So in this example， the accuracy is highly volatile in the number of features and increasing features again can reduce that accuracy。



![](img/b002437c2fa3ec18f3a613ce171a9e15_119.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_120.png)

Additionally， in our example， we testified that none of the features are redundant and in practice when you have this many more features。



![](img/b002437c2fa3ec18f3a613ce171a9e15_122.png)

Generally speaking， you will almost definitely have redundant features。



![](img/b002437c2fa3ec18f3a613ce171a9e15_124.png)

And for example， if we are predicting customer churn， as we've discussed throughout these courses。

 using a variety of customer characteristics， we may have collected extensive data say for each customer that we have across many dimensions。

 and this would be an example in practice of high dimensional space。

 which can make it difficult to apply unsupervised learning methods directly。

 and potentially lead to issues within this cursive dimensionality as we try to create these groupings。



![](img/b002437c2fa3ec18f3a613ce171a9e15_126.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_127.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_128.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_129.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_130.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_131.png)

So that closes out our video here on the curse of dimensionality with that we're going to go back to discussing different types of groupings。

 different types of clustering algorithms， starting off with a glloative hierarchical clustering。

 and I look forward to seeing you there。

![](img/b002437c2fa3ec18f3a613ce171a9e15_133.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_134.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_135.png)

![](img/b002437c2fa3ec18f3a613ce171a9e15_136.png)