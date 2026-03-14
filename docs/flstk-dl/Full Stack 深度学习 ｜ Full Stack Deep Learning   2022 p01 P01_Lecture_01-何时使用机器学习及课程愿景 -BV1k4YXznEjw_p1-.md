# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p01 P01_Lecture_01-何时使用机器学习及课程愿景 -BV1k4YXznEjw_p1-

![](img/f2625c9b79ef1bd654c56af86be78b3c_0.png)

Hey everyone， welcome to the 2022 edition of Fulltack deep learning。 I'm Josh Tobin。

 one of the instructors。 and I'm really excited about this version of the class because we've made a bunch of improvements and I think it comes at a really interesting time to be talking about some of these topics。

 Let's dive in today we're gonna cover a few things First we'll talk about why this course exists and what you might hope to take away from it then we'll talk about the first question you should ask when you're starting a new M project which is should we be using M for this at all and then we'll talk through the highlevel overview of what the lifecycle of a typical ML project might look like。

 which will also give you a conceptual outline for some of the things we'll talk about in this class。

 Really what is full stack deep learning about We aim to be the course and community for people who are building products that are powered by machine learning and I think it's a really exciting time to be talking about ML power products because machine learning is rapidly becoming a mainstream technology and you can see this in startup funding in job postings as well as in the continued investment of large companies in this technology。

 I think it's particularly interesting to think about how this has changed since 2018 when we started。



![](img/f2625c9b79ef1bd654c56af86be78b3c_2.png)

![](img/f2625c9b79ef1bd654c56af86be78b3c_3.png)

![](img/f2625c9b79ef1bd654c56af86be78b3c_4.png)

Teaching the class in 2018， a lot of the most exciting ML powered products were built by the biggest companies。

 You had self-driving cars that were starting to show promise。

 you had systems like translation from big companies like Google that were really starting to hit the market in a way that was actually effective but the broader narrative in the field was that very few companies were able to get value out of this technology and even on the research side now G3 is becoming a mainstream technology。

 but in 2018， G1 was one of the state of the art examples of language models and if you look at what it actually took to build a system like this it was the code in the standardization around it was still not there like these technologies were still hard to apply now on the other hand。

 there's a much wider range of really powerful products that are powered by machine learning Dolly is I think a great example image generation technology on the consumer side Tiktok is a really powerful example but it's not just massive companies now that are able to build machine learning powered products Descript is an application that we at full stack deep learning use all the time。

In fact， we'll probably use it to edit this video that I'm recording right now and startups are also building things like email generation So there's a proliferation of machine learning powered products and the narrative that shifted I think a little bit as well which is that before these technologies were really hard to apply but now there's standardization that's emerging both around the technology stack transformers and NLP starting to seep their way into more and more use cases as well as the practices around how to actually apply these technologies in the world one of the biggest changes in the field in the past four years has been the emergence of this term called MLOs which we'll talk a lot about in this class and so if you asked yourself why why is this changed so rapidly I think in addition to the field just maturing and research continuing to progress I think one of the biggest reasons is that the training of models is starting to become commoditized we showed a couple of slides ago how complicated code for GT1 was now using something like huggingface you can deploy a state of the art NLP model or computer vision model in one or two lines of code on top of that。

Aml is starting to actually work for a lot of applications I think four years ago。

 were pretty skeptical about it。 now I think it's a really good starting point for a lot of problems that you might want to solve and companies are starting to provide models really as a service where you don't even have to download open source package to use it you can just make a network call and you can have predictions from a state of the art model and on the software side a lot of frameworks are starting to standardize around things like Kais and pytorch lightning So a lot of the like spaghetti code that you had to write to build these systems just isn't necessary anymore And so I think if you project forward a few years what's going to happen in M。

 I think the history of the ML is characterized by rise and fall of the public perception of the technology these were driven by a few different AI winters that happened over the history of the field where the technology didn't live up to its height live up to its promise and people became skeptical about it what's going to happen in the future of the field I think what a lot of people think is that this time is different we have real applications。

Of machine learning that are generating a lot of value in the world。

 And so the prospect of a true AI winter where people become skeptical about AI as a technology maybe feels less likely。

 but it's still possible。 a slightly more likely outcome is that the overall luster of the technology starts to wear off。

 but certain applications are getting a ton of value out of this technology。

 And then I think the upside outcome for the field is that AI continues to accelerate really rapidly and it becomes pervasive and incredibly effective。

 And I think that's also what a lot of people believe to happen。

 And so what I would conjecture is that the way that we as a field avoid an AI winter is by not just making progress in research。

 but also making sure that that progress is translated to actual real-wor products。

 That's how we avoid repeating what's happened in the past that's caused the field to lose some of its luster。

 But the challenge that presents is that building m powered products requires a fundamentally different process in many ways than building the types of M systems you create in academia sort。

Process that you might use to develop a model in an academic setting。

 I would call flat earth machine learning flat Earth M。

 this is a process thatll probably be familiar to many people。 you start by selecting a problem。

 you collect some data to use to solve the problem。

 you clean and label that data you iterate on developing a model until you have a model that performs well on the dataset that you collected and then you evaluate that model and if the model performs well according to your metrics。

 then you write a report， produce a Jupyter notebook a paper or some slides and then you're done but in the real world。

 the challenge is that if you deploy that model in production。

 it's not going to perform well in the real world for long necessarily and so ML powered products require this outer loop where you deploy the model into production。

 you measure how that model is performing when it interacts with real users。

 you use the real world data to build data flywheel and then you continue this as part of an outer loop。

 some people believe that the Earth isn't round just because you can't see the outer loop in the M system doesn't mean it's not there And so this course is really about how to do this process of building ML powered products。

