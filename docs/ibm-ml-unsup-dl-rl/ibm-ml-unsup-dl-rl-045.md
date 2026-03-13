# 045：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p45 6_前向传播.zh_en -BV1eu4m1F7oz_p45-

Now let's walk through some of the important terminology that we should keep in mind when working with a neural network and here a multilayer perceptrum。

 as well as some of the basics as to how we get from this first layer up until the final layer that we have from the x's up until the Y's。



![](img/aa3b5855b4090025cb384f3d13520cd6_1.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_2.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_3.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_4.png)

So first off， we have our different weights and those weights will determine how do we combine each one of the different layers along our neural network。



![](img/aa3b5855b4090025cb384f3d13520cd6_6.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_7.png)

So each one of these arrows that will connect x1 to each point each node within that next layer。

 as well as all the lines between the second layer and the third layer。

 these will all signify the specific weights in how to combine each one of these different layers。



![](img/aa3b5855b4090025cb384f3d13520cd6_9.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_10.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_11.png)

We have our input layer， which is just going to be our input data set here just to make it especially clear。

 we can imagine that this first x1， x2 x3 is just going to be the first row where x1 is feature 1。

 x2 is feature 2 and x3 is feature 3。

![](img/aa3b5855b4090025cb384f3d13520cd6_13.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_14.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_15.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_16.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_17.png)

We then have our hidden layers and those are going to be all of these purple nodes that fall between our input layer and what we will define right now as our output layer。

 so everything between our input layer and output layer are going to be called our hidden layers。



![](img/aa3b5855b4090025cb384f3d13520cd6_19.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_20.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_21.png)

And those hidden layers as we specified as we walk through the Python syntax， can be defined。

 however we'd like， however many layers we'd like， we can say we want five hidden layers and there would be five different columns of nodes in between our input layer and our output layer。

 that's something that we would predeefine and all the weights would connect each of those in order to learn this complex model feeding from our input layer through the hidden layers out through the output layer。

 which will be our actual predictions。

![](img/aa3b5855b4090025cb384f3d13520cd6_23.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_24.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_25.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_26.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_27.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_28.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_29.png)

The weights that we set are the different arrows are going to be represented by matrices。

 and each of those different matrices will again just be the way that we combine each layer step by step。

 and those matrices will have to be of the appropriate shape to ensure that if we have an input that's going to be three vector that it transforms it into a four vector in the next layer and then maintains that four vector in the next layer and then brings that down to a three vector in that final layer。

 and I'll walk through this in just a second。

![](img/aa3b5855b4090025cb384f3d13520cd6_31.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_32.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_33.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_34.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_35.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_36.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_37.png)

Our net input will be the sum of the weighted inputs， So that's going to be your z values。

 and that's going to be， again， similar to your linear regression。

 So x1 times sum weight plus x 2 times some weight plus x3 times sum weight will equal one of your values of z。



![](img/aa3b5855b4090025cb384f3d13520cd6_39.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_40.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_41.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_42.png)

And then we will have four different z values for that first layer。

 so our z is actually going to be a four vector as well our Z2 and then our Z3 will be a3 vector。



![](img/aa3b5855b4090025cb384f3d13520cd6_44.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_45.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_46.png)

We then finally have our activation values and those activation values are just going to be taking those Z values that we just discussed and passing them through our activation function。

 So I'm going to briefly skip over a0 here。 but a1 should be a4 vector as well。

 or we just take that z1 and pass it through， for example。

 each one of those different values in that four vector， Pass it through the sigmoid function。

 We can do the same for a2， passing through Z2。 and then for a3， we can pass Z3。

 probably here through a。😊。

![](img/aa3b5855b4090025cb384f3d13520cd6_48.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_49.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_50.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_51.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_52.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_53.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_54.png)

Soft max layer in order to give the predicted probabilities that we'd want output for this classification problem that we have here。



![](img/aa3b5855b4090025cb384f3d13520cd6_56.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_57.png)

Now if we go back to a0。

![](img/aa3b5855b4090025cb384f3d13520cd6_59.png)

A0 is signifying that we want any A to be passed into the next layer。



![](img/aa3b5855b4090025cb384f3d13520cd6_61.png)

Even though we're not doing anything to the x1， x2 and x3。

 that is going to be fed as input into the next layer。

 so we'll often call that a0 just for simplicityimp' purposes。



![](img/aa3b5855b4090025cb384f3d13520cd6_63.png)

![](img/aa3b5855b4090025cb384f3d13520cd6_64.png)