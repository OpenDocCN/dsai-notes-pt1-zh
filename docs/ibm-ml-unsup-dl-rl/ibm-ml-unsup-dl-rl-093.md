# 093：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p93 54_RNN笔记本（选修部分）第1部分.zh_en -BV1eu4m1F7oz_p93-

Welcome to our demo here on recurrent neuralural Nes。

In this demo we're going to be using recurrent neural nets to classify the sentiment on an IMDB data set IMDB is just going to be movie reviews in general。

 so our data consists of 25，000 training sequences。

 so those are just going to be sequences representing different reviews。



![](img/b83542af3177ccc035d17db1e3cde1da_1.png)

And we're also going to have with it 25000 test sequences that we can test how well we're able to train our data。

 and then our outcome will be binary， either it's a positive review or a negative review。

 and we have those。

![](img/b83542af3177ccc035d17db1e3cde1da_3.png)

Those will be labeled for us。So Cars provides a convenient interface to load in this data and this is actually built into Cars as its dataset set and will immediately encode those words as integers。

 and those are going to be based on the most common words and we'll see in just a bit how those are encoded as integers。

And then from there， what we're going to do is we're going to actually show you how to come up with vector representations of those words and then train our actual recurrent neural nets。

So first things first， we import all the necessary libraries， so cars。

 some that I want to point out here is that we're going to bring in embedding and we didn't talk about this much but when we use embedding what we're doing is we're taking our in this case。

 the integers but taking those sequences and taking those words and coming up with word vectors that will represent the syntax or the context of that word in a way。

 So if you have two words that are basically synonym such as doing something fast or doing something quickly or fast and quickly could have very similar meanings。



![](img/b83542af3177ccc035d17db1e3cde1da_5.png)

![](img/b83542af3177ccc035d17db1e3cde1da_6.png)

The embedding will have vectors that are very similar to one another。

 So that gives you another layer of learning that we're going to come up with。

 And that's going to be our embedding layer。 We're also going to import simple Rn N。

 such just going to be that recurrent neural net， We talked about how that is going to be simpler than the versions we're going to learn in later videos such as LSTMs。

 and that it may have that problem of that longer term learning within a longer sequence。

 But just to learn how these cells kind of piece together when to start off here with a simple R and N。



![](img/b83542af3177ccc035d17db1e3cde1da_8.png)

![](img/b83542af3177ccc035d17db1e3cde1da_9.png)

![](img/b83542af3177ccc035d17db1e3cde1da_10.png)

We're then going to initialize the length of our features and we see that max features is 20，000。

 and this is used when we're loading in our data， using the IMDB data set。

 it's going to pick the most common whatever number of Mac features we have here， 20，000。

 the most common 20000 words。

![](img/b83542af3177ccc035d17db1e3cde1da_12.png)

![](img/b83542af3177ccc035d17db1e3cde1da_13.png)

![](img/b83542af3177ccc035d17db1e3cde1da_14.png)

We're then going to， and we discuss this in lecture a bit。 set the maximum length of the sequence。

 and we'll truncate after this， as well as pad if it's not up to that length。

And then we'll decide our batch size here as well when we do our deep learning， of course。

 we always say what the batch size is， so when we run through one epoch that'll be decided by whatever our batch size is and the size of the entire data set。

 so you can imagine with a batch size of 32 and quite a large data set we'll go through many iterations of gradient descent before going through a single epoch。



![](img/b83542af3177ccc035d17db1e3cde1da_16.png)

![](img/b83542af3177ccc035d17db1e3cde1da_17.png)

![](img/b83542af3177ccc035d17db1e3cde1da_18.png)

![](img/b83542af3177ccc035d17db1e3cde1da_19.png)

We're then going to load in our data and when we load in our data， we just call an imdb。

load data and the only parameter we needed to pass here or that we did pass here is going to be that max features there's 20。

000， which again is the 20，000 most common words within our data set。



![](img/b83542af3177ccc035d17db1e3cde1da_21.png)

![](img/b83542af3177ccc035d17db1e3cde1da_22.png)

And that will output our X train and our Y train as well as our X test and our Y test。

