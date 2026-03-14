# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p06 P06_Lecture_02-开发基础设施与工具 -BV1k4YXznEjw_p6-

Hi， everyone。 Welcome to week 2 of Full Sta deep Learning 2022。 Today。

 we have a lecture on development infrastructure and tooling。 My name is Sergey。

 and I have my assistant， Miishka right here。😊。

![](img/d75396a625df6046ea36b19d0d582759_1.png)

So just diving right in， the dream of machine learning development is that you provide a project spec。

 identify birds， maybe some sample data， here's what the birds look like， here's what I want to see。

And then you get a continually improving prediction system， and it's deployed at scale。

But the reality is that it's not just some sample data。

 you really have to find the data aggregate it， process it， clean it， label it。

 then you have to find the model architecture， potentially the pre-trained weights。

 then you still have to look at the model code and probably edit it， debug it， run。

 training experiments， review the results that's going to feed back into maybe trying a new architecture。

 debugging some more code， and then when that's done， you can actually deploy the model。

 and then after you deploy it， you have to monitor the predictions and then you close the data flywheel loop。

 basically your user is generating fresh data for you that you then have to add your data。

So this reality has roughly kind of three components。 and we divided into data in red。

There's development in yellow and deployment in green。

And there are a lot of tools like the infrastructurefraural landscape is pretty large。

 So we have three lectures to cover all of it。 and today we're going to concentrate on the development part。

 the middle part， which is probably what you're familiar with from previous courses。

 most of what you do is model development。We actually want to start even a little bit before that and talk about software engineering。

 You know， it starts with maybe the programming language。And for machine learning， it's pretty clear。

 It has to be Python。 And the reason is because of all the libraries that have been developed for it。

 It's just the winner in scientific and data computing。There have been some contenders。

 so Julia is actually the JU in Jupiter Jupiter notebooks to write Python code， you need an editor。

You can be old school and use Vim or Ems， a lot of people just write in Jupiter notebooks or Jupiter Lab。

 which also gives you a code editor window VS code is a very popular text editor。

 Python specific code editor Py Charm is really good as well。At FSD， we recommend V S code。

 It has a lot of nice stuff。It hast built， you know， in addition to the nice editing features。

 it has built in git version control so you can see your commit， you can actually stage line by line。

 you can look at documentation as you write your code。

You can open projects remotely so like the window I'm showing here is actually on a remote machine that I have SS into。

 you can Li code as you write， and if you haven't seen Ls before。

 it's basically this idea that if there are code style rules that you want to follow like certain number of spaces for indentation。

 whatever you decide you want to do， gotta you should just codify it so that you don't ever have to think about it or manually put that in。

 your tools just do it for you。And if you've run something that just looks at your code all the time。

 you can do a little bit of static analysis。So， for example。

 there's two commas in a row It's not going to run in this file。

Or potentially you're using a variable that never got defined。And in addition。

 Python now has type hints， so you can actually say。

 you know this variable is supposed to be an integer。

 and then if you use it as an argument to a function that expects a float。

 a static type checker can catch and tell you about it before you actually run it。

So we set that all up in the lab， by the way， and you will see how that works。

 It's a very nice part of the lab。A lot of people develop in Jupyter notebooks。

And they're really fundamental to data science。 And I think for good reason。

 I think it's a great kind of first draft of a project。

 You just open up this notebook and you start coding。

 There's very little thought that you have to put in before you start coding and start seeing immediate output。

 So that kind of like。Fast feedback cycle。That's really great。

 And Jeremy Howard is a great practitioner， so if you watch the fast AI course videos。

 you'll see him use them to their full extent。They do have problems though， for example。

 the editor that you use in the notebook is pretty primitive right there's no refactoruring support。

 there's no maybe peeking of the documentation， there's no coppilot which I have now got used to in VS code。

There's out of order execution artifacts， so if you've run the cells in a different order。

 you might not get the same result as if you ran them all in line。It's hard to version them。

 You either strip out the output of each cell， in which case。

You lose some of the benefit because sometimes you want to save the artifact that you produced in the notebook。

Or the file is pretty large and keeps changing。And it's hard to test， because。

It's just not very amenable to the unit testing frameworks and best practices that people have built up。

Counterpoint to everything I just said is that you can kind of fix all of that。

 And that's what Jeremy Howard's trying to do with Nb dev。

 which is this package that lets you write documentation。

 your code and tests for the code all in a notebook。

 The full slide deep learning recommendation is go ahead and use notebooks。

 actually use the VS code built in notebook support， So I actually don't I'm not in the browser ever。

 I'm just in my VS code but I'm coding in a notebook style。

 But also I usually write code in a module that then gets imported into a notebook and with this live reload extension。

 it's quite nice because when you change code in the module and rerun the notebook that it gets the updated code。

And also you have nice things like you have a terminal， you can look at files and so on。

