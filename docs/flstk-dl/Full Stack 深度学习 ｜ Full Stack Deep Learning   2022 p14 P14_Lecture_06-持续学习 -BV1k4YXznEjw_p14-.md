# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p14 P14_Lecture_06-持续学习 -BV1k4YXznEjw_p14-

Hi everybody， welcome back to full stack learning This week。

 we're gonna to talk about continual learning， which is。

 in my opinion one of the most exciting topics that we cover in this class。

 continual learning describes the process of iterating on your models once they're in production so using your production data to retrain your models for two purposes first to adapt your model to any changes in the real world that happen after you train your model and second to use data from the real world to just improve your model in general So let's dive in this sort of core justification for continual learning is that unlike in academia in the real world we never deal with static data distributions and so the implication of that is if you want to use ML and production if you want to build a good machine learning powered product。

 you need to think about your goal as building a continual learning system not just building a static model So I think how we all hope this would work is the data flylywheel that we've described in this class before so as you get more users those users bring more data you can use the data to make better model that better model helps you attract even more users。



![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_1.png)

And build a better model over time and the most automated version of this。

 the most optimistic version of it was described by Andre Carpathy as operation vacation。

 If we make our continual learning system good enough。

 then it'll just get better on its own over time and we as machine learning engineers can just go on vacation and when we come back the model will be better But the reality of this is actually quite different I think it starts out okay so we gather some data we clean and label that data we train a model on the data then we evaluate the model and we loop back to training the model to make it better based on the evaluations that we made and finally we get to the point what we're done we have a minimum viable model and we're ready to ship into production and so we deploy it the problem begins after we deploy it。

 which is that we generally don't really have a great way of measuring how our models are actually performing in production So often what we'll do is we'll just spot check some predictions to see if it looks like it's doing what it's supposed to be doing and if it seems to be working then that's great we probably move on and work on some other project that is until the first problem and pops up and now unfortunately I as a machine learning engineer。

And probably not the one that discovers that problem to begin with It's probably some business user or some PM that realizes that hey。

 we're getting complaints from a user or we're having a metric thats steppedpped and this leads to an investigation This is already costing the company money because the product and the business team are having to investigate this problem eventually they are able to point this back to me into the model that I am responsible for and at this point I'm kind of stuck doing some ad hoc analyses because I don't really know what the cause of of the failure of the model is maybe I haven't even looked at this model for a few weeks or a few months maybe eventually I'm able to like run a bunch of SQL queries paste together some Jupyter notebooks and figure out what I think the problem is so I'll retrain the model I'll redeploy it and if we're lucky we can run an AB test and if that AB test looks good then deploy production and we're sort of back where we started not getting ongoing feedback about how the model is really doing in production The upshot of all of this is that continual learning is really the least well understoodto part of the production machine learning lifecycl and very few companies。

are actually doing this well in production today And so this lecture in some ways is going to feel a little bit different than some of the other lectures。

 A big part of the focus of this lecture is going to be being opinionated about how we think you should think about the structure of continual learning problems This is some of what we say here will be sort of well understood industry best practices and some of it will be our view on what we think this should look like I'm going throw a lot of information at you about each of the different steps of the continual learning process。

 how to think about improving how you do these parts once you have your first model in production and like always we provide some recommendations for how to do this pragmatically and how to adopt it gradually So first I want to give sort of an opinionated take on how I think you should think about continual learning So all defined continual learning as training a sequence of models that is able to adapt to a con stream of data that's coming in in production you can think about continual learning as an outer loop on your training process on one end of the loop is your application which consists of a。

As well as some other codeUse interact with that application by submitting requests。

 getting predictions back and then submitting feedback about how well the model did providing that prediction the continual learning loop starts with logging。

 which is how we get all the data into the loop then we have data curation triggers for doing the retraining process data formation to pick the data to actually retrain on and we have the training process itself then we have offline testing which is how we validate whether the retrain model is good enough to go into production after it's deployed we have online testing and then that brings the next version of the model into production where we can start the loop all over again each of these stages passes an output to the next step and the way that output is defined is by using a set of rules and all of these rules together roll up into something called a retraining strategy Next we'll talk about what the retraining strategy defines for each stage and what the output looks like So at the logging stage the key question that's answered by the retraining strategy is what data should we actually store and at the end of this we have an infinite stream of potentially unlabeled data that's coming from production。

And is able to be used for downstream analysis at the curation stage。

 the key rules that we need to define are what data from that infinite stream are we going to prioritize for labeling and potential retraining and at the end of the stage will have a reservoir of a finite number of candidate training points that have labels and are fully ready to be fed back into a training process at the retraining trigger stage the key question to answer is when should we actually retrain how do we know when it's time to hit the retrain button and the output of the stage is a signal to kick off a retraining job at the data formation stage the key rules we need to define are from among this entire reservoir of data what specific subset of that data are we actually going to train on for this particular training job you can think of the output of this as a view into that reservoir of training data that specifies the exact data points that are going to go into this training job at the offline testing stage the key rules that we need to define are what is good enough look like for all of our stakeholders How are we going to agree that this model is ready to be deployed and the output this。

Stage looks like something like the equivalent of a pull request。

 a report card for your model that has a clear sign-off process that once you're signed off。

 the new model will roll out into Pro and then finally at the deployment and online testing stage。

 the key rules that we need to find are how do we actually know if this deployment was successful and the output of the stage will be the signal to actually roll this model out fully to all of your users in an idealized world。

 the way I think we should think of our role as machine learning engineers。

 once we've deployed the first version of the model is not to retrain the model directly but it's to sit on top of the retraining strategy and babysit that strategy and try to improve the strategy itself over time So rather than training models dayto-day we're looking at metrics about how well the strategy is working how well it's solving the task of improving our model over time in response to changes to the world and the input that we provide is by tuning the strategy by changing the rules that make up the strategy to help the strategy do a better job of solving that task that's a description of the goal。

of our role as an ML engineer in the real world today for most of us。

 our job doesn't really feel like this at a high level because for most of us。

 our retraining strategy is just retraining models whenever we feel like it。

 And that's not actually as bad as it seems。 you can get really good results from ad hoc retraining but when you start to be able to get really consistent results when you retrain models and you're not really working on the model day to day anymore。

 then it's worth starting to add some automation。 alternatively if you find yourself needing to retrain the model more than once a week or even more frequently than that to deal with changing results in the real world。

 then it's also worth investing in automation just to save yourself time。

 The first baseline retraining strategy that you should consider after you move on from ad hoc is just periodic retraining and this is what you'll end up doing in most cases in the near term。

 So let's describe this periodic retraining strategy So at the logging stage will simply log everything a curation will sample uniformly at random from the data that we've logged up until we get the max number of data points that we're able to handle we're able to label or able to train on and then we'll label them using。



