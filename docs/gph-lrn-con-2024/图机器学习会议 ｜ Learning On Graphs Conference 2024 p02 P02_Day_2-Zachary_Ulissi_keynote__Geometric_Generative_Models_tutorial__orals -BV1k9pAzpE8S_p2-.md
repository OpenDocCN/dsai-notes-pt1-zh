# 图机器学习会议 ｜ Learning On Graphs Conference 2024 p02 P02_Day_2-Zachary_Ulissi_keynote__Geometric_Generative_Models_tutorial__orals -BV1k9pAzpE8S_p2-

で、つに。

![](img/25574de0c5f6776e4df6b4107d6f5327_1.png)

How need circle you should get more at。你好惜为你。你嗯。I don is it this voting panel comes back every two seconds or no。

Oh， no， is it because people are joining， though because people got， yeah。O。还要看这个。可以过。好，嗯。对。Oh。

 it's cute。哎可以。嗯不。对。对。嗯。Perfect， I think it's 3 pm we can get started and hell everyone。

 welcome to day two of the law conference 2024 day one was very exciting all of participation and we hope it increases over the next three days。

😊，Okay。Hello。What happened？Yes no today we have a tutorial or the first session today is a tutorial on geometric generative models from Joy Bose。

 He Benheu and Alex Tong。Hello。😊，Is it working no can you hear Yeah。

 I think you cut off for a second but we can hear you now。是。え？Should we start？Yes no。

Now Joey is a postdoc in Michael Bnstein's group at Oxford and he did his PhD in MilA。

 he worked a lot on geometric deep learning and generative models and has produced some instrumental work。

In this field。And Alex is。嗯。听不懂。Alex is an incoming assistant professor at Duke University。

 a postdoc currently with Osa Bengeo has also worked a lot on optimal transport and geometric models with single cell biology Heley is an incoming research assistant at Fair and currently a final her ph。

嗯。😊，嗯的是。He student with Aaron Lipman and she's also involved in the interest instrumental flow matching paper all of our organizers today actually have a lot of experience with workshop organization participation and talks and we hope that。

We hope that they are able to distill all the complex concepts today in a very easy manner and I'm very much looking forward to the tutorial you can get started now。

Okay， thanks thanks for the kind introduction to  Iignation and thanks for people joining online and as well as people in the room physically at Oxford So this tutorial is going to be in three parts and it's about geometric generative models so I'll talk to the first five minutes before passing it on to He who's going take over from there so before we get started。

 the first thing I want to talk about is why do we even want to consider generative models right so generative models beyond images and text has been a new adaptation of a lot of the cool techniques so we've see generative models being applied on domains like proteins。

 robotics， information geometry and even climate modeling so these are all domains that have a lot of rich geometric structure that is not often afforded to you and when you try to take up generative models for text and images and as a result we need to rebuild tools to tackle these problems from the ground up and also from rigorous first principles and this is I guess the gateway point for the tutorial where we kind of build the tools to show you how a lot of the familiar concepts can be。

😊，Elevated to geometric structures and how do we actually build fate models on top of these complex geometries？

So having said that， this tutorial is going to be essentially three things。

 one of them is that it's going to be an introduction to main geometric concepts that we actually use to build these models so we're going to kind try to distill a lot of the key practices that people are currently using to make more efficient and numerically stable models and then finally we're going to try to figure out how to actually code some of these things and see how some of the complex geometric operations actually translate into actual lines of Pytorch code what this is not is that it's not a crash course on differential geometry it's not meant to be super rigorous on the math side of things although we will cover some math concepts and it's not going to be a rundown list of all of our papers or all the papers in this field and what the state of the artrc models are although we will touch upon some some design decisions that people use to build these state of the artrc models and finally it's not a history lesson of all prior attempts to tackleable geometric data with generative models it's really tailor made for what people are currently doing in practice now and the modern generative modeling setup and how this is。

of elevating the field forward。So having said that。

 the tutorial is broken down into three parts roughly an our H。

 the first part is going to be by He and the second part is going to be by me and the third part is going to be by Alex。

 the first part is going to be a primer on modern generative models and then we'll move on to geometry as well as the actual bringing the pieces together and how to build these geometric geneative models。

😊，Wait one second， sorry， this is the long slide deck。2。🤢。



![](img/25574de0c5f6776e4df6b4107d6f5327_3.png)

Yeah， sorry， sorry， that was the wrong slide deck， but this is the correct slide deck now。

So yeah the first part' is going to be by He second part's going to be by me which is how do you build geometric tools and finally Alex will bring everything together in combining geometry and geneerative models in practice so with that i'll pass it on to He and He please feel free to take over the screen control。

😊，Thank you。嗯。

![](img/25574de0c5f6776e4df6b4107d6f5327_5.png)

Okay， so。

![](img/25574de0c5f6776e4df6b4107d6f5327_7.png)

How don't I go back to I'm going to share my screen。Do you see the slides okay？Yes， I believe so。

Okay， so we're going to begin with our first part of the talk about simulation freegenrative models。

So let's dive in so we're going to start by setting the problem that we want to solve in generative modeling and the setting is as follows what we have is some unknown data distribution we denote asQ and we typically have access to it via finite set of samples which we denote it as x1 and it will become clear later on in the talk why this subscriptive one appears here。

 but the setting is that we only have access to finite set of samples from that unknown distribution。

And our goal is to learn a sampler from that unknown distribution queue。

 meaning that we want to learn some function that is able to efficiently generate novel samples from the unknown data distribution that we're interested in。

So this is in general， the goal of the generative modeling problem。Now， in deep geneative modeling。

 what we do is， of course， we learn a neural network with parameter data。

And a generative model could be abstracted into I mean from my point of view。

 it could be abstracted into this topple of two objects。

 so the first object psi of theta is the generator。

 which is a function that allows sampling generating new samples from the learned data distribution。

And。B of theta is the underlying density that is defined by this generator function。

So for some algorithms of deep geneative modeling， we may or may not have access to this underlying density。

 but in any case our goal is always the same， we want to find parameters data such that the underlying density that our generator creates approximates the target density cube。

🤧嗯。So let's try to put it into a more visual scheme。

 we have our finite set of samples from the target distribution queue。

And the way that we typically build a generative model is by choosing some easy to sample source distribution with denote as P and samples are denoted as Qn。

And the generator takes samples from the source distribution and transform them into samples that ideally would be distributed as the target。

And as for the modeling of the density， the underlying density， for example。

 in this case it would look like this， and if we have access to this quantity this P of theta。

 then we would want it to approximate Q。Now， the question is how do we model a psi of theta。

 how do we model our generator， because there are so many ways to do that。

So in deep generative modeling， we will go quickly about these three paradigms and then we will continue to the paradigms that we're going to talk about today which are flow matching and diffusion。

 but autoaggressive models， we have a likelihood based model。

 which sequentially generates the signal that it wants to generate。

 it is likelihood based and thegeneration is sequential。

 meaning that that the generation depends on the dimension of the signal that we want to generate。

 this works very well for language modeling is all of you may know for other lambs。

 but it is less trivial order to use for example for images or even for geometric applications were the order of pixels over coordinates of elements in our data。

 is not very clear。The second paradigm of the modeling。

 which was pretty much the most dominant one until the appearance of diffusion models is GNs。

 and in YNs the generator is simply a neural network that takes latent vectors from some latent space which is typically in a lower dimension than the data and transform them into data sound。

Now training this generator involves defining a discriminator which receives samples from both the data distribution and generated samples from the generator。

 and it needs to predict whether're really little fake。

Training this model involves solving a miniax problem。

 which could be very hard to train its delicate training procedure。

 and we do not have access to computing the likelihood。

 which could be interesting for some scientific applications。

 but it enables fast sampling compared to autoaggressive models， for example。嗯。

And the last paradigm is VAEs where we define a autoencoder except now this latent vector is defined to be sampled from some distribution that we restrict while training。

 so it also enables fast training fast sampling sorry but we do not have access to likelihood as well。

So far， it has shown inferior performance compared to， for example， Gs。

So it has not become such a dominant generative model yet。In this talk。

 we are going to focus on dynamical systems as gene models。

And the way that we're going to view our generative model is。

By a time dependent process so a dynamical system describes the evolution of point samples in time。

 so now we augment our generator with an extra dimension and we have a time dependent generator。

And sampling actually becomes simulating。Given a source distribution。

 we sample a point sample x not and to generate a sample at time t and so the choice here for1 is completely arbitrary。

 but let's say that at time1 we want to reach our target Q and that's why the sub one。

 so we need to learn the components that allow us to simulate this process such that we sample from the target distribution。

Now， there are two main dynamical systems that people have worked with to develop generative models and each of these equations。

Define the instantaneous or the infinitesimal change that a for example make in time。

 so here the infiniteicsimal change in x in the position of x is dictated by a velocity field that tells us in which direction we need to take this infinitesimal step。

And this defines an ordinary differential equation。And for diffusion models。

 we have a stochastic differential equation， so the first part is pretty much the same as the ODE we're now instead of calling this velocity。

 this is called the drift of the SDE。And the stochastic part comes here。

 so we have a diffusion coefficient which is also a deterministic function。

 and here we consider only diffusion coefficients that depend on time and not on deposit position x。

And。A brown in motion so the brown motion is the stochastic part。

 and you could imagine that it means that at every time step you add a little bit of Gausian noise to your process。

So。🤧嗯。Let's see what it means to simulate from these processes so starting with x not we want to solve the initial value problem and this amounts to basically taking the integral over the velocity so starting in x not we consider trajectory that the point follows reaching its final point at time t and。

For every time on this trajectory， the velocity field， as we already said。

 points towards the direction in which we need to take our next infinitesimal step。

And this is a deterministic function， so if we start from x0 again and we take this integral。

 we will end up in the same xt。On the contrary for diffusion， although we write it as an integral。

 it is not as standard integral because because we have here integration over brown and mohe so this is a stochastic thing so。

simulating this process would look something like this and if we simulate it again so we start from the same xn and we take this integration once again。

 we will not end up in the same in the same position okay so this is something that's important to note so compared to flows where we define a deterministic map here we have a stochastic process or a random variable that is defined by this simulation。

Now these are integrals， but if we are working on a computer we would want we wouldn't be able to simulate these things。

 so an important thing to mention is that。We have ways to do that by numerically computing these integrals and there are many standard ways to do that。

 but I think that the simplest solvers for both ordinary differential equations and stochastic differential equations are the oiler or the oiler Mariaama sample methods which basically say that at each time stamp if you want to take we want to move to x of t plus some small delta t which is the discretization interval of what we integrate over。

 so we take a step in the velocity direction times the time interval， the discretization interval。

So and for diffusion， it's similar， except now we have this， again， the brown and motion term here。

🤧嗯。Okay。But we want to build a generative model out of this so the question is where are the probabilities right I mean when we build a generative model we want to be able to make the probability density of our model match on target。

And。In fact， both these dynamical systems have characterization of how either the velocity field or the drift and the diffusion coefficient are tied to the evolution of the probability density in time。

 so if we start with samples from a source distribution P and we simulate their trajectories according to the velocity or according to the SDE that is defined by the drift and the diffusion coefficient。

 then we know how the probability densities will change in time。🤧And。This is a major thing。

 and in fact， we can kind of say that。For flows， if we have UT if we have the velocity。

 so maybe we can build a generative model out of this right so we can we can have UT and make it evolve to probability density over our desire and。

On the same note for diffusion。So。Can we build a geneerative model with this and the answer for flows is quite immediately yes。

 but for diffusion we need one more thing。And we're going to go over it。So。In diffusion。

 the stochastic differential equation actually only describes the forward direction okay。

 so if we start from some known from the data distribution， simulating this equation。

 simulating this SDE would only add more and more and more and more noise。

 eventually at infinite time， reaching a stationary distribution of this stochastic process。Now。

If we want to build a generative model， we need to be able to simulate the reverse one， right。

 we're going to start from a noise sample and then transform it into a data sample。And。To do that。

 there is a reverse process that has the same the same marginal distributions， meaning that。P of T。

 the distribution of time T would be the same for both the forward and the reverse。

 but depends on where we start to simulate it。And。The reverse SDE is formulated with the one more thing that we need。

 and it is the score function， which is the gradient of the log density at time0。Now。

For learning a score based model， it has been shown in diffusion and score literature that we can learn the score。

 the marginal score by regressiongressing to conditional scores and I intentionally write here P data and not Q because the times in diffusion and in flows are reversed so we're clear that here we're taking a sample from the data。

 x is a data sample， and simulate the forward process and that yields the conditional flow on a data sample。

嗯。And。What we call these model simulation free is that for certain SDDs， we have ways to。First。

 to actually sample from this conditional distribution without having to simulate the forward SD。

 they have closed form expressions and the second thing is that we don't have to simulate the process at all or back propagate through it during training this model。

And we're going to be using these similar principles when we build a flow matching model。

 so keep that in mind。He。Okay， so to build a diffusion model， what we need is to learn the score。And。

A few things to note here is that we could only use it with a Gaussian source distribution this is kind of a limitation of diffusion models that is going to be lifted when we use flows and we'll also discuss why it is beneficial to use flows over diffusion for geometric applications and also the solution only reaches the source distribution asymptotically so we have to simulate our。

Our process for a very long time， ideally to infinity so and this occurs errors when we want to simulate the reverse process because we do not really reach the source distribution。

Now。We move on to flows， so when flows things are actually a bit simpler。Because under some。

Very reasonable regularity assumptions on the velocity field。

 it will define a flow psi of t and this flow function defines a deformmorphism in the space which means that it is smooth and it has a smooth inverse and this inverse is defined by simply minus the velocity field so compared to diffusion models where simulating forward in time and simulating backward in time involve different components and different objects that we need here all we need is the velocity field。

嗯。Which makes things a little bit simpler when we want to build a generative model with these。🤧Okay。

So to build a generative model with flows， what we need is to learn the velocity field。

And in fact it is it can define a universal transformation between densities what it means is that we can start off from any source distribution to any target distribution and we are guaranteed that exists some velocity field that transfers between these two and this is compared to the limitation of a Gaussian source for diffusion。

And the second thing is that it is defined on a finite time interval， and again。

 we don't have these errors in the boundary of small mismatch between our distributions。

So we're now going to deep dive into building a flow matching model。

 we're going to revisit diffusion once in a while， but the main focus of the talk is going to be on flow models。

So。We said that we have the flow ODE that describes the motion of point samples in time。

 and correspondingly we have the continuity equation。

 which couples between the velocity and the evolution of the probability density in time and we can see it visualized here in these nice gs。

And now the goal when we want to build a flow generative model is to find a velocity field UT such that the terminal distribution P of1 would match our target Okay so we refined a little bit goal for generative modeling problem for the flow case。

So using flows as generative models were originally introduced in the neural ODE paper by Chan at Al and what they suggested was。

To use the fact that we can compute the log likelihood of the model okay so a little giic massage to the continuity equation would give us the equation here on the right and this basically means that if we can compute the log density over source distribution which is true if we chose it to be a Gausian。

 then we can integrate over the divergence of the velocity field to get the log density of the model at every desired time。

And if we can compute the log likelihood of our model。

 then why not use the maximum likelihood objective to train it？Sounds like a good idea。However。

It requires a lot of computational overhead。Why is there so first we need to simulate Xt during training right because we need to evaluate our velocity field at every XC and furthermore。

 we need to take derivative derivatives of our velocity field which we don't have。

A very efficient way to compute the divergence in high dimensions。

 so we resort to some although unbiased but to an estimator。

But the main thing is that we also have to back propagate for this simulation。

 so this becomes very heavily computationally to even train such models and。

Flows have not been scaled up much until the introduction of flow matching like types of models。

So here we are。So basically flow matching was introduced in the same time with stochastic interpoolents and rectified flows。

 they were all published in the same eyeCL。Conference in 2023。

Although they arrived in the same method eventually。

 they did come from a little bit different perspectives and we're going to follow the flow matchingtting perspective on building a simulation free method for training flows。

い。ok。So the basic idea in flow matching says。😊，Okay， so we said that。

We have the continuity equation that couples between the velocity and and the distribution and time and the core principle that we're going to follow in suggesting are。

Our training objective is the fact that the velocity field generates a probability path if and only if it satisfies the continuity equation。

 so that means that if we could build some probability path that starts from a source distribution and reaches an approximation of our target distribution。

Then。We could regress through the velocity field， and if we have the velocity field。

 then we have a way to generate samples from that distribution。

But to do that we need to do two things so the first thing is we have to build this target probability path and it needs to satisfy the boundary conditions starting at the source and at the end reaching an approximation of the target and we need to have the generating velocity field U。

The second thing that we need is to find a tractable optimization objective for this because at this point。

 a few of you may wonder， okay， but if you have your velocity fielded in hand。

 why do you need to learn it？Right， so we're not going to have it in hand。

 we're going to find some other way to do that and that's going to be involved in finding the tractable optimization objective。

😊，So we're going to start with building our target probability paths and that's going to guide us towards finding our tractable objective。

As well。🤧嗯。So to build a conditional， we build a probability path by building conditional probability paths。

 so our boundary conditions are， as we said before。

 and we use the law of total probability to define our probability path as marginalization over some conditional probabilities。

Where if we choose our probability path， for example， to satisfy that at time zero。

 it is simply the source distribution and at time1 it takes all of the samples to the x1 sample from the data that it is conditioned on and if we plug in these two into these integral we will easily see that it satisfies the boundary conditions。

Just for notation and so we understand what we say。

This is called the marginal path and the conditional path is called the conditional path。

Now we're going to see how we're going to build it， the velocity field that generates it。So。

For the conditional path， there exists some conditional velocity field that generates it。

And we can express the marginal velocity field that generates our marginal probability path。

In terms of the conditional velocity field。Okay， so we turn our problem of finding the marginal to the marginal velocity field into marginalizing over some。

VConitional velocity field that is potentially easy to compute or find。Okay， so。

Looking at this ugly integral。😊，we are not done yet。

 we do need to further find an tractable objective。

 but I want to note that here we could also marginalize over any condition and some useful different conditioning that could be used is to instead of conditioning over only the X1 over only over the data points we could condition on both a source sample and a target sample and this will open up the option to define different kinds of couplings between the source in the target。

 it could look into these two papers below that for example suggested solving mini batch optimal transport couplings between the source and the target to make the learned trajectories。

Straighter and hence。To have a faster inference solving when simulating from such flows。

And another option is to condition an x not。Which will。

As we will see later connects to the epsilon prediction or noise prediction in diffusion models。🤧嗯。

And just for notational simplicity by base rule， we could switch all this to just reverse the conditioning。

 so here we have PT of Z given x so it's like simpler to write it down。Now。

We need to find our conditional flow matching loss that will allow us to actually train such a model。

So。Similar to in score matching， as we said that we could regress to a conditional score to learn our marginal score。

 it turns out that also for flow matching we have this equivalent， so equivalent sorry。

 so the minimizer of the flow matching loss is the same minimizer as the conditional flow matching loss and actually only by building conditional velocities we could learn the marginal one that generates our PT our desired PT。

如不。So we're almost done okay， so we built our target probability path and we define the velocity field that we want to match and we found a tractable objective。

 but the last bit that we're missing is。How do we compute the conditional velocity for some conditional probability paths and the way that we do it and that is pretty。

 I think relatively easy to。To construct。Is to build these conditional paths by。

Building the conditional flows。All right， so the conditional flow simply tells us if we take a poor example at time zero where it will end up at time T and the derivative of the flow is the velocity field right it's connected by the flow ODE。

So。And we will build the conditional flows such that at time zero they leave the point in place and at time one they take all the points to the。

The condition the sample X1。Okay， so we have the conditional probability pack。

 the corresponding velocity field that generates it。

 and the corresponding flow that is defined by this velocity field。ok。Now。

 a popular instance of conditional flows that argued used are aconitional flow。

And they simply they're called laughingine because they're enough in combination of x1 and x0 where alpha t and sigma T are time dependent coefficients and in order to satisfy the boundary conditions we would need alpha t to be0 at time 1 and sigma T to be1 sorry at time zero and sigma T to be。

One to time zero and vice versa， okay？No。To get the velocity field。

 we write down the flow ODE okay so we have the time derivative of the flow as equal to the velocity field evaluated at the point flow website condition on next one。

And。Computing this derivative， we get that it's simply the derivative of alpha t with respect to time time x1 plus sigity dot dot denote derivative with respect to time times x0。

And if we plug in。Like we take x0 out and we plug in that so that we can evaluate the velocity field for every position in space and not just on trajectories of the flow。

 then we end up with this expression。And in fact， we could have various prediction types that are related to flow matching。

 so if we condition our velocity field on x1 and then we marginalize okay so we take the conditional velocity field here z is x1 and we marginalize to get the marginal velocity field we will end up with a velocity field that is a function of x and of what people typically call x prediction or the noiser and we can see that we can transform from the x prediction to the velocity field in a very simple iine function。

The same goes for epsilon prediction， so if we condition on x not and marginalize。

 we will have the expression that connects between the velocity field and epsilon prediction。

And lastly， full condition on both we' have an expression that's dependent on both。So in fact。

 we have shown that all of these types of predictions are transferable and we can move from one to another quite easily。

🤧嗯。One instance of a choice of conditional flows that are used and was proposed in the flow matching paper is to use aine conditionals that connect at0 and xt in a straight line。

 so it basically is a linear function， so alpha t is t and sigma t is 1 minus t and we connect between them in a straight line。

And the corresponding velocity field would be x1 minus x0。🤧Okay。Okay。We could also instantiate。

Flow matching for a choice of a Gaussian source distribution of course and for and in that case x of T will be distributed as a Gaussian and we could also instantiate both the variances exploding and the variances preserving schedulers that are popularly used in diffusion models。

 so we see that we have lost nothing in terms of the expressiveness of our model when we move to flow so we could you know we could also create these paths with flow models but on the other hand we cannot create for example the conditional optimal transport paths with diffusion。

