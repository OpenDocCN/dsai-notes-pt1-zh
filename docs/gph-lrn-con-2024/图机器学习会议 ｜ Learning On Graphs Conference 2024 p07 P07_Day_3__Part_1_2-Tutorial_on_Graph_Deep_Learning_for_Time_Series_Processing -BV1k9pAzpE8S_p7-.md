# 图机器学习会议 ｜ Learning On Graphs Conference 2024 p07 P07_Day_3__Part_1_2-Tutorial_on_Graph_Deep_Learning_for_Time_Series_Processing -BV1k9pAzpE8S_p7-

Welcome to the tutorial session for G deep learning for time series processing forecasting。

 Re and analysis you are most welcome to engage with your organizers and each other in the zoom chat function zoom Q&A or on the tutorial discussions channel on Slack please note that the tutorial will be recorded and then upload it to our YouTube channel for those who are unable to join or who would like to rewatch the session in the future if you prefer not to be recorded you are welcome to turn off your camera and participate in the chat。

😊，Now， without further ado， I will hand it over to our tutorial organizers。

Hi hello hello everyone thank you for being here and for attending the tutorial tutorial so I'm Andrea and I will be the speaker for the first part of the tutorial so these slides are available at the link that you can also find in the chat that we have just posted if you want to follow along。

So let's start with a bit of introduction， so there are many applications。

where basically that are characterized by data that are continuously produced over time by multiple data sources。

 by multiple sensors， there are many examples of this， we have like traffic monitoring systems。

 sensor networks in smart cities and also this is typical of many energy analytics applications。

 but also many applications in science， engineering in general， but also financial market analysis。

 for instance。And all of these applications have one thing in common that is that this time series。

 these temporal data are characterized by reach dependency structure。

So the standard deep learning approach over the years to process time series in particular to forecast。

 time series has been that one of training a single neural network on large collection of this related time series。

Within this framework， each time series is processed independently from the others。

 while the parameters of the model are shared， the main advantage of this approach in its simple efficiency since the model gets traineded on data that come from many different sources。

 but the main downside is that in this way， the dependencies this reach dependency structure that might exist among the10 series is neglected。

What we will see in the tutorial is how we can use GraphD learning to go beyond these limitations of the state of the art。

 so as you know since you are attending this this conference GphD learning operators and graph representations allow to allow us to embed these dependencies directly into the processing as inductive biases and as we will see the resulting models can operate on sets or subsets of correlated time series while still adding the parameters of the model shared。

Unfortunately， as we will also discuss， there are inherent challenges in applying this processing framework to data coming from the real world。

So as anticipated the tutorial will present advances coming from the combination of deep learning approaches for T series processing and deep learning on graphs and in particular we will have a threefold objective where the first objective is to provide a comprehensive framework to build these graph based deep learning time series processing models and at the same time when also to provide methods to address the challenges and the potential pitfalls in using these approaches in practice。

And we will also be providing tools and guidelines to use these approaches to tackle the real world application and also for developing new methods of this kind。

The presentation is complemented by a demo， so by a coding session that will happen in the middle of the tutorial in the middle of the tutorial and the tutorial is also available in a paper format on archive and you find the reference here。

So it's also important to say what this tutorial is not about。

 so this tutorial is not about processing sequences of interactions as typically done some time when processing temporal networks。

 but as anticipated here， graphs will be a tool， representational tool that we will use to represent the possibly dynamic relationships among a collection of time series。

So this is an outline， so in the first part we will be formalizing the problem setting we will be operating on so that one of correlated MC series processing。

 we will also present how graph based representations can be used in this context and then we will see how to build graph neural network architectures to process this kind of data in particular we' talk about an important distinction regarding these models which is the one between global and local models。

Then Ivan will be delivering software demo， so the decoding session and both Ivan and Daniela will discuss the challenges in the second part of the tutorial。



![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_1.png)

上。

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_3.png)

Let's start with the first part and with the problem setting。So。

For what concerns the kind of data that we want to process。

 we assume to have available a collection D of n correlated the time series。

 and each one of the time series can be multivied so we can have multiple observations at each time step for each time series。

Together with the target1 series， we can have covariates。

 these are thesogen variables that might be somehow helpful in doing the processing and these covariates can be both dynamic but also static and in this case we talk about attributes。

We will use the capital letters to denote these stacked representations across the collection of10 series。

And refer to this dimension that spans the collection as the partial dimension。

So what are correlated time series， why the correlated in the name。

 so we assume that data are generated by a time environment stochastic process and that there exist a causality aegange among the time series。

 which means basically that the forecast， the predictions for one specific time series can be made more accurate by taking into account observations that related the time series in the collection。

Furthermore， we also assume that all of these time series are homogeneous so that the observables for each time series are same the same physical meaning。

And that the observations happen synchronously over time and also regularly。

 we don't assume instead that these observations are generated by the same process for each time series。

And so it's important to say that while this assumption might sound very restrictive in practice。

 they can usually be met with appropriateprocessing steps and can also be relaxed as we will see in the second part of the tutorial。

Now we can look at an example， again， traffic monitoring systems are a classic example of this kind of scenario and you can imagine to have many sensor spread around traffic network and each sensor will acquire some measurement regarding the traffic at this position can be like the average speed of vehicles。

 the number of vehicles or things like that， and extragenous variables in this case can be like date time information or also like weather prediction。

For what concerns the static covariates so the attributes。

 you can have information regarding like the type of lane or the sensor is placed on or the presence of traffic lights or things like that。

Clearly， in these kind of scenarios， there are strong dependencies among the resulting a collection of time series。

 and this dependency will reflect the structure of the road network。

So let's start to look at one of the main task， main processing task that one is usually interested in performing when dealing with the time series。

 which is forecasting。And the objective here is pretty much straightforward。

 so the objective is that from a window of past observations。

 we want to forecast the next age time step ahead observations。In particular。

 we want to do that by learning a parametric model of the process generating the data。

And in particular in the tutorial， we will focus on point predictors。

 while probabilistic methods can also be considered。In particular with point predictors。

 these are usually trained by minimizing a cost function that somehow mal measuresasures the forecasting error and using different cost functions。

 you can get predictions of different values， for instance using the MSC， so the mean squared error。

 you are forecasting you are predicting the mean of the stochastic process， while using， for example。

 the MA， so the mean absolute error， you get predictions of the medium of the process。Now。

 a first important distinction that we need to make when talking about forecasting architecture is that between local and global models。

So local models are the family or models that have been typically used in the past for many decades to forecast time series by using like statistical methodss like the box Jenkin approach box Jenkins approach and the idea here is that you have a model whose parameters are specific to each single time series that you want to process。

 that you want to forecast。The advantage of this approach is that the resulting model can easily handle eterogeneity in the dynamics of the time series in the collection since they are tailored to that specific sequence to that specific process。

The downside is inefficiency， which is an inefficiency， which is both in terms of computations。

 computation since you would have to train many different models。

 but it's also in efficiency in terms of sample complexity because again。

 you would need you are using samples from a single term series to fit a model。

At the opposite end of the spectrum we have global models。

 which is what is typically done in deep learning and here we have that we have a single model that is trained on many time series coming from different sources。

 this is similar to what nowadays people call foundation models somehow。

 but usually here this scope can be more limited like for instance to time series coming from a specific domain。

The advantage here again is clearly as anticipated before any in the sample efficiency of the approach and also and this sample efficiency allows for building more complex model。

 more complex architectures that can be used to process the input time series。

The common downside of these two approaches at least in their standard implementations。

 is that they both neglect the dependencies that might exist among the time series。

There are few methods that have been used in the literature to to deal with this beyond the the graph representations we will be talking about later and the the naive option。

 the one that that the the。The straightforward thing to do would be to consider the input collection of time series as a single very large multi at the time series。

 but this clearly as severe scalability issues since this clearly suffers from the course of dimensionality。

 so it results in eye sample complexity and poor computational scalability。

We can instead consider models that operate on sets of time series。

 keeping the parameters of the model shared among this time series。

And an example of these is like attention based architectures like transformers， where in this case。

 attention would be computed with respect to the spatial dimension rather than the temporal axis and this approach can work pretty well but as you know。

 like if you are familiar with also like with the static graphs。

 we know that using the transformers to process these kind of data can work pretty well。

 but clearly the downside is that we are not exploiting any prior on the structure of the dependencies and also on the sparsity of these dependency structure。

There are also other methods that have been used in the literature that for instance rely on dimensionality reduction。

 and here are the idea is that we can extract some shared latent factors from this large collection of time series and use these factors to condition a global model。

And these can work well if data are below rank in certain application。

But the downside of this is that we are losing the local fine grained information。

 which instead as we will see， is captured very well by the graph based approach and of course。

 also these approaches in cases of like very large data set。

 very large collections can suffer from the same scalability issues of the other approaches。

Now we can start talking about these graph based representations， so as anticipated。

 the idea is to use a graph to represent the functional dependencies among the1 series and use this graph as an inductive bias for our learning model。

We can use the graph additionsymmetric to capture the to model these dependencies and additionsymmetric。

 which can be both asymmetric and dynamic so it can vary with the T so with a each time step。

Together with the additionsymmetric metrics， we can also have edge attributes。

 which can be dynamic on their own， and that can be both categorical and numerical。

Finally we can go back to the traffic example and here again we will have that the structure so the graph。

 the additionsymmetric metrics can be extracted from the structure of the road network and you can have like attributes or weights associated to the edges also to encode the road distance and here in this case a dynamic topology can help for instance。

 for taking into account modifications in the traffic network in the structure of the traffic network。

This is a sort of summary of all the information that we have available at each time step。

 so we have our target time series together with the extragen variables that can be both dynamic and static plus the relational information that we just added to the setting。

So now the idea is to use this relational side information to to condition our predictors。

 our forecasting architecture， and these relationships as we will see。

 can act as a regularization to localize the prediction predictions with respect to each node and in particular。

 these can be used to prune any spurrous correlations that might be the result of not taking these structure into account Furthermoremore these approaches far more scalable than standard multivariate models because again。

 we can keep the parameters of the model shared among the time series we are processing in fact we can use these kind of architectures to forecast and to process any subset of the correlated the time series。

In particular， the kind of graph neural networks that have been developed to process these data are called special temporal graphph neural networks to refer to the fact that propagation in these models happens across both time and space and in particular we will focus on those models based on the message passing framework。

And we will do that by considering this template architecture where we have which is composed by an encoder。

 which simply encodes the observations， each observation independently each time step and node。

 and the encoder is then followed by a stack of spatial temporal message passing layer。

 which is the only components of the architecture where the propagation through time and space happens。