![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_3.png)

![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_4.png)

Automated tool our retraining trigger will just be periodic， so we'll train once a week。

 but we'll do it on the last month's data， for example。

 and then we will compute the test set accuracy after each training。

 set a threshold on that or more likely manually review the results each time and spot check some of the predictions and then when we deploy the model will do spot evaluations of that deployed model on a few individual predictions just to make sure things look healthy and we'll move on This baseline looks something like what most companies do for automated retraining in the real world Retraining periodically is a pretty good baseline and in fact it's what I would suggest doing when you're ready to start doing automated retraining but it's not going to work in every circumstance so let's talk about some of the failure modes。

 the first category of failure modes has to do with when you have more data than you're able to log or able to label if you have a high volume of data you might need to be more careful about what data you sample and enrich particularly if either that data comes from a long tail distribution where you have edge cases that your model needs to perform well on but those edge cases might not be caught by just doing。

unniform random sampling or if that data is expensive to label like in a human in the loop scenario where you need custom labeling rules or labeling is part of the product。

 In either of those cases， long tail distribution or human in the loop set up you probably need to be more careful about what subset of your data that you log and enrich to be used down the road。

 second category of where this might fail has to do with managing the cost of freetraining if your model is really expensive to retrain then retraining it periodically is probably not going be the most cost efficienticient way to go especially if you do in a rolling window of data every single time。

 let's say that you retrain your model every week。 but your data actually changes a lot every single day you're gonna be leaving a lot of performance on the table by not retraining more frequently you could increase the frequency and retrain every few hours but this is gonna to increase cost even further The final failure mode is situations where you have a high cost of bad predictions one thing that you should think about is every single time you retrain your model it introduces risk that risk comes from the fact that the data that。

Tining the model on might be bad in some way it might be corrupted it might have been attacked by an adversary or it might just not be representative anymore of all the cases that your model needs to perform well on So the more frequently you retrain and the more sensitive you are to failures of the model the more thoughtful you need to be about how do we make sure that we're carefully evaluating this model such that we're not unduly taking on risk too much risk from retraining frequently when you're ready to move on from periodic retraining。

 it's time to start iterating on your strategy and this is the part of the lecture where we're going to cover a grab box of tools that you can use to help figure out how to iterate on your strategy and what changes to the strategy to make。

 you don't need to be familiar in depth with every single one of these tools but I'm hoping to give you a bunch of pointers here that you can use when it's time to start thinking about how to make your model better So the main takeaway from the section is gonna be we're gonna to use monitoring and observability as a way of determining what changes we want to make to our retraining strategy and we're gonna do that by monitoring just the metrics that actually matter the most important ones for us to。



![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_6.png)

![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_7.png)

and then using all other metrics and information for debugging when we debugging issue with our model。

 that's going to lead to potentially retraining our model。

 but more broadly than that we can think of it as a change to the retraining strategy like changing our retraining triggers changing our offline tests or sampling strategies the metrics we use for observability etc then lastly another principles for iterating on your strategy is as you get more confident in your monitoring as you get more confident that you'll be able to catch issues with your model if they occur then you can start to introduce more automation into your system So do things manually at first and then as you get more confident in your monitoring。

 start to automate them let's talk about how to monitor and debug models in production so that we can figure out to improve our retraining strategy the TlDR here is like many parts of this lecture there's no real standards or best practices here yet and there's also a lot of bad advice out there the main principles that we're going follow here are we're going focus on monitoring things that really matter and also things that tend to break empirically and we're going also compute all the other signals that you might have heard of。

Drift all these other sorts of things， but we're primarily going to use those for debugging and observability。

 What does it mean to monitor a model in production。

 The way I think about it is you have some metric that you're using to assess the quality of your model like your accuracy。

 let's say and you have a time series of how that metric changes over time the question that you're trying to answer is is this bad or is this okay do I need to pay attention to this degradation or do I not need to pay attention So the questions that we need to answer are what metrics we be looking at when we're doing monitoring how can we tell if those metrics are bad and it warrants and intervention and then lastly we'll talk about some of the tools that are out there to help you with this process choosing the right metric to monitors probably the most important part of this process and here are the different types of metrics or signals that you can look at ranked in order of how valuable they are if you're able to get them the most valuable thing that you can look at is outcome data or feedback from your users if you're able to get access to this signal。

 then this is by far the most important thing to look at Unfortunately there's no one size fits all way to do this because it depends a lot on the specifics of the product。

you're building for example， if you're building a recommender system then you might measure feedback based on did the user click on the recommendation or not。

 but if you're building a self-driving car that's not really a useful or even feasible signal to gather you might instead gather data on whether the user intervened and grab the wheel to take over autopilot from the car and this is really more of a product design or product management question of how can you actually design your product in such a way that you're able to capture feedback from your users as part of that product experience and so we'll come back and talk a little bit more about this in the MLl product management lecture the next most valuable signal to look at if you can get it is model performance metrics these are your offline model metrics things like accuracy The reason why this is less useful than user feedback is because of loss mismatch So I think a common experience that many ML practitioners have is you spend let's say a month trying to make your accuracy one or two percentage points better and then you deploy the new version of the model and it turns out that your users don't care they react just the same way they did before or even worse to that new theoretically better。

version of the model there's often very little excuse for not doing this。

 at least to some degree you can just label some production data each day It doesn't have to be a ton。

 you can do this by setting up an oncall rotation or just throwing a labeling party each day where you spend 30 minutes with your teammates labeling 10 or 20 data points each even just like that small amount will start to give you some sense of how your model' performance is trending over time if you're not able to measure your actual model performance metrics then the next best thing to look at our proxy metrics proxy metrics are metrics that are just correlated with bad model performance。

These are mostly domain specific。 So for example， if you're building text generation with a language model。

 then two examples here would be repetitive outputs and toxic outputs。

 if you're building a recommender system， then an example would be the share of personalized responses if you're seeing fewer personalized responses then that's probably an indication that your model is doing something bad if you're looking for ideas for proxy metrics edge cases can be good proxy metrics if there are certain problems that you know that you have with your model if those increase in prevalence then that might mean that your model is not doing very well that's the practical side of proxy metrics today they're very domainspec either you're gonna to have good proxy metrics or you're not but I don't think it has to be that way。

 there's an academic direction I'm really excited about that is aimed at being able to take any metric that you care about like your accuracy and approximate on previously unseen data so how well do we think our model is doing on this new data which would make these proxy metrics a lot more practically useful there's a number of different approaches here ranging from training and auxiliary model to predict how well your main model might do on these。



![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_9.png)

![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_10.png)

