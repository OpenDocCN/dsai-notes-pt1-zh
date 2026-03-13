# 103：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p103 64_自编码器笔记本（选修部分）第3部分.zh_en -BV1eu4m1F7oz_p103-

Welcome back to our auto encoder notebook。 As we saw in the last video。

 we built out our first auto encoder with the encoder and decoder portions of our model。

 And then with the reconstruction， we saw a fairly high air。😊。

And one of the reasons that we had this。Poor model was that we weren't really doing any deep learning。

 So we're going to do here is first， we're going to start off by adding on some extra layers。

We're also going to run it for a few more epochs and see if we can get a lower reconstruction area than we did with PCA and see how much we're able to actually improve on that air。



![](img/789f7d287ba1673f50ac73d01a208020_1.png)

So again， our final encoding dimension will be 64， but this time we're going to be including a hidden dimension of 256。



![](img/789f7d287ba1673f50ac73d01a208020_3.png)

![](img/789f7d287ba1673f50ac73d01a208020_4.png)

And this even't work very similarly to what we just did。

 so this is another time to look through your practice through the functional API for CARS。

We're starting off again with the input and we just say the shape and that's going to just initiate what type of tensor we want to pass through。

And then we're going to have first， that hidden dimension。Which is just going to be a dense。

 So a fully connected layer。 And we're going to pass into that fully connected layer with that hidden dimension of 256。

Our inputs。 So we're just passing our inputs that will produce some output。

 We can pass that output through to another dense layer。 So this is the hidden layer。

 Then this is the encoded layer。 And this will take in that encoder hidden within the function。

And this is the function itself， it's going to be another dense layer now reducing it down to 64 nodes。



![](img/789f7d287ba1673f50ac73d01a208020_6.png)

And then to just create the model， we just say model and we're going from inputs。

All the way out to the encoded outputs。And it doesn't you don't have to write out this middle step as long as there's a connection between what's being input all the way through to the output within your model all you're passing through is what the input is and what the final output is。



![](img/789f7d287ba1673f50ac73d01a208020_8.png)

And then we do our decoder model and again， our decoder model should start off with that encoding dimension of 64。

 as it will be decoding now that latent space that has 64 dimensions。

We're going to have another hm dimension， so it'll take that next step up to 256 nodes and then finally have the 784 nodes。

And we can create our model that takes in the encoded inputs。

And passes out this reconstruction that we have here。



![](img/789f7d287ba1673f50ac73d01a208020_10.png)

And that's going to be our decoder model that just as before we create our outputs by passing in the encoder model with the inputs as its input。

Into the decoder model。That will be that final output to allow us to go all the way from the inputs to that final output that we have here。



![](img/789f7d287ba1673f50ac73d01a208020_12.png)

And then the full model， we can just say the inputs。

 which is these inputs all the way out to these outputs that we just defined。



![](img/789f7d287ba1673f50ac73d01a208020_14.png)

We can look at that full model summary， we looked at the summary before now it doesn't show it's just showing the different models。

 but now you see there's a lot more parameters that it's learning rather than just what we had here at 50。

000 werere up to 217，000 because we have those that 256 node hidden layer in between the inputs and our encoding dimension。



![](img/789f7d287ba1673f50ac73d01a208020_16.png)

![](img/789f7d287ba1673f50ac73d01a208020_17.png)

![](img/789f7d287ba1673f50ac73d01a208020_18.png)

![](img/789f7d287ba1673f50ac73d01a208020_19.png)

So again， we have our full model just going from inputs that we defined to the outputs we defined。

 we can compile it using the optimizer， the loss function that we care about。

 which metrics we want to track。

![](img/789f7d287ba1673f50ac73d01a208020_21.png)

![](img/789f7d287ba1673f50ac73d01a208020_22.png)