The representations extracted by the special temporal message passing blocks can then be mapped to predictions by a decoding block。

Eder block we can look at more finer details of this and you see that the processing here happens at the level of the single time step again in single node for what concern the encoder。

 the resulting a sequence of observation is then processed by the special temporal message passing layers。

TheThe decoder again， will then be operating at the level of the single node and time step to map these representations to prediction。

We can have then several implementations of these special temporal message passing blocks。

 and we can see this as a sort of generalization of standard standard message passing layers where instead of having like static representations associated to each node here we have sequences of representations so what we will need to do is to modify the standard operators that compose a message passing architecture so that they can be they can process sequences。

Clearly， there are many possible designs that exist to do this and that can be somehow matched with the requirements that we have for each specific application。

So the next step would be that of characterizing the possible design paradigms for these special temporary blocks。

 but this would be a good moment for asking any questions or clarify any doubt before moving on so if you have any feel free to use the chat or just unute yourself and ask ask directly。

Okay。No questions， but yeah， anyway， if you feel like some some step of the discussion it' not clear if you're free to use the chat and we will keep an eye on it。

Okay， so deep， there are as I was saying， there are several， okay， we have a question， yeah right。

Oh sorry， they can unmute themselves So if you want to read out the question， Yeah， yeah， Okay。

 so regarding correlation， it's possible to use SD DG and multi the have signals。Okay。

 so I think we are referring to your regular time series。 This is。

This is a good question and there are methods that can be used to model these kind of irregular observations over time and space。

 and we will touch on them in the second part when we will be talking about dealing with the missing data。

 but yeah there are methods to do that。Then。The other question is is the encoder decor also design choice and yes it is you have different operators that you could use in practice in many cases since these are just again first encoding a first transformation and later on just a readout these are usually implemented with standard MLPs but yeah in principle you can use whatever basically。

Okay。So as I was saying there are several methods， several is it okay last is it similar to embeddings in transformers doing code positional information too we will talk about this in a moment。

 there is something similar to this that is actually used often in this kind of models。

 but we will arrive to that point。So for what concerns？

The different designs we can distinguish between time and space models and here in these kind of models。

 the temporal and spatial processing cannot be factorized into separate steps。

 so they happen jointly we then have time and space models which instead as the name suggests they factorize the processing of the temporal and spatial dimensions in two separate steps and finally we have the space then time approach which is basically the time then space in reverse order。

For the time and space， as I was anticipating here， it is that in this model。

 the propagation of representation through time and space happens in a way that means that the resulting model architecture。

 this processing cannot be factorized into separate stages。

And there are several ways of implementing these architectures， for instance。

 one standard way is to integrate message passing into sequence modeling architectures。

 we will see some examples of this。And the other approach is instead to use sequence modeling operators directly to compute messages。

 you a message by single layer。And。Finally， there are also product graph representations。

 which are basically a way of getting a static graph representation from these collections of Teamme series so that we can use the standard message passing operators on that。

So one standard example and actually also from a chronological perspective this is probably listed to the best of our knowledge the first model that has been introduced to do this you can we can start from looking at a standard GRU cell so looking at a standard gated the recurrent and neural network and you can see that here clearly each time series is processed independently from the others。

 we can easily get in this patient temporalor graph neural network version of these by implementing each gate of the recurrent cell by using message passing this is also very similar to the first kind of graph neural networks that have been also used to process to process static graph data but in this case at each step at each update of the cell states。

We perform a message passing at a different time step。

 so we use this network to update the states of DS cell at each time step by reading information at neighbors。

And these kind of diamond space models are known as graph convolution or recurrent and neural networks。

 and there have been many version of these， in particular。

 many different architectures that have been tried to implement these by using different message passing blocks。

In particular， one very popular architecture to do this is known as the diffusion convolution orcurrent neural network。

 which is basically GC RNN， where the message passing operators are implemented by these bidirectional diffusion convolution。

 where basically the idea is to have different wayss for incoming and outgoing edges。

And this was one of the first models that have been used to process time series and has led to many follow up and it's still quite popular and quite effective。

The second example is spatial temporal convolutional networks where here what we are doing is just alternating temporal convolutional filters with spatial convolutions。

 so with convolutions on the graph。So basically here what you are doing is applying standard the temporal convolution at each node separately and again you can use any kind of temporal convolutional filter that you want。

 this is this is going to be a 1D convolutional filter。

And then follow this with a step of message passing by stacking many of this layer。

 you get an architecture where the receptive field will get larger at each layer with respect to both time and space。

In particular， there's this SDGCN， which has been the first architecture of this time of this type to be introduced in the literature and here basically the the blocks are stacks of gated temporal convolutions with polynomial graph filters。

 and similarly to to the recurrent SDGNs， also for these kind of models that have been many different implementations in particular there is one model which is called graph waveNe that has been out since since 2019 and is still very popular and still performs very well in benchmarks and the ideas to basically use more advanced convolutional operators also including delationtion to increase the receptive field of the model。

fasterster。The third example of time and space model is those models that use sequence modeling operators to compute messages and here this one is a simple example where the messages are computed by using a TCN again in this case basically what will you be doing is to just concatenate the the time series so the sequence of observations at the two neighboring nodes and then apply a convolutional filter on the resulting sequence。

Clearly you can use whatever sequence modeling operator that you want and for instance。

 there are many examples of these kind of architectures that use attention operators and in this case this will be cross attention since you will be computing attention between sequences of two different nodes。

We then have the product graph representations and these models。

 these representations come from the simple observation that you can see the temporal dimension as a line graph and combine in some way。

 I mean there are several ways that one could consider。

 combined this temporal graph with a special graph。And then again。

 process the resulting representation using a standard message passing net。For instance。

 you can consider a standard Cartesian product where basically here each node will be， well。

 each node that each time step will be connected to its neighbors and to itself at the previous time step。

There are also other ways of wiring this graph that you might use like the Chroncker product。

 and in this case the idea is to connect each node to its neighbors at the previous time step。

You can also come up with many different methods of wiring this graph and actually I think this is a direction where further studies would be needed also to understand the properties of these product graph representations。

We can then see the time then space approach and this one is very simple also to understand that also the resulting models are very simple to build and the idea here is to just embed each sequence separately into a vector representation and then perform message passing on the resulting graph。

And the advantage here is that this is very easy to implement and also computationally efficient because at the training time again。

 you're not performing message passing a each time step， but only at the last one。

 and we can also use all of the operators that we ready know to process sequences and graphs。

The downside of this is that these two step procedure might introduce a very serious information bottleneck and as you know these can make some of the issues that we commonly have on graphs such as overmoting and overs squing even more serious。

 like it could make very difficult to propagate longer range information across space and time。

And also， since you are performing message passing only with respect to a single graph representation。

 this would also make it more difficult to account for changes in the topologies and also for dynamic edge attributes。

Then we have the space then time approach which again as anticipated is basically the other way around here you are performing a message passing a teach time step separately and then you are encoding the sequence of representations with a sequence model here these approaches have been used quite often in the literature but actually they still have this kind of bottlenecks and this kind of factorized processing that might be might make it more difficult to propagate information and they do not enjoy the same computational advantages time then space models since you are computing message passing for each single time step。

So now I see， maybe there's one question。嗯。Okay， yeah， there are also okay。

 the question is about if we have other options to model the temporal dimension rather than using a line graph。

 yeah for sure， like I think you can consider any representation that you want as long as you take care of somehow preserving the structure of the data。

 also one thing that I' have not said is that when also you perform message passing on that kind of graph。

 you should take care of。Considering that temporal and spatial edges have a very different meaning。

 like for instance， you could use different ways to process the different messages or things like that。

 but yeah in principle， I think it's the general idea is that of using these static graph representations to process these data and I think the optimal way of well the optimal representation that one might use is kind of an open question or rather using different representations might have different properties that my align well or not with your problem。

Is there a method that use timed space then mixed well you can do all sort of wild things the problem is that with the usual timed space approach usually what happens is that after you have included the temporalor information after that you have a static representation right because you perform a message passing only with the representation extracted by the encoder so you have time series going in in the first step or in the time step and then you have a graph coming out which is later on processed of course you can imagine something that was slightly different maybe you can add some kind of skip connections or whatever but yeah also I think these things that you say like timed that mixed is somehow similar。

to the diamond space models。That I was talking about when when Google talking about the convolutional approach。

 in that case， they work exactly like this， so they interlieve the temporal and special filters to progressively increase the receptive field。

Okay， then I think we can move on and then later on we can go back， we can answer further questions。

So。This globality and locality thing I've been talking about quite a bit in the presentation plays a big role also in SDGNs。

So STGNs at least understand and implementation with these template that we have been talking about。

 they are global models so they can handle arbitrary neural sets， so they are inductive models。

And they can use the dependency structures to provide further conditioning on the predictions on the forecasts。

However， this further conditioning might not be enough and the model can struggle in modeling all the hetero heterogeneous dynamics that might be present in the time series collection and as a result the model might need very long observation windows and a very eye model capacity in order to account for all of these different heterogeneous dynamics that might be present。

One way around this has been that the literature has somehow followed is to consider hybrid global and local architectures。

One straightforward way of implementing hybrid architecture would be to just， for example。

 like looking again at our template architecture to turn some components of this architecture into local。

For instance， you could make the encoder and decor of parameters that are specific to the time series that will be processed there。

The clearly the resulting models would be able to capture these local effects much more easily。

 you can see this as kind of similar to having to using like a backbone model and have some layers that are fine tuned or that are specific to the kind of data that you want to process there。

And of course， the downside is that if you want to process many times series using this approach。

 this would result in a very large number of local parameters depending on how these encoder and decor blocks are implemented。

One way toortize thiscourse is to simply consider node embeddings。

 which are just vectors of learnable parameters that can be associated to each node。

 and this slightly goes back to the previous question and you can see this kind of node embeddings as a sort of learnable positional encodings。

 but what they are doing here actually is not encoding the position of the node in the graph。

 but actually taking care of modeling the local components of modeling the specific characteristic of each time series。

These learnable components， these learnable vectors can be fed into the encoder and decor。

 and as I was saying， they amortize the learning of these local time series specific processing blocks。

Allowing for keeping most of the models parameters shared。Clearly。

 we still have the downside of having a number of learnable parameters that in this way scales linearly with the number of time series that we are processing。

 and so there are intermediate solutions that one might consider such as learning embeddings for clusters of time series rather than for each single sequence。

One other issue that is very important to consider here is that when we move to hybrid architecture。

 the resulting model is not inductive anymore so this might sound as a downside and actually is so you definitely lose some of flexibility but in practice you also gain something you transfer learning scenarios so in those scenarios where you have some data from the target domain that you can use to fine your model。