This offline data to heuristics to human the loop methods and so it's worth checking these out if you're interested in seeing how people might do this in two or three years one unfortunate result from this literature though that's worth pointing out is that it's probably not going to be possible to have a single method that you use in all circumstances to approximate how your model is doing on out of distribution data so the way to think about that is let's say that you have you're looking at the input data to predict how the model is going to perform on those input points and then the label distribution changes if you're only looking at the input points then how would you be able to take into account that label distribution change in your approximate metric but there's more theoretical rounding for this result as well All right back to our more pragmatic schedule programming the next signal that you can look at is data quality and data quality testing is just a set of rules that you can apply to measure the quality of your data this is dealing with questions like how well does the data reflect reality how comprehensive is it and how consistent is it over time some。

of data quality testing include checking whether the data has the right schema。

 whether the values in each of the columns are in the range that you'd expect that you have enough columns that you don't have too much missing data。

 simple rules like that。 the reason why this is useful is because data problems tend to be the most common issue with machine learning models in practice。

 So this is a report from Google where they covered 15 years of different pipeline outages with a particular machine learning model。

 And the main finding was that most of the outages that happened with that model did not really a lot to do with Ml at all。

 They were often distributed systems problems or also really commonly they were data problems。

 One example that they give is a common type of failure where a data pipeline lost the permissions to read the data source that are depended on and so was starting to fail。

 So these types of data issues are often what will cause models to fail spectacularly in production。

 The next most helpful signal to look at is distribution drift。

 Even though distribution drift is a less useful signal say user feedback。

It's still really important to be able to measure whether your data distributions change So why is that Well your model's performance is only guaranteed if the data that it's evaluated on is sampled from the same distribution as it was trained on and this can have a huge impact in practice recent examples include total change in model behavior during the pandemic as words like corona took on new meeting or bugs and retraining pipelines that cause millions of dollars of losses for companies because they led to changing data distributions distribution drift manifests itself in different ways in the wild there's a few different types that you might see so you might have an instantaneous drift like when a model is deployed in a new domain or a bug is introduced in their preprocessing pipeline or some big external shift like COVID you could have a gradual drift like if user preferences change over time or new concepts keep getting added to your corpus you could have a periodic drift。

Like if your user preferences are seasonal or you could have a temporary drift like if a malicious user a attackser model and each of these different types of drift might need to be detected in slightly different ways。

 So how do you tell if your distribution is drifted The approach here we're going take here is we're going first select a window of good data that's going to serve as a reference going forward。

 How do you select that reference Well you can use a fixed window of production data that you believe to be healthy So if you think that your model was really healthy at the beginning of the month。

 you can use that as your reference window some papers advocate for sliding this window of production data to use as your reference But in practice most of the time what most people do is they'll use something like their validation data as their reference once you have that reference data。

 then you'll select your new window of production data to measure your distribution distance on there isn't really a super principled approach for how to select the window of data to measure drift on and it tends to be pretty problem specific So pragmatic solution that what a lot of people do is they'll just pick one window size or even though。

Pick a few window sizes with some reasonable amount of data so that's not too noisy。

 and then they'll just slide those windows。 And lastly。

 once you have your reference window and your production window。

 then you'll compare these two windows using a distribution distance metric So what metrics should you use Let's start by considering the one dimensional case where you have a particular feature that is one dimensional and you are able to compute a density of that feature on your reference window and your production window then the way to think about this problem is you're gonna have some metric that approximates the distance between these two distributions。

 there's a few options here the ones that are commonly recommended are the k divergence and the Ks test unfortunately those are commonly recommended but they're also bad choices。

 sometimes better options would be things like using the infinity norm or the one normm。

 which are what Google advocates for using or the Earthmors distance which is a bit more of a statistically principled approach and I'm not going go into details of these metrics here but check out the blog post at the bottom if you want to learn more about why the commonly recommended ones。

Not so good and the other ones are better。 So that's the one dimensional case。

 If you just have a single input feature that you're trying to measure distribution distance on。

 but in the real world for most models， we have potentially many input features or even unstructured data that is very high dimensional So how do we deal with detecting distribution drift in those cases One thing you could consider doing is just measuring drift on all of the features independently problem that you'll run into there is if you have a lot of features you're gonna hit the multiple hypothesis testing problem and secondly。

 this doesn't capture cross-cors。 so if you have two features and the distributions of each of those features stay the same but the correlation between the features changed then that wouldn't be captured using this type of system Another common thing to do would be to measure drift only on the most important features One heuristic here is that generally speaking it's a lot more useful to measure drift on the outputs of the model than the inputs The reason for that is because inputs change all the time your model tends to be robust to some degree of distribution shift of the inputs but if the outputs change than that might be more indicative that there's。

Problem and also outputs tend to be for most machine learning models tend to be lowerdial。

 So it's a little bit easier to monitor。 You can also rank the importance of your input features and measure drift on the most important ones。

 you can do this just heurically using the ones that you think are important or you can compute some notion of feature importance and use that to rank the features that you want to monitor Lastly。

 there are metrics that you can look at that natively compute or approximate the distribution distance between high dimensional distribution and the two that are most worth checking out there are the maximum mean discrepancy and the approximate Earth mover distance。

 The caveat here is that these are pretty hard to interpret。

 So if you have a maximum mean discrepancy alert that's triggered it doesn't really tell you much about where to look for the potential failure that cause that distribution drift。

 a more principled way in my opinion， to measure distribution drift for high dimensionsional input to the model is to use projections。

 The idea of a projection is you take some highdiional input to the model or output and image or text。

or just a really large feature vector， and then you run that through a function。

 so each data point that your model makes a prediction on gets tagged by this projection function and the goal of the projection function is to reduce the dimensionality of that input。

Then once you've reduced the dimensionality， you can do your drift detection on that lowerdial representation of the high-disal data and the great thing about this approach is that it works for any kind of data。

 whether it's images or text or anything else no matter what the dimensionality is or what the data type is and it's highly flexible there's many different types of projections that can be useful you can define analytical projections that are just functions of your input data and so these are things like looking at the mean pixel value of an image or the length of a sentence that's an input to the model or any other functions that you can think of analytical projections are highly customizable they're highly interpretable and can often detect problems in practice if you don't want to use your domain knowledge to craft projections by writing analytical functions。

 then you can also just do generic projections like random projections or statistical projections like running each of your inputs through an autoencor something like that This is my recommendation for detecting。

