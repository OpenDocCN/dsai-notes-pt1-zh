# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p18 P18_Lecture_09-伦理 -BV1k4YXznEjw_p18-

![](img/792f5e37e9d49350c16e6d14fb60e638_0.png)

Hey everyone， welcomelcome to the 9 and final lecture of Fulltack deep Learning 2022 Today。

 we'll be talking about ethics After going through a little bit of context of what it is that we mean by ethics。

 what I mean by ethics when I talk about it， We'll go through three different areas where ethics comes up Both broad tech ethics。

 ethics that anybody who works in the tech industry broadly needs to think about and care about what ethics has meant specifically for the machine learning industry。

 what's happened in the last couple of years as ethical concerns have come to the forefront。

 and then finally， what ethics might mean in a future where true artificial general intelligence exists。

 So first， let's do a little bit of context setting。 even more so than other topics。

 all lectures on ethics are wrong。 But some of them are useful and they're more useful if we admit and state what our assumptions or biases or approaches are before we dive into the material。

 And then I'll also。Talk about three kind of general themes that I see coming up again and again when ethical concerns are raised in tech and in machine learning。

 themes of alignment themes of tradeoff and the critical theme of humility So in this lecture I'm going to approach ethics on the basis of concrete cases。

 specific instances where people have raised concerns So we'll talk about cases where people have taken actions that have led to claims and counterclaims of ethical or unethical behavior。

 the use of automated weapons the use of machine learning systems for making decisions like sentencing and bail and the use of machine learning algorithms to generate art In each case。

 when criticism has been raised part of the criticism has been that the technology or its impact is unethical So approaching ethics in this way allows me to give my favorite answer to the question of what is ethics which is to quote one of my favorite philosophers Luwig Witgenstein and say that the meaning of a word is its use in the。



![](img/792f5e37e9d49350c16e6d14fb60e638_2.png)

Language so we'll be focusing on times when people have used the word ethics to describe what they like or dislike about some piece of technology and this approach to definition is an interesting one。

 if you want to try it out for yourself， you should check out the game something。

 something soup something， which is a browser game at the link in the bottom left to this slide in which you presented with a bunch of dishes and you have to decide whether they are soup or not soup。

 whether they can be served to somebody who ordered soup and by playing a game like this。

 you can discover both how difficult it is to really put your finger on a concrete definition of soup and how poorly maybe your working definition of soup fits with any given soup theory because of this sort of case-based approach。

 we won't be talking about ethical schools and we won't be doing any trolley problems So this article here from current affairs asks you to consider this particular example of an ethical dilemma where an asteroid containing all of the universe's top doctors who are working on a cure for all possible illnesses。

Hurling towards the planet of orphans。 and you can destroy the asteroid and save the orphans。

 But if you do so， the hope for a cure for all diseases will be lost forever。

 And the question posed by the authors of this article is。

 is this hypothetical useful at all for illuminating any moral truths。

 So rather than considering these hypothetical scenarios about trolley cars going down rails and fat men standing on bridges will' talk about concrete specific examples from the last 10 years of work in our field and adjacent fields。

 But this isn't the only way of talking about or thinking about ethics。

 It's the way that I think about it is the way that I prefer to talk about it is not the only one and it might not be the one that works for you。

 So if you want another point of view， And one that really emphasizes and loves trolley problems。

 then you should check out Sergei's lecture from the last edition of the course from 2021。

 It's a really delightful talk and presents some similar ideas from a very different perspective coming to some of the same conclusions and some different conclusions。

 a useful theme。From that lecture that I think we should all have in mind when we're pondering ethical dilemmas and the related questions that they bring up is the theme of what is water from last year's lecture。

 And so this is a famous little story from a commencement speech by David Foster Wallace。

 where an older fish swing by two younger fish says Morning boys How's the water。

 And after he swims away。 one of the younger fish turns the other and says， wait。

 what the hell is water。 The idea is that if we aren't thoughtful， if we aren't paying attention。

 some things that are very important， can become background can become assumption and can become invisible。

 And so when I shared these slides with Sergey， he challenged me to answer this question for myself about how we were approaching ethics this time around。

 And I'll say that this approach of relying on prominent cases。

 risks replicating a lot of social biases。 Some people's ethical claims are amplified And some fall on unhearing ears。

 Some stories travel more because the people involved have more resources and are better connected。

And using these forms of casebased reasoning， where you explain your response or your beliefs in terms of these concrete specifics can end up hiding the principles that are actually in operation。

 Maybe you don't even realize that that's how you're making the decision。

 Maybe some of the true ethical principles that you're operating under can disappear like water to these fish。

 So don't claim that the approach I'm taking here is perfect， But in the end。

 so much of ethics is deeply personal that we can't expect to have a perfect approach。

 we can just do the best we can and hopefully better every day。

 So we're gonna see three themes repeatedly come up throughout this talk。

 Two different forms of conflict that give rise to ethical disputes。 One。

 when there is conflict between what we want and what we get and another when there is conflict between what we want and what others want。

 And then finally， a theme of maybe an appropriate response。

 a response of humility when we don't know what we want or how to get it。

 The problem of alignment where what we want and what we。D will come up over and over again。

 and one of the primary drivers of this is what you might call the proxy problem。

 which is in the end we are often optimizing or maximizing some proxy of the thing that we really care about and if the alignment or loosely the correlation between that proxy and the thing that we actually care about is poor enough then by trying to maximize that proxy we can end up hurting the thing that we originally cared about。

 there is a nice paper that came out just very recently doing a mathematical analysis of this idea that's actually been around for quite some time。

 excuse you can see these kinds of proxy problems everywhere once you're looking for them and the top right have a train and validation loss chart from one of the training runs for the full stack learning text recognizer the thing that we can actually optimize is the training loss that's what we can use to calculate gradients and improve the parameters of our network but the thing that we really care about is the performance of the network on data points it hasn't seen like the validation set or the test set or data in production if we optimize our training loss。

Too much。 then we can actually cause our validation loss to go up。 Similarlyly。

 there was an interesting paper that suggested that increasing your accuracy on classification tasks can actually result in a decrease in the utility of your embeddings in downstream tasks。

 and you could find these proxy problems outside of machine learning as well。

 there's a famous story involving a Soviet factory and nails that turned out to be false。

 but in looking up a reference for it， I was able to find an actual example where a factory that was making chemical machines rather than creating a machine that was cheaper and better chose not to adopt producing that machine because their output was measured in weight So the thing that the planners actually cared about economic efficiency and output was not what was being optimized for because it was too difficult to measure and one reason why these kinds of proxy problems arise so frequently is due to issues of information The information that were able to measure is。

Not the information that we want so the training loss is the information that we have。

 but the information that we want is the validation loss。

 but then at a higher level we often don't even know what it is that we truly need so we may want the validation loss。

 but what we need is the loss in production or really the value our users will derive from this model。

 but even when we do know what it is that we want or what it is that we need we're likely to run into the second kind of problem。

 the problem of a tradeoff between stakeholders going back to our hypothetical example with the asteroid of doctors hurturling towards the planet of orphans。

 what makes this challenging is the need to determine a tradeoff between the wants and needs of the people on the asteroid。

 the wants and needs of the orphans on the planet and the wants and needs of future people who cannot be reached for comment and to weigh in on this concern is sometimes said。

That this need to negotiate tradeoffs is one of the reasons why engineers don't like thinking about some of these problems around ethics。

 And don't think that's quite right because we do accept tradeoffs as a key component of engineering。

 there's this nice o'reilly book on the fundamentals of software architecture the first thing that they state at the very beginning is that everything in software architecture is a tradeoff and even this satirical O'rely book says that every programming question has the answer it depends。

 So we're comfortable negotiating tradeoffs take， for example。

 this famous chart comparing the different convolutional networks on the basis of their accuracy and the number of operations that it takes to run them thinking about these kinds of tradeoffs between speed and correctness is exactly the sort of thing that we have to do all the time and our job as engineers and one part of it that is maybe easier is at least selecting what's called the Pareto front for the metrics that we care about My favorite way of remembering what a Paratofront is is this definition of a data。

