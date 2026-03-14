# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p02 P02_Lab_Intro_and_Overview-实验室介绍与概览 -BV1k4YXznEjw_p2-

Al right，Welcome to the lab portion of the full stack deep learning 2022 horse In this video。

 we're going to go over at a high level the text recognizer application that we'll be building through the rest of these labs。

 We'll also see how to get started with the labs on Google coabb。

 and we'll get comfortable with this Jupiter notebook format that we be using for the labs。

 So we'll start off at the lab Github repository， you can find a link for this in the description of this video。

 This repository is where all of the material for the labs is saved。

 And if we just scroll down a little bit to the readme section。

 can find a description of the labs and all of these badges that say open in coabab。

 So these are what we're gonna want to click to start working on the labs。

 So let's go ahead and click this button to try out this first lab that just as an overview of the application architecture for what we're gonna to be building this time。

 Now， here I am in a Jupiter notebook environment that's being hosted by Google completely free。

 All you need is a Google account。 So you might be asked to sign in。😊。



![](img/b0f386397e60ef986e0ceeef1cdb58e9_1.png)

![](img/b0f386397e60ef986e0ceeef1cdb58e9_2.png)

Create a Google account if you don't have one。 But once that's done。

 you've got a Jupyter notebook that can run Python for you。 So let's go ahead and start running code。

 If I click there on that cell and then hit either this play button here or hit the shift enter。

 Give it a second to connect and our code is executed and pulled in our application and shown it inside of the notebook。

 These Jupyter notebooks are composed of mixed different cell。

 So you can see that as I navigate up and down using the arrow keys。

 you can see there's a highlight coming up that's moving between the different cells of the notebook。

 each cell is either a markdown cell that's got text and media in it that's describing what's going on in the code。

 or it's got just plain code in it。 And then we can see the outputs underneath the cell。

 This cell brought in this iframe class and then used it to embed this application that we're gonna build the web page for it。

 embed it directly in this notebook。 So notebooks are really great for mixing explanation。😊。

And code and interactive components may be built by that code It's really ideal for these kinds of educational settings or it makes for really awesome documentation and some folks even do all their development in notebooks so that they can have this kind of rich interactive experience at their fingertips at all time the first thing that we've done here is embed the application that we're gonna to build inside of the notebook as an iframe but just to underscore that we're building a web application here for our text recognition。

 I'm just going to go ahead and open that link This is the web page we're gonna to build a web page that looks like this one and what we see on the left is where we can put images for our text recognition system go ahead and grab one of these example images down here and submit it and see what happens So if we wait a bit for our tensors to flow。



![](img/b0f386397e60ef986e0ceeef1cdb58e9_4.png)

We get out the output。 So on the left， I read we believe that a comprehensive medical service free to the patient at the point of need and with one standard for all sick people。

 etc ceter。 let's take a look at this output。 So this is a Python string that came from our text recognition system and it says we believe that a comprehensive medical service free to the patient at the point of need。

 and with one standard。 there's an E there。 So it's not perfect。's making some mistakes。

 but you know not bad on this input， it does a pretty good job。 There's some other samples there。

 And you can， if you like upload your own inputs or edit the inputs that we have So what would happen maybe if we zoomed in on that section where we had the mistake previously。

 we can submit that and take a look looks like the error goes away。

 There's some effective context ti that's causing the network to make the mistake there。

 These interactive applications with models embedded in them are helpful even during model development hopping back to that Jupiter notebook let's continue through it and see how。

😊。

![](img/b0f386397e60ef986e0ceeef1cdb58e9_6.png)

This app gets built There's lots of text in this notebook that goes through all the stuff that I'm gonna be talking about at a high level in this video in much greater detail and talks about other choices for how we might have built this application I'm just going hit the highlights here and just strongly recommend that you check the notebook out for yourself and read through the text check out some of the links What we were just looking at above was the frontend or the user facingcing component of the application we're gonna build that entirely in Python in order to keep things as simple as possible in order to make it possible for one person to be able to build this entire application and understand the whole thing themselves but that frontends not the whole thing。

 The frontend is just the thing that we render that we show to people so that they can interact with our application The model doesn't have to be in the same place The thing that takes in images and spits out text that machine learned algorithm that model doesn't have to be in the same place as the front end for our application we put our backend in Amazon web services。

Whi is a tool for building serverless applications。

 We'll talk in detail about what that means and why you would choose that for your machine learning models。

 But the most important thing about this frontend backend distinction is that our frontend our backend are independent of each other。

 So just because we' built this frontend that looks like what we just saw that was built in Python that doesn't mean we can only use our model in that context。

 So this cell here just runs a quick ping to that backend model sends it in image and then waits for it to get back with the predicted text。

 So we use some python libraries here that are designed for working with web services。

 you can check out the comments on that code for details on how it works we sent a URL to an image and we get back the text that was in that image So I read and since this is election year in West Germany Dr。

 Adower is in a tough spot。 Joyce Egenton cables and that's what the Python string above reads So our model is able to get the text there have。

