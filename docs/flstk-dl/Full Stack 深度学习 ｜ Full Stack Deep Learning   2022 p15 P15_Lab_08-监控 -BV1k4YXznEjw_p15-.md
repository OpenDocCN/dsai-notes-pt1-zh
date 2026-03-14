# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p15 P15_Lab_08-监控 -BV1k4YXznEjw_p15-

Welcome back to the full stack deep learning labs。 We are now on the final lab。

 a bittersweet moment because it's really exciting one。

 We're gonna to look at how to monitor models and determine whether our text recognizer that we've built is working in production。

 Let's check it out。 So in the previous lab， we set up a basic UI around our text recognition model and made it possible for anyone to use the model from the browser。

 So that's great。 but we didn't set up any way to figure out what our model was doing。

 We had our experiment management tools to figure out what's going on with our model during trainingee box and in and we want to do something similar with the model that's out there in production。

 And this is distinct from the kind of like basic monitoring that you might do for any application。

 any distributed system where you're going want to know about。😊。



![](img/7d3f972151cf5eb4098c64ffd2e938ac_1.png)

Which instances are running their health。And their system metrics。

 because we're using a cloud provider。 We get all that stuff for free with our setup with an E2 server frontend and a Lambda serverless backend information about both of those things ends up in AW S Cloudwa。

 So that's helpful for responding to traditional outage is。

 but it doesn't help us understand anything about how our text recognizer is behaving in the real world。

 If it predicts garbage text， it's not gonna crash。

 We aren't going to see that alert that we might see if we had a database that failed to write or a frontend server that crashed。

 So we've got to build in some special purpose monitoring to figure out whether our model' predictions are any good or not。

 Gio has some nice basic feedback collection built in。

 So it's literally just a flag to turn on a very basic version of this feedback。😊。

Looking more closely here， it's just this flagging equals true keyword argument。

 And giving that same keyword argument directly to any radio interface will add this extra little bit of user feedback collection。

 So let's take a look at this version of the text recognizer app that's available via radio right now。

 So the change to the interface is pretty small。 But if you look on the right hand side。

 you'll see three buttons that say flag is incorrect。 flag is offensive and flag is other。

 So let's submit an input and then flag its output。 so we can see how this flagging works。

 So I'm going to use the editing tools that are built into the image upload component in our radio app。

 pretty simple editing right now， just cropping。 but let's crop down to one or two lines and then crop out a little bit of text at the top and see what the model does。

 Allright， so interestingly enough， Cu off the top of that capital T in Tories gives us not only a change to。

😊，That particular character changed from a T to a1。 That's maybe a reasonable change。

 but also the R' has become a V。 See like maybe our model might know that there's an R and Tories and is using that to disambiguate what that character is。

 Its interesting behavior。 Let's flag it as other and then head back to the notebook to see what happened when we flagged that data。

 So that flag data has been stored on the local file system of the machine that's running that server。

 that front end server。 And there's a CSV file that has the inputs and outputs and some other metadata in it。



![](img/7d3f972151cf5eb4098c64ffd2e938ac_3.png)

And we can take that CSV， and we can load it with a library like pandas for manipulating tabular data in Python。

 So we can see we've got our flag there。 I like draw your attention to the handwritten text column。

 That's our input image。 And you'll see that rather than there being an image in there。

 there's a reference to a local file。 this is a very common pattern。

 binary data is not stored in the same place as other data。

 We instead store references to that binary data along with everything else like strings， integers。

 timestamps。 If we want to look at the models inputs and outputs together。

 well need to reload that data。😊，So at this point， you could play around with the model for a bit using that editor or uploading your own images and this phase of playing around with your model is a really important one for building understanding of your model and your domain。

It's easier if you're using a model that does some common everyday task。

 like reading text as opposed to a model that does a much less common task。

 like reading an ultrasound image to determine whether an organ is healthy。

 but even in cases where you might have better understanding of the domain。

 you'll probably quickly find that you run out of different ideas for different ways to probe your model。

 So to really learn more about your model， you're going to need some actual users。

 So folks who are taking the class with other people might group up into teams and play around these text recognition models to provide each other feedback。

 But even that is probably going to result in a less diverse collection of inputs to the model than what you find if you pointed this at a large user base in this 2022 edition of the course。

 we've been running our text recognitionr with flagging built in。

 So students have been playing around with the model and flagging some of the issues that they've seen。

