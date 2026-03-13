# 035：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p35 34_降维笔记本第3部分.zh_en -BV1eu4m1F7oz_p35-

Welcome back for part 5 of our notebook here。 Here。

 we're going to introduce Colonel PCA or PC A working with a kernel。

 where we're going to use what we discuss in lecture and that we can come up with a nonlinear combination rather than the linear PCA。



![](img/d402fa65fcd02c609e205665bb6f73e6_1.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_2.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_3.png)

To come up with a way， to say where the high variance is by mapping up to higher dimensions。

 to get that curvature in those lower dimensions。

![](img/d402fa65fcd02c609e205665bb6f73e6_5.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_6.png)

Now。We want to know。Choosing here that our kernel is equal to RBF。

 we can also search through different kernels， and I suggest you looking at the documentation as well。



![](img/d402fa65fcd02c609e205665bb6f73e6_8.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_9.png)

But we can also search through when we're working with RBF using different gammas。

 and that'll tell you essentially how complex that boundary is going to be or how curvy your line that you can project onto will actually be。



![](img/d402fa65fcd02c609e205665bb6f73e6_11.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_12.png)

So we're going to search through different gammas and we're going to use grid search and when we use grid search。

 what we're trying to do is find the best model and when we do this with supervised learning。

 this is clear we can do this with using a scoring methods such as mean squared error or working with the accuracy or whatever other classification score you want to use and optimize on that score。



![](img/d402fa65fcd02c609e205665bb6f73e6_14.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_15.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_16.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_17.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_18.png)

Now when we're using unsupervised learning。It's not quite as clear how we can end up scoring which one of these different models performeds better。



![](img/d402fa65fcd02c609e205665bb6f73e6_20.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_21.png)

But we do need to come up with some type of scoring option in order to decide which gamma or if we wanted to search through different kernels。

 which kernel work the best。

![](img/d402fa65fcd02c609e205665bb6f73e6_23.png)

So what we're going to do here is we're going to introduce a custom scoring method。

 So you'll see here that we define a score。And we'll walk through what that score is。

 but essentially what we're going to do is take a model。Fit a PCA， fit a PCA model to our data。

 and then take the inverse of that。And then see how far away the inverse of that PCA model is from our original data。

 and the lower that value is， the better we did。So let's walk through that here。

 So first we're going to import the kernel PC rather than just PCA。

We're going to import grid search C as we'll be using that in order to find the optimal hyperparameters for our kernel PCA。

And then you'll see in just a second how we're going to incorporate mean squared error in regards to coming up with the best version of our kernel PCA。

So first thing that we're going to do is define a score。So we're going to pass into that score。

The PCA model。As well as our X。 And there's going to be no Y here。 It's just going to be that X。

 right， We're using unsupervised data。 There's no label that we're attributing to this。

All we're doing here with this try and accept is just we want to ensure that we are working with a nuy array rather than working with a pandas data frame。

 So if x is equal to a this x is equal to a pandas data frame， we call dot values。

 and we're working with the array。

![](img/d402fa65fcd02c609e205665bb6f73e6_25.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_26.png)

If it's already an array， then it'll just set X file to that array。



![](img/d402fa65fcd02c609e205665bb6f73e6_28.png)

We're then going to call our PCA model that we passed into the score。



![](img/d402fa65fcd02c609e205665bb6f73e6_30.png)

And we're going to call it on the X Val， and we fit transform our data to get our new version with whatever。

 however many components we're passing through， one component， two component， so on。

 as well as whatever kernel we're using and whatever gamma we're using。



![](img/d402fa65fcd02c609e205665bb6f73e6_32.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_33.png)

Specific to what this PCA model is。

![](img/d402fa65fcd02c609e205665bb6f73e6_35.png)

We're then going to take the output of that。And pass it into this PC dot inverse transform function to get the inverse。

 which should undo what we did， but it can't perfectly undo because we lost some information as we did that original transformation as we did that original dimensionality reduction。

 So it'll take the inverse and that will be our new data in。



