# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p09 P09_Lab_05-故障排除与测试 -BV1k4YXznEjw_p9-

![](img/7d640d096b680199f7cb7eb3a8deaf70_0.png)

Hey there， folks， Charles here for full stack deep learning Today， we're going through the fifth lab。

 which talks about testing， machine learning systems and troubleshooting deep learning models for performance。

As always with the labs， you can find a link to the notebook on coab on the Gitthub repository or you can clone it from there and run it locally。

 you can also find links in the description of this video。

 So first we're going to cover practices and tools for testing and liing Python code in general and then a little bit about some tests for MLl training systems in particular and then we'll spend some time going through what the training step for a neural network in Pytorch looks like tracing it and looking at the actual execution and using that to understand how we can optimize the performance of our models and so troubleshoot issues with models that run too slowly So while Linting and code cleaning is important we want to make it as automated as possible and towards that purpose we bundle up all of our linting and style checking tools into a single command using a tool called precommit so we can see that we've run it here we run this command line tool from the notebook to check all of our files。

The really useful things about pre commitmit is that it installs all of the dependencies for tools in separate environments。

 So you don't have to worry about making sure your development environment for your models is also compatible with the expectations and demands of your linting tools。

 which can sometimes be mutually exclusive even。 So on the first run。

 you'll find that you have to wait a few minutes for all these things to be installed。

 But then after that， it's quite quick。Nice， so it only took a few seconds and that's definitely something you'll want to aim for with these precommit checks。

 These are things that are run on every Git commitit。

 And so that can be pretty often in certain workflows。

 So this list of outputs here is a list of all the checks that are run and whether they passed or failed。

 So they're all green。 So that means that they all passed。

 So some of these checks include just basic hygiene things like not committing in the middle of emerge merge conflict they also include some of our python specific things。

 they're configured via Yaml file like a lot of these tools like the automation tool Github actions or our hyperparameter sweeping tool W andb sweeps。

 pretty common file for configuring these things and the top part here includes basic precommit hooks that check things like the formats of our Yaml files check whether we're accidentally leaking a private key and then below that are our more specialized tools are automatic formatting tool black are Python style Li and check。

😊，Fake 8 and its extensions and shell check for Linting are scripting files。

 Fake 8 is a tool that also needs configuration。 One of the most important reasons to get good at configuring these tools is to ensure that you always have an escape valve。

 So if you just enforce these tools in a simple way。

 every component of the style guide is applied and it's applied to every line in every single file。

 you're going find yourself fighting against these linting tools rather than using them to help you write code more quickly and more effectively。

 So it's important to be able to do things like this large block in the middle of extend ignores that say I'm going to ignore all of these checks because I don't want to apply them right now。

 So for example， we actually turn off the flake8 annotations tool for now because we aren't keeping our type annotations fully in line with that style guide。

 something to ship in a future version。😊，In addition to Liing and checking our python files and some of our data files。

 it's also really important to check all of our shell scripts because these are a very common source of bugs。

 Shell scripts often need to do something pretty simple。

 but in part because theyre older tools and we've learned a lot about how to build tools since then and in part because they come from a time in which engineers kept manuals for their tools and learned them very thoroughly before using them。

 There's a lot of behavior that's quite surprising to the contemporary engineer and unexpected。

 And so that makes these just a really common source of bugs。

 There's a couple ways to help with this。 One of them is to have Liing and checking tools。

 So Shell check is the tool we recommend Shell check is incorporated into our precommit here。

 so it doesn't have to be run separately but it's such a useful tool that I think it's a good idea to incorporate into your editor。

 Your editor should have support for running precommit on files as well So you can run some of your precommit hooks while editing and shell check super fast so it doesn't break your editing workflow recommend。

😊，Dropping in some scripting files， maybe from your own machine from your own projects or from projects that you like from a repository like Gitthub and seeing just how many subtle bugs there can be due to things like files with new lines and spaces in their names or surprising behaviors of arrays or variables in bash scripts。

 of our other suggestions is to slightly change the default settings of bash when you're writing a script so that there are more loud failures。

 One of the biggest issues in machine learning and trying to test machine learning is that failures are silent。

 They're not caught directly。 the system keeps running， even though it's not working。

 This is also feature of bash shells because in general。

 you don't want an error to just kick you out of your login session require you to log back in。

 That's a bad experience。 But when you're writing a script。

 you actually do want failures to cause the shell to terminate That script is not running in a login shell。

 It's running underneath that login shell。Surface that error to the shell that's running our script。

 There is a brief description of these ideas here。 You can also check the original blog post on unofficial bash strict mode by red symbol。

 So in addition to linting and checking some things about the style of our code。

 We also want to check it for correctness。 And so the basic tool for this is writing tests of our code writing code that fails when other code has issues。

 even if those other issues might be silent otherwise。

 So this might start off when youre rapidly developing stuff is just throwing assert statements into your code。

 So assert statements raise an error if the value provided them as false。

 if the value provided them is true。 they keep running。 And that's a decent start。

 especially when you're moving really quickly and and developing maybe even inside of a notebook while you're trying out some functionality。

 But it has some limitations。 One of the biggest ones is these things are going be scattered throughout your code base and hard to find So it's better to collect these up into specific functions that are easily identifiable as testing functions。