And。Just to visualize how these trajectories differ。

 these are conditional trajectories so the black points are initial sampled from x0 and we see how the conditional flow moves them to x1 which is the red point so with condo T we see that these are straight lines and with variance preserving we see that the trajectories sometimes over should the sample and like go above it。

😀嗯。Okay， so we have defined our way to build our gene model。

And let's put it into some recipe that we could follow。

So given a set of samples for Mo target distribution Q。We need to construct a probability path PT。

 such that at time zero， it is the source distribution and at time one。

 it will reach an approximation of our target。And the way that we build these probability paths is by building conditional flows。

🤧And。To learn our generative model， we need to learn a velocity field UT with the conditional flow matching loss regressing to conditional velocities。

our goal is to build marginal flows that generate PT。Okay。

 so now we're going to have a short coding session trying to put these things into。呃。So one sec。我其消。

Do you see my screen。Yes。Allright。Okay。So we're going to build a。

A very simple example of a flow matching model on two dimensional data set and we're going to follow our recipe for building this model so we're going to have our data and then construct the probability path by defining a conditional flow and then we're going to write down the training loop and eventually we're also going to sample from it so I have a pre cloned the Repo we're going to need it for our data。

And here we have defined the architectures or these are standards set up for neural network training。

 we're not going to go over it。So we define our architecture， which is a simple MLP。

 and then we define a model that will represent the velocity field that we're learning and the optimizer that we're going to use。

And now let's define our data。In the assets directory we have two images one is like an image that says log and the second one is one that says 2024 and we're going to create two the distributions based on a mask on these masks so here we have a function that samples uniformly from the white digits or letters and so let's。

Sample from our data。Oh， what happened？I'm not into right directory。H文。Oh， okay。Oh。Yeah。

 I'm not sure where okay， I'll just use。Absolute path， so we don't waste much time on this。

 I don't know what happened。H we switched them。Oh。Okay， something is wrong here and I'm not sure。

What it is。Okay， so let's try the other way around。 we'll try to connect to a。

To a hoster runtime story。HThere aren't any GPUs。🤧Okay。

Do you have any questions in the meantime while I try to solve this problem？Okay。

 so we're going to try it this。I wrong。你是谁。Okay， Jo， do you wantna maybe。

Go ahead and continue and we'll try to get back to it later。Yeah， so I can take over the second part。

 but for what it's worth some of the people in the audience say that it's working for them so I think it's maybe stochastic No I mean I don't know what like first I mean you see that like the images exist in the path and they open them。

So one another thing we could do is， I mean we could。Just maybe go over this， I'm sorry for。

I don't know why why it doesn't work， but maybe let's go real quick over this so if you sample from these masks that I showed you you will get these source samples which write out log and the target samples are going to write out the 2024 and we're going to try to learn a flow model from log to the 2024 distribution。

Okay and now so this is the data and now we're going to define our conditional flows so for for conditional optimal transport we have a very simple expression for our flow so x of t will be1 minus t times x0 plus t times x1 and the velocity field is the time derivative with respect to。

With respect to time， yielding x1 minus x0。So to write down the training loop。

 so we need to take our expectations over time uniformly and we need to sample points from our source and our target distribution。

 so this is the function that samples uniformly from the mask。

We're going to compute our conditional flow x of T and the conditional velocity field UT and run out the model。

To compute the neural network prediction of the velocity field and our loss is simply an L2。

Distance between the predicted velocity field and the target conditional velocity field。

So if we trained this for a while and in the meantime we were supposed to write down our Ouler silver that will allow us to simulate from our model so Eer silver as we presented the in the slides it gives us a recursive。

😊，A recursive formula for for evolving x T in time。

 so xt in the next step would be equal to xt plus the velocity field times the disctization interval that we chose and this discretization interval is defined by the。

The difference between the end point in time and the sharp point in time divided by the number of steps that we want to take。

 and we also need to progress D as we take our steps。

if everything works and we take our model and we sample from oiluler sampler。

 we can see that we generated samples that right out 2024。😊，And eventually， we could。

Animate the point trajectories so this should work I think because it's independent so here we see the flow of like going from the source distribution of writing out log to the target distribution 2024 so I think it's a nice illustration of the flexibility flow matching that we can use any source any target distribution and learn a flow model from that。

Okay。So that was less successful than expected， but let's move on。

 I have like two more slides and we're done。😊，Okay， so。You can see my screen。Oh。

 it's the outer screen。Okay。So flow matching has been extended to very various interesting applications。

 the one that we're going to be focusing also in the next parts are flow matching for general geometries。

 and has also been used for designing equivariant models for two permutations and all sorts of symmetries。

I think the most prominent use of flow matching is the adoption of the stable diffusion3 model to use flow matching instead of diffusion models。

 it has also been used for control generation and to solve inverse problems。

 also utilizing the fact that we could use any prior only source distribution that we want so to learn different types of generative models。

So to sum up this part， we've shown that flows are powerful generative models when they're supervised adequately。

 for example， with flow matching， and it gives us a flexible framework for training generative flows and compared to diffusion。

 they alsofer improved sampling speed and stability when we use the conditional optimal transport paths compared to the variance preservative for example。

😊，So now I think some open challenges on this part is that we still have to simulate the process so sampling a point from the generative model does involve taking multiple forward passes through the neural network which could be prohibitive。

 so it's still a challenge to learn a one step model。

 the performance as well as these models without distillation and the second thing is to scale these two other domains because I mean flow matching has been very successful for images。

 but for example， on language it still does not match the performance of auto aggressivegressive models。

So on that note， thank you for listening and Joey。😊。



![](img/25574de0c5f6776e4df6b4107d6f5327_9.png)

You can take that over。

![](img/25574de0c5f6776e4df6b4107d6f5327_11.png)

Thanks Ha for the great first part of the tutorial， let me start sharing my screen and then oops。



![](img/25574de0c5f6776e4df6b4107d6f5327_13.png)

系。Well。Yeah let's go here， so in the meantime maybe if there's any questions we can take some questions now。

 otherwise we can jump straight to the second part of this tutorial。

 which is going to be how do we build in geometry for machine learning？



![](img/25574de0c5f6776e4df6b4107d6f5327_15.png)

![](img/25574de0c5f6776e4df6b4107d6f5327_16.png)

三。OkayIf there's no questions， I'll get straight to the point and get started。嗯。

So when we think of generative models on manifolds。

 the first question you have to ask yourself like which of the current things actually remain constant and which of the current things need to be rethought from the first principles so for instance our data previously was on regular Euclidean space now the first choice we have to think about is like well what happens to the data when you parameter on a manifold and how do we construct probability paths before they were quite simple there were straight lines and but on manifolds there's no such easy notion of straight lines or the conventional notions have to be adapted and how do you build conditional flows and even how do we define velocity fields all these things need to be kind of elevated to manifold so this brings in many questions the first question being how do you even represent a point on a manifold this is a design decision also in regular flow matching we had kind of a Gaussian source distribution or even in diffusion unfortunately for manifolds theres no canonical definition of a Gaussian distribution they are obviously analogs to it so the choice of a source distribution。

It has to be adapted as well。 So there's different choices， and it's unclear。

 which is always the superior choice。And more importantly。

 when you define your flow map regular Euclidean flows。

 we could easily do alpha t times x1 plus sigma x0 unfortunately manifolds don't have this vector phase structure so we can do addition so this immediately break so this has to be elevated so even defining straight flows is a challenge and lastly the notion of velocity and vector fields also needs to be kind of generalized to manifolds so we're going to tackle these points but but in doing so we're going to try to take a bit from the point of geometry so what do we need to invoke in geometry to enable us to define these concepts for generative models。

😊，The first thing is that well how do we even characterize points on the manifold so your data distribution that we want to represent well to do that we can think of it we have to first define what a manifold is so for the purposes of this presentation and this is not going be extremely rigorous but it's going to be serviceable you can think of a manifold as being a smooth topological space that locally looks like a vector space but globally when you glue together these patches looks totally different as a result we can say that each point belong to this topological space but even when you do this you have to think about consideration so when you do generative models。

 how does the curvature of the manifold affect numerical stability how do we even parameterize this manifold there's more than one choice or do we use extrinsic coordinates do we use intrinsic coordinates and other choices and finally what additional structure do you need to impose to actually do generative modeling so just having a topological space iss not enough even though it's smooth it's not enough we need to add additional structure and we'll see the structure。

We really need is the notion of a metric which allows us to define distances as well as angles and norms。

So moving forward。So let's be a bit more formal about what a manifold is so a manifold as I mentioned earlier。

 there's local patches that kind of look like Euclidean spaces so more formally these things are called charts so each chart is an open subset UI and has a corresponding homemorphism phi that takes you from this chart to a corresponding Euclidean space so in this picture over here we have some manifold and recovering it with these subsets and these subsets may intersect at different points and from each subset there is a chart there's a map phi that takes you to an equivalent Euclidean space and the reason this is cool is because well we know how to do deep learning and geneative Euclidean space so we want to be able to use this fact or exploit this fact to kind of generative modeling on manifolds so the one thing we have note is that when you define charts that cover the manifold we have to first assume that the charts are smooth so we've already added some structure to the problem the smoothness means that the charts and the maps between charts are going to be seen infinity。

Definitely continuous and differentiable， and this is an important criteria because the vector feels that we will define have to satisfy this property。

So in this picture over here， if I define a subset U I want a chart that takes me to a corresponding space that's mapped under the chart and correspondingly if there's another chart UJ there's a corresponding homemorphism phiJ that takes me to a corresponding putting space and now the key condition that holds everything together is how do we glue charts together so more specifically if the chart intersect at certain points we need to be able to handle this criteria this area over here so this chart to chart gluing theres this technique where you can kind of just follow the arrows so you go from phi and you go from phi J to phi I inverse and you can go back to UJ so there's an easy compatibility condition that needs to be satisfied which is to say that whenever charts intersect there's a smooth way of going from chart to chart and as a result you can kind of glue together a manifold by just picking different charts such that they covers the entire manifold and each chart has its own you can think of it as its own phi map which allows you to define a corresponding local。

In space。And that's kind of the high level idea。So having said that。

 how do we actually represent the manifold so there's more than one way of representing the same geometric object。

 the two main ways I'll talk about is extrinsic versus intrinsicsic so what do I mean by extrinsic so extrinsic means you take a manifold and you embedded it in a higherdial Euclidean space so in the picture over here we have a sphere in R3 so R3 being the ambient space and the sphere is S2 and we can say that the sphere lives or is embedded in this higher dimensional Eucclean space and the way you do that is that more formally you can embed any manifold by defining what's called an inclusion map so the inclusion map is this function Yoda that takes every point on the manifold and tends it to an equivalent point on RM。

And the general principle is that we want to kind of when you think about parameterizations of manifolds for generalative modeling。

 we want to think like a deep learner， which is to say that let's make it as close as possible to something that already works and we know iss scalable and we know how to work really well with Euclidean spaces。

 so excentric parameterizations of manifolds are very nice for this exact reason because you can use the coordinate system of the ambient space to do a lot of your deep learning machinery。

So one quick advantage over here is that the ammbient space is a vector space。

 so a lot of the familiar operations like addition are permissible and the ammbient space which may not be permissible on like a regular manifold。

 so that's something to think about。So another equivalent choice is an intrinsic coordinate system so when I say intrinsic I mean a local coordinate system where you just choose charts and the charts that cover the manifold and you do all of your computation in these charts so a very simple example for the sphere because the sphere is such an illustrative example is that you can do something called stereographic projection and you can cover the sphere with two charts and these charts are essentially divide the north pole in the south pole so if you take a point on the upper hemisphere of the sphere and you start drawing a line from that point and see where it intersects the plane that cuts the sphere in half that's called the stereographic projection of the sphere and you need two of these charts so u plus a new negative to cover the top half of the sphere and the negative of the sphere so immediately there's a problem with this parameterization not from a mathematical point of view but from a machine learning point of view the problem is that you slowly reach numerical instability as you get to the poles。

When you're thinking about doing geneative modeling， using these charts。

 you have to be conscious of this fact because for normal geometric applications。

 when we actually do numerical computation this could be a problem so as a result one preferred solution is to maybe perhaps use the extrinsic coordinate system as opposed to the intrinsic coordinate system on the sphere。

So another equally valid choice of coordinate system for the sphere is that you can use what's called almost global coordinate system so any point on the sphere can be represented by some radius from the origin and some angle theta and some angle5 if you give me these three things I could pick any point in the sphere and represent it as a valid parameterization of a point on the sphere。

So many times this is going to be also a favorable parameterization。So again。

 one thing you have to consider is that just when you pick a parameterization what is the actual potential drawbacks right one drawback is that because we're using angles we have to use trigometric functions and can these trigoometric functions be always numerically stable and also their inverses so sometimes when you do computations we have to compute inverses of these trigmetric functions and as a result for small angles they could be numerically unstable。

Okay so the next object that we really need to define is that for diffusion models and flow matching models。

 everything is related to defining velocity fields and vector fields。

 so to generalize the notion of velocity and vector fields we need to define a tangent vector so a tangent vector is essentially for every point P on a manifold you define a function that takes you to RN and we want to kind of essentially say oh。

 a tangent vector is tangent to a point on the manifold so in this picture over here we have the point P and a tangent vector is you can think of it as like living on the tangent plane attached to the point P。

Now we'll have a more formal definition， but the picture is。

Essentially the same for all types of manifold， you want to define a tangent vector that's essentially tangent to some sort of curve at a particular point。

😡，And if you have a local coordinate system， we can always represent the tangent vector using the local basis。

 so in this picture over here， imagine your tangent space。

 which is the set of all possible tangent vectors at a point P you have a basis vector so you can represent each tangent vector at this point using the equivalent tangent basis and this is what we call a local basis to represent the tangent vector。

😡，So a bit more about tangent spaces， so a bit more formally， a tangent space。

 you can define it as like by first defining a parametric curve， which is time dependent。

 if you pick a curve of gamma that goes from let's say minus1 to1 says that it passes through the point P at gamma equals zero。

 you can think of a tangent vector being the time derivative at that point on the curve。

And as a result， you can think of this as the DDT of the curve gamma evaluated at t equals0。

 so that's formally what a tangent vector is it's literally attending to an equivalent curve that's passing through that point。

😡，Now， as a result of this， because we have the tangent vector and we have a local chart at that point。

 we can always say， oh， we can take the basis vectors and RD and pull them back by using the map Phi inverse to define an equivalent set of basis vectors for the tangent space。

😡，So this gives you a local parameterization for how to define a tangent vector using intrinsic coordinates。

So a bit more formally， if you have a point p， you can say that local coordinates of the point p is essentially given to you by a differential so that's say Dhi p。

 which pushes forward tangent vectors from the tangent space at point p to equivalent space RD and you pull it back by taking phi inverse so each tangent vector in the basis of the tangent space can be represented by pulling back through phi inverse。

 the equivalent Rd vectors， and that gives you a tangent basis。

 so as a result we can take the vector V in the tangent space and rewrite it using the coordinate system of the tangent space。

Right。I told you how to define tangent vectors using the local coordinate system。

 but I also said that extrinsic parameterizations of the manifold may be favorable for some computation。

 so the natural question is that can we exploit this extrinic parameterization to define tangent vectors in and the ambient space and the answer is yes。

 and the way we do that is to say that oh you can always embed your manifold in a higher dimensional ambient space and because we did it through this inclusion map which tells you for every point there's the corresponding point in the ambient space。

 we can also pull back the coordinate system of the ambient space back sorry from the manifold back into the ambient space and the way we do that is to say that oh we'll take the phi inverse map which is from the chart and then we compose it with the Yoda and as a result we take the local basis vectors EI and we just add it the Yoda and phi inverse composition and we have an equivalent way of representing tangent vectors but with the coordinate system of the ambient。

SpaceSo this is super powerful and super cool because you can kind of essentially do all your computation in the ambient space using the basis vectors of the ambient space。

😊，So this gives you a natural question which is how should we parameterize your velocity fields right so our velocity fields is kind of what we care about when we're doing flow matching and equivalently for diffusion。

 how should we parameterize them？So one direct option is to say。

 well'll output directly to the local chart so that we'll say the output of our network will be directly on the tan space at a point P。

😡，Or alternatively， we can output them directly in extrinsic bases。

 but the extrinsic basis because they're built from the tangent bases。

 they will automatically be on the tangent space correctly。

 or finally we can kind of take a velocity field in the amient space and project it to the tangent space if you have a projection matrix P。

So in pictures it looks like this， so we have our local parameterization and then our extnic parameterization where we have to change the basis vectors using the basis vectors of the ambient space or alternatively we output essentially in the ambient space without carrying where the manifold is as long as we have a projection matrix to project it back to the tend space at a point P so for certain manifolds like the sphere。

 we often have this enclosed form， but for many manifolds this could be too difficult to compute too expensive or not available in closedsed form and as a result it really depends on which type of manifolds you're working with when determining which method you want to use in practice so for the sphere this is really easy to compute but for another manifold we may not have this。

😊，So now that we've kind of talked about tightening spaces as well as smooth manifolds。

 what other structure do we need to actually invoke or kind of build flow matching and diffusion models？

So the first thing is to realize when we're trying to build flow matching。

 the loss function required us to compare vector fields or velocity fields rather。

 and in this example over here we use the two norm。

 but the problem is in manifolds we don't have an equivalent version of the norm yet so we don't know how to compute this quantity so we need to invoke additional structure to enable us to compute this quantity and more importantly we don't even know how to compute Xt so Xt is this point along the straight line path we had in Euclidedan space which is telling us the particle that's moving from times zero to time1。

😡，So the main structure that we need to add is this notion of a Romanmanian metric and this metric is going to be super useful to us because it allows us to actually define an equivalent norm on the manifold as well as define distances and other useful properties so more formally a Romanmanian metric is a selection or an assignment of inner products that varies smoothly on the tangon space so this inner product consumes two tendon vectors and it' defined on a particular point P and it very smoothly at every point on the manifold。

😊，And this is a very important geometric structure because for spheres。

 the way you define this sphere is to say that every inner product。

W two ti vectors gives you a positively curved quantity。

 So1 over k is essentially the curvature constant of a sphere。

And it's a constant curvature manifold because no matter where you are on the sphere。

 the metric is the same。😡，So I just want to reiterate one more time that choosing our metric is a choice。

 so we've decided to add this choice because we wanted to be able to compute this loss function over here and if we were trying to do other geometric deco learninging operations。

 maybe we don't need to make this choice and more importantly you can equip a different metric for a different space and it's a choice based on the modeler so we could have picked a different Rimani metric on a space and we would have had a different modeling problem and sometimes you choose a metric based on convenience rather than a prescription。

😡，So having chosen a metric， a remaining manifold is formerly a space， a smooth manifold M。

Equipped with a metric G， and this metric G is defined everywhere on the manifold。😡。

So common examples of remaining manifolds are of course the Euclidean space。

 then the metric of the Euclidean space is just one the sphere where the metric is1 over k at every point and hyperbolic spaces which is the foil to a sphere where the metric is negative one over K so here's an example of a hyperbolic space so over here is the hyperbollloid model of hyperbolic geometry there's more than one model hyperbolic geometry。

 but you can see that now this space is negatively curved rather than positively curved which is the sphere。

😊，So why are metrics kind of important so Romanmanian metric is important because as I will explain to you in a second。

 they enable us to compute a lot of important quantities so we can compute length of vectors which is the norm。

 we can compute distances and we can compute angle and again the metric it consumes two vectors on the tangent space so UVV and to actually use this in practice it's just u transpose G where G is the matrix representation of the Romanian metric so G is a positive 70ite a positive definite matrix and it's going to be different for each point on the tangon space or each point on the manifold rather。

😊，So how do we actually use a metric to define norms so the way we define a norm formally is a square root of the inner product between the same vector twice。

 so U U inner product square root is formerly the norm of a vector and it measures the length of the vector。

Similarly， you can define angles as follows， you can say cost theta when you're in a tangent space is the inner product of two tangent vectors UVV divided by their norm。

And once you have this norm， we can now actually define the loss function for flow matching a bit more crisply because what changes between the top one and the bottom one is this addition of this metric。

 and now the norm is computed with respect to the metric。😡。

So now we can actually compare or and compute this loss。Or the norm for this loss rather？

Now so the last thing we need to introduce is that well how do you measure distances on a manifold。

 the distances the way you do that is if you pick two points between the manifold。

 so let's say points P and Q we want to find a curve gamma that passes through P&q and is the shortest path between P and Q。

And the curve gamma that is the shortest path between P andq is called the geodesic。

 and it's also the straightest path in the manifold sense。😡。

So the main idea to actually construct the distance is to say that oh。

 we'll pick a curve or we'll take a parametric curve and we'll measure the length of the vector as we traverse the curve。

 so we'll measure the norm from times0 to1 as we compute the tangent vector to this curve as we traverse from p to Q and the idea is that this because we have a metric we can measure the norm or the length of the vector as we traverse the entire parametric curve and the distance on the manifold is defined as the curve that minimizes this integral。

😡，So it's a very natural definition of a distance。So a couple of facts about this distance is that it is actually the shortest path like I mentioned。

 and the shortest path is called a geodiic， it is also the straightest path in the sense that it is the straightest with respect to the metric。

And because it's the greatestest path， it's also the most kineticically optimal path。

 so traversing this path will give you the lowest kinetic energy and if you pick any other curve between P and Q。

 you will necessarily incur a higher kinetic energy or more energy needs to be used to traverse this path。

So sometimes it's a bit confusing to think about geodsics。

 but it's really easy when you actually visualize them so over here what I have is a visualization of the probability simplex so the probability simplex is essentially you have a manifold where each point is a categorical distribution inside of the interior the simplex and I want to essentially draw a path between this blue dot and this orange dot and each point is along this path is going to be another categorical distribution so if I draw a linear path you can see the line is straight but if I change the metric on the space to another metric which is called the fiser row metric you can see the path essentially bends and this is the bend path is now the shortest path with respect to a different metric and this metric allows you to kind of bend these paths。