And by the way， it enables really awesome debugging。 So if you want to debug some code。

 you can put a break point here on the right。 you see the little red dot。

 and then I'm about to launch the cell with the debug cell command。

 and that'll drop me in into the debugger at that break point。

 And so this is just really nice without leaving the editor。 I'm able to to do a lot。😊。

No books are great， sometimes you want something a little more interactive。

 maybe some you can share with the world and streamreamlet has come along and let you just decorate Python codes。

 you write a Python script， you decorate it with widgets and data loaders and stuff。

And you can get interactive applets where people can。

Let's say a variable can be controlled by a slider and everything just gets reverun very efficiently and then when you're happy with your applet。

 you can publish it to the web and just share that streamlit address with your audience。

 It's really quite great。😊，For setting up the Python environment。It can actually be pretty tricky。

 so for deep learning usually you have a GPU and the GPU needs Kuda libraries。

And Python has a version， and then。Each of the requirements that you use like pi torch or nupy have their own specific version。

 also， some requirements are for production， like torch， but somewhere are only for development。

 For example， Black is a code styling tool or my Pi is a static analysis tool。

 It be nice to just separate the two。So we can achieve all these desired things by specifying Python and kuda versions in environment that Yaml file and use Conda to install the Python and the kuda version that we specified。

 but then all the other requirements we specify in with basically just very minimal constraints。

 so we say like torch version greater than 1。7， or maybe no constraints like nuy any version。

And then we use this tool called PIP tools that will analyze the constraints we gave and the constraints they might have for each other and find a mutually compatible version of all the requirements。

And then locks it so that when you come back to the project。

 you have exactly the versions of everything you used。

 And we can also just use a make file to simplify this。 Now， we do this in lab。

 So you'll see this in lab。And on that note， please go through labs one through three。

 they're already out and。Starts with an overview of what the labs are going to be about。

 then pytorch lightning and Pytorch。 And then we go through CNNs， Transers。

 And we see a lot of this structure that I've been talking about。

So that is it for software engineering。And the next thing I want to talk about are specifically deep learning frameworks and distributed training。

So why do we need frameworks。Well， deep learning is actually not a lot of code if you have a matrix math library like Nmpyine。

Now， fast that AI course does this pretty brilliantly。

 they basically have you build your own deep learning library and you see how very little code it is。

But when you have to deploy stuff onto KUuda for GPU power deep learning。

 and when you have to consider that you might be writing weird layers that have to。

 you have to figure out the differentiation of the layers that you write。

That can get to be just a lot to maintain And so and then also there's all the layer types that have been published in the literature。

 like like convolutional layers， there's all the different optimizers。 So there's just a lot of code。

 And for that you really need a framework So which framework should you use right Well。

 I think Josh answered this， you know， pretty concisely about a year ago and you said ja is for researchers Pytorrchches for engineers and Tensorflows for boomers。

😊，So Pytorch is the full stack deep learning choice， but seriously， though。You know。

 both Pytorch and Tensorflowlow and Jax。They all are similar。

 you define a deep learning model by running Python code。Writing and running Python code。

And then what you get is an optimized execution graph that can target CPUUs， GPUs， TUs。

 mobile deployments。Now， the reason you might prefer pytororch is because it just basically is absolutely dominant right so if you look at the number of models。

 trained models that are shared on Huging face， which is like the largest model zoo。

 we'll talk about it in a few minutes。You know， there's models that are both Pytorch and Tensorflow。

 There's some models on jacks。 There's some models that are Tensorflow only。

 There's a lot of models that are just for pytorch。

If you track paper submissions to academic conferences。

 it's about 75 plus percent Pytorch implementations of these research papers。

 and my face is blocking the stat， but it's something like 75% of machine learning competition winners use PyTch in 2022。

Now Tensorflowlow is kind of cool。 Tensorflow that JS in particular lets you run deep learning models in your browser。

 and Pythtor doesn't have that。 And then Karis as a development experience， is I think。

 pretty unmatched for just stacking together layers， easily training the model。😊。

And mean then there's Jacks， which you might have heard about。 So Jacks， you know。

 the main thing is you need a meta framework for deep learning。 We'll talk about in a second。

 But Pytorch， that's the pick， excellent dev experience。Its people used to say， well。

 maybe it's a little slow， but it really is production ready， even as is。

 but you can make it even faster by compiling your model with torch script。

There's a great distributor training ecosystem， there's libraries provision， audio， 3D data。

 you know， et cetera， there's mobile deployment targets。And with Pythtororch lightning。

 which is what we use in labs。Have a nice structure for how to kind of where do you put your actual model code。

 where do you put your optimizer code， where do you put your training code， your evaluation code。

How should the data loaders look like？And then what you get is if you just kind of structure your code as Pythorch Lightning expects it。

 you can run your code on CPUU or GPU or any number of GPUs or TUs with just you know。

 a few characters change in your code， there's a performance profiler， there's model checkpointing。

 there's 16 bit precision， there's distributor training libraries， it's just all very nice to use。

