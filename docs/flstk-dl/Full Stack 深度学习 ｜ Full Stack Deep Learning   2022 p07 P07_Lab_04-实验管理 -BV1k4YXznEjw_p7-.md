# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p07 P07_Lab_04-实验管理 -BV1k4YXznEjw_p7-

![](img/c68fc77b75323b7cb581abe809627f10_0.png)

Welcome back to Fullt Deep Learning。 We're now on Lab4 on experiment management， Let's dive in。

In this lab， we'll learn why experiment management is so important for M L model development and then how we use experiment management in developing the text recognizer。

 And then finally， we'll go in detail through workflows for a tool called weights and biases to do experiment management for M L model development。

😊，In order to understand why experiment management is so important for machine learning model development。

 let's kick off an experiment。 So this cell here trains a new type of model on a new type of data。

 so let's kick this cell off and see what happens while our model is training immediately lots of information starts getting printed command line we're seeing information about what's running on our machine information about our model and then metric information starts streaming in about performance on the training set performance on the validation set and all of this information is important and useful for knowing what's going on while our model is training while it's learning from the data。

 But if we don't do any further instrumentation of our training process。

 all this information is going to be lost。 you can already see that the printed values for metrics like the loss are being overwritten during training And if I restart this notebook。

 the output will disappear。 or if I'm running this in the command line and I close that window。

 the output will be gone and I might not be able to。

create what the arguments were that I put into my script to train my model。

 We really need to save all of this information to log it and to attach to those logs。

 additional metadata， like the state of our Git repository or the timestamps at which values were measured so that we can correlate our measurements to each other。

 preserve them over time and maintain a record of what's going on in our experiments。

 In the bad old days， you would just do this with Google sheets or some other ad hoc solution。

 Maybe you'd write it yourself。 And this is just a lot of time and effort on something that's not really central to the Ml model development process。

 It's important for enabling that process。 but it's not directly connected with what we're doing in model development。

 It's exactly the kind of thing that we want to use frameworks and libraries that already incorporate lots of best practices and solid engineering and allow us to port our code between projects more easily。

 So the classic tool for tracking what's。😊，On during machine learning experiments is tensor board。

 And in fact， we've already been using it when we've been doing our training runs。

 We've been logging all kinds of information so that is accessible via tensor board。

 We just haven't been looking at it。 So this section of cells goes through pulling up tensor board and taking a look at some of the information logged in our experiments。

 Tensor board is kind of like Jupiter in that it runs separately from the other things that we're doing。

 It's a separate service that we launch and then look at from another process。

 We can do it just inside the notebook with a little bit of notebook magic。

 but the same commands also work from a terminal， you'll just need to open up a browser and point it at the appropriate host and port。

 Let's run these cells and take a look at tensor board。

 So Tensor board takes all of the metrics that we log over time and can display them as charts。

 So looking through here， we can find things like our performance on the training set and the validation set。

😊，The process of model fitting Tensorboard works pretty well for looking at the results of a single experiment。

 But once you start trying to compare across multiple experiments or group across experiments。

 the user experience starts to。 Additionally， because it's this independent service。

 We need to do a lot of management。 like， for example。

 this cell here that's designed to clean up the tensor board processes that were launched previously。

 effectively with Tensor board， you're running a database to log your information to。

 then a web application on top of it that has a nice graphical user interface。

 and that's a pretty common workflow for web developers。

 but it's outside the skill set of most N engineers and ends up being kind of a distraction from what you really want to do。

 which is make good machine learning powered products and applications。😊。

There are lots of different ways to move on from just locally run Tensor board for experiment management。

 This section of the notebook walks through some of the options in detail and talks about their pros and their cons。

 What we at fullstep deep learning recommend is a tool called weights and biases。 In our opinion。

 weights and biases offers the best user experience， both in terms of developing。

 logging and adding it to your codeb and in the graphical interface for looking at your experiments and offers some of the best integrations with other tools like pytorch lightning。

 Caris and even Tensor board and offers the best tools for collaboration。

 using the information about your experiments in a team。

 And one of the biggest benefits is that if you're using one of these frameworks like pytorch lightning or hugging face or Caris。

 adding weights and biases to your existing experiment running code is super easy relative to the number of additional features that you get。

 This collection of4 or5 lines here is the bulk of our weights and。😊。

