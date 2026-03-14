# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p11 P11_Lab_06-数据标注 -BV1k4YXznEjw_p11-

![](img/b89e6635fc33ded509f79b1a63d96868_0.png)

Hey， friends， welcome back to full stack deep learning。 Today。

 we're gonna to be walking through lab number 6 on data annotation。 This lab has two pieces。

 In the first piece， we understand how we go from the data that is save to disk and persisted the data that is stored into data that could be used by our neural networks in pytorch and pytorrch lightning。

 Then in the second half of the lab， we walk through how we go from raw data that we can measure from collect from the world and turned it into annotated data that's ready to be stored and then use downstream to train our machine learning algorithms。

 So the first half of the lab walks through in more detail just how we've been getting these images of handwritten text along with labels to train the neural networks in these past labs。

 So there's a lot of detail in this section about just precisely how things are structured in this particular case。

 And the important part of this lab is not necessarily all of those details about how we use X M L。😊。

Re the annotations or how we combine annotations for subcomponents of the image together to get an annotation for the entire image。

 The important thing in this section is to get a flavor for what the actual more raw data looks like in a particular case。

 and some aspects of that flavor may be shared with other dataset sets that you use， for example。

 that the annotations are in a structured text format and that the inputs are stored as regular image files。

 but the precise details will obviously be different。

 The other important general principle that comes out from this section is that you want rich annotations for your data。

 the information that you gather from the humans who provide the labels for your data should be as rich as possible。

 For example， even though we're interested in the entire paragraph of handwritten text for our final task。

 It's actually extremely useful that the data is annotated at the level of individual lines and even individual words and characters。

 We make use of this to build synthetic data to augment。

Data set combining together lines from different paragraphs into new synthetic paragraphs。

 So here's what an example of a synthetic paragraph looks like Our data synthesis here isn't perfect。

 but it's good enough to help our models squeeze a little bit more learning out of the limited data that we have in general。

 data synthesis is a really underrated technique for bootstrapping your ML powered applications especially at the beginning when data is scarce and all the recent advances in image and text synthesis with neural networks using models like stable fusion and G3 mean that synthesis is only going to become an ever more important component of the training of machine learning models So in that half of the lab we walked through what annotated data looks like on disk and how that becomes something that our neural networks can train on but data we collect from the world doesn't come in that format。

 For example， it's easy to collect the images of the handwritten text just by scanning some pages and digitizing them but the annotation。

going to have to be collected manually。 The tool that we're going to spin up to do this is called labelbel Studio Our purpose here is twofold。

 One is to see what setting up this label studio web service looks like and the other is to get a chance to practice annotating data ourselves it's easy to get into the mindset that oh。

 this task that we're trying to train is super easy It's just reading this is boring。

 why should I spend any time on it， I want to spend my time developing algorithms and deploying applications but this is a mistake getting to know your data well will help you understand the task that you're asking the model to perform better and understanding the final m powered application。

 use case and users is critical for actually being able to write down the rules and process for annotation in such a way that it generates the best final model Data annotation is definitely a great place for a full stack deep learning engineer to apply their skills and their understanding of the entire pipeline。

😊，endIn this section， we're going to use some data was collected during a previous edition of full stack deep learning。

 additional handwritten text data。 The data is publicly accessible and stored on S3。

 and it's just scanned images of some forms with a printed text prompt and handwritten text。

 just like the I am data set that we've been using so far。 but without any of the annotations。

 So setting up label studio and running it from the notebook is a little bit more complex than some of the other things that we've done。

 which have just been basic Python scripts for training or pointing to URLs to look at results in Tensor board or W and B。

 We're going to be running our own web service here。 So first。

 we've got a set are username and password that we' use to log in。 label studios designed。

 is designed to be very secure to use is lots of people their data either is very sensitive。

 for example， its' data about their users or its legal data or health data。

 or even if it's not data that's sensitive because of the possibility of。To users。

 access to really highqu data is something that can be make or break for an ML powered application。

 and so it might be an important component of an organization's competitive advantage because we're setting up our own web service。

 we're gonna to have to set up a way to communicate with that web service and you can certainly do this yourself if you want to run the label studio command from the command line expose a port if you're used to this if you've done these kinds of networking cast before。

 there's nothing particularly complicated with label studio。

 but lots of M engineers don't have that much experience with web development and running services So we're going to use a tool to simplify this process a tool called Ncro Enrooc has been added to the requirements as of this lab。

 So if you are following along with the course live and doing local development on your own machine instead of coab make sure that you rerun the makePip toolsol command so that you had the latest environment。

 including this Ncrooc library in addition to bringing the library and we're going to need an NGoc account。

