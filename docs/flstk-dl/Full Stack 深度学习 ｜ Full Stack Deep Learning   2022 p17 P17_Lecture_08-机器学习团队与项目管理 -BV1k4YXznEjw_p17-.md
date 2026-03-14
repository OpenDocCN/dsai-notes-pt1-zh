# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p17 P17_Lecture_08-机器学习团队与项目管理 -BV1k4YXznEjw_p17-

![](img/dc3925d6d72462a5cce317768c6302a6_0.png)

Hey， everybody， welcome back。 This week， we're going to talk about something a little bit different than we do most weeks。

 Most weeks we talk about specific technical aspects of building machine learning powered products。

 but this week we're going to focus on some of the organizational things that you need to do in order to work together on ML powered products is part of an interdisciplinary team So the reality of building ML powered products is that building any product well is really difficult。

 you have to figure out how to hire great people。 you need to be able to manage those people and get the best out of them。

 you need to make sure that your team is all working together towards a shared goal you need to make good longterm technical choices。

 manage technical debt over time you need to make sure that your managing expectations not just of your own team。

 but also of leadership of your organization and you need to be able to make sure that you're working well within the confines of the requirements of the rest of the org that you're understanding those requirements well and communicating back your progress to the rest。



![](img/dc3925d6d72462a5cce317768c6302a6_2.png)

The organization against those requirements。But machine learning adds even more additional complexity to this。

 Maine learning talent tends to be very scarce and expensive to attract。

Machine learning teams are not just a single role， but today they tend to be pretty interdisciplinary。

 which makes managing them an even bigger challenge。

 Machine learning projects often have unclear timeline and there's a high degree of uncertainty to those timeline。

 machine learning itself is moving super fast and machine learning as we've covered before。

 you can think of as like the high interest credit card of technical debt。

 So keeping up with making good long-ter decisions and not encourage too much technical debt is especially difficult in M Unlike traditional software。

 Ml is so new that in most organizations， leadership tends not to be that welled in it。

 They might not understand some of the core differences between Ml and other technology that you're working with。

 Mach learning products tend to fail in ways that are really hard for lay people to understand。

 And so that makes it very difficult to help the rest of the stakeholders in your organization understand what they can really expect from the technology that you're building and what is realistic for us to achieve So throughout the。

of this lecture we're going to kind of touch on some of these themes and cover different aspects of this problem of working together to build MLpar products as an organization So here are the pieces that're going to cover。

 we're going to talk about different roles that are involved in building ML products we're going to talk about some of the unique aspects involved in hiring ML talent We're going to talk about organization of teams and how the ML team tends to fit into the rest of the org and some of the pros and cons of different ways of setting that up。

We'll talk about managing ML teams and M product management。 And then lastly。

 we'll talk about some of the design considerations for how to design a product that is well suited to having a good ML model that backsit。

 So let's dive in and talk about roles the most common ML roles that you might hear of are things like Ml product manager M ops or M platform or Ml infofr teams。

 machine learning engineers， machine learning researchers or M scientists。

 data scientists So there's a bunch of different roles here and one kind of obvious question is what's the difference between all these different things。

 So let's break down the job function that each of these roles plays within the context of building the ML product。

Starting with the ML product manager， their goal is to work with the ML team， the business team。

 the users， and any other stakeholders to prioritize projects and make sure that they're being executed well to meet the requirements of the rest of the organization So what they produce is things like design docs。

 wireframes， work plans and they're using tools like Jra like notion to help sort of organize the work of the rest of the team the MLO or ML platform team are focused on building the infrastructure needed in order to make models easier to deploy more scalable or generally reduce the workload of individual contributors who are working on different ML models。

 the output of what they build is some infrastructure。

 some shared tools that can be used across the ML teams in your company。

And they're working with tools like AWS， like Kafka or other data infrastructure tools and potentially working with M infrastructure vendors as well to sort of bring best and breed from traditional data and software tools。

 And this new category of M vendors that are providing like M ops tools together to create this sort of best solution for the specific problems The your company is trying to solve。

 Then we have the M engineer。 The Ml engineer Ger is kind of a catch all role。 and。

The way that I like to think of their responsibilities is they're the person who is responsible for training and deploying and maintaining the prediction model that powers the MLPd product。

 They're not just the person who is solely training the model and then handing it off to someone else。

 but they're also responsible for deploying it and then maintaining it once it's in production。

 And so they need to know technologies like Tensorflow for training models。

 but also like Docker for packaging models and making sure that they run on production infrastructure。

 The next role is the ML researcher So this is a role that exists in some organizations that responsibility stops after the models has been trained。

 And so oftentimes these models are either handed off to some other team to productionize or these folks are focused on building models that are not yet production critical or forward looking maybe they're prototyping some use cases that might be useful down the line for the organization and their work product is a trained model and oftentimes it's a report or a code repo that describes what this model does how to use it and how to reproduce。

Their results So they're working with ML trading tools and also prototype having tools like Jupyter notebooks to produce a version of the model that just needs to work once to sort of show that the thing that they're trying to do is possible。

 and then lastly we get the data scientist data scientist is kind of a catch all term for potentially any of the things above in some organizations data science is quite distinct from what we've been thinking of as a machine learning role in this class and these are folks in some organizations that are responsible for answering business questions using analytics So in some organizations a data scientist is the same as an ML researcher or an ML engineer Another organizations data science is a distinct function that is responsible for answering business questions using data the M work is the responsible of an ML team So the next thing that we'll talk about is what are the different skills that you actually need to be successful in these roles we're going to plot this on a two by two on the X axis is the amount of skill that you need in machine learning like how much M do you really need to know on the y axis is the software。

ering skill needed and then the size of the bubble is a requirement on communication or technical writing。

 how good do you have to be at communicating your ideas to other people So starting with Mops or M platform teams this is really primarily a software engineering role and oftentimes where these folks will come into the organization is through their traditional software engineering or data engineering hiring pipeline or even moving over from a data engineering role in another part of the organization Another common pattern for how organizations find Mops or platform engineers is they source them from Ms at their organization。

 it's oftentimes like an M engineer who used to just work on one model and then got frustrated by the lack of tooling so decided to move into more of a platform role the M engineer since this is someone who is required to understand the models deeply and also be able to productionize them this tends to be a rare mix of M skills and software engineering skills So there's sort of two paths that I typically see for folks becoming M engineers oftentimes these are software engineers who have a pretty significant amount of self- teachingeach。