What we won't cover as much is the theory in the math and the sort of computer science behind deep learning and machine learning more broadly。

 there are many great courses that you can check out to learn those materials we also will talk a little bit about training models and some of the practical aspects of that but this isn't meant to be your first course in training machine learning models again there's many great classes for that as well but what this classes about is the unique aspects that you need to know beyond just training models to build great ML power products so our goals in the class are to teach you a generalist skill set that you can use to build an MLP product and an understanding of how the different pieces of ML power products fit together we will also teach you a little bit about this concept of MLOs but this is not an MLOs class Our goal is to teach you enough MLOs to get things done but not to cover the full depth of MLOs as a topic we'll also share best practices from what we've seen to work in the real world and try to explain some of the motivations behind them and if you're on the job marketer if you're thinking about transitioning into。

A role in machine learning we also aim to teach you some things that might help you with ML engineering job interviews and then lastly in practice。

 I think what we've found to be maybe the most powerful part of this is forming a community that you can use to learn from your peers about what works in the real world and what doesn't we as instructors have solved many problems with M but there's a very good chance that we haven't solved one that's like the one that you're working on but in the broader full stackack deep learning community。

 I would bet that there probably is someone who's worked on something similar and so we hope that this can be a place where folks come together to learn from each other as well as just learning from us Now there are some things that we are explicitly not trying to do with this class we're not trying to teach you machine learning or software engineering from scratch if you are coming at this class and you have an academic background in M but you've never written production code before or you're a software engineer but have've never taken an M class before you can follow along with this class but I would highly recommend taking these prerequisites before you dive into the material here because I think get a lot more out of the class once you've learned the fundamentals。

Of each of these fields。 We're also not aiming to cover the full breadth of deep learning techniques or machine learning techniques more broadly。

 We'll talk about a lot of the techniques that are used in practice。

 but the chances are that we won't talk about the specific model that you use for your use case It's not the goal here。

 We're also not trying to make you an expert in any single aspect of machine learning We have projects and a set of labs that are associated with this course that will allow you to spend some time working on a particular application of machine learning。

 but there isn't a focus on becoming an expert in computer vision or NLP or any other single branch of machine learning and we're also not aiming to help you do research and deep learning or any other M field and similarly Mops is this broad topic that involves everything from infrastructure and tooling to organizational practices and we're not aiming to be comprehensive here The goal of this class is to show you end to end what it takes to build an ML powered product and give you pointers to the different pieces of the field that you'll potentially need to go deeper on to solve the particular problem that you're working。

So if you are feeling rusty on your prerequisites， but want to get started with the class anyway。

 here are some recommendations for classes on ML and software engineering that I'd recommend checking out if you want to remind yourself of some of the fundamentals I mentioned this distinction between ML power products and MLOs a little bit and so I want to dive into that a little bit more MLOs is this discipline that's emerged in the last couple of years really that is about practices for deploying and maintaining and operating machine learning models and the systems that generate these machine learning models in production and so a lot of MLOs is about how do we put together the infrastructure that will allow us to build models in a repeatable and governorable way how we're able to do this at scale。

 how we're able to collaborate on these systems as a team and how we're able to really run these machine learning systems in a potentially highscale production setting super important topic if your goal is to make ML work in the real world and there's a lot of overlap with what we're covering in this class but we see ML。

productss is kind of a distinct but overlapping discipline because a lot of what it takes to build a great MLP product goes beyond the infrastructure side and the sort of repeatability and automation side of machine learning systems and it also focuses on how to fit machine learning into the context of the product or the application that you're building So other topics that are in scope of this M product discipline are things like how do you understand how your users are interacting with your model and what type of model they need how do you build a team or an organization that can work together effectively on machine learning systems How do you do product management in the context of M What are some of the best practices for designing products that use Ml as part of them things like data labeling capturing feedback from users etc and so this class is really focused on teaching you end to end what it takes to get a product out in the world that uses M and will cover the aspects of MLOs that are most critical to understand in order to do that a little bit about us as instructors。

 I'm Josh Tobin on cofounder and CEO。machineine learning infrastructure startup called Gantry previously I was a research scientist openaiI and did my machine learning PhD at Berkeley and Charles and Sergey are my wonderful co-instructors who you'll be hearing from in the coming weeks on the history of full stacky learning so we started out as a boot camp in 2018 Sergey and I as well as my grad school advisor and our closecollaborator Peter Ael had this collective realization that a lot of what we had been discovering about making M work in the real world wasn't really well covered in other courses and we didn't really know if other people would be interested in this topic so we put it together as a onetime weekend long boot camp we started to get good feedback on that and so it grew from there and we put the class online for the first time in 2021 and here we are So the way that this class developed was a lot of this is from our personal experience our study and reading of the materials in the field we also did a bunch of interviews with practitioners from this list of companies and at this point like a much longer list as well So we're constantly out there talking to folks who are。

Doing this we're building MLP products and trying to fold their perspectives into what we teach in this class。

 Some logistics before we dive into the rest of the material for today。

 first is if you're part of the synchronous cohort。

 all of the communication for that cohort is going to happen on discord So if you're not on discord already then please reach out to us instructors and we'll make sure to get you on if you're not on Discord if you're not checking it regularly there's a high likelihood that you're gonna miss some of the value of the synchronous course we will have a course project again for folks who are participating in the synchronous option which we'll share more details about on Discord in the coming weeks and there's also I think one of the most valuable parts of this class is the labs。

 which have undergone like a big revamp this time around I want to talk a little bit more about what we're covering there So the problem that we're gonna be working on the labs is creating an application that allows you to take a picture of a handwritten page of text and then transcribe that into some actual text and so imagine that you have this web application where you can take a picture of your handwriting and then at the end you get the text。

