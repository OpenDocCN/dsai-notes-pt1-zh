# 109：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p109 70_GAN的工作原理.zh_en -BV1eu4m1F7oz_p109-

In this video， we're actually going to take a look at the generative adversarial network training procedures。

So diving right in。The first step will be as usual with our neural networks to start with randomly initialized weights for our generator and discriminator networks。



![](img/18038320176b6285b3d6e4006151f6de_1.png)

Now to start our generator process， which is meant to find some distribution that does well in representing the actual values in our training set。

 be those images or whatever it may be。

![](img/18038320176b6285b3d6e4006151f6de_3.png)

![](img/18038320176b6285b3d6e4006151f6de_4.png)

Will be to pass through just a random noise vector。



![](img/18038320176b6285b3d6e4006151f6de_6.png)

So our input's going to be a random noise vector， and the goal is that our generator network will be able to take this random noise and come up with the appropriate weights to generate the complex distributions of our images of whatever our training set is。



![](img/18038320176b6285b3d6e4006151f6de_8.png)

![](img/18038320176b6285b3d6e4006151f6de_9.png)

![](img/18038320176b6285b3d6e4006151f6de_10.png)

And then that generated image should then be the same size as the actual training set images as it will later be fed through our discriminator network。



![](img/18038320176b6285b3d6e4006151f6de_12.png)

![](img/18038320176b6285b3d6e4006151f6de_13.png)

The next step， step three。Will be for the discriminator to predict the probability that the generated image is a real image or one that our network has just created。



![](img/18038320176b6285b3d6e4006151f6de_15.png)

![](img/18038320176b6285b3d6e4006151f6de_16.png)

So once again， that is going to pass through our discriminator network， our produce image。

 and the goal is to discriminate between actual training images and those our generator network is producing。



![](img/18038320176b6285b3d6e4006151f6de_18.png)

![](img/18038320176b6285b3d6e4006151f6de_19.png)

![](img/18038320176b6285b3d6e4006151f6de_20.png)

And the output will be the probability that we are working with an actual image and not a generated one。



![](img/18038320176b6285b3d6e4006151f6de_22.png)

![](img/18038320176b6285b3d6e4006151f6de_23.png)

Step 4。

![](img/18038320176b6285b3d6e4006151f6de_25.png)

We are then going to want to compute the losses， both assuming that the image we just generated is fake。



![](img/18038320176b6285b3d6e4006151f6de_27.png)

And a loss function assuming that the image generated was real。

 and we'll get into this in just a second。

![](img/18038320176b6285b3d6e4006151f6de_29.png)

We're going to call the loss function assuming it was fake， which it is， of course， just a reminder。

 we are working here in the realm of that generated image。



![](img/18038320176b6285b3d6e4006151f6de_31.png)

![](img/18038320176b6285b3d6e4006151f6de_32.png)

So we're going to call the loss function assuming it was fake L0。

 as in how far away was our discriminator from predicting there's a zero probability that this generated image is not real。



![](img/18038320176b6285b3d6e4006151f6de_34.png)

![](img/18038320176b6285b3d6e4006151f6de_35.png)

And then we're going to call the loss function assuming it was real， which again， it's not。

 we're going to call that loss function L1。

![](img/18038320176b6285b3d6e4006151f6de_37.png)

Representing how far discriminator was from predicting this was 100% a real image。



![](img/18038320176b6285b3d6e4006151f6de_39.png)

And then these two combined will produce our total loss function。

 which we can later leverage to train both our generator and discriminator networks。



![](img/18038320176b6285b3d6e4006151f6de_41.png)

![](img/18038320176b6285b3d6e4006151f6de_42.png)

Now， here in step five， we'll make a little bit clear why we have these two different loss functions。



![](img/18038320176b6285b3d6e4006151f6de_44.png)

So our discriminator network is going to only care about correctly predicting that this image is not a real image。



![](img/18038320176b6285b3d6e4006151f6de_46.png)

So I'll want to use that loss function L0。One， we're talking about the discriminator network。

So how far off was the output from predicting zero is what we're trying to get here。

 so we back propagate in relation to the loss。Of。

![](img/18038320176b6285b3d6e4006151f6de_48.png)

How far off we were to saying that this is not a real image。

And then we update the weights of only our discriminator network accordingly。



