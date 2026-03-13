# 092：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p92 53_循环神经网络详解.zh_en -BV1eu4m1F7oz_p92-

InThis video， let's get into the mathematical details behind your currentren neural nets。

So starting out， we have our inputs， W I， where I represents the eith position in our sequence。

 And as we talked about so far， that would be the ethe word within our sentence。And with that。

 we also have SI， which is the state at position I that holds all past information that should be passed through our network。

We have Oi， which is the output， that position I。And to calculate S。

As mentioned in the previous video， we take a function of the linear combination of our input。

Added on to a linear combination of our prior state。

And that function should be some nonlinear activation function。

And then to get our output or our final output， we take a linear combination of our current state and pass that through the activation function and assuming we're trying to predict classes that may be a softm。

 for example。😊。

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_1.png)

So what are we doing here？We get our current state as a function of the old state and that current input。

We then get our current input as a function of that current state。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_3.png)

And we learn the appropriate weights for these function by training through our network。

 so what are going to be the different weights that we need in order to get that current state。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_5.png)

Now， if we think through our matrix multiplication。

 thinking through all this as passing in just a single input。In reality。

 we would pass through a batch at a time。Then we're starting off with an input of dimension R。

 and in our example， that represented a single word from our input。

 so that's going to be a vector of dimension R。We then have S is going to be the dimension of our hidden state。

And T， we are going to use as the dimension of our output vector After passing that through our dense layer at that final layer。

So in order to get the transformations that we need。And thinking back to that。

The visualization that we saw earlier。You will be S by R matrix so that'll take our R dimensional vector。

 our word vector input and return something that is an S vector so it's the same shape as our state。

W is then an S by S vector。 So we'll take that prior state of dimension S and keep it in dimension S。

 So it'll still be an S vector。And then finally， V is going to be a t by S vector。Or a T by S matrix。

 And it'll transform our S vector from that hidden state into something that is of size T or T vector that fits the dimensions of our output。

And with that， we should note that the learned weights U。

 V and W are going to be the same across all positions。 So we saw that unrolled version of our R N N。

And we had that U show up repeatedly。 We should note that that U or that V or that W will be the same throughout。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_7.png)

And as mentioned as well。Throughout， we will often ignore the intermediate outputs and only care about that final output that has seen all inputs from our sequence。

 So thinking about that unrolls R and N， we discussed that output  for being that final output。

 We only really care about that final output。

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_9.png)

In order to train recurrent neural nets， there's going to be a slight variation to our normal back propagation method called back propagation through time that allows us to update the weights within our recurrent neural network。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_11.png)

Now we're not going to get into too much detail about this。

But one can imagine that recurrentin neuralural nets must learn weights by updating across the entire sequence。

 And thus， if the sequence is very long。We are even more prone to that banishing exploding gradient problem than we are with our regular feed forward neural nets。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_13.png)

And in practice。We are going to set a maximum length to our sequences to ensure that they don't get too long。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_15.png)

And with that in mind， if the input is shorter than that maximum， then we just pad that sequence。

 and if it's too long， then we would truncate it。

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_17.png)

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_18.png)

And this ensures uniform input lengths for all of our sequences。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_20.png)

Now we touched on this briefly earlier。But although RNNs are often used for text applications and those are the examples we've seen。

 there are multiple uses for working with such a framework。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_22.png)

They can be used for all types of sequential data， including customer sales， loss rates。

 or network traffic over time。

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_24.png)

Speech recognition， so working with audio input for call center automation and voice applications。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_26.png)

For manufacturing sensor data to tell where along a chain， failure may happen to occur。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_28.png)

And has even been extremely powerful in regards to our ability to now do genome sequencing。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_30.png)

So we talked to all about RNNs and some of the powers of RNNs。

 but one of the major weaknesses of RNs。Is that the nature of that state transition as it's currently constructed？

Makes it hard to leverage information from the dispants， or in other words， early on in our sequence。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_32.png)

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_33.png)

With that in the mind， in our next lecture， we're going to introduce LSDMs or long。

 short term memory。

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_35.png)

We's use similar concepts to what we just learned。

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_37.png)

But have a more complex mechanism for updating the state that allows for longer term memory。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_39.png)

So that closes out our section here on recurrent neural nets。In this section。

 we discussed recurrent neural networks and their motivation in regards to learning neural networks for sequences。

We discuss the practical and mathematical details for how allows for providing context for our sequential data。

And then finally， we touched on the limitations of our current neural nets in accounting for information throughout the entire sequence。

 especially those longer sequences and how in the next video。

 we're going to introduce LSDMs to help account for such issues。😊。



![](img/ec9dd1f7b361e28bea7d295cd73e83b2_41.png)

All right， I'll see you there。

![](img/ec9dd1f7b361e28bea7d295cd73e83b2_43.png)