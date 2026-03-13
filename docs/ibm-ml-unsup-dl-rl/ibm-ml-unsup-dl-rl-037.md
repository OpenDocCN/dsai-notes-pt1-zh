# 037：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p37 36_非负矩阵分解笔记本第1部分.zh_en -BV1eu4m1F7oz_p37-

Welcome to our notebook here on non negative matrix factorization In this notebook。

 we're going to be covering the BBC data set on different articles across five different topics。

 The data has been pre processed so that we have a sparse matrix。

 We'll see what that means in just a second。 With that， we have BBC dot terms。

 which is just a list of the words that are used。As well as BBC。 docs。

 which is just going to be a list of the articles。

![](img/ee1882051886c705657e68b6f43676cc_1.png)

Listed by topic。So at a high level， what we're going to do here is turn our BBC do matrix file into an actual sports matrix。

So it's already in sparse matrix form， as we'll see。 But in general。

 working with a sparse matrix just means rather than having a ton of zeros for many of your columns。

 We're just going to have for each row or column。 we will specify whether or not there is a value there and what that value is。



![](img/ee1882051886c705657e68b6f43676cc_3.png)

![](img/ee1882051886c705657e68b6f43676cc_4.png)

![](img/ee1882051886c705657e68b6f43676cc_5.png)

Rather than when you have a larger， not sparse matrix with a lot of zeros。

 you can end up eating a lot of memory。

![](img/ee1882051886c705657e68b6f43676cc_7.png)

We're then going to decompose that sparse matrix， using non negative matrix factorization。

 and then use the resulting components of that non negative matrix factorizations to analyze the topics that we end up coming up with。



![](img/ee1882051886c705657e68b6f43676cc_9.png)

So the first thing that we want to do is take that bbc。mtx， which is our sparse matrix。

 and we're going to open that file， so now we have our file available as F and then once we have that F object we just call read liness and the output of read liness will be output into content。

So now we have our contents and just to see what that looks like。

 I'm going to run this and you see that it's going to be a list。



![](img/ee1882051886c705657e68b6f43676cc_11.png)

And。What we have beyond the first two values are just going to be a sparse matrix representation。

 And we're going to go through in just that part 1 below what each of these different lines mean。

But first， we want to remove each of these first two values。

 So we're just going to call a content dot pop。 We're going to call0 twice， so we remove。



![](img/ee1882051886c705657e68b6f43676cc_13.png)

The zeroth value， and then we remove the zeroth value again。

So we see that the last one that I removed where was this。Value here from the list。

 And now we should only have from here and below， if we call content。



![](img/ee1882051886c705657e68b6f43676cc_15.png)

Now， in part one。We're going to turn this list。 And currently。

 this is a list of strings into a list of tuples。And that list of tuples will represent a sparse matrix。



![](img/ee1882051886c705657e68b6f43676cc_17.png)

So， that sparse matrix。Is going to have as that first column， the word ID。

As the second column within that tuple， that second value， we're going to have the article I D。

And the third is going to be the number of times that that particular word shows up in that particular article。

So as an example here， if word 1 appears in Art 3， two times， then our element for a list。

 that tuo will be word 1。Article 3 showed up two times。Now， in order to create this tuple。

What we do is this somewhat complicated looking list comprehension。

 I will break it down very quickly if we just do C for C in contents。



![](img/ee1882051886c705657e68b6f43676cc_19.png)

Let's actually， that'll just give us the exact list that we saw before。

 So let's just first call that split。

![](img/ee1882051886c705657e68b6f43676cc_21.png)

And we see that we've now split that string that originally had into the three separate values。

 So we're on our way there。 And then all we do from there。



![](img/ee1882051886c705657e68b6f43676cc_23.png)

Is map over a floatat since。It'll be difficult to get a float integer just out of that 1。0。

 We map over a float， and then we take the integer of that float so that we're only working with integers each time mapping。

 So first， we map to each one of the values in this tuple。 the float。 Then we map over the integer。

 And then we set that output as just a tuple。

![](img/ee1882051886c705657e68b6f43676cc_25.png)

And if we look at the output for just those first aid values， we see that we now have a list of tus。

1，1，1，1，7，2， so on and so forth， telling us the word， the article。

 and then the count of that word within that article。



![](img/ee1882051886c705657e68b6f43676cc_27.png)

![](img/ee1882051886c705657e68b6f43676cc_28.png)