For highdisional and unstructured data and it's worth also just taking note of this concept of projections because we're gonna to see this concept pop up in a few other places as we discuss other aspects of continual learning distribution drift is an important signal to look at when you're monitoring your models and in fact it's what a lot of people think of when they think about model monitoring So why do we rank it so low on the list Let's talk about the cons of looking at distribution drift I think the big one is that models are designed to be robust to some degree of distribution drift The figure on the left shows sort of a toy example to demonstrate this point。

 which is we have a classifier that's trained to predict two classes and we've induced a synthetic distribution shift just shifting these points from the red ones on the top left to the bottom ones on the bottom right these two distributions are extremely different the marginal distributions in the chart on the bottom and the chart on the right hand side have very large distance between the distributions but the model performs actually equally well on the training。

as it does on the production data because the shift is just shifted directly along class fire boundary So it's kind of a toy example that demonstrates that distribution shift is not really the thing that we care about when we're monitoring our models because just knowing that the distribution has changed doesn't tell us how the models has reacted to that distribution changed and then another example that's worth illustrating is some of my research when I was in grad school was using data that was generated from a physics simulator to solve problems on realworl robots and the data that we used was highly out of distribution for the test case that we cared about and the data looked like these kind of very lowfidelity random images like on the left and we found that by training on a huge variety of these lowfidelity random images。

 our model was able to actually generalize to realwld scenario like the one on the right so huge distribution shift intuitively between the data the model was trained on and the data it was evaluated on but was able to perform well on both beyond the theoretical limitations of measuring distribution drift this can also just be hard to do in practice you have to pick window sizes correctly。

You have to keep all this data around， you need to choose metrics。

 you need to define projections to make your data lower dimensional so it's not a super reliable signal to look at and so that's why we advocate for looking at ones that are more correlated with the thing that actually matters the last thing you should consider looking at is your standard system metrics like CPU utilization or how much GPU memory your model is taking at things like that So those don't really tell you anything about how your model is actually performing but they can tell you when something is going wrong okay so this is a ranking of all the different types of metrics or signals that you could look at if you're able to compute them but to give you a more concrete recommendation here。

 we also have to talk about how hard it is to compute these different signals in practice we'll put the sort of value of each of these types of signals on the y axis and on the X axis we'll talk about the feasibility like how easy it is it to actually measure these things measuring outcomes or feedback has pretty wide variability in terms of how feasible it is to do。

Depends a lot on how your product is set up and the type of problem that you're working on measuringas model performance tends to be the least feasible thing to do because it does involve collecting some labels and so things like proxy metrics are a little bit easier to compute because they don't involve labels whereas system metrics and data quality metrics are highly feasible because there's great off shelf libraries and tools that you can use for them and they don't involve doing anything sort of special from a machine learning perspective So the practical recommendation here is getting basic data quality checks is effectively zero regret especially if you are in the phase where you're retraining your model pretty frequently because data quality issues are one of the most common causes of bad model performance in practice and they're very easy to implement the next recommendation is get some way of measuring feedback or model performance or if you really can't do either of those things that a proxy metric even if that way of measuring model performance is hacky or not scalable This is the most important signal to look at and is really the only thing that we'll be able to reliably tell。

If your model is doing what it's supposed to be doing or not doing what it's supposed to be doing and then if your model is producing lowdial outputs。

 like if you're doing binary classification or something like that。

 then monitoring the output distribution， the score distribution also tends to be pretty useful and pretty easy to do and then lastly。

 as you evolve your system like once you have these basics in place and you're iterating on your model and you're trying to get more confident about evaluation。

 I would encourage you to adopt a mindset about metrics that you compute that's borrowed from the concept of observability So what is the observability mindset we can think about monitoring as measuring the known unknowns So if there's four or five or 10 metrics that we know that we care about accuracy latency user feedback then monitoring approach would be to measure each of those signals we might set alerts on even just a few of those key metrics on the other hand。

 observability is about measuring the unknown unknowns It's about having the power to be able to ask。

Ar questions about your system when it breaks for example。

 how does my accuracy break out across all of the different regions that I've been considering What is my distribution drift for each of my features not signals that you would necessarily set alerts on because you don't have any reason to believe that these signals are things that are going to cause problems in the future but when you're in the mode of debugging being able to look at these things is really helpful And if you choose to adopt the observability mindset which I would highly encourage you to do especially in machine learning because it's just very。

 very critical to be able to answer arbitrary questions to debug what's going on with your model。

 then there's a few implications first you should really keep around the context or the raw data that makes up the metrics that you're computing because you're going to want to be able to drill all the way down to potentially the data points themselves that make up the metric that has degraded it's also as a side note helpful to keep around the raw data to begin with for things like retraining the second implication is that you can kind of go crazy with measurement you can define。

Lots of different metrics on anything that you can think of that might potentially go wrong in the future。

 but you shouldn't necessarily set alerts on each of those or at least not very or at least not very precise alerts because you don't want to have the problem of getting too many alerts。

 you want to be able to use these signals for the purpose of debugging when something is going wrong。

 Drift is a great example of this It's very useful for debugging because let's say that your accuracy was lower yesterday than it was the rest of the month。

 Well， one way that you might debug that is by trying to see if there's any input fields or projections that look different that distinguish yesterday from the rest of the month。

 those might be indicators of what is going wrong with your model。

 and the last piece of advice I have on model monitoring and observability is it's very important to go beyond aggregate metrics。

 Let's say that your model is 99% accurate and let's say that's really good but for one particular user who happens to be your most important user it's only 50% accurate。

 Can we really still consider that models to be good。

 And so the way to deal with this is by flagging important subgroups or cohorts of data。

And being able to slice and dice performance along those cohorts and potentially even set alerts on those cohorts。

 Some examples of this are categories of users that you don't want your model to be biased against or categories of users that are particularly important for your business or just ones where you might expect your model to perform differently on them like if you're rolled out in a bunch of different regions or a bunch of different languages It might be helpful to look at how your performance breaks out across those regions or languages。

 All right， that was a deep dive in different metrics that you can look at for the purpose of monitoring The next question that we'll talk about is how to tell if those metrics are good or bad。

 There's a few different options for doing this that you'll see recommended。

 one that I don't recommend， and I alluded to this a little bit before is two sample statistical tests like aKS test。

The reason why I don't recommend this is because if you think about what these two sample tests are actually doing。

 they're trying to return a P value for the likelihood that this data and this data are not coming from the same distribution。

And when you have a lot of data， that just means that even really tiny shifts in the distribution will get very very small P values because even if the distributions are only tiny bit different if you have a ton of samples。

 you'll be able to very confidently say that those are different distributions but that's not actually what we care about since models are robust to small amounts of distribution shift better options than statistical tests include the following you can have fixed rules like there should never be any null values in this column you can have specific ranges so your accuracy should always be between 90 and 95% there can be predicted ranges so the accuracy is within what in an office shelflf anomaly detector thinks is reasonable or there's also unsupervised detection of just new patterns in this signal and the most commonly used ones in practice are the first two fixed rules and specified ranges but predicted ranges via anomaly detection can also be really useful especially if there's some seasonal in your data The last topic I want to cover on model monitoring is the different tools that are available for monitoring your models the first category。