Or on the other hand， maybe they are someone who's trained in machine learning traditionally like they have a science or engineering PhD。

 but then they switch careers into software engineering after grad school or after undergrad and then later decided to fuse those two skill sets M researchers。

 these are your M experts。 So this is kind of the only role in this list。

 but I would say it's pretty typical still to see graduate degree or another path to these roles are these industrial fellowship programs like Google brain residency that are explicitly designed to train people without a PhD in this distinct skill of research。

 Since data science is kind of like a catch all term for a bunch of different roles in different organizations。

 it also admits a variety of different backgrounds and oftentimes these are undergrads who went to a data sciencespec program or they're science PhDs who are making the transition into industry。

 And then lastly， MPs oftentimes these folks come from a traditional product management background。

 but they do need to have a deep under。Sting of the specifics of the ML development process and that can come from having。

 you know， work closely with M teams for a long time。

 having just a really strong independent interest in M。

Or oftentimes what I see is folks who are former data scientists or ML engineers who make the switch into PM can be really effective at PMing MLl projects because they have a deep understanding in the technology。

 one other distinction that I think is worth covering when talking about the variety of different roles and ML organizations is the distinction between a task M engineer and a platform M engineer。

 This is a distinction that was coined by Sya Shankar in blog posts that's linked below and the distinction is that some ML engineers are really responsible for like one ML pipeline or maybe a handful of ML pipelines that they're assigned to。

 And so they're the ones that are day in and day out responsible for making sure this model is healthy。

 making sure that it's being updated frequently and that any failures are sort of being accounted for。

 these folks are often like very overburden， this can be a very sort of expansive role because they have to be training models and deploying them and understanding where they break Since ML engineers are often spread so thin Some ML engineers end up taking on a role。

That looks more like a ML platform team or ML ops team where they work across teams to help ML engineers automate tedious parts of their jobs。

 And so we in our parlance， this is called an ML platform engineer or ML ops engineer。

 but you'll also hear this refer to as an ML engineer or a platform M engineer So we've talked a little bit about what are some of the different roles in the process of building ML power products。

 Now let's talk about hiring So how to think about hiring M specialists and if you are an MLl specialist looking for a job how to think about making yourself more attractive as a job candidate So a few different things that we'll cover here。

 the first is the AI talent gap， which is sort of the reality of M hiring these days。

 and we'll talk about how to source for M engineers if you're hiring folks we'll talk about interviewing and then lastly。

 we'll talk about finding a job four years ago when we started teaching full stack deep learning。

 the AI talent gap was the main story in many cases for what teams found difficult about building with ML There was just so。

 so few people that understood this technology。That the biggest thing blocking a lot of organizations was just they couldn't find people who are good at machine learning Four years later。

 the AI talent gap persists and there's still know news stories every few months that are being written about how difficult it is for companies to find M talent but my observation day to day in the field is that it tends to be less of a blocker than it used to be because we've had four years of folks sorting careers into M and four years of software engineers emerging from undergrad with at least a couple of M classes in many cases under their belts So there's more and more people now that are capable of doing M but there's still a gap and in particular that gap tends to be in folks that understand more than just the underlying technology but also have experience in seeing how seeing how it fails and how to make it successful when it's deployed So that's the reality of how difficult it is to hire machine learning folks today especially those who have production experience So if you are hiring M folks how should you think about finding people if you're hiring。

Ml product managers or M platform or MOs engineers the main skill set that you need to look for is still the sort of core underlying skill set for those roles。

 So product management or data engineering or platform engineering in general but it is critical to find folks who have experience at least interacting with teams that are building production M systems because I think one sort of failure mode that I've seen relatively frequently especially for M platform teams is if you just bring on folks with pure software engineering background a lot of times it's difficult for them to understand the user requirements well enough in order to engineer things that actually solve the users problems Use here being the task Ms who are the ones who are going to be using the infrastructure data will focus for the rest of the section mostly on these two roles engineer and M scientists So there's a right and a wrong way to hire M engineers and the way oftentimes looks maybe something like this So you see a job description for the unicorn and machine learning engineer the duties for this person are they need to keep up with seed of the art they need to implement。

New models from scratch as they come out they need a deep understanding of the underlying mathematics and ability to invent new models for new task as it arises。

 they need to also be able to build tooling and infrastructure for the ML team because ML teams need tooling to do their jobs they need to be able to build data pipelines as well because without data ML is nothing they need to deploy these models and monitor them in production because without deploying models youre not actually solving a problem So in order to fulfill all these duties。

 you need these requirements as this unicorn and MLE role you of course need a PhD you need at least four years of TensorFlow experience four years as a software engineer。

 you need to have publications and Nps or other top ML conferences。

Experience building largescale distributed systems。 And so when you add all of this up。

 hopefully's becoming clear why this is the wrong way to hire ML engineers。

 There's just not really very many people that fit this description today。

 if any and so the implication is the right way to hire M engineers is to be very。

 very specific about what you actually need from these folks。 and in most cases。

 the right answer is to primarily hire for software engineering skills， not M skills。

 you do need folks that have at least a background in M and a desire to learn M。

 and you can teach people how to do M if they have a strong interest in it。

 they know the basics and they're really strong in the software engineering side。

 Another approach instead of hiring for software engineering skills and training people on the M side is to go more junior。

 most undergrads in computer science these days graduate with M experience。

 and so these are folks that have traditional computer science training and some theoretical M understanding。

 So they have sort of the seeds of being good at both M and software engineering。

 but maybe not a lot of experience in either。And then the third way that you can do this more effectively is to be more specific about what you really really need for not the ML engineering function in general。

 but for this particular role so not every ML engineer needs to be a Devopps expert to be successful not every ML engineer needs to be able to implement new papers from scratch to be successful either for many of the ML engineers that you're hiring what they really need to do is something along the lines of taking a model that is pretty established as something that works well pulling it off the shelf or training it using a pretty robust library and then being able to deploy that model into production so focus on hiring people to have those skills。

 not these aspirational skills that you don't actually really need for your company Next let's talk about a couple things I've found to be important for hiring M researchers The first is a lot of folks when they're hiring ML researchers they look first at the number of publications they have in top conferences I think it's really critical to focus entirely on the quality。

