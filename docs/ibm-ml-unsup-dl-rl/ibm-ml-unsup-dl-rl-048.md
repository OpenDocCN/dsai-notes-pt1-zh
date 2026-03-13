# 048：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p48 9_神经网络介绍笔记本（选修部分）第1部分.zh_en -BV1eu4m1F7oz_p48-

Welcome to our lab here on the Introuction to our neural networkss。



![](img/51ee3f7771c662356b048682f48dda00_1.png)

Here we're going to start off with an exercise based on different logic gates。

 such as working with the and functionality or or functionality。 And then as we'll see later on。

 working with X or in order to find if we can actually use a single perceptron。

 as we discussed in lecture in order to come up with these and or or functions。

 as well as these more complex ones such as X or。

![](img/51ee3f7771c662356b048682f48dda00_3.png)

![](img/51ee3f7771c662356b048682f48dda00_4.png)

![](img/51ee3f7771c662356b048682f48dda00_5.png)

![](img/51ee3f7771c662356b048682f48dda00_6.png)

![](img/51ee3f7771c662356b048682f48dda00_7.png)

So the first thing that we want to do is import our libraries so we call。



![](img/51ee3f7771c662356b048682f48dda00_9.png)

Important on pi S NPp， matpot lid do piPo as PLt， and then we're going to introduce the sigmoid function。

 which we discussed in lecture， where we're just going to set sigmoid equal to1 over1 plus E to the negative x。



![](img/51ee3f7771c662356b048682f48dda00_11.png)

![](img/51ee3f7771c662356b048682f48dda00_12.png)

![](img/51ee3f7771c662356b048682f48dda00_13.png)

Where we can pass in values of x。

![](img/51ee3f7771c662356b048682f48dda00_15.png)

And if that value of x is very high， we'll end up with a value very close to one。

And if that value is very low， we'll have a value low being negative。

 then we'll have a value close to zero。

![](img/51ee3f7771c662356b048682f48dda00_17.png)

And as we saw with the graph， it'll range between the values of 01。

 but can't go any lower than zero and can't go any higher than one。



![](img/51ee3f7771c662356b048682f48dda00_19.png)

So we're just going to define that sigmoid function。

 which is just going to be 1 over 1 plus e to the negative x。



![](img/51ee3f7771c662356b048682f48dda00_21.png)

![](img/51ee3f7771c662356b048682f48dda00_22.png)

We're then going to plot it out and all we're going to do is say that we want 100 values equally spaced between negative 10 and 10。



![](img/51ee3f7771c662356b048682f48dda00_24.png)

![](img/51ee3f7771c662356b048682f48dda00_25.png)

And then we're going to take the sigmoid of each one of those values。

 which we just defined above so that we get the activation。 and then we're just going to plot。



![](img/51ee3f7771c662356b048682f48dda00_27.png)

![](img/51ee3f7771c662356b048682f48dda00_28.png)

The different values on the X axis versus the activation on the y axis。

 and the rest is just drawing lines and creating a grid and ensuring that we have the right y limits so that we don't go too far negative are too far positive。



![](img/51ee3f7771c662356b048682f48dda00_30.png)

![](img/51ee3f7771c662356b048682f48dda00_31.png)

So we run that。And we see that as a value gets close to negative 10。

 we're essentially at  zero for our blue line and as our value our x axis gets close to 10。

 then our y axis gets very close to positive one。

![](img/51ee3f7771c662356b048682f48dda00_33.png)

![](img/51ee3f7771c662356b048682f48dda00_34.png)

![](img/51ee3f7771c662356b048682f48dda00_35.png)

So with this in mind and how the sigmoid function works。With the idea that as we move higher again。

 we get closer to one and as we go negative， we get closer to zero。



![](img/51ee3f7771c662356b048682f48dda00_37.png)

Just to highlight as well at the point of 0 itself， we're at around 0。5。

 as you see the grid line at 0。5 for that y axis， so that's going to be exactly 50。

50 chance of either being1 or0 if we were to create a threshold。



![](img/51ee3f7771c662356b048682f48dda00_39.png)

![](img/51ee3f7771c662356b048682f48dda00_40.png)

![](img/51ee3f7771c662356b048682f48dda00_41.png)

![](img/51ee3f7771c662356b048682f48dda00_42.png)

Now a logic gate is going to take in two billions。

![](img/51ee3f7771c662356b048682f48dda00_44.png)

So two different inputs。Either usually true or false。

 but we're going to signnify true as one and false as zero。



![](img/51ee3f7771c662356b048682f48dda00_46.png)