sem monitoring tools So this is a pretty mature category with a bunch of different companies in it and these are tools that help you detect problems with any software system。

 not just machine learning models and they provide functionality for setting alarms when things go wrong and most of the cloud providers have pretty decent solutions here but if you want something better you can look at one of the observability your monitoring specific tools like honeycomb or Datadog you can monitor pretty much anything in these systems and so it kind of raises the question of whether we should just use systems like this for monitoring machine learning metrics as well there's a great blog posts on exactly this topic that would recommend reading if you're interested in learning about why this is feasible but pretty painful thing to do。

 and so maybe it's better to use something that's M specific here in terms of M specific tools there' are some open source tools the two most popular ones are evidently AI and ylogs and these are both similar in that you provide them with samples of data and they produce a nice report that tells you where is their distribution shifts how your model metrics changed etc the big limitation of these tools is that they don't。

all the data infrastructure and the scale problem for you you still need to be able to get all that data into a place where you can analyze it with these tools and in practice that ends up being one of the hardest parts about this problem the main difference between these tools is that whylogs is a little bit more focus on gathering data from the edge and the way they do that is by aggregating the data into statistical profiles at inference time itself so you don't need to transport all the data from your inference devices back to your cloud。

 which in some cases can be very helpful and then lastly there's a bunch of different SAS vendors for ML monitoring and observability My startup Gantry has some functionality around this and there's a bunch of other options as well All right so we've talked about model monitoring and observability and the goal of monitoring and observability in the context of continual learning is to give you the signals that you need to figure out what's going wrong with your continual learning system and how you can change the strategy in order to influence that outcome Next we're going to talk about for each of the stages in the continual learning。

Loop what are the different ways that you might be able to go beyond the basics and use what you learn from monitoring and observability to improve those stages。

 The first stage of the continual learning loop is logging。 And as a reminder。

 the goal of logging is to get data from your model to a place where you can analyze it。

 And the key question to answer is what data should I actually log for most of us。

 the best answer is just to log all of your data storage is cheap and it's better to have data than not have it but there's some situations where you can't do that。

 for example， if you have just too much traffic going through your model to the point where it's too expensive to log all of it if you have data privacy concerns。

 if you're not actually allowed to look at your user' data or if you're running your model at the edge and it's too expensive to get all that data back because you don't have enough network bandwidth if you can't log all of your data。

 there's two things that you can do the first is profiling the idea of profiling is that rather than sending all the data back to your cloud and then using that to do monitoring or cability or retraining instead you can compute statistical profiles of your data。

On the edge that describe the data distribution that you're seeing。

 So the nice thing about this is it's great from a data security perspective because it doesn't require you to send all the data back home。

 It minimizes your storage cost。 and lastly， you don't miss things that happen in the tails which is an issue for the next approach that we'll describe And the place to use this really is primarily for security critical applications。

 The other approach is sampling and sampling you'll just take certain data points and send those back home。

 The advantage of sampling is that it has minimal impact on your inference resources so you don't have to actually spend the computational budget to compute profiles and you get to have access to the raw data for debugging and retraining and so this is what we'd recommend doing for pretty much every other application describe a little bit more detail how statistical profiles work because it's kind of interesting let's say that you have a stream of data that's coming in from two classes cat and dog and you want to be able to estimate what is the distribution of cat and dog over time without looking at all of the raw data So for example maybe in the past you saw。

thThree examples of a dog and two examples of a cat a statistical profile that you can store that summarizes this data is just a histogram so the histogram says we saw three examples of a dog and two of a cat and over time as more and more examples stream in rather than actually storing those data we can just increment the histogram and keep track of how many total examples of each category that we've seen over time and so like a neat fact of statistics is that for a lot of the statistics that you might be interested in looking at quantiles means。

Accuracy， other statistics， you can compute， you can approximate those statistics pretty accurately by using statistical profiles called sketches that have minimal size。

 So if you're interested in going on a tangent and learning more about an interesting topic in computer science。

 that's one I'd recommend checking out Next step in the continual learning loop is curation to remind you the goal of curation is to take your infinite stream of production data。

 which is potentially unlabeled and turn this into a finite reservoir of data that has all the enrichments that it needs like labels to train your model on the key question that we need to answer here is similar to the one that we need to answer when we're sampling data at log time。

 which is what data should we select for enrichment。

The most basic strategy for doing this is just sampling data randomly。

 but especially as your model gets better。 most of the data that you see in production might not actually be that helpful for improving your model and if you do this。

 you could miss rare classes or events like if you have an event that happens one time in every 10000 examples in production but you are trying to improve your model on it。

 then you might not sample any examples of that at all if you just sample randomly a way to improve on random sampling is to do what's called stratified sampling The idea here is to sample specific proportions of data points from various subpopulations So common ways that you might stratify for sampling in ML could be sampling to get a balance among classes or sampling to get a balance among categories that you don't want your model to be biased against like gender Lastly the most advanced and interesting strategy for picking data to enrich is to curate data points that are somehow interesting for the purpose of improving your model and there's a few different ways of doing this that we' cover the first is to have this notion of interesting data。

Driven by your users which will come from user feedback and feedback loops the second is to determine what is interesting data yourself by defining error cases or edge cases and then the third is select an algorithm to find this for you and this is a category of techniques known as active learning if you already have a feedback loop or a way of gathering feedback from your users in your machine learning system which really should if you can then this is probably the easiest and potentially also the most effective way to pick interesting data for the purpose of curation and the way this works is you'll pick data based on signals that come from your users that they didn't like your prediction so this could be the user churned after interacting with your model it could be that they filed a support ticket about a particular prediction the model made it could be that they click the thumbs down button that you put in your products that they change the label that your model produce for them or that they intervene with an automatic system like they grab the wheel of their autopilot system if you don't have user feedback or if you need even more ways of gathering interesting data from your system。

Then probably the second most effective way of doing this is by doing manual error analysis The way this works is we will look at the errors that our model is making we will reason about the different types of failure modes that we're seeing willll try to write functions or rules that help capture these error modes and then we'll use those functions to gather more data that might represent those error cases two subcategories of how to do this one is what I would call similaritybased curation and the way this works is if you have some data that represents your errors or data that you think might be an error then you could pick an individual data point or a handful of data points and run a nearest neighbor similarity search algorithm to find the data points in your stream that are the closest to the one that your model is maybe making a mistake on the second way of doing this which is potentially more powerful but a little bit harder to do is called projectionbased curation the way this works is rather than just picking an example and grabbing the nearest neighbors of that example instead we are going to。