![](img/d402fa65fcd02c609e205665bb6f73e6_37.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_38.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_39.png)

And then what we're going to do is take the original data that we had。

And see how far off that is from our inverse transform that we just did。And in order to do that。

 we'll just take the mean squared error。Now when we do a score。

 we want to get the highest value possible， when we do mean squared error。

 obviously we want to minimize our mean squared error。

 so we're just going to multiply it by negative one so that we can optimize by getting the highest value。

And that's going to be our scoring function。From there。

 it should be as simple as any other grid search that we've worked with in the past。



![](img/d402fa65fcd02c609e205665bb6f73e6_41.png)

You're going to set your parameter grid， which is going to be gamma and we'll loop through different gamma values。

 It's going to be the dictionary and the number of components and we'll loop through different numbers of components。

 Now， I'll let you know， generally speaking， the higher the number of components。

 The better this transform inverse transform will work。

 But this will allow us to hone in on the right level of gamma。



![](img/d402fa65fcd02c609e205665bb6f73e6_43.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_44.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_45.png)

But then going to do grid search CV。We're going to say that we want to pass in the kernel PCA。

 and the things that we don't want to search over， but want to keep the same through every single loop is going to be that the kernel is equal to RBf。

 and we want it to fit the inverse transform。 If we don't call this when we call the PC A。

 Then we won't have the option to call this inverse transform that we have called up here during our scoring function。

 So we say fit inverse transform equals true。 We can then pass in our parameter grid that we defined up here。

 And then we can pass in the score that we just created。



![](img/d402fa65fcd02c609e205665bb6f73e6_47.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_48.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_49.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_50.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_51.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_52.png)

We say n jobs equal thank1， just to say we want to paralyze as much as possible。

 and then using this kernel PCA that we're defining here。

 we can call kernel PC do fit on the data and get our best estimator to see which one of these gammas perform the best。



![](img/d402fa65fcd02c609e205665bb6f73e6_54.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_55.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_56.png)

So I'll run that， and that will take just a second to run。 So I'm going to pause the video， oh。



![](img/d402fa65fcd02c609e205665bb6f73e6_58.png)

There it is。 Never mind。 And we see here that we have for our gamma value，0。5 was the best。



![](img/d402fa65fcd02c609e205665bb6f73e6_60.png)

Option in regards to that transform to inverse transform。 And we see that the number of。

Components is equal to 4， which is the max value， which is what I said。

 usually when you are working with looping through the number of components。

 the max value will be the one chosen。

![](img/d402fa65fcd02c609e205665bb6f73e6_62.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_63.png)

But now we can see that we should probably use that gamma equals 0。

5 when choosing our gamma for our kernel PCA。

![](img/d402fa65fcd02c609e205665bb6f73e6_65.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_66.png)

Now， for part 6， we're going to show you how you can use PCA built into your modeling pipeline in order to perhaps use it to make your logistic regression work better on the data that you have。



![](img/d402fa65fcd02c609e205665bb6f73e6_68.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_69.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_70.png)

So we're going to be loading in this very large data set。

 which is the human activity recognition using smartphones。 We've seen this before。

 It has tons of different columns。 We can look at the shape here， and see that。😊。



![](img/d402fa65fcd02c609e205665bb6f73e6_72.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_73.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_74.png)

It is 10，299 rows and 562 different columns， so we're going to try and reduce that number of columns。



![](img/d402fa65fcd02c609e205665bb6f73e6_76.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_77.png)

So what we're going to do is we're going to first import the different libraries needed。

 our pipeline， standard scalar， stratified shuffle split to keep that same ratio of each one of our different outcome values。



![](img/d402fa65fcd02c609e205665bb6f73e6_79.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_80.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_81.png)

We're now using logistic regression and we can pull in our accuracy score since we're doing a classification problem here。