Now， another possibility is fast AI software， which is developed alongside the fast AI core。

And it provides a lot of advanced tricks like data augmentations， better weight initializations。

 learning rates schedulechedries。It has this kind of modular structure where there's data blocks and learners and then even vision text tabular applications。

The main problem with that I see is the code style is quite different。 and in general。It's。

It's a little bit different than mainstream Pytorch。

 it can be very powerful if you go in on it at FSDL， we recommend PyTtorch lightning。

TensorF is not just for boomers， right， FSDL prefers Pytorch because we think it's a stronger ecosystem。

But Tensorflow is still perfectly good。 And if you have a specific reason to prefer it。

 such as that's what your employer uses， You're gonna have a good time。 It still makes sense。

 It's not bad。 Jackx is a recent， a more recent project from Google。😊。

Which is really not specifically deep learning， it's about just general vectorization of all kinds of code and also auto differentiation of all kinds of code。

Including physics simulations， stuff like that。And then whatever you can express in JaX gets compiled to GPU or TU code and super fast。

For deep learning， there are separate frameworks like Fl or Haiku。And， you know， here at FSDL。

 we say， use it if you have a specific need， Maybe you're doing research on something kind of weird。

 That's fine。 Or， you know， potentially， you're working at Google。 you're not allowed to use Ptorch。

 That could make it a pretty good reason to use Js。



![](img/d75396a625df6046ea36b19d0d582759_3.png)

There's also this notion of meta frameworks and model zoos that I want to cover。

 So model zoos is the idea that。

![](img/d75396a625df6046ea36b19d0d582759_5.png)

Sure， you can just start with blank pytorage。 But most of the time you're going to start with at least a model architecture that someone's developed and published。

 And a lot of the time， you're going to start with actually a pre trained model。

 meaning someone trained the architecture。On specific data。

 they got weights that they then saved and uploaded to a hub and you can download and actually start not from scratch。

 but from a pretrain model。 Onyx is this idea that deep learning models are all about the same right Like we know what an MLP type of layer is。

 We know what a CNN type of layer is。 and it doesn't matter if it's written in pytorage or Tensorflow or cafe。

 whatever it's written and we should be able to actually ported between the different code bases。

 because the real thing that that we care about are the weights and the weights are just numbers。

 right。So Onyx is this format that lets you convert from Pythtorch to Tensorflowlow and vice versa。

And it can work super well。 It can also not work super well。 You can run into some edge cases。

 So if it's something that you need to do， then definitely worth a try。

But it's not necessarily going to work for all types of models。Hugging face has become an absolutely。

Sttellar repository of models， starting with NLP， but have since expanded to all kinds of tasks。

 audio classification， image classification， object detection， there's 60。

000 pretrain models for all these tasks。There is a specific library of transformers that works with Pytorrch。

 Tensorflow， jacks。Alsol is 7。5，000 data sets that people have uploaded。

There's also a lot more to it。 It's worth checking out。 You can host your model for inference。

 and there's， there's community aspects to it。So it's a great resource。

Another great resource specifically for Vi is it's called Tim。

 a state of the art computer vision models can be found on Tim to search Tim GitHub。Next up。

 let's talk about distributed training。So the scenarios are we have multiple machines represented by little squares here with multiple GPUs on each machine。

And you are sending batches of data。To be processed by a model that has parameters right。

 and the data batch can fit on a single GPU or potentially not fit on a single GPU。

 and the model parameters can fit on a single GPU or potentially not fit in a single GPU。

So let's say the best case， the easiest case is your batch of data fits on a single GPU。

 your model parameters fit on a single GPU， and that's really called trivial parallelism。

You can launch independent experiments on other GPUs， so maybe do a hyperparameter search。

Or potentially， you increase your batch size until it can no longer fit on one GPU。

 And then you have to figure something else out。And what you have to then figure out is， okay， well。

 my model still fits on a single GPU， but my data no longer fits on a single GPU。

 so now I have to go and do something different。And what that different thing is。

 usually is data parallelism。It lets you distribute a single batch of data across GPUus。

And then average gradients that are computed by the model across all the GPUs。

So it's the same model on each GPU， but different batches of data because a lot of this work is across GPU。

 we have to make sure that the GPUs have fast interconnect so GPU is connected usually through a PCI interface to the computer。

 and so if there's no other connection， then all the data has to flow through the PCI bus all the time。

It's possible that there is a faster interconnect like NV link between the GPUs。

 and then the data can leave the PCI bus alone and just go straight across the fast interconnect。

And the speedup you can expect is if you are using server cards like A 100s， A600s， V100s。

It's basically a linear speed up for data parallelism， which is really cool。

If you're using consumer cards like 2080s or 3080s， we'll talk about it a little further down。

Then unfortunately， it's going to be a sub linear speed up。 So maybe if you have four GPUs。

 it'll be like a 3 x speed up。 if you have 8 GPs， maybe a 5 x speed up。