In particular having an hybrid architecture allows for keeping the shared parameters of the model shared。

 well sorry， keeping most of the part global components of the model shared and just fine tune the local parts。

 in this case， just fine tune the embeddings。And in particular has been shown in the literature that regularizing these node embeddings can facilitate this transfer learning procedure even further。

We can start and we can have a look at some empirical results。

 So these four are very popular benchmarks for correlated time series forecasting in the upper most part of the table here。

 we have some baselines some reference architecture that have been developed by using the the template that we just saw in particular。

 we have these GCR andNs that that are used。That are basically time and space models。

 and then we also have different implementations of time then space models。

And then we have also three architectures from the state of the art。

So we can see that when we add these local embeddings， so for the baselines。

 when we make these models， hybrid， so when we introduce some part that is modeling the characteristic of each time series separately。

 we see that in performance you can improve by a white margin。

 so much so that with these very simple architectures。

 in many cases you are able to match the performance of much more complex state of the art models。

Also much deeper with many more parameters。We can then have a look at some transfer learning results。

 so here what we did basically was taking four data from four different traffic networks。

 we trained a model on three of these networks and then performed the transfer step on the fourth one。

The first observation is that the fine tuned model。

 so the models after transfer learning perform much better than the zero shot inductive model。

 this is somehow to be expected and we might also expected this gap to be aware if we use more be to train the model。

But the other very interesting thing that we can see is that the performance of the hybrid architecture where just the local components are fine tuned are fit on the new data are much better than those of the fine tuned global model。

In particular， these variational and clustering thing are regularizations that have been studied in the literature this paper cited here and with by using these regularizations。

 performance in transfer can be improved even further。

So now we have reached the end of the first part， we formalized the problem of processing a correlated time series。

 and we show we can use graph representations for modeling the dependencies among them。

 we discussed the forecasting problem and this important distinction between global and local deep learning models for time series。

 which is a crucial aspect of the whole thing。And we then saw approaches to building a special temporal graph neural networks and the associated tradeoffs。

Before discussing the challenges， we will look at software implementations of the above in particular。

 Iva will be presenting a tutorial of library that we developed。

 but before that we can answer some questions and have a short five minutes break。

Would be interesting internal analyzeize the causes of the local behavior yeah would definitely be interesting There are different things that you can do one analysis that for instance we did in this paper here was to cluster the node embeddings well actually the clustering was done into end and then we looked at the resulting time series clusters and actually you could see that the corresponding time series were quite different in particular we used the data set of like load consumption and there it was。

Easily， easy to see that those time series at very different behaviors， for。

 for example like very different consumption patterns and in the same days of the week。

 but yeah I think this is you could probably also do something which is more fine grained if you also yeah。

 if you use some way of representing some local components which is more interpretable。

 but I think this is kind of an open question， yeah you can definitely try to use these somehow to analyze the different behaviors。

The other question is。So the static graph after time series is included to a graph is it a universal problem in both local and global models。

 can it be solved with the name cultural aggressive context。So okay。

 the fact that the time series or collapsed during a static graph representation is typical of time them space models。

This does not happen in time and spells approaches， as we saw before， like for instance。

 with recurrent graph convolutional networks， there the representation is not collapsed。

But this thing of having these global and local aspects， it's orthogonal tool models， I think it's。

 I mean it's even orthogonal to graph based approaches。

 it does play an important role here since you want to take this dependencies among times zero into account but this is something orthogonal tool tool to the specific architecture that you are using。

Could you comment on which approach time and space time and space is space then time is commonly used for traffic monitoring？

Well， these， again， there are many architectures they use many different that are wired in many different ways。

 so the literature is very， very rich in this regard and there are also surveys that try to classify all of these different approaches。

I'd say the time and space approach is the one that is most commonly used。

 but this also depends on the kind of constraints that you have。

 particularly if you have computational constraints the time then space approach works pretty well but I think the fact that that approach can work pretty well also depends on the kind of data right so if you really need to model a longer range interaction in that case a time then space approach might not be enough。

My suggestion and later on， when we try to come up with some guidelines is that you can definitely start from using a time than space as a first try since again it's much easier to train much more scalable and that gives you good results and we will also talk about quantifying well。

 seeing what good results mean。Then then it it's fine。It's actually much more easy to use。

Is there any loss function that takes to account to the the error the violence and the error for SDGNN？

Well， I think you are referring to， well， again， you can。Fit these models using。

Basically any kind of error function that you want， for instance。

 if you want to take the variance into account and by taking the violence into account I mean quantifying the variance you can train a probabilistic model as I was saying this is not covered by the tutorial we are just considering point predictors for simplicity but there are many resources on how to build the probabilistic time time series forecasting models and I'd say that this is pretty much orthogonal to the discussion we are having here yeah for sure there are methods that you can use to quantify uncertainty and yeah。

Okay， so I think we can have the the short five minutes break and no problem， no problem。

 nothing to be sorry about， we will have the five minutes break and we will be back with the tutorial at yeah yeah。

In five minutes。

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_5.png)

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_6.png)

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_7.png)

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_8.png)

Okay， so hello everyone， we can continue with the second part。

 so now we add the tutorial or actually I think we ski this slide with the tutorial information you can there。

Okay， sorry。

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_10.png)

Okay， okay。Just click here， Okay， are you okay。

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_12.png)

Okay， yeah， sorry。So in this demo in this part we will see how we can actually build special temporalor GenN and we will use for this purpose。

 an open source library that we developed in our lab which is called Torch Special Emlar special Emper you have all the links here so we have the documentation of this website and the GiHub Ra here you have all the links in the PDF online as well as the QR code so I will leave it this year for a couple of seconds if you want to open it from the from the QR code。

Yeah， so。We will move back to， sorry。Okay。落啲诶。嗯过个关 o嗯。Okay， so。I was saying。Now， we can嗯。We can。

 we will start by。 let me attach to the okay。So in this notebook。

 what we will see is to explore all the functionalities that are available in TSL。

 so in Torch Special Emperor， which is a library relying on Pytorrch and Pytorch geometric to and also Pythrch light to ease all the research or in general to。

Okay， sorry， to foster also research and accelerate research on spatial temporal data processing using graph neural networks。

So let's start by installing all the necessary computation。Necessary dependencies。

 so here we are simply installing the environment， what I suggest you is to have an environment in which you install your torch。

 your fabric torch or end Ptorrch version and then install torch special a temporal afterwards and it will install all the other dependencies。

So now at the moment here， we are using torch version 2。5。Point1。Okay， let's just， this is just。

 let's say routine code just to import everything we will need and to have a look at which version of these dependencies we are using now。

In the meantime， let me just briefly introduce the library so。

TSL is not just a collection of layers let's say it's not just if you want something which is like easily plug and play and have all the possible implementations of SDGN online。

 it might not be the right library， what instead want to have inside is to cover entirely the pipeline from the data acquisition to the downstream task。

 so we start by loading a given data set and on the data we can apply all the usual pre processing stuff that it's needed when we want to do foring for instance。

 like scaling and normalization of the data， or all the tools for a reampmbling the data according to the given sampling rate we have and to cluster data。

This is we have strong focus also on data coming from real world。

 so all the real time series where each observation is associated with a specific time step in the real life。

Also tools to handle missing data and other irregularities and we will see something more in the second part of the tutorial and then of course we have all the parts about the modeling and inference such as all the layers that we might need and ways to build our own SDGN。

So let's start by loading a data from the ones available in TSL， we have a wide array of data sets。

 we have some data sets on traffic forecasting for traffic forecasting。

 some data sets for air quality or energy analytics。

Most of these other sets usually are widely used benchmarks in research in this special temporal data processing community。

We will start by using a MeA， which is a data set that Andrea was showing before in the tables。

 it's a traffic data set so it contains data from 207 loop detectorors in LA。

The sampling rate is five minutes and we have approximately four months of day。

So we load the data sets simply as you I think you are already used to with other libraries。

 so we import the data sets from the TSL dot data sets and we have we download it in a specific folder。

And here we can see that we have approximately 34000 time steps which corresponds to the five minutes for four months。

 we have 207 nodes so the the detectors and for each node we have just one channel which is the detected speed so the time series each time series is univaried。

Although we have 207 of them。Okay， so let's print some statistics about these data sets some information。

 so we know that the sampling rate is five minutes。

 we know that there are missing values and approximately they are at 8% of the data。

We can have in principle exogenous variables， so the data that come with exo exogenous variables and among these we have a matrix that tells us the distance among this sensor。

So this covariate here， data。covariates， it's a way to。

 let's say it's a storage for all information that we might need for our task。

 but that are not the target of our downstream task。So in this case。

 we have the distance between each of the nodes， which should be in theory the road distance from a node to each other。

We can have a look at it， so here what what these metrics is storing is basically that from the node0 to node1 and until9 we don't have any we don't have a path essentially so the distance is infinity while for instead one to two。

 we have this distance in kilometers if I'm not wrong。No， maybe， no， maybe it's less。Anyway。

 now we can print also the data set how does the data set look like， so here we have the nodes。

 these are the nodes ID， we have just one channel。And for each of these notes。

 we have an observation every five minutes， so as I was saying before。

 so we start at March and these are the readings for each of the sensor。Okay， fine。

How we can build a graph out of this because what we have now。

 these are the only information we have， so the distance between sensors and the data that we have。

We have different ways in which we can basically build a graph when we don't have let's say a given ground root graph we will see later on in the tutorial more complex way to build a new graph but here what we are what we will do is essentially to compute a similarity score between nodes given starting from their distance so will use a ocean kernel based on the distance just to convert the distance into some measure of similarity so here the closer the nodes the I would be the score so before the diagonal was zero now it's one。

Okay， this is a standard way， let's say， to build a graph based on the distance。

 it has been used in several works。Now let let's use this method instead， which is from here。

 what essentially we did is to get a similarity score out of the distance using this function。

 this is pre let's say it's embedded in the data set so each data set can implement its own similarity methods and we have some default one。

While this get connectivity instead applies all the usually done post computation to post processing to the similarity metrics to make it a real ideasymmetricsymmetric so now we want to say let's say let's threshold the data so all the data that are below 0。

1 we consider them as zero so no edge between two points we remove that loops we normalize the weight on a given axis So either the let's say incoming edges or outgoing edges and we want the layout to be edge index so this is a very Pyth geometric language let's say so here the edge index is simply a list of edges which is then stored in a。

In our dense。We can have a look at it， so we have this number of edges or 1 and 500 more or less。

