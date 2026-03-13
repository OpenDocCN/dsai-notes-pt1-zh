# 002：开始本地大型语言模型的 Llamafile｜Beginning Llamafile for Local Large Language Models (LLMs) p02 1_由 Mozilla 介绍 Llamafile 概述.zh_en -BV1e6421Z7sg_p2-

![](img/2c6985640765b72a723373659d401483_0.png)

Hey there， My name is Stephen Hood， I work for Mozilla and my role at Mozilla is to oversee open source AI projects and initiatives and one of my projects is Lmaophil which we're here to talk about today Lmaophil is is a project that makes it very easy to use open models on any consumer hardware without much technical understanding or knowledge with very little installation or configuration it makes what is really a stack of complexity for using open source AI and collapses it down to just a file that you download and run。

We were inspired to create Lmophil from our own research into the state of open source AI technology。

 we found that there's a tremendous amount of activity today in the open source AI space but a lot of the tech is still too hard to use so there are a huge number of models to choose from to begin with but once you have those models they come in different formats the formats keep changing as the technology evolves and then you have to make decisions once you have a model about what runtime you're going to use so how you're actually going to get it up and running in order to do inference against that model and depending on your model。

 your platform and your chosen runtime solution you may have a lot of things you have to do you may have to compile C or C++ code and make sure you have the right tool chain installed to do that you may have to install and manage Python dependencies in some cases you have to install native software。

And sometimes all these things sort of come together， and it can be complicated。

 and our concern has been that the harder it is to use open source AI。

 The less likely developers are to adopt it， or the more slowly they will adopt it。

 And that could hinder its evolution， which we think is incredibly important。

 we think that having competitive， advanced open source AI technology in the hands of the public。

 is critical to ensuring that AI does not end up being dominated by a handful of commercial interests。

 the way the Internet has arguably become。 And that's a cause we care about a lot being Mozilla。

 the makers of Firefox， we care about the web， and therefore we have to care about AI as well。

 because it's already transforming the way we all use the web and the way we use computers。

So with that in mind， we createdllma file and what we did was we took two different projects and we combined them in a unique way。

The first project is Lamus。cppP this is a very popular open source project that makes it possible to run quantized open models on everyday consumer hardware。

 so these are models that are compressed down so they require less memory to run。

 and then the software itself is a very highly efficient C++based inference engine that can execute these models on everyday hardware whether it' be MacBooks or Windows machines or Linux boxes。

So we took that project and we combined it with another one called cosmopolitan。

Cosmopolitan is a project created by Justine Tney and what it does is it makes it possible to compile C and C+ plus programs in a way that they will run on almost any computer I don't mean。

You create different executables， one for Windows， one for Linux，1 for Mac and so forth。

 The same compiled file runs unaltered on all these different platforms。

 You can even run them on Raspberry Pis。 It's a pretty remarkable little bit of technology there that she has created。

 And we working with Justine combine these two projects together。

 And what we it allows us to do is take any open weights in the Gg Uf format。

 which is a popular open model format today。 You can find these unhuging face and other various places。

😊，And you can then wrap it in the Luma file executable code。

 which adds only 40 or 50 megabytes to what is a multigabyte file that's basically a rounding error and now that file you can distribute and people can download it no matter what kind of computer they're running and it will just work when they run it。

 it brings up their web browser with a web-based locally hosted chatbot interface。

The AI is running entirely on the user's local machine。

 You could unplug it from the Internet at that point， and it will continue to work。

 So it's 100% local。 It's 100% private to the user so they can use it with confidence that no one else is listening in。

 no one else is doing anything with their data。And this works like I said on almost any computer。

 if you have a GPU， a graphics processing unit which can accelerate large language model inference。

 then it will detect it， whether it be NviIDdia or AMD and this is a big deal because quite often AMD does not have the same support that NVIDdia has in the AI machine learning space。

 so we've been able to address that as well。We have on our website our GitHub repo。

 a number of Lma files example LAMma files that you can download and use。

 and I will in future videos， I can talk more about how to create your own LAMma files。

 how to use LaMma file yourself， and how to also use it as a developer。

 as a drop in replacement for open AI。Hope you enjoy using Lamaophil thanks a lot。