A publication is not the quantity and this unfortunately requires a little bit of judgment about what high quality research looks like。

 but hopefully there's someone on your team that can provide bad judgment。

 it's more interesting to me to find machine learning researchers who have one or two publications that you think are really creative or very applicable to the field that you're working in or have really really strong promising results than to find someone who's published 20 papers but each of them are just sort of an incremental improvement to the state of the art if you're working in the context of a company where you're trying to build a product and you're hiring researchers then I think another really important thing to filter for is looking for researchers who have an eye for working on problems that really matter。

 a lot of researchers maybe through no fault of their own because of the incentives in academia focus on problems that are trendy if everyone else is publishing about reinforcement learning then they'll publish about reinforcement learning if everyone else is publishing about generative models then they'll making incremental improvements to generative models to get them a publication but what。

Re want to look for， I think， is folks that have an independent sense of what problems are important to work on because in the context of your company。

 no one's gonna be telling these folks like， hey， this is what everyone's gonna be publishing about this year。

 Oftentimes experience outside of academia can be a good proxy for this。

 but it's not really necessary。 It's just sort of one signals to look at。

 If you already have a research team established， then it's worth considering hiring talented people from adjacent fields hiring from physics or statistics or math。

 at openaiI， they did this to really strong effects。

 they would look for sort of folks that were really technically talented but didn't have a lot of M expertise and they would train them an M。

 This works a lot better if you do have experienced research who can provide mentorship and guidance for folks。

 I probably wouldn't hire like a first researcher that doesn't have M experience。

 And then it's also worth remembering that especially these days you really don't need a PhD to do M research。

 Many undergrads have a lot of experience doing M research and graduates of some of these industrial fellowship programs like Google。

or Facebooks or open AIs have learned the basics of how to do research regardless of whether they have a PhD。

 So that's how to think about evaluating candidates for ML engineering or ML research roles。

 and the next thing I want to talk about is how to actually find those candidates So your standard sources like LinkedIn or recruiters or onc recruiting all work。

 But another thing that can be really effective if you want to go deeper is every time there's a new dump of papers on archive or every year at Nps and other top conferences just keep an eye on what you think are the most exciting papers and flag mostly the first authors of those papers because those are the ones that tend to be doing most of the work and are generally more recruitable because they tend to be more junior in their careers  beyond looking at papers you can also do is something similar for good reimplementations of papers that like So if you are looking at some hot new paper and a week later there's a reimplementation of that paper that has high quality code and hits the main results than chances are whoever wrote that implementation。

I probably pretty good and so they could be worth recruiting you can do a lot of this in person now that ML research conferences are back in person or you can just reach out to folks that you are interested in talking to over the internet Since there's a talent shortage in M it's not enough just to know how to find good M candidates and evaluate them。

 you also need to know how to think about attracting them to your company I want to talk a little bit about from what I've seen what a lot of M practitioners are interested in the roles they take and then talk about ways that you can make your company stand out along those axes So one thing a lot of ML practitioners want is to work with cutting edge tools and techniques to be working with latest state of the art research Another thing is to build knowledge in an exciting field to like a more exciting branch of M or application of M working with excellent people probably pretty consistent across many technical domains but certainly true in M working on interesting datas this is kind of one unique thing in M since the work that you can do is constrain in many cases to the datas you have access to being able to offer unique dataset sets can be pretty powerful。

Probably again true in general， but I've noticed for a lot of ML folks in particular。

 it's important for them to feel like they're doing work that really matters。

 So how do you stand out on these axes， you can work on researchoriented projects。

 even if the sort of mandate of your team is primarily to help your company doing some research work that you can public and that you can point to as being indicative of working on the cutting edge。

 open source libraries， things like that can really help attract top candidates you want to emphasize the ability of folks to sort of build skills and knowledge in an exciting field。

 you can build a team culture around learning， so you can host reading groups in your company you can organize learning days。

 which is something that we did at open AI where we dedicate back then a day per week just to be focused on learning new things but you can do it less frequently than that professional development budgets。

 conference budgets， things like this that you can emphasize。

 and this is probably especially valuable if your strategies to hire more junior folks or more software engineering oriented folks and train them up in machine learning emphasize how much they'll be able to learn about ML。

One sort of hack to being able to hire good ML people is to have other good ML people on the team This is maybe easier said than done。

 but one really high profilefi hire can help attract many。

 many other people in the field and if you don't have the luxury of having someone high profilefi on your team you can help your existing team become more high profile by helping them publish blogs and papers so that other people start to know how talented your team actually is when you're attracting M candidates。

 you can focus on sort of emphasizing the uniqueness of your data and recruiting materials so if you have the best data for a particular subset of the legal field or the medical field emphasize how interesting that is to work with how much data you have and how unique it is that you have it and then lastly just like any other type of recruiting selling the mission of the company and the potential for M to have an impact on that mission can be really effective Next let's talk about M interviews What I would recommend testing for if you are on the interviewer side of an M interview is to try to hire for strengths and meet a minimum bar for everything else。

Hep you avoid falling into the trap of looking for unicorn and Ms So some things that you can test are you want to validate your hypotheses of candidate strengths So if it's a researcher。

 you want to make sure that they can think creatively about new ML problems and one way you can do this is to probe how thoughtful they were about previous projects if they're engineers if they're MLs then you want to make sure that theyre great general software engineers since that's sort of the core skill set and M engineering and then you want to make sure they meet a minimum bar on weaker areas so for researchers I would advocate for only hiring researchers in industry context who have at least the very basics in place about software engineering knowledge and the ability to write decent code if not really high quality production ready code because in context of working with a team other people are going to need to use their code and it's not something that everyone learns how to do when they're in grad school for for software engineers you want to make sure that they at least meet a minimum bar on machine learning knowledge and this is really testing for like are they passionate about this field that they have put in the requisite effort to learn the basics of M that's a。

Indiic that they're gonna learn M quickly on the job if you're hiring them mostly for their software engineering skills So what do M interviews actually consist of So this is today much less welldefined than your software engineering interviews some common types of assessments that I've seen are your normal sort of background and culture fit interviews whiteboard coding interviews similar to you see in software engineering pair coding like in software engineering but some more M specific ones include pair debugging where you and an interviewer will sit down and run some Ml code and try to find hey where's the bug in this code oftentimes this is Ml specific code and the goal is to test for how well is this person able to find bugs and M code since bugs tend to be where we spend most of our time in machine learning math puzzles are often common especially involving things like linear algebra take home projects other types of assessments include applied M questions So typically this will have the flavor of hey here's a problem that we're trying to solve with M let's talk through the sort of high levell pieces of how we'd solve it what type of algorithm we'd use what type of system。