With edge weight， so this is our weighted agency metricss。

Here we have some operation just to convert it back to metrics and to have also some the sparse to check that the sparse weights of these metrics are still the same edge where it as before。

Okay。Now， let's say that from now， what we had is data set， which is still not in Pytorch。

 it's mainly using nuumpy and pandas。Indeed let's say a more user friendlyly view of the data set and we can compute and do our standard analysis on that。

 but then when it comes to training a model and feeding this data to a neural model we have to switch to the Pytorarch interface and we have this special temporal data set this basically comes in our help and here what we do is to pass to this data target variable so it wants to know what is the goal of our of our task basically what we want to predict in case of forecasting the connectivity metrics。

 any covariate or exgen variable if we have them so under a was。Mentioning， for instance。

 the encoding of the time of the day that might be used here at the moment in this example we don't have any but we can add them easily the mask telling if data is available or not and then we have this parameters here window horizoniz and stride that decides how this data set is then split into Windows。

Which are then feed fed in our model so。For those who are not familiar with the sliding window approach。

 what usually is done when you have when we have a long time series is to split it in Windows and filling the model with just a small windows of data and then predict let's say what we K steps I in this case the steps side are defined with the horizoniz parameter the length of the look back window is defined with the window parameter and then we have a straight parameter that defines。

How many。How many time serbs are there between a sample and the other？Okay。

 here you have all the let's say， all the parameters that that they are that you can pass to these classes class and the effect that they will have on your time series。

So if are if there are questions， please just write them to the channel。

 I will have a look to the channel sometimes from time to time and so on。关币英语。😔，Okay， I'm sorry。

 it wasn't a previous question， okay。Okay， so。TheThis data set once we once we create this data set now we basically have splited the long time series into smaller time seriess and all of these are now different samples of our data set so now we have time this number of samples here。

And still we have the nodes and channels as before。

 each of the sample will have 12 time steps inside。And we can actually have a look at it， so yes。

 we have 12 time steps and still the same number of nodes and features。Okay， we can go we can go。

 I mean here we have these， okay， the torch data set。

 you can access the different torch data is the data set that we just built and you can access the any samples as if it as if it's list so in this case we are getting the first sample in the list。

And this this data object that is returned is basically an extension of the to geometric data object。

 but it's wrapped in our。Customom TSL data object and here what we have is the let's say we built upon the original data and here we have to inner kind of dictionaries let's say storage object where that in which we store all the inputs variable and all the target variable so here all the inputs are the ones that are given to the model and the target are one in that instead we want to we want as output you don't have to specify all this information when you build your data set you have the maximum flexibility to edit all of these but if you don't do it the most common let's say procedure is adopted so your time series is splitted and when you pass your target so the target time series becomes both the input and it's named as the x and the target and it's name as the Y。

Okay。Now。Yeah， here we can see what the input is containing， so the x。

 the index and the weight of the index， so basically the input to model are the same that we have seen in the tutorial。

 so the X and the a sym metrics。And the target is just the y， so we are not again。

 we are not predicting graphs as output but just time series graphs is just a mean that we used to process for processing in the our input。

Okay。We can in this case， we can see if we have a mask and we have it。

 so the mask is telling if data is missing or not， in this case。

 all the data that we are seeing are not missing。And we can see if there are transforms。

 so the transforms here are the scrs that we usually use to scale the data so like for normalization or applying the mean Max scalar。

 but we will see them very soon。Okay， these are other information I can skip them to， I mean。

 you can have the this tutorial has much more information just to try to explain all the steps you might be interested in。

 but the core you can we can skip some of them to。To do the core part so an important thing is that the batching of the dataset is done very efficiently by in the model and you can access actually batch of data as if it's at least as I said before so here we are getting the first five steps the first five samples in the data and so they are batched in a single object the advantage of these approach is that it comes when we are dealing with static graphs so we have the time series let's say that the connection the relationships among the time series is time series are not changing or evolving over time so we can assume that we have a single graph and so in this case we can have these advantage advantage batching where it's basically we are stacking the time series but we are not stacking the edges and so。

The graph is the same for all of them。Okay， in this part here we are basically splitting the data set into training validation and test and we are fitting our standard scr。

 so are the ones that basically is removing the mean and divided by the unit， the variance。Okay。

When we call actually DMM dot set up， we are actually。

Fitting all the all this information this Dm here， it's this passion import data module for those who are familiar with Pytorch lighting is an instance of the Pytorrch lighting data module。

Okay now let's go directly to the model part。As customer in all these libraries。

 we have all the neural network apart inside the submodule TSL points and N。

Here we have a collection of different layers that can be used， blocks， which are。

 let say implement logic which is a bit more complex than layers such as encoders or decoders for our model。

 and we have also some models that can be already used。

Now for this tutorial we will design a custom SDGN and this will be done by following the time then space parting so we are what we want to do is to embed all the temporal information in a single vector so we end up with an unattributed graph and then we do the message passing on this single graph。

We will also make use of northern beddings， so the parameters which are specific of each node and this will make our SDGN a global local model。

So let's have a look at the I hope I executed all the important cells。This is the model。

 the time then space model， so here we have the node embedding tables which has a smaller size with respect to the hidden size we are using。

 we will have the encoding step we as we said before it's just a feature encoding step so this takes us input a single time step and returns a single timeap it' applied pointwise。

Then we will have the time part， which is in this case， a group GU。

And then we will take only the last state of this GRU。

 So the encoding of the time serious and this for the special part。

 we will use the diffusion convolution， which is the operator that。

Is used also in the diffusion convolution re neural network and this yearnN and we will in the end have the single vector for each node and we will have the decod part which is done by an MLlP so the logic it's here the one that I just mentioned so we can go ahead。

We can make our own by changing the number of layers， for instance， we can say two or n layers here。

We can increase the hidden size and embedding size and the number of special message passing layers。

Okay， let's have a look at the model now。This is our model and this is the number of parameters that we have at the moment。

I also want to show you the and how the data looks like for our input so what we are giving us input to the model well。

 first of all， this is what the model see for time series so let's say we are at the node zero at sample zero。

And these are the 12 time steps we are going we are giving a input to the model and we wanted to predict the next 12 in this case。

And this is what the。This is what the a single， let's say， a local un model will usually see so。

The time series of a node has the input and its output。

What instead we can do thanks to the graph processing is to also have us input let's say。

 the set of the time series of the neighbors of the node。So here we have 11 nodes。

 so maybe it's not that easy to see something that can be helpful for the prediction。

 but what can be what's the rationale behind it is that in this time series using this time series to forecast this signal on the right。

s actually it helps， it has improvements with respect to forecasting the signal only using the signal on the left。

These this value on the on the left hand side are。Normalize as you can see from the scale。

 which is not the same for the target in this case， but for convenience。

 we also plot the scale target here。So there is a question so in the first part of the diagram we were discussing that SDGN for traffic modeling such as DC CRnN are time end space models here we implement the time then space model is the result the same please correct me if I'm misunderstood so these are different approaches different paring so these CRnN is indeed a time end space model as we briefly mentioned before but we will have a deeper discussion right after the notebook a time then space model is is faster it's more scalable and so this is also why we chose to have this one in in these demo also for efficiency purposes but also the performance of these approaches are not that's bad as one might might think compared to。

诶。Other time and space models。So in the end， we will see， I mean。

 we are not looking at something which is， let's say a very stupid network。

 it's something that theoretically can work very， very well。ok。嗯。Okay。

 are we now go to the training part。We have in these TSL。t engines。

 we have a predictor module which is a lightning module that can be used to ease all let's say the burden。

 which is that we usually have to do define the training loop and validation loop and the evaluation pipeline in general。

 we can import our the metrics that we define still in our library。

 usually the metrics that are used in traffic forecasting in forecasting in general。In the meantime。

Okay， I'm starting the training now。We won't do a very deep training。

 it's just to see also in this case that the， let's say that the burden of time then space model is not that big compared to other time series forecasting methods。

So here what the library is saying is that we are giving a s to this model， the X， so the input。

 the edge index and the weight of the edges。

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_14.png)

We can have a look at what's happening during training now。So this is the train loss。

So the model is learning something。

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_16.png)

好。But yeah， I invite you if you are interested to maybe tune some fiber parameters here and train your let's say your own SDGN on this example。



![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_18.png)

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_19.png)

Or for the sake of didally is basically everything it's I think we can simply stop here the training and。

Just do the evaluation part just to see what happens in the end。This is the， we are loading。

 let's say the best model， which of course， is not， I mean， a fully trained model at the moment。

And we are testing it the test on the same splits that are used usually in publication。

And here we can see the results， so this test my is the MEE is the absolute error in the entire horizon this at 15 means after 15 time stepss and after sorry after 15 minutes so after three time steps in the future while at 60 is after one hour so at 12 time steps in the future and all these metrics are here because we define them here。

 you can have you can have your own， you can monitor whatever you want by changing or the metrics here。

Okay， so this is pretty much it， you have the tutorial it's these demo is available in the slides and also on the website of the tutorial。

 the library is online， you have it on Giub and read the docs。If there are questions。

 I'm happy to take questions on this part and if there are not questions。

 I will move to the second part of the。Okay。So。Wch Chinese one。Okay。嗯，嗯， here。And thank you。



![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_21.png)

ok。So I hope you're still here with me in this second part of the tutorial we will see we will have a look at the challenges that are inherent to this problem setting。



![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_23.png)

So we will see we would have a look at four challenges so the first one is on scalability。

 so how we can deal with large collections of time series so we can process the data when they are when we have lots of them。

A second problem that also came into questions before。

 so what to do when we have missing observations or misaligned observations in our time series。

 and then Danielle will cover we'll cover the latent graph learning part so what to do when the underlying graph is not known so how we can learn the dependencies that are existing between the time series and also how we can evaluate these graph based models。

 how we can understand if a model is optimal or not。So let's start with the scalability part。

Scalidity is actually a feature， right， because in using graphs。

 what we can achieve is to have a single inductive model， so a global model。

While still we are conditioning on related time series in a sparse fashion。

 so we are not giving us input to the model the entire set of time series or just one of them。

 but we are using the ones that we think we think are the most relevant for the prediction of the target time series。

And in doing so， we reduce the cost of the usual operation of considering all the other time series from。

Being quadratic on the number of time series to be instead a linear of the number of these edges。

 so the dependencies that we found in the time series。

But scalability can also be an issue here because we have data that as the name suggests。

 are spanning the two dimensions。The first one being the spatial dimension。

 so the number of time series that we have， but the other one being the time dimension。

 so the number of time steps that we have in each time series。So in real words， applications。