😊，And the main thing I want to highlight here is that different metrics induce different geodesics。

 so you can have the same space but by choosing a different metric。

 you've changed the geometry of the problem。So the Romanian metric is really a very important gadget。

Thank。Okay， so because we pick the Romanmanian metric， we can compute distances。

 we can compute angles， we can compute norms， what does that actually allow us to do？😡。

So if you recall in the flow matching equation， we had to compute this loss function。

 which is the L regression of the target velocity field with the one that you're learning。Also。

 because we can compute distances， we can now compute an equivalence between these two objectives。

 which is to say that we can use the X1 prediction or target prediction framework and say that we would just want to minimize the distance between the predicted endpoint and the actual ground truth endpoint。

 so this is a completely equivalent loss function to the first one except that it uses a different parameterization。

 so your network over here would predict the endpoint rather than the velocity at an intermediate step。

Similarly， for diffusion， we wanted to predict kind of the score function。

 and we want to regret to the score function so we can do that now because we have essentially the same norm。

And as a result， we can also compute the equivalent of target prediction for diffusion models。

 which is called epsilon prediction， we want to predict the noise added to your sample。😊。

And I remind you， all of this is connected in the Euclide setting with an ODE and an SD。😊，However。

 one thing that's still not clear to us so far is how do we actually integrate on the manifold because we don't know how to simulate this OD and including spaces and the reason we don't know how to simulate it is because again the manifold by itself doesn't have a vector space structure so we don't know how to do an oiler step yet or oiler marama step for SDs。

So to make this a bit more crisp。When you simulate flows and diffusion models and euclide in settings。

 the actual order integration is telling you， oh， the t plus one step is xt plus the velocity prediction times sometime delta。

 so we just move along this path for time delta T and we get to the next point xT plus1。

 similarly for diffusion models， we have a similar prediction where you move along this direction which is controlled by your score function。

 plus you add a little bit of browning motion and then you just keep going along this path。Now。

 what bricks for manifold？And you need the next point to be guaranteed to be on the manifold because if you just arbitrarily add two things。

 you will not be on the manifold。😡，And finally for the SD use case。

 the hard thing is how do you define Browning motion on a manifold because at the very beginning of this I told you there is no canonical definition of the Gaussian distribution and even though there's always a way to define Brownian motion。

 it may not be easy to compute in closed form and for many manifolds of interest this is actually a very tricky object to compute。

😊，All right so to compute the actual order integration or equivalent of integration we need to introduce a couple more manifold operations so the first thing we' have to introduce is like how do we move from a point on the manifold back to a tangent space and how do we move from the manifold to a space and vice versa and finally how do you move vectors between tangent spaces so the first operation is the exponential map will take a vector and find the corresponding vector on the manifold corresponding point on the manifold。

😊，And the logarithic map is the inverse of the exponential map where you take a point of the manifold and you bring it back to nicovalentton space。

 and finally the last operation is called parallel transport。

 which tells you if you move a vector from one tendon space to another。

 what is the curve that has to follow so that you don't change the vector in some sense。

So the first operation as I mentioned is an exponential map which tells you if you give me a tangent vector at a point P。

 how do I find the corresponding point on the manifold if I do an operation。

 so in pictures exponential map is very simple， it says that oh if I started at a point P and my instantaneous velocity is V and if I find the curve gamma that corresponds to this velocity vector。

😡，Where will this curve take me if I follow this curve for time one。

 so if I follow this curve in the direction of V for time one。

 wheres the point on the manifold that I will land？😡。

And the output of the exponential map as a result of this is a point on the manifold。

 and we travel a unit of time along this curve gamma。

 and in conceptually this is kind of similar to addition in Euclidean space。😊，So for spher geometry。

 the close form expression of one exponential map is this nasty looking term over here。

 so already you can see that computing this is a bit more complicated and involved compared to just adding in Euclidean spaces and this operation will of course change depending on which manifold you're in。

😊，Now the inverse of the exponential map for most cases is logarithmic map and the logarithmic map is exactly the same idea。

 but in reverse， yeah。So there's a question in the audience。Yes。

That are very for all the people doing that。It's it can be expensive yes oh sorry， me Jo。

 can you also repeat the question yes of course。Let me just go back to the slide。

 but the question was the operation and the expression at the bottom of the sphere isn't expensive to compute all points。

So in this case of the sphere， it's not super expensive because it's just like cosine。

 but for like other manis like hyperbolic spaces， what will become tricky is that this will become inverse cosine and it will become numerically unstable near the boundaries of the manifold。

 so this is a very good question。😊，は。Sorry， can you speak up？对。啊こで。真的题。嗯。但 basis事也不用。But that心里。

Consider a lot of point that integrated take。So the question was is this for every P on the manifold so this exponential map is defined for every like it's an injective map yeah so it's defined for every point on the manifold every point on the remaining manfold will have a corresponding exponential map and most of the time it will also have a corresponding logarithic map。

没听见啊。So thanks for the question so the logarithic map is the inverse of the exponential map and it's well defined only in a local neighborhood of the exponential map where this becomes a deffmorphism and the idea is the following if you start at a point Q and if I was to follow the curve backwards in time which is the corresponding vector V I would end up in right so this is the intuitive definition of a logarithic map and the expression below is the logarithic map for sphere so you can see the expression is very different than the one for exponential map and it's because it's doing something completely different。

😊，So the last operation I want to introduce is this notion of parallel transport。

Oh sorry this should say parallel transport not log map but the parallel transport the idea is is the following if I have two tangent spaces at P and Q and I want to move the vector V from one tangent space at P to another tangent space at Q how do I move it so I don't essentially change the vector in some sort and I'm being intentionally vague about what I mean by change？

But the idea is that the inner product between V and W， if you have two vectors over here。😡。

If you take this inner product at every point along the curve it's going to be the change it's going to be the same rather so there's no change in the inner product so the metric remains the same so this is the corresponding vector V that's transported along this curve gamma and hits the tan space at Q and parallel transport is a unique operation and it's also reversible which means that you can take inverse parallel transport which takes you from a point on Q or a vector on  Q and transports you back to a vector on P along the same curve。

😡，Right， so these manifold operations seem a bit esoteric and it's unclear why we need them and I'll try to tie it back into the generative modeling use case。

 which is to say that， well the target velocity in the Euclidean case was just simply UT of x given Z。

 but if you write the actual expression for this it's the time derivative at Xt。

 so this is a particular choice of the path you choose and this is essentially telling you if I pick a straight line between x0 and x1。

 what is the velocity if I was traveling at linear speed between x0 and x1 at a particular point？

So really the straight line is a judiic and Euclidean sense。

 so how do we elevate this in the Romanian sense so in the Romanian sense。

 we have a corresponding expression， which is to say that。

We want to take a log map at a point along the juDistic。

 so logM remember it takes you to a point on the tangent space。

 so this is now a1ant vector and you want to divide it by1 over1 minus t rather。

 and that tells you how fast you need to go。So again。

 straight line in Euc the Euclidean spaces is a judiic in the remaining sense。

 we have a corresponding expression as well。So how would you actually calculate this in practice so one example is for the manifold S3 so S3 is the group of 3D rotations。

 so it's orthogonal matrices in 3D。And the logarithtic map for SO3 happens to be the matrix algorithm and inside the matrix algorithm the input is a rotation matrix so how do we take the rotation matrix and transform it back to its corresponding tangent vector unfortunately if you compute the logarithtic map naively you have to compute this infinite power series which is truncated with end steps and as a result this is a very expensive operation to compute so even though mathematically it's very easy to define what this is in practice this could be very difficult。

😊，And this is a place where from a machine learning point of view。

 you have to make a very conscientious design decision， which is to say that， oh。

I can change the representation of my input so instead of representing 3D rotations as a rotation matrix。

 I can change it to another representation， which is the rotation angle representation and the rotation angle representation is just simply this expression over here。

 which tells you the corresponding angle and the radius in the direction you have to so a vectors omega and the axis of rotation around mega。

😊，And as a result， this is a completely different representation of the same object。

 but the logarithmic map for this representation is much easier to compute。

 you don't have to deal with an infinite power series of matrix powers。😊，And in practice。

 when we do implement things in SO3， we prefer this operation because it's much faster to compute。😊。

All right， so now that we kind of develop intuition for how to do the manifold operations。

 we can now actually formally define how to do inferences on manifold so intuitively the easy expression that changes is。

Instead of doing addition， we do exponential map at Xt so we compute everything in the tangent space。

 our UT is a tangent vector multiplied it by Delta T。

 and then we take the exponential map to get to T plus1。😡，Similarly for diffusion models。

 we also take the exponential map of the term inside the expression over here。

 and the other thing to note is that the Z T previously was a regular normal distribution。

 this is not a tangent normal distribution， but this is a normal distribution and the tangent space at point P。

😊，And remember， the tendon space is also an Euclidean space。Equivalent to Euclidean normal。Right。

 so the one thing that we still need to talk about is for diffusion models specifically。

 we need to be able to compute the score function and the score function is this gradient of px or PT of x。

 but we still haven't defined how to compute the gradient on a manifold。

 so this is one of the last pieces we need to actually complete the story。😊。

So the way we compute gradient is that we want to elevate the gradient operation to be a Romanmanian gradient and the Romanmanian gradient has a very similar interpretation to Euclidean gradient。

 so if you give me a smooth function on the manifold。

 the Romanmanian gradient is the direction of steepest ascent critically with respect to the metric。

 so using the metric G so the formal definition is that we want to find the gradient the Romanmanian gradient to be nabla G of F inner product with a vector v such that it is equal to the differential at that point f of the function f by。

 p。And the vector that satisfies this is the unique vector nage， which is at the tang space at M。

So in Euclidean spaces， the equivalent understanding would be it's the directional derivative。

However。This definitionThis definition is a bit abstract。

 so how do we actually use this definition in practice so when you want to express the remaining ingredient gradient in coordinates operationally it just means that you take the matrix representation of the matrix of the metric。

 compute its inverse and multiply it by the Euclidean gradient。😊。

So this gives you an operational tool of how to actually use the remaining gradient for actual computations。

Because everything about the geometry of the problem is encoded in this matrix representation of the matrix metric G。

Similarly， we might want to compute the log density at a point P so a lot of the times one of the benefits of using flow mapping models is that you can compute the change of variables or the change of log density along the trajectory。

 but to do this we need to define an equivalent version of the divergence operation so just like there's a Euclidean divergence operation there's a Romanmanian equivalent of the divergence operation。

😊，And the main idea is very simple， so the divergence of a vector field tells you how much of the vector field is a source or a sink at a point。

 so similarly we can compute the divergence just by invoking the metric as follows so in a coordinate chart。

 you can essentially take the square root of the determinant G which is the metric。

 and then all you have to do is like correct for it when you compute the term inside the sum by square rootG and we do it component wise for each of the different components of the vector field。

😊，So what would it look like for Euclidean spaces again。

 so remember the metric for Euclidean spaces is just identity so this term would be one and this term would be one over here and then you recover the Euclidean divrgence operation。

So yeah， and machine learning， we prefer to do compute this divergence in a chart because it's a very simple operation to just code up。

All right， so the summary so far is that when we think about building deep learning on manifolds specifically。

 we need to take care of certain things， the first thing is that each manifold requires different design considerations。

 the first tip when we think about this is that we want to pick a parameter transition of the manifold that makes it as close as possible to a Euclidean problem and the reason for this is that we know how to do things in Euclidean space really well and we want to kind of like be as close to a Euclidean setting so that all out of our deep learning pipeline works。

And then the other thing you want to be careful about is that certain manifold operations may be inherently numerically unstable。

 especially close to the boundary for manifold， so you might want to reparamettize or pick a different geometry where such a problem may not persist。

😊，And the final I guess tip is that at least for manifold use cases。

 diffusion seems harder to do than blow matching and the reason for this is that well we have to compute the score which of course is a mining gradient and for diffusion you have to also define browning motion on a manifold and that's not so trivial to do for certain manifolds so the real question you have to ask yourself is that when you're doing geneative modeling on manifolds do you really need to do an SD or can you get away with just an ODE and most of the time an ODE will give you more bend for your buck。

And finally there is no canonical Gaussian distribution on a manifold。

 so choosing the prior is also a design decision， many people choose a uniform prior over the entire manifold because it's easy to do。

 but of course full matching is compatible with any prior so you can pick any source distribution and go to any target distribution。

 which is again not possible for Gaussian sorry for diffusion models on manifolds because you have to pick a Gaussian like prior。

😊，So I'll take a couple of questions now before we go to live coding and as we're going to live coding。

 I want to quickly recommend a couple of Python packages that kind of help you know。

 allow you to do these geometric computations a lot more simply。So I actually had a question。😊，Go it？

Yeah， in your experience， is there sort of problems for which the intrinsic representation actually helps compared to the extrinsic one？

Yeah so that's a very good question so the intrinsic representation helps whenever it's hard for you to find a projection from the extrinsic representation so a lot of the time the projection operator could be very nasty to compute or could be expensive or you may not have it in post form and in those cases you prefer an intrinsic representation。

😊，So see， thank。But that's a good question yeah and unfortunately like this answer varies from manifold to manifold。

Any other questions before we jump to coding？

![](img/25574de0c5f6776e4df6b4107d6f5327_18.png)

Not anything that I see in the chat， but yeah， we can go ahead。Let's try to code some things。

 let's try to connect the runtime。Okay， let's see all right will。Better yeah， all right。

 so we're going to try to code something very simple just to give you some intuition for some of the things we've talked about。

 And the first thing is that。We're going to code up。

Because we talk a lot about it with geometryomeranosphere。

We'm going to try to visualize some of the geometric concepts on this sphere。

So the first thing is that we just import a bunch of these packages。And if it cooperates。

We will continue。Okay， so it's running now， we'll also use the geoOt package because this is the one package that has a lot of the nice manifold operations that's already written for you and it's extremely simple to use almost stupidly simple so I want to kind of illustrate the beauty of this package and it's not my package。

😊，All right， so as this is running， so we're going to take the first example to be a sphere embedded in RD plus one。

 so this is an extrisic representation， so by definition。

 every point in RD plus one whose norm is equal to one can be thought of as living on the unit sphere。

😊，And to do that we're first going to code up the inner product in the sphere so the inner product on a sphere is essentially the Romanmanian metric so we're going to code up the Romanmanian metric on a sphere and the first thing we do is that the Romanmani metric will take in two points so X and y so X and y are both tang vectors and we want to compute an inner product with them so the first thing I will do is to check that X and y are actually valid points and then next I will do is because we're embedding the sphere and Rd plus1 we can use the metric of the ambient space as well so this means that the inner product here is extremely simple and already I see that wholepit is helping me out over here。

😊，But it's essentially essentially essentially the regular inner product on Euclidean space where you do x times y and you sum across the dimensions。

 and this is essentially what we're going to implement。

So it's stupidly simple and the reason it's stupidly simple is because we've chosen to embed the sphere in RD plus one。

 the ambient space。😊，If we had chosen an intrinsic parameterization。

 we would have to be a bit more careful。As a result of this。

 the norm monoossphere can just exploit the Romanian metric。

 So remember the norm is the square root of the inner product of the vector。

 So how will we do it so the norm is。Product is， so we'll use the product inner product function that we've defined earlier。

So spear inner product。X， Y， and then。The norm is going to be the square root of this inner product。

And that's simply the norm induced by the Romanmanian metric。Finally， as I mentioned earlier。

 this manifold is embedded in R D plus one， so we need to be able to project to the manifold So any point on the ambient space can be projected to the manifold by simply taking the the。

😊，Norm of the sphere， so actually I'll just code it up like this。Stop Nor。Yeah。呃。So yeah。

 so at every point divided by its norm， plus some small epsilon to ensure numerical stability just allows you to project to the sphere directly。

So while this runs。Now another equally valid parameter transition of the sphere is in the polar coordinate system。

 which is we want to pick two angles theta n phi， and this allows you to define any point of the sphere by taking a radius r away from the sphere。

 and then using the angles data n phi so if you want to convert between this polar coordinates to an xy Z coordinate system。

 what we would do is say that the first coordinate x is sine theta cos phi。

 the second coordinate is sine theta sine phi and the Z coordinate is co datata。And as a result。

 we can actually use this coordinate system to visualize things on the sphere。

 So I've already written a function that does this for me， so I won't code it up from scratch。

 but essentially。😊，What it does is that it plots this shaded version of a sphere or unit sphere for you。

😡，Okay。Okay， so now that we've done that， we can also see the difference between this intrinsic coordinate system versus an extrinsic coordinate system。

 so I have another function over here which essentially tries to visualize the sphere by generating a lot of random points and then projecting to the sphere by using dividing by its norm。

😊，And as a result。You will see now it gives you an equally valid parameter position of the sphere。

 So each of these points is not on the sphere， as you can see。

 So I picked the random generated a random set of thousand points and then divided by a norm。

 and I'm on the hypersphere。😊，Okay。So the next thing we were going to talk about is a different manifold operation so there's the exponential map logorithic map and parallel transport each of these are a bit more complicated so it's easier to code this up with the package geoopt so for the packet geo optt what I've already done for you is kind of define this class sphere and as you can see the exponential map is simply self- X map where the first argument is the point on the sphere and and second argument is the velocity you're following to reach the endpoint of the exponential map and correspondingly we have the log map we have the distance and we have the parallel transport on the sphere so the beauty of this package is that instead of coding up these equations from scratch we have simple one line expressions that give you the entire computation for free。

😊，Okay， so given that we have the Xmap logmap and parallel transport。

 I want to define a geodic interpoent on this sphere。

 so the judic interpoent is the actual interpoent that we will use to do fl matching。

 so we need to be able to compute this which is essentially if you give me two points。

 let's say x0 and x1， what is the equivalent point Xt if I follow this this path。

So the Ju to signal interpot， I mean， it's quite simple to code up because we've defined our function before and it's really。

So we wanted to find X T， and we were just going to brutally copy this expression over here。

 so the expression says。We want to use the Spear X map。😡，At x。 x0， then we multiply by time t。

Of the Spear log map also at x0 of a0 x1。😡，So what is this saying。

 we're saying that we're starting at a point x1。And then we want to move to x0。Along trajectory。

 the judi that links them together for time T。And that's how you're going to get XT。Okay。

 so let's actually try to visualize as youistic so I have。Two points on the hyperphersphere。

 so these two are red dots， and now I want to compute the judaic between them。

Which is going to be a curved path between these two points。

Right so to compute the ju between these two points。

 what I will do is say that I will take a discretization of time， so I will take 200 steps between 0。

01 and 0。99 and I will compute the trajectory， which is to say that I'll compute the point X T at every point。

🤧嗯。So。Quite simply because we have our Judaic interpent。We will consume points zero on one。

 give it a time parameter and compute the trajectory and attach it to the trajectory。

So if you do this。We should get a curved path between these two points。

 and this path is the shortest path between these two points traversing this field。

So very simple to do and if I was seeing a flow matching model。

 what I would do is I would take the time derivative of this path of a particle that's moving along this path。

So that was sphere in the time remaining I also want to expose you to another geometry which is not too different than the sphere。

 which is toroidal geometry， so it's going to be a taus in 3D。

 so a tous in 3D is very similar to a sphere but it's geometrically quite different。

 so we have two angles again， so theta n phi and we have two different raddis are in like a minor radius R and a major radius capital R and a taus is essentially the product manifold of two circles。

😊，If you take the product manifold of two circles， you have essentially what's a Taurus。

And to do that， we can kind of just generate some tourist data， so I've created a tourist for you。😊。

And it essentially looks like this， so this is quite a fact tous and I' generate random points on this taus and I'm going to do the same trick as before。

 which is to say that oh， I want to visualize geodiics on a taus。

 so what does the corresponding geodiics on a Tarus look like？😡，嗯。Yeah， so the tourist is。诶。

So we're going to use the geoOt package。And the tourrus is essentially going to be a cross product between the spherical manifolds as well as。

Oops， this should be。2。So。Yeah。Oh correct one second。Yeah。

 so it's a product manifold between two S1 circles and we're going to make a judiic interline again。

So the Ju ecosystem interpoent is essentially the same idea as before。

 but the thing that we changed compared to the sphere is that we just change the manifold。

 but the rest of the operations remain the same so X map， T times log map。

 everything else is the same。So。As you can see over here， when you do this。

 you have a corresponding geoistic that takes you from this point over here and wraps around the touristus and gets you to this point over here。

 the same idea， same thing and that's kind of the beauty of the geoO package is that it abstracts away all the actual nasty analytic expressions and gives you a very clean interface to code this up。

嗯。The last thing I want to mention is that the tutorial also contains a very detailed example of hyperbolic geometry that I will not go through because it's a lot more tricky to explain。

 but I invite you to try it yourself there's a lot more cool things over here such as the hyperbolic inner product and finally also the equivalent of Gaussian distributions if I run everything。

 so equivalent of defining a Gaussian distribution on hyperbolic space and how that changes based on the geometry of the problem。

So that's kind of where I want to leave you all， so I will take any further questions if there are for this part of the tutorial。

 if not I'll pass it on to Alex afterwards。So there is a question in the audience。Could you please？

Tell me which part of the code you were talking about。

I'm actually not sure if the audience is allowed to unmute themselves to ask question， but they。😊。

I think the question is other cases other than SO3。

 where the matrix representations have less issues on the log length P map。

So S3 is a broader class than what I described， so S with3 is specifically a lead group and for all lead groups。

 the logarithic map is the matrix logarithm， so you will have issues competing the matrix logarithm for any lead group。

So not just SO3， any SN， ON， and other stuff。But that's a good question。