And we can look at the length of each and they should be equal to that 25，000 that we just discussed。

 it'll take just a second to loadier， and here we see 25，000 train sequences and 25。

000 test sequences。

![](img/b83542af3177ccc035d17db1e3cde1da_24.png)

![](img/b83542af3177ccc035d17db1e3cde1da_25.png)

![](img/b83542af3177ccc035d17db1e3cde1da_26.png)

Now， as mentioned， we're also going to pad or tranquaate each of our sequences using that max length that we discussed earlier。

 which was equal to 30 as we see right over here。

![](img/b83542af3177ccc035d17db1e3cde1da_28.png)

So we set sequence。Pulling in that sequence that we've pulled here。In terms of。

From that pre processingcess library。

![](img/b83542af3177ccc035d17db1e3cde1da_30.png)

We have the functionality of padding our sequences。

So KaRS has something built in in order to quickly pad or truncate our sequences。

 and we set x train to that max length as well as our X test to that max length。



![](img/b83542af3177ccc035d17db1e3cde1da_32.png)

![](img/b83542af3177ccc035d17db1e3cde1da_33.png)

And now， when we look at the shape of those sequences。

 we see that there are now 25000 different examples。



![](img/b83542af3177ccc035d17db1e3cde1da_35.png)

![](img/b83542af3177ccc035d17db1e3cde1da_36.png)

Where each one of those examples is of length 30。And if we want to see what one of those examples look like。

 we see here that again， this is meant to represent a bunch of words and each one of those words are represented by a single integer。



![](img/b83542af3177ccc035d17db1e3cde1da_38.png)

![](img/b83542af3177ccc035d17db1e3cde1da_39.png)

Now， our goal is to build out a recurrent neural net。And in order to do so。

 we should dive in a bit into what this embedding layer is as well as how this simple R&M layer works。



![](img/b83542af3177ccc035d17db1e3cde1da_41.png)

So rather than using pre trained word vectors， we're going to learn what those vectors actually are using this embedding layer。



![](img/b83542af3177ccc035d17db1e3cde1da_43.png)

![](img/b83542af3177ccc035d17db1e3cde1da_44.png)

Now， when I say that again， that embedding will allow you to have that context so that similar words will have vectors close to each other so if we're talking about X dimensional space。

 let's see what was our dimensional space here。

![](img/b83542af3177ccc035d17db1e3cde1da_46.png)

![](img/b83542af3177ccc035d17db1e3cde1da_47.png)

![](img/b83542af3177ccc035d17db1e3cde1da_48.png)

啊。We put in word bedding of 50， we see it down here。

Then we are going to have a vector that has 50 numbers。 and in 50 dimensional space。

 one vector should be close to the other if they're similar in meaning。



![](img/b83542af3177ccc035d17db1e3cde1da_50.png)

![](img/b83542af3177ccc035d17db1e3cde1da_51.png)

And we're going to learn whether they're similar in meaning using this embedding layer。



![](img/b83542af3177ccc035d17db1e3cde1da_53.png)

So the layer maps each integer into a distinct dense word vector of length output dim as we just mentioned。

And we can think of this as learning that word vector embedding on the fly。

 So using the context of IMDB reviews。 So it will be specific to IMDB， which could be powerful。

 something to note if you're trying to do embeddings on your own。

 there are pre trained embedding embeddings available， such as word to V。

 and because that's pre traineded and makes it easy to actually。

Take whatever data set and automatically use that embedding and come up with vectors that are similar to one another if they're synonyms。



![](img/b83542af3177ccc035d17db1e3cde1da_55.png)

We then are going to have， again， that input dimension should be the size of our vocabulary。

 and then the input length specifies the length of the sequences that the network is going to expect。

 And we just discussed how we're going to keep that at 30 by padding or truncating accordingly。



![](img/b83542af3177ccc035d17db1e3cde1da_57.png)

![](img/b83542af3177ccc035d17db1e3cde1da_58.png)

![](img/b83542af3177ccc035d17db1e3cde1da_59.png)

![](img/b83542af3177ccc035d17db1e3cde1da_60.png)

We then have our。Simple R&N layer。

![](img/b83542af3177ccc035d17db1e3cde1da_62.png)