Integration into our experiment running script。 And we'll see what each of these lines of code does for us as we go through a logged experiment in the rest of this lab。

 in order to complete the rest of this notebook， you'll need a weights and biases account as with Github the free tiers very generous for work that is open to the public and the text recognizer project fits really comfortably within that free tier。

 but it can be a lot more limited for work that is private except for academic accounts Again。

 much like Github you're uncomfortable with using if you're uncomfortable with using closed governance and partially closed source tools。

 there's some other recommendations for experiment tracking including aflow。

 which is a fully open governance open source project for somebody who's trying to develop a deep learning project across the entire stack。

 the ease use of weights and biases is really important for maintaining velocity and not getting bogged down on details of how the logging is working。

 So we recommend that you at least create an account and check it out as part of these labs。

 you can run this wbi login cell to get instructions on。😊。

How to create account or to provide your API key if you already have one In this next cell。

 we're gonna launch a similar experiment to the one that we just tried with tensor board。

 but instead log information to weights and biases。

 This experiment can take between 3 and 10 minutes to run while that's happening。

 make sure to keep reading through the rest of the notebook。

 So we see some new things in our output Now， there's some information from weights and biases and in particular。

 a link to where we can find the information being logged from our run to weights and biases。

 So let's check that out。 after we've made it through at least one epoch of training and validation。

 we' be able to see these charts here。 So you'll see we've logged some metrics from validation metrics from training。

 and inputs and outputs of our model。 This information is streaming live so we can see it update in this dashboard as our experiment is running。

 As you can see， there's multiple different tabs for viewing the information from a single experiment。

 And let's walk through each of them one at a time。 We've landed on the charts。😊。



![](img/c68fc77b75323b7cb581abe809627f10_2.png)

Tab but the high levelvel information about our run is actually in the overview tab。

 So this tab has metadata information like when did the run start。

 how long did it last What operating system and python version we're using and which Git repository and what was the state of that Git repository if we scroll down to the bottom of the page。

 we can also find our configuration information， all of the command line arguments that we passed in which include our model hyperpara and our trainer parameters and some summary information about what our metrics were like most recently or at the end of the run There's also a tab for systemmetric information So not metrics about our machine learning model directly like its accuracy or its performance on some data but metrics about the system on which we are running our training like how much the CPU is being utilized or how much of our memory we are using most importantly for accelerated deep learning there's lots of stats about how。

We're using our GPU or GPU。 So this system only has a single GPU and so we can only see one line here。

 but for systems with multiple GPUs， we would see utilization， temperature。

 memory allocation and other metrics for all the GPs simultaneously This is great for quickly catching performance regressions The next tab。

 the model tab has information about our model the overview and the system metrics we get for free by using weights biases at all for our logging this model tab we get by adding a single line1an B dot watch to our logging The next tab。

 the logs tab captures the output in the terminal， So this is great for being able to catch things like error messages or warning messages that might otherwise whiz by as our output gets filled up with things logged during training。

 In fact， from this I can see that the training run in the coab notebook has actually finished because the test。

😊。

![](img/c68fc77b75323b7cb581abe809627f10_4.png)

![](img/c68fc77b75323b7cb581abe809627f10_5.png)

have been logged The next tab is the files tab， which includes some nice things like a requirements do text。

 if you're running this locally， you'll also see a environment Yaml file and if you have edited the labs relative to the Git repository。

 you'll also see a dit patch， which represents the difference between your Gi state and what's been committed to the version control system and that's super helpful for being able to catch bugs that were introduced in the middle of developing a model in the middle of creating a commit or a pull request that save my bacon more than once。

 the logs， the files the system metrics in the overview are all things that we get completely for free by including weights and biases in our experiment running script but there's also things that we get by adding a little bit extra This tab。

 the artifacts tab is one of those things It includes all of the binary files that we're generating during our training which include model checkpoints and the inputs and outputs。