We need to build to support it Another common assessment is probing the past projects that you've listed on your resume or listed as part of the interview process asking you about things you tried will work what didn't work and trying to assess what role you played in that project and how thoroughly you thought through the different alternative paths that you could have considered and then lastly Ml theory questions are also pretty common in these interview type assessments that's sort of a universe of things that you might consider interviewing for if you're trying to hire M folks or that you might expect to find on an M interview if you are on the other side and trying to interview for one of these jobs and the last thing I'll say on interviews is theres a great book from chip when the introduction to machine learning interviews book which is available for free online which is especially useful I think if you're preparing to interview for machine learning roles speaking of which what else should you be doing if your goal is to find new job and machine learning the first question I typically hear is like where should I even look for M jobs your standard sources like LinkedIn and recruiters all work M research conference。

Can also be a fantastic place。 Just go up and talk to the folks that are standing around the booths at those conferences。

 they tend to be looking for candidates and you can also just apply directly。

 And this is sort of something that people tell you not to do for most roles。

 but remember there's a talent gap in machine learning So this can actually be more effective than you might think when you're applying what's the best way to think about how to stand out for these roles。

 So I think like sort of a baseline thing is for many companies。

 they really want to see that you're expressing some sort of interest in you've been attending conferences you've been taking online courses you've been doing something to sort get your foot in the door for getting into the field better than that is being able to demonstrate that you have some software engineering skills again for many organizations hiring for software engineering is in many ways more important than hiring for M skills。

 if you can show that you have a broad knowledge of M。

 So writing blog posts that synthesized a particular research area or articulating a particular algorithm in a way that that is new or creative or compelling can be a great way to stand out。

 but even better than that is demonstrating an ability to。Shi M projects and the best way to do this。

 I think if you are not working in M fulltime right now is through side projects。

 these can be ideas of whatever you want to work on。

 they can be paper reimpleations or they can be your project for this course and then probably if you really want to stand out maybe the most impressive thing that you can do is to prove that you can think creatively an M think beyond just reproducing things that other people have done but be able to win caagle competitions or published paperss。

 And so this is definitely not necessary to get a job in M。

 but this will sort of put your resume at top of the stack。

 So we've talked about some of the different roles that are involved in building Ml products and how to think about hiring for those roles or being hired for those roles。

 the next thing that we're going to talk about is how machine learning teams fit into the context of the rest of the organization Since we're still in the relatively early days of adopting this technology。

 there's no real consensus yet in terms of the best way to structure an M team。

 But what we'll cover today is taxonomy of some of the best practices for different maturity。

Levels of organizations and how they think about structuring their m teams And so we'll think about this as scaling a mountain from least mature M team to most mature So the bottom of the mountain is the nascent or ad hoc archetype So what this looks like is your company has just started thinking about No ones really doing it yet or maybe there's a little of it being done on an ad hoc basis by the analytics team or one of the product teams and most smaller medium businesses are at most in this category but some of the less technology for larger organizations still fall in this category as well So the great thing about being at this stage is that there's a ton of low hanging fruit often for m to come in and help solve but the disadvantage if you're going go in and work in an organization at this stage is that there's often little support available for ml projects。

 you probably won't have any infrastructure that you can rely on and it could be difficult to hire and retain good talent plus leadership in the company may not really be bought into how useful M could be so that's some of the things to think about if you're gonna go take a role in。

One of these organizations once the company has decided， hey， this ML thing is something exciting。

 something that we should invest in， typically they'll move up to an ML RD stage So what this looks like is they'll have a specific team or subset of their RD organization that's focused on machine learning they'll typically hire researchers or PhDs and these folks will be focused on building prototypes internally or potentially doing external facing research so some of the larger oil and gas companies manufacturing companies telecom companies we in the stage even just a few years ago although they've in many cases moved on from it now if you're gonna go work in one of these organizations。

 one of the big advantages is you can get away with being less experienced on the research side and since the M team isn't really going be on the hook today for any sort of meaningful business outcomes another big advantage is that these teams can work on longterm business priorities and they can focus on trying to get to what would be really big wins for the organization but the disadvantage is to be aware。

If you're thinking about joining a team at this stage or building a team at this stage is that oftentimes since the ML team is sort of siloed off into an R&D part of the organization or a separate team from the different products initiatives。

 it can be difficult for them to get the data that they need to solve the problems that they need to solve It's just not a priority in many cases for other parts of the business to give them the data and then probably the biggest disadvantage of this stage is that it doesn't usually work。

 it doesn't usually translate to business value for the organization and so oftentimes ML teams kind get stuck at this stage where they don't invest very much in M and M is of siloed and so they don't see strong results and they can't really justify doubling down the next evolution of ML organizations oftentimes is embedding machine learning directly into business and product teams So what this looks like is you'll have some product teams within the organization that have a handful of M people side by side with their software or analytics teams and these ML teams will report up into the sort of engineering or tech organizations。

Instead of being in their own sort of reporting armA lot of tech companies when they start adopting M sort pretty quickly get to this category because they're pretty agile software organizations and pretty tech forward organizations anyway and a lot of the financial services companies tend towards this model as well the big of overwhelming advantage of this organizational model is that when these M teams ship stuff successfully it almost always is able to translate pretty directly to business value since people that are doing M sit side by side with folks that are building the product or building the feature that the M is going be part of and this gives them a really tight feedback cycle between new ideas that they have for how to make the better how to make the product better with M into actual results as part of the products the disadvantages of building M this way are oftentimes it can be hard to hire and develop really great people because great M people often want to work with other great people it can also be difficult to get these M folks access to the resources that they need to。

Really successful So that's the infrastructure they need the data they need or the compute they need because they don't have sort of a central team that reports high up in the organization to ask for help。

 and one other disadvantage of this model is that oftentimes this is where you see conflicts between the way that M projects are run a sort of iterative process that is high risk and the way that the software teams that these M folks are part of our organized sometimes you'll see conflict between folks getting frustrated with the M folks on their team for not shipping quickly or not being able to sort commit to a timeline that they promised the next M organization Archetype will cover is independent machine learning function What this looks like is you'll have a machine learning division of the company that reports up to senior leadership so they report to the CEO or the CTO or something along those lines This is what distinguishes it from the M&D Archetype where the M team is often reporting to someone more junior and the organization often a corner as sort of a smaller bet This is the organization making a big bet to investing in machine learning so often。