X is going to be all values except for activity。 Y is going to be the activity。

And then we're going to initiate our stratified chael split and we'll call this in just a bit when we want to get our average score。



![](img/d402fa65fcd02c609e205665bb6f73e6_83.png)

Now， this get average score。Is going to just be a function that does all the steps in the pipeline to standard scaling。

 PCA， and then logistic regression， and all we're going to change at each one of the steps is the number of components。



![](img/d402fa65fcd02c609e205665bb6f73e6_85.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_86.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_87.png)

So we set this pipe equal to this list and we pass it into our pipeline as we've done before。

We have our scores， which are just blank。 So we initiated our pipeline， but haven't fit anything yet。

 We have our scores equal to that blank。 But then using the S S S that we initiated here。

 that stratified shuffle split。

![](img/d402fa65fcd02c609e205665bb6f73e6_89.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_90.png)

And we're going to get five different splits since we set the number of splits equal to five。



![](img/d402fa65fcd02c609e205665bb6f73e6_92.png)

And for each of those， we'll get a new X train and a new X test。

 as well as a new Y train and a new Y test。

![](img/d402fa65fcd02c609e205665bb6f73e6_94.png)

And we can call pipe， that being the pipeline we created here。



![](img/d402fa65fcd02c609e205665bb6f73e6_96.png)

Dot fit on our X train and Y train。And then once we do that five different times throughout each time we're also going to get the accuracy score on the test set。

 So once it's fit on the training set， we can see the actual score on the test set。

 we'll have five different scores， and then we'll output the average of those five different scores。



![](img/d402fa65fcd02c609e205665bb6f73e6_98.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_99.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_100.png)

We're going to set the number of ends from 10 up to 500， so we see our original data was 562。



![](img/d402fa65fcd02c609e205665bb6f73e6_102.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_103.png)

We're going to see if we reduce the number of dimensions。

 is there a point where perhaps we don't need all of the data set or even perhaps some improvement with lower dimensions。



![](img/d402fa65fcd02c609e205665bb6f73e6_105.png)

So we're going to get our score list by running this get average score that we defined find up here on each n in this option of ends that we have here。



![](img/d402fa65fcd02c609e205665bb6f73e6_107.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_108.png)

So I run this。And this one will actually take some time。 So I'm going to pause the video here。

 and I'll see you in just a bit as we touch on the results from running this function。 All right。

 I'll see there。😊。

![](img/d402fa65fcd02c609e205665bb6f73e6_110.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_111.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_112.png)

All right。 now that has。Finish running and it may have taken a couple minutes。



![](img/d402fa65fcd02c609e205665bb6f73e6_114.png)

Let's see what these scorereless came out as。We run this and this should be in the same order as our ends that we have here。



![](img/d402fa65fcd02c609e205665bb6f73e6_116.png)

And we see that after a certain point once we get to the 450500 range。

 there doesn't seem to be any more improvement in adding more variables and adding more features。



![](img/d402fa65fcd02c609e205665bb6f73e6_118.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_119.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_120.png)

And we can see this with the plot as well， just plotting out ends versus our different score lists。

 and we can see that it really plateaus and it's not even starting at 0 here on the Y axis。

 starting at 0。84。 So we see that adding on all these extra dimensions doesn't really add that much extra value in regards to the logistic regression。

 So we could probably shrink this down to even 100 features here。



![](img/d402fa65fcd02c609e205665bb6f73e6_122.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_123.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_124.png)

Or 200 features and still have a pretty high accuracy。

 depending on what you're trying to get at and be able to speed up the process of how long it will take to learn this model。



![](img/d402fa65fcd02c609e205665bb6f73e6_126.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_127.png)

That closes out our demo here on Diality reduction， and I'll see you back at lecture。 Thank you。



![](img/d402fa65fcd02c609e205665bb6f73e6_129.png)

![](img/d402fa65fcd02c609e205665bb6f73e6_130.png)