And this time， we're going to run it for5 epochs。 So we have the batch size equal to 32。

 So itll run the gradient every 32 samples that we go through。

 And we're going again from X train flat as the input to X train flat as that output。

 Those two will be the same。 And in the middle， itll be coming up with that encoding layer。



![](img/789f7d287ba1673f50ac73d01a208020_24.png)

So you run this and this can be for five epochs and it may take a bit of time。

 so I'm going to pause the video and we'll come back as soon as this is done running again。



![](img/789f7d287ba1673f50ac73d01a208020_26.png)

![](img/789f7d287ba1673f50ac73d01a208020_27.png)

So now our model has been fit to， again， our X train flat and our X train flat。

 both as input and output。 We can then call full model dot predicts on our test set。 And again。

 that's just going to try to。Deconstruct and reconstruct or bring it down to that latent say space and then reconstruct that original image。

So'll run that and then again， get that mean squared error of that reconstruction。

 passing in that new decoded image， as well as the original excess flat。 And when we run that。

 we see that we get a score of 84。3。 So it did better than PCA Now。

 Now that we've done5 epochs and done this deeper network。



![](img/789f7d287ba1673f50ac73d01a208020_29.png)

![](img/789f7d287ba1673f50ac73d01a208020_30.png)

Now， let's see if we can improve even further。 We noticed that so far we're only using so many epochs。

 I want to see if maybe we introduce further training as we see the。

Loss continued to go down and it hadn't plateaued yet， and the accuracy continued to go up。

 see if we can actually get a better performance as we increase the number of epochs。



![](img/789f7d287ba1673f50ac73d01a208020_32.png)

![](img/789f7d287ba1673f50ac73d01a208020_33.png)

So this function that we have here。

![](img/789f7d287ba1673f50ac73d01a208020_35.png)

We'll actually just be putting together all the steps that we have here above。



![](img/789f7d287ba1673f50ac73d01a208020_37.png)

So this you can imagine was just copied and pasted。

 so we're not going to go through that again into this。



![](img/789f7d287ba1673f50ac73d01a208020_39.png)

Function， and then we just say the number of epochs that we want to run through。

 And this is the part that's different。 wherefore I enraged the number of epochs。

 we keep fitting the model to the training set and recall that if we're not reinitiating the model。

 then what we are doing when we say only one epoch here。



![](img/789f7d287ba1673f50ac73d01a208020_41.png)

For the entire range of epochs， it's going to pick up the training where the last one left off。

 so we'll see the results for just one epoC， for two epochs for 3B epochs and so on。And with that。

 every single time， we'll get the decoded images， get our reconstruction loss。

We will append that to this list that we have initiated up here。



![](img/789f7d287ba1673f50ac73d01a208020_43.png)

And we will say the reconstruction loss after each number of epochs is so on and so forth to see whether or not we continue to improve。

So we initiate that function， and then we're going to run this now for 10 epochs。

 right numberumber of epochs is the only argument we have available。



![](img/789f7d287ba1673f50ac73d01a208020_45.png)

We run this for 10 epochs。And this will take just a bit of time to run。

 We had the five epochs take X amount of time。 This will take about double the amount。

 So we'll come back as soon as that's done running。



![](img/789f7d287ba1673f50ac73d01a208020_47.png)

So now we see the results。After 10 epochs， we are able to， for the most part。

 continue to reduce that reconstruction error。 That means squared error。

 We do see that it is not monotonically decreasing at some points， it seems to waver。

 but you see towards the end of 10 epochs， we get it down to the mid 60s compared to that higher number that we had earlier。

 So those extra epochs， even though if we look at the accuracy score。

 it may not be improving that much。We do see that it continued to decrease that mean squared error of that reconstruction score。

So that closes out our video here in regards to just working with the auto encoders in the next video we'll introduce variational auto encoders and how we can leverage those Allright。

 I'll see you there。

![](img/789f7d287ba1673f50ac73d01a208020_49.png)

![](img/789f7d287ba1673f50ac73d01a208020_50.png)