😊，Scientist from Josh Wills， which is a data scientist is better at stats than any software engineer and better at software engineering than any statistician。

 So this Pareto front that I've drawn here is the models that have are more accurate than anybody who takes fewer flops and use fewer flops than anybody who is more accurate。

 So I think rather than it fundamentally being about tradeoffs。

 one of the reasons why engineers maybe dislike thinking about these problems is that it's really hard to identify the axes for a chart like the one that I just showed。

 it's very hard to quantify these things。 And if we do quantify things like the utility or the rights of people involved in a problem。

 We know that those quants are far away from what they truly want to measure。

 there's a proxy problem。 In fact， But even further once measured where on that front do we fall as engineers we maybe develop an expertise in knowing whether we want high accuracy or low latency or computational load。

 but we are not as comfortable deciding how many current orphans， we want a trade。

What amount of future health So this raises questions both in terms of measurement and in terms of decision making that are outside of our expertise So the appropriate response here is humility because we don't explicitly train these skills the way that we do many of the other skills that are critical for our job and many folks engineers and managers in technology seem to kind of keep in their bones prefer optimizing single metrics making a number go up so there's no tradeoffs to think about and those metrics are they're not proxies they're the exact thing that you care about my goal within this company。

 my objective for this quarter， my North star is user growth or lines of code and by God I make that go up So when we encounter a different kind of problem it's important to bring a humble mindset a student mindset to the problems to ask for help to look for experts and to recognize that the help that you get any experts that you find might not be immediately obviously what you want or。

Use to Additionally， one form of this that we'll see repeatedly is that when attempting to intervene because of an ethical concern。

 it's important to remember this same humility。 It's easy to think when you are on the good side that this humility is not necessary。

 but even trying to be helpful is a delicate and dangerous undertaking one of my favorite quotes from the system' Bible。

 So we want to make sure as we resolve the ethical concerns that people raise about our technology that we come up with solutions that are not just part of the problem。

 So the way that I resolve all of these is through user orientation by getting feedback from users。

 we maintain alignment between ourselves and the system that we're creating and the users that it's meant to serve。

 And then when it's time to make tradeoffs， we should resolve them in consultation with users。

 And in my opinion， we should tilt the scales in their favor and away from the favor of other stakeholders including within our own organization。

 And then humility is one of the reasons why we actually listen to users at all because we。



![](img/792f5e37e9d49350c16e6d14fb60e638_4.png)

A humble enough to recognize that we don't have the answers to all these questions。

 All right with our context and our themes under our belt。

 let's dive into some concrete cases and responses。

 We'll start by considering ethics in the broader world of technology that machine learning finds itself in。

 So the key thing that I want folks to take away from this section is that the tech industry cannot afford to ignore ethics as public trust in tech declines。

 we need to learn from other nearby industries that have done a better job on professional ethics。

 and then we'll talk about some contemporary topics。

 some that I find particularly interesting and important throughout the past decade。

 the technology industry has been plagued by scandal。

 whether that's how technology companies interface with national governments at the largest scale over to how technology systems are being used or manipulated by people creating disinformation or fake social media accounts or targeting children with automatically generated content that hacks the YouTube recommendation system。

 and the impact of this。

![](img/792f5e37e9d49350c16e6d14fb60e638_6.png)

Been that distrust in tech companies has risen markedly in the last 10 years。

 so this is from the public affairs Pse survey just last year。

 the tech industry when from being in 2013， one of the industries that the fewest people felt with less trustworth than average to rubbing elbows with famously much distrusted industries like energy and pharmaceuticals and the tech industry doesn't have to win elections。

 so we don't have to care about public polling as much as politicians but politicians care quite a bit about those public opinion polls and just in the last few years。

 the fraction of people who believe that the large tech companies should be more regulated has gone up a substantial amount and comparing it to 10 years ago its astronomically higher。

 so there will be substantial impacts on our industry due to this loss of public trust。

 So as machine learning engineers and researchers we can learn from nearby fields。

So I'll talk about two of them， one a nice little bit about the culture of professional ethics in engineering in Canada and then a little bit about ethical standards for human subjects research So one of the worst construction disasters in modern history was the collapse of the Quebec bridge in 1907 75 people who were working on the bridge at the time were killed and a parliamentary inquiry place the blame pretty much entirely on two engineers in response there was the development of some additional rituals that many Canadian engineers take part in when they finish their education that are meant to impress upon them the weight of their responsibility so one component of this is a large iron ring which literally impresses that weight upon people and then another is an oath that people take a non-legally binding oath that includes saying that I will not henceforward suffer or pass or be privy to the passing of bad workmanship or faulty material I think the software would look quite a bit different if software engineers。

Took an oath like this and took it seriously。 One other piece I wanted to point out is that it includes within it some built in humility。

 asking pardon ahead of time for the assured failures。

 lots of machine learning is still in the research stage。 And so some people may say that oh， well。

 that's important for the people who are building stuff。

 but I'm working on R and D for fundamental technology。 So I don't have to worry about that。

 But research is also subject to regulations。 So this is something I was required to learn because I did my PhD in a neuroscience department that was funded by the national insts of health。

 which mandates training in ethics and in the ethical conduct of research。

 So these regulations for human subjects research date back to the 1940s when there were medical experiments on unwilling human subjects by totalitarian regimes。

 This is still pretty much the cornerstone for laws on human subjects research around the world through the Helsinki declaration。

 which gets regularly updated in the US the touchstone bit of regulation on this， the 1973。

Rearch Act requires， among other things， informed consent from people who are participating in research。

 and there were two major revelations in the late 60s and early 70s that led to this legislation not dissimilar to the scandals that have plagued the technology industry recently。

 one was the infliction of hepatitis on mentally disabled children in New York in order to test hepatitis treatments and the other was the non-treatment of syphilis in black men at Tuskegee in order to study the progression of the disease in both cases。

 these subjects did not provide informed consent and seemed to be selected for being unable to advocate for themselves or to get legal redress for the harms they were suffering。

 And so if we are running experiments and those experiments involve humans involve our users。

 we are expected to adhere to the same principles and one of the famous instances of mismatch between the culture in our industry and the culture of human subjects research was when。

Researchers at Facebook studied emotional contagion by altering people's newsfeeds。

 either adding more negative content or adding more positive content。

 and they found a modest but robust effect that introducing more positive content caused people to post more positively when people found out about this there were very upset the authors noted that Facebook's data use policy includes that the users data and interactions can be used for this。

 but most people who were Facebook users and the editorial board of PS where this was published did not see it that way So put together。

 I think we are at the point where we need a professional code of ethics for software hopefully many codes of ethics developed in different communities that can bubble up。

 compete with each other and merge to finally something that most of us or all of us can agree on and that is incorporated into our education and acculturation of new members into our field and into more aspects of how we build。

To close out this section， I wanted to talk about some particular ethical concerns that arise in tech in general。

 first around carbon emissions and then second around dark patterns and user hostile designs。

 The good news with carbon emissions is that because they scale with cost。

 It's only something that you need to worry about when the costs of what you're building what you're working on are very large at which time you both won't be alone in making these decisions and you can move a bit more deliberately and make these choices more thoughtfully。

 So first， what are the ethical concerns with carbon emissions Anthropogenic climate change driven by CO2 emissions raises a classic tradeoff。

 which was dramatized in this episode of Harvey Birdman attorneytor at law in which George Jetson travels back from the future to sue the present for melting the ice caps and destroying his civilization。

 So unfortunately， we don't have future generations present now to advocate for themselves。

 The other view is that。😊，This is an issue that arises from a classic alignment problem。

 which is many organizations are trying to maximize their profit that raw profit is based off of prices for goods that don't include externalities like the environmental damage caused by carbon dioxide emissions leading to increased temperatures and clactic change。

 So the primary dimension along which we have to worry about carbon emissions is in compute jobs that require power。

 that power has to be generated somehow， and that can result in the emission of carbon。

 and so there was a nice paper linked in this slide that walked through how much carbon dioxide was emitted using typical USbased cloud infrastructure and the top headline from this paper was that training a large transformer model with neural architecture search produces as much carbon dioxide as five cars create during their lifetime。

 So that sounds like quite a bit of carbon dioxide and it is in fact。