😊，Testing classes and then hook those up into testing tools。

 So the standard tool for testing Python code is called Pt。 And if you pass P test a file name。

 it'll look for any classes to start a test， any functions to start with test underscore and run them and report the results。

 So for this simple test of our character error rate calculating function。

 we get both an indication that our test pass and also this report of coverage which lines in the code base were executed during this test This coverage report comes from an additional tool called code that's nicely compatible with pi test and actually also other testing tools for multi languageguage code bases So that test character error rate function was located inside the same file as our implementation of character error rate that's another good workflow when you're moving really quickly。

 but it can make it hard for tests to be discovered both by automated tools and by other engineers。

 So it's better practice to start collecting up your tests at the level of a module。

Level of a project into a test folder。 So it's easy to see at a glance what all the testing files are and name them clearly。

 So it's obvious what they test。 And in this section of the lab。

 we walk through the design of specific test for an error handling component of our custom Pythtors lightning callbacks and explains why we chose to structure the tests in the way that we did and also which kinds of tests we chose not to implement。

 which is at least as important as the tests that we do choose a sculpture is defined not just by the rocks that are there but by the rocks that are not there in addition to testing the code in our implementations It's also a good idea to test the code in our dock strings and additionally。

 we want code in our doc strings so that people looking into our methods and our classes know what kinds of behaviors to expect。

 of course without testing that code is susceptible to just like the rest of our implementations we want to use this module in the Python standard library。

Test， incorporate with P test and run it。 So you can see right above my head。

 there's some Python code in this doc string that looks like a session in a Python interpreter with doc test。

 we actually can execute that code and check that the outputs are as expected。

 You may even have seen some of these code snippets in libraries that you've used like nupy and not realize that these were actually executable and tested bits of code。

So far， we've mostly been talking about general practices for testing Python projects in general and nothing really specific to machine learning in the lab we go through a little bit on basic ideas around testing data and data handling code we go into a little bit more detail about tests for training and one of the most important kinds of tests for training our memorization tests tests that check whether a model is capable of memorizing a small amount of data。

 also known as the overfit a single batch test So here's what one of those tests look like the most important bit is there in the middle where we invoke our run experiment script with a specific set of arguments that cause it to run on the actual data that we want to train on but to only look at a single batch in each epoC so that's using the overfit batches command line argument for the Pytorch Lightning trainer Pytorch lightingning also makes basic testing of our training script easier with flags like fast Devron that are discussed in another section of。

It can be difficult to incorporate these tests into your continuous integration because most cloud services either don't provide GPU acceleration or provide it only at a very high cost。

 The cheap and dirty solution is to set up a regularly running job on a development machine that you want to take care to make sure it doesn't interrupt any other work you also want to make sure that this tests runs as quickly as possible so that's as feasibleas as possible to run it on the cheapest infrastructure that you can one way to have a really nice slider that you can use to decide how long the tests run is to set the number of epochs that this tests run for how many times is this single batch of data presented to the network and then set based off of empirical observation what value of the loss you would expect to get within that number of epos and that gives you a hierarchy of memorization tests that run from maybe five minutes on a commodity GPU all the way up to a whole hour on really nice a100s。

You can titrate the running of those tests to your budget and your needs You can run one of these memorization tests here in this lab notebook just by flipping the running memorization flag from false to true should take about 10 minutes to run on typical hobbyist hardware and you can take a look at the results in the weights biases dashboard that gets generated。