This is also the archetype where you'll start to see MLPMs or platform MLPMs that work with researchers and ML engineers and some of these other roles in order to deliver like a crossfunction product the big advantage of this model is access to resources so since you have a centralized ML team you can often hire really really talented people and build a talent density in the organization and you can also train people more easily since you have more ML people sitting in a room together or in a zoom room together in some cases Since you report to senior leadership you can also often like marshahal and more resources in terms of data from the rest of the organization or budget for compute than you can in other archetypes and it makes it a lot easier when you have a centralized organization to invest in things like tooling and infrastructure and culture and best practices around developing ML in your organization the big disadvantage of this model is that it leads to handoffs and that can add friction to the process that you as an ML team need to run in order to actually get your models into production and the last ML organization archetype the。

The goal if you're trying to build M the right way at your organization is to be an M first organization So what this looks like is you have buy in up and down the organization that M is something that you as a company want to invest in you have an M division that works on the most challenging longterm projects and invest in sort of centralized data and centralized infrastructure but you also have expertise in M in every line of business that focuses on quick wins and working with the central M division to sort of translate the ideas they have the implementations they make into actual outcomes for the products that companies building so you'll see this in the biggest tech companies like the Googles and Facebooks of the world as well as startups that were founded with M as a core guiding principle for how they want to build the product and these days more and more you're starting to see other tech companies who began investing in M4 or five years ago to start to become closer to this archetype there's mostly advantages to this model you have great access to data it's relatively easy to recruit and most importantly it's probably easiest in this archetype out of all of them to get。

valuealue out of M because the products teams that you're working with understand machine learning and really the only disadvantage of this model is that it's difficult and expensive and it takes a long time for organizations that weren't born with this mindset to adopt it because you have to recruit a lot of really good M people and you need to culturally embed M thinking into your organization。

The next thing that we'll talk about is some of the design choices you need to make if you're building an ML team we'll talk about how those depend on the archetype of the organization that you fit into The first question is software engineering versus research So to what extent is the M team responsible for building software versus just training models The second question is data ownership So is the M team also responsible for creating publishing data or do they just consume that from other teams and the last thing is model ownership the M team are they the ones that are going to productionize models or is that the responsibility of some other team in the MLR&D archetype typically you'll prioritize research over software engineering skills and the ML team won't really have any ownership over the data or oftentimes even the skill sets to build data pipelines themselves and similarly they won't be responsible for deploying models either and in particular models will rarely make it into production so that won't really be a huge issue embedded M teams typically you they'll prioritize software engineering skills over research skills and all researchers if they even have researchers will need to have strong software engineering。

Skills because everyone's expected to deploy M team still generally doesn't own data because they are working with data engineers from the rest of the organizations to build data pipelines。

 but since the expectation in these types of organizations is that everyone deploys typically ML engineers will own maintenance of the models that they deploy in the M function archetype typically the requirement will be that you'll need to have a team that has a strong mix of software engineering research and data skills So the team size here starts to become larger a minimum might be something like one data engineer one ML engineer potentially a platform engineer or DevOs engineer and potentially a PM but these teams are often working with a bunch of other functions so they can in many cases get much larger than that and in many cases in these organizations you'll have both software engineers and researchers working closely together within the context of a single team usually at this stage M teams will start to have a voice in data governances discussions and they'll probably also have some strong internal data engineering functions as well and then since the M team is centralized at this。

They'll hand off models to a user， but in many cases。

 they'll still be responsible for maintaining them。

 although that line is blurry in a lot of organizations that run this model Finally in M first organizations。

 there's no real standardization around how teams are research oriented or not but research teams do tend to work pretty close to with software engineering teams to get things done In some cases the M team is actually the one that owns companywide data infrastructure because M is such a central bet for the company that it makes sense for the M team to make some of the of main decisions about how data will be organized and finally if the M team is the one that actually built the model。

 they'll typically hand it off to a user who since they have the basic M skills and knowledge to do this they'll actually be the one to maintain the model and here's all this on one slide if you want to look at it all。

 so we've talked about machine learning teams and organizations and how these come together and the next thing we're going to talk about is team management and product management for machine learning the first thing to know about product management and team management for M is that it tends to be really challenging's a few。

The first is that it's hard to tell in advance how easy or hard something is going to be。

 So this is an example from a blog post by Lucas Bw where they ran a Cago competition and in the first week of that Cago competition。

 they saw a huge increase in the accuracy of the best performing model They went from 35 to 70% accuracy within one week and they were thinking this is great like we're going to hit 95% accuracy and this contest is going to be huge success。

 but then if you zoom out and look at the entire course of the project over three months it turns out that most of that accuracy gain came in the first week and the improvements thereafter were just marginal and that's not because of a lack of effort the number of participating teams was still growing really rapidly over the course of that time So the upshot is it's really hard to tell in advance how easy or hard something is in and looking at signals like how quickly are we able to make progress on this project can be very misleading a related challenge is that progress on M projects tends to be very nonlinear So it's very common for projects to stall for weeks or longer。

Ideas that you're trying just don't work or because you hit some sort of unforeseen snag with not having the right data or something like that that causes you to really get stuck And on top of that in the earliest stages of doing the project it can be very difficult to plan and to tell how long the project is going to take because it's unclear what approach will actually work for training a model that's good enough to solve the problem and the upshot of all this is that estimating the timeline for a project when you're in the project planning phase can be very difficult In other words。

 production MLl is still somewhere between research and engineering another challenge for managing ML teams is that there's cultural gaps that exist between research and engineering organizations these folks tend to come from different backgrounds they have different training they have different values goals and norms For example。

 oftentimes stereotypically researchers care about novelty and about how exciting the approaches that they took to solve problem whereas again stereotypically oftentime software engineers care about did we make the thing work and in more toxic cultures these two sides often can clash。

And even if they don't clash directly， they might not really value each other as much as they should because both sides are often necessary to build the thing that you want to build to make matters worse when you're managing a team as part of an organization。

 you're not just responsible for making sure the team does what they're supposed to do but you also have to manage up to help leadership understand your progress and what the outlook is for the thing that you're building since M is such a new technology。

 many leaders and organizations， even in good technology organizations don't really understand it So next I want to talk about some of the ways that you can manage machine learning projects better。

 And the first approach that I'll talk about is doing project planning probabilistically So oftentimes when we think about project planning for software projects we think about it as sort of a waterfall where you have a set of tasks and you have a set of time estimates for those tasks and a set of dependencies for those tasks and you can plan these out one after another So if task G depends on task D and F then task G will happen once those are done if task D depends on。