But rather than saving that user feedback locally onto the server that's running our model。

 we've been storing that data in a service called Gantry。 So in general。

 local logging is kind of a bad idea。 You don't want to have large local logs because they increase the burden on the server that's serving your frontend application and a certain point theyll start to become a problem。

 So radioio with this flagging mechanism supports logging of this user flag data to whatever backend you want by using a flagging callback class。

 So you can design your own callbacks or use one of their callbacks just like we added features to our Pytor Lightning training using a callback system。

 So we've got a new piece of our library here。 Now a flagging dot pi file in the app radio module that includes this ganttry image to text logger that specifically does the kind of logging that we need for a model that takes in images and。

turns text。So looking at the docs for that image to text logger。

 we can see a kind of high levell description of what it's going to do。

 It's going to send data to S3， the cloud storage service in Aws。 And if you want。

 you can examine the code and you'll see that we split up into two phases first。

 we send the image data to S3。 And then we put just the URL for that S3 image along with other useful data like the model's outputs and what flag the user chose and we send all that to Gantry in a second step。

 So this is that same pattern that we had when logging things locally。

 we separate out the binary data from everything else that we're logging。

 So in the rest of the lab notebook we walk through a kind of exploratory model analysis by analogy to exploratory data analysis where we look at the behavior of this model to try and understand whether it's working well。

 And so to do that。Need to pull the data from the service that we logged it to from Gantry。

 and we do it with this Gq do query call here。 The result of that is another pandas data frame。

 And so we analyze it with the typical kind of Python data science data analysis tools。

 pandas and plotting libraries。 So that's what happens in the notebook。

 but we can do the exact same analysis more easily inside the Gantry Ui。

 So Gantry is a pretty early start much less well established than many of the other tools that we've been using model monitoring is even more on the leading edge of what people are doing with applied Ml than model deployment。

 which was less mature than experiment management， which was in turn less mature than model training。

 So this is kind of bleeding edge of tooling that we get to in this class。

 And so Gantry is actually in closed beta。 but anybody who's taking the class following along with these notebooks。

Is invited to join this be。 So follow that link if you want to be able to follow along and try your own and try your own workflows with this text recognizer data。



![](img/7d3f972151cf5eb4098c64ffd2e938ac_5.png)

So I follow one of the links in the lab notebook to the Gantry UI logged in。

 and I'm now looking at this dashboard showing data that was logged from our production text recognizer in the top right。

 you can see the time range I'm looking at I'm looking at the first two months that the text recognizer was available in the 2022 version So just to orienter cells at the top there。

 we've got a kind of timeline view， showing the user feedback events streaming in over time。

 seeing how many are coming in one of the really great things about using a service for this kind of logging whether it's Gantry or one of the more traditional monitoring tools for distributed systems is that they keep track of time for you This is actually a really hard problem and especially handling date times yourself can be a real pain and it's the sort of thing that you really want to outsource to a library or an application and a quick note。

 the interface might look a little bit different if you're checking out this video not。

When it was uploaded in September of 2022， but maybe weeks or months later， As I said。

 these tools are at the cutting edge and are really rapidly evolving。

 So scrolling down to the bottom of the screen here。

 One of the things that we were logging was which flag we users choosing were they choosing that the model was incorrect that the model's behavior was offensive or were they choosing that it was something else。

 And if we hover here， we can see that the most common choice， seems to be the incorrect category。

 So when users are complaining about our model they're complaining that it's wrong。

 not that its behavior is offensive or upsetting。 So that's a good sign。

 really this kind of behavioral monitoring of your models to make sure they're not doing something that upsets people is really critical。

 This may be surprising to people I think but one of the definitional features of these deep learning powered applications is that they work on data that humans really care about on images on tax on audio So it's a lot easier to upset。

😊，someone or hurt someone's feelings with a deep learning application than with you a database or a calculator or a email service These are really truly the kind of P0 bugs that you should be worried about they're not unlike the metrics that people use in service level agreements for traditional applications like 99th percentile latency they may not be obviously important they may not affect the bulk of interactions with your application but the negative consequences can be extremely severe。

 they can be user churn， they can be negative attention on social media or traditional media and if even taking that into account this seems still not that important to you consider that Google produces some of the highest quality deep learning models for image understanding and for text but they haven't as of the time of recording in this video put out those models as products or services even though some of their peer institutions that create these same models like。