Which we're going to pass in the number of units so we can think again to our diagram that we saw earlier and we can say how many units we want that to output。

 we can say what type of activation to use， 10 H is usually best as we pass through our simple R&N。

 but we have our options of working with others and feel free to play around with that。



![](img/b83542af3177ccc035d17db1e3cde1da_64.png)

And then we have our kernel initializer and our recurrent initializer。

Which are going to be the initial values for our weight matrices。 again。

 that kernel initializer is going to be the weights for the input and the currentrent initializer is going to be the initialized weights for those state layers。



![](img/b83542af3177ccc035d17db1e3cde1da_66.png)

![](img/b83542af3177ccc035d17db1e3cde1da_67.png)

![](img/b83542af3177ccc035d17db1e3cde1da_68.png)

Here we're actually going to change the activation to reu if you see。

 so you can try going back to 10h and see how that works。

 and then we're also going to just pass in that input shape。

 which is just going to be if we call xtrain dot shape1。

 then we should have and we can just look at what that is。



![](img/b83542af3177ccc035d17db1e3cde1da_70.png)

![](img/b83542af3177ccc035d17db1e3cde1da_71.png)

![](img/b83542af3177ccc035d17db1e3cde1da_72.png)

X train， Dutcht shape。1。

![](img/b83542af3177ccc035d17db1e3cde1da_74.png)

And we see that that's going to be of shape 30， and that doesn't matter how many examples we have in general。

 when you're trying to pass a shape， you're going to be passing in that shape of what a single vector would look like。



![](img/b83542af3177ccc035d17db1e3cde1da_76.png)

So let's build out our first R&M。

![](img/b83542af3177ccc035d17db1e3cde1da_78.png)

So our R and N hidden dim is going to be equal to 5。



![](img/b83542af3177ccc035d17db1e3cde1da_80.png)

Our wood embedding dimension is going to be 50。 So again， we're going to take those。



![](img/b83542af3177ccc035d17db1e3cde1da_82.png)

Integers that we currently have。And given their context。

 come up with an embedding where it's going to transfer each one of those single values into a vector that's of dimension 50。



![](img/b83542af3177ccc035d17db1e3cde1da_84.png)

We're then going to initialize our model， add on our embedding layer。Passing the max features。



![](img/b83542af3177ccc035d17db1e3cde1da_86.png)

As well as the word embedding dim that max features is going to be what we have here。

 20000 to give us what the actual。

![](img/b83542af3177ccc035d17db1e3cde1da_88.png)

![](img/b83542af3177ccc035d17db1e3cde1da_89.png)

Input dimension is。And then our word embedding is going to tell us our new dimensions。

 And then that's going to be the first layer once we have。



![](img/b83542af3177ccc035d17db1e3cde1da_91.png)

![](img/b83542af3177ccc035d17db1e3cde1da_92.png)

Our new embedding and our data ready to be fed forward。 We can pass that through our simple。

 recurrent neural network。We pass in the number of hidden dimensions， which is just five。

We then call out our kernel initializer， as well as our recurrent initializer， again。

 initializing those weights for that first layer， for our input， as well as that state layer。



![](img/b83542af3177ccc035d17db1e3cde1da_94.png)

What this is is just random normal with very tight standard deviation around that zero for random values。

 and then this is just going to be a diagonal matrix where along a diagonal。

 we're going to have a bunch of ones。

![](img/b83542af3177ccc035d17db1e3cde1da_96.png)

This shouldn't make that large of a difference starting off。

 You can try just removing these and using the default values， which we have up here。 I've tried it。

 and I believe they are around similar performance。 I think this outperforms it by just a bit。



![](img/b83542af3177ccc035d17db1e3cde1da_98.png)

![](img/b83542af3177ccc035d17db1e3cde1da_99.png)

![](img/b83542af3177ccc035d17db1e3cde1da_100.png)

We then set our activation to Relo。Input shape equal to that Xtrain dot shape that we just saw。

 And then finally， to get just one output， because we just want positive or negative。

 we add on that dense layer with the activation of sigmoid。



![](img/b83542af3177ccc035d17db1e3cde1da_102.png)