And then with those two inputs， it'll return either a 0 or a  one。

 depending on the rulery defined for that input。So we have here the truth table for a logic gate that shows the output。

 given that we're working with an or gate。 So if we think about an or gate。

 if either one of our two values。

![](img/51ee3f7771c662356b048682f48dda00_48.png)

![](img/51ee3f7771c662356b048682f48dda00_49.png)

![](img/51ee3f7771c662356b048682f48dda00_50.png)

Are equal to one。Then our output should be true， one is equivalent to true。

And the only time we should get false or zero is if both of our inputs are equal to zero。



![](img/51ee3f7771c662356b048682f48dda00_52.png)

So what we want to know is can we come up？

![](img/51ee3f7771c662356b048682f48dda00_54.png)

With a neuron that uses the sigmoid activation function that we just define that comes up with the values between 0 and 1。



![](img/51ee3f7771c662356b048682f48dda00_56.png)

That will allow us to always output the appropriate values of either 0 or one。



![](img/51ee3f7771c662356b048682f48dda00_58.png)

![](img/51ee3f7771c662356b048682f48dda00_59.png)

And the idea being that if the threshold is over 0。5， then the sigmoid would predict one。

 if it's less than 0。5， then it would predict 0。

![](img/51ee3f7771c662356b048682f48dda00_61.png)

So we pass in x1 and x2， and each of those will take on a value of either zero or 1。



![](img/51ee3f7771c662356b048682f48dda00_63.png)

As well as the intercepts， and then we multiply each of those by a certain weight。



![](img/51ee3f7771c662356b048682f48dda00_65.png)

![](img/51ee3f7771c662356b048682f48dda00_66.png)

Now， again， by limiting the inputs of x1 and x2 to be either0 or 1。

 we can simulate the effects of this logic gate that we just saw on the table above。



![](img/51ee3f7771c662356b048682f48dda00_68.png)

![](img/51ee3f7771c662356b048682f48dda00_69.png)

hich we saw over here。

![](img/51ee3f7771c662356b048682f48dda00_71.png)

The goal is to find the weights represented by the question marks we have here in this image。

 such that it returns an output close to zero or one depending on what the inputs are。



![](img/51ee3f7771c662356b048682f48dda00_73.png)

![](img/51ee3f7771c662356b048682f48dda00_74.png)

So the idea would be if we think through the or problem， if both x1 and x2 are equal to 0。

 then we want to output a0， otherwise if either of them are equal to one。

 then we would want to pass in a1。

![](img/51ee3f7771c662356b048682f48dda00_76.png)

![](img/51ee3f7771c662356b048682f48dda00_77.png)

![](img/51ee3f7771c662356b048682f48dda00_78.png)

So， we have。

![](img/51ee3f7771c662356b048682f48dda00_80.png)

To think about what those weight should be， and if we think about the plot that we have above。



![](img/51ee3f7771c662356b048682f48dda00_82.png)

If it's going to be very negative， again， negative 10 or less。

 then we would have a value very close to 0。 and if it's very positive， positive 10。

 then it's very close to 1。 So that's our goal。

![](img/51ee3f7771c662356b048682f48dda00_84.png)

![](img/51ee3f7771c662356b048682f48dda00_85.png)

![](img/51ee3f7771c662356b048682f48dda00_86.png)

So thinking this through。You can see at the picture here below we already have the weights。

 but let's talk through how these weights will actually work in outputting the actual value that you want for this orgate。



![](img/51ee3f7771c662356b048682f48dda00_88.png)

![](img/51ee3f7771c662356b048682f48dda00_89.png)

If x1 and x2 are both equal to0。The only value that's going to affect Z。



![](img/51ee3f7771c662356b048682f48dda00_91.png)

This equation that we have over here。Is going to be that intercept term of B。



![](img/51ee3f7771c662356b048682f48dda00_93.png)

And because we want the results for 00， if both x1 and x2 are equal to 0 to be close to 0。

 B should be negative。 It has to be less than 0 to ensure that our sigmoid function outputs a value less than our 0。

5。Now， if either x1 or x2 is 1， we want the output to be close to one。



![](img/51ee3f7771c662356b048682f48dda00_95.png)

And that means that the weights associated with x1 and x2 should be enough to offset that negative 10 that we have for B。

So if we give B that value of negative 10。W1 and W2 each have to be at least greater than 10。

 so we set them each to 20， so if x1 is equal to1， then we have negative 10 plus 20， positive 10。

 we pass that through the sigmoid function and that would output a value very close to 1。