😊，Model， so the image inputs and the text outputs of our text recognition system。

 These all live among the artifacts on the weights and biases page for our experiment。

 heading back to the notebook where we're running the lab。

 Let's take a look at the final set of outputs here。

 we can see those first couple lines that are new loggged by weights and biases。

 then a bunch of information that we've seen loggged previously and at the bottom。

 we'll see a quick summary and actually the history of the metrics during the run all in the standard output。

 you'll also see this maybe with a little bit less polish in the output if you run in a terminal。

 we can also see another link back to that page where we can view the results of our experiment in the browser。



![](img/c68fc77b75323b7cb581abe809627f10_7.png)

This section of the lab walks through everything that I just showed you in the weights and biases interface。

 all the different tabs， what code we added in order to get them and what they're useful for。

 one thing I wanted to point out is that on top of being able to view this information inside the weights and biases interface。

 we can also view without having to leave the notebook。

 we can embed almost any page on weights and biases into a notebook using an iframe。

 So let's see what that looks like。 So here's that same run page that we were just looking at。

 but it's now inside of our coab notebook。 This is great for being able to share information programmatically play around with which things you're looking at with Python and not have to switch context out of your notebook into a web app while you're looking at your results。

 At any time， we can also click open page in the top right to take a look at this in a full browser window。

 One more navigation tip if we click around all these are clickable and we open。😊。



![](img/c68fc77b75323b7cb581abe809627f10_9.png)

These tabs again， inside of the Jupiter notebook， we can also navigate backwards and forwards using these two arrows at the top in this section of the notebook。

 we take a closer look at those inputs and outputs that have been saved to weights and biases。

 These are not just logged as raw media but in the form of tables that associate the input image with the ground truth label and the model outputs。

 we can see we have our three columns with the input image。

 the ground truth label and a predicted label。 this model isn't trained for very long So predictions aren't very good。

 We can adjust the aesthetics of the table， clicking around making the image larger。

 we can click in on an image and look at it more closely。

 this is a pretty hard line to read terrible indiscretions。 The writing of Don Juan is what I see。

 looks like that actually is the ground truth string。 the predicted string is not exactly that。

 We can also do a little bit of exploratory data analysis in this interface example。

I can take this ground truth string and calculate its length。

Or I can create an entirely new column like one that calculates the frequencies with which letters are appearing in the model's outputs。

Looks like the models's， outputs contain the letter E and the letter T and spaces quite a bit。

 Maybe it's hopping on to just the single letter statistics of our input text。

 Getting these tables to work does require a little bit of extra code。

 everything before that that we've seen was using features built into Pytorch lightning in the Pytorch lightning weights and biases integration。

 You can check out how we implemented this in this section of the notebook that pulls in the relevant classes and methods。

😊。

![](img/c68fc77b75323b7cb581abe809627f10_11.png)

Where experiment management tools really shine relative to something like Tensor board is when you have lots and lots of experiments that are all related to some poor project like training this text recognition system。

 And you want to be able to slice and dice them， filter them， compare them。

 track information over long periods of time and share those results with others。 So in this section。

 we take a look not just at one experiment that we ran in a few minutes in a coab。

 but at a much longer running project with much longer training runs for hours on multigpU systems。

 this project was part of the debugging and feature edition work while updating the FSDl course from 2021 to 2022。

 So let's pull that page up on left， you can see all the different runs that are part of this project。

 We've actually filtered them down to just the longer training runs using the filter tool。

 And then we' kind of customize the charts here to pull out important information like which model had。

😊。

![](img/c68fc77b75323b7cb581abe809627f10_13.png)

Performance on the test set in terms of character error rate and also what all of the models。

 character error rates and losses were on the test set。We continue scrolling。

 we can also find metrics over time during training， like the training loss。 In addition。

 we made custom charts where we derive new metrics from the things that we logged like this generalization gap chart。

 though it's at the difference between the training loss and the validation loss。

 when that's strongly negative。 that means our model is heavily overfitting。

 And we can see that some of our runs have been heavily overfitting that training set。

 we can also look at and compare model inputs and outputs at the end of training on the training set and the validation set here。

 we can see that the final text recognizers doing pretty well on the training set。

 getting its predicted string to pretty closely match that ground truth string。

 we keep scrolling down and take a look at the validation data。

 We see that it also transfers pretty well to the validation set。

 at any point while going through this interface， we can also click in and take a look at a specific run。

 Now we're looking at metrics and charts and all the other information that we walked through previously。