But it's important to remember that power is not free。

 And so there is a metric that we're quite used to tracking that is at least correlated with our carbon emissions。

 our compute spend。 And if you look the cost runs between one and 3 million to run the neural architecture search that emitted five cars worth of CO2 and 1 to $3 million is actually a bit more than it would cost to buy5 cars and provide their fuel。

 So the number that I like to use is that four USbased cloud infrastructure like the US West1 that many of us find ourselves in $10 of cloud spend is roughly equal to 1 worth of air travel costs。

 So that's on the basis of something like the numbers in the chart indicating air travel across the United States from New York to San Francisco。

 I've been taking care to always say USbased cloud infrastructure because just changing cloud regions can actually reduce your emissions quite a bit。

 there's actually a factor of nearly 50 x from some of the cloud regions that have the most carbon intent。

powerer generation like A Southeast 2 and the regions that have the least carbon intensive power like C central1 that chart comes from a nice talk from huggingface that you can find on YouTube part of their course that talks a little bit more about that paper and about managing carbon emissions interest in this problem has led to some nice new tooling one code carbonbon Io allows you to track power consumption and therefore CO2 emissions。

 just like you would any of your other metrics。 and then there's also this Mlco2 impact tool that's oriented a little bit more directly towards machine learning。

 The other ethical concern in tech that I wanted to bring up is deceptive design and how to recognize it。

 and an unfortunate amount of deception is tolerated in some areas of software。

 The example on the left comes from an article by Neion and it all that shows a fake countdown timer that claims that an offer will only be available for an hour。

 but when it hits zero， nothing happens。 The offer is still there。

There's also a possibly apocryphal example on the right here。

 You may have seen these numbers next to products when online shopping。

 saying that some number of people are currently looking at this product。

 this little snippet of jascript here produces a random number to put in that spot。

 So that example on the right may not be real but because of real examples like the one on the left it strikes a chord with a lot of developers and engineers。

 There's a kind of slippery slope here that goes from being unclear or maybe not maximally upfront about something that is a source of friction or a negative user experience in your product and then in trying to remove that friction or sand that edged down。

 you slowly find yourself being effectively deceptive to your users on the left is a nearly complete history of the way Google displays ads in its search engine results It started off very clearly colored and separated out with a bright color from the rest of the results and then about 10 years ago。

That colored background was removed and replaced with just a tiny little colored snippet that said add。

 And now， as of 2020， That small bit there is no longer even colored。 It's just bolded。

 And so this makes it difficult for users to know which content is being served to them because somebody paid for them to see it versus being served up organically。

 So a number of patterns of deceptive design also known as dark patterns have emerged over the last 10 years。

 you can read about them on this website deceptive dot design。

 There's also a Twitter account at dark patterns where you can share examples that you find in the wild。

 So some examples that you might be familiar with are the roach motel named after a kind of insect trap where you can get into a situation very easily。

 but then very hard to get out of it。 If you've ever attempted to cancel a gym membership or delete your Amazon account。

 then you may have found yourself a roach in a motel。

 Another example is trick questions where forms intentionally make it difficult to choose the option that most users want。

 for example。Using negation in an onstandard way， like check this box to not receive emails from our service。

 One practice in our industry that's on very shaky， ethical and legal ground is growth hacking。

 which is a set of techniques for achieving really rapid growth in user base or revenue for a product and has all the countnotations you might expect from the name hack LinkedIn was famously very spammy when it first got started。

 I'd like to add you to my professional network on LinkedIn became something of a meme。

 and this was in part because LinkedIn made it very easy to unintentionally send LinkedIn invitations to every person you'd ever emailed。

 they ended ended up actually having to pay out in a class action lawsuit because they were sending multiple followup emails when user only clicked to send an invitation once and the structure of their emails made it seem like they were being sent by the user rather than LinkedIn and the use of these growth hacks goes back to the very inception of email Homail market itself in part by tacking on a signature to the。

Of every email that said P I love you get your free email at Homail。

 So this seemed like it was being sent by the actual user。

 I grabbed a snippet from a top 10 growth hacks article that said that the personal sounding nature of the message and the fact that it came from a friend made this a very effective growth hack。

 but it's fundamentally deceptive to add this to messages in such a way that it seems personal and to not tell users that this change is being made to the emails that they're sending So machine learning can actually make this problem worse if we are optimizing shortterm metrics。

 these growth hacks and deceptive designs can often drive user and revenue growth in the short term。

 but they do that by worsening user experience and drawing down on goodwill towards the brand in a way that can erode the longterm value of customers when we incorporate machine learning into the design of our products with a B testing we have to watch out to make sure that the metrics that were。

Don't encourage this kind of deception。 So consider these two examples on the right。

 the top example is a very straightforwardly implemented and direct and easy to understand form for users to indicate whether they want to receive emails from the company and from its affiliates in example B the wording of the first message has been changed so that it indicates that the first box should be checked to not receive emails while the second one should not be tick in order to not receive emails And if your AB testing。

 these two designs against each other and your metric is the number of people who sign up to receive emails then it's highly likely that the system is going select example B So taking care in setting up AB tests such that either they're tracking longer term metrics or things that correlate with them and that the variant generation system that generates all the different possible designs can't generate any designs that we would be unhappy with as we would hopefully be unhappy with the deceptive design and。

And I think it's also important to call out that this problem arises inside of another alignment problem。

 We were considering the case where the longter value of customers and the company's interests were being harmed by these deceptive designs。

 but unfortunately that's not always going to be the case。

 the private enterprises that build most technology these days are able to deliver broad social value to make the world a better place。

 as they say。 but the way that they do that is generally by optimizing metrics that are at best a very weak proxy for that value that they're delivering like their market capitalization。

 And so there's the possibility of an alignment problem where companies pursuing and maximizing their own profit and success can lead to net negative production of value and this misalignment is something that if you spend time at the intersection of capital and funding leadership and technology development。

 you will encounter it。 So it's important to consider these questions ahead of time and come to your own position。

 whether that's treating this as the price of。Business or the way the world works。

 seeking ways to improve this alignment or considering different ways to build technology But on the shorter term。

 you can push for longer term thinking within your organization to allow for better alignment between the metrics that you're measuring and the goals that you're setting and between the goals that you're setting and what is overall good for our industry and for the broader world and you can also learn to recognize these user hostile design patterns。

 call them out when you see them and you can advocate for a more usercented design instead so to wrap up our section on ethics for building technology broadly。

 we as an industry should learn from other disciplines if we want to avoid a trust crisis or if we want to avoid the crisis getting any worse and we can start by educating ourselves about the common user hostile practices in our industry and how to avoid them now that we've covered the kinds of ethical concerns and conflicts that come up when building technology in general。

 let's talk about concerns that are specific。

![](img/792f5e37e9d49350c16e6d14fb60e638_8.png)

![](img/792f5e37e9d49350c16e6d14fb60e638_9.png)

