# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p10 P10_Lecture_04-数据管理 -BV1k4YXznEjw_p10-

Hey， everyone， welcome to week 4 of Full Start deep learning。My name is Sergey， I have my assistant。

 Miishka right here。There she is。

![](img/b504e8b4a14df6fda034b1e6930892f7_1.png)

And today we're going to be talking about data management。

 One of the things that people don't quite get as they enter the field of machine learning is just how much of it is actually just dealing with data。

 putting together data sets， looking at data munching data。It's like half of the problem。

And it's more than half of the job for a lot of people。But at the same time。

 it's not something that people want to do。The key points of this presentation are going to be that you should do a lot of it。

 You should spend about 10 times as much time exploring the data as you would like to。

And let it really just flow through you。And usually the best way to improve performance。

Of your model is going to be fixing your data set， adding to the data set。

 or maybe augmenting your data as you train。And the last key point is keep it all simple。

 You might be overwhelmed， especially if you haven't been exposed to a lot of this stuff before。

 there's a lot of words and terminology in different companies。 You don't have to do any of it。

 and in fact， you might benefit if you keep it as simple as possible。That said。

 we're going to be talking about this area of the MLOs landscape。

And we'll start with the sources of data。There's many possibilities for the sources of data。

 You might have images， you might have text files， you might have maybe logs， database records。

But in deep learning， you're going to have to get that data onto some kind of local file system disk right next to a GPU so you can send data and train。

And how exactly you're going to do that is different for every project， different for every company。

So maybe you're training on images and you simply download the images。

 that's all it's going to be from S3。Or maybe you have a bunch of text that you need to process in some distributed way。

 then analyze the data， select the subset of it， put that on the local machine。

 or maybe you have a nice process with a data lake that ingests logs and database records and then from that you can aggregate and process it。

So that's always going to be different， but the basics are always going to be the same。

 and they concern the file system， object storage and databases。

So the file system is the fundamental abstraction， and the fundamental unit of it is a file。

 which can be a text file or a binary file。 It's not versioned。

And it can be easily overwrittenden or deleted。And usually this is the file system is on a disk that's connected to your machine。

 maybe physically connected or maybe attached in the cloud。

Or maybe it's even the distributed file system， although that's less common now。

 and we'll be talking about directly connected disks。The first thing to know about。

Diss is that the speed of them and the bandwidth of them is a quite。Quite a range from hard disks。

 which are usually spinning magnetic disks to solid state disks。

 which can be connected through the Saa protocol or the Nvme protocol。

 And there's two orders of magnitude difference between the slowest。

 which is like Saa spinning discs and the fastest， which are NVme solid state disks。

And making these slides， I realized， okay， I'm showing you that。

 but there's also some other latency numbers you should know。

 So there is a famous document that you might have seen on the Internet originally credited to Jeff Dean。

 who I think credit that Peter Norviig from Google。But。I added human scale numbers in pers。

 So here's how it's going to go。 So if you access the L1， L2， maybe even L 3 cache of the CPU。

It's a very limited store of data， but it's incredibly fast。 It only takes a name a second to access。

 And in human scale， you might think of it as taking a second。

And then accessing GM is the next fastest thing， and it's about100 times slower。

 but it's still incredibly fast。And then that's just kind of finding something in Ram。

 But reading a whole megabyte sequentially from Ram is now。250 microseconds。

 which if the cache access took a second now it's taken two and a half days to read a megabyte from Ram。

And if you're reading a megabyte from a Sa connected SSD drive， now you're talking about weeks。

 So it's one and a half weeks。And if you're reading1 megaby of data from a spinning disk。

 now we're talking about months。And finally， if you're sending a packet of data from California across the ocean to Europe and then back。

 we're talking about years on a human scale。And 150 millisecond on the absolute scale。

And if you GPU timing info， I'd love to include it here。 so please just send it over to Fullstock。