![](img/18038320176b6285b3d6e4006151f6de_50.png)

Now， where does our L1 penalty come into play？Our L1 penalty。Again。

 the L1 being assuming that our image produced was actually real。

That's going to be ignored by our discriminator network because our discriminator network doesn't want to optimize assuming that a fake image is real。



![](img/18038320176b6285b3d6e4006151f6de_52.png)

![](img/18038320176b6285b3d6e4006151f6de_53.png)

The goal of computing L1 is to ultimately tell us if we're doing a good job of producing an image that seems realistic。



![](img/18038320176b6285b3d6e4006151f6de_55.png)

![](img/18038320176b6285b3d6e4006151f6de_56.png)

And thus， our output from our discriminator is too far from one。



![](img/18038320176b6285b3d6e4006151f6de_58.png)

So we compute our gradients in regards to L1 and backpo through without training the discriminator and pass that out ultimately to our generator。



![](img/18038320176b6285b3d6e4006151f6de_60.png)

![](img/18038320176b6285b3d6e4006151f6de_61.png)

So rather than using that gradient to train our discriminator network。

We use it to actually update our weights for our generator network。



![](img/18038320176b6285b3d6e4006151f6de_63.png)

So we continue to back propagate using that L1 gradient to update the weights of our generator。



![](img/18038320176b6285b3d6e4006151f6de_65.png)

Now what's missing in this training procedure that we just walked through？



![](img/18038320176b6285b3d6e4006151f6de_67.png)

The goal of our discriminator network is to learn to classify fake images as fake。

 real images as real。

![](img/18038320176b6285b3d6e4006151f6de_69.png)

![](img/18038320176b6285b3d6e4006151f6de_70.png)

And for it to actually improve its ability to distinguish between fake and real images。



![](img/18038320176b6285b3d6e4006151f6de_72.png)

We're also going to have to give it images from the actual training set。



![](img/18038320176b6285b3d6e4006151f6de_74.png)

So our next step。

![](img/18038320176b6285b3d6e4006151f6de_76.png)

Is for the real images within our training set。To be passed through。

And we want to calculate the probability that。The image passed through from our actual training set is real。



![](img/18038320176b6285b3d6e4006151f6de_78.png)

So we passed that real image XR before we were working with XG that X generated。

 and want to know the probability that this is actually a real image。

 and that's going to be the goal of the discriminator now。



![](img/18038320176b6285b3d6e4006151f6de_80.png)

![](img/18038320176b6285b3d6e4006151f6de_81.png)

We then compute a new L1 loss for our real image。So again， when trying to predict real。

We're actually going to want our discriminator to output one。

 so we're actually going to use this also to update our discriminator function。



![](img/18038320176b6285b3d6e4006151f6de_83.png)

And the L1 in regards to Xr is going to be how far off our probability was。



![](img/18038320176b6285b3d6e4006151f6de_85.png)

What should be as close to one as possible from that value of one？



![](img/18038320176b6285b3d6e4006151f6de_87.png)

And we can use that L1 loss。To train and update our weights appropriately within that discriminator network。



![](img/18038320176b6285b3d6e4006151f6de_89.png)

![](img/18038320176b6285b3d6e4006151f6de_90.png)

And we repeat this procedure with new random noise from the generator each time。



![](img/18038320176b6285b3d6e4006151f6de_92.png)

And continue until images from the generator begin to look real。



![](img/18038320176b6285b3d6e4006151f6de_94.png)

Now， we must note that values of losses from discriminator and generator may still be fluctuating when the generator is producing realistic images。

 so we can't use these alone to determine when to stop training。



![](img/18038320176b6285b3d6e4006151f6de_96.png)

![](img/18038320176b6285b3d6e4006151f6de_97.png)

![](img/18038320176b6285b3d6e4006151f6de_98.png)

And there are ways of quantifying image quality within our generative process such as the inception score to help us find where we should actually stop training。

 that's a bit beyond the scope of the lesson， but I encourage you looking into it if you want to delve a bit deeper into working with GANS。



![](img/18038320176b6285b3d6e4006151f6de_100.png)

![](img/18038320176b6285b3d6e4006151f6de_101.png)

![](img/18038320176b6285b3d6e4006151f6de_102.png)

![](img/18038320176b6285b3d6e4006151f6de_103.png)