C， which depends on task A you'll start task D after A and C are done etc。 But in machine learning。

 this can lead to frustration and badly estimated timelines because each of these projects has a higher chance of failure than it does in a tiable software project What we ended up doing at Open AI was doing project planning probabilistically So rather than assuming that like a particular task is going to take a certain amount of time instead we assign probabilities to the likelihood of completion of each of these tasks and potentially pursue alternate tasks that allow us to unlock the same dependency in parallel。

 So in this example maybe task B and task C are both alternate approaches to unlocking task D。

 So we might do both them at save time。 And so if we realize all of a sudden that task C is not going work and task B is taking longer than we expected then we can adjust the timeline appropriately and then we can start planning the next wave of tasks once we know how we're going to solve the prerequisite tasks that we need a corollary of doing machine learning project planning probabilistically is that。

Should't have any path critical projects that are fundamentally research research projects have a very。

 very high rate of failure rather than just saying like this is how we're going to solve this problem instead you should be willing to try a variety of approaches to solve that problem that doesn't necessarily mean that you need to do them all in parallel but many good machine learning organizations do So one way to think about this is if you know that you need to build like a model that's never been built in your organization before you can have like a friendly competition of ideas if you have a culture that's built around working together as a team to get to the right answer and not just rewarding the one person who solves the problem correctly Another corollary to this idea that that many machine learning ideas can and will fail is that when you're doing performance management it's important not to get hung up on just who is the person whose ideas worked in the long term it's important for people to do things that work like over the course of many。

 many months or years if nothing that you try works then that's maybe an indication that you're not trying the right things you're not execute effectively but on any given project。

Like on a timeline of weeks or a quarter then the success measure that you should be looking at is how well you executed on the project。

 not whether the project happened to be one of the ones that worked One failure mode that I've seen in organizations that hire both researchers and engineers is implicitly valuing one side more than the other So thinking engineering is more important than research which can lead to things getting stuck on the M side because the ML side is not getting the resources or attention that they deserve or thinking that research is more important than engineering。

 which can lead to creating ML innovations that are not actually useful So oftentimes the way around this is to have engineers and researchers work very closely together in fact sometimes uncomfortably close together like working together on the same codebase for the same project and understanding that these folks bring different skill sets to the table。

 Another key to success I've seen is trying to get quick win so rather than trying to build a perfect model and then deploy it trying to ship something quickly to demonstrate that this thing can work and then iterate on it over time and then the last thing that you need to do if you're in。

Of being the product manager or the engineering manager for an ML team is to put more emphasis than you might think that you need on educating the rest of your organization on how M works diving into that a bit more if your organization is relatively new to adopting M I'd be willing to bet that a lot of people in the organization don't understand one or more of these things for us as like M practitioners it can be really natural to think about where M can and can't be used but for a lot of technologists or business leaders that are new to M that uses of M that are practical can be kind of counterintuitive and so they might have ideas for M projects that are feasible and they might miss ideas for M projects that are pretty easy that don't fit their mental model of what M can use Another common point of friction and dealing with the rest of the organization is convincing the rest of the organization that the M that you built actually works Business leaders and folks from product teams typically the same metrics that convince us as M practitioners that this model is useful won't convince them like just looking at an F1 score。

or an accuracy score doesn't really tell them what they need to know about whether this model is really solving the task that it needs to solve for the business outcome that they're aiming for And one particular way that this presents itself pretty frequently is in business leaders or in other stakeholders not really sort of wrapping their heads around the fact that ML is inherently probabilistic and that means that it will fail in production and so a lot of times where ML efforts get hung up is in the same stakeholders potentially that champion in the project to begin with not really being able to get comfortable with the fact that once the model is out in the world it's users are going to start to see failures that it makes in almost all cases and the last common failure mode in working with the rest of the organization is the rest of the organization treating ML projects like other software projects and not realizing that they need to be managed differently than other software projects do and one particular way that I've seen this become a problem is when leadership gets frustrated at ML team because they're not able to really accurately convey how long projects are going to take to。

CompSo educating leadership and other stakeholders on the probabilistic nature of ML projects is important to maintaining your sanity as an ML team If you want to share some resources with your execs that they can use to learn more about how these projects play out in the practice of real organizations I would recommend Peter Bele's AI strategy class from the business school at UC Berkeley and Google's people in AI guidebook which we be referring to a lot more in the rest of the lecture as well the last thing I'll say on educating the rest of the organization on M is that MP I think play like one of the most critical roles in doing this effectively to illustrate this I'm gonna make an analogy to the two types of ML engineers and describe two prototypical types of MLP that I see in different organizations So on one hand we have our task MP these are like a PM that's responsible for a specific product or specific product feature that heavily uses M these folks will need to have a pretty specialized knowledge of M and how it applies to the particular domain that they're working on。

For example， they might be the PM for the trust and safety product for your team or particular recommendation product for your team and these are probably the more common type of MLPMs in industry today。

 but an emerging type of MLPm is the platform MLP Pla MLPs tend to start to make sense when you have centralized ML team and that centralized ML team needs to play some role in educating the rest of the organization in terms of what are productive uses of M in all the products that the organization is building because these folks are responsible for managing the workflow in and out of the ML team so helping filter out projects that aren't really high priority for the business or aren't good uses of ML helping proactively find projects that might have a big impact on the product or the company by spending a lot of time with PMs from the rest of the organization and communicating those priorities to the ML team and outwards to the rest of the organization This requires a broad knowledge of ML because a lot of what this role entails is trying to really understand where。

can and should and shouldn't be applied in the context of all the things the organization is doing And one of the other critical roles that platform MLTPs can play is spreading ML knowledge and culture throughout the rest of the organization not just going to PMs and business stakeholders from the other product functions and gathering requirements from them but also helping educate them on what's possible to do with ML and helping them come up with ideas to use ML in their areas of responsibility that they find exciting so that they can over time really start to build their own intuition about what types of things they should be considering ML to be used for and then another really critical role that these platform MLPs can play is mitigating the risks of we've built a model but can't convince the rest of the organization to actually use it by being really crisp about what are the requirements that we actually need this model to fulfill and then proactively communicating with the other folks that need to be bought in about the model's performance to help them understand all the things that will need to understand about the model to really trust its performance so platform MLPs are I think。