So what format should data be stored on the local disk， If it's binary data like images or audio。

 just use the standard formats like Jpegs or MP3。That it comes in， if they're already compressed。

 you can't really do better than that。For the metadata like labels or tabular data or text data。

Compress Json or text files， just fine。Or parquette is a table format that's fast。

 It's compressed by default， as it's written and red。 It's compact， and it's very widely used。 Now。

 let's talk about object storage。I think of it as an API over the file system， where。

The fundamental unit is now an object， and it's usually binary。

 So it's maybe an image or a sound file， but it could also be a text。

We can build inversioning or redundancy into the object storage service。

So instead of a file that can easily be overriden and isn't versioned， we can say that an object。

 whenever I update it， it's actually just updating the version of it。

S3 is the is the most common example。And it's not as fast as local file system， but it's fast enough。

 especially if you're staying within the cloud。Databases are persistent。

 fast and scalable storage and retrieval of structured data systems。

The metal model that I like to use is that all the data that the database holds is actually in the Ram of the computer。

 but the database software ensures that if the computer gets turned off。

 everything is safely persisted to disk， and if it actually is too much data for RAM。

 it scales out to disk， but still in a very performant way。Do not store binary data in the database。

You should store the objects store URLs to the binary data in the database instead。

Postgress is the right choice。 It's an open source database。 And most of the time。

 it's what you should use。 For example， it supports unstructured Json and queries over that unstructured Json。

But SQLite is perfectly good for small projects。 It's a self contained binary。

 Every language has an interface to it， Even your browser has it。

And I want to stress that you should probably be using a database， most coding projects。

 like anything that deals with collections of objects that reference each other。

 like maybe you're dealing with snippets of text that come from documents and documents of authors。

And maybe authors have companies or something like that。 This is very common。

 and that code base will probably implement some kind of database。

And you can save yourself time and gain performance if you just use the database from the beginning。

And many Ml ops tools specifically are at their core databases。

 like weights and biases is a database of experiments。 Huging face model H is a database of models。

 label studio， which we'll talk about is a database of labels。Plus， obviously。

 user interfaces for generating the labels and uploading the models and stuff like that。But。

Coming from an academic background， I think it's important too。Fully appreciate databases。

Data warehouses are stores for online analytical processing。As opposed to databases。

 which are data storage for online transaction processing。And the difference I'll cover in a second。

 But the way you get data into data warehouses is another acronym called E TL， extract。

 transform load。 So maybe you have a number of data sources here。 it's like files， database。

 O TP database and some。Sources in the cloud， youll extract data， transform it into a uniform schema。

 and then load it into the data warehouse and then from the warehouse。

 we can run business intelligence， queries， we know that it's archived。

 and so what's the difference between OL appss and OLTPs like why are they different。

Software platforms instead of just using Postgres for everything。

So the difference is O laps for analytical processing are usually column oriented。

 which lets you do queries。 What's the mean length of the text of comments over the last 30 days。

And it lets them be more compact because if you're storing a column。

 you can compress that whole column in storage。And OLTPs are usually row oriented。

 and those are for queries。 select all the comments for this given user。

Data lakes are unstructured aggregation of data from multiple sources。

So the main difference to data warehouses is that instead of extract transform load。

 it's extract load into the lake and then transform later。And the trend is unifying both。

 So both unstructured andstructured data should be able to live together。

 The big two platforms for this are snowflake and data bricks。

 And if you're interested in this stuff。This is a really great。Book。

That walks through the stuff from fresh principles that I think you will enjoy。

Now that we have our data stored， if we would like to explore it。

 we have to speak the language of data， and the language of data is mostly sQL， and increasingly。

 it's also data frames。SQL is the standard interface for structured data。 It's existed for decades。

 It's not going away。 It's worth。Being able to at least read。

 and it's well worth of being able to write。And for Python， Padas is the main data frame solution。

 which basically lets you do SQL like things， but encode code without actually writing SQL。