😊，For this individual run。 So it's really easy to switch between looking at one run。

 multiple runs to go in and change the filters we're applying to look at different types of runs。

 And so we invite you to play around inside this project。

 Take a look at some of the things that we've logged and see some of these weights and biases best practices in action in this section。

 we go into a little bit more detail about how we store and version large binary files。

 including media like those input images and model checkpoints in weights and biases。

 Let's pull up the page to look at the artifacts for that quick experiment we ranm And let's go ahead and check out one of our model checkpoints。

 The specific version doesn't matter that much。 So an individual artifact paged like this one has all kinds of information about an individual version of a specific collection of files that we're logging to weights and biases。

 So an artifact is kind of like a directory more so than an individual file with a model check。😊。



![](img/c68fc77b75323b7cb581abe809627f10_15.png)

![](img/c68fc77b75323b7cb581abe809627f10_16.png)

We're really kind of just saving a single file， but in general。

 artifacts can store an entire directory of directories and files。

 and then they are linearly versioned so starting from version 0。

 the first one we logged all the way up to the latest version and with the Pytorch Lightning integration they also get some useful tags like best to identify the model that had the best performance on the metric that we're tracking and this comes just using a pytorch lightning callback that's generic across experiment management systems the model checkpoint callback when we're looking at an individual version of an artifact。

 we can see all kinds of additional metadata and data about that artifact。

 So for example we can see who created it when they did that which track experiment is associated with the creation of this artifact that's all available on this overview tab we can also customize metadata that gets logged with the artifact by default Pytorch lightning attaches all of our hyperpara from the command line and metric values。

SoIn the individual files and folders that are associated with this artifact and we can view the metadata about which runs。

 created which artifacts and which runs used which artifacts by looking in this graph view the lineage view artifact storage is available as part of the free tier of weights and biases。

 The storage limits right now in August of 2022 cover 100 gigab of artifacts and experiment data。

 it is possible to delete data。 And so if you've got good experiment hygiene and you write some scripts。

 you can remove unused artifacts And so make those 00 gigabtes run pretty far。

 There's also a nice interface where you can look at how much you're storing and compare it to your limits on W in addition to viewing the information that we've logged in that nice browser interface that we started in or in these embedded iframes inside of our notebooks。

 we can also get programmatic access to the information that we've stored in W will' want to use。😊。

API to do that。 This is a very powerful and flexible API。

 There's a whole section of the lab that walks through how to do a kind of Ml ops workflow with this API where you go from the model that is running in production all the way back to the training data that ended up in that model。

 But it could also be used for just simple things like pulling down the data that we just logged in this cell here as a panda's data frame and computing some values for our。

 maybe plotting them as well。 The API also gives us programmatic access to that graph of which runs created。

 which artifacts and which runs used， which artifacts created by other runs。

 because we're logging the data that we're seeing during training that's coming out of our Pytorch data loaders to weights and biases information that's normally ephemeral like the augmented version of a data is being tracked and stored and we can access it programmatically and this is super useful for detecting issues in your data。

😊，ssessing pipeline in your data augmentation and catching those issues and in resolving them quickly when logging。

 it's really important to include as much information as possible。

 So we had get information we had system information we had specialized things that were logging from inside of our models and it's important to log all that information because if you don't log it when you have it and it disappears。

 you can't recreate it And it's hard to know which particular piece of information is going be the critical piece that you need to resolve some bug。

 correct an outage or to fundamentally improve the performance of your Ml system。

 The downside of this is that all that information that you've logged can sometimes be really overwhelming。

 and it's especially not particularly useful to anybody outside of the project who doesn't know what all these numbers mean what all these different names mean。

 And so in order to make this information a little bit more legible。

 you want to use something that pulls the information out and reformats it。