Out of it。 And so the way that this is going to work is we're going to build a web backend that allows you to send web requests。

 decodedes those images and sends them to a prediction model an OCR model that will develop that will transcribe those into the text itself。

 and those models are going be generated by a model training system that will also show you how to build in the class。

 and the architecture that will use will look something like this will use state of the art tools that we think balance being able to really build a system like this in a principled way without adding too much complexity to what you're doing All right so just to summarize this section。

 machine learning power products are going mainstream and in large part this is due to the fact that it's just much much easier to build machine learning models today than it was even four or five years ago。

 And so I think the challenge ahead is given that we're able to create these models pretty easily how do we actually use the models to build great products and that's a lot of what we'll talk about in this class And I think the sort of fundamental challenges is that there's not only different tools that you need in order to build great。

Products but also different processes and mindsets as well。

 And that's what we're really aiming to do here in FSDL。

 So looking forward to covering some of this material and hopefully helping create the next generation of ML power products。

 The next topic I want to dive into is when to use machine learning at all。

 what problems is this technology useful for solving。

 And so the key points that we're gonna cover here are the first is that machine learning introduces a lot of complexity。

 And so you really shouldn't do it before you're ready to do it。

 and you should think about exhausting your other options before you introduce this to your stack on the flip side that doesn't mean that you need to a perfect infrastructure to get started。

 and then we'll talk a little bit about what types of projects tend to lend themselves to being good applications of machine learning and we'll talk about how to know whether projects are feasible and whether they'll have an impact on your organization。

 But to start out with when should you use machine learning at all。

 So I think the first thing that's really critical to know here is that machine learning projects have a higher failure rate than software projects in general the statistic that。



![](img/f2625c9b79ef1bd654c56af86be78b3c_6.png)

![](img/f2625c9b79ef1bd654c56af86be78b3c_7.png)

YouMost often floated around in blog posts or vendor pitches is that 87% this very precise number of machine learning projects fail。

 I think it's also worth noting that 73% of all statistics are made up on the spot So this one in particular I think is a little bit questionable whether this is actually a valid statistic or not anecdotally I would say that from what I've seen it's probably more like 25% It still a very high number still very high failure rate。

 but maybe not the 90 is% that people are quoting。The question you might ask is why is that the case right。

 why is there such a high failure rate for machine learning projects？You know。

 one reason that's worth acknowledging is that for a lot of applications。

 machine learning is fundamentally still research， So 100% success rate probably shouldn't be the target that we're aiming for。

 but I do think that many machine learning projects are doomed to fail maybe even before they are undertaken。

And I think there's a few reasons that this can happen。

 So oftentimes machine learning projects are technically infeasible or they're just scope poorly and there's just too much of a lift to even get the first version of the model developed and that leads to projects failing because they just take too long to see any value。

 Another common failure mode that's becoming less and less common is that a team that's really effective at developing a model may not be the right team to deploy that model into production。

 And so there's this friction after the model is developed where the model maybe looks promising in a Jupyter notebook。

 but it never makes the leap to prod。 And so hopefully you'll take things away from this class that will help you avoid being in this category。

 Another really common issue that I've seen is when you as a broader organization are not all on the same page about what we would consider to be successful here。

 And so I've seen a lot of machine learning projects fail because you have a model that you think works pretty well and you actually know how to deploy into production but the rest of the organization。

can't get comfortable with the fact that this is actually going be running and serving predictions to users So how do we know when we're ready to deploy And then maybe the most frustrating of all these failure modes is when you actually have your model work well and solves the problem that you set out to solve but it doesn't solve a big enough problem and so the organization decides。

 hey this isn't worth the additional complexity that it's going to take to make this part of our stack I think this is a point I want to double click on which is that really I think the bar for your machine learning project should be that the value of the project must outweigh not just the cost of developing it。

 but the additional complexity that machine learning systems introduce into your software and machine learning introduces a lot of complexity to your software So this is kind of a quick summary of a classic paper that I would recommend reading。

 which is the high interest credit card of technical debt paper the thesis of this paper is that machine learning as a technology tends to introduce technical debt at a much higher rate than most other software and the reasons。

The authors point to are one an erosion of boundary between systems。

 so machine learning systems often have the property， for example。

 that the predictions that they make influence other systems that they interact with if you recommend a user a particular type of content that changes their behavior and so that makes it hard to isolate machine learning as a component in your system。

It also relies on expensive data dependencies。 So if your machine learning system relies on a feature that's generated by another part of your system。

 then those types of dependencies the authors found can be very expensive to maintain it's also very common for machine learning systems to be developed with design antipatterns somewhat avoidable but in practice very common and the systems are subject to the instability of the external world if your user's behavior changes that can dramatically affect the performance of your machine learning models in a way that doesn't typically happen with traditional software So the upshot is before you start a new M project。

 you should ask yourself are we ready to use ML at all。

 do we really need this technology to solve this problem and is it actually ethical to use M to solve this problem to know if you're ready to use M some of the questions you might ask do we have a product at all。

 do we have something that we can use to collect the data to know whether this is actually working are we already collecting that data and storing it in the same way if you're not currently doing data collection then it's gonna be difficult。