Our advice is to become fluent in both。 this is how you interact with both transactional databases and analytical warehouses and lakes。

Pandas。Is really the workhorse of Python data science， I'm sure you've seen it。

I just wanted to give you some tips if pandas is slow on something。

 it's worth trying D data frames have the same interface。

 but they parallellyze operations over many cores and even over multiple machines。

 if you set that up。And something else that's worth trying if you have GPUs available is rapids and video rapids。

Lets you do a subset of what Padas can do， but on GPU so significantly faster for a lot of type types of data。

So talking about data processing， it's useful to have a motivational example。

 So let's say we have to train a photo popularity predictor every night。And for each photo。

 training data must include。Maybe metadata about the photo such as the posting time。

 the title that the user gave location where taken。Maybe some features of the user。

And then maybe outputs of classifiers of the photo for content， maybe style。

So the metadata is going to be in the database。The features we might have to compute from logs and the photo classifications we're going to need to run those classifiers。

So we have dependencies。 Our ultimate task is to train the photo predictor model。

 but to do we need to output data from database， compute stuff from logs and run classifiers to output their predictions。

 What we'd like is to define what we have to do and。As things finish。

 they should kick off their dependencies and everything should。Ideally， not not only be files。

 but programs and databases。We should be able to spread this work over many machines。

And we're not the only ones running this job， or this isn't the only job that's running on these machines。

 How do we actually schedule multiple such jobs。Airflow is a pretty standard solution for Python。

Where it's possible to specify the acyclical graph of tasks using Python code。

And the operators in that graph can be SQL operations or actually Python functions and other plugins for airflow。

And to distribute these jobs。The workflow manager has a queue。

Has workers that report to it will restart jobs if they fail and will ping you when the jobs are done。

Prefect is another solution that's meant to improve over airflow， it's more modern。

And Dagter is another contender for the airflow replacement。

The main piece of advice here is don't over engineerineer this。

You can get machines with many CPU cores and a ton of Ram nowadays。

And Ex itself has powerful parallelism， streaming， tools that are highly optimized。

And this is a little bit of a contrived example from a decade ago。

 but Hadoop was all the rage in 2014。 it was a distributed data processing framework。

 and so to run some kind of job that just aggregated a bunch of text files and computed some statistics over them。

The author set up a Hadoop job and it took 26 minutes to run。

 But just writing a simple eunix command that reads all the files， grips for the string。

 sorts it and gives you the unique things was only 70 seconds。

And part of the reason is that this is all actually happening in parallel。

So it's making use of your cores pretty efficiently。

 and you can make even more efficient use of them with the parallel command or here it's an argument to XArgs。

And that's not to say that you should do everything just in Ex。

 but it is to say that just because the solution exists doesn't mean that it's right for you。

 It might be the case that you can just run your stuff in a single Python script on your 32 core PC。

Feature stores you might have heard about。The situation that they deal with is all the data processing we're doing is generating artifacts that we'll need for training time。

So how do we ensure that in production， the modeler was trained。

Cs data where the same processing took place as happened during training time。

 and also when we retrain， how do we avoid recomputing things that we don't need to recompute？

So feature stores are a solution to this that you may not need。



![](img/b504e8b4a14df6fda034b1e6930892f7_3.png)

The first mention I saw feature stores were was in this blog post from Uber describing their machine learning platform。

 Michelangelo。And so they had offline training process and an online prediction process。

 and they had feature stores for both。

![](img/b504e8b4a14df6fda034b1e6930892f7_5.png)

That had to be insane sync。Teecon is probably the leading SaS。Solution to a feature stores。



![](img/b504e8b4a14df6fda034b1e6930892f7_7.png)

For open source solutions， feast is a common one。And I recently came across feature form that looks pretty good。

As well。So this is something you need， check it out if it's not something you need。

 don't feel like you have to use it。In summary。Binary data like images。

 sound files may be compressed text store as object。