Test are great tests are important， but they are only half the battle when a test fails。

 That's just the beginning of the story， a story that doesn't end until we've resolved the issue that caused the test to fail。

 And one of the trickiest things to resolve with deep neural networks is performance in the sense of speed latency throughput。

 But resolving these issues is really important， the faster you can train models。

 the faster you will learn about the task about the model architectures and about how to solve your problem and the more performance your code is the larger scale you can operate at simply ignoring the complexities of problems and scaling your way out of your issues has turned out to be a pretty winning strategy in the world of deep neural networks。

 troublehoot the performance of deep neural networks can be challenging。

 but there's some often lowhan fruit that you can find。

 if you know how to run profiling and tracing of your network training code to look for the obvious simple bottlenecks that arise。

From some of the foot gunns that are handed to you by your training framework。

 So this section of lab has two components。 We run model training for just a single epoch with profiling turned on we've added profiling to the code base this time around using some features of ptorch and ptorrch lightning and then we take a look at that profiling data in two different ways first at a high level just looking at some of the summary statistics of what happened when our model was training to see what things we should be looking for to notice these basic bottlenecks that we can get around and then we take a much deeper dive into a trace of our training steps execution。

 which lets us know everything that happened on the CPU and GP during a couple of training steps and this trace viewers are really unbeatable tool both for optimizing your code and finding bottlenecks and also for gaining a better understanding of just what's happening while your model is training So we'll go through it in detail because that level of。

Standing of your model training is really important for being able to troubleshoot performance with no extra tooling。

 just using the profiling built into lightning and nothing else。

 you'll get a summary table of results with practice。

 you can make use of this summary table and draw insights from it but is not really the best way of viewing information。

 there's a much friendly review of some of the same information available via tensor board and our training script saves the required profiling information to weights and biases and weights biases has an integration with tensor board which allows us to actually see it inside their interface。

 So let's check that out。 So there's a couple different views of the information that's been saved by the profiler you can use this dropdown menu to switch between them。

 We're going focus on this overview tab which is the place where you can go to get a quick sense of what's going on during your training step。

 So there's some configuration information about our system letting us know what kind。



![](img/7d640d096b680199f7cb7eb3a8deaf70_2.png)

Of accelerators we are using how many of them we're using and some basic information about them。

 There's documentation in the lab and links to additional documentation about what each of these things mean。

 the most important metric that we're gonna to look at is GPU utilization So GPU utilization tells you what fraction of time there is actual work going on on your GPU。

 and this is a number that we want to get as high as possible90% or higher the GPus are the most expensive component of our training system and they're the component doing the work that is otherwise hardest to optimize So we want to make sure that that is our bottleneck optimize code as the bottleneck on the hardest piece to optimize So the goal then is for that pie chart in the top right that tells us which aspect of our training step is the bottleneck to be on kernel aka executing operations on the GPU as much as possible。

 So we'd love to see just the blue donut in the top right。

Saying that it's 100% GPU utilization or 99。9% GPU utilization and other things like copying memory to and from the GPU loading data or otherwise running things on the CPU take over a much smaller fraction in the exercises will suggest some changes that you can do to run less optimized versions of training and see the difference easy to miss down at the bottom there's a performance recommendation section you'll need to scroll down to see it on a lot of screens。

 if there's an obvious issue with your code， maybe you're not using multiprocessing for data loading。

 maybe your GPU utilization numbers are really low there will be some suggestions here on ways to resolve that bottleneck this code has pretty high GPU utilization and even high streaming machine efficiency so there's no performance recommendations here but some of the most common low-han fruits will be suggested here so always make sure to check that but it won't be able to catch everything and some of the suggestions are fairly general the only way to know what to do in your specific。

To improve the performance of your pytorch code is to look at the details of what occurred。

 identify the bottlenecks and resolve them for that， I like to use the trace viewer。

 which is based on a trace viewing tool originally developed for the chromroium browser because we've saved our information two weekss of biases we can see it inside their interface just like we can see information about our models。

 information about inputs and outputs and other charts。

 we can display that inside of a notebook if we like。

 But when viewing stuff full screen and doing a ton of interaction。

 It's nicer to look at it inside the Wb interface。 So let's go check that out。 So here's our trace。

 This trace has all the operations that were executed on the CPU and the GP laid out in time moving from left to right during a couple of training steps on the top here。

 we have operations on the CPU bottom， we have the operations on the GPU can be overwhelming at first sight。

 There's a ton of information here。 It's honestly a little bit scary to see just how much happens when we do something。