Build your first M system And do we have the team that will allow us to do this knowing whether you need Ml to solve a problem？

 I think the first question that you should ask yourself is， do we need to solve this problem at all。

 or are we just inventing a reason to use M because we're excited about the technology。

 have we tried using rules or simple statistics to solve the problem with some exceptions。

 I think usually the first version of a system that you deploy that will eventually use M should be a simple rulebased or statisticsbased system because a lot of times you can get 80% of the benefit of your complex M system with some simple rules Now。

 there's some exceptions to this if the system is an MlP system or a computer vision system where rules just typically don't perform very well。

 But as a general rule， I think if you haven't at least thought about whether you can use a rulebased system to achieve the same outcome。

 then maybe you're not ready to use M yet。 And lastly， is it ethical。

 I won't dive into the details here because we'll have a whole lecture on this later in the course。

 Next thing I want to talk about is if we feel like we're ready to use M in our organization。

We know if the problem that we're working on is a good fit to solving it with machine learning。

 The sort of TlDR here is you want to look for like any other project prioritization。

 you want to look for use cases that have high impact and low cost。

 and so we'll talk about different heuristics that you can use to determine whether this application of machine learning is likely to be high impact and low cost and so we'll talk about heuristics like friction in your products。

 complex parts of your pipeline places where it's valuable to reduce the cost of prediction and looking at what other people in your industry are doing。

 which is a very underrated technique for picking problems to work on and then we'll also talk about some heuristics for for assessing whether a machine learning project is going to be feasible from a cost perspective overall prioritization framework that we're going to look at here is。

Projects that you want to select are ones that are feasible。

 So they're low cost and they're high impact。 Let's start with the high impact set of things。

 So what are some mental models you can use to find high impact Ml projects。

 And these are some of the ones that we'll cover。 So starting with a book called the Econs of AI。

 And so the question of this book asks is， what problems does machine learning make economically feasible to solve that were maybe not feasible to solve in the past。

And so the sort of core observation in this book is that really at a fundamental level。

 what AI does is it reduces the cost of prediction before maybe you needed a person。

 and that person would take five minutes to create a prediction。 It's very expensive。

 It's very operationally complex。 AI can do that in a fraction of a second for the cost of essentially running your machine or running your GPU Che prediction means that there's going to be predictions that are happening in more places。

 even in problems whereas too expensive to do before。

 And so the upshot of this mental model for project selection is。

Think about projects where cheap prediction will have a huge business impact。

 like where would you hire a bunch of people to make predictions that it isn't feasible to do now。

 the next mental model I want to talk about for selecting highimpact projects is just thinking about what is your products need And so I really like this article called three principles for designing ML power products from Spotify and in this article they talked about the principles that they use to develop the Discover weekly feature。

 which is I think is like one of the most powerful features of Spotify and really the way that they thought about it is this reduces frid for our users reduces the friction of chasing everything down yourself and just brings you everything in a neat little package And so this is something that really makes their product a lot better。

 And so that's another kind of easy way to come up with ideas for machine learning projects Another angle to think about is what our types of problems that machine learning is particularly good at。

 And one exploration of this mental model is an article called software 2。0 from Andre Carpathy。

 which is also definitely worth a read。The kind of main thesis of this article is that machine learning is really useful when you can take a really complex part of your existing software system。

 so a really messy stack of handwritten rules and replace that with machine learning。

 replace that with gradient descent。And so if you have a part of your system that is complex。

 manually defined rules， then that's potentially really good candidate for automating with M。

 And then lastly， I think it's worth just looking at what other people in your industry are doing with M。

 and there's a bunch of different resources that you can look at to try to figure out what other success stories with MR。

 I really like this article covering the spectrum of use cases of M and Netflix。

 there are various industry reports。 This is a summary of one from algorithmthmia。

 which kind of covers the spectrum of what people are using M to do。 and more generally。

 I think looking at papers from the biggest technology companies tends to be a good source of what those companies are trying to build with M and how they're doing it as well as earlier stage tech companies that are still pretty M forward and those companies I think are more likely to write these insights in blog posts than they are in papers And so here's a list that I didn't compile。

 but I think is really valuable of case studies of using machine learning in the real world that are worth going。

If you're looking for inspiration of what are types of problems you can solve and how might you solve them Okay so coming back to a prioritization framework。

 we talked about some mental models for what M projects might be high impact and the next thing that we're going to talk about is how to assess the cost of a machine learning project that you're considering So the way I like to think about the cost of machine learning projects is there's three main drivers for how much a project is going to cost The first and most important is data availability So how easy is it for you to get the data that you're going to need to solve this problem the second most important is the accuracy requirement that you have the problem that you're solving and then also important is the sort of intrinsic difficulty of the problem that you're trying to solve So let's start by talking about data availability the kind of key questions that you might ask here to assess whether data availability is going to be a bottleneck for your project is do we have this data already and if not how hard is and how expensive is it going to be to acquire how expensive is it not just to acquire but also to label if your labelers are really expensive。

Then getting enough data to solve the problem really well might be difficult。

 How much data will we need in total， this can be difficult to assess a priori。

 but if you have some way of guessing whether it's 5000 or 10，000 or 100000 data points。

 this is an important input and then how stable is the data So if you're working on a problem where you don't really expect the underlying data to change that much over time then the project is gonna be a lot more feasible than if the data that you need changes on a dayto-day basis So data availability Vidi is probably the most important cost driverri for a lot of machine learning powered projects。

 because data just tends to be expensive。And this is slightly less true outside of the deep learning realm。

 It's particularly true in deep learning where you often require manual labeling。

 but it also is true in a lot of other ML applications where data collection is expensive。

 And lastly， on data availability is what data security requirements do you have if you're able to collect data from your users and use that to retrain your model than that bodes well for the overall cost of the project if on the other hand。

 you're not even able to look at the data that your users are generating。

 then that's just gonna to make the project more expensive because it's gonna to be harder to debug and harder to build a data flywheelMoving on to the accuracy requirement。

 the kinds of questions you might ask here are。How expensive is it when you make a wrong prediction on one extreme you might have something like a self-driving car where a wrong prediction is extremely expensive because the prospect of that is really terrible。

 On the other extreme is something like let's say potentially a recommendationer system where if a user sees a bad recommendation once it's probably not really going to be that bad。

 maybe it affects their user experience over time and maybe and causes them to churn。

 but certainly not as bad as a wrong prediction in a self-driving car。

 you also need to ask yourself how frequently does the system actually need to be right to be useful。

 I like to think of systems like Dolly2， which is an image generation system as like a positive example of this where you can if you're just using Dolly2 as a creative supplement。

 you can generate thousands and thousands of images and select the one that you like best for your use case。

 So the system doesn't need to be right more than like once every end times in order to actually get value from it as a user On the other hand。

 if the system needs to be 10% reliable， like never ever make a wrong prediction。

