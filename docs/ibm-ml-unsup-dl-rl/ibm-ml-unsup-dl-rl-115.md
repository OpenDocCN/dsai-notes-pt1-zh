# 115：IBM《机器学习（无监督学习、深度学习和强化学习、毕业项目）｜machine learning》中英字幕 p115 76_RL笔记本（选修部分）第2部分.zh_en -BV1eu4m1F7oz_p115-

Welcome back。 Here。 we're in part 1。 In our first video。

 we introduced everything that we needed to understand for Open AI gym。

 And here we're going to run a bunch of game simulations through to the end。



![](img/df55a7efc076a9b003a6fcc93368e430_1.png)

And start to observe and gather data on what type of actions actually lead to rewards。



![](img/df55a7efc076a9b003a6fcc93368e430_3.png)

So some things to keep in mind is we're going to want to store each one of the different run throughs we're going to name each one of those episodes right we discussed earlier how from start to finish will be a single episode we start again that'll be the second episode and so on and so forth we're going to do that for  a thousand00 or more episodes here we're actually going do it for 40。

000 episodes。And at each step， we're going to want to save the observation where we currently are。

The action taken from that observation。And then the current reward。

 whether or not we've reached a reward state。

![](img/df55a7efc076a9b003a6fcc93368e430_5.png)

And some things to keep in mind as we go through this。

 we're going to want to reset our environments after each episode so that we started new。

We saw before how we output this tuple of new observation， reward。

 whether or not it's done in that extra information， every time we take a step。

 every time we take an action。And we're going to want to continue this game until done is equal to true until we're actually finished running through。

 that's going to be one of these outputs that we have。



![](img/df55a7efc076a9b003a6fcc93368e430_7.png)

And we saw how we did that up above。And then some things that we can work with if we think about M doaspace。

 n， that's going to give us the number of possible actions。 and if we think about that。

 that should be four possible actions up down right and left。And then if we want to take a sample。



![](img/df55a7efc076a9b003a6fcc93368e430_9.png)

That's going to be that random step and that's what we're going to be doing as we gather data。

And then if we want to know how many possible states are on the environment。

 we can call n dot observation space do n and that will tell us how many possible states are。

 and if we think about our  four by four grid， that means it's going to be 16 different states。

 so things that you can look at in general as you work with these environments。



![](img/df55a7efc076a9b003a6fcc93368e430_11.png)

![](img/df55a7efc076a9b003a6fcc93368e430_12.png)

![](img/df55a7efc076a9b003a6fcc93368e430_13.png)

So walking through what we have here。We're going to reinstate our environment。



![](img/df55a7efc076a9b003a6fcc93368e430_15.png)

We're saying that we want to go through 40，000 full episodes， so resetting up until the end。

So that's going to be our goal as we run through this for loop。

And are going to save all of them in this life memory。So then we say four I the number of episodes。

 and I'm actually going to pull this out so that we can look at some of these steps separately。



![](img/df55a7efc076a9b003a6fcc93368e430_17.png)

So this is the entire for loop that I'm going to pull out and put into a separate cell。



![](img/df55a7efc076a9b003a6fcc93368e430_19.png)

![](img/df55a7efc076a9b003a6fcc93368e430_20.png)

So we have the old observation， so starting off our old observation will'll be starting at 0。0。

Whether or not it's done， we're starting at false。 The total reward at this point is equal to 0。

 And the episode memory we have with， again， we're in this for loop that we just pulled out is set to blank。



![](img/df55a7efc076a9b003a6fcc93368e430_22.png)

![](img/df55a7efc076a9b003a6fcc93368e430_23.png)

And while not done。We take a new action。And that new action just going to be a random action？

And we saw earlier how we can call random action using m。actionspace。sample。

We can then get using that new action and using MD dot step。

 right that's how we take an action within our environment， we can get the current observation。

 the reward， whether or not we're done。And then our total reward for these steps will be equal to the pass reward plus the new reward。

 And generally speaking， this will only add up to 0，1， because once we hit one。

 we've hit the end of the game， or we could end the game if we land in a hole。

 and therefore we'd stay at 0。Then what we want to save at each step。Is going to be the observation。

 so where were before？What action did we take， What was the reward once we took that action。

 and then which episode were on， which will stay the same until we end the game， right。

 This is still all within this while loop。

![](img/df55a7efc076a9b003a6fcc93368e430_25.png)

And we keep doing this until we hit done。

![](img/df55a7efc076a9b003a6fcc93368e430_27.png)

So let's actually just run this portion， I'm going to put this in the cell below。



![](img/df55a7efc076a9b003a6fcc93368e430_29.png)

So we've run now one episode。And we said I， let's just see what our Is from a different for loop we have I is equal to4。

 so they're all going to be listed as episode4， but if we look at the episode memory now。



![](img/df55a7efc076a9b003a6fcc93368e430_31.png)

![](img/df55a7efc076a9b003a6fcc93368e430_32.png)

We see that we took a few steps observation zero， the actions that we took until we hit the end of the game。

And we hit the end of the game by falling in a hole。

 and therefore we didn't end up actually by ending in a hole。

 we didn't actually reach our reach through to our reward。



