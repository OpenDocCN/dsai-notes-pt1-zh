# 038：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p38 37_非负矩阵分解笔记本第2部分.zh_en -BV1eu4m1F7oz_p38-

Welcome back to our notebook In this video， we're going to actually conduct non negative matrix factorization。

If you recall， just before we created our data frame that had each one of our different articles for each row。

 and then for each column， we had each one of the different words。

 and the values were how often those words showed up in each one of the different articles。

We're going to decompose that into different topics。And we will end up with two matrices。

 One will be each one of the words and how much they relate to each topic。

 and then the other one will be how to take those topics and recreate those documents that we have。

So in order to do non negative matrix factorization。

 we're going to have to define how many components we want。

 we're going to set the number of components equal to five。

 which is the number of topics that we actually had in the original documents。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_1.png)

And this will allow us to later on compare to see how related the new topics are to the actual topics that we had within each one of the different articles。

So we import NmF。We then call the NMF， we set the number of components。

 our initialization is going to be just random with a random state of 818 recall that with non negative matrix factorization you're not guaranteed to get the same exact solution every single time。

We're going to pass in our sparse matrix。Calling model fit transform on that sparse matrix。

 and that will output this doc topic。

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_3.png)

Which is just going to be a data frame that's going to be of shape 2225。

 which is going to be the number of articles that we had。

 and then we have reshaped that into just the five topics that we now want。

 so rather than having 2025 by if we recall co was about somewhere in the900s in regards to the number of words。

 we have reduced it down to five topics。

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_5.png)

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_6.png)

Now we want to look at the components of this model。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_8.png)

And when we look at the components， all that is is going to be the different words。

And how they make up each one of the different topics that we now have。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_10.png)

So we're going to create a new data frame， which is we're going to call here topic word。

And we're going to pass in that model dot components， that's going to be this output here again。

 going to be the waiting for each one of the words for each particular topic。

We want the index equal to just we'll call it topic one through topic 5。

And then the columns are going to be those words that we pulled in earlier。 And when we look at this。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_12.png)

We can see that。

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_14.png)

We have。For each one of the different topics， how much each one of the different words contribute to that particular topic。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_16.png)

Now， just to make further sense of how this relates the topics and the words。

 as well as the articles and the words， we recall that the original data had five topics， business。

 entertainment， politics， sports and tech。

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_18.png)

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_19.png)

Now I'm going to do topics per dock， and we're going to again pass in the。

Actual values of that doc topic that we pulled in earlier。

We're then going to set as our index rather than if we recall what the docs actually look like。

 this is going to be the different articles。

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_21.png)

It's going to be each one of these different values， business dot 001， business do 002。

That first word before that dot is going to tell us which topic we're working with。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_23.png)

So we're just going to call i dot split on that dot。

And then we're just going to take the first value， so we'll have all business。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_25.png)

Or later on， all entertainment， so on and so forth。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_27.png)

And then our columns will be topic one， topic two， so on and so forth。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_29.png)

And when we look at this， we can see that we have that。

Taking that each one of those original documents and saying which topic they most relate to。

 So you see， business seems to relate most to topic 2。

And we see that repeatedly for every one of the different business topics and then for tech。

 we see that topic four， I believe。

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_31.png)

What we'll see in just a second， what we're going to do here is order to reset the index so that this indexes its own column。

We're then going to group by that index and get the average value for each one of the topics。

And when we do that， we can see that topic one， the max value is politics。Topic two was business。

 topic three was sports， so on and so forth。 Let's just quickly。Just to make clear。

 show you what that matrix looks like。 This is the matrix。

 And we're just seeing which one of these have the highest values。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_33.png)

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_34.png)

And that's how we end up with these different groups。And then to make this perfectly clear。

 we see that topic 1 should， for example， relate to politics。 So if we take our topic word。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_36.png)

That we saw up here。We're going to transpose that so that each one of the different topics are going to be the columns。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_38.png)

And then we will sort by first topic one。With the highest values on top， so ascending goes false。

 and we see party， labor， government， elects， Blair， these all tend to highly relate with politics。

 which makes sense。

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_40.png)

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_41.png)

And if we did topic three， which was sport。We can see that game， play， so on and so forth。

 and you can play around with this with each one in the different topics。

So we see that with this unsupervised model， if perhaps you don't actually have your topics available。

 you can come up with this in a way types of clusters with the non negative matrix factorization。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_43.png)

That closes out our video here and our notebook on non negative matrix factorization。

 And I'll see you back in lecturecher。 All right。 Thank you。😊。



![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_45.png)

![](img/1bfb8c88d0e83e174f3276f7c0cbb2bb_46.png)