In order for it to be useful then it's just gonna to be more expensive to build these systems and then what are the ethical implications of your model making wrong predictions is like an important question to consider as well and then lastly on the problem difficulty questions to ask yourself is this problem well defineded enough to solve with M are other people working on similar things doesn't necessarily need to be the exact same problem but if it's a sort of a brand new problem that no one's ever solved with M before that's going to introduce a lot of technical risk Another thing that's worth looking at if you're looking at other work on similar problems is how much compute did it actually take them to solve this problem and it's worth looking at that both on training side as well as on the inference side because if it's feasible to train your model but it takes five seconds to make a prediction and for some applications that will be good enough and for some it won't and then I think like maybe the weakest heuristic here but still potentially useful one is can a human do this problem at all a human can solve the problem then that's a decent indication that a machine learning system might be able to solve it？

Well， but not a perfect indication， as we'll come back to。

 So I want to double click on this accuracy requirement。

 Why is this such an important driver of the cost of machine learning projects？

 The fundamental reason is that in my observation， the project cost tends to scale like super linearly in your accuracy requirement。

 So as a very rough rule of thumb every time that you add an additional9 to your required accuracy。

 So moving from 99。9 to 99。99% accuracy might lead to something like a 10x increase in your project costs because you might expect to need at least 10 times as much data if not more in order to actually solve the problem to that degree of accuracy required。

 but also you might need a bunch of additional infrastructure monitoring support in order to ensure that the model is actually performing that accurately。

 Next thing I want to double click on is the problem difficulty。

 So how do we know which problems are difficult for machine learning systems to solve the first point I want to make here is this is like I think like a classically hard problem。

To really answer confidently。 And so I really like this comic for two reasons。

 The first is because it gets at this core property of machine learning systems。

 which is that it's not always intuitive which problems will be easy for a computer to solve and which ones will be hard for a computer to solve in 2010 doing Gs lookup was super easy and detecting whether a photo was a bird was like research team in five years level of difficulty。

 So not super intuitive as someone maybe be outside of the field。

 the second reason I like this comic is because it also points to the sort of second challenge in assessing feasibility in M。

 which is that this field just moves so fast that if you're not keeping up with what's going on in the state of the art。

 then your understanding of what's feasible will be stale very quickly。

 building an application to detect whether a photo is of a bird is no longer a research team in five years problem。

 It's like a API call in 15 minutes tag problem。 So take everything I say here with a grain of salt because the。

asibility of ML projects is notoriously difficult to predict Another example here is in the late 90s the New York Times when they were talking about sort of AI systems beating humans at chess predicted that it might be 100 years before a computer beats human echo go or even longer and less than 20 years later。

 machine learning systems from Deep mindd beat the best humans in the world at go。

 these predictions are notoriously difficult to make。 but that being said。

 I think it's still worth talking about。 And so one heuristic that you'll hear for what's feasible to do with machine learning is this heuristic from Andrew。

 which is that anything that a normal person can do in less than one second we can automate with AI。

 I think this is actually not a great heuristic for what's feasible to do with AI。

 but you'll hear it a lot。 So I wanted to talk about it anyway。

 there's some examples of where this is true So recognizing the content of images understanding speech。

 potentially translating speech， maybe grasping objects with a robot and things like that are things that you could point to as evidence for Andrew's statement being correct。

 but I think there's some really obvious counter。Examples as well。

 Maine learning systems are still no good at things that a lot of people are really good at。

 like understanding human humor or sarcasm， complex inhand manipulation of objects。

 generalizing to brand new scenarios that they've never seen before。

 this is a heuristic that you'll see it's not one that would recommend using seriously to assess whether your project is feasible or not There's a few things that we can say are definitely still hard in machine learning。

 I kept a couple of things in these slides that we talked about being really difficult in machine learning when we started teaching the class in 2018 that I think I would no longer consider to be super difficult anymore unsupervised learning being one of them。

 but reinforcement learning problems still tend to be not very feasible to solve for real world use cases。

 although there are some use cases where with tons of data and compute reinforcement learning can be used to solve real world problems within the context of supervised learning there are also still problems that are hard。

 So things like question answering a lot of progress over the last few years still these systems aren't perfect text summarization video prediction building 3D models。

Another example of one that I think I would use to say is really difficult。

 but with Nerf and all the sort of derivatives of that I think is more feasible than ever。

 real world speech recognition so outside of the context of a clean data set in a noisy room can we recognize what people are saying resisting adversarial examples doing math。

 although there's been a lot of progress on this problem as well over the last few months。

Solving world word problems or boguard problems。 This is an example， by the way。

 of a boguard problem。 It a visual analogy type problem。

 So this is kind of a laundry list of some things that are still difficult。

 even in supervised learning。 And so can we reason about this。

 what types of problems are still difficult to do。 So I think one type is where not the input to the model itself。

 but the prediction that the model is making in the output of the model or that is like a complex or high dimensional structure or where it's ambiguous So。

 for example，3D reconstruction， The 3D model that you're outputting is very high dimensional。

 And so that makes it difficult to do for Ml video prediction。

 not only high dimensional but also ambiguous just because。

 you know what happened in the video for the last five seconds。

 there's still maybe infinite possibilities for what the video might look like going forward。