To machineine learning just in the past couple of years。

 there have been more and more ethical concerns raised about the uses of machine learning。

 and this has gone beyond just the ethical questions that can get raised about other kinds of technology。

 So we'll talk about some of the common ethical questions that have been raised repeatedly over the last couple of years。

 and then we'll close out by talking about what we can learn from a particular subdispline of machine learning medical machine learning。

 So the fundamental reason I think that ethics is different for machine learning and maybe more salient is that machine learning touches human lives more intimately than a lot of other kinds of technology。

 So many machine learning methods， especially deep learning methods make human legible data into computer legible data。

 So we're working on things like computer vision on processing natural language and humans are more sensitive to errors in and have more opinions about this kind of data about images like this puppy than they do about the other kinds of data manipulated by computers like abstract syntax trees。

 So because of this there are more。olds with more concerns that need to be traded off in machine learning applications。

 And then more broadly， machine learning involves being wrong pretty much all the time。

 There's the famous statement that all models are wrong， though some are useful。

 And I think the first part applies at least particularly strongly to machine learning。

 Our models are statistical and include in them randomness， the way that we frame our problems。

 the way that we frame our optimization in terms of crossenttropiece or divergences。

 and randomness is almost always an admission of i。 Even the quintessential examples of randomness。

 like random number generation and computers and the flipping of a coin， are things that we know。

 in fact， are not random， truly， they are， in fact， predictable。

 And if we knew the right things and had the right laws of physics and the right computational power。

 than we could predict how a coin would land。 We could control it。

 We could predict what the next number to come out of a random number generator would be。

 whether it's pseudoran or based on some kind of hardware randomness。

 And so we're admitting a certain degree。😊，Of ignorance in our models。

 And that means our models are going to be wrong， and they are going to misunderstand situations that they are put into。

 And it can be very upsetting and even harmful to be misunderstood by a machine learning model。

 So against this backdrop of greater interest or higher stakes。

 A number of common types of ethical concern have coalesced in the last couple of years。

 And there are somewhat established camps of answers to these questions and you should at least know where it is you stand on these core questions。

 So for really important questions that you should be able to answer about about anything that you build with machine learning are is the model fair and what does that mean in this situation。

 I the system that you're building accountable， who owns the data involved in this system。

 And finally， and perhaps most importantly and undergirding all of these questions is should this system be built at all。

 So first is the model we're building fair。 The classic case on this comes from criminal justice from the compass system for predicting before trial。

 whether a defendant will be arrested again。So if they're arrested again。

 that's suggests they committed a crime during that time。

 And so this is assessing a certain degree of risk for additional harm while the justice system is deciding what to do about a previous arrest and potential crime。

 So the operationalization here was a 10 point rearrest probability based off of past data about this person。

 And they set a goal from the very beginning to be less biased than human judges。

 So they operationalized that by calibrating these arrest probabilities and making sure that if。

 say a person received a two on this scale， they had a 20% chance of being arrested again。

 And then critically that those probabilities were calibrated across subgroups。

 So racial bias is one of the primary concerns around bias in criminal justice in the United States。

 And so they took care to make sure that these probabilities of rearst were calibrated for all racial groups。

 the system was deployed it is actually used all around the United States。 its proprietary。 So its。

Difficult to analyze， but using the Freedom of Information Act and by collating together a bunch of records。

 some people at Propublica were able to run their own analysis of this algorithm。

 and they determined that though this calibration that compass claimed for arrest probabilities was theirs So the model was not more or less wrong for one racial group or another。

 the way that the model tended to fail was different across racial groups。

 So the model had more false positives for black defendants。

 So saying that somebody was higher risk but then them not going on to reoffend and had more false negatives for white defendants。

 So labeling them as low risk and then them going on to reoffend。 So despite north point。

 the creators of compass taking into account bias from the beginning。

 they ended up with an algorithm with this undesirable property of being more likely to effectively falsely accuseduse defendants who were black than defendants who were white。

This report touched off a ton of controversy and back and forth between Proplica。

 the creators of the article and Northpoint creators of compass and also a bunch of research And it turned out that some quick algebra revealed that some form of racebased bias is inevitable in this setting So the things that we care about when we're building a binary classifier are relatively simple you can write down all of these metrics directly So we care about things like the false positive rate。

 which means we've imprisoned somebody with no need the false negative rate。

 which means we missed an opportunity to prevent a situation that led to an arrest and then we also care about the positive predictive value。

 which is this rearrest probability that compass was calibrated on So because all of these metrics are related to each other and related to the joint probability distribution of our modelss labels and the actual ground truth if the probability of rearrest differs across groups then we have to have that some of these numbers are different across groups and that is a form。

Of racial bias。 So the basic way that this argument works just involves rearranging these numbers and saying that if the numbers on the left side of this equation are different for group 1 and group 2。

 then it can't possibly be the case that all three of the numbers on the right hand side are the same for group1 and group 2。

 And I'm presenting this here as though it only impacts these specific binary classification metrics。

 but are， in fact， a very large number of definitions of fairness， which are mutually incompatible。

 So there's a nice a really incredible tutorial by Arvin Nionan who is also the first author on the dark patterns work on a bunch of these fairness definitions what they mean and why they're in commensurate。

 So I can highly recommend that lecture。 So returning to our concrete case。

 if the prevalences differ across groups， then one of our things that we're concerned with。

 the false positive rate， the false negative rate or the positive predictive value will not be equal。

 and that's something that people can point to and say that's unfair in the middle that positive predict。

Value was equalized across groups in compass。 That was what they really wanted to make sure was equal across groups。

 And because the probability of rear arrestrest was larger for black defendants。

Then either the false positive rate had to be bigger or the false negative rate had to be bigger for that group。

 And there's an analysis in this Cho dechova 2017 paper that suggests that the usual way that this will work is that there will be a higher false positive rate for the group with a larger prevalence。

 So the fact that there will be some form of unfairness that we can't just say， oh， well。

 all these metrics are the same across all groups。 and so everything has to be fair。

 That fact is fixed， but the impact of the unfairness of models is not fixed。

 the story is often presented as， oh， well， no matter what the journalists would have found something to complain about。

 there's always critics。 And so you know you don't need to worry about fairness that much。

 But I think it's important to note that the particular kind of unfairness that came about from this model from focusing on this positive predictive value LED to a higher false positive rate。

 more unnecessary imprisonment for black defendants。

 the false positive rate and the positive predictive value were equalized across groups that would have LED to。

Higher false negative rate for black defendants relative to white defendants and in the context of American politics and concerns about racial inequity in the criminal justice system。

 bias against white defendants is not going to lead to complaints from the same people and has a different relationship to the historical operation of the American justice system。

 and so far from this being a story about the hopelessness of thinking about or car about fairness。

 This is a story about the necessity of confronting the tradeoffs that are inevitably going to come up。

 So some researchers that Google made a nice little tool where you can try thinking through and making these tradeoffs for yourself。

 It's a loanne decision rather than a criminal justice decision。

 but it has a lot of the same properties。 you have a binary classifier。

 you have different possible goals that you might set either maximizing the profit of the loaning entity or providing equal opportunity to the two groups and it's very helpful for building intuition on these fairness metrics and what it means to pay。

1 over the other。 And these events in this controversy kicked off a real flurry of research on fairness。

 And there's now been several years of this fairness accountability and transparency conference fact。

 There's tons of work on both algorithmic level approaches to try and measure these fairness metrics incorporate them into training and also more qualitative work on designing systems that are more transparent and accountable。

 So the compass example is really important for dramatizing these issues of fairness。

 but I think it's very critical for this case and for many others to step back and ask whether this model should be built at all。

 So this algorithm for scoring risk is proprietary and uninterpretable。

 It doesn't give answers for why a person is higher risk or not and because it is close source。

 there's no way to examine it。 and achieves an accuracy of about 65%， which is quite high。

 given that the marginal probability of reoffense is much lower than 50%。

 but it's important to compare the baselines here。togethergether a bunch of nonexperts like you would on a jury has an accuracy of about 65% and creating a simple scoring system on the basis of how old the person is and how many prior arrests they have also has an accuracy of around 65%。

 And it's much easier to feel comfortable with the system that says if you've been arrested twice than you have a higher risk of being arrested again。

 And so you'll be imprisoned before trial than a system that just says， oh， well。

 we ran the numbers and it looks like you have a high chance of committing a crime。

 But even framing this problem in terms of who is likely to be rested is already potentially a mistake。

 So a slightly different example of predicting failure to appear in court was tweeted out by Moore heart。

 who's one of the main researchers in this area， choosing to try to predict who will fail to appear in court。

 treating this is something that is then a fact of the universe that this person is likely to fail to appear in court and then intervening on this and punishing them for that for that fact。

 it's important to recognize why people。Fil to appear in court in general。

 Often it's because they don't have childcare to cover for the care of their dependents while they're in court。

 they don't have transportation。 Their work schedule is inflexible or the court appointment schedule is inflexible or unreasonable。

 Itd be better to implement steps to mitigate these issues and reduce the number of people who are likely to fail to appear in court。

 For example， by making it possible to join court remotely。

 that's a far better approach for all involved than simply getting really。

 really good at predicting who currently fails to appear in court。

 So it's important to remember that the things that we're measuring the things that we're predicting are not the be all and all in themselves。

 the things that we care about are things like an effective and fair justice system。

 And this comes up perhaps most acutely in the case of compass。

 when we recognize that rerest is not the same as recidivism is' not the same thing as committing more crimes。

 being rearrested requires that a police officer believes that you committed a crime。

 Police officers are subject。To their own biases and patterns of policing result in a far higher fraction of crimes being caught for some groups than for others。

 And so a real goal in terms of fairness in criminal justice might be around reducing those kinds of unfair impacts and using past rest data that we know has these issues to determine who is treated more harshly by the criminal justice system is likely to exacerbate these issues。

 There's also a notion of model fairness that is broader than just models that make decisions about human beings。

 So even if you're decidinging a model that works on text or works on images。

 you should consider which kinds of people your model works well for。 And in general。

 representation both on engineering and management teams and in data sets really matters for this kind of model fairness。

 So it's unfortunately still very easy to make machine learning powered technology that fails for minoritized groups。

 So， for example， off the shelf computer vision tools will often fail on dark。

