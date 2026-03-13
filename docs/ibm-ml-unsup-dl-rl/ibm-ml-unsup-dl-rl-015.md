# 015：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p15 14_维数灾难笔记本第2部分.zh_en -BV1eu4m1F7oz_p15-

Building off of what we just discussed in two dimensions。 So we had our square and our circle。

 And we saw that 21 per cent of our square was outside of the circle。



![](img/b80f59e4271847e53f2572a06664fff0_1.png)

We are now going to push that out into three dimensions and work with a sphere rather than a circle and a cube rather than a square。



![](img/b80f59e4271847e53f2572a06664fff0_3.png)

Now， I want to remind you how this ties back to a data science problem。

 The idea here thinking about the two dimensions is that we have for each dimension。

 that is a different feature。 So a different covariate。 So we have covariate A， Covariate B。

 Both have been normalized。 And we can think that the values lie between negative one and one for each one of those。

 and they've been standardized。 So they have a mean of 0 and a standard deviation of one。



![](img/b80f59e4271847e53f2572a06664fff0_5.png)

![](img/b80f59e4271847e53f2572a06664fff0_6.png)

![](img/b80f59e4271847e53f2572a06664fff0_7.png)

![](img/b80f59e4271847e53f2572a06664fff0_8.png)

![](img/b80f59e4271847e53f2572a06664fff0_9.png)

![](img/b80f59e4271847e53f2572a06664fff0_10.png)

If we think about this value and look at the square here。



![](img/b80f59e4271847e53f2572a06664fff0_12.png)

The idea is that to be a single unit away from that center value。



![](img/b80f59e4271847e53f2572a06664fff0_14.png)

That would indicate that you're one standard deviation away， whether you're pointing horizontally。

 vertically or diagonally。 And we can see all the different values that lie one standard deviation from the mean。

 That's your unit circle。 And then all values that are outside of that circle are going to relate to those that are far away from the mean above one standard deviation from the mean。

 But still within that negative one to one range。 So we see。

 given that we're working with values between and negative one and one for our covariates。



![](img/b80f59e4271847e53f2572a06664fff0_16.png)

![](img/b80f59e4271847e53f2572a06664fff0_17.png)

![](img/b80f59e4271847e53f2572a06664fff0_18.png)

![](img/b80f59e4271847e53f2572a06664fff0_19.png)

![](img/b80f59e4271847e53f2572a06664fff0_20.png)

These are the values outside the circle that will be， in a sense， outliers。



![](img/b80f59e4271847e53f2572a06664fff0_22.png)

Now， in that same sense， if we were to add on a third dimension Covariate C。

 And that's what we're planning to do here。 The idea is。

 we're still working from negative one to one。 Still。

 our sphere now will indicate one standard deviation away in any direction， now。

 not just diagonally but diagonally within space。

![](img/b80f59e4271847e53f2572a06664fff0_24.png)

![](img/b80f59e4271847e53f2572a06664fff0_25.png)

![](img/b80f59e4271847e53f2572a06664fff0_26.png)

![](img/b80f59e4271847e53f2572a06664fff0_27.png)

And then anything outside that sphere， we can then again think of in the same sense that we just did with the circle and the square that this is more than one standard deviation away still between negative one and one and see how many outliers we have。



![](img/b80f59e4271847e53f2572a06664fff0_29.png)

![](img/b80f59e4271847e53f2572a06664fff0_30.png)

So again， with the square we had 21% now we're moving to plotting in three dimensions。



![](img/b80f59e4271847e53f2572a06664fff0_32.png)

I'm going to show you step by step how you can plot some of these values in three dimensions so that you can go home or leave this notebook and then be able to plot in three dimensions yourself as well。



![](img/b80f59e4271847e53f2572a06664fff0_34.png)

![](img/b80f59e4271847e53f2572a06664fff0_35.png)

So the first thing that we do is we are going to have to import this axes 3D library Now。

 if we don't do this。And we try to createate our figure。