So it's ambiguous and it's high dimensional， which makes it very difficult to do with M dialogue systems。

 again， very ambiguous， very openended， very difficult to do with M and openended recommender systems。

 So a second category of problems that are still difficult to do with M are problems where you really need the system to be reliable。

 machine learning systems tend to fail in all kinds of unexpected and hard to reason about ways。

 So anywhere where you need really high precision or robustness is going to be more difficult to solve using machine learning So failing safely out of distribution。

 for example， is still a difficult problem in M。 robustness to adversarial attacks is still a difficult problem in M and even things that are easier to do with low precision。

 like estimating the position and rotation of an object in 3D space can be very difficult to do if you have a high precision requirement。

 The last category of problems I'll point to here is problems where you need the system to be able to generalize well to data that's never seen before。

 This can be data that's out of distribution it can be where your system needs to do。

Something that looks like reasoning or planning or understanding of causality These problems tend to be more in the research domain today。

 I would say one example is in the self-driving car world dealing with edge cases。

 very difficult challenge in that field but also control problems in self-driving cars you know those stacks are incorporating more and more M into them whereas the computer vision and perception part of self-driving cars adopted machine learning pretty early the control piece was using more traditional methods for much longer and then places where you have a small amount of data again。

 like if you're considering machine learning broadly。

 small data is often possible but especially in the context of deep learning。

 small data still presents a lot of challenges。Summing this up like how should you try to assess whether your machine learning project is feasible or not。

 First question you should ask is do we really need to solve this problem with M at all。

 I would recommend putting in the work upfront to define what is the success criteria that we need and doing this with everyone that needs to sign up on the project in the end。

 not just the M team。 let's avoid being an M team that works on problems in isolation and then has those projects killed because no one actually really needed to solve this problem or because the value of the solution is not worth the complexity that it adds to your product then you should consider the ethics of using M to solve this problem and we'll talk more about this towards the end of the course in the ethics lecture that it's worth doing literature review to make sure that there are examples of people working on similar problems trying to rapidly build a benchmark data that's labeled so you can start to get some sense of whether your models performing well or not and only then building a minimum viable model So this is potentially even just manual rules or simple linear regression deploying this into production if it's feasible to do so or at least running this on your existing problem So。

Baseline and then lastly， it's worth just restating making sure that you once you've built this minimum viable model that may not even use M just really asking yourself the question of whether this is good enough for now or whether it's worth putting in the additional effort to turn this into a complex M system The next point I want to make here is that not all M projects really have the same characteristics and so should be and so you shouldn't think about planning all M projects in the same way I want to talk about some archetypes of different types of M projects and the implications that they have for the feasibility of the projects and how you might run the projects effectively。

 And so the three archetypes I want to talk to are defined by how they interact with real worldor users and so the first archetype is software 2。

0 use cases and so I would define this as。Taking something that software does today。

 So an existing part of your product that you have， let's say。

 and doing it better more accurately or more efficiently with M。

 It's taking a part of your product that's already automated or already partially automated and adding more automation or more efficient automation using machine learning then the next archetype is human in the loop systems。

 so this is where you take something that is not currently automated in your system。

 but it's something that humans are doing or humans could be doing and helping them do that job better more efficiently or more accurate accurately by supplementing their judgment with Mbased tools。

 preventing them from needing to do the job on every single data point by giving them suggestions of what they can do so they can shortcut their process in a lot of places。

 Human the loop systems are about making the humans that are ultimately making the decisions more efficient or more effective。

 And then lastly， autonomous systems。 And so these are systems where you take something that humans do today or maybe is just not being done at all today and fully automated with。

To the point where you actually don't need humans to do the judgment piece of it at all。

 And so some examples of software 2。0 are if you have an ID that has code completion。

 can we do better code completion by using M Can we take a recommendation system that is initially using some simple rules and make it more customized can we take our video game AI that's using this rulebased system and make it much better by using machine learning Some examples of human and loop systems would be building a product to turn hand drawnwn sketches into slides you still have human on the other end that's evaluating the quality of those sketches before they go in front of a customer or a stakeholder So it's a human in the loop system。

 but it's potentially saving a lot of time for that human email auto completion。

 So if you use Gmail you've seen these email suggestions where itll suggest sort of short responses to the email that you got I get to decide whether that email actually goes out to the world So it's not an automation system it's a human in the loop system or helping a radiologist to their job master。

Of autonomous systems are things like full self-driv Maybe there's not even a steering wheel in the car。

 I can't interrupt the autonomous system and take over control of the car even if I wanted to。

 or maybe it's not designed for me to do that very often fully automated customer support。

 So if I go on a company's website and I interact with their customer support without even having the option of talking to an agent or with them making it very difficult to talk to an agent。

 that's an autonomous system or for example， like fully automating website design。

 so that to the point where people who are not design experts can just click a button and get a website design for them。

 And so I think some of the key questions that you need to ask before embarking on these projects are a little bit different depending on which archetype your project falls into。

 So if you're working on a software 2。0 project then I think some of the questions you should be concerned about are how do you know that your models are actually performing improving performance over the baseline that you already have how confident are you that the type of performance improvement that you might be able to get froml is actually going to generate value for your business if it's just one。