It's not that uncommon to deal with high frequency time series and also with large scale time series so this is for instance the example in smart cities like traffic forecasting or environmental monitoring。

 for instance， if we want to monitor the air quality， the quality of the hair in open environments。

 or this can also be the case in finance where we have different prices the level of segments or even even lower。

For a large amount of in this case， of socks of entities in general。So。

Usually the problem is that we have a large amount of data and we want to process them altogether。

 and this is particularly true when we want to account for the long range independenceencies that might exist either in time or in space or in both of them。

So for instance， if we want to capture something， which is very far in time and space。

Now let's have a look at what's the actual computational complexity of STGNs。

If we consider a time end space model a general formulation。

 so there are very different implementations of time end space models。

 but in general what they do is that they need some notwise temporal processing。

And also they have a special processing that scales usually with the number of time steps。

 so if we consider， for instance， regarding neural network， a graph base regarding neural network。

 at each time step we do in this case， L layer of message passing。

So what happens is that then we scale。Let's say we need to do the number of message passing operationsions times the number of the time stepspsos input。

So the first step towards improving this scalability is given by the time then space model in which instead only the temporal processing is still done not wise。

 but then we just stepped to do the message passing on a single graph。

 so on this is let's say as if we had just a single timetamp。And in this case。

 we have an additive computational complexity， let's say。

 from with respect to edges and time steps instead of multiplicative。And as we've also seen before。

 this is not an advantage that space then time models have instead。Okay。

 this is quite a good achievement， but it might not be enough when we have very large graphs or very long range dependencies so what else can be done a possible approach。

 a possible solution could be to reduce the the computational complexity by considering some subgraphs of the full network so this can be done for instance。

 by selecting subset of target nodes and then just considering the egogo graph of the introduced subgraph on the subset of nodes up to a given order of the neighborhood。

And then just do the training on this subset， another option could be to revive the graph to reduce the number of edges since we are now not scaling with the number of nodes。

 but the edges we can have some further specificification over there。

All these methods are actually borrowed from the static graph community。We have some example。

 for instance， graphage or a drop edge or many other。

The problem is that the subseampling might break long range dependencies if we have。

 so if we consider a very small K， then we might actually lose some important dependency instead if we use a large k。

 we might instead end up with the original graph for something similar。

Another disadvantage is that the learning signal may be noisy。

 so we might have problems in optimizing our network。Another option instead is to sorry。Okay。

 another option is to instead move part of the computation before training， so high of training。

This actually helps us in removing all the burden of the heavy lifting part in something that we need to do repeatedly during training so a possible let's say an example is represented by sign。

Whi is this architecture that precomputes some representation of the neighbor of a node and then allows this allows us to sample the obtained features as if they were IAD samplesd。

These works have been done to reduce， let's say， to enable scalability on large sta graphs。

And a possible extension and extension to instead， the spatial temporalor setting is given by。

These other work as GP in which we also have a further precomput made over time。

 so instead of besides having the precomput of the node features we also the let's say this partial neighborhood feature。

 we also have an encoding vector that tells us what has been the history of each node。

 this can be done through an Estate network， this is done actually through Estate network in this work which is basically a deep RNN which an randomized weight。

And then we can after this encoding， we we propagate these encodings to the graph using powers of a given graph shift operator。

 so a function of our agency metrics。And here we have the example of the architecture just to have a visual representation of it。

 so what happens is that we go the time series go inside this big recurrent network and then gives us input of our encoding as output of a new encoding at each time step that it's propagated over space in non trainable fashion。

Now we reduce the training step。The cost of the training step。

 which is now independent of the length of the window， the number of nodes or the number of edges。

 the turning step is now simply constant basically because we can sample these features from this newly constructed data set as if they wear IID and also the performance are not actually bad so they they match set of the art。

But they have some downsides， the first one being the fact that we are now extracting more and more features。

 so the dimension of the vector， which goes inside this MLP。

 so the downstream network is now much bigger than the initial time series。And also。

 it's more reliant on cyber parameter selection as expected。

 because now many parts of the network are not trained。Finally what another option。

 another option to reduce the computational complexity is to use some coars or grain representation of the input so instead of using the time series at the initial samplinging rate for in each processing in each step of the processing。

 we can reduce this processing this resolution both in time and space actually。

 and to do it in space we can rely， for instance on graph pooling。

 which are techniques to reduce the sides of the graph by associating a subset of nodes to a super node in a new graph。

So now what we can do is to reduce the number of operations that are needed to reach the same receptive field as the original graph with a deeper network。

 but also the downside is that we are introducing bottlenecks in propagation of information now。

So for the first part on this challenge on this scalability that's sit。

 see if there are good question on this part， I'm happy to answer。I hope everything was clear。Okay。

 so。I will move to the second challenge， which is dealing with missing data。

 leaving just a couple of seconds for if there are some questions。Okay。So until now。

 we assume that we are dealing with complete sequences so that at each time step and for each node we unobered a valid observation。

This is of course， not the case in real world applications where。

Usually we have missing data that are due to different reason。

 it might be due to for faults of the sensor that can be transient or permanent so we can actually lose sensor somehow at a certain point。

 we might have a synchronicity among the time series and these results in having missing values in let's say in a not synchronous way among the time series or or any error in general。

And the problem is that most of forecasting methods do not consider this this problem they assume to deal with complete sequences。

 so what we what。What we need to do now， we need to a way to fill in these missing values to reconstruct the missing data that we have in the input。

The problem of time series imputation is precisely the problem of estimating the missing observations in a sequence of values term。

Here we can see we have the same figures before， but now some values are missing and what we added is auxiliary binary variable。

 which is this mask M， which the notes if a value is missing or available。In the end。

 what we want to do is to provide an estimate for all those values for which the mask was zero as input。

There are different data types we provide a taxonomy which is based on this conditional distribution。

 so on the distribution， on the distribution of the mask conditioned on other values of the mask。

The point missing case is the let's say the the similar to the missing completely at random case so here what we are saying is that the probability of a point being missing is the same acrossor nodes and time step and this is basically associated with the same the bernoulli with the same cost or the berulli with the same constant to each node and timest and this is something that usually models。

 let's say the communication error that we might have in a remote sensor application。

Instead in the block missing we have that this distribution is not independent from missing data at other node or time steps。

 so we might have a block of missing data if we in the case of the temporal block missing。

 so here we might have， for instance a fold that generates consecutive missing values。

 we might have partial block missing if for instance we have a breakout in a region of a MaredD or a combination of them。

When we have missing values， we need to make some adjustment when we optimize our the parameters of our model。

So here what we want to do is to compute the loss only on the validd observations that we have。

 so here the loss function that we are using in for instance the MSC is then weight by the mask just to say okay let's just take the values that are really available in our problem these loss is usually the one that is used or forecasting with missing values or to compute our reconstruction loss on the data we have when instead we do imputation sometimes we might need to inject some missing to train our model or to do evaluation of our model and so we mark some of the observations that we have us missing and then in this way we obtain ground root labels。

And of course， this data cannot be used in the model to obtain the imputations。🤧嗯。

In deep learning literature， there are different approaches to take this problem。

And one of the prominent approaches is to use outdoor aggressive models， so for instance， R inNs。

In this case， what happens is basically that we process our input sequence and as soon as we find missing values。

 we impute the missing values using the prediction coming from the recurrent neural network。

 so using the observation that we had before。So。This is effective in exploiting all the past information that we have at a single node node。

 and we can also account for future observation。 if we use， for instance， a bidirectional model。

 But if you look at this process， what you can。Easily imagine is that if we have a temporal sequence of missing values here we are continuously updating。

 let's say， providing forecast for that value using prediction from the RNN。

 so we might have compounding of errors along the temporal block。

And another downside is that this approach is struggling in capturing nonlinear space and time dependencies that might be present in the data。

So again， what we can do is to use the relational information that we have to condition the model。

 so instead of having a single model for each time series for the entire set of time series。

 we can have a model that can be applied to any time series and can take as input our graph to specify the relationship independenceencies in the data。

An example of this is represented by green， where similarly to what we did for the graph convolutional RNN for forecasting。

 we integrate the graph processing into the other aggressive approach that we have just seen。

So here in general， what we do with we do in these approaches is to model this distribution。

 so the probability of a datum given the entire sequence。Into three independent steps。

 the first two beings the information， getting the modeling the distribution。

 the condition of the information at the previous time step or subsequent time step and this is what is done typically by a bidirectional R&N。

 which is the model we saw just so before。What message passing allows us is to also consider related concurrent observations。

And this is powerful， for instance， when we have long sequences of missing values in a given node。

 but we still have information at neighboring nodes。

Imutention is usually done as a preprocessing step， as I said before， for a downstream task。

 for instance， forecasting， and this is often necessary and for the nature of the forecasting models at PM that expect complete sequences as input。

And so the pipeline that usually is done is to imputee this missingval then proceed with the forecast afterwards。

But this， of course， might introduce biases due to the errors in imputs。

Another use case instead is that of using an imputation model in place of a forecasting model so we can also simulate to we can imagine to have a longer sequence that the one we have。

 but with full of or empty， let's say from values， so full of missing values。In this case。

 we can adopt essentially also imputation methods that are not meant for this。

But this is of course a workaround， so this performance might be also the performance Mars performance might be also poor due to the absence of values。

 So if these methods， the methods that we use are strongly relying on values， for instance。

 are the boundaries， this is not a good choice。What instead we can do is to take a more direct approach where we want to avoid at all this reconstruction step and directly deal with irregular observations。

So in this case， what we do is to have a forecasting SDGN。

 which is tailored and thought to deal with values that might be incomplete。

So here we have several benefits， the first one being that we now can learn directly how to leverage only the valid observations so we don't need to to impute or to carry on some estimates of the missing values that we have。

 and this is done precisely for the downstream task that we have at hand。And also。

 we avoid the computational burden that is usually that comes with the imputation processing in the in upper prepro step。

Now after besides imputation， another important topic that can be considered is that of digital sensing。

 which is the practice of estimating unmeasur data states using the data the data in the model that we have so if we have a set of nodes we might be interesting in knowing okay what will be the observation。

 the corresponding observation at a given node that I do not have in my data。

And now here we can actually see the power of graphs in doing so because we can exploit these relational dependencies to condition estimates。

 this is actually what we do to conditioned estimates on data that are closed in some space which can be inside in this case it's the center space so that the space where time series are。

And thanks to the inductive property of message passing。

 we can enter new nodes and edges very easily。 And this is also useful where。