Raw string and we can interact with it with our python codes。 we can put it somewhere else。

 We can interact from the command line as well if we want do our frontend in javascript。

 We've got these two separate pieces to see how these pieces fit together and how they relate to the rest of the application。

 Let's pull up a little diagram here。 This is an interactive diagram built with Miro which a nice visualization tool that let's us see how all these pieces fit together。

 nicely embedded in this Jupyter notebook here even have to go to another web page。

 So we were starting up here at this user session So that's the person interacting with our model。

 and they're talking to that frontend server that's in Python using the library radio and that frontend servers doing a couple things。

 It's sending requests to that backend that does our predictions and AWs Lada And then you might have may have noticed of those buttons for flagging that allows users to give feedback on how the model is doing and we'll use a tool called Gantry to。

😊，Take in that feedback， analyze it and use it to improve our models。

 The information for how to run our server and how to run our backend。

 those live in a container registry on Amazon web services containers are built using Docker So this is the handoff point between the way that we build models and the way that they end up in production interacting with users So I think it's helpful at this point to actually stop and zoom back out and start over at the beginning to go from the training of models which if you've taken a class that covered deep learning and talked about building models with pietorch training on GPus that'll be familiar to you and then through the process of iteratively building a model until we're ready to put it in production so we'll approach the handout from the other direction So to start off we need to determine what kind of computers we're going to use to build our neural networks。

 and because neural networks operate by doing sequences of really large matrix multiplications in other array。

Oper that are much faster on GPUus than on CPUus。 We're gonna need GPUus to do our computing。

 That's one reason why we're using Google coabab as one of the ways to deliver the labs。

 It's because Google coab comes with free GPUus to set that up in any coabab is to go to runtime change runtime type and pick a hardware accelerator and pick GPU So it should already be the case for this lab for you。

 And to check that we actually have that GPU。 we can run this command and videodia Smi。

 that's kind of like a top command or a check of what processes are running but just for the GPU。

 so we can see we've got a GPU。 It happens to be a Tesla P100。

 you might get something different and nothing's running on it yet because we haven't done anything for building our full application。

 we use the Lambda Las GPU cloud You should check them out if you're interested in having access to more compute than just maybe your home machine or what coab provides for free because we're doing all of our really heavy work on the GPU。

😊，We actually don't need to write the majority of our model development code in some fast language In the end。

 we're going be running stuff on a GPU with lower levelve libraries anyway so it makes a lot of sense to use a language that's easier to write in even if it's a little bit slower because the performance bottleneck is the stuff that's happening in C+ on a GPU the language that people use in deep learning is Python there's really great libraries through developing neural networks in Python libraries that do GPU acceleration from Python and also have the things that we need to train neural networks like automatic differentiation so that we can run gradient descent easily so the library Pytorrch that kind of bridges the gap between Python and C+ gives us that GPU accelerated array math that we need along with a bunch of neural network primitives and architectures So the cell just demonstrates the basic way to interact with torch creating tensors or arrays manipulating the mathematically and then asking for gradients even Pytorrch is a little bit too low。

Level for when you're developing neural networks。 It doesn't include a highlevel framework for training neural networks or any of the other kinds of engineering type tasks you need like saving your work as your model is training So we use pytorch lightning as our highleve training engineering framework They've got really great documentation for pytorch lightning including a bunch of videos like this one embedded in the notebook。

 We'll talk a lot about how to use pytorch and how to use pytorch lightning in a future lab So we've got the libraries that we need to build our models What we're missing is the kind of developer tools around model building creating a machine learning model is kind of like writing code。

 we're trying to create a computer program that can do something take some inputs return some outputs It's just that this computer program happens to be a giant pile of numbers inside of tensors and so because things are similar to general software development we want developer tools but because they're so different we need slightly different developer tools One of the most important things that。

😊，Need is the ability to sort of track what's going on as we're making changes and just baseline gi for version control doesn't do a great job for that。

 So the tool that we use to solve those problems to track our experiments as we're trying out different configuration values and to track the artifacts or large binary files that we generate as we're doing those experiments we choose weights and biases so let's pull down a page from weights and biases and embedded in the notebook like we did with our application So you can see there's lots of information here there's charts that tell how our model was doing over time and across epos the inputs and outputs of our model。

 the ground truth labels and then there's all kinds of other information like system metrics included as well Lots and lots of rich information that gets logged for us using the weights and biases tool we can also take that information and turn it into nice dashboards for communicating results to people toss these on pull requests use them as blog posts to share our。

For internal communication， use them for checking on long running， training jobs。

 all kinds of things。So we incorporate that information and add annotations and additional information to make it easier to draw insights。

 Once we've trained and developed our model， we've run our experiments。

 we need to turn that into something that we can use in production Pytorch lightning will be throughout training。

 saving the current values of the weights of our model to disk。

 we store those on weights and biases cloud。 And then when they're ready to be deployed to production。

 compile them down to an artifact that's a little bit more independent that doesn't require all of our development code because we've stored that artifact on weights and biases。

 we can take a look at it。 And so our model file is saved alongside a bunch of really useful metadata and other information So we can see the lineage of this artifact what other artifacts were generated along the way to creating it and which jobs were run in order to create those We've now made it through the entirety of the application from training a model all the way up to putting it in production and interacting with。

😊，As a user so let's take a look at that diagram again。

 we've looked at all of these pieces in this short video and over the course of the labs。

 we'll see how all of these pieces are put together to create a working。

 continually improved machine learning application I'm looking forward to。



![](img/b0f386397e60ef986e0ceeef1cdb58e9_8.png)