Pcent better I that really worth the cost Then do these performance improvements lead to what's called a data flywheel。

 which I'll talk a little bit more about with human in the loop systems。

 you might ask a different set of questions before you embark on the project。

 Like how good does the system actually need to be useful if the system is able to automate 10% of the work of the human that is ultimately making the decisions or producing the end product。

 Is that useful to them or does that just slow it slow them down。

 How can you collect enough data to make it that good。

 I it possible to actually build a data that is able to get you to that useful threshold for your system and for autonomous systems。

 the types of questions you might ask are what is an acceptable failure rate for the system。

How many nines in your performance threshold do you need in order for this sort of not to cause harm in the world and how can you guarantee like how can you be really confident that it won't exceed that failure rate。

 And so this is something that in autonomous vehicles， for example。

 teams put a ton of effort into building the simulation and testing systems that they need to be confident that they won't exceed the failure rate that's except the very very low failure rate that's acceptable for those systems。

 I want to double click on this data flylywheel concepts for software 2。

0 we talked about can we build a data flylywheel that lead to better and better performance of the system And the way to think about data flylywheel is it's this virtuous cycle where as your model gets better。

 you are able to use better that better model to make a better product which allows you to acquire more users and as you have more users。

 there's users generate more data which you can use to build a better model and this creates this virtuous cycle。

 And so the connections between each of these steps are also important in order for more users to allow you to collect more data。

You need to have a data loop you need to have a way of automatically collecting data and deciding what data points to label from your users or at least processes for doing these in order for more data to lead to a better model that's kind of on you as an MLpractition you need to be able to translate more data more granular data more labels into a model that performs better for your users and then in order for the better model to lead to better users。

 you need to be sure that better predictions are actually making your product better。

 Another point that I want to make on these project archetypes is I would sort of characterize them as having different tradeoffs on this feasibility versus impact  two by two that we talked about earlier。

 So 2。0 projects since they're just taking something that you already know you can automate and automating it better。

 tend to be more feasible but since you already have an answer to the question that they're also answering。

 they also tend to be lower impact on the other extreme autonomous systems tend to be very difficult to build because the accuracy requirements in general are quite high。

 but the impact can be quite high as well。Because you're replacing something that literally doesn't exist and human in the loop systems tend to be somewhere in between where you can really like you can use this paradigm of machine learning products to build things that couldn't exist before but the impact is not quite as high because you still need people in the loop that are helping use their judgment to complement the machine learning model There's ways that you can move these types of projects on the feasibility impact matrix to make them more likely to succeed。

 So if you're working on the software 2。0 projects you can make these projects have potentially higher impacts by implementing a data loop that allows you to build continual improvement data flywheel that we talked about before and potentially allows you to use the data that you're collecting from users interacting with this system to automate more tasks in the future So for example。

 in the code completion IDde example that we gave before you can if you're building something like Githubcopilot then think about all the things that the data that you're collecting from that。

Be useful for building in the future。 you can make human in the loop systems more feasible through good product design and we'll talk a little bit more about this in a future lecture。

 but there's design paradigms in the product itself that can reduce the accuracy requirement for these types of systems and another way to make these projects more feasible is by adopting sort of a different mindset。

 which is let's just make the system good enough and ship it into the real world so we can start the process of seeing how how real users interact with it and using the feedback that we get from our humans in the loop to make the model better。

 And then lastly， autonomous systems can be made more feasible by adding guardrails or in some cases。

 adding humans in the loop。 and so this is you can think of this as the approach to autonomous vehicles where have safety drivers in the loop early on in the projects or where you introduce teleoperation so that a human can take control of the system if it looks like something is going wrong。

 I think another point that is really important here is despite all of this talk about。

What's feasible to do with ML， the complexity that ML introduces in your system。

I don't mean by any of this to say that you should do necessarily a huge amount of planning before you dive into using ML at all。

 just make sure that the project that you're working on is the right project and then just dive in and get started。

 and in particular， I think a failure mode that I'm seeing crop up more and more over the past couple of years that you should avoid is falling into the trap of tool fetishization。

 So one of the great things that's happened in ML over the past couple of years is the rise of this MLOs discipline。

And alongside of that has been proliferation of different tools that are available on the market to help with different parts of the ML process。

 And one thing that I've noticed that this is caused for a lot of folks is this sort of general feeling that you really need to have perfect tools before you get started You don't need perfect tools to get started and you also don't need a perfect model And in particular。

 just because Google or Uber is doing something like just because they have a feature store as part of their stack or they serve models in the particular way doesn't mean that you need to have that as well。

 And so a lot of what we'll try to do in this class is talk about what's the middle ground between doing things in the right way from a production perspective。

 but not introducing too much complexity early on into your project。

 So that's one of the reasons why FStL is a class about building ML powered products in a practical way and not an ML opps class that's focused on what is the state of the art in the best possible infrastructure that you can use and a talk and blog posts and associated set of things on this concept that I really like is this。

Mlops at reasonable scale push by some of the folks from Covio and the sort of central thesis of MLop reasonable scale is you're not Google。

 you probably have a finite compute budget， not entire cloudud you probably have a limited number of folks on your team you probably have not an infinite budget to spend on this and you probably have a limited amount of data as well。

 And so those differences between what you have and what Uber has or what Google has have implications for what the right stack is for the problems that you're solving。

 And so it's worth thinking about these cases separately。

 And so if you're interested in what one company did and recommends for an M stack that isn't designed to scale to becoming Uber scale that Id recommend checking out this talk。

 to summarize what we've covered so far， machine learning is an incredibly powerful technology but it does add a lot of complexity And so before you embark on a machine learning project you should make sure that you're thinking carefully about whether you。

