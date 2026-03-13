# 102：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p102 63_自编码器笔记本（选修部分）第2部分.zh_en -BV1eu4m1F7oz_p102-

Welcome back to our lab here on auto Enrs。 If you recall in the last video。

 we used PCA in order to reduce the number of dimensions down to 64 and then reconstructed our image and then saw how far off our reconstructed image was from our original image。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_1.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_2.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_3.png)

Now we're going to do the same thing except using auto encoders and using neural nets。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_5.png)

Now so far， whenever we built out our neural nets， we've been using CAs and from CARs。

 we've been using this sequential API where we just add on layers。

 and that's a bit of a simpler way of building out your neural nets。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_7.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_8.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_9.png)

What we're going to introduce here is the functional API。

And that's going to be fairly simple as well and we'll walk through the steps here in regards to actually building out these complex architectures。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_11.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_12.png)

And what itll allow for is for more flexibility in building out your models。

 so if you if you think back to our convolutional neural net discussion and we didn't have a notebook there。

 but if you want to build some of those more complex architectures such as inception or Resnet you would have to actually use this functional API in order to build out layers such as with inception where youre concatenating a bunch of different types of layers together or resnet where you want to bring along portions of the layer to further layers。

 you'll have to use something like the functional API。

 so it is worth getting a hang of as we talk through it here。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_14.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_15.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_16.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_17.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_18.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_19.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_20.png)

So we're going to import from Tensorflowlow。carara's this input。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_22.png)

And this dense layer， and we'll see how those are used in just a second， as well as importing model。

 which will be the key function in regards to the functional API within CAS。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_24.png)

So the goal that we have when we build out our auto encodecor will be to build out three different models。

 we'll have that full auto encoder。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_26.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_27.png)

And that's taking that inputs， remember the inputs and the outputs would be the same。

 but taking that image and then ultimately reconstructing that image at the back end。

 so I'll be the encoder and decoder。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_29.png)

Where we deconstruct and reconstruct these images。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_31.png)

We'll have the encoder portion， which is just the portion that will take inputs。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_33.png)

And try to bring them into that latent space and then the decoder。

 which will take that latent space and try to reconstruct it。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_35.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_36.png)

So we're setting our encoding dimension to 64 as we did with PCA。

 so we're going to have that latent space in 64 dimensions。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_38.png)

We have to define when we use the functional API。What our input is going to be。

 so this is just creating a blank tensor。when we say a tensor and that's where the word Tensor flow comes from。

 all we're talking about is an array in a certain amount of dimensions。

 so we're going to be using probably here two dimensions， one dimension is the number of samples。

 the other dimension which we wanted to define the shape of is going to be how many actual features we're going to have if we kept it at 28 by 28 then for shape you'd probably put in 28 by 28 and then you'd leave out the 60。

000 still。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_40.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_41.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_42.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_43.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_44.png)

But we're initiating this blank tensor， and we're going to need this in order to define what the flow of our inputs to outputs actually are within the functional API。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_46.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_47.png)

And then the reason why it's called a functional API。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_49.png)

Why we have the term function involved is they become clear right here。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_51.png)

We create this dense layer， and this is similar to the dense layer we saw before， or we call dense。

 we say what's going to be the dimension of that dense。

 how many hidden nodes are they going to be and what type of activation are we going to use？



![](img/9d71c9b9f39a29149c8df74b401d0ac5_53.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_54.png)

And this portion right here is actually a function。

And that function will be able to take a certain input。And all we have to do is say， as an input。

 we want these inputs that we defined here， that tensor。And then we only have a simple model here。

So we're going and this is just the encoder model， the encoder portion in order to bring together all the steps。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_56.png)

We call model。And we call the inputs and the outputs， and as long as the inputs and outputs match up。

 even if we added on a bunch more layers in between here， which we'll see later on。

 as long as the first value is able to reach that final value given our functional API and how we defined each one of the different steps。

 the model will be able to bring those all together。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_58.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_59.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_60.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_61.png)

So we have our encoder model， which is this model that starts with the inputs and ends with the outputs from this encoded portion。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_63.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_64.png)

We then are going to have our decoder model， which again。

 we're going to have our input defined and this time the input for the decoder model。

 if we think about the idea that is taking that latent space and reconstructing our image。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_66.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_67.png)

Should be the dimension of that latent space， so it's going to be taking in vectors with the of 64。

 that encoding dim that we have here。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_69.png)