And that's due to the fact that the consumer cards don't have as fast as an interconnect。

So data parallelism is implemented in Pytorch in the distributed data parallel library。

There's also a thirdpart library called Hraoid and you can use either one super simply using Pytororch Lightning you basically say what's your strategy if you don't say anything then it's single GPU。

 but if your strategy is GDPP， then it uses the PyTtorch distributed data parallel。

 if you use a strategy Ho， then it uses Hood。It seems like the speedups basically about the same。

 there's no real reason to use H over distributed data parallel。

But it might make it easier for a specific case that you might have。 So it's good to know about。

 But the first thing to try is just distributed data parallel。Now we come to a more advanced area。

 which is now we can't even fit our model。 Our model is so large。 It has billions of parameters。

 It doesn't actually fit on a single GPU。 So we have to spread the model。

Not just the data over multiple GPUs。And there's three solutions to this。

So sharded data parallelism starts with the question。What exactly。Is in the GPU memory。

 What has taken up the GPU memory。So okay， we have the model parameters。

The floats that make up are actual layers。We have the gradients， we need to know about the gradients。

 because that's what we average to do our back。But we also have optimizer states and that's actually a lot of data for the atom optimizer that's probably the most often used optimizer today。

 it has to be statistics about the gradients basically and in addition。

 if you're doing kind float 16 training， then your model parameters and gradients might be float 16 but the optimizer will keep a copy of them as F 32 as well。

 so it can be a lot more data。And then， plus， of course， you send your batch of data。

 So all of this has to fit on a GPU。But does it actually have to fit on every GPU is the question。

So the baseline that we have is， yeah let's send all of this stuff to each GPU。

 and that might take up like 129 gigabytes of data in this in this example。

 this is from the paper called zeroro optimizations towards training trillion parameter models。Okay。

 so what if we shard the optimizer states， sharding is a。

Concept is from databases where if you have one source of data。

 you actually break it up into shards of data such that across your distributed system。

 part of your each node only sees a shard， a single shard of the data。So here。

 the first thing we can try is we can shard the optimizer states。 Each GPU doesn't have to have。

All the optimizer state， it just has to have its little shard of it。

And we can do the same for gradients。And that's called zero2。And then。Pretty crazily。

 we can also do it for the model parameters themselves。 And that's called 0 of3。

 And that can result in a pretty insane order of magnitude reduction in memory use。

 which means that your batch size can be 10 times bigger。

I recommend watching this helpful video that I have linked。

 but you literally pass around the model parameterss。Between the GPUus as computation is proceeding。

 So here we see four GPUus，4 chunks of data entering the GPUs。

And what happened is GPU 0 had the model parameters for that first part of the model。

 and have communicated these parameters to the other three GPUus。

And then they did their computation and once they were complete with that computation。

 the other GPUs can actually delete the parameters for those first four layers。

 and then GPU1 has the parameters for the next four layers and are broadcasts them to the other three GPUs who are now able to do the next four layers of computation。

 and that's just in the forward pass and then you do the same with gradients and optimizer states in the backward pass。

This is a lot to implement。 Thankly， we don't have to do it。

 It's implemented by the deep speed library from Microsoft and the fair scale library from Facebook and recently actually also implemented natively by Pythtorge。

So in Pytororch， it's called fully sharded data parallel instead of 0，3。 And with Pytorch lightning。

 you can actually try sharded D。With just a tiny bit of a change， try it。

 see if you see a massive memory reduction that can correspond to a speed up in your training。Now。

 the same idea， the zero3 principle right， is that the GPU only needs the model frames it needs in the moment for the computation it's doing at this moment。

The same principle can be applied to just a single GPU。

 You can get a 13 billion parameters onto the GPU and you can train a 13 billion parameter model on a single V100。

 which doesn't even fit it natively。And fair scale also implements this and calls it CPU offloading。

There's a couple more solutions， model parallelism， take your model， your model。

 let's say it has three layers and you have three GPUs。 you can put each layer on a GPU right。

 and in Pythtor you can just implement it very trivially But the problem is that only one GPU will be active at a given time。

 So the trick here。Is that and once again， implemented by libraries like Deep speed and fair scale。

 they make it better。 So they pipeline this kind of computation so that GPUus are mostly fully utilized。

 although you need to tune the amount of pipelining and the batch size and exactly how you're going to split up the model into the GPUus。

 So this isn't as much of fire and forget solution like like sharded data parallel。

And another solution is Tensor parallelism， which basically is observing that if there's nothing special about a matrix multiplication that requires the whole matrix to be on one GPU。

 you can distribute the matrix over GPUs。So megaron LM is a repository from NVIDdia。

 which did this for the transformer model and is widely used。

So you can actually use all of these if you really need to scale。

And the model that really needs to scale is a GT3 sized language model， such as Bloom。

 which recently finished training。So they use zero datata parallelism， tensor parallelism。

 pipeline parallelism， in addition to some other stuff。 and they called it 3D parallelism。