Simple like training step。 So the lab walks you through how to orient yourself in this and find some of the familiar pieces of neural network training。

 Forwards passes， backwards passes， parameter updates。 And I'll walk you through that visually now。

 and you can see detailed text instructions in the lab。 So the top half here is our CPU execution。

 And we can see these stacks of colored bars。 And this stack of colored bars is a call stack。

 The top level is the highest level method。 And inside of that method， other methods get called。

 So at the very top clicking here。😊，And pulling up information， we see something like optimizer step。

 a high levell part of our lightning interface。 As we click in， we get more detail。

 We see that we're calling the atom optimizers step。

 which itself is calling multiplication square root division operations。

 the things that are part of the atom algorithm for computing weight updates If you want to find something familiar in this trace highlight it。

 There's a search bar in the top right。 so you can type in names of methods or classes or components and see what pops up。

 So we're using our resnet transformer here。 So there should be a resnet。

 there should be a transformer。 there should be encoder and decoder methods。

 We can type all of those into the search bar and see what happens So typing in Resnet highlights these components。

 You can see they're a little bit higher up vertically because Resnet is a very high level of abstraction。

 If we're more specific and search for something like convolution。

 we'll start to see things that are lower down in the call stack， more specific method。😊。

Find the beginning and ending of our training step by looking for Resnet。

 which is the beginning the encoding of our input images and then the transformer。

 which is the second step of our forward pass， decoding the images out into text。

 We've now highlighted the transformer component and we can zoom in to a portion of the trace to see the forwards pass。

 So to zoom in to pan around and otherwise look around inside this trace。

 We use these floating tools here。 We can move them around， take put them out of the way。

 So it's zoom in on that forwards pass here。😊，哦。We can see our resnet forwards pass and our transformer forwards pass here。

 We can identify the end of the forwards pass。 There's our softm that calculates our probability distribution from our logics。

 This area here that we've zoomed in on is our forwards pass。

 If we pan to the right and look below into this separate thread。 we can see the backwards pass。

 The backwards pass is always pretty easy to identify for one。

 it happens in a separate thread from everything else。 And2。

 you can just type the word backward into the search bar to highlight it。

 So there's our backwards pass。 if we look at the beginning of our backwards pass in more detail。

 We can see that it starts with calculating the loss。

 And if we scroll to the right through the progress of the backwards pass。

 We can see that it ends with a convolution operation。 So the first convolution in our resnet。

 So it is indeed moving backwards through our network。 Re to our forwards pass。

 let's take a look at the very beginning and see if we can understand what the trace is showing us about what is happening。

😊，During our training step with the goal of building a better mental model for our pieytorch code that will help us identify performance bottlenecks。

 Since we're focusing on the forwards pass and at the bottom， the activity on the GPU。

 let's hide the details of the backwards pass by clicking this button。😊，So if we search for data。

 we can find the end of our data loading here， and then we can see the first operation on the GPU for our forwards pass here with this mem copypy from the CPU to the GPU。

 So from the host to the device the most important thing to know about neural network code that is accelerated by GPUs is that activity happens asynchronously between the CPU and the GP while some components of the CPU are working hard to get information about the batch from the CPU's memory over to the GP's memory。

 the main python process is continuing to move through the computation graph you find in your module to figure out what operation it's going to want to apply to that data once it's on the GPU we can see this information here after the copy is finished a kernel is launched an operation written effectively in C to run quickly on the GPU is kicked off the names for these are pretty complicated some of the information in them is。

Not documented at all because it's proprietary has to do with which fast matrix algorithms are incorporated into Nvidia's libraries。

 You can pull out sometimes basic information about whether it's a convolution or a matrix multiplication or some special neural network operation like dropout from the name what's more important is to connect it back to the highlevel operation in Python and in Pytorch that we wrote that kicked off this kernel so we can click highlight this incoming flow to see which CPU operations led to that GP kernel being executed。

 So let's go a little further find one of the longer running operations click to see its flow and it looks like it's coming from our co2 D pytorch module。

 So in general with these flow events which you want to see is that the CPU is deciding what work to do picking out which kernels to run long before they're actually executed on the CPU。

 And the reason why is that the GP can never get ahead of what the CPU is doing the CPU is in。

Drivers C， but the CPU is not doing the hard work。 The GPU is doing the hard work and it's also more expensive。

 So we never want our expensive， hardworking fast accelerator to be waiting on its slower moving host So the way that we can tell that the GPU right here is not waiting on the CPU is that there are no or very few areas in this GPU stream that are gray that are the color of the background we see colored blocks indicating that kernels are being executed on the GPU。

 So this is synonymous with the GPU utilization metric that we see reported in the systems tab on W andb that we see when we run NviDdia Smi from the command line or that's reported in the top level of the Pytorch profile or tensor board integration For example of slightly worse GPU utilization where the GPU is waiting on the host。

 Check out the atom step component by searching in the search bar。

 you'll see on the bottom there there's a lots of tiny colored。