Newer trend in ML organizations， but I think one that can have a big impact on the success of ML organizations when you're in this phase starting to build a centralized ML team or transition from a centralized M team to becoming an ML first organization One question I get a lot about ML product management is what's the equivalent of agile or any of these established development methodologies for software in M is there something like that that we can just take off the shelf and apply and deliver successful M products and the answer is there's a couple of emerging ML project management methodologies the first is crisp D。

 which is actually an older methodology but it was originally focused on data mining and has been subsequently applied to data science and M and the second is the team data science process TSP from Microsoft What these two things have in common is that they describe the stages of ML projects as sort of a loop where you start by trying to understand the problem that you're trying to solve acquiring data building a model evaluating it and then finally deploying it？

So the main reason to use one of these methodologies would be if you really want standardization for what you call the different stages of the project lifecycle if you're choosing between these two TDSP tends to be a little bit more structured it provides sort of more granular list of roles。

 responsibilities templates that you can use to actually execute on this process CRDM is a bit higher level So if you need an actual like granular project management framework then I would start by trying tDP but I say more generally it's reasonable to use these if you truly have a largescale coordination problem if you're trying to get a large MLT working together successfully for the first time but I would otherwise recommend skipping these because they're more focused on traditional data mining or data science processes and they'll probably slow you down so I would sort of exercise caution before implementing one of these methodologies in full the last thing I want to talk about is designing products that lend themselves well to being powered by machine learning So I think the fundamental challenge in doing this is a gap between what you。

Use expect when they're handed in AI powered products and what they actually get and so what users tend to think when they're given in AI powered products is their mental model is often human intelligence but better and in silicon so they think it has this knowledge of the world that it has achieved by reading the whole internet oftentimes they think that this product knows me better than I know myself because that' has all the data about me from every interaction I've ever had with software they think that AI powered products learn from their mistakes and that they generalize to new problems because it's intelligence it's able to learn from new examples to solve new tasks but I think a better mental model for where you actually get with an ML powered products is a dog that you train to solve a puzzle so it's amazing that it can solve the puzzle and it's able to solve surprisingly hard puzzles but at the end of the day it's just a dog solving a puzzle and in particular dogs are weird little guys they tend to fail and strange and unexpected ways that we as people with human intelligence。

Might not expect they also get distracted easily like if you take them outside。

 they might not be able to solve the same problem that theyre able to solve inside。

 they don't generalize outside of a narrow domain。 The stereotype is that you can't teach an old dog new tricks and in Ml it's often hard to adapt general knowledge to new tasks or new context。

 dogs are great at learning tricks， but they can't do it if you don't give them treats and similarly。

 machine learning systems don't tend to learn well without feedback or rewards in place to help understand where they're performing well and where they're not performing well。

 And lastly， both dogs learning tricks and machine learning systems might misbehave if you leave them unattended。

 The implications is that there's a big gap between users mental model for machine learning products and what they actually get from machine learning products。

 So the upshot is that goal of good Ml product design is to bridge the users's expectation with reality。

 and there's a few components to that The first is helping users understand what they're actually getting from the model and also its limitations。

 The second is that。Filures are inevitable， we need to be able to handle those failures gracefully。

 which means not over relylying on automation and being able to fall back in many cases to human the loop and then the final goal of ML product design is to build feedback loops that help us use data from our users to actually improve system One of the best practices for ML product design is explaining the benefits and limitations of the system to users one way that you can do that is since users tend to have misconceptions about what AI can and can't do focus on what problem the product is actually solving for the user not on the fact that it's AI powered and similarly the more openended and human feeling you make the product experience allowing users to enter any information that they want to or ask questions in whatever natural language that they want to the more they're going to treat it as human like and expose some of the failure modes that the system still has So one example of this was when Amazon Alexa was first released one of the sort。

Controversial decisions that they made was they limited it to a very specific set of prompts that you could say to it rather than having it be an openended language or dialogue system and that allowed them to really focus on training users to interact with the system in a way that it was likely to be able to understand and then finally the reality is that your model has limitations and so you should explain those limitations to users and consider actually just baking those limitations into the model as guardrails so not letting your users provide input to your model that you know the model is not going to perform well on so that could be as simple as you if your NLP system was designed to perform well on English text then detecting if users input text in some other language and either warning them or not allowing them to input text in a language where your model is not going perform well the next best practice for ML product design is to not over relyly on automation and instead try to design where possible for human in the loop automation is great but stad automation can be worse than no automation at all。

So it's worth thinking about even if you know what the right answer is for your users。

 how can you add low fririction ways to let users confirm the model's predictions so that they don't have a terrible experience when the model does something wrong and they have no way to fix it one example of this was back when Facebook had an autotagging feature of recognizing your face and pictures and suggesting who the person was they didn't just assign the tag to the face even though they almost always knew exactly who that person was because it'd be a really bad experience if all of a sudden you were tagged in some picture of someone else instead they just add like simple yes know that lets you confirm that they in fact got the prediction that this is your face correctly in order to mitigate the effect of when the model inevitably does make some bad predictions there's a couple of patterns that can help there the first is it's a really good idea to always bake in some way of letting users take control of the system like in a self-driving car to be able to grab the wheel and steer the car back on track if it makes a mistake and another pattern for mitigating the cost of bad predictions。

Is looking at how confident the model is in its response and maybe being prudent about only showing responses to users that are pretty high confidenced。

 potentially falling back to a rules-based system or just telling the user that you don't have a good answer to their question and the third best practice for ML product design is building in feedback loops with your users So let's talk about some of the different types of feedback that you might collect from your users and the X axis is how easy it is to use the feedback that you get in order to actually directly make your model better on the y axis is how much friction does it add to your users to collect this feedback so roughly speaking you could think about like above this line on the middle of the chart is implicit feedback that you collect from your users without needing to change their behavior and on the right side of the chart are signals that you can train on directly without needing to have some human intervention the type of feedback that introduces the least friction to your user is just collecting indirect implicit feedback on how well the prediction is working for them So these are signals about user behavior that tend to be a proxy for model performance。