But they also write that since they started their endeavor。

 the zero stage3 performance has dramatically improved， and if they were to starch over again today。

 maybe they would just do sharded data parallel， and that would just be enough。So in conclusion。

 you know， if your modeling data fits on one GPU， that's awesome。

 if it doesn't or you want to speed up training， then you can distribute over GPUs with distributed data parallel。

If the model still doesn't fit， you should try 03 or fully sharded data parallel。

There's other ways to speed up。 There's 16 B training。 There's maybe some special， you know。

 fast kernels for different types of layers， like transformers。

You can maybe try sparse attention instead of normal dense attention。

So there's other things that these libraries like Deep speeded and Fair Scll implement that you can try。

And there's even more tricks that you could try， for example， for NL P。

 there's this position onco stuff。You can use something called alibi。

 which scales to basically all length of sequences。

 so you can actually train on shorter sequences and use this trick called sequence length warm up where you train on shorter sequences and then you increase the size and because you're using alibi。

 it should not mess up your position coding。And then for vision。

 you can also use a size warmup by progressively increasing the size of the image。

 you can use special optimizers， and these tricks are implemented by a library called Mosaic ML Composr。



![](img/d75396a625df6046ea36b19d0d582759_7.png)

And they report some pretty cool speed ups， and it's pretty easy to implement。



![](img/d75396a625df6046ea36b19d0d582759_9.png)

And they also have a cool web tool。 I'm a fan of these things that basically lets you see the efficient frontier for training models。

 time versus cost。😊，Kind of fun to play around with this Mosaic Camel Explorer。

There's also some research libraries like FFCV， which actually try to optimize the data flow。

There are some simple tricks you can maybe do that speed it up a lot。

These things will probably find their way into mainstream pietorch eventually。But。

It's worth giving this a try， especially if， again， if training on vision models。

 The next thing we're going to talk about is compute that we need for deep learning。

 I'm sure you've seen plots like this from Open AI。 This is up through 2019， showing on a log scale。

 just how many times the compute needs for the top performing models have grown。

And this goes even further into 2022 with the large language models like G 3。

 They're just incredibly large and required an incredible amount of petaflops to train。So basically。

 NVIDIdia is the only choice for deep learning GPUs。And recently。

 Google TPUus have been made available in the GCP cloud， and they're also very nice。

And the three main factors that we need to think about when it comes to GPUs are how much data can you transfer to the GPU。

Then how fast can you crunch through that data， And that actually depends on whether the data is 32 B or 16 bit。

And then how fast can you communicate between the CPU and the GPU and between GPUs？

We can look at some landmark and video GPUs， so the first thing we might notice is that there's basically a new architecture every year。

 every couple of years， it went from Kepler with the K80 and K40 cards in 2014。

Up through A peer from 2020 on some cards are for server use。 Some cards are for consumer use。

 If you're doing stuff for business， you're only supposed to use the server cards。

 The Ram that the GPU has allows you to fit a large model and a meaningful batch of data on the GPU。

 So the more Ram， the better these are this is like kind of how much data can you crunch through。

In a unit time。And there's also， I have a column for Tensor T flops。

 which are special tensor cores that Nvidia specifically in for deep learning operations。

 which are mixed precision F 32 and float 16。 These are much higher than just straight 32 Bterof flops。

If you use 16 bit training， you effectively double or seww your aim capacity。

It we looked at the ter flops， These are theoretical numbers， but how do they actually benchmark。

Laing the Labs is probably the best source of benchmark data。

And here they show relative to the V100 single GPU。 How do the different GPUus compare。

 So one thing we might notice is the A100， which is the most recent GPU。 That's the server grade。

Is over 2。5 faster than V100 You'll notice there's a couple of different A100s。

 the PCIe versus SxM4 refers to how fast you can get the data onto the GPU and the 40 gig versus 80 gig refers to how much data can fit on the GPU。

Also recently， there's R TX， A 4000，5000，6000， and so on cards and the A 40。

 and these are all better than the V100。Another source of benchmarks is AIME。

They show you time for Resnet 50 model to go through 1。4 images in Inet。

The configuration of 4 a100s versus 4 V 100 is three times faster in flow 32 and only one and a half times faster in flow 16。

There's a lot more stuff you can notice， but that's what I wanted to highlight。

And we could buy some of these GPPUs， we could also use them in the cloud。So Amazon Web Services。

 Google Cloud Platform， Microsoft， Azure are all the heavyweight cloud providers。

Google Cloud Pla out of the three is special because it also has TPUs。

And the startup cloud providers are Lame the labs， Paspace， Correweave， Data crunchch， Javis。

 and others。So briefly about TPUs。So there's four versions of them。 for generations。

 The TP U V 4 are the most recent ones， and they're just the fastest possible accelerator for deep learning。

 This graphic show speed ups over a 100， which is the fastest Nvidia accelerator。

 But the V4s are not quite a general availability yet。 The V3s。