There's another one that the Euclidean geometorries are preferred because we have better intuition。

 but our spherical and harmonical systems also well understood。

 but I'm also not sure what harmical means here。诶。Yeah。

 I'm not sure about the question but I think the euccleidein geometry is preferred because of the numerical stability it offers so if in those systems like harmonical there's equivalent numerical stability I would imagine it's okay but my intuition tells me it won't be so actually they meant hyperbolic systems yeah so hyperbols yeah so that's a very good question so hyperbolic geometry。

 what I didn't go through and you should definitely try going through this on your own is that we have to do a lot of additional tricks to make things numerically stable so for example you have to clamp more aggressively we have to also have a smaller epsilon value because we were dealing with hyperbolic sine and cosine functions and they blow up really quickly because the reason they blow up is because each hyperbolic signine and cosine function is going to be like E to the x E to the power X so the value of x controls how big the value could be so if x is greater than 40 you cannot。

I said and。Floating point precision。So that's why we have to climb these things。Yeah。

I hope that answers that question。yeah I don't think there are any more questions all right in that case I will pass it on to Alex and Alex。

 if you want to share your screen。

![](img/25574de0c5f6776e4df6b4107d6f5327_20.png)

You turn off your audio。Can hear us？ButYes。Thanks。好嘅事啊。



![](img/25574de0c5f6776e4df6b4107d6f5327_22.png)

![](img/25574de0c5f6776e4df6b4107d6f5327_23.png)

Where did my cur go。嗯。不。

![](img/25574de0c5f6776e4df6b4107d6f5327_25.png)

是。

![](img/25574de0c5f6776e4df6b4107d6f5327_27.png)

Oh， okay嗯。Yeah， so this is the third part where we're talking about how to mash these things together。

 So the geometric part and the general evolve part and well mostly focus on flow matching things。

 because well， I think it's the easiest one。 But we can talk about why that's the case in in。

In a bit， so I'm going to talk about sort of applications and sort of categories of applications where it's sort of easier。

 where it's harder than talk about how to go from theequideian case of flow matching to Romanmani Fl。

And then look at sort of a case study in protein design so in particular afold2 and how they sort of represent these types of manifold in proteins because that I think gives us an interesting idea about what coordinate spaces are useful when intrinsicsic and extrinsic are useful in practice and then flow matching on a tous this is one of the cases in proteins but it's sort of it's very easily to realize we'll go through that one with code and then finally think about sort of practical tips for these types of levels。

Cool yeah so we looked at this slide but basically the first category things are where you have a nice logric map and exponential map and you can parameterize geodesics and everything is fairly easy right so it' these types of nice parameters manifolds are very widely doesn't appear again okay I see yeah so this sort of first categories is I would say most of the focus on geometric generative models it's much easier the second category。

Is on these nonparmetric models So like is where it's data drivenve or not sort of not easy shapes right so it's sort of much harder to parameterize well you know where is the where is the e map what is the geodestic right so these are also very interesting and data driven So you know for instance。

 one of the things I' is in single cell So we know sort of data driven manifolds are quite interesting but much harder to deal with So I think these sort of geometric geneative models we mostly focused on the first case。

 but then I'll sort of give hints on how to do the second thing。嗯。Okay。

 cool so Euclideion to Romanian flowmeing Okay so this is sort of notation reminder Z is sort of the conditioning so we're picking endpoints here so that's a tuple x and x1。

Q is the distribution over x0 is and x1 so most of the time this is independent。

 this is what this says is you pick x0 and x1 independently and then in Eukuadideian the easiest way to do this is just to take a straight line between the two take a convex combination with regard to T and when youre only interpolate and so when you do that then you get a velocity that is very easy which is x1 minus x0 so here you have depicted right you sort of pick your random of points from your distribution and then you pick an xt in the middle and you have a vector field that goess from x to x1 in a unit time。

诶。Okay， so for Romanmanian flow matching， we'll use the same first two parts。

 so we'll take an X0 and x1 in the Romanmani setting and exactly what those objects are。

 it depends on your coordinate system， but here's the sort of X and x1 and then say you take the same independent distribution。

And then the interesting part begins is how do you take XTs right so this subtraction and addition operations are not defined on running manifolds as Joe is saying。

 but instead we have to use the exponential map and the log entry map so this is sort of the formula for XT and I'll give an intuition what this means in a second。

And then for the flow what we have is log x t like it click x1 and and then1 or minus d right so okay so why is this sort of the right thing to do Okay so on a sphere you can look at the interlation between x and x1 Okay so what is this sort of saying right so。

I think of it as well okay， maybe it's easiest to visualize in the Eradian setting so for Euclidean we know that the expent for map is just a plus B and the log good thing map is B minus a。

And so you can rearrange these things right into a form that looks exactly like reminding flow matchingching and so if you take the X man。

 you sort of get the well x0 plus stuff and then you get log map which is t times x1 minus x0 and so this is sort of the same formula right you can rearrange this back into the original sort of con combination and so this is sort of the intuition is basically you're starting in x0 you're adding t times the flow that you would need to get from x0 to x1 and time1 and then that's think get your xt right so sort of move for t amount of time in the direction x1。

And then for the flow， you say， well， okay， I want to go from xt to x1 in time1 minus d。

 and so that's the flow computed with respect to sort of xt and x1。

And so this is how it works in general in specific cases right there's many ways to parameterize these maps so it sort of depends but this is this is the way and so just one thing right is this requires very efficient evaluation of your exponential log group for maps so that's sort of a lot of the tricks are well how do you do this efficiently and well some fault you can't right so if you can't do this。

 what do you do well you have to do something more complicated where you integrate XT over time and then you analytically calculate sort of the the XT dots so the UT here？

So yeah， that's the sort of this is the basics， all you need to know of sort of to go from E into your money flow matching is to go from this first half。

 which is sort of very simple kind of combinations and then use the exponential en by map instead。

Coole， so do I have any questions before moving on this part？有一个。

Okay okay so one right piece that we were thinking before is this sort of log likelihood computation right so in the Eidideian setting it's very useful to figure out the sort of log the log likelihood at a time1 giving your vector field in your prior and so this sort of gives you the calculation but it requires the divergence with respect to de metric and so you can use the Romanmanian divergence to calculate the log likelihood at any time using this sort of formula right so in Eucquaidideian case it simplifies because the determinant G is just one so you get the standard determinant form back and so this sort of all you need is well you can do flowting and then you can calculate log likelihood if you would like using this formula。

Cool okay， so we sort of talked about just how you go from Eonian to remind flow me。

 but how is this sort of useful in practice so I'm going to talk a little bit about protein design and the representations there and then we can look deeper into sort of the different choices we can make。

诶。Okay， so protein design basically we're trying to design proteins which I'll describe in a bit that I sort of satisfy some properties and so that's the to hear why to do this well we proteins can do a lot of interesting things。

 mostly medicines， vaccines maybe for climate change we'll see what the applications are but I'm parents to do this。

But okay so whatmics you want to decide for so there's sort of many objectives and you can think of them as sort of all functions of the structure and a little bit of sequence and so what does that mean right so you sort of there' there's function you want to optimize and then you want to generate structures that satisfy these properties。

Okay so well what is a protein I guess that's the that's the real question Okay so what are proteins right so they are we we sort of name them as sequences because they're sort of repeating patterns of molecules。

There's a lot of them well' there's sort of a lot of combinations right they're built up of this 20 you know I said building blocks so sort of sequences of 20 different v capy size 20 and then you sort of the sequence folds into some structure and that has some function that's the idea and then that that sort of does something and you would like to figure out well let's make a new protein and what does it do。

Okay so but how do we represent proteins so proteins are really these very long strings long molecules that are chain like so is the sort of backbone of that that forms a sequence and the sequence what the sequence really tells you is what the side chain is so what hangs off the ends and so what does that mean right you sort of have this couple of representations one is sort of a cloud of atoms so the first is like you could look at the 3D coordinate of each atom location and so this should be a sequence of length n and there's about 19 atoms per amino acid if you count thehys and three coordinates right so you could represent them in  Eukrainian space and we could in fact do eukine fluact on all of this or are confusion whatever in fact this is actually currently done also this way so weve talked about that in a minute and so。

The other way we're representing this， I think which is very interesting is sort of is a set of reference frames in Se3 so sort of a central location for each amino acid and then a rotation around it and so that tells you sort of the orientation of thatbone and that's the sort of these triangles here so that's the sort of main。

Main pieces and then you also have the side chains and so here you have sort of a series of rotations off of the main backbone and so this is a series of torsion angles right so that's what these little green things are representing here is how rotate are relative to the previous torsion angles。

Okay so so okay， so why would you ever choose this sort of crazy representation Okay so I guess there's there's a couple reasons okay so this is just a different reputation right there's 20 different amino acids you sort have different colors for different types of molecules and different lengths and then the backbones is sort of parameterizing where the C alpha atom is so the main one and the neuroation around it。

嗯。But so why would you want to do this representation Well there's a couple of reasons I think one is that well it's。

I would say it really enables us to use our prior knowledge about how these molecules are structures in particular right we know these bond likes pretty much precisely right so instead of having to figure out exactly how distant these two bonds are。

 you could say， well， I know exactly how far they are away from each other and sort of rotated around and figure out well where should it be relative previous one and not having to clear out in 3D space。

 whether it well is too far away or too close right。

And so that's one reason I guess another is that well this is sort of a bunch of independent components and so this is sort of easier to learn from a machine learning perspective so you could also parameter this for instance as sort of all torsion angles so you could do torsion angles for the background too and then you have a lot a long series of torsion angles for the entire thing and this is a possible urbanization but it's not as good because it's very global right so if you make a small change in one place and it can change things all along the protein so you change a small angle somewhere and it may clash in another place and so this is sort of harder to learn too so this is sort of twofold good right you have very independent but also builds in our prior knowledge about structures。

Oh嗯。Great， so。Okay， so Alpha fold was a model well and helped win the recent chemistry Nobel Prize that goes from sequence to a 3D structure。

We're mostly going to be talking about this last part that's the structure model and that's the part with a lot of geometry in it everything else doesn't really have much geometry it's mostly big embedding spaces either sort of the length of the protein or the length squared of the protein so that's sort of the main processing components but the last part is sort of sort of use the geometry that we know about proteins so that's what builds in the sort of SF3 and SOG space。

And okay， so what does that look like？Let's see okay so so why do we care about this right in terms of the generative modeling framework right so off all two is not really a generative model I would say。

It goes from a sequence to a structure， although you could。

claim it is in some ways because it sort of starts at a structure and then do three steps to refine that so maybe a conditional derivativeative funnel but the real reason that we're interested in this is because almost all of the protein generative models right now use the same structure model as the backbone of the network to sort of think about 3 e space。

嗯。Cool， so okay， so the structure model of World two right it takes in backbone frames and a representation of a protein in sort of a single andeditation and outputs updated backbone frames and then also angles for the side chains。

Right okay so and this is the code for it okay so thinking about this a little bit right we have sort of s is single representations Z is pair we take in sort of these identity ridges and then we slowly update the rigids and then we have a sort of angle reant which takes the current representation and gives you angles back and so at the end you just get angles and updated rids in this very nice form Okay so but we can look at exactly the shapes here right that's sort of interesting so。

😊，The rigidges here are represented as a n residues by seven and the angles are seven by two right so these sort of seem just like magic numbers and I'll explain where these come from in a second。

呃。こ。Okay so why are they calling ridges and they're calling them ridges Riges here are elements of S3 so that's a translation and a rotation。

 and so the seven comes from the translation part being R3 and a rotation part being cartoonernion representing a rotation in SO3。

And so what's interesting here is right is directly saying here's seven coordinates for the sort of the thing on Se3 and output those seven coordinates right so that's sort of the you know treating these as intrinsic parameters of the model you're outputting already directly so the second part is angles so angles right or so2 and so right there this rotations okay so what's interesting right is that there are okay so there's seven rotations here but why is it outputting is extra two dimensions right well it turns out what alpha fold actually y can I see。

Okay， me all get away to this。So。So it actually predicts sort of a point in r2 and then projects it to the unit circle so this is a very interesting parameterization right so that is one choice you can make so and why do they choose it right so they choose to predict a point in just r2 and then predicted the circle instead of say directly predicting a point on the circle this is another choice that you can make right so this sort of gets that well what what what parameterization should you pick for your for your manifold。

And so we'll sort of go over exactly why they might have chosen this in the next month。Okay， so。Yeah。

 so here， right what you do for the angle resonancesonnet is you take in this representation。

It's sort of a residential network to get the projection。

 you divide by the norm and so that all that does is just give you a point on the unit circle。

 which you could always transform into a portion that you would like to。2。Okay。

 so flow matching on a touristus。 Okay， so the touristus is interesting， right。

 So what do we mean by touristus， So this is sort of the one dimensional touristus right Tore is mostly showing that the two dimensional。

 that's how I would tell us and of course。😊，O fold is using seven dimensions right so okay so what is that right parameterization for this kind of thing so there's sort of many choices right so one that's nice is well you could parameterize rotation matrices so rotation around sort of the center coordinates as a two by2 rotation you could also directly parameterize how far you are along the circle in terms of movement in radance or you could do this other thing right which is predict a point in r2 and then project down right so these are really the different questions is well you know which which one should you pick right I guess as the question and I think it depends on the application and we'll go through sort of well which one might be good in what setting。

呃，不给。Yeah， okay， so。Now we will go to sort of coding the torus and also to show you how to code from thequian example that Tiy had to a reponian flow matchingching of the torus and think about the permitation of the tourus and the peritation of the vector field。

Okay。是。It does not give me my cursor。嗯。嗯咯。这。Okay。All right。

 so this is the same notebook that Kelly was showing。So you can see right we have this MLP。To in。

 yes， okay， excellent。Okay， so we have a MLP rights， which and。We have the same model Okay。

 so if youve run this on the log in 2024， well， you get。

He sort of gets distributions changing over time right so that's what we saw a little while ago lets how to transform this and so now we'll see how to use this and get the remodeling setting so okay so in the To flow matching so first thing we're going to do is there's just a little change in the data set right so okay so use just some nice plotting functions。



![](img/25574de0c5f6776e4df6b4107d6f5327_29.png)

This allows us to convert between spaces。Sort of how do you make a surface of the torsus in 3D and then visualize it？

And okay， so this is the part where we start。 So okay。

 so we take the same source distribution that we had。

 we do some sort of multiplication and moving it around to get it in the right places。

 and then we can visualize it。 so same looking log to 2024。

But then we can also visualize on the Taus in 3D right so there's many different pization of the Taurus right so this is sort of a projection on 2D right so this is looking at the intrinsic coordinates in two dimensions and looking at them so this is sort of think about it if you wrap the thing around the top and the bottom that's what you would get。

And this is the visualization in 3D。Where did they go？Okay， I'm somehow deep inside the touristrus。

Yes， well there is a Taus here， but it seems I can't zoom out and trust me there's a log in 2024 here somewhere。

Interested。Is that right？不差不的。Okay。We will zoom out， hopefully。Alex。

 maybe press the how the home button， it should like reset the。The home shaped button， excellent。

 perfect， thank you。Okay， yes， this is excellent Taurus And so yeah okay so yeah。

 there's the 2024 on top and the log on bottom and so I put these here because well sort of that the log is near the bottom of negative pi and than the 2024 is near the top and so there's sort of many different ways right so all we require from flow matching is that they master the distribution at time zero and then actually has other distribution time1 So there's sort of many flow choices we could use and so in fact we can use the  Uquainian flow on the tous right we can use  Ulinian flow in the intrinsic parameters of the tous and still get a valid answer right so this is the first thing we're going to do。

😊，Okay， so I'm going to define a couple useful things here。So this is a symmetric modulus。

 which is we'll use that in sec that's not used yet。

And here's the sort of the this is how you would do it in Euclidean， right。

 so let's define the log map of Euckuidean， right it's x1 minus x。

And then we can calculate X T in the equi setting， right， which would be using that formula in the。



![](img/25574de0c5f6776e4df6b4107d6f5327_31.png)

In the slides we have let see。

![](img/25574de0c5f6776e4df6b4107d6f5327_33.png)

So we have。Yes。And。

![](img/25574de0c5f6776e4df6b4107d6f5327_35.png)

嗯。Okay， so I'm looking at this。From running and flow matching right we're defining XT as。嗯。Well。

 sort of in this case x0 plus t times the log x1 minus x0 so this is log x1 minus x0 and so this is the correct xt same thing as doing theequt setting and the get U T is also the same right so all we do here is just reformingly the sort of the simple this way into something that looks like the log map so that when we change the log map and the exponential then we'll get to towards flow matching so。

So this is exactly the same， so I'll start running this。

And the same parameterization so what is this doing this is basically pretending that we're in 2D right we're just projecting the taus into Eaninian space and then new flow matching and because flow matching doesn't sort of go outside of the Tarus we're just still moving along the Taus but we won't cross this sort of pi to negative pi boundary right so this is sort of interesting is we'll get a flow but we won't sort of go the fastest way we'll go around in terms of the Equainian setting。

诶。So this is running。 Oh， wait， I better。 I'm sorry， it's not running because I。

Did a hacky trick where I just time sleep so that it wouldn't die。Let's see， Yes， okay。

 so I'll try this again。😊，诶。Yes， okay。 so we're running this Yeah。

 so because in the beginning of this thing， we lost the we lost the。The T forward。

 and so we couldn't。Yeah， that was a problem。Okay， so this is running and we'll see see sort of what the Elideian flow matching if you were to do it looks like on the Taus and then we can start coding the the how you do that sort of Armanian flow matching with the XM that sort of correct and does geodesics so this one can sort of cross the boundary of pi pi to negative pi and go around the other way。

哦。Okay， so yeah， so。Oiler integration。It's the same thing。

 we'll also use the same oil integration for this。诶。And then flights。4。嗯。一。

Yeah so this should be done shortly and then we'll talk about the changes you need for the other。

Further tos。Okay， so yeah， well we'll start talking about this Okay so for the tourists。

 what might you need to change。I would say， so the log mapap definitely is going to need to change and then the computation of XT is going to change。

And I think the computation of UT probably it shouldn't need to change and we'll see why that is in the。

You should be done here。 Okay， so yes， so we have the flow now。 So Okay， so this plus the traories。

 right， So we started at the LOg。And then these plots sort of the directories， right。

 but we noticed that the L should sort of go， if it was going the closest way it would go downwards and around sort of the boundary here。



![](img/25574de0c5f6776e4df6b4107d6f5327_37.png)

And yeah， so we can see this in sort of different types of animations。诶。

I guess the interesting one is this 3D on the Taus， so we'll see in this in a second。诶。

the I guess the the basic idea here is well it really doesn't like flows are nice because you can choose whichever one you want and as long as it matches then you're good to go so is this one easiest to learn that's the next question right。

And probably not， but。yes，冇事。诶， o。And this always runs slower when people are watching。冇事啊。嗯。

唔该我这己时间啊。不致。Okay， so yeah， you can see two things here， right， so the animation。

We can see the moving up and then for the Taus。We can see that this sort of goes the long way around。

 so the sort of the inside， there's nothing。But it goes sort of around the outside of the tors here。

Okay， cool， so how do we get to Romanmani inflammation？Okay。

 so for this we're going to use this symmetric modulus function Okay。

 so what is this actually saying and why do we need Okay。

 so our x0 x1 our values between negative pi and pi。

And what we would like to do is sort of go the correct direction around the circle right so we have two points x0 and x1。

And if we do what we currently do。Then。You always go in the same direction right so if this is the boundary between pi and negative pi when we do x1 minus x0。

 that means we will always go in around the circle this way so all the vectors even if you were say here and here you would go around the circle this way right so that would be a very long flow and so what the symmetric muds does so we added here。

🤧Is picks the right way Okay， so well this math is a little bit complicated right。

 but you so think about the case where you sort of you have。

X0 is is like a little bit above zero and x1 is a little bit below zero， so like near negative pi。

Then we get。A little bit so so we add the two together and it's well just about。

Well it's a little bit more than zero right it's sort of like you know two epsilon over two so just epsilon mod2 pi minus and so this this is why you might want to do this is it sort of basically says which direction is closest then points the  error out in the closer directions so you sort have two choices and that goes the right way。

So yeah， that's the first thing we need to do。 so this is logmap which tells you how to get from x0 to x1 as fast as possible on the Tos。

And then the other parts we need to do is add a symmetric modulus here。Which says， well。

 no matter what I want to do， I want to keep in the range of。Pi to negative pi to pi。

And so these are should be the only two changes we need for this and we will start training again。

And so UT doesn't need to change right so what does UT say it says take my current Xt and take my current X1。

 which are both。诶。placelaces locations on the UniICcle and then does a log map to say well which direction is the fastest and then says I should get there in time1 minus t and so effectively construct sort of this vector and then says I would like to get there in some amount of time in the time remaining essentially in one minus t。

Okay， so all this runs。 Okay， so all we need to do to change this function so。

Since we're using these two coordinates， all that needs to change is we need to make sure that it stays within pi negative pi and pi。

And so we just need to add this symmetric modulus here， which says， well， take my sort of XT update。

And well， clip it sort of wrap it around if you need to。

 so if you go outside of above pi or below negative pi then I'll just go back in the circle。Cool。

 so that defines sort of the oil int on the circle instead of on our two。Everything else is the same。

 except we need the wrap。嗯Okay。Next， so this should be finishing up。



![](img/25574de0c5f6776e4df6b4107d6f5327_39.png)

Okay， so it looked like this before where went sort of well。Pretended it was like Equideian space。

 but this one should pretend it's like a Taus and go the shorter way around the Taus。诶。Okay。

 and so here's the updated plot， so you can see the LG now goes down or mostly goes down and around and then back up to the top to the 202。

诶。Yeah， here we can animate it。Hello。And then make a lot of the tourists again。그。Yes， you request。嗯好。

Yeah， that's a good question。Yes， sorry， so Jacob was asking sort of why some of the points go the long way around。