In applications we disensing as a cost。Now an application that we can see here is basically an example of using methods that are thought for just doing imputent to instead being applied to virtual sensing。

 so in this case we are simply adding a fictitious node in our data and without any observation and we see what happened when we want to reconstruct the to infer the corresponding and series and this is done with an graph impution model。

And the results are not bad so we can still recover something as we can see from the figure。

 but of course there are several assumptions that are needed。

 so we need a high degree of homogeneity between the sensors need to assume that we have a capability to actually reconstruct given neighbors using only neighbors and many more。

This concludes the part on the first two challenges before leaving the stage to Daniela。

 I'm here to answer any question。So if you have a question on the scalability part or the missing data part。

 I'm happy to answer。ok。I think that's it， so I will leave the floor to Daniela。Okay。

 so hello everyone， I'm Daniela Zambon， despite the name probability that you see is still andriachini。

But so here we are， so I'm going now talk about this other problem related to Latin graph learning。

 which stem for the fact that all that we have seen so far rely on the fact that we do have some relations that span or connect our time series but what if such information is not available。

 of course we can imagine scenarios where we have several time series but no relations are given to us or for example。

 we do have some time series or some relations but we do not know all of them or other scenarios could also be that the information that is given to us is not trustworthy so we cannot assume them to be reliable enough so we want to learn them from data so this in fact the possibility to learn a relations from the data that we have the time series that we have is something that holds the potential to apply。

All that we have seen so far also to other。Scenas， so。Okay， I see another。Okay， not sorry。

 This is just someone in。In a waiting room I cannot add it anyways。

 so I will move on so okay so I was saying learning these type of relations from data so here we have seen that we see that we have sometimes serious and we wanted to deploy or devise a model that is able to extract some information in different ways so what it's central to this topic is the fact that in order to really rely on graph neural networks we expect the graph that we extract from data to be in some sense a sparse so that we can rely on all all the advantages that we have seen from the previous presentations on you know low computation。

 maybe scalability linear in the number of edges and not having to scale quadraically in the number of nodes。

These somehow can also serve as a sort of regulargizers for our attention mechanism so that we can remove or drop several connection that we know or we find out that are not relevant to make our prediction better and so focus on only in the most relevant ones and lastly to introduce a still this topic I want to say that this graph instruction process can go under different terms in in the literature。

 we may find a graph structure learning as the most used one but other names can be appropriate as well so in this case I'm using Latin graph learning because it's what it's more informative for what we are doing here so we have time series that we want to extract some relations that are in some sense before the time series or underneath and conditioned。

The realizationizations of the time series that we are seeing。

So the first approach probably is the simplest one is that okay we have a set of time series。

 a collection a time series， we compute some sort of similarity between all pairs of time series so for example we can take the Pearson correlation or a grandeur causality from the time series。

 so in this sense we construct given a time series a matrix that store all these similarities and then as I was saying that we may be interested really in extracting something that is a sparse we may need to apply some sort of thresholding on top of these method and yeah so in this sense first we decide what the similarity should be for us and then from the graph that has been extracted we use our STGNs。

Or any graph based model actually。 So another approach is that instead。

Assumes that this graph is not something that we compute on top of our time series。

 but is actually something that as in some sense， determined the idization of the time series that we are that we have available so this typically relies on some sort of assumption like that of signals smoothness。

 so we say that or we assume that our signal is smooth in the sense that those time series that are more similar to each other is because they are more related to each other and they are caused by a sort of some infrastructure that is underneath our time series and by optimizing specific losses like the one that I'm showing here which is basically a total variation of the signals that we have we can try to recover the topology。

in some sense， determine our observations。 So， of course， the optimization of this type of。

Losses also usually require that we not necessarily this but in general this set of losses may require that we constrain。

 so here of course we are optimizing with respect to L in the left hand side of the equation or a on the right hand side on the left hand side we have a laplaian L as to be a lapplian so we need to constrain L or the matrix cell that we are optimizing or finding to be indeed a matrix representing a lapplian so this actually is also interesting because in some sense it allows us to enforce some sort of sparsity as well within this optimization problem。

So this family of approaches are commonly derived within the framework of graph signal processing。

 which have of course， strong guarantees in this respect。

What we are more concerned of in this presentation is that of taskoriented Latin graph learning where we are tackling this problem from a more let me say integrated way where we are learning the relations in a way that our model in the end is performing best at the task we are considering so we are optimizing a downstream taskca and possibly we want to learn it end to end so this is a difference from previous one where we basically make a first step where we train the model or we train the relations or we learn the relations and then we apply them or we consider them along with the model that we are optimizing。

So for this task oriented letter graph learning we have two main approaches。

 I'd say the first one is a more direct one where basically our matrix so the graph that we are trying to learn is modeled as a real matrix and by end where n is the number of nodes and we' are trying to optimize it order to of course as I said。

 maximize the performance of the downstream task， the second one is a probabilistic approach of learning instead of let me say a deter deterministic matrix say。

 we are learning a random variable a distributed according to some distribution P of fee that we are modeling in some way that we've seen shortly。

 and often this is done mainly allow our random variable A or our graph to be。

Nly discrete object Okay， so often we constraint PP to produce a random variables A which are binary and this is so in this set01。

So again， as I mentioned， the key challenge is probably here in this optimization framework of deep learning is to retrieve a in a way that this is in the end a sparse subjects that allows us to not only find few relations that are very relevant to the task at hand。

 but also to allow to maintain our computation sparse and therefore efficient。

 this is in particular not trivial in all those techniques that we use mainly nowadays which are based on gradient- based optimization。

So。This direct approach as I said， is not a probabilistic one is as opposed to that and where we have it is our late variable a that is here a model in some ways as a function C of some edge scores parameters p so here we can think of phi as a starting point as a three parameters in floating parameters or real numbers and by n matrix and these numbers are completed three parameters but then is the role of the。

The function C to transform these parameters in some adjacency matrix okay this could be any functions could be。

 for example， a sigmoid if we want to maintain these differentiability or we may just threshold and say all positive numbers goes to1 and the negative one goes to zero So another thing is that now I said that phi could be set or a matrix of three parameters this is only one of the possibilities could be indeed an n by matrix of parameters。

 but could also be itself a functions of other parameters or for example。

 a function of the input itself Okay so in this case a represented phi explicitly the dependency on some input data x and some other parameters till the phi。

So on top of these fee we apply this function which allows us to enforce different type of structures。

 of course the first that probably you are thinking of is making a a binary matrix。

 but these not only the only application we can make， for example。

 a a K and N graph so that we allow to design or to construct the graphs that have a bounded degree so again this could be。

 for example for scalability issues。Or some other more interesting or relevant for the application structures。

 for example， a tree direct acyclic graph or any other structure as far as we have a way to enforce some structure。

So another important or often used trick to model these edge scores and this is intended to address the fact that these edge scores are in principle quadratic in a number of notes is to。

Exploit some factorization of this fee so in this case we have that we have other two mates。

 this is a ZS and Zt so Z source node or an Zt for the target nodes so we can interpret these two metrics which are smaller in the end in terms of number of parameters。

As embeddings of the nodes and by means of these embeddings and some either directly a product or some functions of the two。

 we can extract some parameters fee and then in the end we see again produce an adjacentsymmetric matrix a So again Z of T can be。

Can be three parameters themselves or once again functions of the input data。

So there are several advantages of this approach which are mainly on the fact that this is an easy to implement a solution。

 the parameterization really infinite in the sense that all the layers that we have available in our deep learning framework are endless and also because we have all these deep learning。

Solution， they are supposed to work immediately once once applied within the optimization tools that we have available on the downsides。

 what we have is that as these are are the three parameters are in the end real numbers we usually end up with M square computational complexity that as you can imagine does not scale well if we have a large number of time series that we want to consider altogether。

 especially during training where we haven't reached a point where in the end graph can be considered a sparse entity。

 but we are considering all possible connections that may exist and in that case the computation as you can imagine is quadratic in the number of nodes。

Indeed one can okay leave the parameters to be n squared and then apply some specificification on top of that。

 but that specificification is actually cutting all the connection that are zero removing from the computational graph and in that sense the gradients cannot path through that path and so it's harder to train such type of parameterizations。

And finally， so given the two shortcomings that I just mentioned。

 this makes the parameterization requires some extra care to be able to allow to find what we are actually looking for。

So the second family of models is a probabilistic relyli on a probabilistic approach whereas I anticipated is based on creating a parametric distribution P of P and this parametric distribution is for our Egyptiansymmetric a so there are different type of parametermetricization for these P of V probably the most straightforward one is one on the left where we assume that all edges are of our graphs are independent between each other so we can model each of these edges as a Bnoulli and so in this sense we have that AIJ。

 so our variable or。Component of a digitency matrix relating to the edge IJ is simply a Bernoulli independent from all the others whose parameter is given by the sigmoid of the three parameters or edge score P IJ。

On the right hand side on the box on the right we have another family of distribution so as you can see changing this distribution allows us to rely on certain assumptions or not or force some structural prior on our on the graph that we are optimizing and so in this box on the right we have a distribution that forces graphs with a fixedes degree in this case K so basically what we have is that for each node we have this list of edge scores so P1 to PIN and based on this we computer softm so that this in some sense gives us the parameters of a categorical distribution and then from this categorical distribution we can sample without replacement K nodes and this will give us the neighborhood of。

No I。Indeed， this is a case without replacing and when exactly Knots。

 extension can be designed so that we may rely on assumptions such that we have at most Knot。

 but yeah， so these are just two examples。Okay， clearly here I talked as if fee were three parameters indeed all the parameterizations that we have seen for the direct methods such as the dependency on the input data or the possibility of a factorizing fee as the inner product or the similarity of node embeddings are still feasible and actually could be advisable in certain scenarios and here on the bottom right we also see a way to enforce。

Some dependence is on top of these parameters free from the input data so that we design a probability distribution that is conditioned on exogenous variables。

 for example， or yeah the input x itself。So learning graph distribution may not be as simple as in the other case and so we should pay some attention of what we are doing here so basically we can construct several type of losses to optimize our distribution P of fee so the first that we see on the left is basically averaging our predictions basing on all ideally all possible realization a from the distribution P so basically we are sampling an e metrics。

 we are producing a prediction x hat and then we compare like for example the mean squared error or the mean absolute error of that a single prediction with respect to the actual observation that we have here the not as x without the hat。

Then we average all the losses that we can collect and this will be our loss on the right- hand side instead we have a slightly different approach where basically we swept the expectation within the loss so that basically we are making prediction for several a and then taking the average among all the prediction and only then we assess the discrepancy with the organization that we have observed so this seems actually similar but just to let you know there are some theoretical results for the right-hand side losses that are actually more that yeah。