Metadata about the data like labels or user activity with objects should be stored in the database。

Don't be afraid of SQL。But also know if you're using data frames。

 there are accelerated solutions to them。If dealing with stuff like logs and other sources of data that are disparate。

 it's worth setting up a data lake to aggregate all of it in one place。

You should have a repeatable process to aggregate the data you need for training。

Which might involve stuff like airflow。And depending on the expense and complexity of processing。

 a feature store could be useful。At training time， the data that you need should be copied over to a file system on a really fast local drive。

 and then you should optimize GPU transfer。So what about specifically？

Data sets for machine learning training。Hugging phase data sets is a great hub of data。

 There's over 8000 dataset sets revision vision， NLP， speech， et cetera。

So I wanted to take a look at a few example data sets。Here's one called GitHub code。

 It's over a terabyte of text。1 hundred15 million code files。The Huging F library。

 the dataset sets library allows you to stream it， so you don't have to download the terabyte of data in order to see some examples of it。

And the underlying format of the data is Parqut tables。So there's thousands of parquette tables。

 each about half a gig。That you can download piece by piece。

Another example data set is called REcaps。Pretty recently released。

12 million image texts from Redddit。The images don't come with the data you need to download the images yourself。

 make sure as you download， it's multi threadreaded， they give you example code。

And the underlying format down of the database are the images you download。

 plus JSON files that have the labels or the text that came with the images。

So the real foundational format of the data is just the JSON files。

 and there's just URLs and those files to the objects that you can then download。

Here's another example data set， common voice from Wikipedia， 14，000 hours of speech。In 87 languages。

The format is MP3 files， plus text files with the transcription of what the persons saying。

There's another interesting dataset set solution called ActiveLoop， where you can also explore data。

 stream data to your local machine， and even transform data without saving it locally。

It it has a pretty cool viewer of the data。 So here' is looking at Microsoft Coco。

 computer visions data set。And in order to get it onto your local machine， it's a simple hub upload。



![](img/b504e8b4a14df6fda034b1e6930892f7_9.png)

The next thing we should talk about is labeling。And the first thing to talk about when it comes to labeling is maybe you don't have to label data。



![](img/b504e8b4a14df6fda034b1e6930892f7_11.png)

Self supervised learning is a very important idea that you can use parts of your data to label other parts of your data。

So in natural language， this is super common right now。

 and we'll talk more about this in the foundational models lecture。But given a sentence。

 I can mask the last part of the sentence and to use the first part of the sentence to predict how it's going to end。

 But I can also mask the middle of the sentence and use the whole sentence to predict the middle。

 or I can even mask the beginning of the sentence and use the completion of the sentence to predict the beginning。

In vision， you can extract patches and then predict the relationship of the patches to each other。

And you can even do it across modalities， so open AI clip。

 which we'll talk about in a couple of weeks。Is trained in this contrastive way where a number of images。

And the number of text captions。Are given to the model。

And the learning objective is to minimize the distance between the image and the text that it came with。

And to maximize the distance between the image and the other texts。

 the and when I say between the image and the text。

 the embedding of the image and the embedding of the text。And this led to great results。

 This is one of the best vision models for all kinds of task right now。

Data augmentation is something that must be done for training vision models。

 There's frameworks that provide， including torch vision that provide you functions to do this。

 It's changing the brightness of the data， the contrast， cropping it， skewing it， flipping it。

 all kinds of transformations that basically don't change the meaning of the image but change the pixels of the image。

 This is usually done in parallel to GPU training on the CPU and interestingly。

 the augmentation can actually replace labels。So there's a paper called Simclear。

 where the learning objective is to。Extract different views of an image and maximize the agreement。

Or the similarity of the embeddings of the views of the same image。

And minimize the agreement between the views of the different images。

So without labels and just with data augmentation and a clever learning objective。

 they were able to learn a model that performeds very well for even supervised tasks。