![](img/b80f59e4271847e53f2572a06664fff0_37.png)

And then from that figure， we get our current axes and make them 3D projections。



![](img/b80f59e4271847e53f2572a06664fff0_39.png)

We'll see that we get in error。

![](img/b80f59e4271847e53f2572a06664fff0_41.png)

We will have to first import that library。That axes 3D to give us that option。



![](img/b80f59e4271847e53f2572a06664fff0_43.png)

So。We pull this in。

![](img/b80f59e4271847e53f2572a06664fff0_45.png)

And I'm going to run through just as we did before。

 have to cell above so we can see this step by step。 But now that I've imported axes 3D。

 we see that now， rather than working in two dimensions。

 you can see how we can start to work within three dimensions。



![](img/b80f59e4271847e53f2572a06664fff0_47.png)

![](img/b80f59e4271847e53f2572a06664fff0_48.png)

![](img/b80f59e4271847e53f2572a06664fff0_49.png)

So hopefully this is exciting to see that we have values for X， Y axes and then now a z axis as well。



![](img/b80f59e4271847e53f2572a06664fff0_51.png)

![](img/b80f59e4271847e53f2572a06664fff0_52.png)

We're then going to draw our cube。 Now we have here this idea of combinations and taking the products。

 I don't want to walk too much into it。 I'll show you quickly how the product works。

 and I would suggest you can look at the combinations and see how it works as well built off of this product that I'm about to create。



![](img/b80f59e4271847e53f2572a06664fff0_54.png)

![](img/b80f59e4271847e53f2572a06664fff0_55.png)

![](img/b80f59e4271847e53f2572a06664fff0_56.png)

But right now， we're taking the product of three R's and the R is just defined as negative 1 and one。



![](img/b80f59e4271847e53f2572a06664fff0_58.png)

In order to make this a little clear。

![](img/b80f59e4271847e53f2572a06664fff0_60.png)

We're going to use three different lists of two， rather than negative one and one， though。

 we're going to use one and2。3，4， and。5，6。 And when I take the product。

 I'll take the list so that we can see this output。 otherwise it's just a generator object。



![](img/b80f59e4271847e53f2572a06664fff0_62.png)

We also have to make sure that we import that library。



![](img/b80f59e4271847e53f2572a06664fff0_64.png)

You see that it comes up with every possible combination， not accounting for ordering。 So1，3 and 5。

 taking the first value from each of the lists， then1，3 and 6。 So first， first and second。

 and then 1，4，5，1，4，6。

![](img/b80f59e4271847e53f2572a06664fff0_66.png)

![](img/b80f59e4271847e53f2572a06664fff0_67.png)

2，3，5 so you can see how it's going through each one of these different values ensuring that it covers all the different possible combinations。



![](img/b80f59e4271847e53f2572a06664fff0_69.png)

![](img/b80f59e4271847e53f2572a06664fff0_70.png)

So it does that with negative 1，1。 And then the combinations of value of 2 will give you values of two for each one of different combinations。

 and I wouldn't worry too much about it。 That point here is given again that。



![](img/b80f59e4271847e53f2572a06664fff0_72.png)

![](img/b80f59e4271847e53f2572a06664fff0_73.png)

![](img/b80f59e4271847e53f2572a06664fff0_74.png)

We're pulling out an S and an E， it's going to output two different values when we get that combination。



![](img/b80f59e4271847e53f2572a06664fff0_76.png)

We're going to take the sum of S minus E， and that has to be equal to。This is using our R1。

 R1 minus r 0 in order for it to be an edge on our Q。So that's all it's trying to do。

 is's trying to find where each of our edges lie。

![](img/b80f59e4271847e53f2572a06664fff0_78.png)

Now I'm going to pull out this portion of code just to show you how one line is drawn in three dimensional space。



![](img/b80f59e4271847e53f2572a06664fff0_80.png)

![](img/b80f59e4271847e53f2572a06664fff0_81.png)