And AI absolutely do。 And part of that is because they have had some cases in the past in which demonstrations or early betas of these deep learning powered applications have generated a ton of controversy by their behavior。

 So this is definitely something that we want to track。 but much like content moderation。

 We don't just want to find out after the fact。 We don't want to have to wait for somebody to tell us that the model has done something wrong or that's upset them。

 We want to try and find that out automatically。 So scrolling back up one of the metrics that we're calculating here on our data is this detoxify model suites obscenity score that tries to determine whether text is obscene。

 So this really does fall still in that kind of smoke test category that we discussed in the testing lab and lecture。

 So these aren't gonna to catch every way that a model can be obscene or every way that text can be distressing or create bad experiences for users。

 but it will catch some of the。😊，Worst examples and the easiest things to find。

 So we computed this metric using this feature of gantry called projections。

 which we can find over here。 The really nice thing about projections is that we don't need to think of them ahead of time because they're functions of the data that we've logged。

 We can run them in the future。 once we've realize that something is important。 In addition。

 we can run them out of the environment where our models actually doing inference。

 So we can do really expensive things like run another natural language processing model on our text。

 In the case of these detoxify models that are checking for identity attack， text or obscene text。

 we can also calculate more pedestrian metrics like the length of the text， the entropy of the text。

 we can apply these both to the ground truth strings。

 if we have them and to the output text of the model， create for comparison。

 and we can also apply them to the inputs， not just the output。

 So we have some projections the work on images here， like calculating the pixel intent。😊。

And the sizes of the images returningturn back to that timeline view where we started。

 we can see that that obscenity metric goes up a bit at one point。 So there's a bump。

 And the immediate question is， is that okay， or is that bad like a bump up here to point03 obscenity。

 does that mean our model is like slinging slurs around or does it just mean that maybe a user submitted an image that had a swear word in it。

 So we could dive onto that specific data point and we'll do some raw data analysis in a second。

 But with really large amounts of data， what you want to be able to do is to compare distributions of values So to compare the distribution of values on some stable baseline where you trust your model behavior to what's going on in production where you don't know whether the model is doing the right thing or not。

 So once your model is running stably in production。

 you'll probably have past behavior of your model to compare to last month when everything was Gucci。

 But when your models first deployed like we're looking at now。

you have to compare to is the data that you were using during model development。

 So the validation and testing data。 So we've also ingested this data into gantry so we can do a comparison。

 So heading over to the distribution tab in the same section where we were previously looking at the timeline we want to be able to compare those numbers to some acceptable baseline so we can calculate those exact same projections on data from our validation and testing environment and compare it to what's going on in production。

 So let's take a look。 So now we've got two distributions one in a darker maroon color and another in a brighter orange color and we can see and compare these two distributions to each other。

 So the orange distribution is for values observed on the test set and the maroon distribution is for the values we're observing in production。

 So this chart up here in the top left So are distribution of values on this obscenity metric here and we can see that if anything the obscenity value。

Were higher in the testing environment than they are in production。

 So if we were happy with how the model is behaving during testing。

 then we don't have any reason to believe that the model is behaving particularly poorly here in production So monitoring your model or issues that might upset users or that might result in you becoming the main character of Twitter for a day is really important especially for models that generate content that generate images or generate text。

 but it's also important to know whether the model is doing a good job， not just not causing harm。

 and so we want to be able to debug our models in this same interface。

 and we can also do this kind of model debugging workflow in the notebook In the ideal case。

 you have user feedback that allows you to calculate some of the same metrics that you used during training。

 so that might be accuracy or character error rate or even loss。

 but that's not always possible in training， we have access to ground truth labels and in production that's almost always。

Not the case。 Setting it up so that your users can provide those to you is probably a good choice for an early ML powered product。

 but in the end， users are coming to this product because they want to automate the correct answer not provided themselves So until you can get a hold of those ground truth labels and calculate those metrics from training that you care about the next best thing is to calculate values that are correlated with what you care about So this requires some amount of domain expertise usually to know what features of input images or output text might be important for detecting bugs in your model。

 One thing that's been mentioned a couple of times from the introduction of the transformer model all the way through the testing and model monitoring lectures is that these attentionbased models are very prone to repetition and one of the signs of repetition is that the output text becomes much more predictable so we can check that。

With this ganttry production that calculates the entropy of the output text。

 there are also cases where text models might produce text with entropy that's way too high。

 For example， that has a uniform distribution over characters。

 And this projection will catch that as well。 And looking at these distributions。

 we now see a more concerning difference。 We see that there's a lot more low entropy output text in production。

 the maroon distribution， then in our outputs in test， the orange distribution。

 So inside the Ganttry Ui， we can filter down to these low entropy outputs。

 And then look at the raw data， the input images and the output text。 So let's add that filter。