Same would hold if we had x2 equal to 1 and x1 equal to zero。



![](img/51ee3f7771c662356b048682f48dda00_97.png)

And then if both of them are equal to1， then we end up with positive 30 and again we get once we pass positive 30 through the sigmoid function。

 we have a value very close to 1， so as long as either x1 or x2 are equal to 1。

 given the weights 2020 and intercept negative 10， we have the value of 1， and if both of them are 0。

 then we have the value of0 passed through our sigmoid function。



![](img/51ee3f7771c662356b048682f48dda00_99.png)

![](img/51ee3f7771c662356b048682f48dda00_100.png)

![](img/51ee3f7771c662356b048682f48dda00_101.png)

![](img/51ee3f7771c662356b048682f48dda00_102.png)

So here we see how we can come up with the appropriate weights。



![](img/51ee3f7771c662356b048682f48dda00_104.png)

To ensure that we actually complete this orR functionality。So I run this。



![](img/51ee3f7771c662356b048682f48dda00_106.png)

And the idea that we have here is that we create a function for the logic a that will take our W1 and our W2。



![](img/51ee3f7771c662356b048682f48dda00_108.png)

As well as our B。 And then return the sigmoid of W1 times X1 plus W2 times X 2 plus B。

 which is what we hope to ultimately output， given that。



![](img/51ee3f7771c662356b048682f48dda00_110.png)

![](img/51ee3f7771c662356b048682f48dda00_111.png)

We are passing in the z of W1x1 plus w2 x2+ B into our sigmoid。

 and then running the sigmoid and then hoping for a value of one。



![](img/51ee3f7771c662356b048682f48dda00_113.png)

![](img/51ee3f7771c662356b048682f48dda00_114.png)

Or zero depending on what we want our outputs to be if we're using an or gate or an end gate。

 so on and so forth。

![](img/51ee3f7771c662356b048682f48dda00_116.png)

![](img/51ee3f7771c662356b048682f48dda00_117.png)

And then we're going to test it。By saying for each one of these values，0，0，0，1，1，0， and1，1。



![](img/51ee3f7771c662356b048682f48dda00_119.png)

We want to output。4 a and B。Given our test。What is going to be the actual value of？A and B。

 and again， if we pass in that sigmoid， then we will end up with a value that is。One。

 once we pass you know 10 or 01 when we call MP dot rounds， it'll start off maybe with 0。9。

 and then we round it to ensure that we get a one。

![](img/51ee3f7771c662356b048682f48dda00_121.png)

So we have our or gate， which is just going to be equal to our logic gate。

 which we defined as just the sigmoid of that linear combination。



![](img/51ee3f7771c662356b048682f48dda00_123.png)

![](img/51ee3f7771c662356b048682f48dda00_124.png)

And we pass in our weights W1， W2 and our intercept of negative 10。 We test that orgate。

 and we get our different outputs， and we see that it matched up according with what we saw our or gate should actually be。



![](img/51ee3f7771c662356b048682f48dda00_126.png)

![](img/51ee3f7771c662356b048682f48dda00_127.png)

![](img/51ee3f7771c662356b048682f48dda00_128.png)

Now let's quickly look at the end gate and how we can come up with the end gate。



![](img/51ee3f7771c662356b048682f48dda00_130.png)

![](img/51ee3f7771c662356b048682f48dda00_131.png)

So with the end gate， when we look at the table that we have here。



![](img/51ee3f7771c662356b048682f48dda00_133.png)

If both of them are false， which is our00， then it should output false。



![](img/51ee3f7771c662356b048682f48dda00_135.png)

If only one of them is true， then they're not both true。 That's the ands gate， right。

 with the and gate you want both one input and the second input to both be true。



![](img/51ee3f7771c662356b048682f48dda00_137.png)

![](img/51ee3f7771c662356b048682f48dda00_138.png)

So we'd still have a zero， and it would stay zero unless both the inputs are both true。

So can we come up again with the appropriate weights to ensure that if they are both true？

Then we end up with a truth value， otherwise we get a false value。



![](img/51ee3f7771c662356b048682f48dda00_140.png)

So as we see here。We set the B equal to a negative value。 That's negative enough。



![](img/51ee3f7771c662356b048682f48dda00_142.png)

That even once we add on just one of these values， whether it's W1 or W2。



![](img/51ee3f7771c662356b048682f48dda00_144.png)

That would be the equivalent of just one of those being true。 We still have a negative output。