Are still super fast and they Excel at scaling。 So if you use。

 if you have to train such a large model that you use multiple nodes。

 multiple and all the cores in a TPU， then this can be quite fast。 Each TPU has 128 gigs of Ram。

 So there's a lot of different clouds， and it's a little bit overwhelming to actually compare prices。

 So we built a tool。For cloud comparison， cloud GP comparison。 So we have AWS， GP， Azure。

 Lambda Labs， Paspace， Javis Labs， data crunch， and we solicit pool requests。

 So if you know another one， like coreweave， make a pool request to this CSV file。

And then what you can do is you can filter。 So for example， I want to see。

Only the latest generation GPUs。I want to see E。Only four or eight GPU machines。

And then maybe particularly， I actually don't even want to see。The I want to see only the a100s。

 So let's only select the a10s。 So that narrows a down， right， So if we want to use that。

 that narrows a down。And。Furthermore， maybe I only want to use the 80 gig versions。

S nearests it down further。 and then we can swordar by per GPU price or the total price。

And we can see the properties of the machines。 right， So we know the GP Ram。

 but how many virtual CPUs and how much machine Ram do these different providers supply to us。 now。

 let's combine this cost data with benchmark data。

![](img/d75396a625df6046ea36b19d0d582759_11.png)

![](img/d75396a625df6046ea36b19d0d582759_12.png)

And what we find is that something that's expensive per hour is not necessarily expensive per experiment。

Using Lambda La benchmarking data， if you use the 4 x V 100 machine， which is the cheapest per hour。

And you run an experiment using a transformer's model that takes 72 hours。It'll cost 1750 to run。

But if you use the 8 x A100 machine， it will only take eight hours to run。

And it'll actually only cost $250。And there's a similar story if you use conves instead of transformer models。

 less dramatic， but still， we find that the8 by a 100 machine is both the fastest and the cheapest。

So that's a little counterintuitive。 So I was looking for more benchmarks。 So here is Mosaic Camel。

 which I mentioned earlier。 They're benchmarking the Resnet 50。And this is on AWS。

 What they find is the8 X， A 100 machine is one and a half times faster and 15% cheaper than8 X V 100。

 So this is a conve experiment。 And here's a transformer experiment。P，P T 2 model。

 So the8 x A 100 machine is twice as fast and 25% cheaper than the A X V 100 machine。

And it's actually three times faster and 30% cheaper than the ADX T4 machine。

 which is a Turing generation GPU。 a good heuristic is use the most expensive per hour GPU。

 which is probably going to be a 4X or 8x A100 in the least expensive cloud。

And from playing with that cloud GPU table， you can convince yourself that the startups are much cheaper than the big boys。

 So here I'm filtering by a 100s and the per GPU cost on Lambda Las is only $110 per hour。And on GCP。

 Azure and AWS， it's at least know， $3。67。But what if you don't want to use the cloud。

 There's two options you could build your own。Which is， I would say， easy， or you can buy prebuilt。

 which is definitely even easier。 Lambda Labs builds them， and Vdia builds them。

 and then just PC builders like super microro and stuff like that。 build them。

 You can build a pretty quiet PC with， with a lot of Ram。 And let's say， you know。

2390s or 280 tis or something。 That would maybe be 5 to 8000。

It take you a day to build it and set it up。Maybe it's a rite of passage for deep learning practitioners。

 Now， if you want to go beyond for 2000 series， like 2080s or2 3000 series like 3090s。

 that can be painful just because。There's a lot of power that they consume and they get hot。

 So prebu can be better。 Here's a $12000 machine with 2 a 500， which each have 24 gigs Ram。

 It's going to be incredibly fast。Or maybe on8 GPUs now this one is going to be loud。

 you're gonna to have to put it in some kind of special facility like a colo and actually lame the labs can can store it in their colo for you。

 It'd be maybe $60，000 for 8 a6000， which is a really， really fast server。

Lameda Labs also provides actionable advice for selecting specific GPUs。

There is a well known article from Tim Detmer's that is now slightly out of date because there's no apere cards。

 but it's still good。 He talks about more than just GPUus。

 but also about what CPUU to get and the Ram。 So recommendations that I want to give is I think it's useful to have your own GPU machine just to shift your mindset from minimizing cost of running in the cloud to maximizing utility of having something that you already paid for and just maximizing how much use you get out of it。

But to scale out experiments。You probably need to enter the cloud and you should use the most expensive machines in the least expensive cloud。

TPs are worth experimenting with if you're doing large scale training。

 Lada Labs is a sponsor of the full site deep learning projects that our students are doing this year。

 It's actually an excellent choice for both buying a machine for yourself。

 and it's the least expensive cloud for a100s。😊，Now that we've talked about compute。

 we can talk about how to manage it。So what we want to do is we want to launch an experiment or a set of experiments each experiment is going to need。

A machine or machines with GPU or GPUs in the machine。