Let me。All this。

![](img/b80f59e4271847e53f2572a06664fff0_83.png)

We copy this， we're going to move it above。And we're saying for S and E。

 we don't care too much about that。 But what we do care about is this zip of S and E and then plotting that。

 So in order to see what that， well， this the star is going to ensure that it unpacks it。

 So rather than just creating generator object， we'll see that actual output。

 And I'll actually print here so we can see what。

![](img/b80f59e4271847e53f2572a06664fff0_85.png)

![](img/b80f59e4271847e53f2572a06664fff0_86.png)

![](img/b80f59e4271847e53f2572a06664fff0_87.png)

This output looks like。So。Zipping S and E。And then I'm going to break。

 so we're just going to plot one line。

![](img/b80f59e4271847e53f2572a06664fff0_89.png)

So I'm going to run this。R is not defined yet， if you forgot to copy that in。

 say r equals negative 11。

![](img/b80f59e4271847e53f2572a06664fff0_91.png)

![](img/b80f59e4271847e53f2572a06664fff0_92.png)

And we see that we plotted this one line。Now the zip S。

This is going to be our x values of our two points， the y values of our two points。

 and the z values of our two points。

![](img/b80f59e4271847e53f2572a06664fff0_94.png)

So we're plot from negative one， negative  one， negative  one， up to negative  one， negative  one，1。

 so that's the idea that we're seeing here。And it's hard to see in three dimensional space。

 but we are going from negative one， negative1 negative one up to 111。



![](img/b80f59e4271847e53f2572a06664fff0_96.png)

Now， when we run through all the different lines， all we're doing is using this plot 3D。

 which will work exactly the same as just plot in two dimensional space。

 And that is just creating those lines connecting those two dots the same way you would do in two dimensional space。

 calling ax dot plot。

![](img/b80f59e4271847e53f2572a06664fff0_98.png)

![](img/b80f59e4271847e53f2572a06664fff0_99.png)

![](img/b80f59e4271847e53f2572a06664fff0_100.png)

![](img/b80f59e4271847e53f2572a06664fff0_101.png)

So if I don't run the brake here and do this for let the for loop run all the way through。

 you see here that we now have our cube connecting each one of these points that we have here。



![](img/b80f59e4271847e53f2572a06664fff0_103.png)

![](img/b80f59e4271847e53f2572a06664fff0_104.png)

The next step that we want to do in order to draw our sphere is we're first going to create this mesh grid。

So I'm going to copy this above into a different cell。And in order to make this a little clear。

 this is going to be the number of points。

![](img/b80f59e4271847e53f2572a06664fff0_106.png)

If you were to do without the J， the J just in general， so you know。

 within Python means a complex number。

![](img/b80f59e4271847e53f2572a06664fff0_108.png)

We are working here with the J， not because we're working with complex numbers。

 but the complex numbers just let us know that rather than counting by 20。

 we want 20 points in between 0 and two times pi。

![](img/b80f59e4271847e53f2572a06664fff0_110.png)

![](img/b80f59e4271847e53f2572a06664fff0_111.png)

That's all we're doing here by using the complex number。But we're going to reduce this。

 just for our example， to3 and 2， so F。

![](img/b80f59e4271847e53f2572a06664fff0_113.png)

Three values and two values。 And the idea is that when we want to plot along many different points and we want to cover So here it's supposed to go from0 to two times pi。

 and we want to have three different values， so it'll go0， then pi then two times pi。



![](img/b80f59e4271847e53f2572a06664fff0_115.png)

And then we're also going from 0 to pi with just two values。 So 0 to pi。

 And the idea is that we want to plot all the possible combinations of these points。

 And in order to do that， we have to create this mesh grid so that we have 0，0， as well as 0 and pi。

 and then。

![](img/b80f59e4271847e53f2572a06664fff0_117.png)

![](img/b80f59e4271847e53f2572a06664fff0_118.png)