There's a nice free tier with some limits on how long you can run a service。

 what kinds of URLs you're allowed to use， but just for this quick demo with labelbel Studio。

 the free tier will be perfect for our needs。Encrooc sets up a tunnel so that we can communicate with a service running on our machine over the public internet without having to worry about things like firewalls or port forwarding or the kinds of things that if you're not an expert。

 if you're not practiced with networking can lead to a lot of long confusing debugging sessions while you try to figure out how to talk to the web service you're running。

 And then the last step is we're going install label studio just long enough to try it out in this lab。

 labelbel studio is really more of an application than a library。

 So it doesn't really belong in our model development environment in a practical setting。

 we'd set this up in a separate virtual environment from everything else that we're doing or maybe even run it inside of a Docker container。

 once we've got all that done， we can run this cell to kick off a label studio instance。

 should take about 30 seconds to get started once it has。

 you should be able to navigate to the Enro URL and log in with the username and password printed by the cell below。

 So once we click that link。😊。

![](img/b89e6635fc33ded509f79b1a63d96868_2.png)

Up our label studio interface， we log in with those credentials。



![](img/b89e6635fc33ded509f79b1a63d96868_4.png)

Now it's time to create a project by importing some data Back in the Jupyter interface。

 Let's see how we do that data upload。 labelbel Studio isn't built expecting that it's going to be run on the same machine with all of your data。

 your data might be distributed over lots of machines。 And in most cases。

 it's going to be stored in some cloud storage service。

 Even if that's an onprem private cloud for sensitive data。 And so in general。

 what you're going to upload is a manifest or list of URLs pointing to data。

 So we want to upload this particular file to label studio。

 This Cv file with the URLs for all of the FSDL handwriting data。

 So this file is what we're gonna upload to label studio and the simplelist way to upload it is for this file to be on the same machine as the browser that you're using to access label studio。

 So if you're developing locally， running and executing Jupyter notebooks on the same machine as your browser。

 this is easy just head to that path upload that's manifest。CSv fileIf not， for example。

 if you're running the labs on coabab， the manifest csv file is in a cloud machine and you'll need to pull it down to the machine you're running the browser on in order to upload it to label studio So here's what that looks like in coabab We check out the files tab in the files tab。

 we navigate to FSdl tax Rer 2022 Las data raw FSDl handwriting and find manifest csv and download it back in the label studio interface。

 we click create project head to the data import and upload our cv Man Each individual annotation is called a task in label studio so we want to click treat CSsv as list of tasks and then we click save to upload the file。

 So now the links to all of our data have been pulled into labelbel Studio to start labeling we need to describe the annotation task in labelbel Studios format We can just click any data point to pull up a prompt to get started。



![](img/b89e6635fc33ded509f79b1a63d96868_6.png)

Label Studio has a domainspec language for describing the annotation interfaces that looks a little bit like HTML。

 Luckily， you don't have to write the whole thing from scratch yourself。

 You can use one of their templates。 There's lots of different options here for different tasks。

 And if you don't see your task in here。 you can check out the different templates and mix and matchsh them to create something that works for your task。

 but we'll just start from the optical character recognition template here。 By default。

 we've now got this labeling interface here。 We can label regions of the image as containing text or handwriting。

 That's not a distinction that we're interested in。 So let's get rid of those two labels。