It's going to need some kind of setup like a Python version， couda version and video drivers。

 Python requirements， like a specific version of Pythtorch。 And then it needs a source of data。

 So we could do this manually。 We could use a workload manager like SLm。

 We could use Docker and Kubernetes， or we could use some software specialized for machine learning。

If you follow best practices for specifying dependencies like Conda and P tools that we covered earlier。

 then all you have to do is log into the machine， launch an experiment， right。

 activate your environment， launch the experiment， say how many GPUs it needs。 if you， however。

 have a cluster of machines， then you need to do some more advanced。

Which is probably going to be sLm， which is an old school solution to workload management that's still that's still widely used。

This is actually a job from the big science effort to train the GT3 sized language model。

 so they have 24 nodes。With 64 CPU and 8 GPs on each node。

Slerm is the way that they launched it on their cluster。

Docker is a way to package up an entire dependency stack in in something that's lighter than a full on virtual machine。

 andviDdia Docker is also something you'll have to install， which lets you use GPUs。

And we'll actually use this in live， so we'll talk more about it later。

Kubernetes has kind of emerged as the best way the most popular way to run many Docker containers on top of a cluster。

 Cubeflowlow specifically is a project for machine learning。

 Both of these are Google originate an open source projects。

 but they're not controlled by Google anymore， So with Cubflowlow。

 you can spawn and manage Jupiter notebooks。 you can manage multistep workflows。

 It interfaces with Pytor and Tensorflow， and you can run it on top of Google Cloud platform or AWS or Azure or on your own cluster。

And。It can be useful， but it's a lot。 so it could be the right choice for you。

 but we think it probably won't be。Slm and Qubeflow。

 they make sense if you already have a cluster up and running。

 But how do you even get a cluster up and running in the first place。 And before we proceed。

 I try not to mention software as a service that doesn't show pricing。 I find that， you know。

 when you go to the website and it says， call us or whatever， Contact us for a demo。

 that's not the right fit for the FSTL community。 We like to use open source ideally。

 but if it's not open source， then at least something that's transparently priced。



![](img/d75396a625df6046ea36b19d0d582759_14.png)

![](img/d75396a625df6046ea36b19d0d582759_15.png)

AW SageMaker is a solution you've probably heard about if you've used Amazon Web services。

 and it's really a set of solutions。 It's everything from labeling data to launching notebooks to training。

 to deploying your models and even to monitoring them。 And notebooks like a central paradigm。

 they call it sagemakerr studio。And Sagemaker could totally make sense to adopt if you're already using A W S for everything。

If you're not already using AWS for everything， it's not such a silver bullet that it's worth adopting necessarily。

 but if you are， it's definitely worth a look， so for training specifically they have some basically prebuil algorithms and they're quite old school。

 but you can also connect any other algorithm yourself， it's a little more complicated。

And right away， you have to configure a lot of， I am， you know。

 roles and security groups and stuff like that。 It might be overwhelming if all you're trying to do is train a machine learning model。

That said， they do have increasing support for Pythtorch。 Now。

 notice if you're using sage maker to launch your Pythtorch training。

 you're going to be paying about 15% to 20% markup。

 So there's special sage maker instances that correspond to normal AWS GPU instances。

 but it's more expensive。They do have support for using spot instances。

 and so that could make it worth it any scale is a company from the makers of Ray。

 which is an open source project from Berkeley。And recently， they released Ray Tra。

Which they claim is faster than sageMaker， so the same idea basically lets you scale out your training to many nodes with many GPUs。

 but it does it faster， and it has better spot instant support where if a spot instance gets killed during training。

 it recovers from it intelligently。And any scale， any scale is software is a service that makes it。

 you know， really simple to provision compute with one line of code。

 you can launch a cluster of any size。That ease of use comes at a significant markup to Amazon Web services。

Grid。aiI is makers of Pythorch lightning， and the tagline is seamlessly trained hundreds of machine learning models on the cloud with zero code changes。

If you have some kind of main dot Pi method that's going to run your training and that can run on your laptop or on some local machine。

 you can just scale it out to a grid of instances by prefaing it with grid run and then just saying what kind of instance type？

How many GPUs should I use spot instances and so on？And you can also。

 you can use their instances or you can use AWS under the hood。

And then it shows you all the experiments you're running and so on。 Now。

 I'm not totally sure about the long term plans for grid dot AI。

 because the makers of Pythorch lightning are also rebranding as lightning dot AI。

 which has its own pricing。So I'm just not totally sure， but it's if it sticks around。

 it looks like a really cool solution。There's also non-mine learning specific solutions like you don't need sageMaker to provision。

 compute on AWS。 You could just do it in a number of ways that people have been doing。

 you know provisioning AWS instances， and then uniting them into a cluster。

 you can write your own scripts， you can use Kubernetes。

 you can use some libraries for spot instances， but there's nothing you know we can really recommend that's super easy to use。

Determined AI is a machine learning specific open source solution that lets you manage a cluster either on Prem or in the cloud。