![](img/df55a7efc076a9b003a6fcc93368e430_34.png)

And then when we're saving these episode memories， we also want to save when we put this into a panda's data frame。

 which is what we're ultimately going to do。 We're going to want to save with it。

 So here we have the observation， the action， the reward， the episode。

 All these will be different features in our data frame。 We're also going to want the total reward。😊。



![](img/df55a7efc076a9b003a6fcc93368e430_36.png)

Which is just going to be those values that we added up as new rewards came in。

 So if we did hit a reward and we did get to one， then we'd attribute one to every single one of our different observations。



![](img/df55a7efc076a9b003a6fcc93368e430_38.png)

And then we want to look at this value i times total rewards divided by number of steps。

And what that does。 And we'll look at this a little bit later on once we actually have some that reached that reward state。

 is it's going to weight each of our steps， according to how close we were to achieving that award。

 So in the first step， that action is probably not as important as that last step that LED to our reward。

 So that will get a higher reward step， a higher decay reward is what that's going to be called。



![](img/df55a7efc076a9b003a6fcc93368e430_40.png)

![](img/df55a7efc076a9b003a6fcc93368e430_41.png)

So we're going to extend that life memory using this。



![](img/df55a7efc076a9b003a6fcc93368e430_43.png)

Oh we life memory size。Up here， and I pulled that out of the for loop。



![](img/df55a7efc076a9b003a6fcc93368e430_45.png)

But we can add on that episode memory and let's just look at what that episode memory looks like。

And you see that it's going to be the same as before。

 except now we added on that total reward plus that decay reward。



![](img/df55a7efc076a9b003a6fcc93368e430_47.png)

So that's going to be everything here within the for loop。

 and then that's going to be added onto our life memory。



![](img/df55a7efc076a9b003a6fcc93368e430_49.png)

Which is our list here。

![](img/df55a7efc076a9b003a6fcc93368e430_51.png)

And then at the end， we're going to take that life memory and put that into a panda's data frame。

So let's run this and this will take just a bit to run as we' running through 40。

000 different iterations。So now it's run， we're going to call memorydf。describe。



![](img/df55a7efc076a9b003a6fcc93368e430_53.png)

And we see for each one of our features that we have the number of observations or what where we were and the average space where we were。

 And usually you're probably closer to the beginning before we end。The actions taken。The reward。

 that mean reward will be useful to understand， but we'll get a little bit deeper into understanding the full reward that probably means more around the total reward amounts where we have 2。

4。

![](img/df55a7efc076a9b003a6fcc93368e430_55.png)

![](img/df55a7efc076a9b003a6fcc93368e430_56.png)

![](img/df55a7efc076a9b003a6fcc93368e430_57.png)

And what's going to be even or make this a bit clear， well， first， let's look at the shape。

We see that we have 306，997 different rows。And if we look at。A couple of values here。



![](img/df55a7efc076a9b003a6fcc93368e430_59.png)

Let's say memory D F。Such that the memory。DF is， or let's say。Let's just look at the first episode。

 or let's look where there's actually a reward。Well let's look where the total reward equals one。

 and I'll explain why in just a second。

![](img/df55a7efc076a9b003a6fcc93368e430_61.png)

So we have our total reward here equal to one。And we see that it took quite a few steps to get there。

And now we can start to understand what each one of these new rows actually mean。



![](img/df55a7efc076a9b003a6fcc93368e430_63.png)

So if we look at the reward at each individual step。And these are in order。Well。

 let's also just look at a single one of the episodes。

 so let's say episode is 182 because we see that that ended in a reward。

So if we look at each one of these different steps， they're in order。

And we see that the reward is zero all the way up until the end when we got to that final step。

And with that， our total reward。Is then equal to one。 and it says that for Epi 182。

 given the steps that we took， we were able to get to that end goal that we were looking for。

And then the decay reward weights each one of the steps differently。

According to how close we were to getting to that final step。 So here we are only a step away。

 So we get more decay reward。 whereas at the beginning in the first one。

 those steps probably as aren't as important in regards to getting to that final reward that we're looking for。

 And that's going to play a role in deciding how we're going to ultimately optimize on our rewarding system。



![](img/df55a7efc076a9b003a6fcc93368e430_65.png)

Then， to see。How often we actually got to the reward。 We're going to group by episode。

 So grouping all of our values by episode were summing the reward and recall that each episode will either sum to 0 or sum to one。

 As we saw here， these are all zeros for the reward feature， except for that final value。

 So if we group by episode， it'll either be 0 or one。 And if we take the average value。

 Then we can see on average， how often we were successful。😊。

And we see that we were only successful 1。4% of the time， so if we just take random steps。

 we probably won't be successful very often。

![](img/df55a7efc076a9b003a6fcc93368e430_67.png)

So the next goal and what we'll discuss in the next video is actually leveraging the data that we gathered all those 300000 x amount of rows that we now have to come up with more intelligent steps to take throughout our system。

 All right， I'll see you there。😊。

![](img/df55a7efc076a9b003a6fcc93368e430_69.png)

![](img/df55a7efc076a9b003a6fcc93368e430_70.png)