A Jupyter notebook incorporates additional media and text around code in order to make that code more comprehensible。

 So the feature for this in weights and biases is reports。

 We've heavily used reports in updating the text recognizer from 2021。

 You can check out some of the reports that we wrote to track the work that we were doing at the link in the notebook。

 We walk through a couple different types of reports and how you might use them in an Ml model development workflow。

 So the first one we look at is the dashboard report which just grabs some structured subset of the output from one experiment。

 maybe a couple experiments and it's designed for quickly going and seeing what's going on in an ongoing experiment or quickly comparing to experiments to each other。

 So let's take a look at a dashboard report for one of our training runs。

 So you can see there's a lot less information than there are on those run pages or project pages。😊。

And there's an attempt to organize it for it to be really quick to see differences。

 to see the difference between a baseline model like these two lines up here and the current model。

 like these two lines down here， we can see that the loss is going down faster for the new model than it was in the baseline model these charts are heavily customized here in this dashboard like we've incorporated log scaling so that it's easier to see differences in the loss once the values get low and we can also bring in those system metrics so that we can compare and contrast the sort of system level performance of our code。

Another type of report is kind of like a little bit of extra documentation for a pull request。

 So when you're making a pull request and changing a bunch of model training code or a bunch of data processing code。

 it could be really difficult for somebody reviewing your code to feel confident that the changes you've made are good without seeing some additional media and charts that don't always fit well inside of a pull request and in addition。

 if you just screenshot charts and put them in the pull request。

 then it could be difficult to discover those things they're no longer connected to all the other metrics that you are logging in your experiment management system so we can use these reports in weights and biases to keep all that information together。

 So here's an example of a report I added to a PR to the FSDL code base that checks that while I'm refactoring the code I'm not changing our ability to fit data that the training loss goes down in almost the same way before。

AndAfter the refactor， including links to Github like that link in the top right and including links as we scroll down to information logged to weights and biases so that the version control system state and the state that we're tracking in our experiment management system are linked to each other。

 And then the last kind of report that we consider is one that incorporates tons of Add of additional text。

 additional context and information and effort to make really beautiful charts out of the loggged information。

 so that we can communicate information not just to other people who are reviewing our code and our pull requests or checking in on the status of our projects。

 but to people outside of our team， maybe other stakeholders inside our organization or maybe even to the broader machine learning community so there's tons of really cool examples of these on the weights and biases website。

 the one we look at here is one for the Dolly Min project now known as Cayon。



![](img/c68fc77b75323b7cb581abe809627f10_18.png)

That incorporates tons and tons of really nice， juicy details on how Dolly Min was trained and what the metrics look like during training。

 what the outputs looked like。 And this is all in the context of all the information that's logged there and you can click in at any time to take a closer look at what's become one of the most popular open machine learning models。

 In addition to running individual experiments carefully chosen where we want to see the impact of changing some single parameter or changing some single piece of code。

 There are times when we want to run lots and lots of experiments。

 and the most common workflow for that is when we want to optimize the hyperparameter of our machine learning system。

 So these are the things like the learning rate and batch size or the number of layers or really any of these configurable pieces of our system that we don't optimize by gradient descent。

 the best way that people have found to。😊，Pick what these values should be。

 you can go with values that are close to what people have found in the past you can kind of carefully build intuition around what those values should be。

 or you can just try lots and lots of them in a big sweep of hyperparameter values and in general at FSDL we recommend you just use the hyperparameter sweeping workflows built into your other tooling so if you're using sageMaker you might have tools for it if you're using ray for scaling up your distributed training there are tools for hyperparameter optimization in ray and there's tools for parameter optimization and weights and biases that are really pretty easy so in order to use the weights and biases hyperparameter optimization tool we just need to write a YaAl file So there's an example of one of these Yaml files in the lab and it's got lots of comments which makes it look maybe a little bit longer and more complex than it needs to be but effectively we need to specify what command we want to run and what the parameters are and that yam。