It's cluster management， distributed training。Experiment tracking， hyperparameter search。

 a lot of extra stuff。It was a startup also from Berkeley。 It got acquired by HP。

 but it's still an active development。 It's really easy to use。 You just install determined。

 get a cluster up and running。 You can also launch it on AWS or GCP。



![](img/d75396a625df6046ea36b19d0d582759_17.png)

That said， I feel like a truly simple solution to launching training on many cloud instances still doesn't exist。

 So this is an area where I think there's room for a better solution。



![](img/d75396a625df6046ea36b19d0d582759_19.png)

And that cannot be said about experiment management and model management。

 because I think there's great solutions there。So what experiment management refers to is， you know。

 as we run machine learning experiments， we， we can lose track of which code parameters data set generated。

 which model。 When we run multiple experiments， that's even more difficult。

 We need to like start making a spreadsheet of all the experiments we ran and the results and so on。

Tenssorboard is a solution from Google that's not exclusive to Tensorflow。

 It gives you this nice set of pages that lets you track your loss and see where your model saved。

 And it's a great solution for single experiments。😊。

It does get unwieldy to manage many experiments as you get into dozens of experiments。

 MLflow tracking is a solution that is open source。 It's from databricks。

 but it's not exclusive to databricks， It's not only for experiment management。

 It's also for model packaging and stuff like that。

 but they do have a robust solution for experiment management you do have to hosted yourself weights and biases is a really popular。

 super easy to use solution that is free for public projects and paid for private projects。

 they show you all the experiments you've ever run slice and dice however you want for each experiment they record what you log like your loss。

 but also stuff about your system， like how utilize your GPU is， which is pretty important to track。

And you basically just initialize it with your experiment config， and then you log anything you want。

 including images。 And we're actually going to see this on lab 4， which is this week。



![](img/d75396a625df6046ea36b19d0d582759_21.png)

They also have some other stuff like you can host reports and tables is a recent product that lets you slice and dice your data and predictions in really cool ways。



![](img/d75396a625df6046ea36b19d0d582759_23.png)

Determined that AI also has an experiment tracking solution， which is also perfectly good。

 And there's other solutions， too， like Neptune and Comet。And a number of others， really。

Often we actually want to programmatically launch experiments by doing something that's called hyperparmeter optimization。

 so maybe we want to search over learning rates， so as we launch our training。

 we don't want to commit to a specific learning rate。

 we basically want to search over learning rates from you know 0。0001 to 0。1。

 even more awesome if like this was done intelligently。

Where if multiple runs are proceeding in parallel， the ones that aren't going as well as others get stopped early and we get to search over more of the potential hyperparameter space。



![](img/d75396a625df6046ea36b19d0d582759_25.png)

Weights and biases has a solution to this that's very pragmatic and easy to use。 It's called sweeps。

 The way this works is you basically add a Yaml file to your project that specifies the parameters you want to search over and how you want to do the search。

So here on the right， you'll see we're using this hyperband algorithm， which is the state of the art。

 hyperparameter optimization algorithm。And then you launch agents on whatever machines you control。

 the agent will pull the sweep server for a set of parameters。Run an experiment， report results。

 pull the server for more parameters and keep doing that。



![](img/d75396a625df6046ea36b19d0d582759_27.png)

And there's other solutions。 This is pretty a table stakes kind of thing。

 So sagemakerr has hyperparameter search， determined AI has hyperparameter surge。

 I think of it as just。It's a part of your training harness。

 So if you're already using weights and biases， just use sweeps from weights and biases。

 If you're already using determine， just use hyperra search term determined。

 It's not worth using some specialized software for this。And lastly。

 there are all in one solutions that cover everything from data to development to deployment。

The single system for everything。For development， usually a notebook interface。

 scaling a training experiment to many machines， provisioning the compute for you。

Tracking experiments， versioning models， but also deploying models。And monitoring performance。

 managing data， really all in one。 Each maker is the， you know， the prototypical solution here。



![](img/d75396a625df6046ea36b19d0d582759_29.png)

But there's some other ones like gradients from paper space。 So look， look at these features， Notes。

 experiments， data sets， models and inference。

![](img/d75396a625df6046ea36b19d0d582759_31.png)

Or domino data Las， you can provision compute， you can track the experiments。

You can deploy a model via rest API， you can monitor the predictions that the API makes。

 and you can publish little data applets kind of like streamrelit。

You can also monitor spend and you see all the projects in one place。

Domino's meant more for kind of non deep learning machine learning。

 But I just wanted to show it because it's a nice set of the all in one functionality。

 So these all in one solutions could be good。😊。

![](img/d75396a625df6046ea36b19d0d582759_33.png)

But before deciding we want to go in on one of them。

 let's wait to learn more about data management and deployment in the weeks ahead。



![](img/d75396a625df6046ea36b19d0d582759_35.png)

And that is it for development， infrastructure and tooling。Thank you。