And so that I would say it's because some of the points it's the closer way to go around basically right so if you're going from the very top of the log to the very bottom of the 2424 then it's closer than going sort of around the other way right and so depending on exactly where they are placed than you would go this way right so one of the interesting things is like these seem very strange and maybe you want to do sort of more shorter paths on average so this does not give you sort of optimal transport paths but if you did a pairing that was sort of related to which points were closer overall instead of an independent pairing then you would get paths that went sort of probably all the way all around that way and closer way。

Thats be I guess it's depends on where they are so if you move the log up a little bit。

 then it'll probably shift and half will go each way basically so it sort of mix。呃 cool。Yeah。

 so you have the animation and yeah you can see that some split up and some split down it's kind of interesting to sort of do a bunch of mixing and then they all appear in the 2024 and slowly。

Gain resolution again。Yeah。好。And yeah you can see the trajectories here right they look very different instead of going around the outside。

 most of them go through the inside yeah and the 2024 here looks fairly ugly。

 but I think it's just because it's squished yeah。呃 cool。Okay， let's see。 So yeah there's this Okay。

 so that's the the main thing is is this So I was going to talk a little bit about parameterization So there's a couple of different ways to parameterize these functions right so here we say。



![](img/25574de0c5f6776e4df6b4107d6f5327_41.png)

Let's let's make our network output something in two dimensions right and this is going to be something or we're going to clip it basically between to match the UT right so this what is this this sort of outputting something in the range of the logarithic map of the unit circle so that's saying it should be sort of it's outputting a velocity vector around the circle so it could be either going if it's positive it's going this way if like going this way but it's sort of outputting in the in the tang space on the manfolds which is kind of interesting but you could parameterize it other ways right so you could also parameterize it so that you predicted the X0 for instance right so instead of。



![](img/25574de0c5f6776e4df6b4107d6f5327_43.png)

![](img/25574de0c5f6776e4df6b4107d6f5327_44.png)

Using this as VT， we could say instead that the model outputs x sort of some predicted x1， right？

So if we did this instead and that means well this is sort of target prediction instead and we could do and then we could calculate VT so then VT right would be would be。

But we could use the same get UT function， right， which says how fast would I need to go， right。

 get me the vector that takes me from xt to x1 and time t。

So I can use that function again to say VT should be equal to get UT。

To get me from Xt to X1 pre in team out of time， right？

Okay so this is this is a very small change right but now it's very interesting right because our're model is out putting something on the manifold right so a point on a manifold and now're now we're calculating the vector to get there right so this is a different type of parameterization right it should it's interesting because well oftentimes for different kinds of manifold it's easier to predict something on the manifold rather than the tangon space right so。

This is a common trick is well we have for instance。

 like many models that are good at outputting the object directly。

 but not so many that are good at outputting the tanon space in fact for protein SR3 for instance almost all the models try and predict the sort of nice looking protein predicted x1 and then calculate the velocity to get there in team out of time and so and that's just because of sort of numerics and how do you parameterize these models and leveraging sort of existing work to parameterize and predict sort of molecular shapes or whatever manifold shapes you have。

Yes， so this should work exactly as well， so now you have a VT and this allows you to。You know。

Pterage your model this way and the final thing right would be to change it so that you predict regress against X1 directly and so instead of doing a loss here。

 you can instead do a loss that was on the X1 pre。So。If you did instead x1 pred and x1。

And oftentimes you want a scaling factor here， so it turns out the inian space。

 the right scaling factor is sort of1 over t1 over1 minus t in manifold space it's much less clear oftentimes it's fair unknown but that's sort of a heistic right it's sort of one of these loss weighted things that all of them should converge it doesn't matter although some of them should work much better than others so you can always sort of do this trick where you predict X1 instead and oftentimes it's easier to get losses in this space than others so for sort of the taus it doesn't matter but more complicated samples of life。

Cool。Yeah， so that's what I wanted to say mostly with this example。

 is there any questions on sort of this thing before I move on sort of you know best practices？



![](img/25574de0c5f6776e4df6b4107d6f5327_46.png)

Yeah。小机发觉嗯。呃 ok。

![](img/25574de0c5f6776e4df6b4107d6f5327_48.png)

All right， so yeah， we did this sort of parameterization on the Taurus。P and director Field。Okay。

 so some some ideas right so Econate Verian it's very easy in this simple touristrus space the touristus can right live on many dimensions so we showed a 2D plot a 3D plot but also you could do a 4D for instance right if you perpeize these sort of unit vectors and so if the network for instance outputted sort of 2 by R2 then you could also get four dimensional so sort of many different extrinsic vers of the tous。

Let's see so some parameters are better， I would say for code because well you make fewer errors so yes。

 you could do this rotation matrix and in fact there are some tors that way but oftentimes it's harder。

 some are easier some are more stable and the best one depends on your sort of application so whether intrinsic or estric I would say in Taus mostly people use the middle one and the one on the right I would say the middle one is nice because it's very simple right you output exactly one dimension per tous dimension but it has sort of this discontinuity at negative pi to pi so some people are worried right the network sort of needs to say the same thing is going to happen in a pi negative pi and that's oftentimes a little bit hard to enforce So what's nice about unit vectors is there's no sort of discontinuity right you're doing it there's one higher dimension everything is smooth and so although you do have to output twice as many dimensions right so memory constraints well code。

Like this so I guess this is one of the reasons why we often do one higher dimension is to make it smooth right so sort of you could do in the minimum dimension but oftentimes it's not very smooth and you have a closed metaphorifold and so in fact that's the same thing that's happening in the alpha fold tube right。

Right so SO3 can be represented in many ways， Coernion is one of them and one thing that's nice is that it's very smooth you could also represent them as a thing in a3 the sort of axis angle orientation but this also has a discontinuion so oftentimes it's better in practice you represented in one more dimension everything is smooth than trying to minimize dimensions。

呃ello。Yeah so I think it's careful balance right so S3 can also be represented as rotation matrices and are in in nine elements right but well that's many extra elements so it's sort of a balance on what it takes so sort of smooth but fewer things is probably right but not kind of position。

个。And see how the final thing is vector field parameterization most of the code that I've seen is oftentimes doing this target prediction so you have a network outputs the X1 and that calculates the vector field to get there in a correct amount of time and so this can be equivalent in many senses。

 especially in theequian space both analytically and practically in for spaces it's almost never equivalent。

Analytically， but is often large just as well。Yeah so choosing right。

 this is sort of for whether you do an intrinsic versus extrinsic parameterittization and I think sort of choosing the smooth one is probably best although for well examples is' often easier to do photo to。

哦。Okay， so going sort of practical conclusions， I guess Romanian spaces we mostly prefer flows and that's because diffusions are really hard to find except in very simple settings。

 it's much easier to find paths than diffusions。And this sort of manifold spaces the parameterizable ones。

 which is the ones we've mostly talked about today。

 and then the ones that are not parametermatizable and those are much harder。

As far as we know and finally the per really matters so I can show you an example of what doesn't but generally making it more liquid is good Okay。

 so here's an example of what doesn't work that I was trying to code。Okay。

 so equivalently right you should be able to do this parameterization of Xy and project and so I made this X Y MLP before this but all it does is instead of it output sort of twice the dimensions。

 right？So that's a point at R2 for every dimension of the Taus。

You can normalize it and then get sort of the predicted theta back right and return the vector field and so yes this in theory works but in practice works very poorly and so I think this is sort of a numeric thing right because while you're differentiating your back property our tangent this clip right and this division so you know maybe there's a problem here right so you could definitely do this better right but this is a different sort of parameters for the tous。

Same things apply。But when you do it it just sort of collapseapses right so well the different parameterss definitely matter a lot basically and so you know while many things should work。

 the training is sort of the stable training I think is the hard part and it sort of is a little bit of playing around if figure out what works best I don't think anyone has sort of a general role and exactly how you parameterise these things。

But well， I guess the closest you can get to a general wall。Is like this， making it more Euclidean。

 making it more like means square error is sort of the best we can do。

And so that's the general suggestion， but it depends on the applicant。Okay， so yeah。

 final thing I want to talk about is sort of， well。

 what is there that we don't know how to do and what are sort of the open questions。Okay go right。

 let's go back to this。Okay so one of the big things is sort of do you need the echovariance right so alphaphafold2 has all these echovariant pieces built in but if you look at alpha fold3 it in fact removes them and directly parameterizes the sort of representations in Rn and then the atoms in their coordinate space right so they totally skip this。

And。Well， this is this was sort of a you know， it's a question of whether you need echovaris at scale sort of。

 and I think this is still very open problems and people have opinions either way and you know this is I guess under debate exactly when you would need sort of these types of models especially when you scale up。

Because you can sort of softly enforce sevariance in other ways。Yeah。

Yeah and so this parameterization piece I think is the key of many of the geometric dynamic models。

 how do you sort of parameterize the model in a stable way and learning dynamics nobody really has analyzed carefully on would say' of lots of tricks that people use but nobody says like why this really works or has looked at lost landscapes or anything like this that I know of so this I think is an interesting question about you what sort of parameterizations can we get better at figuring out a pra what sort of parameters you might want to use for your specific problem。

嗯。And so I guess this is getting back to the first part。

 but move that when should you sort of use these tricks versus just you know parameterizing in the ambient space or trying to embed an ambience space if you can and doing it that way。

I guess， you know， it sort of depends if you want sort of this constraint or if you want to softly enforce something potentially how you know how much do you need this to be the case？

And then yeah sort of well Joey's been asking this question for a while that you know what are we going to do after flows and infffuions for this thing so I guess this is a good question is can we get sort of more efficient architectures right so there's some problems with the current models。

 especially in terms of well how do you do inference quickly especially on running manfolds it's very unclear if you can do sort of better than many steps。

And the final thing is sort of these nonparmetable manifolds I think they're very interesting ideas about data driven manifolds。

 but we don't know how to do it very efficiently other than sort of integrating ODs and matching them so still very useful but not as efficient as when you have these nice parameterizations。

诶。Cool， yeah， so that's the end of what I had I guess we're quite early， but so it goes。Thanks。

 any other questions？Yeah。Hi， Alex， there is a question on hisla。Okay， addressreris。

 I can say loud so everyone can hear it Adris says。



![](img/25574de0c5f6776e4df6b4107d6f5327_50.png)

A question from the third coding section of the tutorial that for points on the cut logos of the torus or for any generic manifold。

 how do you determine where these points go， do you make any systematic choice or does any arbitrary choice make sense？

That's a great question。😊，I mean， in this code， we do whatever the modulus of Python does。

 right so we specifically see oh stop sharing screens or I'll study again。呃う。So in this case。

 we literally use the modulus operator Python， I believe that says if you are exactly zero。

 then you stay positive， so it should be。It should be that like anything that is exactly on the border goes towards the positive side。

 but in practice right these are real numbers so it pretty much doesn't matter what you do on the exact boundary。

But there is sort of this problem in that you parameter ci the manifold and it should in theory。

 if it learns correctly， so do something very smooth and lip shits on the border。

 but in practice of course you can look at this and this is not always the case right？Yeah。

 could be better peruts。Maybe I can a bit about this issue so so since you have these points like the cut logo of the of the Tourrus so your conditional flow would be would have a discontinuity on this points of the cut logo so in general this。

Viols the conditions that you need on the velocity field for everything to be well defined。

 but in practice since we're using neural networks which do which are smooth typically if you use a smooth activation function then。

It kind of averages out in the learning and in the smoothness of the network。

 so that's like another point to add about discontinuities。😊。

I mean you could treat this thing by zeroing out the velocity field around these point and smoothing out the behavior around there。

 but I think that in practice I don't think that anyone has seen like a critical effect of this because these typically are zero measure events。

 so they don't affect much the outcome of the gene model。这。有。P。



![](img/25574de0c5f6776e4df6b4107d6f5327_52.png)

Yeah he says thanks for the answer， does anyone else have any question you can ask on the QA on Zoom in the chat Vol soon as luck。

 so we are tracking questions then。They do follow up questions for Holly based on the previous question。

 does this also applies for singularities？What do you mean by singularities。

 maybe you can write in this slide right it's a telephone game。😊，Because they are。

Liistening to the talk， what anyway say。In the meantime someone asked for the code and the slides。

 I think Jo already sent it in the tutorial as like， but they will be also available。

 I guess in your website。Y， yeah。So。So it will be publicly available。走嗯拜。Yeah。

 address is answering right now， so in the in the follow up。😀嗯，呵呵。😊，嗯。



![](img/25574de0c5f6776e4df6b4107d6f5327_54.png)

Okay， have the。 Okay， you have access to it。 Okay， okay， okay。Can you see this Hol Yeah， yeah。

 I can see it Yeah， we can see it yeah。So I guess these are similar issues to what happened in the Tos。

 for example， because I mean on the sphere， that's where the example of the herable theorem is that you cannot have a velocity field that does not zero out at any point。

So for this specific instance of the sphere， I think you could also do something like what I suggested previously of smoothing out your conditional flow around the singularity points。

Which are in this case would be like the cut locus on the sphere would be the antipoal point for the next one point that you condition right to you would have an issue only on the antipoal point。

So then again。😊，You could you know， it's a bit artificial but modify your conditional flow so that an epsilon neighborhood of that singular point will not flow to x1 so it's like we would also have a nonzero density on the sphere because the current way that what it does is like imagine that you start from the antipo point and you just kind of shave the sphere all to x1 so we actually have areas where the conditional flow is not defined and also theres zero density conditional。

So you could smooth it out and again i'm not sure how much of a practical relevance it has， but for。

But maybe， maybe it does incur some issues。Does it answer your question， And？嗯你。😊，IThink it does。

Yeah。That's funny setting。く。Great。😊，🤧嗯。Great。All right。Right， so。



![](img/25574de0c5f6776e4df6b4107d6f5327_56.png)

I think if there is no further questions。Yeah， we would like in the name on behalf of the organizer just to thank all of you to the tutorial organizers like Joy Heley。

 Alexander， the three of you。Or like a nice tutorial， which is like three words long。

 but we know it's a lot， but there was different parts， it was self contained。

 easy to follow and also with a lot of code， but I think that it's also really。Valuable in the field。

 we would like also to remind the people that there is a feedback form。

For the tutorials that I will send it right now in the channels like here in the chat。

 and it helps a lot both the tutorial organizers and also the meetH organizers to know how we can improve like the experience。

 especially this virtual experience。Yeah， thank you very much to all of you。Thanks。Thank you。Yeah。

 and we will come back in 15 minutes， like in the clock time in a bit more than 15 minutes with the next keynote session by Saakri Lucy。

 so maybe we can have just a break right now for 15，20 minutes and come back six well well。

 I in Europe it was six so in 20 minutes。Thank you very much。

 all of you for attending the conference， Bye bye。明拜。Hello。No you cant hear me right， okay。

 sorry I had some problems。Yeah， I was saying that the。Yeah thank you very much all for coming。

 like welcome Professor Sachri and thank you very much for being here and we are very pleased to have you here in luck having a keynote。

😊，Just aign note before the keynote we have horror presentations and also poster presentation so just we have a long pro line ahead yeah with no further ado I will present Professor Sakari so Professor Sakari is a research scientist the Meta。

 the third team and he's a gen Profesor the chemical engineer at CMU。



![](img/25574de0c5f6776e4df6b4107d6f5327_58.png)

![](img/25574de0c5f6776e4df6b4107d6f5327_59.png)

He also appeared at Z at MIT and apostoc Kist Stan for。

 and the work that Professor Sakri does is an AI for chemistry and climateimate applications model to understand and design nanoscale interface is usually using machine learning。

Some examples are computational methods for catlysis as we can see and machine learning models to pick their properties or active learning methods to guide the systems so with no further ado I will leave the states to Professor Saakri so thank you very much。

😊，Super， thanks so much for the invitation。 And it's also fun to be here。

 I know a bunch of the students who worked with me at Carnegie Mill and really appreciated the log conferences and the mentorship and other things for some of the projects。

😊，Um， so。Today I wanted to talk about a couple of recent data sets and efforts that we've released openly that I think might be relevant。

 especially for people as they start to think about downstream applications of these really cool GNNs and other graph learninging techniques that have been developed over the past couple of years。

And specifically， I'm going to touch on two， one is OMAT 24 and OCX 24。

 and I'll get into exactly what we did there and how that might be useful for some of these energy challenges that are so important to the world today。

So just a little bit of background on that。What fair and the fair chemistry team are for those who aren't super familiar with their work。

 so FAR is fundamental AI research at meta， most of the things that you're probably you've probably seen from fairR are things like Lama 1。

2，3， vision models like segment anything， embodied AI agents inside of robots and similar and different language translation methods。

One nice thing about being a fair is there is a general commitment to open and reproducible research and so that's really helpful to work with the community and open as much as we can and move super fast it's been really fun to see how much the community has helped on some of these open science challenges。

One of the nice things about being at fair is obviously lots of GPUs for training large models。

 but we also have access to a lot of idle data centers for running simulations and so that's sort of focused a lot of our efforts on how do we build the right computational data sets to help train the graph models and other fancy GNNs and things like that that we want to use for chemistry and specifically the chemistry team is about 15 people we mostly work on open science things like catalysis and direct air capture and other materials science problems but more recently some of our team has also helped internally with display materials for headsets and ARVR applications。

Okay， so really what I want to talk about today is。

Focused on catalysis and for those of you who it might have been a while since your last chemistry class。

 a catalyst is basically something you use in order to make a chemical transformation move faster。

And this is specifically important if we want to try and electrify various processes around the world to address challenges in climate change。

The most well known process is you take renewable energy and you charge batteries and that can help with grid applications or light EV so small cars super well established。

 we see that in our day to day all the time。But there's a ton of other uses of chemistry and materials。

 other sort of industrial applications that we also need to find ways to decarbonize So things like heating of buildings I'm sitting in a house。

 I think it has a natural gas system。 I wish I could find ways of electrifying that。 We have methods。

 agriculturegric ammonia takes a huge amount of。ennergy across the world in order to make the fertilizer that we need trucks are harder to electrify。

 it's really hard to build an electric plane， consumer products like plastics and industrial things like concrete and other applications。

 all of these also need to be electrified。And so there's not one way that we can do it， but。

There are a lot of chemical methods that we can use to produce these other things that are important。

 and so one way that we might do that is use renewable energy along with building blocks like CO2 or water and then upcycle those into more valuable things like pure hydrogen or fuels for aviation or heavy duty to transportation or chemicals as precursors for plastics or concrete and steel for industry。

And so I'm not going to go into a ton of detail on exactly the mechanisms and how we design the catalyst。

 but I'm happy to follow afterwards if any of you have questions。

 I just want to get across that a lot of these challenges that we need to work on for sustainability are fundamentally catalyst problems because these are all things that we need to know how to make。

So as one example for steam methane reforming， we want to make hydrogen and that might be take methane and water and try to make CO2 and something like hydrogen and a traditional process might be use oil as a process and along with the hydrogen that you make。

 you might also make a bunch of CO2 which is maybe something that we want to try and avoid。Now。

The equivalent electrochemical process is we might take water and electricity and split that into hydrogen gas and oxygen gas。

And this is a much cleaner process in that it just has water as an input and hydrogen and oxygen is an output。

 Ogen is all around us。But of course we need to find a way of actually doing this fast and so often we think about things like platinum surfaces in order to actually help us split this water and get the hydrogen so that's what the catalyst is。

For something like sustainable aviation fuel， if any of you have heard of that basically again planes are really hard to electrify so an alternative to trying to get batteries and planes is instead try and use renewable electricity and precursors like carbon monoxide or CO2 along with hydrogen that weve formed up here in order to make。

Higher hydrocarbons or alcohols basically jet fuel and again this is basically a way of doing this more sustainably that jet fuel can then be used inside of a plane and the key is that it came from renewable sources and renewable electricity so again these are just a couple of applications。

Now one。Actually try and discover a new catalyst for one of these processes it's actually really hard and there's several key steps along the way so the first is you have to be able to identify potential catalyst materials like one of the materials i'm going to consider are they surfaces or little nanoparticles or some porous material there's not one right answer people have thought about all of these。

Over the past few decades in this field。The second is we need to find ways of prioritizing and screening them。

 basically which are the ones that are most worth our time。And finally。

 once we make some predictions here， we need ways of actually testing these in lab to say。

 what are the best ones？And have we actually discovered something that can be used in a real industrial process？

Now most of the work in our group has been focused on this screening process and I think we've made a lot of progress I'm not going to go into a ton of detail。

 but I just want to highlight for anyone who hasn't seen the work。

 we've been running these open competitions and leaderboards for the mission learning community to introduce heterogeneous catalysis and why these things are important if you go to opencatalysproject。

org those live leaderboard we see submissions every few months it's been super fun to see the community proposed new models for the space I'll get into a little bit about the datas but the datas are also really big these are really interesting grass learning challenges。

And it's also been fun because we've had both academic and industrial and other。

Competitors basically submit models so for example。

 Google Deep Mind and Microsoft Research Asia and Tencent AI Lab have all contributed models that have ended up on the state board which has been super fun we also run challenges at places like Nps to basically keep the ideas fresh and then periodically have new new challenges to help the models keep on moving further so the last one was in 2023 we're not going to have one in 2024 but I think we'll have one in 2025。

Okay。So at the end of the day， we need data in order to train these large models。

 we want models that are large and generalizable and work across many。

 many different types of chemistry。Our group has released a bunch。

So OC20 and OC22 are for catalysis and inorganic materials， ODAC is a data set。

Of materials for direct air capture， basically taking CO2 out of the air and then trying to store that。

We have a more recent model called in data OAT 24 on inorganic materials I'll talk a little bit more about that in detail all of these are done with electronic structure calculations。

 basically really high fidelity physical simulations there's hundreds of millions of examples this basically makes it again a really interesting graph challenge we don't just want graphs that work on small data sets graph models we want graph models that can scale the hundreds of millions of examples and I think that's really help push the field forward in terms of how fast and scalable these methods have to be。