Bars indicating that kernels are running for short periods interspersed with large areas of gray。

 So this is a potential target for optimization。 if we want to make things run faster。 Unfortunately。

 solutions to this are not a stable component of pytorch at the time of this recording。

 and so there's not a simple plug and play replacement to resolve this issue。

 In addition to visually assessing the GPU stream to see where there are blocks of color and where there are blocks of gray we can also use the search to find moments of synchronization between the CPU and the GPU where the CPU realizes that it needs the results of work from the GPU in order to decide what to do next it must then wait for the GP to finish。

 report that it's done sometimes transfer information back to the host and then during that time the GP will set idle。

 So an example of this occurs for our res transformer in the handoff between the resnet component and the transformer component essentially because we're asking for more detailed type information than the CPU。

😊，has available without waiting for the GPU to finish calculating So this is another potential target for optimization。

 but given that we're at 90% GPU utilization， we're already doing well enough on this hardware that it's probably better to think about looking into faster GPUs that could crunch through this the same operations in less time rather than making possibly brittle changes to our code that will break if the problem definition changes in pursuit of small amounts of improved performance。

On top of walking through this tour in the lab notebook。

 we also pull out some additional suggestions for the first things to try when you need to troubleshoot your model。

 One piece of really great advice that I originally got from some of the folks at openaiI is to only focus on optimizing your forwards pass not your backwards pass。

 The backwards pass is dependent on what happens in the forwards pass。

 So optimizations to one will generally improve the other。

 but the forwards pass is easier to reason about and more under our control。 In addition。

 the forwards pass is the trickier bit。 This is the part where pytorch is building up the computation graph for all the operations that you want to do。

 Once that's done and you ask for gradients。 the compute graph is fixed and so pytorch is much more able to do optimizations and operate more quickly So in a sense。

 the backwards pass takes care of itself。 Another general suggestion you'll hear is to make your batches as large as possible while still fitting in your GPU Ram。

😊，This time spent in Python whole milliseconds to look up which kernel to use for this particular input is more easily dwarfed by the time it takes to actually perform those operations if it's on a matrix with millions。

 even billions of entries that might take milliseconds seconds to execute on the GPU if it's a matrix with only a few entries than it。

 it'll execute in nanoseconds much， much faster than Python can even finish the function execution that kicked off that GPU work。

 So hopefully this tour in the lab in the video have helped you build better intuition for the really unintuitive features of Pythtorch code that make optimization so difficult most folks building a deep learning application across the entire stack will become an expert in this kind of performance optimization because you can't be an expert in every piece of the stack but understanding the execution model a bit better and being able to read traces will help you both。



![](img/7d640d096b680199f7cb7eb3a8deaf70_4.png)

Write pytorch code that doesn't hit any of these sharp edges and resolve them when they arise。

 We close out with some suggestions for how to reach really high GP utilization。

 getting a nicer CPU than you might expect that you need looking into data loader best practices to avoid bottlenecking and other basic things that you can do that are the highest leverage ways to make sure you're not leaving a ton of potential performance on the table or spending dollars per hour for GPus to sit idle。

 In a way that could be resolved with a change to a single line of Python。 In my experience。

 I found that a quick audit like this can identify those kinds of lines and give speed ups from 20 to 50%。

 So we've just got two quick exercises here。 I would suggest the most important exercise is to just spend some time with that trace。

 If you're running on your own hardware you might want to compare and contrast with coabab to see what kinds of differences you can observe。

 We've selected arguments and designed our model to work well。😊，OnThe cards and setups that we use。

 But you might find different settings。 optimizeimise it for the setup that you have。 If you do。

 we'd love to hear about it。 Make a WB report out of it。 share it with us on Twitter。

 post it here on YouTube， and we'll check it out。 We also suggest that you compare what it looks like when you turn off multi processing for data loading。

 setting numb workers to0。 instead of the default value that we select0 is actually ptorrchches default value。

 and you'll see that it leads to very low GPU utilization in almost all circumstances。

 We also have a quick exercise you can try out these linting tools and write yourself a quick test。

 So that's everything for the troubleshooting and testing lab covering some of the basics of the lowest hanging fruit and the highest leveraged things to tackle in making sure your networks train fast。

 run correctly and have clean maintainable code。 Thanks for watching。

 post any questions you have in the comments and happy troubleshooting。😊。



![](img/7d640d096b680199f7cb7eb3a8deaf70_6.png)