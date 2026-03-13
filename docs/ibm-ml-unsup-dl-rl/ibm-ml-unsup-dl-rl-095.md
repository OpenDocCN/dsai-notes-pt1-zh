# 095：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p95 56_长短期记忆网络（LSTM）.zh_en -BV1eu4m1F7oz_p95-

In this section， we're going to introduce the more complex version of recurrent neural networks。

 such as long short term memory， which we have listed here or LSTM。

 as well as gated recurrent units or GRUs， which are going to be a bit simpler version。

 but the same concept of LSDMs， as well as other subjects in regards to recurrent neural networks。

 which you'll see as we introduce the learning objectives right now。

So our learning goals for this section。We're going to cover LSDMs and how they can help to solve that long term memory problem that we discuss with their current neural nets。



![](img/09af771104b45fdfb955b6b32603238d_1.png)

![](img/09af771104b45fdfb955b6b32603238d_2.png)

We'll discuss gated recurrent units or GRUs， which are another solution to that long term memory problem。

 That's not quite as sophisticated as LSDMs， but it's going to be more efficient to train and often have similar performance。



![](img/09af771104b45fdfb955b6b32603238d_4.png)

We'll discuss sequence to sequence models or trying to predict another sequence given a certain sequence。

 which will be powerful for language translation and helping us to understand how perhaps words are pieced together or sequences are pieced together that may be different lengths but related to one another。



![](img/09af771104b45fdfb955b6b32603238d_6.png)

And then finally， we're going to cover some common enterprise applications of LSDM models that you may look to use yourself within the workplace。



![](img/09af771104b45fdfb955b6b32603238d_8.png)

![](img/09af771104b45fdfb955b6b32603238d_9.png)

Now， we discussed how the matrices that we're using for current neural nets tend to weaken the signal of those earlier inputs as we start to get further down that sequence。



![](img/09af771104b45fdfb955b6b32603238d_11.png)

And what we're ultimately going to need is some structure that allows us to keep some portions unchanged over many steps to maintain information from earlier in the sequence。



![](img/09af771104b45fdfb955b6b32603238d_13.png)

And this is going to be the problem that our long， short term memory recurrent neural nets will go about addressing。



![](img/09af771104b45fdfb955b6b32603238d_15.png)

Now， the way that LSTM accomplishes this is that it makes remembering easy with a bit more of a complicated update mechanism for defining our current neural net's current state。



![](img/09af771104b45fdfb955b6b32603238d_17.png)

Now by default， we already know that LSTM should remember information from that last step。

 the same as with recurrent neural nets。

![](img/09af771104b45fdfb955b6b32603238d_19.png)

But on top of that， rather than keeping or adjusting past information。

 we have more flexibility in retaining or forgetting a large portion from those prior steps。



![](img/09af771104b45fdfb955b6b32603238d_21.png)

Besides just that last step。

![](img/09af771104b45fdfb955b6b32603238d_23.png)

Now， LSDMs are just going to be a special kind of a current neural network。

 and they were invented back in 1997。

![](img/09af771104b45fdfb955b6b32603238d_25.png)

It's still called state of the art， though， because although the concept is a bit older。

 the computing power that allows it to actually make it applicable is going to be new。



![](img/09af771104b45fdfb955b6b32603238d_27.png)

![](img/09af771104b45fdfb955b6b32603238d_28.png)

Now the idea behind it is that it's going to add in an explicit memory unit。



![](img/09af771104b45fdfb955b6b32603238d_30.png)

And the key to that unit is that it adds on a few additional gate units。

 which you can think of as gates that allow for information to be passed along and how long we will continue to allow that to stay in memory。



![](img/09af771104b45fdfb955b6b32603238d_32.png)

![](img/09af771104b45fdfb955b6b32603238d_33.png)

![](img/09af771104b45fdfb955b6b32603238d_34.png)

With that in mind。The cell will have an input gate。

 which given a certain value will tell the system to store that value in memory。



![](img/09af771104b45fdfb955b6b32603238d_36.png)

![](img/09af771104b45fdfb955b6b32603238d_37.png)

We'll have quote unquote， a forget gate， which， again。

 given a certain value will determine whether that information will be removed from memory。



![](img/09af771104b45fdfb955b6b32603238d_39.png)

![](img/09af771104b45fdfb955b6b32603238d_40.png)

And then finally， we're going to have our output gate。

 which is going to fire off the response to move the current hit unit forward within our network。



![](img/09af771104b45fdfb955b6b32603238d_42.png)