![](img/b83542af3177ccc035d17db1e3cde1da_103.png)

![](img/b83542af3177ccc035d17db1e3cde1da_104.png)

So now we have our model and we can look at the summary。



![](img/b83542af3177ccc035d17db1e3cde1da_106.png)

And we see we have to train a bunch of parameters for that embedding layer。



![](img/b83542af3177ccc035d17db1e3cde1da_108.png)

Then for the simple Rn N。If we think about it， we're going to have in that initial matrix going from our input to our state layer。



![](img/b83542af3177ccc035d17db1e3cde1da_110.png)

We should have a 50。 We have。 we're trying to。 we have 50 as input。

 and then we have five hidden cells。We add on that bias term。So we end up with。

50 times 5 plus the  five bias term。 So 255 waits there。

 And then to go from one state layer to the next， we recall that we're going to use a 5 by 5 matrix to keep that same dimension。



![](img/b83542af3177ccc035d17db1e3cde1da_112.png)

So that's going to be another 25 weights that we learn and that's how we get 280 parameters that we are currently learning and finally that dense layer。

 which will just be those five input plus the bias term。



![](img/b83542af3177ccc035d17db1e3cde1da_114.png)

![](img/b83542af3177ccc035d17db1e3cde1da_115.png)

We can then call our optimizer， we're going to use RMS Pro with a learning rate of 0。0001。



![](img/b83542af3177ccc035d17db1e3cde1da_117.png)

We're going to use binary cross entropy since we're deciding between 0 and 1。

 we're going to use that optimizer that we just discussed。

 and we're going to track that accuracy as well。

![](img/b83542af3177ccc035d17db1e3cde1da_119.png)

And then finally， in order to fit that model， we can pass that in， pass in our X train， our Y train。

We pass in our batch size， which should we defined early as 32， the number of epochs。

 as well as a validation set， which is going to be our X test and Y test to allow us to evaluate how well we're actually performing and whether we're overfitting on that holdout set。



![](img/b83542af3177ccc035d17db1e3cde1da_121.png)

So you run this and this will take just a bit， so I'm going to pause the video here。

And we'll come back when this is done running and discuss the results。



![](img/b83542af3177ccc035d17db1e3cde1da_123.png)

![](img/b83542af3177ccc035d17db1e3cde1da_124.png)

So now our model has run， went through the10 epochs。

 as we're starting to learn or probably have learned at this point。

 oftentimes it will take a bit for our deep learning models to actually learn each one of the weights and optimize on the models that we're trying to run。



![](img/b83542af3177ccc035d17db1e3cde1da_126.png)

![](img/b83542af3177ccc035d17db1e3cde1da_127.png)

Now we're going to call model RnN。 evaluatevalu and we're going to evaluate on our test set。

 so on our X test and Y test we call evaluate this will take just a second， not too long。

 and we're going to get our score and our accuracy That score is just going to be our binary cross entropy loss。

 so our loss score and our accuracy will just be our actual accuracy and we have all these lines here we're going to scroll down to the bottom since we've printed it out and we see that we have a score that log loss of 0。

45 and then a test accuracy of 0。78。

![](img/b83542af3177ccc035d17db1e3cde1da_129.png)

![](img/b83542af3177ccc035d17db1e3cde1da_130.png)

![](img/b83542af3177ccc035d17db1e3cde1da_131.png)

![](img/b83542af3177ccc035d17db1e3cde1da_132.png)

So that closes out this video and in the next video we're going to briefly touch on different ways that we can manipulate the models that we just went to trying different parameters and hyperparameter we're not going to go through all possible different parameters and hyperparameter。

 but we will discuss them and after we go through it in the next video I suggest that you as well at home go through and try playing around with each one of the different parameters All right。

 I'll see you there。

![](img/b83542af3177ccc035d17db1e3cde1da_134.png)

![](img/b83542af3177ccc035d17db1e3cde1da_135.png)

![](img/b83542af3177ccc035d17db1e3cde1da_136.png)

![](img/b83542af3177ccc035d17db1e3cde1da_137.png)

![](img/b83542af3177ccc035d17db1e3cde1da_138.png)