And then head over to the data tab to look at the raw data so we can see both the input and output data。

 the feedback flags stuff that we logged from the production application in this view。

 and we can also see all of those projections that we calculated alongside them。

 we can do typical table operations here filtering and sorting。

 but I'm going to instead focus on some of these raw data points here。 so scrolling over。

We've got our output text and our input images here， and we can see that our output text here。

 these low entropy output examples， we do have this repetition。

 and even worse it's not repetition of say full English sentences but repetition of what looks to be total gibberish。

 So there's two possibilities here。 our outputs are bad so that either means something is changed about the input output mapping。

 So the model running in the test setting is not identical to the one running in the production setting。

 or there's a difference in the inputs。 So given that we had some testing to check that the model outputs were what we expected。

 I'd put the probability that it's issue with the model lower。

 And so the first thing I'm going to check is has something changed about the inputs。

 So let's take a look at that ingested test data to see if we can observe any immediately obvious differences between that test data and the data we're getting in production。

Note that if you know your data well from having worked with this test data for a long time or in the case of a running application。

 if you're regularly checking in on the production data。

 then you can just look at the problematic data in production and have good intuition for what the differences might be so we can click these images to look at them larger see what these look like if you've been following along with these labs they should be familiar this I am handwriting database images。

 this is what they look like， dark background white text they're all exactly the same size。

 640 by 576。 we'd see they've also got all basically the exact same contrast level and generally they all look pretty similar to each other。

 Let's go back to the view where we were looking at the production data and see in what ways does the production data differ from what we see here。

So one of the things that immediately jumps out to me is that a lot of our user inputs are white backgrounds with dark text on them。

So this， for example， this goes back to our data preprocessing。

 When we set up the I am handwriting dataset sets， we actually inverted the images。

 and this was to get better stability and faster training in our network and that information was not propagated all the way up to the model running in production because it was treated as part of the data preprocessing not as part of what the model did during training。

 So as much as possible we want to avoid having preprocessing steps that change the distribution of the data in that kind of way away from what users are likely to submit that isn't incorporated into the actual model code。

 So we could try solving this problem by incorporating that inversion step into our preprocessing。

 But if we keep looking at the data will see that some of our users also do upload dark backgrounds with like text on them。

 And so fundamentally really the nature of text is that it has a high contrast against the background to make it easy to read。

 So we don't really want to incorporate。This preprocessing step into our network what we want is to train a network that can handle text that has a variety of different backgrounds。

 This is going to require some changes to our model。

 our model currently works on only grayscale images which means if we had an image of the exact same brightness everywhere but with red letters against the blue background our model would fail and it's also likely going to mean tuning hyperparameters because we selected our hyperparameters while working with a much narrower distribution of data with big numerical differences so the numerical values of the pixels are going to change really substantially in a way that might affect optimization stability or weight initialization values or any number of other differences So resolving these kinds of bugs is challenging it's going to require some knowledge of data。

 some domain expertise， some knowledge of features of input images and identifying the right fix is going to require that data expertise and also understanding of the model and the training process itself and notice that this light text dark background examples。

That are coming in the models also doing poorly here。

 And so that probably has to do with some of the other differences between the I am handwriting data and the data that users are submitting that data was collected all at one time and at one place probably similar writing implements。

 similar lighting conditions and these are all things that we can't enforce on the users of our product without making it effectively useless we were able to get the performance of our model down pretty low by doing different experiments。

 building a more sophisticated model， but we ended up with a model that did well on data drawn from the exact same distribution that we had in our benchmark。

 but no one really wants a model that just purely does well on some benchmark。

 and this is a huge issue with machine learning and probably one of the most common reasons why models that make for promising demos don't actually end up creating useful products。

 This orientation towards benchmarks with for example。

 the imagenet classification challenge and the orientation towards。😊。

Saing state of the art on specific metrics on heldout data that animates really useful features of the ML community like Cale and papers with code These things have been critical for fostering the ML community through this last decade of really rapid technological advance and research progress but some of those instincts and habits and cultural tendencies fail when it comes to making useful machine learning powered products So that's the utility of a really rich logging system that aims to capture not just the known issues but the unknown issues logging lots and lots of data logging the raw values。

 input and output and putting that into the kind of user interface where ML engineers or other stakeholders can uncover these issues and work on how to resolve them So we provide some facilities for looking at this raw data inside the notebook interface as well and in the exercises suggest that you look through and。