These were really expensive to compute something like a billion CPU hours in total。

 And all of these have been open source for commercial or non-commercial uses。Okay。So today， again。

 I'm going to talk about two things。 One is Omat 24。

 this data set to help us with this first task of identifying potential catalyst materials。

 And then later， I'll talk about Ocx 24， which is basically a data set to combine experimental computational catalyst discovery for the end。

 And I'm going to skip over this middle steps only because we've had previous talks that log on screening catalysts and a bunch of other public demos and lectures and other things。

 So if you have questions about this middle step， feel free to follow up afterwards。

 and I'm happy to point you in the direction of more resources。😊。

So let's talk a little bit in detail about inorganic materials of discovery。

So for those of you who don't have a materials background， which I would expect is probably most。

 that's fine， a simplified view of the world is basically we have materials that are made 100% of zinc。

Or 100% of copper， this is basically a little simple phase diagram of copper。

And if to we want to hypothesize a new material like zinc copper with a specific structural form。

 and this is a graph conference so you can already see that these look sort of like graphs depending on how you define a bond inside of one of these systems。

 I hypothesize a structure， I calculate the energy with my favorite physical simulation and now this forms a lower hole。

 this is basically a phase diagram and what it says is anything that's up here is going to end up being more stable as a linear combination of pure copper and zinc copper。

Now if I come up with a new hypothesis for a different structure。

 so this is the same 5050s in copper， but it has a different graph structure or a different crystal structure and this one is lower energy。

Now， I have updated my haul to basically say， this one is better than that one。

 And so I no longer need to consider this one is either metatable or unstable。

 And I'll get back to that in a second。 And so this is now the best。Material at 50，50。

 and anything up here will decompose if。A combination of zinc copper and 100% copper。Okay。

 if I hypothesize a better structure at a different composition， so 75% saying 25% copper。

 now this one gets used for the lower haul and gets updated。

And this is basically the process of materials discovery。Now， of course。

Real life is not quite the simple。 This is an oversimplified view of reality。

 And so a really common counter example of material science is diamond versus graphite， so。

For any of you who has thought about jewelry， I'm sure you've heard the phrase diamondiamonds are forever。

That basically implies that if you have a diamond， that's the most stable thing and that will last forever。

But both of these are 100% pure carbon， the graphite in the pencil is also 100% pure carbon。

And if you do a calculation， at room temperature pressure， sort of like all around us。Actually。

 what you find is that graphite is the more stable structure。

 And so what this says is if you wait an infinite number of years。

Eventually the diamonds that we have in will eventually decompose into graphite。Now in practice。

 this process is so slow that it's not something we need to worry about。

 I'm not going to go worried about the diamond and my wife's diamond ring basically decompos into graphite on our time scales。

 the barrier to go from this to this is really large so it would take a really， really。

 really long time for that to happen， but nonetheless if I waited a long enough amount of time。

 eventually I would form the most stable structure which is graphite。

Now the other key here is that in practice， these phase diagrams are actually very dependent。

On the conditions that you testament and so if you look at really high temperatures and really high pressures like you see in the mantle of the earth。

That's actually where diamonds are formed and so at those specific conditions。

 actually diamonds are the most stable structure， and so in the mantle of the earth the diamonds that are formed will last forever。

But as soon as you go to room temperature， again， bring them up to the surface。

 graphite becomes a more stable structure。And I'm just bringing up these examples just to help you understand what it means to discover an inorganic material。

 because this is super relevant to catalysis。 And there's been so much excitement about inorganic materials discovery and grass networks that we need to keep these ideas in mind as we actually start to use these things in practice。

😊，Okay， so bringing this all together， basically what happens is we want to calculate formation energies that's basically how stable is a material we're going to use graph networks in order to do that。

The materials that are most stable along thishu， we're going to call stable materials。

Everything that is very close to this hall， we're going to call metatable。Maybe we can make it。

 maybe we can't it's a little hard to know how fast it'll decompose diamond I would lump into that category of room temperature and pressure there's usually a little rule of thumb of about 0。

1 Ev per atom that people use to say how close is close for thishull。

And then anything that is way higher energy than that。

 we would say this probably not is unstable again， it doesn't mean you can't make it。

 but it means it's also unlikely and probably it's not going to show up in your life。

And so when we propose a new material either with some generative model or our favorite heurtick。

 we can classify based on these methods， so things up here will be unstable。

 things close to the hole will be metas stabletable and if you find new material that is way below the whole of the currently known materials that's when you can say okay computationally I have discovered a new material and maybe that's when as you're going and try and get my experimental friends to make it。

And a couple of the metrics that the G network community has used to try and track these things are things like the stability rate。

 if I hypothesize a bunch of new materials， what is the percentage that are actually stable so below the whole or perhaps stable。

 unique and novel that is they are interesting and new and no one has that I hypothesize them in the past。

Okay。So let's talk a little bit about how to predict these formation energies and again this is one of the areas where graph networks have really changed the game and are being used in the day to day now so we're really lucky to have these physical simulations that we can use as ground truth。

They're not perfect， but they are very helpful in practice basically we're trying to model all the electrons inside of one of these systems。

 we use computational chemistry codes， solve things like the Schchrodinger equation or use things like density functional theory to approximate and then solve that instead and the really nice thing about these quantum chemistry codes is although they're quite slow they can run on classical computers and if you have a large enough data center you can run a lot of them in parallel。

One of the things to keep in mind is that these simulations are really， really， really slow。

 So every time you want to。Calculate the property of your material it's something like 100。

000 CPU seconds per calculation and if you look at most of the large generalizable GNNs that are used in the space today that are trained on millions or hundreds of millions of structures most of those run on the time scale of about a tenth0 of a GPU second per calculation and so these are already about a million times faster and again I can't thank the graph learning community enough for helping with these tasks' been really remarkable how much progress has been made in the past few years in this space。

Oh。Okay。So there's already some nice benchmarks by others in the field on whether or not models are useful for computational materials discovery and so if you go to Mathbench Disc。

 this is basically organized by the Maial Project， which is a Open Science organization at Laws Bkeley National Lab。

They've run this website they've had this challenge up for I think about a year year and a half now most of the top methods on this leaderboard are various types of graph networks so Magnet chargeNe Mace7net or aquiformer V2 I'll go into this one in a minute all of these are various types of message passing or Gph networks each of them has a really cool paper associated with them Magnet and chargenet Magnet came from UCSD Spingongs group chargenet came from Gi Car's group at Lawrence Bkeley National Lab Macce's Gabojani's model from University of Cambridge7net is a recent modification of Nequip from a really wellknown group in Korea and Orb is a model that was developed by Oral materialsial which is a startup in space so it's really cool to see a lot of recent progress。

One of the metrics here is the mean absolute error for calculating the formation energy， the MAE。

 the units of this are electron volts per atom。And so 0。026 is pretty good already。

 If we go back and look at one of these holes， I said。

 if you want to classify is something metatable or not， you need to be within about 0。1 Ev per atom。

So 0。1 is significantly larger than 0。036 and so already we're seeing a lot of progress。

 these models are getting pretty helpful for day to day discovery。

All of these models that are listed here are trained in the same data set。

 so the same set of about 1。6 million structures that came from about 150，000。Simulations。

 we were doing little local relaxations。And all of this training data came from the materials project again。

 it was a really， really well known and very， very helpful open science effort in the materials community that's been running for over a decade now。

😊，Another thing to keep in mind with all of these is that。

When you're training on millions of structures。It's not surprising that the graph networks have also gotten quite large。

And so if you look at some of the models from a few years ago， they were reasonably small。

 hundredunds of thousands of parameters in these GNNs。But as you go to larger and larger systems。

 larger and larger data sets， again， not surprisingly。

 the models themselves have also gotten larger and deeper and more expressive。

 and so it's not uncommon to see tens of millions of parameters now just to give you a sense of scale in these sort of systems。

Now， of course， all of these are trained on the same training data set。

 and so another thing that you could ask is what if we just add more training data？

So I'm going to go into that in a second。Let's talk a little bit about what this model is。

 so EQV2 small dens is basically a specific architecture acroformer V2 and DenEs is a denoing strategy that we use for augmentation。

So we actually didn't come up with this specific architecture。

 Ecroformer V2 was developed in collaboration with Teshm's group at MIT。

 she's super well known in the Ecrovarian GNN space。

 I don't want to go through all the details of this network basically I just want to get across the input is a 3D graph。

 the output is something like the energy or the force， the force is a vector。

There's usually a bunch of message passing steps in these systems where basically all the atoms are talking to all of their nearby neighbors and we repeat that message passing step a few times。

And then at the end， we have an output head that basically takes these loan representations from the GNN and then tries to predict the final properties of interest。

There's a lot of work that's been that's gone into how to make these things more scalable and more efficient。

 so for example， there's different convolution strategies like ESEN was a model from the fair chemistry team that basically came up with a better faster。

 more scalable convolution strategy that's used inside of Aquiformware V2。

And a lot of this work was done by Ela Lau， he was a。

In turn on the fair chemistrytry team two summers and one summer ago。

 and basically the V2 is basically taking the original eformer model from Tes from Tesla's group and then scaling it up to work at scale on hundreds of millions of。

Examples。Now the other thing I mentioned was this denoizing strategy for augmentation and this is basically coming from a limitation of most of the data sets that have been available and so in the inorganic community most of these simulations are local relaxations。

 local optimizations of a structure you start with the hypothetical structure。

 you calculate the forces and you move downhill to try and find a lower energy structure and so the data themselves is usually not incredibly unique a lot of those structures along the way are often very similar。

So we've been thinking a little bit about how to help in this case one opportunity is various types of denoising your data augmentation strategies。

 so one of them is called Dens this is up on an archive it was basically an intern project from Eloon about not this past summer but the summer before basically given a structure you basically try and modify it and then the goal of the model in this augmentation system is basically please tell me the original structure。

That had this specific force spectra that I know is true。

And so this augmentation basically gives an auxiliary system that allows us to train on other points nearby while still using the same original training data set。

 so this is basically an augmentation strategy。嗯。You can do it for a relaxed point too if this thing is mostly relaxed I can try and bump this quite a bit and then again the goal of the model afterwards is please tell me the structure that has a very。

 very small force nearby and hopefully it will map back to the original structure。

Another thing that we could do is we could say well。

 maybe this is just a problem with the underlying data set。

 why don't we just try and build data sets that are far from equilibrium if diversity is good。

 why don't we just go ahead and make。Data sets that are more diverse。

 And so that's specifically what we did with this Omat 24 data set。So OED 24 is massive。

 it's more than 100 million non equilibriumbri structures。

 it's useful across many different application areas like catalysis and optics and electronics and many more inorganic materials are used in many。

 many， many different areas of material science。These were。

Structures that were made by taking known stable structures and then using really high temperature sampling strategies to get really far from the original equilibrium。

 we also ran some short timescale moleculardynamics at 1000 Kelvin or 3000 Kelvin really。

 really really hot， or we try and rattle days a bunch in order to come up with again。

 hypothetical other structures that we think are going to be useful for training the GNNs that we care about。

This data again is all open source， they're hosted on a hugging face。

 the models that are trained in the data are also available on Hi and F。

If you use these larger data set， you can train much larger GNNs and because of digital training data you can do quite a bit better。

 so if you just trained in the original MP Triage 1。

6 million data set I said that you could get an MAE of about 0。036 electron volts for atom。

If you train a very similar model。That's larger， 90 million parameters because we have more data now。

 the total data set is about 100 million structures， you can get that down to about 0。

02 electron volts per atom。Which is starting to get really close to the underlying errors associated with DftT。

 That is， the models are so good that they are approximately as good as the noise in the underlying training data set itself itself。

 which is really amazing。😊，so these models are openly available now anyone in the world can go ahead and use them and now this data set is open。

 I expect that we'll see some more models come up in Equiforma V2 as others in the community and try and scale these things even further。

Okay， another area that we can go is we can say well。Maybe we're limited by hypothetical materials。

 why don't we work with generative AI to propose new structures and so one example is motivated by Meta's Mo Gen。

 which was released least earlier this fall， basically a state of the art movie and audio generation system。

Our team has been working with some of the people behind Mo Gen to apply various types of models to materials discovery and these are also areas where a lot of others in the community have also had some really interesting progress so one model that I'll talk about in a little bit more detail is we can fine tune language models that's a reasonable strategy we can implement flow matching methods。

For this graph。Systems， that's what this gift is coming from。

 or we can try and combine the best of both worlds and I'll talk a little bit about that this flow LM model is going to come out is already up on archive and will be at Nps in just a couple of weeks in December。

And the cool connection here is it's been really fun to see some of the ideas that have been developed at meta for things like Mo Gen and specifically the audio generation system in Mo Gen is a flow matching method developed by。

😊，Others in fair like Ricky Chan and your Lipin and others in the community。

 we can apply these same flow matching methods that were originally developed for audio generation and we can apply them to materials so it's been super fun to see this back and forth between the different communities。

😊，So let's talk a little bit about what some of these structure generation systems might look like。

So on the left is one system that we worked on that was inspired by some work by Ais Bro Guuk at the University of Toronto。

 Nate Grover did this N is a PhD student at NYU， he was interning with us。あ。

Not a summer but the summer beforehand and basically what we did was we took Lama two models and fine tune them to generate these graph structures by saying please give me Xyz structures and the shape of the system the lattice cell and the really fun thing was that at the time we were able to show that these language models were competitive with some of the models that were available for。

Structure generation like CDVE， the source code for this is up on GitHub and the paper was released at ICClA this summer。

Anna Samm and others in our group， Ben Kiurt Miller， who also recently just joined。

 worked on another system taking flow matching methods， like I said。

 from Fair and applying them to materials generation and they were able to show that they could do even better than a model that had come out in the intermediate time between these two called diCP and basically what this model is doing is basically deforming this thing。

 to find an even better crystal structure given a hypothesis。

Wwhich is the initial one and if you look at the original structure。

 I probably most of you probably haven't done enough material science to see that the original ones are not very realistic。

 but by the end they're starting to look fairly ordered and it looks pretty close to maybe an oxide system。

 these little red atoms are probably oxygen atoms and these blue ones are probably some sort of metal。

One of the really interesting observations that we saw was also that this chemistry language model seemed to work really well for telling us what types of materials to make。

 and this flow matching method seemed to do a really good job of putting the atoms in the right place。

And so a more recent model， we call F LLM basically combines these two for best of both worlds。

 we basically use a language model to tell us what sort of structures we should be trying to make。

 like what's the composition or maybe an initial gas on the crystal structure。

And then once we have an initial guest， we use flow matching with this Romanmanian flow matching method that I talked about previously。

To basically come up with a denoy structure that is even more stable and better than the original initial guess。

And so again， this is up at on archive now， I think the source code is also up or will be shortly and it will be presented at Ns in a couple of weeks。

Okay。And。Again， one of the really fun things about this area right now is that there's not one right method for generating graphs for this system。

These ideas and strategies are synergistic and that means that we're going to see probably a lot of progress in the next couple of years as people mix and match these different methods。

 So if you take this crystal generation LMbased method and the slow matching one and combine them。

 you get a method that is way better at predicting stable structures than either of these alone and again just for me like opportunity for methodological improvements in the space I think it just says the field is wide open and it also says that all the work that's going into other methods for generative AI in language and imaging video a lot of these are probably going to be applicable to graph generation as well。

Okay， so I want to switch gears a little bit and talk a little bit about how we make data sets and test materials by catalysts in a lab。

And specifically， we have this OCX24 data set。That we recently released that。

 I think will help the community make some progress。

So OCX24 is basically open Catalys 2024 the X stands for experimentaliment。

 it's a combined computational experimental data set。

That we think is going to be useful to help us make AI drivenri discovery and catalysis even more meaningful。

 And I would say the data set and the methods that we release as part of this paper are relatively simple。

 And so I think there's going to be a lot of low hangingging fruit and progress from the external community and making these even better。

 And I hope that someone on this call comes up with an even better strategy than what we came up with。

😊，Okay， so in summary， basically what this data set is is it's a computational data set of little molecules sitting on surfaces that we think are correlated with experimental catalysis。

They were calculated for about 19，000 stable metastabletable materials。

 and I'll talk about where we got those from。With each of these materials， we formed many。

 many different hypothetical catalyst surfaces and placed these little oddates on them。

And so in total， we used some of the state of the A graph neural networks that we developed as part of the Open Catalyst project to run hundreds of millions of simulations。

Run DFT calculations on the best of those。 And then that forms the computational part of this data set。

 So these are things that we can calculate。With simulations that we think are going to be relevant to the final experimental testing。

On the experimental side， I'm going to go through in a little bit more detail。

 basically we have two different synthesis techniques。😊。

Lots of characterization to help us identify what materials we made in the testing platform so that we can actually say。

 what is this catalyst doing？So this data set was done for two different types of chemical reactions。

Both of which I touched on at the beginning of this presentation， so one is hydrogen evolution。

 given water and electricity split it to make hydrogen gas and oxygen gas， this is useful for making。

 again， hydrogen as a energy source for a hydrogen fuel cell。

And the second is the CO2 reduction reaction， CO2 plus hydrogen plus electricity。

Makes fuels or hydrocarbons or plastic precursors。

![](img/25574de0c5f6776e4df6b4107d6f5327_61.png)

So。How do we know which material is actually going to be stable as a catalyst so if we go to an open database like the materials project there are about 150000 structures about 60000 of those have actually been synthesized experimentally。

Most of those are not actually going to be stable when you actually put them in water and you apply a potential and try to make this thing into a real catalyst。

And so if you screen for stability， basically you find even though we know about 150，000 materials。

Actually under reaction conditions， when this thing is actually happening， only about 6。

000 will stick around for a long time， the other 149。

000 will probably decompose under those conditions。😊，So 6000 is a good starting point。

 but we also wanted to try and expand this number a little bit we wanted even more possibilities。

And so in addition to the materials Project， we also consider two other data sets。

 Alexandria and OTMD。And in total， if you take all three of these together and you screen for ones that are going to be stable again under these conditions。

 you find about 19，000 materials that are probably going to be okay to actually test if you can synthesize them and get them into a real catalytic reactor。



![](img/25574de0c5f6776e4df6b4107d6f5327_63.png)

Okay。So。What do we want to do？The dream would be basically， we have 19000 materials。

 We order a whole bunch of those。 make a whole bunch。

And then test them all and then release an experimental data site。Unfortunately。

 real life in real catalysis is just quite a bit more complicated。So what do we actually have to do？

Um， so first， we had to try and figure out which is 19，000 we actually wanted to make。

 either with some screening strategy or some sampling strategy ahead of time。Next。

We had to go and work with a bunch of experimental partners to try and find synthesis techniques to actually make the materials that we care about。

Then we had to find a way to get those things that we made in nanopart form to actually stick to the electrode in the membrane so that we could use them in a catalytic reactor。

And often we had to repeat this process a few times。

 so not every way of making the materials is compatible with how we were getting them to actually stick to the electrode。

And finally， even once you get them on the electrode and you run the reaction。

 you have to characterize the materials to make sure that you actually made what you wanted to make。

 so if I came up with a hypothetical structure， I don't want to test something random。

 I actually want to test the structure so that I know that I can match computational experimental data at the same time。

Often I don't actually make。What I was hoping to make。

 and so I might have to go back and tune or change or come up with a new synthesis strategy and then repeat this whole process again。

And finally， on the timescale of years， we were able to make a couple of hundred of examples of these materials and so these couple of hundred are the ones that were released in OCS24 and these are the ones that were actually tested at scale。

So what this process looks like is basically there's two strategies。

 chemical reduction and spark ablation， chemical reduction is a technique that was done at the University of Toronto spark ablation is a method from a startup called VS particle。

Again， we had characterization and lots of of these electrolyzs in parallel to basically try and testest these things this is a picture of a chemical reduction robot from aeys Cheem speede system。

 this is a VS particle machine that's doing the spark aation to make these arbitrary nanoparticles in these little film form。

And finally， this is a picture of this testing apparatus to basically they say how good are these it actually making？

Real catalyst and turning CO2 into something more valuable。

And the sort of data that we get out looks something like this。

 so at three different applied voltages， different conditions。

 how much energy are we putting into the system。These catalysts will make varying amounts of hydrogen or methane or CO or ethylene or other chemicals。

 So for example， for this copper nanoparticle system at low current voltages。

 it makes mostly CO and at very high applied potentials。

 it makes mostly hydrogen And depending on the synthesis strategy。

 we get qualitatively similar results， but not quantitatively the same。 So for example。

 chemical reduction versus spark ablation you get some small differences in how much CO you make at intermediate current densities。

 So this is really just to give you an idea of what is the data that look like， what are the outputs。

 what are we trying to fit And these are all things that we want to correlate with the graph networks。

And there predictions on the catalyst site。Okay。In total， we made about 500 materials。

 200 of those were clean enough and deduplicated for testing。

And order magnitude too larger than any other data set that we're aware of。

For these sorts of electrochemical conditions at high current density that have been released openly。

You can see that some areas are much denser than others。

 so we have a lot of zinc and silverber and copper and gold systems， some areas are harder。

 there's not that many materials that are stable that have gallium or inium and so these are a little bit less common just to give you an idea of what the distribution of the data set looks like。

And one really cool example， again， just going back to what we're using these graph networks for。

 I want to highlight some of the results for the hydrogen evolution reaction and so I've said this a couple of times but basically hydrogen evolution reaction HR is basically water tu electricity forming hydrogen。

😊，Gas and oxygen gas。And if you just take really cheap。

But simple descriptors based on the composition of the catalyst。

 so like what is the atomic mass of the elements and the radii and electron negativity？

You can fit the experimental data on the Y axis， the predicted voltage。

To the experimental voltage and you can get a reasonable parodity plot， so an MA of about 0。09。

 I would say it's not the most beautiful linear regression and set of residuals that I've ever seen。

 but nonetheless the R squared is about 0。5 that's somewhat reasonable in this space。

If you then go and predict all the other materials that are hypothetically possible。

 you see that there's a whole bunch of materials that are predicted to be way better than the ones that we tested in gray。