So this is an example by Joy Bull andweni from MIT on how a computer visionbased project that she was working on ran into difficulties because the face detection algorithm could not detect her face。

 even though it could detect the faces of some of her friends with lighter skin and in fact。

 she found that just putting on a white mask was enough to get the computer vision model to detect her face。

 So this is unfortunately not a new issue in technology is just a more salient one with machine learning。

 So one example is that hand soap dispensers that use infrared to determine when to dispense soap will often work better for lighter skin than darker skin and issues around lighting and vision and skin tone go back to the foundation of photography let alone computer vision the design of film of cameras and printing processes was oriented around primarily making lighter skin photograph well as in these soap-called Shiley cards that were used by Kodak。

😊，For calibration， these resulted in much worse experiences for。

People with darker skin using these cameras。 There has been a good amount of work on this and progress since four or five years ago。

 one example of the kind of tool that can help with this are these model cards。

 This particular format for talking about what a model can and cannot do that was published by a number of researchers。

 including Margaret Mitchell and Tim Gabriel， it includes explicitly considering things like on which human subgroups of interest。

 Many of them minoritized identities， how well does the model perform。

 Huging face has good integrations for creating these kinds of model cards。

 I think it's important to note that just solving these things by changing the data around or by calculating demographic information is not really an adequate response。

 If the CEO of Kodak or their partner had been photographed poorly by those cameras。

 then theres no chance that that issue would have been allowed to stay for decades。

 So when you're looking at inviting people for talks hiring people or joining organizations。😊。

You try to make sure that you have worked to reduce the bias of that discovery process by diversifying your network and your input sources。

 The diversify tech job board is a really wonderful source for candidates and then there are also professional organizations inside the M world。

 black and AI and women in data science being two of the larger and more successful ones。

 these are great places to get started to make the kinds of professional connections that can improve the representations of these minoritized groups in the engineering and design and product management process where these kinds of issues should be solved。

 A lot of progress has been made， but these problems are still pretty difficult to solve an unbiased face detector might not be so challenging。

 but unbiased image generation is still really difficult。 For example。

 if you make an image generation model from internet scrape data without any safeguards in place than if you ask it to generate a picture of a CEO it will generate the stereotypical CEO。

 a sixfoot or taller white man， and this applies。Acroross a wide set of jobs and situations people can find themselves in。

 And this led to a lot of criticism of early text image generation models like Dolly。

 and the solution that openaiI opted to this was to edit prompts that people put in。

 if you did not fully specify what kind of person should be generated then race and gender words would be added to the prompt with weights based on the world's population。

 So people discovered this somewhat embarrassingly by writing prompts like a person holding a sign that says or pixel art of a person holding a text sign that says and then seeing that the appended words were then printed out by the model。

 Suffice it to say that this change did not make very many people very happy and indicates that more work needs to be done to devbi image generation models at a broader level than just fairness。

 we can also ask whether the system we're building is accountable to the people it' serving or acting upon。

 And this is important because some people consider。

Explanation and accountability in the face of important judgments to be human rights。

 This is the right to an explanation in the European Union's general data protection regulation GDPR。

 there is a subsection that mentions the right to obtain an explanation of a decision reached after automated assessment and the right to challenge that decision the legal status here is a little bit unclear there's a nice archive paper that talks about this a bit about what the right to an explanation might mean but what's more important for our purposes is just to know that there is an increasing chorus of people claiming that this is indeed a human right and it's not an entirely new concept and it's not even really technology or automation specific as far back as 1974 has been the law in the United States that if you deny credit to a person you must disclose the principal reasons for denying that credit application and in fact。

 I found this interesting It's expected that you provide no more than four reasons why you denied them credit but the general idea that somebody has。

A right to know why something happened to them in certain cases is enshrined in some laws。

 So what are we supposed to do if we use a deep neural network to decide whether somebody should be advanced credit or not。

 So there are some offthe shelf methods for introspecting deep neural networks that are all based off of input output gradients。

 How would changing the pixels of this input image change the class probabilities in the output。

 So this captures a kind of local contribution。 But as you can see from the small image there。

 It doesn't produce a very compelling map。 And there's no reason to think that just changing one pixel a tiny bit should really change the models's output that much。

 One improvement to that called smooth grad is to add noise to the input and then average results。

 kind of getting a sense for what the gradients look like in a general area around the input。

 There isn't great theory on why that should give better explanations。

 but people tend to find these explanations better。

 And you can see in the smooth grad image on the left there that you can pick out the picture of a bird。

 It seems like that is giving a better explanation or。

that we like better for why this network is identifying that as a picture of a bird。

 There's a bunch of kind of hacking methods like specific tricks you need when you' when you're using the re activation。

 there's some methods that are better for classification like gradcam one that is more popular integrated gradients takes the integral of the gradient along a path from some baseline to the final image and this method has a nice interpretation in terms of cooperative game theory。

 something called a Shapley value that quantifies how much a particular collection of players in a game contributed to the final reward and adding noise to integrated gradients tends to produce really clean explanations that people like but unfortunately。

 these methods are generally not very robust。 their outputs tend to correlate pretty strongly in the case of images with just an edge detector there's built in biases to convolutional networks in the architectures that we use that tend to emphasize。

😊，Size certain features of images。 what this particular chart shows from this archive paper by Julius Atobeo。

 more It heart and others is that even as we randomize layers in the network going from left to right。

 we are randomizing starting at the top of the network and then randomizing more layers going down。

 even for popular methods like integrated gradients with smoothing or guided back propagation。

 we can effectively randomize a really large fraction of the network without changing the gross features of the explanation and resulting in an explanation that people would still accept and believe even though this network is now producing random output。

 So in general， introspecting deep neural networks and figuring out what's going inside them require something that looks a lot more like a reverse engineering process that's still very much a research problem。

 There's some great work on dist on reverse engineering primarily vision networks and then some great work from anthropic AI recently on transformer circuits that's reverse engineering large language model。

And Chris Ola is the researcher who's done the most work here。

 but it still is the sort of thing that even getting a loose qualitative sense for how neural networks work and what they are doing in response to inputs is still the type of thing that takes a research team several years So building a system that can explain why it took a particular decision is maybe not currently possible with deep neural networks。

 but that doesn't mean that the systems that we build with them have to be unaccountable and somebody dislikes the decision that they get and the explanation that we give is well the neural networks said you shouldn't get alone and they challenge that it might be time to bring in a human in the loop to make that decision and building that in to the system so that it's an expected mode of operation and is considered an important part of the feedback and the operation of the system is key to building an accountable system so this book automating inequality by Virginia Ubanks talks a little bit about the ways in which technical systems as they're built today。

Are very prone to this unaccountability where the people who are in most impacted by these systems。

 Some of the most critical stakeholders for these systems， for example。

 recipients of government assistance are unable to have their voices and their needs heard and taken into account in the operation of a system。

 So this is perhaps the point at which you should ask when building a system with machine learning。

 whether they should be built at all and particular to ask who benefits and who is harmed by automating this task。

 In addition to concerns around the behavior of models。

 increasing concern has been pointed towards data and in particular。

 who owns and who has rights to the data involved in the creation of machine learning systems。

 It's important to remember that the training data that we use for our machine learning algorithms is almost always generated by humans。

 and they generally feel some ownership over that data。

 And we end up behaving a little bit like this comic on the right。

 where they hand us some data that they made， and then we say， oh， this is ours Now。 I made this and。