So B plus W2 times 1， we'd still have negative 20 plus 10。

 and we'd end up with negative 10 as long as it's negative。



![](img/51ee3f7771c662356b048682f48dda00_146.png)

![](img/51ee3f7771c662356b048682f48dda00_147.png)

Our sigmoid function will output a value less than 0。5， and we round that down to 0。

 The only way that we end up with a positive value is if both of these are true。



![](img/51ee3f7771c662356b048682f48dda00_149.png)

![](img/51ee3f7771c662356b048682f48dda00_150.png)

Then we have negative 20 plus 11 plus 10。And then we'd end up with positive  one。

 we pass positive one into our sigmoid function， and we have a value greater than 0。5。

Now these W1s and W2s can essentially be any number， well first let's show that this works。



![](img/51ee3f7771c662356b048682f48dda00_152.png)

We see that it outputs zero for every single value， except for11 as it should with our and gate。



![](img/51ee3f7771c662356b048682f48dda00_154.png)

We can see also， if we wanted to， we can make these values any value less than。



![](img/51ee3f7771c662356b048682f48dda00_156.png)

Negative 20 or less than the absolute value of 20。 so that once it's added on， it remains negative。

 but once both of those are added on， then it becomes positive。

 So both values have to add up to something greater than 20 and be less than 20。



![](img/51ee3f7771c662356b048682f48dda00_158.png)

![](img/51ee3f7771c662356b048682f48dda00_159.png)

So we could run this and see that again， we get all the correct values。



![](img/51ee3f7771c662356b048682f48dda00_161.png)

Now we're going to do the same thing for the n or gate and the n and gate and and or and an n just means not or and not n。

 so the opposite of or and the opposite of ns， and we'll see why this is important once we get to the next exercise。



![](img/51ee3f7771c662356b048682f48dda00_163.png)

![](img/51ee3f7771c662356b048682f48dda00_164.png)

![](img/51ee3f7771c662356b048682f48dda00_165.png)

So not or is just going to be the opposite of the or So if it's。

Any of these three values in this table， which would have all been true for the or。

 then we set it to false。And we only keep it at true if both values are equal to zero。



![](img/51ee3f7771c662356b048682f48dda00_167.png)

And thinking through which weights will work， we just need to ensure that we have a。



![](img/51ee3f7771c662356b048682f48dda00_169.png)

Positive value， if both are equal to 0。 Otherwise， we have a negative value。



![](img/51ee3f7771c662356b048682f48dda00_171.png)

So we just have to ensure that these are negative and their absolute value are both greater than B。

And then we have our N or gate， we'll double check the outputs。



![](img/51ee3f7771c662356b048682f48dda00_173.png)

And we see that 0，0 is equal to one， otherwise they're all0。



![](img/51ee3f7771c662356b048682f48dda00_175.png)

And then finally， we're going to close out this video with N end。



![](img/51ee3f7771c662356b048682f48dda00_177.png)

Where we'll see， again， this is just the opposite of the actual and。

 So and would only be true if both values were equal to true。 now that we do the opposite。

 it's true every other time， except for when both values are equal to true。



![](img/51ee3f7771c662356b048682f48dda00_179.png)

![](img/51ee3f7771c662356b048682f48dda00_180.png)

![](img/51ee3f7771c662356b048682f48dda00_181.png)

So what we need to do is we need to ensure that。As long as we have the inputs。Both being。

1 that will cancel out the B that we have here， otherwise。We always have a positive value。

So we do that by saying that these two added together W1 and W2 will outweigh the B。

 otherwise on their own they can never outweigh that B value。

 and that will ensure that we always have what we have here in terms of the not and gate。



![](img/51ee3f7771c662356b048682f48dda00_183.png)

![](img/51ee3f7771c662356b048682f48dda00_184.png)

![](img/51ee3f7771c662356b048682f48dda00_185.png)

And we can see that that holds us well。

![](img/51ee3f7771c662356b048682f48dda00_187.png)

Now in the next video， we're going to pick Ben up。

![](img/51ee3f7771c662356b048682f48dda00_189.png)

And discuss why there is a limit to only working with a single neuron and how we can build off of a single neuron。

 create another layer of neurons。

![](img/51ee3f7771c662356b048682f48dda00_191.png)

![](img/51ee3f7771c662356b048682f48dda00_192.png)

As we do with our multi layer perception and come up with this X or functionality。

 which we'll discuss in the next video。

![](img/51ee3f7771c662356b048682f48dda00_194.png)

All right， I'll see you there。

![](img/51ee3f7771c662356b048682f48dda00_196.png)