Pi coming from our count from 0。Through22 pi， we then have pi and 0。

 and then pi and pi for our second axis and so on and so forth。

 So that's the idea of the mesh grid to allow you to plot on each one of these multiple points。 Now。

 it has two outputs。

![](img/b80f59e4271847e53f2572a06664fff0_120.png)

![](img/b80f59e4271847e53f2572a06664fff0_121.png)

![](img/b80f59e4271847e53f2572a06664fff0_122.png)

![](img/b80f59e4271847e53f2572a06664fff0_123.png)

For each one of the different grids， those are both equal in shape。So you have 0 and 1。

 That's supposed to be your X and Y here， we're plotting in three dimensions。

 And all we're doing is taking that two dimensional graph。



![](img/b80f59e4271847e53f2572a06664fff0_125.png)

![](img/b80f59e4271847e53f2572a06664fff0_126.png)

And we're expanding that to create our sphere by using each one of those different points and taking the cosine of each of these values ranging from。



![](img/b80f59e4271847e53f2572a06664fff0_128.png)

![](img/b80f59e4271847e53f2572a06664fff0_129.png)

From U， you go from0 to2 pi， and then from V， from 0 to pi。



![](img/b80f59e4271847e53f2572a06664fff0_131.png)

Multiplying them together。 And then first Z， we just get cosine of V。

 and that will createate our sphere。So we're going to have our three points。

 all these multiple points。 And right now， they're just points out in space。

And in order to connect all those spaces into one final sphere。

 we're going to use this plot wire frame， which will connect all those dots together。😊。



![](img/b80f59e4271847e53f2572a06664fff0_133.png)

![](img/b80f59e4271847e53f2572a06664fff0_134.png)

So we call x dot plot wireframe on the X， Y， and z。



![](img/b80f59e4271847e53f2572a06664fff0_136.png)

And we run this。And then we see all these different points that were created in three dimensional space。

 all being connected by this wire frame。

![](img/b80f59e4271847e53f2572a06664fff0_138.png)

And ultimately， we saw here how to plot in 3D and may be difficult to visualize how much extra empty space there is。



![](img/b80f59e4271847e53f2572a06664fff0_140.png)

![](img/b80f59e4271847e53f2572a06664fff0_141.png)

But if we think about it in terms of the equations。

 the volume of this sphere is given by 4 over 3 pi R cubed。

 And since we're working with a cube with a radius of2 R。

 it's going to have two R cubed in terms of volume。And when we calculate the percent of that cube。

 again， thinking， thinking of this as three different covariates。



![](img/b80f59e4271847e53f2572a06664fff0_143.png)

We can see that the volume outside the sphere is going to be one minus that volume of the sphere。



![](img/b80f59e4271847e53f2572a06664fff0_145.png)

![](img/b80f59e4271847e53f2572a06664fff0_146.png)

Four over three times pi r cubed， over two are cubed。You do some cross multiplication。

 You end up with 1 minus pi over 6， and approximately 48 per cent of your values being outside the cube。

 So working with that same range of negative one to one。



![](img/b80f59e4271847e53f2572a06664fff0_148.png)

![](img/b80f59e4271847e53f2572a06664fff0_149.png)

![](img/b80f59e4271847e53f2572a06664fff0_150.png)

And that same radius being described as your standard deviation and being beyond that being a bit of an outlier。

 we see that 48% cent of our values are now outliers。

 Now that we've moved up to three dimensional space。



![](img/b80f59e4271847e53f2572a06664fff0_152.png)

![](img/b80f59e4271847e53f2572a06664fff0_153.png)

So that closes out this video in the next video we will continue and show you how you can actually generalize this to even higher dimensional space and see those percentages as we continuously increase the number of dimensions。



![](img/b80f59e4271847e53f2572a06664fff0_155.png)

![](img/b80f59e4271847e53f2572a06664fff0_156.png)

![](img/b80f59e4271847e53f2572a06664fff0_157.png)