In particular， the large datasets used to train the really large models that are pushing the frontiers of what is possible with machine learning are produced by crawling the internet by searching over all the images all the text posted on the Internet and pulling large fractions of it down。

 and many people are not aware that this is possible， let alone legal。 And so to some extent。

 any consent that they gave to their data being used was not informed。 and then additionally。

 as technology has changed in the last decade and machine learning has gotten better。

 what can be done with data has changed。 somebody uploading their art a decade ago。

 certainly did not have on their radar， the idea that they were giving consent to that art being used to create an algorithm that can mimic its style。

 And you can in fact， check whether an image of interest to you has been used to train one of the large text image models。

 specifically this have I beentrained co website will search through the layon data that is used to train the stable diffusion models for。

Images that you upload so you can look to see if any pictures of view were incorporated into the dataset。

 And this goes further than just pictures that people might rather not have used in this way to actual data that has somehow been obtained illegally。

 Theres an arts technical article a particular artist who was interested in this found that some of their medical photos。

 which they did not consent to have uploaded to the Internet。

 somehow found their way into the lay on data。 And so cleaning large web scraped data sets from this kind of illegally obtained data is definitely going to be important as more attention is paid to these models as they are productized and monetized and more on people's radar。

 even for data that is obtained legally saying， well。

 technically you did agree to this does not generally satisfy people。

 Remember the Facebook emotion research study technically some reading of the Facebook user data policy did support the way that they were running their experiments。

 but。Many users disagreed Many artists feel that creating an art generation tool that threatens their livelihoods and copies art down to the point of even faking watermarks and logos on images when told to recreate the style of an artist is an ethical use of that data and it certainly is the case that creating a sort of parrot that can mimic somebody is something that a lot of people find concerning dealing with these issues around data governance is likely to be a new frontier。

 Emod Must of stable diffusion has said that he's partnering with people to create mechanisms for artists to opt in or opt out of being included in training data sets for future versions of stable diffusion I found that noteworthy because Mutq has been very vocal in his defense of image generationeration technology and of what it can be used for but even he is interested in adjusting the way data is used。

 There's also been work from tech For artist like Holly Herndon who。

Was involved in the creation of have I been trained around trying to incorporate AI systems into art in a way that empowers artists and compensates them rather than imiserating them just as we can create cards for models。

 we can also create cards for data sets that describe how they were curated what the sources were and any other potential issues with the data and perhaps in the future。

 even how to opt out of or be removed from a data。 So this is an example from a hugging face as with model cards。

 there's lots of good examples of data cards on hugging face。 There's also a nice checklist。

 the Dion ethics checklist that is mostly focused around data ethics but covers a lot of other ground。

 they also have this nice list of examples for each question in their checklist of cases where people have run into ethical or legal trouble by building an ML project that didn't satisfy a particular checklist item running underneath all of this has been this final。

 most important question of whether the system should be built at all。 one particular。

Use case that very frequently elicits this question is building m powered weaponry。

 M powered weaponry is already here。 It's already starting to be deployed in the world。

 There are some remote controlled weapons that use computer vision for targeting。

 deployed by the Israeli military in the West Bank using this smart shooter technology that's designed to。

 in principle， take normal weapons and add computer visionbased targeting to them to make them into smart weapons Right now。

 this deployed system shown on the left， uses only sponge tipped bullets。

 which are designed to be less lethal， but they can still cause serious injury and according to the deploys in the pilot stage。

 So it's a little unclear to what extent autonomous weaponry is already here and being used because the definition is a little bit blurry。

 So for example， the hayro drone shown in the top left is a loitering munition a type of drone that can fly around hold this position for a while and then automatically destroy any radar system that locks onto it。

 This type of drone was used in the。goro Kak war between Armenia and Azerbaijan in 2021。

 But there's also older autonomous weapon systems。 The phalanth Sea whiz is designed to automatically fire at targets moving towards naval vessels at very。

 very high velocities。 So these are velocities that usually only achieved by rocket munitions not by manned craft。

 and that systemss been used since at least the first Gulf War in 1991 there was an analysis in 2017 by the economists to try and look for how many systems with automated targeting there were and in particular how many of them could engage with targets without involving humans at all。

 So it'd be the last section of human out of the loop systems。

 But given the general level of secrecy in some cases and hype and others around military technology。

 it can be difficult to get a very clear sense。 and the blurring of this definition has led some to say that autonomous weapons are actually at least 100 years old。

 for example， antipersonnel mines that were used starting in the 30s and in World War2 attempt to detect。

person has come close to them and then explode。 and in some sense， that is an autonomous weapon。

 And if we broaden our definition that far， then maybe lots of different kinds of traps are some form of autonomous weapon。

 but just because these weapons already exist and maybe even have been around for a century does not mean that designing M powered weapons is ethical antiperson minds。

 in fact， are the subject of a mind band treaty that a very large number of countries have signed。

 unfortunately not some of the countries with the largest militaries in the world。

 but that at least suggests that for one type of autonomous weapon that has caused a tremendous amount of collateral damage。

 there's interest in banning them。 And so perhaps rather than building these autonomous weapons that we can then ban them。

 it would be better if we just didn't build them at all。

 So the campaign to stop killer robots is a group to look into if this is something that's interesting to you brings us to the end of our tour of the four common questions that people raise around the ethics of building an M system。

I've provided some of my answers to these questions and some of the common answers to these questions。

 But you should have thoughtful answers to these for the individual projects that you work on。

 First is the model fair， I think it's generally possible。

 but it requires tradeoffs I the system accountable。

 I think it's pretty challenging to make interpretable deep learning systems where interpretability allows an explanation for why a decision was made。

 but making a system that's accountable where answers can be changed in response to user feedback or perhaps user lawsuit is possible。

 you'll definitely want to answer the question of who owns the data upfront and be on the lookout for changes。

 especially to these large scale internet scraped data sets。 And then lastly。

 should this be built at all。 You'll want to ask this repeatedly throughout the life cycle of the technology。

 I wanted to close this section by talking about just how much the machine learning world can learn from medicine and from applications of machine learning to medical problems。

 This is a field I've had a chance to work in。And I've seen some of the best work on building with M L responsibly come from this field。

 And fundamentally， it' because of a mismatch between machine learning and medicine that impedance mismatch has LED to a ton of learning。

 So first， we'll talk about the fiasco that was machine learning and the Covid-19 pandemic。

 then briefly consider why medicine would have this big of a mismatch with machine learning and what the benefits of examining it closer might be。

 And then lastly， we'll talk about some concrete research on auditing and frameworks for building with M L that have come out of medicine。

 First， something that should be scary and embarrassing for people in machine learning。

 Medical researchers found that almost all machine learning research on Covid-19 was effectively useless。

 This is in the context of a biomedical response to Covid-19。 That was an absolute triumph。

 In the first year。 Vas prevented some tens of millions of deaths。

 These vaccines were designed based on novel technologies like lipid。

particles for delivering mRNA and even more traditional techniques like small molecule therapeutics。

 for example， Paxlovid， the quality of research that was done was extremely high。

 so on the right we have an inferred 3D structure for a coronavirus protein in complex with the primary effective molecule in Paxlovid。

 allowing for a mechanistic understanding of how this drug was working at the atomic level and at this crucial time。

 machine learning did not really acquit itself well。

 so there were two reviews one in BMj and one in nature that reviewed a large set of prediction models for Covid-19。

 either prognosis or diagnosis， primarily prognosis in the case of the winein at all paper in BMj or diagnosis on the basis of chest X rays and CT scans and both of these reviews found that almost all of the papers were insufficiently documented did not follow best practices for developing models and。

Not have sufficient external validation testing on external data to justify any wider use of these models。

 even though many of them were provided as software or APIs ready to be used in a clinical setting。

 So the depth of the errors here is really very sobering。

 A full quarter of the papers analyzed in the Roberts at all review used a pneumonia data set as a control groups。

 The idea was we don't want our model just to detect whether people are sick or not。

 just having having Covid patients and healthy patients might cause models that detect all pneumonias as Covid。

 So let's incorporate this pneumonia data set。 But they failed to mention and perhaps failed to notice that the pneumonia data set was all children。

 all pediatric patients。 So the models that they were training were very likely just detecting children versus adults because that would give them perfect performance on pneumonia versus Covid on that data set。

 So it's a pretty egregious error of modeling and data construction alongside a bunch of other。