And specifically， the material that is most well known for this reaction platinum。

 which was not included in a data set shows up as a star。

 basically what this model is saying is there should be a whole bunch of materials that are better than platinum。

Which is hypothetically exciting。Now， what was really exciting to us is that if instead of using these really simple。

Composition features。Instead， we used。Features that came from these graph networks trained on the Open Catalyst project。

And so these are things like hydrogen or OH absorption energies feeding into the same linear model。

 We get a fit that looks qualitatively very similar。 The parodity plot is。Very similar。

 R squared is also about 0。50。6。 Me is about the same。

But when it comes time to make predictions this is a much more physically meaningful and realistic graph。

 it has its characteristic volcano shape that shows up in the catalyst literature。

 basically what it says is that there's a sweet spot here that catalysts like platinum helping the lie。

😊，And this this platinum catalyst is in this model predicted to be one of the best known materials。

 and this actually matches what happens in real life。

 There's a couple of other materials here that we think are a little bit better that we're trying to test。

But we're not making these predictions that there's going to be ones that are way better and if you actually tested most of these materials that came out of this original model。

 I think you would find that almost all of these are actually not better than platinum practice。

And so this is basically a long- winded way of saying that we were really happy that some of the models and GNNs that were developed and by the academic community for the Open Catalyst project seemed to have better extrapolation and more physically meaningful outcomes than if we had just used really naive descriptors at the beginning of this project。

Okay so in summary， I hope I was able to convey that this is a super exciting time to be at the interface of AI and chemistry and Ma science。

 the graph learning community has just made a ton of progress in the past few years some of these models are really helpful now we're starting to think more about downstream applications and all the other things that have to go right for materials discovery in this space。

UThis has really been a community effort， it's not any one group。

 it's been this sort of competition and back and forth and open leader boards that have driven a lot of this progress。

 which has been really fun to see。Another fun thing has been。

Materials sorry models that were developed for the Open Catalyst Project Leader Board also seem to be correlated with models that are state of the art for other areas like interorganic materials discovery。

 and so I showed that basically acroformer V2 a model that is currently state of the art for catalysts also can be made state of the art for inorganic materials。

We've released a new data set for the community OAT 24 that I think will drive a lot of progress in the next year or two to come up with even better models。

And finally， I touched on some emerging work on high throughput experimental data sets。

These are hard finding materials and testing them across many。

 many different types of elements and compositions is really complicated we've released the first data set from our group in this space OCX 24 I expect there will be others from the community。

 I think we're really at the beginning of this journey and I think there's a lot of opportunity for graph networks to help in this area as well either by helping with the original sort of synthetic simulation data sets and making those more accurate or perhaps directly helping us predict experimental and computational results in making the bridge together。

Finally， I just want to say everything I talked about today was 100% of team effort it's been awesome to see all the collaborators work together to enable these new data sets and system so we have a large group at Fair we also have really close collaborators at Carnegie Mellon like John Kitchen collaborators at the University of Toronto like Ted Sargent and others and finally I didn't touch much on the direct air capture work but I mentioned it that's mostly been in collaboration with Georgia Tech we've also had some recent interns here have contributed to a lot of these projects so with that I'll stop there happy to take any questions and if I don't address anything and you have any questions afterwards feel free to shoot me an email or follow up on our GiHub page and post issuess if you see anything interesting thanks so much。

Thanks so much， Hu。Thanks so much so who has been very insightful， very exciting。😊。

I've learned a lot about how to use it， I think you have conveyed very properly how like all the methods based on graphs are。

They help to speed out both the process and they are also very fast compared to other computational methods。

So I think it's very， very interesting and very insightful。😊，If anyone want to ask questions。

 we have the chat open， I think there is one question also as Slack and anything else。

Raphael Arto ask you a question， Professor Sakachri。

 would be interesting to predict the parasite processes within the material discovery？Yeah。

 I think what you're asking， but correct me if I'm wrong is basically。

If you're trying to make these things， you might make a bunch of other things by accident that may be parasitic in that you're trying to make something。

 but there's some other processes that you don't want to happen。And that really gets to real life is。

Often not about the thermodynamics， which is this really simplified view of inorganic materials that I was showing about talking about before。

I'm just going back here， but actually it's about the kinetics of these different processes that are all happening at the same time and often the one that's the fastest wins so the reason I I in the rest of the field have focused on the thermodynamic sort of view of the world of just what is the most stable is。

It is way simpler， and it was easier to make models and dataset sets for those challenges。But。

Now that we have these we need to we need to go the next step and think about these kinetic processes in parallel that you're talking about and I think that's where the community is heading。

 So using the graph networks that have been developed to say what is the rate of reaction to go from A to B to C and what are all the other processes that's exactly what you're going to have to do to get models that fit the experimental data even better than the current ones so we're just at the beginning stages of that。

One of the reasons there's been so little work in that area is just these。

These simulations were so expensive and so slow and so annoying to converge in the past。

 We just weren't asking the， the more physically meaningful questions that we' were hoping to do。

 But now that we have these GnNs， I think we can really move very fast。You're a great dancer。

He says thanks as well yeah also Shatania had a question about that's also a question that came to my mind when you were showing the table about the data sets and architectures how responsible are in terms of improving the performance they field the data sets and architectures so the exact question is how do you weight the data sets or architectures in terms of importance in pushing the frontiers in material discovery like。

Yeah， that's a great question。So the short answer is I think。

I think all of these have to happen in parallel and have happened in parallel and so what I mean by that is if we had gone back five years and we had said。

 okay， the only thing we're going to do is methods development for GNNs。

I think we would have made progress。But we wouldn't necessarily have come up with architectures that we're going to scale the hundreds of millions of examples。

And so the one thater。That are the best right now and these really large leader boards are also the ones that are simultaneously expressive and fast scale。

 and that's important in practice。Um I。On the flip side。

 if we had only done data sets and we had never done any work on the GNN side。

 the original GNNs five years ago in the material space like CGCNN。

 they were so cool to see and were way ahead of their time in terms of like how they were thinking about the world。

 but they were clearly not expressive enough to really get at these sort of kinetic questions and if you move atoms a little bit what are the properties going to be。

And so I think， again， it has to be both at the same time。

There's a limit to how far we can push the data generation side。

 so a billion hours of compute for all these different data sets and hundreds of millions of examples。

We can continue to do that for a while， but like realistically we're not going to get 100 times more data here。

 there's sort of like an upper limit on what's possible。

 so we either have to get smarter about how we generate the data into choose new structures。

 Maybe that'll be more active learning or other methods of。

Selecting points that are the most meaningful or it could be higher fidelity simulations。

 These are approximations of reality there are perfect。

 Probably there will be smaller data sets of really。

 really accurate calculations that will come next。 So there's still a lot of work on the data set side as well and I also just want to convey if you look at the open Catas project The board。

 we've seen the steady stream of。😊，New model architectures every few months。

 I think there's going to be another one that'll show up pretty soon on the leaderboard。

I hope that that continues for a while every time another one comes from either us the community it gets me really excited I don't know what's going to happen next year in all honesty like obviously we're trying some random experiments to see other cool things that we think might be interesting but again a lot of the advances have come from the academic community and we've been surprised so I really hope someone on this call comes up with the next architecture next year。

Yeah。😔，Yeah， definitely， definitely。诶。So before we finish， Sam will ask。

 what do you see as the biggest challenge facing the field over the next few years？Yeah。

 I think that' there's so many things that have to happen here。

I would say maybe the most important is。Really on the experimental data set side and so。

Going through all this work。Right， we came up with systems that were。

I would say quite industrially relevant， like the way that we were testing these catalyst materials was。

Maybe not exactly the thing that you would use at full scale in industry， but they were。

Pretty close to the current densities that you would see they were pretty meaningful。

We did hundreds of examples here， and I showed you。

These are definitely not perfect models that have been developed so far there's a lot of scattered here。

And so I think there's a ton of room to make these data sets even bigger。

And that's going to be important if we want more data driven methods in the actual experimental discovery process。

😊，And so that's not， I would say not wildly creative on my part。

 there's a ton of people in the community who are pushing on this that could be high throughput systems。

 but if you do high throughput you also need to make sure it's still industrially relevant。

 it could be automated robotic systems to basically do these things faster。

 it could be other ways of testing， probably it's going to be a combination of all the above。

But I think it's going to be really important to make these experimental discovery processes statistically significant if you look at the way most of it's been done in the past it's been individual papers and individual compositions a few at a time and it's it's really hard to make progress in data driven methods when that's the state of the art so these are all hard problems。

😊，To be honest this， it's not clear if it's going to happen in one or two years or two or five years or five or 10 years。

 but that's clearly a direction the field has to go。Yeah， thank you very much。

 I think we are running out of time， so I would like to thank you again。

I think you transmit all the excitement about the field， it has been very very insightful talk。

 So thanks Pro Professor Sachri。😊。

![](img/25574de0c5f6776e4df6b4107d6f5327_65.png)

Thanks so much for the invitation。Thanks， thanks。 our pleasure。And we are going to move to the oral。

 so I will pass the Nick to Yanang。

![](img/25574de0c5f6776e4df6b4107d6f5327_67.png)

Hey， yeah guys， yes， yes， I'm here。 We just trying to， yeah。

 just trying to make Steve as co host as well。So。All right。

 so I guess well just begin because we are data behind schedules。All right guys， so welcome。

 this is the second oral presentation and my name is Ymba I'm from Cornell University I'll be today's host for the presentation section and with to do let's welcome our first speaker I think it's Christopher Ger who is currently a Todoc at the University of Woodba in Germany and I think he will be presenting to us comparing hierarchical Network quotations based on relative entropy。

So Christba， are you here？Have you made him co yeah， I just made。All right， yes。

 I'm here and now I can also say it。B。Sorry。In other words。Yeah， whenever you're ready。Yeah。

 just I'm currently at the local meetup in Germany and in case you hear some noise in the background then you know what's going on。

It's a poster session going on at the moment and some band is rehearsing for a little bit of music later today。

But okay， let's go with the presentation， I am going to talk about comparing hierarchical network partitions based on relative entropy。

 which is a work together with Ingo Schs。So。Just to start with the too long didnn't listen version。

So we can partition networks in many different ways， which means that we often compare partitions。

And the problem that we see with common partition similarity scores is that they ignore link patterns。

 Me like the Jakard index and variance based on mutual information。

So what we do is we propose a measure that we call flow divergence。

 which is a link away our partition similarity score and based on the combination of describing random walks and the ideas behind the Ko pipeline divergence。

So comparing partitions。Let's say we have some， some items。

And they are partitioned in some certain way， let's say this our reference partition and now we have another way of partitioning them。

 let's say like this， then we can take， for example。

 the Jaar index to compute how similar are those two partitions of the same elements and basically what happens here is that we look at all the elements that are blue and we're checking how many of those agree in this case two out of four that are blue would agree between the two partitions。

And to compare those。Go through the colors and check between partition A and B for each color。

 how well do they agree and we might have to search through all the different groups because the colors might be flipped somehow in one of the partitions。

Here， the Jaar index would tell us well the agreement between B and A is12。

And we can also come up with other ways of parting the same elements。

Where the Jakar index will tell us。That the agreement is the same。

 so these two partitions C and D have the same agreement with ASB has and similar things would happen for mutual information based measures。

 essentially because they are considering set overlaps。

 but we're interested in comparing network partitions， so if I take these partitions from before。

And draw them as a network and interpret what we have as communities in this network。 Now。

 we would commonly just use the same measure。 again， the Jakarard index to compare。

How well does B align with this reference A， how well does C align， how well does D align。

 and the Tracard index would tell us that， well， they all agreed to the same extent。And then。

You should ask the question， well， but what about the links？

Because if we consider these partitions as community structures。

 then there should be some difference between C and D for B and C， we can see okay。

 there is a lot of symmetry， so they are probably the same。

 but there should be a distinction between D and the other ones。

So our idea is here to combine ideas behind the KL divergence and random walks on networks。

So the careL dirgence， I'm sure that many of you have heard what it is and know it。

 but just to recap， let's say we have set X of some letters， ABC DE， and they appear according to P。

And let's say now we have a sequence of those letters coming from some source。

 and we want to describe the sequence。What we can do is we can design Afman code。

Based on the frequencies and use one code word for a symbol。

How many bits do we need to use per symbol in theory。

 well that is given by the entropy of this distribution P？In this case，1。96 bits。Now。

 let's say we have an estimate of P that is Q， perhaps based on the frequency of the symbols that we have observed。

And assuming that these are the two frequencies with which the symbols appear。

 we would design a different code to encode the sequences。

Would then encode it like this and would get an entropy of 2。17。What now the K L divergence does is。

It tells us if we use a code that is optimized for Q to describe symbols that are distributed according to P。

 How many bits extra are we going to pay for this， So what we're going to pay then is the entropy of P plus the KL divergence of Q to P 0。

15 bits， and that is different from the difference between the two entros。Okay。

 so the KL divergence tells us how much do we pay extra for using the wrong code。

 So to say by assuming a different distribution than the correct one。Now。

 let's take a look at random walks。 Now we just， we have a graph。 We take a random walk on it。

 and now it's this random walk that generates a sequence of symbols for us。

 and these symbols are now the nodes。We can describe the sequence of nodes in a similar way by assigning code words to the nodes and then using one codeword first step of the random walker to say where the random walker is。

The description length for that in this case where we don't have communities is the entropy over the node of visit rates。

 so we actually don't need to simulate a random walk or assign code words that is just for illustration what we are actually interested in are the visit rates for the notes。

Which in undirected graphs， we can compute them directly。

 otherwise we might compute them with the power iteration。

 perhaps using Pat or other means of calculating a stationary distribution。

And if we then combine these two equations one and two。

 we can essentially rewrite the entropy in the shape that looks like a random walk。

 we have the random walker at a node U with probability P U。

 and then it will take steps from note U to note V。

With a certain probability toUV and the number of bits that we need to describe this step is log2 of PV if we don't have communities。

 but we are actually interested in communities because we want to compare network partitions so this cost in bits。

That is lock2 of PV， we're going replace it by something that we call here S of M UV。

 so S is simply a function that tells us given a partition M stepping from U to V。

 what is the rate at which the Roman worker does this and lock two of that will give us a number of bits that we need。

And we're going to use the map equation， which is an information theoreticalore objective function for community detection tool。

Otain such an S that tells us how much do those steps cost？So let's get back to this network。 Now。

 if we look at it， we see there are three communities。

What we can do then for obtaining a more efficient description of a random walk。

Is that we can assign code words that are unique within communities。But now we need to add also。

Module boundaries or community boundaries with specific code wordss to enter and to exit the communities so that it becomes。

Uniquely decodable and we can communicate in which community are we and when I say I visit the note that has the name00。

 we know by the context of the community where we're in which of those ones I mean。

And if we then take the same one and walk again to encode the same sequence， we would do it。

With these code words where the colors indicate from which module the code words come。And。

The code length， so the number of bits that we need to use per step is given by the map equation。

 which is shown here to the right，Essential it's a combination of weighted combination of entropy so in the front。

We have the entropy of switching between modules and Q is the rate at which we need to use this code word。

 and then this sum contains one entropy term for each of the modules and a weighting factor for that that corresponds to the fraction of time that the random walker spends in the module。

If we were interested in detecting communities， we would minimize this by searching through all possible assignments of notes to communities and select the one that gives us the shortest description。

One nice property of the map equation is that we can actually rewrite it。

 that it looks like the random walk description that we saw on the previous slides so we have a random walker at an O view it takes a transition from U to V and the cost or the rate at which the random walker does this is now given by mapps in。

Which I show in a moment， but it says that based on these modules。

 what is the rate of a random worker given these modules stepping from U2 V？Again。

 we don't need to simulate an actual random work。😡。

And we can now that we have these communities visualize the coding structure of the network as the tree annotated with code words。

But again， we don't actually eat code words， we are interested in the stationary remote visit rates and the transition rates between modules。

Mapsson basically looks at this coding structure and says， okay。

 if I have this coding structure given or this transition and visit rates for the random walker that are normalized by module。

What is the rate that the random worker goes， let's say from the green module to a node 1 or is in the green module and goes to note 10 and so on。

Now， the partition dissimilarity score that we propose， we call it flow divergence。

As I mentioned is based on the combination of the KL divergence and describing random walks on networks。

 and the idea is that we want to measure how many extra bits do we need to describe the random walk if we use so to say an estimate B of the networks。

 so to say true partition a。So we designate again one partition as the true one and then look at another one that we can and compare to it。

 so going back to this example from the beginning we take the reference partition to the left and we'll look at this coding schema that coding scheme that is derived from that and use the one to the right to compare。

So essentially here on the left， we have again， the KL divergence on the right side。

 the map equation， rear rate to the shape of a random walk。We combine those to kind of。

Look like a combination of KL divergence and the map equation to answer the question。

 how many bits do we need extra？But this is not quite what we want。

 because if we now substitute in the definition， then we will see that we only get a difference between。

To partition quality scores。 So what we're actually going to do， there's a small difference here。

 We are using the partition dependent transition rates。

Which are derived from this coding scheme that I have drawn。I's a tree like this one？

And we basically simply normalize them properly。Now you might look at this and see like， oh my God。

 we have a double sum over the notes that doesn't look very efficient。

 but it actually turns out that because of the regularities and redundancies in this coding structure。

 we only need to consider M times n many pairs of communities and nodes and typically there are much fewer communities than nodes。

 so we don't need to consider the full m times n so M squared many pairs of node because of the redundancies in the community structure。

So if we now look at the initial example again， if we use the map equation to measure which partition is best and then we see the reference partition A is best next B and C。

 they have the same code length of 3。73 bits and D is the worst in this case，4。47 bits。

 Now when we use our partition dissimilarity score to compare pairs of partitions we see that in the first row here in the table actually well a is completely identical to A B and C have the same dissimilarity and D is somewhat well a bit。

 not quite a bit， but half a bit may be more similar to A than B and C and the reason for that is that the overlap between a and D。

Is in the node with a higher degree， So the nodes that have a higher visit rate where the random walker has a higher probability of being there so that the resulting coding structure for describing the random walk changes less compared to partitions B and C。

 So here we see that only because a partition has a better quality。A better quality score。

 in this case， B and C than D， they don't need to be more similar to the reference partition。

And we can also do the same thing with hierarchical partitions here we have two partitions where one has just single level of communities。

 the other one has two levels of communities。Then we can look again at those coding schemes and make the comparisons。

And because I think I'm running out of time I would like to conclude so our motivation was that communities are based on link patterns。

 but common partition similarity scores， they typically ignore links because they weren't designed for this application and our solution is that we combine the ideas behind thescribe between random walks in the KL divergence to develop a link aware partition similarity score which is asymmetric it does not require matching of communities between partitions like we had in the Tacard index that we need to match and find the best match between two partitions。

And we can naturally compare hierarchical partitions because this is based on random walks in the coding scheme that allows us to describe part transitions between any pairs of notess。

No， thank you very much for your attention。All right， so thank you so much Christopher。

 so we still have a few minutes for Q&A， I think there is one on our Zoom。

 which is how does this differ from metaPA prior based approaches？

I'm afraid I'm not aware of that approach， so I might not be able to answer that question。

 Could you say again， I'm I think I cannot see the I asked the chat， perhaps。No。

 I don't see the message in the chat。You can't see message。

 So it's it's the Q and A function on the hand。Okay， so。I think it's asked by VK and if you are here。

 can you。IThink I can。Make your co hosts to let you clarify right I guess。二 k。Yes， if you would like。

 I think you can now use your microphone to clarify your question if you want。

So how does this differ from metapath prior based approaches？I think the question is。Is it okay。

 meet okay half prior okay， I mean， I'm since I'm not familiar with it。

 I'm I'm going to look it up and see and perhaps if the the one who asked the question is so interested。

 perhaps just drop me an email and we can discuss it offline because right now I believe I cannot answer that question unfortunately。

I guess from my understanding metapath based approaches it's just like a more fed version of your random walk based once because Mepaths have stipulated schemes for those walks but random walks you don't really require the node types of each staff in your random walk I guess so that's I guess my understanding Okay cool so do we have any more questions coming。

Yes we can still take one and two。All right。嗯。Iuess if there's no question at this point I do have a question so I think so first I guess it's a comment I think it's a very nicely intuitive idea to kind of use the random walk to kind of encode the structures of the communities and also use page rank to kind of they efficiently compute the planning probabilities of random walks。

 which makes you basically the the metric to be quite efficient and。

And computable so I think that's very nice and I guess one question I would have is about like have you tried your measure on some of the real world data sets and test some of the community detection algorithms performance based on your matrix and what kind of insights can we possibly get from doing that。

Well I have looked a little bit into what happens if we have limited data and the thing that usually happens is that community detection algorithms。

 they say that okay， now it's easier to find communities there's missing data so it's kind of like okay。

 now we overfit。But as this happens， the flow divergence actually becomes larger。

 so what you might think you gain in compressibility it's you're getting further and further away from the let's say like the true partition if if we can say that we have something like that。

All right， thanks， so I guess we had another question from referral Art Tello。

I think the comment is that one question would be interesting to check arbitrary lamps。

 Im not sure if that's clear enough for you to answer。Arbitrarily linked。

I'm not sure if that perhaps relates to the random works。

 so I'm actually considering the statistics of random walks in the limit。

 so there's not a fixed length if that it was perhaps I didn't make that clear enough in the presentation。

Okay， thanks。So I guess with that I guess we will just conclude the first presentation by Christopher thank you very much for your time and that's excellent talk we will move on to our second presentation by。

I think it's going to be presented by Antona Jolie， who is currently a PhD student at CNRS in France。

So I think he will be presenting graph Cosing with message passing guarantees。

 so I'll now transfer the host， yes the co hosts to An。And。Oh， hi everyone。 Can you hear me。