For non vision data augmentation， if you're dealing with tabular data。

 you could delete some of the table cells to simulate what it would be like to have missing data for text。

 I'm not aware of like really well established techniques， but you could maybe delete words。

 replace words of synonyms， change the order of things。And for speech。

 you could change the speed of the file， you could insert pauses， you could remove some stuff。

 you can add audio effects like echocho。 You can strip out certain frequency bands。

Synthetic data is also something where the labels would basically be given to you for free because you use the label to generate the data。

 so you know the label， and it's still somewhat of an underrated idea that's often worth starting with。

 we certainly do this in the lab， but it can get really deep so you can even use 3D rendering engines to generate very realistic vision data where you know exactly the label of everything in the image and this was done for receipts in this project that I linked here。

You can also ask your users if you have users to label data for you。

 I love how Google Photo does this， they always ask me is this the same or different person。

 and this is sometimes called the data Flywheel right where I'mivised to answer because it helps me experience the product。

 but it helps Google train their models as well because I'm constantly generating data。But， usually。

You might have to label some data as well。And data labeling always has some standard set of features。

 there's bounding boxes， key points or part of speech tagging for text， there's classes。

 there's captions。 What's important is training the annotators so whoever will be doing the annotation。

 make sure that they have a complete rule book of how they should be doing it because there's reasonable ways to interpret the task。

So here are some examples， like if I am only seeing the head of the fox。

 should I label only the head or should I label the inferred location of the entire fox behind the rock。

That's unclear。And quality assurance is something that's going to be key to to annotation efforts because different people are just。

Differently able to adhere to the rules。Where do you get people to annotate。

 you can work with full service data labeling companies。

You can hire your own annotators probably part time and maybe promote the most， the most。

ABble ones to quality control。Or you could potentially crowd source。

 This was popular in the passive of mechanical Turk。

The full service companies provide you the software stack， the labor to do it， and quality assurance。

 and it probably makes sense to use them。So how do you pick one。

 you should at first label some data yourself to make sure that you understand the task and you have a gold standard that you can evaluate companies on。

Then you should probably take calls with several of the companies or just try them out if they let you try it out online。

Get a work sample and then look at how the work sample agrees with your own gold standard and then see how the price of the annotation compares。

Scale AI is probably the dominant data labeling solution today。

And they take an API approach to this where it' you create tasks for them and then receive results。

And there are many other annotations like label box supervisly， and there's just a million more。

Label Studio is an open source solution that you can run yourself。

There's an enterprise edition for managed hosting， but there's an open source edition that you can just run in the Docker container on your own machine。

 We're going to use it in the lab。And it has a lot of different interfaces for text， images。

 you can create your own interfaces， you can even plug in models and do active learning for annotation。

Diffgramra is something I've come across， but I haven't used it personally。

 they claim to be better than label Studio。

![](img/b504e8b4a14df6fda034b1e6930892f7_13.png)

And it looks pretty good。An interesting feature that that I've seen some software offerings have is evaluate your current model on your data。

And then explore how it performed such that you can easily select subsets of data for further labeling or potentially find mistakes in your labeling and just understand how your model is performing on the data。

 Theres aquarium learning and scale nucleus are both solutions to this that you can check out。



![](img/b504e8b4a14df6fda034b1e6930892f7_15.png)

Snorkel， you might have heard about。And it's using this idea of weak supervision where if you have a lot of data to label。

Some of it is probably really easy to label if you're labeling sentiment of text and if they're using the word wonderful。

 then it's probably positive。 So if you can create a rule that says if the text contains the word wonderful。

 just apply the positive label to it and you create a number of these labeling functions and then the software。

😊，Intelligently composes them， and it could be a really fast way to go through a bunch of data。

There's the open source project of snorkel， and there's the commercial platform。

And I recently came across rubrics， which is a very similar idea that's fully open source。