stttle errors around proper validation and reporting of models and methods。

 so I think one reason for the substantial difference in responses here is that medicine。

 both in practice and in research has a very strong professional culture of ethics that equips it to handle very。

 very serious and difficult problems。At least in the United States。

 medical doctors still take the Hippocratic oath， parts of which date back all the way to Hipppocrates。

 one of the founding fathers of Greek medicine， and one of the core precepts of that oath is to do no harm Meanwhile。

 one of the core precepts of the contemporary tech industry represented here by this M L generatedrated Greek bust of Mark Zuckerberg is to move fast and break things with the implication that breaking things is not so bad。

 And while that's probably the right approach for building lots of kinds of web applications and other software when this culture gets applied to things like medicine。

 the results can be really ugly one particularly striking example of this was when a retinal implant that was used to restore sight to some blind people was deprecated by the vendor and so stop working and there was no recourse for these patients because there is no other organs。

capableable of maintaining these devices。 The news here is not all bad for machine learning。

 There are researchers who are working at the intersection of medicine and machine learning and developing and proposing solutions to some of these issues that I think might have broad applicability on building responsibly with machine learning。

 First， the clinical trial standards that are used for other medical devices and for pharmaceuticals have been extended to machine learning。

 the spirit standard for designing clinical trials and the consort standard for reporting results of clinical trials。

 these have both been extended to include M with spirit AI and consort AI to links at the bottom of this slide for the details on the contents of both of those standards。

 one thing I wanted to highlight here was the process by which these standards were created and which is reported in those research articles which included an international survey with over 100 participants and then a conference with 30 participants to。

Come up with a final checklist and then a pilot use of it to determine how well it worked。

 So the standard for producing standards in medicine is also quite high and something we could very much learn from in machine learning So because of that work and because people have pointed out these concerns。

 progress is being made on doing better work in machine learning for medicine。

 this recent paper in the journal of the American Medical Association does a review of clinical trials involving machine learning and finds that for many of the components of these clinical trials standards。

 compliance and quality is very high incorporating clinical context。

 stating very clearly how the method will contribute to clinical care but there are definitely some places with poor compliance。

 for example， interestingly enough very few trials reported how low quality data was handled how data was assessed for quality and how cases of poor quality data should be handled。

 I think that's also something that the broader machine learning world could do a better job。

And then also analysis of errors that models made， which also shows up in medical research and clinical trials as analysis of adverse events。

 this kind of error analysis was not commonly done and this is something that in talking about testing and troubleshooting and in talking about model monitoring and continual learning we've tried to emphasize the importance of this kind of error analysis for building with ML there's also this really gorgeous pair of papers by Lauren Oakden Rainer and others in the Lancet that both developed and applied this algorithmic auditing framework for medical M。

 So this is something that is probably easier to incorporate into other ML workflows that is a full- on clinical trial approach but still has some of the same rigor incorporates checklists and tasks and defined artifacts that highlight what the problems are and what needs to be tracked and shared while building a machine learning system。

 one particular component that I wanted to highlight and is here indicated。

is that there's a big emphasis on failure modes and error analysis and what they call adversarial testing。

 which is coming up with different kinds of inputs to put into the model to see how it performs。

 So sort of like a behavioral check on the model。 These are all things that we've emphasized as part of how to build a model Well。

 There's lots of other components of this audit that the broader M community would do well to incorporate into their work。

 there's a ton of really great work being done a lot of these papers are just within the last three or six months。

 So I think it's a pretty good idea to keep your finger on the pulse here so to speak in medical M the Stanford Institute3I in medicine has a regular panel that gets posted on YouTube they also share a lot of great other kinds of content via Twitter and then a lot of the researchers who did some of the work that I shared Lauren Oton Rainer。

 Benjaminhn are also active on Twitter along with other folks who've done great work that I didn't get time to talk about like Judy de Choia and Matt Lungren closing out this section like medicine。

Learning can be very intimately intertwined with people's lives。 And so ethics is really。

 really salient。 Perhaps the most important ethical question to ask ourselves over and over again is should this system be built at all。

 What are the implications of building this system of automating this task are this work and it seems clear that if we don't regulate ourselves。

 we will end up being regulated and so we should learn from older industries like medicine rather than just assuming we can disrupt our way through。

 So as our final section， I want to talk about the ethics of artificial intelligence。

 So this is clearly a frontier， both for the field of ethics trying to think through these problems and for the technology communities that are building this。

 I think that right now， false claims and hype around artificial intelligence are the most pressing concern。

 but we shouldn't sleep on some of the major ethical issues that are potentially oncoming with AI。

 So right now， claims and hyperbole and hype around。



![](img/792f5e37e9d49350c16e6d14fb60e638_11.png)

![](img/792f5e37e9d49350c16e6d14fb60e638_12.png)

Official intelligence are outpacing capabilities， even though those capabilities are also growing fast。

 and this risks a kind of blowback。 So one way to summarize this is say that if you call something autopilot people are going to treat it like autopilot and then be upset or worse when that's not the case。

 so famously there was an incident where somebody who believe that Teslas lane and breaking assistant system autopilot was really full self-driving was killed in a car crash。

 and this gap between what people expect out of M systems and what they actually get is something that Josh talked about in the project management lecture。

 So this is something that we're already having to incorporate into our engineering and our product design that people are overselling the capacities of M systems in a way that gives users a bad idea of what is possible And this problem is very widespread even large and mature organizations like IBM can create products like Watson。

 which was a capable question and answering。

![](img/792f5e37e9d49350c16e6d14fb60e638_14.png)

Sytem and then sell it as artificial intelligence and try to revolutionize or disrupt fields like medicine and then end up falling far short of these extremely lofty goals。

 they've set themselves And along the way， they get at least the beginning journalistic coverage with pictures of robot hands reaching out to grab balls of light or brains inside computers or computers inside brains So not only do companies oversell what their technology can do。

 but these overstatements are repeated or amplified by traditional and social media。

 and this problem even extends to academia。 there is a infamous now case where Jeff H in 2017 said that radiologists at that point where like Wiily coyote already over the edge of the cliff and haven't realized that there's no ground underneath them and that people should stop training radiologists now because within five years now deep learning is going to be better than radiologists Some of the work in the intersection of medicine and M that I。

😊，Was done by people who were in their radiology training at the time cut around the time。

 this statement was made and were lucky that they continued training as radiologists while also gaining M expertise so that they could do the slow hard work of bringing deep learning and machine learning into radiology。

 This overall problem of overselling artificial intelligence。 you could call AI snake oil。

 So that's the name of an upcoming book and a new substack by Arvin Neionan are now very good friend。

 And so this refers not just to people overselling the capabilities of large language models or predicting that will have artificial intelligence by Christmas。

 But people who use this general aura of hyp and excitement around artificial intelligence to sell shoddy technology an example from this really great set of slides linked here a tool elevate that claims to be able to assess personality and job suitability from a 3 second video。

 including identifying whether the person in the video is a change agent or not。 So the call here is。

😊，ToSe out the actual places where there's been rapid improvement in what's possible with machine learning。

 for example， computer perception， identifying the contents of images， face recognition。

 Neion in here even includes medical diagnosis from scans from places where there's not been as much progress。

 And so the split that he proposes that I think is helpful is that most things that involve some form of human judgment like determining whether something is hate speech or what great an essay should receive These are on the borderline。

 Most forms of prediction。 especially around what he calls social outcomes。 So things like policing。

 Jo， child development， these are places where there has not been substantial progress。

 and where the risk of somebody essentially writing the coattails of G3 with some technique that doesn't perform any better than linear regression is at its highest。

 So we don't have artificial intelligence yet。 But if we do synthesize intelligence agents。

 a lot of thorny ethical questions are going to immediately arise。



![](img/792f5e37e9d49350c16e6d14fb60e638_16.png)