File basically acts to configure a controller for this hyperparameter sweep。

 This controller is a lightweight process that lives on the weights and biases servers and waits to get requests from agents that do the actual work of training our models and seeing how well those hyperparameter work so we can use the exact same code that we used for training our models in this hyperparameter sweep all we need to do is launch an agent on whatever machines we have for participating in this。

 So let's go ahead and launch one here。 So the agent will run just one set of hyperpara because we pass this argument count equals one but in general。

 you can just not provide that argument and for many types of hyperparameter sweeps this agent will just run forever trying out different values and you can just terminate at any time either from the machine or from the weights and biases interface。

 There's lots of neat features of this really， really dead simple approach to doing hyperparameter。

And one of them is that it's really trivially easy to launch two separate agents on one machine with two GPUs one agent per GPU or one agent per four GPUs。

 however you want to do it just by changing environment variables before calling this wanddi agent command So the couda visibleible devices environment variable changes which GPUs are visible to an individual process And so if you open two terminals and do coud visibleible devices equals0 in one and coud visible devices equals1 and the other。

 then your two GPUs will run independent experiments for you and you'll be able to run your hyperparameter optimizations twice as fast with very little effort。

 So there's a couple of different exercises associated with this lab So the first and the simplest one is that you can launch your own agent to contribute to a big hyperparameter sweep that we've started here at full stack deep learning trying out 10 million different potential hyperparameter combinations for this lines of text data and this CNN transformer architecture。

If we run this cell， it'll pull up a dashboard for that hyperparameter optimization sweep and if we take a look at that dashboard on weights and biases。

 we can see there are some nice charts showing the final value of the validation loss neat little chart showing which parameters had the most influence on that final value so it looks like the most influential parameter in the first 200 experiments we run is the learning rate though the transformer dimension is also showing up as important bigger transformer dimension means lower validation loss。

 there's also these really nifty parallel coordinates chart where you can filter down runs to see which ones have the worst validation losses which ones have the best validation losses and you'll see the charts above are being recalculated so we can see for the runs that had let's say the highest learning rate。

 given that we're only caring about the runs that had the highest learning rate。

 what then becomes the most important parameter for changing the value。



![](img/c68fc77b75323b7cb581abe809627f10_20.png)

loss and what impact do those parameters have So a really nice interface for just some quick exploratory analysis of what comes out of your hyperparameter optimization experiments。

 and then you can pull the data down and do a rigorous statistical dive on it or just say Yolo and pick the best performing parameters and start a big training run to contribute to that sweep just change the value of this count It takes about 30 minutes to run an individual run in the sweep to check one configuration of the hyperpara unless of course it crashes in the first 30 seconds due to out of memory errors which happens for some combinations of hyperparameter on some systems The second exercise asks you to get a little bit more familiar with the wand B SDK in Python Almost everything that we do in the full stack deep learning text recognizer codebase uses the integration with Pythtor lightning。

 but if you're interested in logging things manually like recreating our image and text logging in those structured table。

😊。

![](img/c68fc77b75323b7cb581abe809627f10_22.png)

Or if you're interested in using weight biases with a different framework that doesn't have an integration。

 then you'll need to learn some of these methods。 The third exercise invites you to try and find your own good hyperparameter for the line CNN transformer。

 I'd be especially interested to see if anybody's able to find a set of hyperparameters that are better than the ones that come out of our big hyperparameter optimization sweep if you observe any interesting phenomena while you're training this line CNN transformer。

 grab those charts， put them into a W&B report and either share it with other folks in the full stack deep learning community on YouTube or on Twitter or if you see a bug or some strange behavior。

 open an issue on Github include your W&B report or run pages and we'll take a look。😊。

The last exercise， which requires the most additional coding。

 involves using the torch metrics library to add some additional statistics logging around tensors and we'll give you a chance to see what it looks like to write the code that actually calculates the metrics that get logged to experiment management systems and do a bit of a deeper dive on metrics and logging in Pytorch lightning and torch metrics。

 So that's everything for this week's lab on experiment management。 Next time。

 we'll see how to troubleshoot the training of deep neural networks。

 how to do code quality assurance in an ML code base and how to write and run tests for our machine learning system。

 I'm looking forward to it。 See you then。😊。

![](img/c68fc77b75323b7cb581abe809627f10_24.png)