Discover common types of failures so you can build that kind of regression testing suite that we talked about in the monitoring and testing lectures So I just wanted to walk through a couple of those now a couple of the failure modes that I noticed in the data I encourage you to try and uncover your own So first a good number of users send printed or rendered text not just handwritten text and there's an instinct for a lot of engineers to say I only promised you a handwritten text recognize that input component is called handwritten text I said submit handwritten text to this model and get an output and so to tell people to RtFM while there's definitely a time and a place for that response in this particular case it's clear that it's very surprising to users that something that can recognize handwritten text can't recognize printed text that's not something that is true of humans who can read handwritten text and the really good news about this is that actually it's very easy to synthesize this。

Of text printed text is maybe a little harder。 getting images of printed text along with annotations will be tricky。

 but rendering text is one of the most important features of a number of applications and programming languages。

 And so it should be really easy for us to synthesize this kind of text with really high quality ground truth。

 We also trained our model on paragraphs。 The data set was called I am paragraphs that we trained on。

 And so really， we should only expect that our model can recognize handwritten paragraphs of text。

 but users seem to want to be able to pull out characters from more complicated spatial arrangements of text。

 So here's an example， somebody uploaded the architecture diagram for our text recognizer。

 And this is not organized like a paragraph。 So this is gonna to be a much tougher issue to resolve。

 We're probably gonna need to rearchitect the model really substantially in addition to collecting the kind of data that could be used to learn how to handle text with a more complex spatial distribution。

 but this is also something that we might。😊，Be able to tackle by taking our line and paragraph data and manipulating it to create new kinds of images。

 Another issue that comes up is we get text that includes symbols that are outside our character set like this check mark for reasons of convenience。

 we worked with basically an ASI character set。 but that's probably going to be too restrictive for handling the kinds of data that users put into this model。

 So this is a smaller maybe rearchitecting of our model to handle a broader variety of outputs and again。

 the need for the collection or synthesis of additional data that covers those new types of characters。

 And then lastly our users upload text with much more heterogeneous backgrounds than the solid colored backgrounds that we used in training。

 This is another one that's probably resolvable with data synthesis we can grab generic images off the Internet and place our text on top of it either the text image or rendered or synthesisthesized text and that should help close this gap so。

One that's probably resolvable purely with data synthesis and augmentation。

 So you may have noticed some themes in the suggestions that I gave for types of problems and ways to resolve them in general。

 the resolution to issues with your model is going to be changes to your data data is really what determines the quality of models far more than changes to modeling and especially more than changes to our infrastructure in our engineering setup。

 That's why when you're just getting started， one thing you're really going to want to do is either make as much use as possible of pretrained models。

 we could be using a pretrained resnet model in our resnet transformer here and to make use of data synthesis and other techniques that allow you to bootstrap small amounts of data into large amounts of data。

 So jumping back to the notebook in the notebook itself rather than walking through that user interface we create some similar charts and derive similar conclusions to what we just did in the Gantry user interface and at。



![](img/7d3f972151cf5eb4098c64ffd2e938ac_7.png)

The bottom， just above the exercises， we synthesize some of those takeaways on how we might improve this text recognition model。

 One point that I think you'll notice if you run through the lab notebook yourself directly manipulating the data frame of production and testing data is you'll see that there's a lot of fairly brittle and boilerplate code for manipulating that data frame generating those plots and you'll also notice that we bring the entire data into memory as a single data frame and that's something that's not going to scale to a really large production machine learning system So really though it's possible to do this kind of analysis in something like a notebook really the right tool for this job is a UI on top of a database So something like gantry just as the right tool for experiment management is not a bunch of data frames full of information about your experiments but rather a user interface on top of a database like Tensor board or Mflow or weight biases So just a heads up。

Interested in using Gantry to analyze your own applications and not just look at the textrer data that we've logged here。

 you'll need to apply to join the full beta by joining the waitlist so that's everything for this lab on monitoring and in fact for all the labs in the course of these labs we've gone from thinking about how to write neural networks in pytorch how to train them with lightning and how to set up a model architecture through training and experiment management。

 annotating and storing and processing data， turning the resulting model into an application that someone who's not a machine learning engineer can actually use and then finally taking steps towards closing the loop and using what happens in that application to drive the development and improvement of better models on the basis of better data So we've gone through you might say the full stack of building a deep learning application in the course of these labs So I hope you're able to take what we've learned from these labs。

and use it to create your own machine learning powered product and if you do。

 we'd love to hear about it on the full stack deep learning Twitter or in the comments below thanks a ton for working through these labs and for watching these videos。

 good luck and happy building。

![](img/7d3f972151cf5eb4098c64ffd2e938ac_9.png)