Would be more advisable in general to use that if you are really looking for a probabilistic model for our predictions。

 so here I just take taken the expectation this not need not to be the expectation could be other like functions like the median and so on。

So a more general approach in the spirit of a probabilistic model is probably to design a loss that is based or constructed as a discrepancy between the predictive distribution so P of theta of x so the distribution of our prediction x head as determined by our light and variable a against the distribution of our observations okay so this could be in the end this delta discrepancy or divergence could be the Kbalib could be for example the continuous rank probability score could be an energy distance so there are several of them。

 so this is just to say that there are different losses。

 but all these losses share one crucial or potential issue that is the fact that if you。

To optimize this directly with the gradient based optimization。

 we need to take the gradient of these fee parameters and these fee parameters are exactly the same parameters where we are integrating our loss our dose that comes at theedex of the expectation so there are。

 of course analytical solutions for certain scenarios or certain loss but many times these are unfeasible Monte Carlo approaches which are also allowed used may not be always feasible why because if we take a sample。

 for example， take the top left loss if we take a sample a and then we compute a loss and we want to take the gradient sec to B there is no connection in the computational graph towards B so for these reasons there are some strategies to design Monte Carlo。

one approach reliance on the parameterization trick。

 which is basically a smart way to rewrite our distribution P P of a as a function where the or actually what we are。

Rivewriting is a itself as a function of our parameters phi in another component。

 another random variable， which has its own distribution。

 this is sort of fixed so that in the end what we are our optimization goes through or inside of the function G towards the parameters fee。

 we living inside the probability distribution or the random variable epsilon here。

So this is some sense the couples the randomness originating from epsilon from the parameters that are the ones that we want to learn。

So this is March Cho and used very much in practice。

 so in the end you can see that basically the gradient with respect to phi can be ported inside the expectation with respect to the epsilon and now this time the loss function do depends on the parameters。

So this is actually practical and the only downside or the major downside that need to be pointed out is the fact that this is in the end。

 sort of needs to rely on sort of continuous relaxation in know that to have discrete or sparse a matrices。

And so yeah， this is to say that if we need a continuous relaxation。

 then the computation at least during training cannot be sparse so conversely another approach that is the one that we tackleled with Andrea relies on score function gradient and sums this type ofestims rely on this smart trick where basically we are rewriting the loss on the left hand side of this equation as the one on the right hand side and as you can see while the expectation is still taken with respect to P of p we see that the gradient now is shifted towards the right hand side and so it's taken with respect to the log likelihood of our graph So the log likelihood log P of p so this is。

嗯。This is really smart or convenient for us because that allows us to。To keep the computation parts。

 as we will see within the loss or actually through our message passing operators because a is just a sample if you are already thinking about Montecar approximation is a sample from P of P can remain discrete and sparse throughout our to the computation of our prediction x of head and also the loss。

 whereas all the gradient part is。Deferred to the computation of the log likelihood or actually the differentiation of the log likelihood so while the first evident disadvantage of these score function gradients in S this is evident if you ever try to use any of them is the fact that they often bring up a lot of variances in our gradients。

 so that this in the end can slow down the training curves quite a bit。

 however for many applications， there are variance reduction techniques。

 this could be general purpose ones or more specific to tailored to to the use case that you have at hand like in our case。

 but yeah what already mentioned is that the most interesting part is the fact that the computation can remain sparse。

So and as you can see in both two scenarios， so with this pathwaywise approximation or using the reitization trick or the score function formulation。

 we have that we can take samples either from Epsilon or directly from our Egyptiansymmetric say and then then take the gradient after having sampled one or more。

Samples。So this slide is to show that indeed there is some computational advantage。

 so with this these are we are here comparing score function gradient symmetry or actually the training。

Time or GPU memory during training for methods based on score function。

 ready estimator or method based on the rametization trick。

So because we are decoupling the loss from the likelihood and the gradient is taken only with respect to the gradient。

 the message passing operation， which are the ones that would greatly benefit from sparse graphs are those。

嗯。Are actually those that allows us to maintain the memory footprint under control as well as the training time。

 so you seek it easily with a number of nodes growing we reach easily out of memory issues here。Yeah。

 so。Again， this is to stress that there are some setbacks on using these。

 but there are also several solution might be you may need to devise your own solution for specific cases。

 but there are solutions for these high variance setbacks。

So lastly on these topics what I wanted to stress is that okay we have used probabilistic models the main purpose so far was just to obtain some discrete or sparse graphs out of these learning process but still we have a probabilistic models so why don't we look at the edge probabilities that we learn and see if these actually can be insightful for us for our application and so to exc some relevant information out of them of course this is not necessarily the case in all scenarios because the training and the learning process be well pose but anyways if we are able to。

To obtain such edge probabilities， indeed this provides valuable insights for explainability purpose and also in the end on top of that also achieve better informed decision making。

One interesting issue in this setup is that okay graphs in this case are Latin variables so and as such in real data we don't have any observation about them so this makes extremely hard for us to evaluate whether the edge probabilities that we are extracting from data throughout our end to end learning process are actually meaningful in any way。

 so this is an inherent problem so we it's really harder to think of our dataset set where we have such ground truth in order to assess whether the learned probabilities are actually calibrated to the true hidden ones so what we can do is of course visualized and see if expert can confirm that this is meaningful or reasonable but what we decided to。

Study more in detail is learning guarantees that allows us to some way bypass this problem。

 so we study from a theoretical perspective， whether we are able to in some ways to learn the Latin distribution and learning in a way that is calibrated to to one unknown model that existed。

 so what we find out is that if we are able to minimize a certain losses。

 losses that allows us to say that that are equal to zero only if the two distributions that we see here are actually identical。

 if we are able to minimize them， what we are saying is that we are perfectly matching the distribution of the output from the data generating process so is matched by the distribution that is output。

from our predictive model okay so if this happens what we can say is that under certain assumption this is not by default and is not in general true。

 but under some assumption what we have is we know that also the probability distribution associated with the la and variable needs to be equal to the true one that generated the observed data so while this is not true in general for graphs that are stronger actually not stronger but milder these conditions for which these results hold are much milder so this is reassuring that indeed operating with graphs and in such in this way be could provide interesting and reliable edge probabilities。

So yeah， so this concludes the related to Latin graph learning and so if you have any question at this point。

 I'm happy to take them。Otherwise， I will move on to the last challenge。

 So yeah there is one question I have two question actually assume we have a prior graph created by an experter in a field。

Good Latin graph create a better graph compared to such prior graph。 This is the first question。

 The second question is， in case of prior knowledge is required， Ex of physical system。

 Can we in the prior graph knowledge into the learning pipeline， Okay， so yeah。

 these are two extremely relevant questions， So regarding the first one。

If they relate or not to each other depends， first of all。

 on the approach that you followed while extracting this graph so for example。

 if you are computing this graph as a pearson correlation of the time series what you have is the pearson correlation between the time series so a measure or estimate of the linear correlation of of all pairs of time series if you're following the last approach based on task oriented the optimization。

 so basically optimizing the downstream task what we what we will obtain is simply the best the best graph at least according to the optimization procedure that we devised to optimize the downstream performance so in this sense is what Andrea was calling a laangeer in the sense that is。

Really oriented to optimizing the final performance of the model so。

Depending on the graph that you are giving or you are considering as a prior。

 this may or may not match， so typically you can reinterpret the relations that are extracted as would disre。

 help me make a better prediction to that time series or not。

And then enforcing some sort of sparsity prior where we try to maintain the degree of each node the small。

 we are trying to promote the solution that look only at the most relevant。Relations。Yeah。

 so this is for the first question regarding the second one in or prayer knowledge is required can we yeah so there are different ways so that the short answers is yes。

 as far as you can design a probability distribution for example， P of P that reflects that prior。

 this could be for example， if you know that you have several physical sensors deployed in an environment and you know that sensor too far apart in this physical environment have no relation at all。

 then you can enforce it in your probability distribution by simply setting to zero those parameters this of course extend a little bit beyond that for example。

 if you know that the graph that you're looking for should be a sort of tree。

or a truly like then this is trick to parameterize but still in principle it is possible so for example in our case of Kn N grass or our and also of others is not always easy to do that or actually it is easy to design is not necessarily easy to optimize for example for score function gradient dei what you have as well to do is also to take the gradient of the log likelihood okay so but the log likelihood of random extraction of k nodes without replacement as is not so easy to write it down and so to optimize analytically so in our case we relied on smart optimization from other people that find a numerical approximation of that and turn out to work well。

But so depending on the prior that you want to enforce， this can lead to easier hard solutions。

Or parametermettizations。诶ok。So。Oh， thank you。So， okay， I think that if there are no other questions。

 I can move on to the last part， which is model quality assessment。

 This last part address the point that， okay， we have all these models。 We have several。

Okay so we have all these models， we have our raw data。

 we have our prediction now the last question of course is always through okay but how good my model my model is indeed our losses already provides us as a metric of goodness of fit but it's interesting to see that as the number of time series grows。

 the length of this time series grows， then we may have also intricate interdependencies among this time series can make a single number out of our training process a little bit restrictive to understand what is going on and if everything is working as expected。

 so beyond asking whether our model is good， we also want to ask whether our model is optimal and where if there are regions or aspects。

 for example， the temporal processing or，The special processing that would benefit from some further designing improvements。

And lastly， of course the question is okay， now that I identified where the model may need improvements now I need solutions or guidelines to act in order to improve to make these improvements indeed here I spoke about optimality optimality is in general depends on the criteria that you choose could be for example as I said。

 optimizing the a predictive loss like mean absolute error， but needs not to be like that。

So we will see why and how relational information also within the task of assessing the quality of our models can be relevant or actually the solution to make this process feasible so a typical scenario that we are all facing is that we decided whether model f is better than model FB based on the performance that we see on the test set and then we say that model a is better than model FB if performance that we see from one model is statistically better than the other model and we say that a model A is in general within a family at least of model is the optimal one if there are no other model that is better than that so this is pretty standard and what we may encounter often is that okay say that I have this model f。

I may consider it good enough or actually may say it seems working appropriately but I hope to make a better model how can I say whether this model is the optimal one or I can move forward or start working on it even further order to improve it and find a better model so this is a question that is hard to answer because either we still we're still in the process of changing our models and finding a new one and until we find an actually better one but if we don't find it we still don't know whether we should look for again for it or not so unless we have of course some prior knowledge of the for example the level of noise that we have on our observed data that of course it somehow sets the baseline that we or yeah the baseline won to reach。