And add in our own。 We want our annotators to pick out each line of the text。

 We saw that that was useful for synthesizing data。 So let's add a line label。

 And you can see there's some nice features。 this interface。 You can click and drag you can rotate。

 you can zoom in so that you can annotate more precisely the location of the text and then enter the text in the region in this interface element down here。

 There's a few settings for this interface。 We probably do want to allow people to zoom and to rotate images。

 So let's turn those on and then save our labeling configuration。

 Now we're ready to get started and we can click on a form to start annotating。

 It's really important to spend a little bit of time with your annotation Ui。

 debugging both the user interface and seeing what kinds of edge cases or ambiguities the task has so that you can write more clear annotation instructions。

 For example， as I go to highlight this line here at the top。

 information is the resolution of uncertainty。 There's some important ambiguities to resolve。

 Do we want。😊，To get the top and bottom， left and right of every single letter。

 even if that means including things from other lines to what extent do we want to rotate our annotated region so that it follows the line of the text and then while annotating the actual content should we make our best effort to understand the text that's been written should we refer back to the typed text at the top if we're unsure of what a particular letter is if there's a misspelling should we correct that misspelling and use the printed text at the top lots of these questions can only be answered by a model developer who knows what kind of information models are going to need or someone who's thinking about the entire problem end to end Our goal here is not to design a neural network that can correct people's spelling or that given the handwritten text a human wrote on this form given a printed prompt infer what that printed prompt was。

 we want to train a model that can look at the handwritten text and determine what letters。

Actually present。 So we don't want a correct spelling。

 We don't want to use the printed text in place of the handwritten text。

 And given the current setup of our model， which doesn't have an output token。

 that means unsure we want our annotators to make a best effort at annotating every letter。

 Also looking at this interface， I've noticed that there is a polygon region selector included here that allows annotators to select not just rectangles but more precise polygons to cover a line。

This is an interesting case because you might argue that being able to have those finer grained information about exactly where the text is is useful and you can always replace the polygon with the smallest rectangular region that covers it instead。

 but all of our data handling code was written assuming that we would have rectangular regions。

 So let's go ahead and get rid of that option in the labeling UI。

Heading back to settings and the labeling interface。

 Let's take a look at the actual code for that interface。

 And even without reviewing the actual documentation for it。

 it's pretty clear the polygon tool is coming from line 10 here。 Let's delete it。 Additionally。

 you'll see there's an instructions tab where you can start writing these labeling instructions that say things like cover all text with your annotation and indicate how to resolve ambiguities。

 you'll want to be really precise in these instructions in order to get the highest quality annotations out of your labelers。

 So I strongly recommend taking 15 or 20 minutes to click through some of these forms and annotate them。

 annotate at least two or three beginning to end all of the lines in the handwritten text so that you can get a sense for what the task looks like in its entirety。

 you'll also want to look through more of the data for some of these edge cases that come up in the exercise suggests that you take a look at a few particular forms that have interesting issues。

 If you don't spend time getting。😊，To know your data before you start to build your models。

 before you start to create your full ML powered application。

 you will eventually as part of debugging problems with your models。

 debugging problems with your data pipeline， find yourself back here at the data source。

 trying to understand some weird misbehavior or subtle issue。



![](img/b89e6635fc33ded509f79b1a63d96868_8.png)

So that's the first suggested exercise。 The second exercise suggests trying a slightly more complicated way to hook up data to labelbel Studio by connecting it to cloud storage in S3。

 doinging so requires you to create an AWS account and follow some instructions from the label Studio docs。

 This gets you a lot closer to how data is set up in label Studio for actual production applications。

Lastly， if you're running this notebook locally， this teardown cell is important。 at the end here。

 it removes label studio from the environment and gets us back to our model development environment。

 which is what we're going to need in future labs。 It also shuts down that label studio service that you aren't running a web server pointed at the public Internet for longer than just trying it out for a quick demo。

 That's everything for this data annotation lab that walks you through how to take in raw data。

 annotate it by spinning up your own annotation web service and then convert it to a form that's ready to be used by neural networks and Pytorch And the next lab will jump back。

 jump back to where we left off on model development and see how we deploy our trained models into production。

😊。

![](img/b89e6635fc33ded509f79b1a63d96868_10.png)