an error case like the one on the bottom right where there's a person crossing the street with a bicycle and then we're going to write a function that attempts to detect that error case。

 and this could just be trading a simple neural network or it could be just writing some heuristics The advantage of doing similaritybased curation is that it's really easy and fast you just have to click on a few examples and you'll be able to get things that are similar to those examples and this is beginning to be widely used in practice thanks to the explosion of vector search databases on the market。

 It's relatively easy to do this And what this is particularly good for is events that are rare they don't occur very often in your data。

 but they're pretty easy to detect like if you had problem with your self-driving car where you havellmas crossing the road a similarity searchbased algorithm would probably do a reasonably good job of detecting otherllmas in your training set on the other hand。

 projection-based curation requires some domain knowledge because it requires you to think a little bit more about what is the particular error case that you're seeing here and write a function to detect it but it's good for more。

subtle error modes where a similarity search algorithm might be toocograed it might find examples that look similar on the surface to the one that you are detecting。

 but don't actually cause your model to fail The last way to curate data is to do so automatically using a class of algorithms called active learning the way active learning works is given a large amount of unlabeled data what we're going try to do is determine which data points would improve model performance the most if you were to label those data points next and train on them and the way that these algorithms work is by defining a sampling strategy or a query strategy and then you rank all of your unlabeled examples using a scoring function that defines that strategy and take the ones with the highest scores and send them off to be labeled I'll give you a quick tour of some of the different types of scoring functions that are out there and if you want to learn more about this then I'd recommend the blog post linked on the bottom you have scoring functions that sample data points that the model is very uncondent about you have scoring functions that are defined by trying to predict what is the error that the model would make on this data。

PoinIf we had a label for it， you have scoring functions that are designed to detect data that doesn't look anything like the data that you've already trained on So can we distinguish these data points from our training data If so maybe those are the ones that we should sample and label we have scoring functions that are designed to take a huge data set of points and boil it down to the small number of data points that are most representative of that distribution Lastly there's scoring functions that are designed to detect data points that if we train on them we think would have a big impact on training so where they would have a large expected gradient or would tend to cause the model to change its mind so that's just a quick tour of different types of scoring functions that you might implement uncertaintybased scoring tends to be the one that I see the most in practice largely because it's very simple to implement and tends to produce pretty decent results but it's worth diving a little bit deeper into this if you do decide to go down this route if you're paying close attention you might have noticed that there's a lot of similarity between some of the ways that we do data curation。

 the way that we pick interesting data points and the way that we do monitoring I think that's no coincidence。

Monitoring and data curation are two sides of the same coin They're both interested in solving the problem of finding data points where the model may not be performing well or where we're uncertain about how the model is performing on those data points So for example。

 userdriven curation is kind of another side of the same coin of monitoring user feedback metrics both of these things look at the same metrics stratified sampling is a lot like doing subgroup or cohort analysis making sure that we're getting enough data points from subgroups that are important or making sure that our metrics are not degrading on those subgroups projections are used in both data curation and monitoring to take high- dimensionmensional data and break them down into distributions that we think are interesting for some purpose and then in active learning some of the techniques also have mirrors in monitoring like predicting the loss on an unlabeled data points or using the model' uncertainty on that data point Next let's talk about some case studies of how data curation is done in practice The first one is a blog post on how openaiI trained Dolly to to detect malicious inputs。

To the model there's two techniques that they used here。

 they used active learning using uncertainty sampling to reduce the false positives for the model and then they did a manual curation actually they didn't kind of an automated way but they did similarity search to find similar examples to the ones that the model was not performing well on The next examples from Tesla。

 This is a talk I love from Andrey Carpathy about how they build data flylywheel at Tesla and they use two techniques here。

 one is feedback loops so gathering information about one userss intervene with the autopilot and then the second is manual curation via projections for edge case detection and so this is super cool because they actually have infrastructure that allows ML engineers when they discover new edge case to write an edge case detector function and then actually deploy that on the fleet。

 that edge case detector not only helps them curate data but it also helps them decide which data to sample which is really powerful The last case study I want to talk about is from Crus they also have this concept of building a continual learning machine and the main way they do that is through feedback loops So that's of。

Qu tour of what some people actually use to build these data curation systems in practice There's a few tools that are emerging to help with data curation scale nucleus and aquarium are relatively similar tools that are focused on computer vision and they're especially good at nearest neighborbased sampling at my startup Gantry we're also working on some tools to help with this across a wide variety of different applications Con recommendations on data curation random sampling is probably a fine starting point for most use cases but if you have a need to avoid bias or if there's rare classes in your data you probably should start even with stratified sampling or at the very least introduce that pretty soon after you start sampling if you have a feedback loop as part of your machine learning system and I hope you're taking away from this how helpful it is to have these feedback loops then userdrin curation is kind of a no brainer this is definitely something that you should be doing and is probably gonna be the thing that is the most effective in the early days of improving your model If you don't have a feedback loop then using confidence-based active learning is a next best bet because。

It's pretty easy to implement and works okay in practice And then finally。

 as your model performance increases， you're gonna have to look harder and harder for these challenging training points at the end of the day。

 if you want to squeeze the maximum performance out of your model。

 there's no avoiding manually looking at your data and trying to find interesting failure modes there's no substitute for knowing your data after we've curated our infinite stream of unlabeled data down to a reservoir of labeled data that's ready to potentially train on the next thing that we need to decide is what trigger are we going to use to retrain and the main takeaway here is that moving to automated retraining is not always necessary in many cases just manually retraining is good enough but it can save you time and lead to better model performance so it's worth understanding when it makes sense to actually make that move the main prerequisite for moving to automated retraining is just being able to reproduce model performance when retraining in a fairly automated fashion So if you're able to do that and you are not really working on this model very actively anymore then it's probably worth implementing some automated retraining if you just find yourself retraining this model super。

it'll probably save you time to implement this earlier when it's time to move to automated training。

 the main recommendation is just keep it simple and retrain periodically like once a week rerun training on that schedule The main question though is how do you pick that training schedule so what I would recommend doing here is doing a little bit of like measurement to figure out what is a reasonable retraining schedule and you plot your model performance over time and then compare to how the model would have performed if you had retrained on different frequencies you can just make basic assumptions here like if youre retrain you'll be able to reach the same level of accuracy and what you're gonna to be doing here is looking at these different retraining schedules and looking at the area between these curves like on the chart on the top right the area between these two curves is your opportunity costs in terms of like how much model performance you're leaving on the table by not retraining more frequently and then once you have a number of these different opportunity costs for different retraining frequencies you can plot those opportunity costs and then you can sort of run the ad hoc exercise of trying to balance where is the rate tradeoff point for us between the。