Like did the user churn or not these tend to be super easy to collect because they're often instrumented in your product already and they're really useful because they correspond to important outcomes for our products。

 The challenge in using these is that it's often very difficult to tell whether the model is the cause because these are highlevel sort of business outcomes that may depend on many other things other than just your model's prediction so to get more directly useful signals from your users you can consider collecting direct implicit feedback where you collect signals from the products that measure how useful this prediction is to the user directly rather than indirect for example。

 if you're giving the user recommendation you can measure whether they clicked on the recommendation or if you're suggesting an email for them to send did they send that email or did they copy the suggestion so that they can use it in some other application Oftentimes these take the form of did the user take the next step in whatever process that they're running did they take the prediction you gave them and use it downstream for whatever tasks they're trying to do The great thing about this type of feedback is that you can often train on it directly。

It gives you a signal about which predictions the model made that were actually good at solving the task for the user but the challenge is that not every setup of your product lends itself to collecting this type of feedback so you may need to redesign your product in order to collect feedback like this next we'll move on to explicit types of user feedback explicit feedback is where you ask your user directly to provide feedback on the model's performance and the lowest friction way to do this for users tends to be to give them some sort of binary feedback mechanism which can be like a thumbs up or thumbsdown button in your product this is pretty easy for users because it just requires them to click one button and it can be a decent training signal there's some research and using signals like this in order to guide the learning process of models to be more aligned with users preferences if you want a little bit more signal than just was this prediction good or bad you can also ask users to help you categorize the feedback that they're giving they could for example like flag certain predictions as incorrect or offensive or irrelevant or not useful to me can。

Even set this up as a second step in the process after binary feedback so users will still give you binary feedback。

 even if they don't want to spend the time to categorize that feedback and these signals can be really useful for debugging but it's difficult to set things up in such a way that you can train on them directly another way you can get more granular feedback on modelss predictions is to have some sort of freet input where users can tell you what they thought about prediction this often manifests itself in support tickets or support requests for your model This requires a lot of work on the part of your users and it can be very difficult to use as a model developer because you have to parse through this like unstructured feedback about your models predictions yet it tends to be quite useful sometimes in practice because since it's high friction to actually provide this of feedback the feedback that users do provide1 be very high signal it can highlight in some cases the highest friction predictions Since users are willing to put in the time to complain about and then finally the gold standard for user feedback if it's possible to do in the context of your product and your user experience is to have users。

Correct the predictions that your model actually makes。

 So if you can get users to label stuff for you directly then that's great then you're in a really good spot here。

 And so one way to think about where this can actually be feasible is if the thing that you're making a prediction for is useful to the user downstream within the same product experience that you're building not is this useful for them to copy and using in a different app but is it useful for them to use within my app So one example of this is in product called greatcope which Serge built there is a model that when students submit their exams it tries to match the can written name on the exam with the name of the student in the student registry now if the model doesn't really know who that student is if it's low confidencefid or if it gets the prediction wrong then the instructor can go in and recategorize that to be the correct name that's really useful to them because they need to have the exam categorize the correct student anyway but it's also a very direct super。

ory signal for the model。 So it's best of both worlds。

 wheneverever you're thinking about building explicit feedback into your products。

 it's always worth keeping in mind that users are not always as altruistic as we might hope that they would be and so you should also think about how is it gonna be worthwhile for users to actually spend the time to give us feedback on this the sort of most foolproof way of doing this is as we described before to gather feedback as part of an existing user workflow but if that's not possible if the goal of users providing the feedback is to make the model better then one way you can encourage them to do that is to make it explicit how the feedback will make their user experience better and generally speaking the more explicit you can be here and the shorter the time interval is between when they give the feedback and when they actually see the product get better the more of a sort positive feedback loops this creates for that the more likely is that they're actually gonna do it a good example here is to acknowledge user feedback and adjust automatically so if your user provided。

Fedback saying hey， I really like running up hills then sort of good response to that feedback might be great。

 Here's another hill that you can run up in 1。2 kilometers。

 They see the results of that feedback immediately and it's very clear how it's being used to make the product experience better Les good is the example to the right of that where the response to the feedback just says thank you for your feedback because I as a user when I give that feedback there's no way for me to know whether that feedback is actually making the product experience better so it discourages me from getting more feedback in the future the main takeaway on product design for machine learning is that great M powered products and product experiences are not just taking existing product that works well in both Ml on top of it。

 they're actually designed from scratch with machine learning and the particularities of machine learning in mind and some reasons for that include that unlike what your users might think machine learning is not superhuman intelligence encoded in silicon and so your product experience needs to help users understand that in the context of the particular。

Problem that you are solving for them also needs to help them interact safely with this model that has failure modes via he in the loop and guardrails around the experience with interacting with that model and finally great M products are powered by great feedback loops because the perfect version of the model doesn't exist and certainly doesn't exist in the first version of the model that you deployed。

 And so one important thing to think about when you're designing your product is how can you help your users make the product experience better by collecting the right feedback from them this is a pretty young and underexported topic and so here's a bunch of resources that I would recommend checking out if you want to learn more about this many of the examples that we used in the previous slides are pulled from these resources and in particular the resource from Google in the top bullet point is really good if you want to understand the basics of this field So to wrap up this lecture we talk about a bunch of different topics related to how to build machine learning products as a team and the first is machine learning roles and the sort of takeaway here is that there's many different skills involved in production machine learning production。

Is inherently interdisciplinary so there's an opportunity for lots of different skill sets to help contribute when you're building machine learning teams since there's a scarcity of talent。

 especially talent that is good at both software engineering and machine learning it's important to be specific about what you really need for these roles but paradoxically as an outsider it can be difficult to break into the field and the sort of main recommendation that we had for how to get around that is by using projects to build awareness of you're thinking about machine learning the next thing that we talked about is how machine learning teams fit into the broader organization we covered a bunch of different architects for how that can work and we looked at how machine learning teams are becoming more standalone and more interdisciplinary and how they function Next we talked about managing M teams and managing M products managing ML teams is hard and there's no silver bullet here but one sort of concrete thing that we looked at is probabilistic project planning as a way to help alleviate some of the challenges of understanding how long it's going to take to finish machine learning projects and then finally we talk about product design in the context of machine learning and the main。

way there is that today's machine learning systems are not AGI right they're limited in many ways。

 and so it's important to make sure that your users understand that and that you can use the interaction that you build with your users to help mitigate those limitations so that's all for today and we'll see you next week。



![](img/dc3925d6d72462a5cce317768c6302a6_4.png)

![](img/dc3925d6d72462a5cce317768c6302a6_5.png)