We're then going to pass that through a dense layer。

 so we call that dense layer and we want that dense layer to be reconstructing our image back up to 784 dimensions。

 we use the activation of sigmoid and what we pass into that dense layer is in this parentheses as you would with a function。

 we pass in the encoded inputs。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_71.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_72.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_73.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_74.png)

And then our decoder model will just be， again， that model and then saying what the input is and what the output is。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_76.png)

And then to define the full model。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_78.png)

First， we're going to say what our outputs are going to be for that full model and if you recall our decoder model as currently constructed。

 just takes in this input， which is just a blank tensor。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_80.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_81.png)

Instead， we're going to define the input。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_83.png)

As the encoder model inputs。 So it's going to move from inputs， pass out the encoder model output。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_85.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_86.png)

So it's actually going to take that input that we define since we have that within the function。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_88.png)

The output of the encoder model will then become the input for our decoder model。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_90.png)

And then that decoder model will， by default， because of the way that we constructed the decoder model。

 output that final dense layer that reconstructs our image。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_92.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_93.png)

And then our full model will just be the inputs， which are defined up here that in those initial inputs。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_95.png)

And then those final outputs， which we just defined， which run from。

 we just created all the steps needed， runs from these inputs through the encodeder model。

 then through the decoder model， and then outputs this reconstruction。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_97.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_98.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_99.png)

So we run this， we now have our full model available to us。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_101.png)

We set our model。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_103.png)

Inputs equals inputs， output equals outputs， it's already what we did here。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_105.png)

And then the steps from there in regards to compiling and fitting the model are the same as with the sequential API。

 we call compile， we define our optimizer， our loss function。

 and then what metrics were want to track， we're going to track accuracy。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_107.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_108.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_109.png)

And then we're going to run this just for one epoch here on our X train flat and then recall that our output's also going to be x train flat。

 our input and output are going to be the same。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_111.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_112.png)

We fit the model。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_114.png)

Running through just one epoch， this will take just a second。 And afterwards， I'm also。

 we have the option here to actually just look at the summary。 So we'll do full。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_116.png)

Model that summary， which we'll look at。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_118.png)

Right after this is done。And three， two， one。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_120.png)

Look at the summary。And we can see that it passes through that input of dimension 784 down to dimensions of 64。

 that being our latent space and then reconstructs that image。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_122.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_123.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_124.png)

Now， the way that cars works。Is because we built out these smaller intermediate models。

 but actually trained them along the way。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_126.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_127.png)

That encoder model that we。Fit into this full model that we defined up here。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_129.png)

Has actually been fit to the data， so we can actually。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_131.png)

Encodeode our image and output that 64 dimensional space。 So we run this。

 and we see we took our X test flat， which is originally 100 by 784 and reduced down the dimensions down to 64。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_133.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_134.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_135.png)

And we will look at that and see that we now have those values which represent the different pixel values for or that 64 dimensional version of the encoded image。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_137.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_138.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_139.png)

And now now that we have this available。Our goal is to see what the reconstruction error actually is。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_141.png)

So we want to use the trained autoencoder to generate reconstructed images and then compute the pixel wise distance between that reconstructed image and the original image and see how he did compared to our baseline PCA。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_143.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_144.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_145.png)

So we have our full model， which will both encode and decode our model so we can just pass in X test flat into our full model。

 which has already been trained to predict what the。

 if you think about if we call full model dot predict on X test flat。

 it's going to try to bring that down to the latent space。

 so encode it and then deco it again into what should be exactly the same if it was able to do it perfectly。

 So all the decoded images will。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_147.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_148.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_149.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_150.png)

When don't we run full model。predict， both encode it and decode it。

 so it's doing that reconstructing step here。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_152.png)

And we get our decoded images， and then we can run that MSE reconstruction that we defined above to see what the actual error is on our decoded images compared to our original X test flat。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_154.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_155.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_156.png)

So you run this and we see that it is significantly worse。For recall。

 we had around 95 for the mean squared error。 And now we're at 346 about。

 We could ran for more epochs as well， rather than just one epoch to better fit the model。

 But even when with five epochs， you'll see that still does a bit worse。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_158.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_159.png)

And in the next video， we'll see how instead of just using one hidden later。

 perhaps we make our network a bit deeper。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_161.png)

And still run for a bit more epochs， and hopefully from there we start to actually do better and perform better than what we saw with the PCA model。



![](img/9d71c9b9f39a29149c8df74b401d0ac5_163.png)

![](img/9d71c9b9f39a29149c8df74b401d0ac5_164.png)

All right， I'll see you in the next video。

![](img/9d71c9b9f39a29149c8df74b401d0ac5_166.png)