in that we get from retraining more frequently and the cost of that retraining。

 which is both the cost of running the retraining itself as well as the operational costs that we'll introduce by needing to evaluate that model more frequently That's what I'd recommend doing in practice。

 a request I have for research is I think it'd be great。

 I think it's very feasible to have a technique that would automatically determine the optimal retraining strategy based on how performance tends to decay how sensitive you are to that performance decay your operational costs and your retraining costs。

 So I think eventually we won't need to do the manual data analysis every single time to determine this retraining frequency。

 if you're more advanced， then the other thing you can consider doing is retraining based on performance triggers This looks like setting triggers on metrics like accuracy and only retraining when that accuracy dips below predefined threshold big advantages to doing this are you can react a lot more quickly to unexpected changes that happen in between your normal training schedule。

 It's more cost optimal because you can skip a retraining if it wouldn't actually improve your models。

But the big cons here are that since you don't know in advance when you're gonna be retraining。

 you need to have good instrumentation and measurement in place to make sure that when you do retrain。

 you're doing it for the right reasons and that the new model is actually doing well these techniques I think also don't have a lot of good theoretical justification and so if you are the type of person that wants to understand why theoretically this should work really well I don't think you're going to find that today and probably the most important con is that this adds a lot of operational complexity because instead of just knowing hey at 8 am I know my retraining is going live and so I can check in on that instead this retraining could happen at any time So your whole system needs to be able to handle that and that just introduces a lot of new infrastructure that you'll need to build then lastly an idea that probably won't be relevant to most of you but is worth thinking about because I think it could be really powerful in the future is online learning where you train on every single data point as it comes in it's not very commonly used in practice but one sort of relaxation of this idea that is used fairly frequently in practice is online adaptation The way online adaptation。

Works is it operates not the level of retraining the whole model itself。

 but it operates on the level of adapting the policy that sits on top of the model。 What is a policy。

 a policy is the set of rules that takes the raw prediction that the model made like the score or the raw output of the model and then turns that into the actual thing that the user sees So like a classification threshold is an example of a policy or if you have many different versions of your model that you're enssembling。

 what are the weights of those ensembles or even which version of the model is this particular requests going to be routed to an online adaptation rather than retraining the model on each new data point as it comes in instead we use an algorithm like multi-arm bandits to tune the weights of this policy online as more data comes in So if your data changes really frequently in practice or you are have a hard time training your model frequently enough to adapt to it then online adaptation is definitely worth looking into Next we've fired off a trigger to start a training job and the next question we need to answer is among。

All of the labeled data in our reservoir of data Which specific data points should we train on for this particular training job most of the time in deep learning we'll just train on all the data that we have available to us but if you have too much data to do that then depending on whether recency of data is an important signal to determine whether that data is useful you'll either slide a window to make sure that you're looking at the most recent data therefore in many cases the most useful data or we'll use techniques like sampling or online batch selection if not and a more advanced technique to be aware of that is hard deck in practice today is continual fine tuning and we'll talk about that as well so the first option is just to train on all available data so you have a data that you'll keep track of that your last model was trained on then over time between your last training and your next training have a bunch of new data come in and you'll curate some of that data then you'll just take all that data you'll add it to the data and you'll train the new model on the combined data So the keys here are you need to keep this data version controlled so that you know。

Data was added at each training iteration and it's also important if you want to be able to evaluate the model properly to keep track of the rules that you use to curate that new data So if you're sampling in a way that's not uniform from your distribution。

 you should keep track of the rules that you use to sample so that you can determine where that data actually came from second option is to bias your sampling toward more recent data by using a sliding window the way this works is at each point when you train your model you look backward and you gather a window of data that leads up to the current moment and then at your next training you slide that window forward and so there might be a lot of overlap potentially between these two dataset sets but you have all the new data or like a lot of the new data and you get rid of the oldest data in order to form the new data a couple of key things to do here are it's really helpful to look at the different statistics between the old and new datasets to catch bugs like if you have a large change in a particular distribution of one of the columns that might be indicative of a new bug that's been introduced and one challenge。

That you'll find here is just comparing the old and the new versions of the models since they are not trained on data that is related in a very straightforward way if you're working in a setting where you need to sample data。

 you can't train on all of your data but there isn't any reason to believe that recent data is much better than older data then you can sample data from your reservoir using a variety of techniques the most promising of which is called online batch selection normally if we're doing stochastic gradient descent then what we do is we would sample mini batches on every single training iteration until we run out of data or until we run out of compute budget in online batch selection instead what we do is before each training step we sample a larger batch like much larger than the mini batch that we ultimately want to train on we rank each of the items in a mini batch according to a label awareware selection function and then we take the top end items according to that function and train on those the paper on the right describes a label awareware selection function called the Redable holdout loss selection function that performs pretty well on some relatively large。

Dataets and so if you're going to look into this technique。

 this is probably where I would start The last option I will discuss。

 which is not recommended to do today is continual fine tuning。

 The way this works is rather than retraining from scratch every single time instead just only train your existing model on just new data The reason why you might want to do this primarily is because it's much more cost effective the paper on the right share some findings from grubhub where they found a 45x cost improvement by doing this technique relative to sliding windows but the big challenge here is that unless you're very careful it's easy for the model to forget what it learned in the past so the upshot is that you need to have pretty mature evaluation to be able to be very careful that your model is performing well on all the types of data that it needs to perform well on before it's worth implementing something like this So now we've triggered retraining we've selected the data points that are going go into the training job we've trained your model run our hyperparameter sweeps if we want to and we have a new candidate model that we think is ready to go into production the next step is to test that model the goal of this stage is to produce a report。

Our team can sign off on that answers the question of whether this new model is good enough or whether it's better than the old model and the key question here is what should go into that report Again。

 this is a place where there's not a whole lot of standardization but the recommendation we have here is to compare your current model with the previous version of the model on all the following all of the metrics that you care about all of the slices or subsets of data that you flag is important all of the edge cases that you've defined and in a way that's adjusted to account for any sample and bias that you might have introduced by your curation strategy an example of what such a report could look like as the falling across the top we have all of our metrics in this case accuracy precision and recall and then along the left are all of the data sets and slices that we're looking at so the things to notice here are we have our main validation set which is like what most people use for evaluating models but rather than just looking at those numbers in the aggregate we also break it out across a couple of different categories in this case the age of the user and the age of the account that belong to that user and then below the main validation set we also have more specific。