Another interesting approach to model model quality assessment and this is why I introduced the previous slide is that of studying correlation among the time series studying correlations is a way understand whether in our predictions or actually in the data that we are trying to model there is information that we were not able to extract from data。

 in fact here consider these prediction residual so the difference from our prediction and the true or the actual observation from the monitor environment this difference allows us to understand better whether there is optimality or not of our model so in particular if we see that there is some dependencies among the residuals it means that there is a structural information left in the data in our residuals that our。

Model was not able to extract， so in this sense， there is a margin for even further improving our model because there are structural information in that data。

So receiving correlation analysis that sense is independent from the type of performance metrics that we are interesting in。

 of course does not tell us how much I can improve。

 just say look there are something some data that is correlated so probably you should you could make it better but doesn't tell you how much。

 but in the end this is also interesting because it doesn't need to be a comparative analysis。

 I don't need two terms and say okay looking at those two I can say the which one is better it's an absolute answer in some sense。

So here we're talking about correlations， most of the research so far have focused on serial correlation。

 so correlation along the temporal dimension but there are also works that study correlation in the special dimension here we are I'm presenting an approach that is special temporal。

 so address space at time at the space and time at the same time and so these works as follow basically from our data we have that at every time step we have our graphs and ourserv so we can construct these special temporal graph by connecting not only the engines along the special dimension but also the temporal dimension and by looking at single statistics of correlation and this is single sta or design functions that you're seeing here so the sign of the inner product of the residuals。

This is a probably the simple simplest way to check whether there is other direct or indirect correlation between the two residuals so well by making these averages of all design we are designing。

Pretty simple and straightforward test that allows us to make to understand whether there is some correlation left in the data。

 Well， these are spliting to two part because one addresses partial correlation So the one in red and the other addresses only temporal correlation So here we have these ws which are the weights which are basically known zero for all the edges of the graphs and can set a sort of influence or sort of importance of different edges。

 so the most important part here is that while correlation in principle so while studying correlation in principle we should like look at all possible correlations that are existing among the residuals So grow square whether athlete both in time in number of nodes here we are only focusing on the most relevant ones which are the most the。

All the possible connections that are more likely to lead to correlation so this is why again graphs allows us to design a statistical tests that are statistically powerful although the data dimensionality grows a lot so this in the end thanks to the fact that we have a sign leads us to distribution free statistical test and also these scales linearly in the number of nodes okay so this is a test that can be applied globally a graph level considering space and time altogether can be split looking only a special or temporal at once。

 but it can also go into more finer detail by looking at single nodes or for example single time step or even more localized both in space and time。

So this concludes actually the parts where we are addressing many challenges that have been not resolved but were substantial research have been carried out and now I would move on very quickly on future direction that we see as interesting relevant for future research。

 so the first and foremost is probably this about hierarchical modeling where as we have seen so far the SDGNNs or the models that we have considered so far look at the data as they are so with the same fine grainined scale with which data comes but also considered in。

Higher order dependencies by creating hierarchy instructions like pooling of nodes or even along the temporal dimension。

 this allows us to consider both coarser grain scales。

 but also at the same times reach farther apart information within our data。And in the end。

 also it's it's from application standpoints is also important insert certain scenario to produce forecastings that are at an aggregated level。

 I think， for example that。Power load forecasting we may not be interesting at a single household predictions but maybe a more aggregated level。

 and in the end also allows us as mentioned here to reconcise all these different predictions made a different level in order to make a more reliable predictor。

Another interesting approach is that of considering state space models。

 these state space models as the intrinsic advantage of being in some sense Markovviian in the sense that observations or predictions are made only from the current state of our system and this system is updated from the previous state at every time step and could be possibly also driven from an input graph。

 so this is decoupling all the three dimensions， input states and outputs allows us to have different type of graphs with different relations that can allow us to either in the former setup of a hierarchical modeling or for other reasons to have different graphs for different purposes and this is also amenable for the reason that this allows us to make prediction only when needed。

If you needed to prolong the state update， we can do it if we observe， for example。

 a certain point observation from the model system， so true observation of Y of T。

 we can also feed back in the same way as Kman filter operates for linear system。

 we can feed back this information back to our system and update and improve our state estimates and repproagate forward with an improved modization of the system。

Another relevant scenario is that of inductive learning where several issues can occur to our monitored environment。

 for example， we can see changes in toppologies， we can see new node be added to our network for each for example know we don't have node embeddings yet。

 so we need to make some。adadaptation in order to apply our model to these new settings or even transfer the entire model to a new network。

 this is useful not only for forecasting， but all sorts of application related to this type of data and of course。

 as we change the data that we are dealing with this can also in in performance degradation that is what we are trying to to maintain as expected。

So lastly， benchmarking is another topic that needs to be addressed in the future。

 at least according to us， we have some large data sets， open data sets。

 these spans energy and traffic flows mainly where of this type of models have been designed for since the beginning。

 but this is of course doesn't provide yet comprehensive benchmarking environment。

 at least in my opinion， we also have some software like towardsr special temp that Andrea and even have designed but also basic TS are useful resources in order to try to standardize model implementation evaluation。

 cross validation and so on and so forth， but yeah we are not。Cosees。Close to what， for example。

 Open graphph benchmark is now。So all in all to conclude what we have seen is a framework for modeling time series。

 and this combine the deep learning for time series and deep learning for graphs。

This combination of the two is turned out to be extremely fruitful in several applications。

 doesn't need to be always like so， but we have seen that this inclusion of relational biasy on one model was really a game changer in several tasks。

And this not only allows us to improve performance， but also more at a fine level。

 we have a possibility to share parameters， so we have to better ratio between training data and model complexity and also overcoming all those issues that may be present in this data。

 for example， missing data or other irregularities in our data。Finally。

 I feel or we fail to suggest global local models as a safe starting point for modeling this application。

 producing data similar to the ones that we have described to you so far。

So of course we have discussed the challenges I also want lastly to point out to our tutorial paper。

 which is the one written here at the bottom and also the library toch a special T both of them I hope you find it useful we are pretty proud of both of them and if you have any feedback on them also please write us a to us so I hope in the end to have。

诶。Trev brought to you some interested in these topics in all these models and also these solutions。

 I think there is huge room for exchange of ideas with a related fields and so please reach out to us anytime if you have ideas that you want to talk to us too about and here I leave you with a link of our group graphraph machine learning group in Lugano。

 Switzerland where you can find also a list of publication。

 all the materials of these tutorial and of course also our contact information。

So at last don't forget to fill in the feedback form for the tutorial that you might find on the Slack right and probably also we can post it here in the chat so thank you everyone if you have still any question please ask I'm free to address them。



![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_25.png)

The best they can。Oh sunra， here so。Thank。Okay， yes， so the feedback form is in the chat right now。

There's a question that came up in the Q&A if you want to take a look。And the Q onsl， you mean？

On the Zoom Q&A， I can read it out loud and paste it in the chat。

The question is in scenarios where multiple time series have irregular time intervals。

 some occurring more frequently than others， what are the recommended approaches for data representation and graph neural network architecture to effectively learn interdependencies among these time series？

😊，Okay， I you want to answer。Okay， so very much depend maybe you have other takes on it very much depends on the level of the scale。

 if you will， of these that you require your predictions to be， for example。

 if you really need fine grain predictions than it's probably easier to some sort create some sort of upscaling of the less frequent time series。

 but these need not to be the case because we have seen， for example。

 when you're trying to combining。Sa covariates or exogenous variables like those coming from the weather or you want to encode in a traffic prediction model。

 maybe the hour so to identify peak hours but maybe minutes are not so relevant so in the end what you just bring in is just an upscaling of that information and use it to make your predictions in in that sense is not necessarily bit of a problem。

 of course， if this each time series comes is less regular so produces observation asynchronously or very regular time steps then there are models that probably are more appropriate to them to address them could be for example continuous time models where they try to address this irregularities so they take data as they can and then they propagate。

Aong the temporal dimension and but they don need to have such a regular information coming all the time this is I'm assuming the time series are not on the same sampling rate right Yeah yeah I pretty much agree so like if you have some preprocessing that you can do to to deal with the irregularity and this preprocessing can also be something as complex as like fitting some imputation model or some rescaling or well or as simple as an up samplingling if it's just a matter of having like a different frequencies of the observation then it's fine if it's something more structural that the observations are by nature like intermittent or something like that then continuous time models are the way to go for example so in traffic forecasting the data that we that we have is's actually。

The passage of a car under a certain sensor I don't know how to call it and so you have yeah in principle a synchronous observation but in the end of the data that we have available in the collapsal information within mostly would say if you have five minutes five minutes windows yeah five minutes windows and then they count how many cars have passeda through that lane within five minutes so in the end this is another way to actually downsample the data that you have depends a lot on the application I would say but on the other spectrum if we are trying to deal with a scenario more similar to temporal graphs then probably those techniques are not the most appropriate。

And then continuous time models， I think have an edge。Awesome， there's also one more question。

 but I'm not sure if it has already been answered， I put it in the chat again they say when can it be beneficial to learn an adjacency matrix that depends on time as well for example we drop or create new edges between time steps or there works that do that。

Okay。Yeah I think like the NI paper the neural relational inference paper that was cited in the slides and was the graph learning approach based on the parameter transition trick in that one the graph that you're learning is conditioned on each window of observation so it is dynamic it like the actual output of the model is conditioned on the observations at specific time steps。

Regarding like when these can be beneficial again， it's really like problem dependent。

 like if you have like a physical system where like deposit position of sensors， for instance。

 changes over time， then having something like that would be really I mean。

 DDDD only thing that would make sense to consider to learn this kind of depend is if instead what you are modeling is something more static。

 then might be okay to just learn something which is static another I think important dimension of this is how fast these changes in the topologies are happening because if changes are happening slowly over time。

 then what you could do is something similar to conin learning where the graph that you learn is static。

 but you also have some mechanism to have it like adapt。

But so that after a while you can check whether that kind of topology is still valid still giving you adequate performance and if not you can update it and yeah。

Awesome， any final questions from the audience？All right， if not。

 thank you all for the fantastic tutorial， we really appreciate the time and care you've clearly put into this into making this tutorial so accessible and informative。

 please feel free everyone to continue discussions in the tutorial Disc channel or by directly contacting the organizers as a reminder the tutorial feedback form is linked in a chat we really appreciate any feedback that you may have and we will also share this feedback with the organizers themselves。

😊，So that brings us to the end of this first part of today's conference。

 please join us later for a keynote from Zavia Breen and oral presentation Thank you very much have a great rest of your day。

Thank you， thank you， thank you all by。

![](img/ff9d476a8b35c0eb1dc5acf292f1d5c2_27.png)