Really need Ml to solve the problem that you're solving and whether the problem is actually worth solving at all。

 given the complexity that this adds。 And so let's avoid being M teams that have their projects get killed because we're working on things that don't really matter to the business that we're a part of All and the last topic I want to cover today is once you've sort made this decision to embark on an M project。

 What are the different steps that you're gonna to go through in order to actually execute on that project and this will also give you an outline for some of the other things you can expect from the class。

 So the running case study that we'll use here is a modified version of a problem that I worked on when I was at openaiI。

 which is pose estimation。 Our goal is to build a system that runs on a robot that takes the camera feed from that robot and uses it to estimate the position in 3D space and the orientation。

 the rotation of each of the objects in the scene so that we can use those for downstream tasks and in particular so we can use them to feed into a separate model which will be used to tell the robot how it actually can grasp the different objects in the scene machinech learning projects start like any other project。



![](img/f2625c9b79ef1bd654c56af86be78b3c_9.png)

![](img/f2625c9b79ef1bd654c56af86be78b3c_10.png)

A planningning and project setup phase。 And so what the types of activities we'd be doing in this phase。

When we're working on this pose estimation project are things like deciding to work on pose estimation at all。

 determining whether how much this is going to cost， what resources we need to allocate to it。

 considering the ethical implications and things like this a lot of what we've been talking about so far in this lecture。

 once we plan the project then we'll move into a data collection and labeling phase。

 And so for pose estimation， what this might look like is collecting the corpus of objects that we're gonna train our model on。

 setting up our sensors like our cameras to capture our information about those objects。

 actually capturing those objects and somehow figuring out how to annotate these images that we're capturing with ground truth。

 like the pose of of the objects in those images。 One point I want to make about the lifecycle of MLl projects is that this is not like a straightforward path。

 machine learning projects tend to be very iterative。

 and each of these phases can feed into any of the phases before。

 as you learn more about the problem that you're working on So for example。

 you might realize that actually it's way too hard for us to get data in order to solve this problem or it's really difficult for us to label。

Poose of these objects in 3D space。 But what we can do is it's actually much cheaper for us to annotate like per pixel segmentation。

 So can we reformulate the problem in a way that allows us to。

 to use what we've learned about data collection and labeling to plan a better project。

 Once you have some data to work on， then you enter the sort of training and debugging phase。

 And so what we might do here is we might implement a baseline for our model。

 not using like a complex neural network， but just using some openC functions。

And then once we have that working， we might find a state of the art model and reproduce it。

 debuggger implementation and iterate on our model。

 runs some hyperparameter sweeps until it performs well on our task。

 This can feed back into the data collection and labeling phase because we might realize that we actually need more data in order to solve this problem or we might also realize that there's something flawed in the process that we've been using to label the data that we're using data labeling process might need to be revisited but we can also loop all the way back to the project planning phase because we might realize that actually this task is a lot harder than we thought or the requirements that we specified at the planning phase trade off with each other so we need to revisit which are most important So for example maybe we thought that we had an accuracy requirement of estimating the pose of these objects to one1th of one centimeter and we also had a latency requirement for inference on our models of 1100th of a second to run on robotic hardware and we might realize that hey we can get this really really tight accuracy requirement or we can have really fast inference。

's very difficult to do both。 So is it possible to relax one of those assumptions Once you've trained a model that works pretty well offline for your task。

 then your goal is going to be to deploy that model。

 test it in the real world and then use that information to figure out where to go next for the purpose of this project that might look like piloting the grasping system in the lab so before we roll it out to actual users can we test it in a realistic scenario and we might also do things like writing tests to prevent regressions and evaluate for bias in the model and then eventually rolling this out into production and monitoring it and continually improving it from there and so we can feedback here into the training and debugging stage because oftentimes what we'll find is that the model that works really well for our offline data set once it gets into the real world。

 it doesn't actually work as well as we thought whether that's because the accuracy requirement that we had for the model was wrong。

 like we actually needed it to be more accurate than we thought or maybe the metric that we're looking at the accuracy is not actually the metric that really matters for success at the downstream task that。

tryrying to solve that could cause us to revisit the training phase。

 We also could loop back to the data collection and labeling phase because common problem that we might find in the real world is that。

There's some mismatch between the training data that we collected and the data that we actually saw when we went out and tested this。

 We could use what we learned from that to go collect more data or mind for hard cases。

 like mind for the failure cases that we found in production。 And then finally。

 as I alluded to before we could all the way back to the project planning phase because we realize that the metric that we picked doesn't really drive the downstream behavior that we desired。

 just because the grasp model is accurate doesn't mean that the robot will actually be able to successfully grasp the object So we might need to use a different metric to really solve this task。

 or we might realize that the performance in the real world isn't that great。

 And so we maybe need to add additional requirements to our model as well。

 maybe it just needs to be faster in order to run on a real robot。

 So these are kind of like what I would think of as the activities that you do in any particular machine learning project that you undertake。

 but there's also some sort of crossproject things that you need in order to be successful。

 which we'll talk about in the class as well。 you need to be able to work on these problems together as a team。

 and you need to have the right infrastructure。oling to make these processes more repeatable。

 And these are topics that we'll cover as well。 So this is like a broad conceptual outline of the different topics that we'll talk about in this class。

 And so to wrap up for today， what we covered is machine learning is a complex technology。

 And so you should use it because you need it or because you think it'll generate a lot of value。

 but it's not a cure all。 It doesn't solve every problem。

 It won't automate every single thing that you wanted to automate。

 So let's pick projects that are gonna be valuable。 But in spite of this。

 you don't need a perfect setup to get started。 And let's spend the rest of this course walking through the project lifecycle and learning about each of these stages and how we can how we can use them to build great ML powered products。



![](img/f2625c9b79ef1bd654c56af86be78b3c_12.png)