![](img/b504e8b4a14df6fda034b1e6930892f7_17.png)

So in conclusion for labeling， first， think about how you can do self supervised learning and avoid labeling。

If you need to label， which you probably will need to do。

 use labeling software and really get to know your data by labeling it yourself for a while。

After you've done that， you can write out detailed rules and then outsource to a full service company。

Otherwise， if you don't want to outsource， you can't afford it。

 you should probably hire some part time contractors and not try to crowdsource because crowdsourcing is a lot of。

Quality assurance overhead。 It's a lot better to just find a good person who can trust to do the job and just have them label。

Lastly， in today's lecture， we can talk about versioning。

I like to think of data versioning as a spectrum。Where the level0 is unversioned and level 3 is specialized data versioning solution。

So label level 1， level 0 is bad。 Okay， where you have data that just lives on the file system or is。

On S3 or in a database。And it's not versions， so you train a model， you deploy the model。

 and the problem is when you deploy the model， what you're deploying is partly the code。

But partly the data that generated the weights， right。 And if the data is not versioned。

 then your model is， in effect， not versioned。 And so what will probably happen is that your performance will degrade at some point and you won't be able to get back to a previous level of high performance。

So you can solve this with level1， each time you train。

 you just take a snapshot of your data and you store it somewhere。

So this kind of works because you'll be able to get back to that performance by retraining。

 but it'd be nicer if I could just version the data as easily as code。

 not through some separate process。And that's where we arrive at level 2。

 where we just we version data。Exactly in the same way as aversion code。

So let's say were having a data set of audio files and text transcriptions。

So we're going to upload the audio files to S3。 that's probably where they were to begin with。

And the labels for of the files we can just store in a parqutette file or a JSON file where it's going to be the S3 URL and the transcription of it。

Now。Even this metadata file can get pretty big， it's a lot of text， but you can use Git LFS。

 which stands for large file storage and we can just add them and the Git addd will version the data file exactly the same as your version your code file。

And this can totally work。You do not need to definitely go to level 3 would be using a specialized solution for versioning data。

And this usually helps you store large files directly。And it could totally make sense。

 but just don't assume that you need it right away。 if you can get away with just Git LFS。

 that would be the FSDL recommendation。If it's starting to break。

 then the leading solution for level3 versioning is DVC。

And there's a table comparing the different versioning solutions like Payderm。

 but this table is biased towards DVC because it's。

By a solution that's GitHub for DVC called DAGsHub。And the way DVC works is you set it up。

 you add your data file。And then the。Most basic thing it does is it can upload to S 3 or Google Cloud storage or whatever。

 some other network org storage， whatever you set up。 Every time you commit。

 it'll upload your data somewhere and it'll make sure it's versioned。

 So it's click a replacement for Gi L of S。 But you can go further。

And you can also record the lineage of the data， So how exactly was this data generated。

 how does this model artifact get generated so you can use DVC run to mark that and then use DVC to recreate the pipelines The last thing I want to say is we get a lot of questions at FSDL about Priphy sensitive data。

And this is still a research area， there's no kind of off the shelf solution we can really recommend。

Federated learning。Is a researcher that refers to training a global model from data on local devices without the model training process。

 having access to the local data。 So its there's a federated server that has the model and it sends what to do to local models。

 and then it syncs back the models。And differential privacy is another term。

This is for aggregating data such that even though you have the data。

 it's aggregated in such a way that you can't identify the individual points。

 so it should be safe to train on sensitive data because you won't actually be able to understand。

The individual。Points of it。And another topic that is in the same vein as learning on encrypted data。

 So can I have data that's encrypted that I can't decrypt。

But can I still do machine learning on it in a way that generates useful models？

And these three things are all research areas。 And I'm not aware of like really good off the shelf solutions for them。

 unfortunately。

![](img/b504e8b4a14df6fda034b1e6930892f7_19.png)

That concludes our lecture on data management。Thank you。