I can hear you okay。Okay， fine。Okay， so hi everyone。

 I'm Autoly and I'm here to present our work G Coing with Me Ba Guarants。

 which has been accepted this year to Ns。So in the recent years graphraph neural networks struggled with large graph search those used in recommended system。

 one of the techniques employed to solve this issue is to use graphcoing and consist in reducing the size of the graph by mapping the node of the original graph to some cost node in a constant graph。

 but we might wonder if training a JN on a custom graph is probably close to training it on the original graph。

So I will begin by introducing some notation on graph Coing。

 I will use the notation of enage to cast with some generalization。

 so graph Coing is defined on original graph and produce a cos graph GC and the matrix Q the coing matrix Q defines the mapping from the original graph to the constant graph if QIJ is zero as a nonzero value it means that the noded J of the original graph is mapped to the node I in the co graph。

Then we can define for all signal living in the original graph， such a feature。

 we can define the constantsine signal X with the cosine em matrix cube and we can define the uplifted signal in the original dimension。

 the reconstructed signal Tdx with to define this uplifted signal we use as。Lifting matrix Cus。

 which is the moon and horizontalverse of the matrix cube。

 then we can define the projection operator P， which is the constant and then lift operator。

Now I would like to introduce a very classical spectral guarantee introduced by Lucas or by Andreas Lucas。

 it is named the restricted spectral approximation constant。

 it is the of all the signal living in the low frequency of the lab of the signal and the difference between the signal and the reconstrict signal with the projection of peritoppy it can be used it can be seen as a quality measure for the cosonning and many classical coonic algorithms aimed to minimize this spectral guarantee。

Now we just briefly recall that GNN relies on neighbor propagation。

 the node features at a step L depends off the node feature at the previous step and the multiplication by a so called propagation matrix。

 which and we we denote it as it can be the adjacency with different kind of normalization。

But we might wonder what is the best choice of propagation metrics on the custom graph？

So we can think to use the same normalization as in the original graph or weighted version proposedd by one。

 but with these classical choices， spectral guarantees on the coast name does not lead to mis passing guarantees。

And to solve that tissue， we propose a new propagation matrix on the constant graph which depends on the coine matrix and the uplifting matrix you can remark that even if S issymmetric matrix or propagation matrix is oriented。

 you can see on a very tall example with the propagation matrix means the adjacency。

 you can see the original graph。The constant graph and then are propagation matrix。

 which has the same weights as the constant adjustmentancy。

 but you divide it by the size of the source super nodes。

 so it is a weighted propagation which is independent of any costing algorithm used to to produce it。

With this new propagation matrix， we're able to bounce the difference between the all signal propagated in the original graph and the constant signal propagated in the constant graph without propagation matrix and then up 15 in the original dimension。

The key point is that this upper bounds depend off a spectral guarantee namely the RSA constant。

 that means that if we have a good costonic algorithm with strong spectral guarantees。

 we will have a better bound and we will have a better reconstructed signal。To have this the。

 we make additional assumptions that are easily verified for most of the propagation metrics and laplash use。

So here you can see some example， some illustration of this the， so it is the same plot。

 one with one with a log scale and one with a linearar scale。

 so you can see our bound or theoretical bound in dot lines so you can see it is very tight for small cosonic ratio for small coonic ratio or propagation matrix is the only one belows that bound so it emphasizeizes how good the bound is for small cosonic ratio。

 so we compare it to like the two classical choice I have introduced before so the same  normalization and the weight and we also compared the two symmetric version of a co matrix one with a cos matrix Q and one with aliftlifting matrix Kus。

So。If we make some additional assumption about the loss function or the activation function。

 we're able to extend the propagation theorem to gene training and to bound the loss of the。

The loss of the gene trained on the co graph and the loss of the gene trained on the original graph。

 this happen bound still depends on the RA constant。

But activation the activation function assumption is quite strong and for now it's only verified for the activation function being the identity。

 so does the SGC network simplified graph convolution。

But additional work could be to extend this assumption to other activations function and then author GN。

So to illustrate this example we conducted several gene training on the wellknow Coine sitesr dataset set and the 100 times larger graph readdit for nodeclass。

 so you can see that for the AGC networks which verifies all our assumption。

 the benefits of a costing metrics are more evident and it outperforms all the other sort choices of propagation metrics。

 when we relax the assumption on the activation function。

 and we have results with a classical GCN we can see that our results are a bit blue and our propagation metrics is still competitive especially for high costing ratio。

 but some other choices might be a good idea such the weighted version of fun。啊。To conclude。

 I would say that we in this work we directly link the curA。

The spectral guarantees and propose a new propagation matrix。

To have like message passing guarantees and then G training guarantees on the custom graph。

This is only the case for thiscoonic matrix， which has the particular to be oriented。

 but doesnt is independent of any cosonic algorithm used to produce it。

Future works could be to explore the。The activation function assumption。

 especially because it is found quite a strong assumption to have。

To to hire to have the communication property， but other works could also include them。

Could also include additional orderura assumption we made。thank you for your attention。

 if you have any question I would be glad to answer it and you can also see me at the poster session if you have any other question。

All right， so thanks so much for your exciting talk and I think we still have quite a few minutes for questions。

Yes。So I guess we can wait a little while for people to have any questions if they have。

I guess Ive had a quick question like can you elaborate a bit on the good on this page like the when you use GC and instead of STC to count the experiments。

 we can see that the other coerc I that's in your baseline SC Di actually your better result Im actually I was probably a little bit lost and you elaborate a bit on like why that happens since what is inside we can get from that。

Do do you mean why do we have like less good results with G compared to S you。Yes， yes。

 like when the coerccing method is not your proposed one， but the one and baseline。Oh yeah。

 this is mainly because we provide ourthereorem about the GNN with strong assumption on the activation function namely when the activation function and uplifting matrix commute can commute for all the features and that is only verified for now for the activation function being thejaency。

 that's your ACC network， but for the GCN we have a non nonlinear with value。

 so this means that it breaks on assumption， so that's why the benefits or method are more evident because we cannot bound it like the training of the GNN and the weight deviation is like very similar to the classical normalization but just you put more weight on the cell loop and you put more weight if the cluster of the costing is heavier at this node。

That's why it's a bit better than the classical normalization and it is more interpretable for that case。

 so that's why it maybe sometimes it has better results。

see so does he like give us some empirical insights like when we are not we are not we do not have the nice guarantee of the activation function in STC what kind of coering method we could potentially use。

 does that give us some insight about it Yeah in fact the method that I used is more a postpro after you coing you can use any coing algorithm and any coing algorithm would give you a matrix Q and a matrix Q+ and a constant graph GC so you can after when you decide to train GNN on your co matrix you can use different choice of propagation metrics like the same normalization or of vision so I would say if you go with LTC you should definitely try propagation metrics and otherwise I think you should try perform metrics and the weighted version which is kind of smarter。

Smarter choice rather than the same normalminization。All right， thanks for the answer。

 I think we do have one question from the audience。

 so the question is what are the limits of that propagation function？那。

What do you what do you mean I limits， more particularly like。As simply， like。

the main drawbacks of the method， because I would say two。嗯。

Okay obviously I would say the strong assumption of the on the activation function is one of the biggest limits we also making another assumption that the features lives in the low frequency of the graph so that is but it's more an assumption that we make classically for graph concerningning so that means it would be it would work more on Ophillos graphs than on aphios graph because if you merge two nodes in the same super nodes but they have different levels you will have very a lot of difficulty to distinguish it so we make another assumption about the features living in the low frequency。

Of the graph and z on the low frequencies of the lapation and then on the previous preserved space。

 so this is another stronger assumption。But as we used picture guarantees， it is quite classical。

I hope I answered your question。Yes， I think。Yeah， so guess we can dismiss this question。All right。

 so if there are no more questions coming up at this point， I guess we will just。

Conclude our presentation for this paper here and thanks so much again Ena for your presentation and again。

 if the audience have more questions coming up， you can also feel free to post it in our Slack channel as well。

And。All right， so I guess we'll proceed to our next speaker。



![](img/25574de0c5f6776e4df6b4107d6f5327_69.png)

All right， so our last presentation today is going to be given by Mihail Miraoff。

 who is currently a researcher at Ys。So I think the title for the presentation today would be revisiting Gal monthly Mesure has been written here in the slide so Mikil very nice to meet you and whenever are you ready。



![](img/25574de0c5f6776e4df6b4107d6f5327_71.png)

Nice meet too， can you hear me？Yes， I can hear you good and can you see my slides， yes。

Yeah good perfect okay so my name is Michelderonov。

 this is a work withmia Pracarenkawa in which is named Travis Gr coop measures and yes we both work in Yic。

😊。

![](img/25574de0c5f6776e4df6b4107d6f5327_73.png)

So。The half level graph is like the property of the graph that if like it has like its nodes are divided in some classes。

 then if nodes of the same class tend to be connected。

 then we say that graph is schematicic if node of the opposite classes of different classes not con signing tend to be connected then we say that the graph is heterotraophilic so the question is like how to formally define this property and like how to measure it。

😊，One important comment here is that measuring homophily is not the same as measuring genome performance it's kind of a common knowledge that forphiligraphs gens tend to work better but actually if let's say I take fully heterophiagraph with only two plus like here on the slide clearly any gen will work good on it so like it will be able to guess labels and there is some other evidence that for very heterophiagraphs gens also work well。

😊，Another argument about it is that caopphily was like discussed and was like the property long before Gens like built a thing so when we' talking about caopphily we don't want to just predict general performance。

 basically what we want is to capture our intuition about which graphs are more homoophilic so if I see to graphs and I think that this graph is more more homoophilic I expect this measure to give the same answer that like this graph is more homoophilic。

😊，So well do it in the following way like we approach。😊。

Question we will see previously used caphi measures。

 we will discuss some of their properties and basically we will formulate like what properties we actually want from good ca measure。

And I said we conclude that all previous measures doesn't have a least one of these properties and I said we provide the measure which has all of these properties。

 so let's see previous hum monthly measures。😊，Here there are a lot of like notation and like create it's hard to understand it from like the scratch。

 let me just describe in words what measures we have like it's here on the right there are like four measures。

😊，First one is Hm ofly。Basically， so let's say I call an H homomophilic if it's connect to nodes of the same class。

 so I take the graph I calculate the number of all homomophilic ages and divide it by the total number of ages it's like the simplest measure possible and it's called H homomophilia。

😊，Second measure is a bit different here I calculate like for each node。

 I calculate the number of neighbors of the same class and divide it by the degree of this node in this case I can say this is like the measure of how homoophilic this particular node is and after I over like average it over all nodes and it's like I deoteed by I name it by a node homophiia。

😊，Next one is a bit complicated to let me just like give a sketch it。

 basically I take all notes of the same class， I calculate number of its neighbors of the same class and divide by the total degree of all notes of this class and this is kind of a homoophilia of the class and after that I do some other ca of it and average it。

😊，Last one is adjusted hephiia and it's an interesting one so like this part on the right which is DI divided by two times a number of ages so how we can think about it it's D is the total degree of a class like the sum of four degrees of the class and two times number of ages is clearly the total degree of like old gap and so this is kind of the fraction of degree of corresponding to my class so if I would draw the ages completely randomly then I would expect the number of comeophilic ages inside this particular class like the fraction of its ages to be like this D divided by two e squared so on the en numerator the right part basically is kind of expected number of comeophilic ages。

😊，Like so I see the ash H， so it's like the real aed number of cometic ages。

 I subtract the like expected number of comeinetic ages and I third I do a normalization by denmerator。

😊，So it's like the idea of address come matter。😊，Now let's see some properties。

 we will see them for H caopphi and based on it， we will formulate it like the properties which really want from came。

So HF is just a fraction of homoophilic ages in the graph so what like what's good about this measure first of all。

 if all ages are phophilic then I get the fixed number one so for any graph if all ages in the graph are comeophilic I get this like the same number like the good upper bound which is like one。

😊，The same is for all heterophigraphs。 So like， E ages are heteroophilic， I get 0。

 like no matter what graph is， e ages a heteroophilic，0， perfect。😊。

The last one is kind of monolatonicity that like if I have a graph and I add a hemophilic cage。

 then Hmophil increases or if I have a graph and I like delete heteroophilic age。

 there hophil decreases increases， sorry， so that's kind of like good properties which we want from like homoophil measure。

😊，Now let's see what's the downside。Contue three graphs like all of them are a doary model so basically I have a bunch of node and for each to note the probability of an age is like I' know one divided by three or whatever like some constant proion so like the ages are completely independent from labels。

😊，So suppose we have only two classes and balance of passes is like 15 like 150 notes in first class 150 notess of a second class。

 so on average I'll have like half of the ages will be homoophilic so age homoophilly will be 0。5。😊。

But if I have the same like ultratrine model， I have roughly two classes and the balance of the class is 90 to 10。

😊，And again， like draw this asent， the average expected H cooppyly is 0。82。😊。

If I have five classes like uniform and like 20 notess in every cloud and I draw I just randomly then expect that H homophiia will be 0。

20 but it's clear that like from like intuitive perspective all of these graphs should have like the same kind of neutral homoophilic because like in all of these graphs the structure of the gap itself has nothing to do as labels so it would be reasonable to expect to have like the same number in all of these like three cases so this is kind of a drawback of Hphi it basically says that if I have graphs with different balances or different number of classes。

 then H homophi is not reliable to compare such like such data sets like which of them is more homoophil or heteroophilic so let's see what properties we have based on these examples because three is like exactly what we mentioned before so if all just a homoophilic we want to have some upper bound or marks es are heteroophilic we want to have some lower bound I mean and we want some an ethnicity which says that。

😊，Additional home affiliateages or removing curulator affiliate ages must increase H。😊。

The next property is like the key property which exactly the property which H homoophil lacks is like constant baseline。

 so how we formulateted so we want to say that like if a structure for graph is independent from labels。

 then we have some constant value so we do it in the following way， we fix all node degrees。😊。

And all fast labels， I we kind of cut all ages so like now every node has like not ages but like kind of half of ages like sticking from it and I we red ages randomly so we preserve degrees and preserve labels。

 but all ages are grown randomly。😊，In this case， we expect to have some fixed air base value。

So we want our measure for every graph play made this way to have expected value error base。😊。

Okay now there are like two other minor properties but let me mention then just to mention one of them is that if I formally add one more class which is not present in the graph so there's no node of this class I just formally increased number of classes by one then homoflly measure shouldn't change and the second one is that if I rename or order classes then homoly measure shouldn't change most of these properties mentioned in like formally by Platon of fatalal including this like crucial constant baseline property。

😊，Now， let's see like what properties these measures have。😊，First of all。First three。

 H of and No of and class of doesn't have constant baseline。 So basically。

 they are not reliable for comparing deficits with like different class balances。

Next measure iss a adjusted homophily which was termulated by pla of fe and it incorporatepo in itself constant baseline。

 basically it's like what I described before is that like numerator will be zero if we draw ages randomly then like on the left it's like kind of absorbed number homoophilic ages on the right it expected number of homoophilic ages and so it has constant baseline。

 but unfortunately this measure doesn't have a minimal agreement and it doesn't have monooneity for some lower value of a homophie。

😊，And our main contribution is that we formulated the new measure and biascaophile。

 which has all of these properties。😊，So let's see， basically we will just see this formula for nearly the rest of the talk and just。

😊，Think why it truly satisfies the properties which we described。

we denote by CI the fraction of comemoilia ages inside class I and by CIAIJ half of this schiophilia ages between classes I and J。

😊，Now we reduce the formula below first of all it's dependent on some coefficient alpha。

 but let's for now ignore the second term so it's not really important it's more technical and the crucial term is the first one which is right now in black and let's see what it means that like why it makes sense at all。

😊，So first of all， suppose we have no featureophillicages。Then all CAJ is0。In this case。

 numerator and denominator basically is the same value， so we get one。

And like the test is equal to one so we have maximum agreement perfect the same for minimum agreements。

 suppose we have no heteroophilic ages， no homoophilic ages， sorry。

 so CI is equal to zero and all ages are heteroophilic。😊，In this case。

 all these square roots are zeros。😊，And the numinator is basically the negative of denominator and we get minus one。

 so we get minimum agreement。😊，I'll say nothing about monity but you can like it can be proven that like it has monity in most cases。

 whatever and now let's see that like the most important part is like why this has constant baseline like the main property which like。

😊，Previous measures slides excluding adjust come of them。

So suppose we are in this setting when we cut all ages and withdraw them randomly。😊。

Suppose like we are in the setting and we know that like the fraction of degree of class I and the fraction of the degree of class J。

😊，So suppose I know it now what can I say about coefficient CI and CIAIJ so like I know like the fraction of the fraction of like this half ages which correspond to the first class and to the second class。

 like what can I say about CIAI and CAIJ？😊，First of all。

 like CI will be just the product of like fraction D divided by2 e like squared so it's just the probability that I pick like the first one from from this fraction the second from the same fraction for CAJ you have the same and for CJ is like half of the htoophillicages well have this formal DI times Dj divided by like this2 e times2 e so。

😊，呃，sorry哦。Okay， okay， so the conclusion here is that based on this formula， we can see that。😊。

If we are in this random reding cage situation， then we expect this square root of CICJ to be equal to CIJ。

So basically this proves that the expected value of numerator is zero， which is good。

 like this basically proves that constant baseline。So yes。

 so another kind of intuition how to interpret it is that this like if I know how many homomatophilic cages I have so I know CI and CJJ。

 then I can expect the number of heteroterophilic cages to be square root of CI CJJ。😊。

And so this square root this kind of expectation and CJ is like what I really absorb。😊。

And I take the difference and like if the difference is zero。

 then I'm exactly in this random situation， which is constant baseline and yeah if it's above。

 then it's more homoophilic， if it's below， it's more heteroophilic。😊，ok。

So now yeah now let's see what the second term is about and like why we at all need them need the second term the problem with the first term is a following so suppose only one C like the CI is equal to is more than zero so let's say c11 is bigger than zero and every other like C like C22 etc as zero so like we have only hophilic ages only in one class basically。

😊，Then you see that all square roots will be zeros。

And in this case like the first term will be just minus one because like the en numerator will be just minus the deminator and that's clearly not what we want like I mean it violates like the minimum agreement and it violates like monooneonicity because like now if Pte increase c11 a bit。

 the first one will still be minus1。😊，And so the second term kind of fixes this。

 so like in this special case when like only once CAAI is bigger than zero。

 like the second term practices is I won't dig deeper， but here what you can note also is that，😊。

The second term can be arbitly small， basically， we multipllyied by some alpha where alpha can be any like any number。

 basically。😊，Okay。So this is our formula which like has like all desirable properties for all alpha bigger than zero。

 but in practice since alpha can arbitrarybit small we recommend just to drop it and use as a first because like it's the most crucially has nearly all properties like so excluding that it's violates monoonicity and minimal agreement in this rare special case other than that like like this first is perfect and like we like we recommend to use it yeah by the way。

😊，It's like we given an example of property which we give an example of measure which satisfies all properties。

 but we do not say it's like unique measure， maybe like clearly there are like other measures which also satisfy this property。

😊，Okay， so it's like again， the review table with properties and measures here you can see that。

And bysophilia V， alpha begin than zero has like all properties and when we drop the second term we just have some like small star with minimal agreement and with mentalix。

😊，Okay， I think this is nearly it， so in our paper we also include some examples of desirable and desirable behavior of homo measures and we compare them for different synthetic and drill deficits。

Also we tried to solve the same problem for directory graphs oh well by the way like this thing also can be applied to weight graphs so like it will work the same way。

 but like for directory graphs unfortunately properties themselves contradict each other so like it can be proven that if we directly from like these properties for direct case。

 then there will be a contradiction so we need some other properties。😊。

I think that Su we also will have a poster session tomorrow so come there and like ask us some questions we'll be glad to hear them thank you。

 that's it。

![](img/25574de0c5f6776e4df6b4107d6f5327_75.png)

好。All right great so thanks so much for Todd， I think we still have a little we're actually running behind the schedule and actually the postal has already begun but we still have a because we started late we still have a few minutes I guess we can still take one or two question if there's any。

Yeah， it's usual， I guess I give people a little bit time to type in questions and。

I guess I will also just utilize this small gap if there is any question to ask my question。

 so I think you started by motivating about the relationship between you know homoly measures and the performance of GNs so I guess my question will be later on because you have proposed your own homothly measure is there any insights that we can talk about between your homoly measure and GM performance。

Oh we haven't measured it yet， but in jail like I tried to actually distance from this genetic performance because again as I said。

 like genetic works well for poly fo homophiligraph but for polyliccatephigraphs they also like have like surprisingly performance so they like the drop in the performance uses in the between then there's like neutral homophiia yes I see。

😊，And also， I guess the other question that I had was like。

 do you have some of the real statistics that you obtain after running your measure on real world data sets。

 like some of the very popular data benchmarks that we use？

It's like not statistic we basically we have values of like different measures for different real benchmarks and we can see that sometimes like let's say if。

😊，Let's say H homoopia says that this benchmark is more than like one benchmark is more complicated than another one and we can see that this happens just before there is some class im balance or something。

 we can see that double measure works better， that it compares them in the way which we expect intuitive we formula with our properties。

😊，Yeah， I mean it would be nice if we have some kind of。

 I guess counter counterintuitive examples of things that we think should be homo monoly。

 but at your measure is not and things like that would be very nice to show if there's yes but like it would be not nice in some way because it will say that this measure actually bad。

 like it means that you haven't counted in something。

 it mean that like our intuition wasn't captured by the following true the idea was yes so but like if you find such examples it would like it would be like step forward。

 maybe you'll find some other properties。Yes， so I think we did not have more questions coming up。

Yes， so if there's anyone wanted to have more discussion。

 please feel free to post questions comments in our select channel and with said I think we will just finish today's oral presentation thanks again very much for Mha's talk and our poster session is right now underway in our gather town hall I think so I would encourage those of you who are interested to move there thanks very much again for everyone for coming。



![](img/25574de0c5f6776e4df6b4107d6f5327_77.png)