validation sets that correspond to particular error cases that we know have given our model trouble or a previous version of our model trouble in the past these could be like just particular edge cases that you've found in the past like maybe your model handles examples of poor grammar very poorly or it doesn't know what some Gen Z slang terms mean these are examples of failure modes you've found for your model in the past that get rolled into data sets to test the next version of your model in continual learning just like how training sets are dynamic and change over time evaluation sets are dynamic as well as you curate new data you should add some of it to your training sets but also add some of it to your evaluation sets for example if you change how you do sampling you might want to add some of that newly sampled data to your eval set as well to make sure that your eval set represents that new sampling strategy or if you discover new edge case instead of only adding that edge case to the training set it's worth holding out some examples of that edge case as a particular unit test to be part of that offline evaluation suite two corollaries to note of the fact that evaluation sets are dynamic first。

That you should also version control your evaluation sets just like you do your training sets The second is that if your data is evolving really quickly。

 then part of the data that you hold out should always be the most recent data。

 the data from the past day or past hour or whatever it is to make sure that your model is generalizing well to new data once you have the basics in place a more advanced thing that you can look into here that I think it's pretty promising is the idea of expectation tests the way the expectation tests work are you take pairs of examples where you know the relationship so let's say that you're doing sentiment analysis and you have a sentence that says my brother is good if you make the positive word in that sentence more positive and instead say my brother is great then you would expect your sentiment classifier to become even more positive about that sentence these types of tests have been explored in NLP as well as recommendation systems and they're really good for testing whether your model generalizes in predictable ways and so they give you more granular information than just aggregate performance metrics about how your model does on previous。

unseen data One observation to make here is that just like how data curation is highly analogous to monitoring so is offline testing just like in monitoring。

 we want to observe our metrics not just in aggregate。

 but also across all of our important subsets of data and across all of our edge cases one difference between these two is that you will in general have different metrics available in offline testing and online testing For example。

 you are much more likely to have labels available offline in fact you always have labels available offline because that is how you're going train your model but online you're much more likely to have feedback and so even though these two ideas are highly analogous and should share a lot of metrics and definitions of subsets and things like that one point of friction that will occur between online monitoring and offline testing is that the metrics are a little bit different So one direction for research that I think would be really exciting to see more of is using offline metrics like accuracy to predict online metrics like user engagement and then lastly once we've tested our candidate model offline。



![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_12.png)

It's time to deploy it and evaluate it online So we talked about this last time so I don't want to reiterate too much but as a reminder。

 if you have the infrastructural capability to do so then you should do things like first running your model in shadow mode before you actually roll it out to real users then running an AV test to make sure that users are responding to it better than they did the old model then once you have a successful AV test rolling it out to all of your users but doing so gradually and then finally if you see issues during that rollout just to roll it back to the old version of the model and try to figure out what went wrong So we talked about the different stages of continual learning from logging data to curating it to triggering retraining testing the model and rolling out to production and we also talked about monitoring and observability which is about giving you a set of rules that you can use to tell whether your retraining strategy needs to change and we observed that in a bunch of different places the fundamental elements that you study and monitoring like projections and user feedback and。



![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_14.png)

![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_15.png)

Uncertainty are also useful for different parts of the continual learning process and that's no coincidence I see monitoring and continual learning as two sides of the same coin we should be using the signals that we monitor to very directly change or retraining strategy So last thing I want to do is just try to make this a little bit more concrete by walking through an example of a workflow that you might have from detecting an issue in your model to altering the strategy this section describes more of a future state until you've invested pretty heavily in infrastructure it's going to be hard to make it feel as seamless as this in practice but I wanted to mention it anyway because I think it provides like a nice end state for what we should aspire to in our continual learning workflows I think you would need to have in place before you're able to actually execute what I'm going to describe next is a place to store and version all of the elements of your strategy which include metric definitions for both online and offline testing performance thresholds for those metrics definitions of any of the projections that you want to use for monitoring and also for data curation subgroup。



![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_17.png)

or cohorts that you think are particularly important to break out your metrics along the logic that defines how you do data curation。

 whether it's sampling rules or anything else。 and then finally the specific data sets that you use for each different run of your training or evaluation Our example cont improvement at loop starts with an alert and in this case that alert might be our user feedback got worse today so our job is now to figure out what's going on So next thing we'll use is some of our observability tools to investigate what's going on here and we might run some subgroup analysis and look at some raw data and figure out that the problem is really mostly isolated to new users and next thing that we might do is do error analysis so look at those new users and the data points that they're sending us and try to reason about why those data points are performing worse and what we might discover something like our model was trained assuming that people are gonna to write emails but now users are submitting a bunch of text that has things that aren't normally found in emails like emojis and that's causing our model problems So here's where we might make the。

Change to our retraining strategy We could define new users as a cohort of interest because we never want performance to decline on new users again without getting an alert about that。

 then we could define a new projection that helps us detect data that has emojis and add that projection to our observability metrics so that anytime in the future if we as part of an investigation to see how our performance differs between users that are submitting emojis and ones that are not we can always do that without needing to rewrite the projection next we might search our reservoir for historical examples that contain emojis so that we can use them to make our model better and then adjust our strategy by adding that subset of data as a new test case So now whenever we test the model going forward。

 we'll always see how performs on data with emojis in addition to adding emoji examples as a test case。

 we would also curate them and add them back into our training set and do a retraining then once we have the new model that's trained we'll get this new model comparison report which will include also the new cohort that we defined as part of this process and the new emoji edge case data set that we defined and then finally if we're doing。

ual deployment， we can just deploy that model and that completes the continual improvement loop So to wrap up what do I want you to take away from this continual learning is a complicated rapidly evolving and poorly understood topic so this is an area to pay attention to if you're interested in seeing how the cutting edge of production of machine learning is evolving and the main takeaway from this lecture is we broke down the concept of a retraining strategy which consists of a number of different pieces definitions of metrics subgroups of interest projections that help you break down and analyze high- dimensionmenional data performance thresholds for your metrics logic for curating new data sets and the specific data sets that you're going use for retraining and evaluation at a high level the way that we can think about our role as machine learning engineers once we've deployed the first version of the model is to use rules that we define as part of our observability and monitoring suite to iterate on the strategy for many of you in the near term this won't feel that different from just using that data to retrain the model however if you'd like to but I think thinking of this as a strategy that you can tune at a higher level is a productive way of understanding it as。

You move towards more and more automated retraining Lastly。

 just like every other aspect of the ML lifecycle that we talk about in this course。

 our main recommendation here is to start simple and add complexity later in the context of continual learning what that means is it's okay to retrain your models manually to start as you get more advanced you might want to automate retraining and you also might want to think more intelligently about how you sample data to make sure that you're getting the data that is most useful for improving your model going forward that's all for this week see you next time。



![](img/310dfd78a1c7ed8a9f97beaf830e1e8b_19.png)