Now we want to prepare the actual sparse matrix that we're going to be passing into our NMF into our non negative matrix factorization。



![](img/ee1882051886c705657e68b6f43676cc_30.png)

So we're going to import nuumpy and pandas， and we're also going to import from sippi do Sprse the COO matrix。

 which will give us a means of passing in the way we have our data currently constructed into a sparse matrix。



![](img/ee1882051886c705657e68b6f43676cc_32.png)

So we're going to specify what our rows are going to be。 So since these start off。

 if you look back up here， it's going to actually start with word 1 article 1。

Just for it to match up with Python syntax， we're going to make it word 0， Art 0， so on and so forth。

So we call that。Every single value， X1， these are going to be our rows。

We want our rows to be each one of our different documents。So we say x1 minus-1。

 So it's going to be whatever this idea is minus-1。

For x within sparse matrix that we have defined here。 And then x 0， recall thus the word I D。

 We're going to subtract one from that x0 value， and that will be our different columns。

 So our different columns will be each one of our different words。



![](img/ee1882051886c705657e68b6f43676cc_34.png)

![](img/ee1882051886c705657e68b6f43676cc_35.png)

And then the actual values are going to be the amount of times that that shows up。



![](img/ee1882051886c705657e68b6f43676cc_37.png)

And when we call CO O matrix， we pass in the values。And then with that。

 we have the related rows and columns for where those value should actually fall。

 So it'll plug that in if we have row 1 column 1， it'll plug in whatever that value is。

 So for this second one， it'll say。Rowwen。Or row 7， column 1。Plug in the value too。So we run this。

And just to make this perfectly clear， we're actually going to recreate from that sparse matrix an actual pandas data frame。

 So we know what our actual matrix that we're working with that we're doing non negative matrix factorization on actually is made up of。

😊。

![](img/ee1882051886c705657e68b6f43676cc_39.png)

So we're going to pull in the actual terms， and these will relate。 The0 will be the0 term。

 The first will be the first term， so on and so forth。So we say from this。Flat file， BBC dot terms。

 we'll call F dot read lines again。 And then just to access that first value。

 which is going to be the actual word。 We call C dot split on that string。

 That'll output strings as before。 And we only want the first value。

And that will be our output for words。

![](img/ee1882051886c705657e68b6f43676cc_41.png)

And I'll run this， and we see。The different words that come out。



![](img/ee1882051886c705657e68b6f43676cc_43.png)

And then we'll do the same thing for each one of our document names。



![](img/ee1882051886c705657e68b6f43676cc_45.png)

We can do that。All the codes the same， except we're working with a different flat file。

 and we can see all the different document names。

![](img/ee1882051886c705657e68b6f43676cc_47.png)

![](img/ee1882051886c705657e68b6f43676cc_48.png)

And then I'm going to take that COO。Which we initialize here。

 which is just going to be a sparse matrix。

![](img/ee1882051886c705657e68b6f43676cc_50.png)

We're going to turn that into a numpy array。

![](img/ee1882051886c705657e68b6f43676cc_52.png)

Pass that into our data frame， and we're going to set our column equal to those words we pulled out and our index equal to those columns。



![](img/ee1882051886c705657e68b6f43676cc_54.png)

![](img/ee1882051886c705657e68b6f43676cc_55.png)

So this is going to be the actual original data frame that we're working with。

This is going to be Article 1， business 0，01。And we see that the word ad showed up once。

 The word sales showed up five times profit 10 times so on and so forth。

 And you see the reason why we'd want a sparse matrix is because we'd have all of these zeros for almost every single one of these different articles。

 because we need a separate column for every single word that showed up in any single one of the articles。

 which is why we generally work with sparse matrices。 when we're doing natural language processing。😊。



![](img/ee1882051886c705657e68b6f43676cc_57.png)

![](img/ee1882051886c705657e68b6f43676cc_58.png)

![](img/ee1882051886c705657e68b6f43676cc_59.png)

![](img/ee1882051886c705657e68b6f43676cc_60.png)

So now we have。A。Data frame that we want to work with。

 and the next step will be to decompose our matrix using non negative matrix factorization。

 and we'll save that for the next video， and I look forward to seeing you there。



![](img/ee1882051886c705657e68b6f43676cc_62.png)

![](img/ee1882051886c705657e68b6f43676cc_63.png)