good idea as a field and as individuals for us to think a little bit about these ahead of time。

 So there's broad agreement that creating sentient intelligent beings would have ethical implications。

 just this past summer， Google engineer Blake Lemoyne became convinced that a large language model built by Google Lambda was。

 in fact， conscious。 And almost everyone agrees that that's not the case for these large language models。

 but there's pretty big disagreement on how far away we are。 and perhaps most importantly。

 this concern did cause a pretty big reaction， both inside the field and in the popular press。

 In my view， it's a bit unfortunate that this conversation was started so early。

 because it's so easy to dismiss this claim if it happens too many more times。

 we might end up inurd to these kinds of conversations in a boy who cried AI type situation。

 There's also a different set of concerns around what might happen with the creation of a selfimp。😊。



![](img/792f5e37e9d49350c16e6d14fb60e638_18.png)

Artificial intelligence。 So there's already some hints in this direction for one。

 the latest Nvidia GPU architecture Hopper incorporates a very large number of AI design circuits pictured here on the left。

 the quality of the AI design circuits are superior。

 this is also something that's been reported by the folks working on TpUus at Google。

 there's also cases in which large language models can be used to build better models， for example。

 large language models can teach themselves to program better and large language models can also use large language models。

 at least as well as humans。 This suggests the possibility of virtuous cycles in machine learning capabilities and machine intelligence and failing to pursue this kind of very powerful technology comes with a very substantial opportunity cost。

 This is something that's argued by the philosopher Nick Bostrom in a famous paper called astronomical waste that points out just given the size of the universe。

 the amount of resources and the amount of time it will be around。 There's a huge。😊。

Cost in terms of potential good， potential lives worth living that we leave on the table。

 if we do not develop the necessary technology quickly。

 But the primary lesson that's drawn in this paper is actually not that technology should be developed as quickly as possible。

 but rather that it should be developed as safely as possible。

 which is to say that the probability that this imagined galaxy or universe spanning utopia comes into being that probability should be maximized。

 And so this concern around safety originating the work of Bostrom and others has become a central concern for people thinking about the ethical implications of artificial intelligence。

 And so the concerns around self-improving intelligence systems that could end up being more intelligent than humans are nicely summarized in the parable of the paperclip maximizeimr also from Bostrom。

 at least popularized in the book superintelligence。

 So the idea here is the classic example of this proxy problem in alignment。😊。

So we design an artificial intelligence system for building paper clips。

 so it's designed to make sure that the paperclip producing component of our economy runs as effectively as possible。

 produces as many paper clips as it can， and we incorporate self-improvement into it so that it becomes Spartan more capable over time。

 At first， it improves human utility as it introduces better industrial processes for papercs。

 but as it becomes more intelligent， perhaps it finds a way to manipulate the legal system and manipulate politics to introduce a more favorable tax code for paperclip related industries。

 And that starts to hurt overall human utility even as the number of papercs created and the capacity of the paperclip maximizer increases。

 And of course， at the point when we have mandatory national service in the paperclip mines or that all matter in the universe is converted to paperclips we've pretty clearly decreased human utility as this paperclip maximizer has maximized its objective and increased its own capacity。

 So this still feels fairly far away。😊，And a lot of the speculations feel a lot more like science fiction than science fact。

 but the stakes here are high enough that it is certainly worth having some people thinking about and working on it and many of the techniques can be applied to controlled and responsible deployment of less capable Ml systems as a small aside。

 these ideas around existential risk and superintelligences are often associated with the effective altruism community。

 which is concerned with the best ways to do the most good。

 both with what you do with your career one of the focuses of the 80000 hours organization and also through charitable donations as a way to by donating to the highest impact。

 Charrities and nonprofits have the largest positive impact on the world。

 So there's a lot of very interesting ideas coming out of this community and it's particularly appealing to a lot of folks who work in technology and especially in machine learning。

 So it's worth checking out。 So that brings us to the end of our planned agenda here after giving。😊。

Some context around what our approach to ethics in this lecture would look like。

 We talked about ethical concerns in three different fields。 First。

 past and immediate concerns around the ethical development of technology。

 then up and coming and near future concerns around building ethically with machine learning。

 and then finally a taste of the ethical concerns we might face in a future where machine learning gives way to artificial intelligence with a reminder that we should make sure not to oversell our progress on that front。

 So I got to the end of these slides and realized that this was the end of the course and felt that I couldn't leave it on doer and sad note of unusable medical algorithms and existential risk from superintelligs。

 So I wanted to close out with a bit of a more positive note on the things that we can do。

 So I think the first and most obvious step is education。

 a lot of these ideas around ethics are unfamiliar to。



![](img/792f5e37e9d49350c16e6d14fb60e638_20.png)

![](img/792f5e37e9d49350c16e6d14fb60e638_21.png)

People with a technical background， there's a lot of great longer form content that captures a lot of these ideas and can help you build your own knowledge of the history and context and eventually your own opinions on these topics。

 I can highly recommend each of these books。 The alignment problem is a great place to get started。

 It focuses。😊，Pret tightly on ML ethics and AI ethics。

 it covers a lot of recent research and is very easily digestible for an ML audience。

 You might also want to consider some of these books around more tech ethics like weapons of math destruction by Cat O'neill and automating inequality by Virginia Ubank from there。

 you can prioritize things that you want to act on make your own two by two around things that have impact now and can have very high impact for me。

 I think that's things around deceptive design and dark patterns and around AI snake oil。

 Then there's also places where acting in the future might be very important and high impact for me。

 I think that's things around ML weaponry behind my head is existential risk from superintelligs on super high impact。

 but something that we can't act on right now。 and then all the things in between。

 you can create your own two by two on these and then search around for organizations。

 communities and people working on these problems to align。😊。



![](img/792f5e37e9d49350c16e6d14fb60e638_23.png)

Self with And by way of a final goodbye as we're ending this class。

 I want to call out that a lot of the discussion of ethics in this lecture was very negative because of the framing around cases where people raised ethical concerns。

 But ethics is not and cannot be purely negative about avoiding doing bad things。

 the work that we do in building technology with machine learning can do good in the world。

 not just avoid doing harm。 We can reduce suffering。

 So this diagram here from a neuroscience from a brain machine interface paper from 2012 is what got me into the field of machine learning in the first place。

 It shows atetraplegic woman who has learned to control a robot arm using only her thoughts by means of an electrode attached to her head。

 And while the technical achievements in this paper were certainly very impressive。

 The thing that made the strongest impression on me reading this paper in college was the smile on the woman's face in the final panel。

 If you've experienced this kind of limit mobility either yourself。😊，Or in someone close to you。

 then you know that the joy， even from something as simple as being able to feed yourself。

 is very real。 We can also do good by increasing joy， not just reducing suffering。

 despite the concerns that we talked about with text to image models。

 they're clearly being used to create beauty and joy。 or as Ted Underwood。

 a digital humanity scholar put it to explore a dimension of human culture that was accidentally created across the last 5000 years of captioning。

 That's beautiful。 And it's something we should hold on to。

 That's not to say that this happens automatically by building technology。

 the world automatically becomes better。 but leading organizations in our field are making proactive statements on this。

 Open AI around longterm safely around longterm safety and broad distribution of the benefits of machine learning and artificial intelligence research。

 Deep mind stating which technologies they won't pursue and making a clear statement of a goal of broadly benefit humanity。

 The final bit of really great news that I have is that。😊。



![](img/792f5e37e9d49350c16e6d14fb60e638_25.png)

The tools for building M L Well that you've learned throughout this class align very well with building M L for good。

 So we saw it with the medical machine learning around failure analysis。

 And we can also see it in the principles for responsible development from these leading organizations Deep mind mentioning accountability to people and gathering feedback。

 Google AI mentioning it as well。 And if you look closely at Google AI's list of recommended practices for responsible AI。

 Use multiple metrics to assess training and monitoring。 Under limitations。

 Use tests directly examine raw data， Monor and update your system after deployment。

 These are exactly the same principles that we've been emphasizing in this course around building M L powered products the right way。

 these techniques will also help you build machine learning that does what's right。

 And so on that note， I want to thank you for your time and your interest in this course。😊。



![](img/792f5e37e9d49350c16e6d14fb60e638_27.png)

And I wish you the best of luck as you go out to build with M。 L。

