# 图机器学习会议 ｜ Learning On Graphs Conference 2024 p06 P06_Day_3__Part_2_2-Xavier_Bresson_keynote__oral_presentations -BV1k9pAzpE8S_p6-

So they have logic they have also limited logical reasoning。 so for example， there is Terry Taow。

 which is who is like a very strong mathematician and I try you know the last version of GT and you say basically oh they are actually very limited again you need to you need to give them a lot of precise pump to make it work so they have a limited logical reasoning so what open AI try recently to do is to improve that by learning you know。

😊。

![](img/c4b0d8eef1833c42d9a658dd61469cc2_1.png)

Chain of third for example and also for inference to do search algorithms so it improves this limitation。

 but it's still not there there are also limited graphs and in capabilitiesbilities。

 even again if they have seen the test set of the graph task during training。For GNNs。

 I think strength is basically to be able so if you have a graph like this and you have a question。

 for example， is the Monaisa in the same city as Alice's friend Bob， so you have here Monaisa。

 you have the city， you have Els， you have Bob so。😊。

If you do multiple layers of GNN what you will do you will basically learn a multih path that will go through the solution of your task so they are very they are very you know good to do that and there are very effective for many different modities you know now we're talking about text but they are also very good for for example。

 know physics biology comingial optimization and also chemistry and we know that chemistry that was a very good year for AI so there was a nobel price in chemistry for Al For and Al For as you know is a niche transformer so predicting the pairwise distances between residue amino acid sequences so this is a graph neural network basically limitations for GNNs is basically they like graph foundation model in the scale of natural language processing and computer vision of course the community as well a lot on that it's very interesting to push in this direction but。

You know there is this emergent property basically means that we need to go beyond the scale of training data and compute to get something very powerful so we are not yet there the problem is basically know the data set we don't have like large data set variable OGB is still comparatively small compared to INe which has 150 gigabyte of images。

 the hardware for running spa in algebra is not optimized it's much slower than standardout dance operations existing pre2GnNs because basically of that they are small they are not doing you。

Billions of parameters this is basically millions of parameters and I think also something which is today which is an limitation is that industry has not yet found some interesting application of GNN because industry is really driven you know the AI research and AI product why we have GPT today because industry got interesting indeed deep learning and then also to develop you know product so it's not yet clear you know how to make profitable stuff of GNs it will come I think but it's not yet there。

So combining anLM and GNNs basically is it means developing a joint training a joint text and graph foundation model this is of course a very attractive idea。

 very promising idea but today I think the issue is that there is a very huge imbalance between the knowledge coming from text from LLMs and the knowledge coming from GNN so of course what we would like to do is to do some discount kind you know of architecture where we have the text then text will go through an LLM it will process it it will give us some vectors the same also for graph it will go to the GNN will process it we give us some vector and then the vectors will be we be。

😊，I'm sorry we be processed together with self attention or cost attention and we need for example。

 we do text generation so the fact that you know we have a huge difference between these two domains。

 I think this is this is very challenging。So what it means it means basically that we need to tailor you know the combination of LLMs and GNNs to get some value of that。

 so for example what we can do we can use the LLM using the vast knowledge of LLM and then try to improve the performance of small scale text attributed。

Graphs and we can also do the reverse one that you know we can use a knowledge graph， for example。

 to constrain the LLM to give more precise response okay so reducing hallucations this way。

So this is what we will do next， so we will give you two walks that we have done and this is really focusing on text reing task so the first work will be we will use LLMs to enhance GNN reing so this is a work basically that is taking LLM reing abilities to improve GNN predictions。

😊，And and it's pretty actually effective and robust the second paper we will review is basically a GNN withNNs LNM reing。

 so this is a foundation work where we try to put together all the benefits of LLMs GNNs and also something that we call graphHag that I will explain。

😊，Okay， so let's see first the first technique here。

 so the technique is called tape so here the idea would be to use LLM knowledge to improve the quality of the node features in a tag okay so if we have better node feature then we would be able to predict you know with more accuracy。

😊，With higher accuracy， so the question is how do we extract again。

 information from an LLM for a specific tag task？😊，So to do that。

 we are going to prompt theLLM because。LLM again， accumulated so much knowledge that what we would like to do is to prompt the prediction of the LLM。

 but the same we also want to understand its reasoning。Okay so we will ask the so given for example。

 an article the article will be the title the abstract we would ask the poem question so basically predicted the class but the same time we will ask the LLM to give us you know its reasoning so why did you did you decide for this for these predictions okay。

So we could also reasoning an explanation if you want， okay？

So now that we have you know the sequence of words for abstract title or explanation prediction。

 this is not something that we can directly use with GNS so what we need to do basically is to have a mapping from sequence to vector okay so we want to take this input sequence of wordss and then output a D dimensional vector that will summarize this information and that will improve basically the。

The excosivity of this node feature， so remember that in this example a node is an article okay。

 this is another article and then what we want is to predict if there is a relationship。😊，And。

 and what if you want to predict is， you know the class。😊，Okay。So what we propose。

 we propose something that is going to leverage both Opre and open source LLMs。

This technique is a kind of integrator between a close LLM。And an open LLM。Okay。

 so the closed LLM can be GT that can be Gi so we know that this disclosed this proprietary LLMs actually are better than the open ones Okay so we can go on the little bulb of LLMs and you see that always the top two G and Gi okay so the unfortunately for researcher proprietary proprietaryry LLMs they are better than the open ones but the problem is the closed LLMs is basically that they only provide sequence of words right they don't provide the vectors that we want to train the GN so in contrast if you look at the open source LLM like Lama。

Or Gma basically they are going to provide the text but also the vectors right so we have access to everything inside the architecture。

 we have the hidden vectors， the hidden features， but also you know the output everything is given to us。

So what we decided to do in 2022 so at that time GPT 3。

5 was the best closed LLM so this is the one that we used so we have our article so this is the node I in the graph given the title of the abstract we query we get to the explanation we get the prediction and then what we do is that we are going to convert the sequence of for example of what in the prediction by using。

A language model， also like a small one， which is the beta in this situation。

 it has 129 million parameters。And here let me zoom in so what we do is basically so we have our sequence of what token so this is basically the explanation if you want and here we have。

 you know in any transformer architecture you can have a class token basically something that will summarize the sequence of well so we give us an input these we go through transformer layers we output would put and then we get the class token after L number of transformer then we go through an MLLP or small MLLP to fine tune on the training set okay so the training set we know the correct class so we want basically to finet the MLLP on the correct class of the training set so in the MLP here it's a small one only two layers。

 the first layer are basically we just be some features and then it will go through another layers to get the number of classes so for example that can be 47 if it is good。

4 if it is OGB archiveAU。So this guy here is going to be a vector of seven seven all dimensions。

 so this is actually going to be our enrich feature so this feature will represent the input sequence that we have here Okay and this is very tailored to the。

To the task that you want to solve okay， so this way we are able to get enrich rich feature for the explanations and which feature for you know title abstract and then we can also have a prediction feature。

Okay so what we should do if we are in 2024， actually we should change you know the smaller the beta language model by now using a large language model okay so why we can do that at the university is basically if you take for example Lama 2 or Gma you can fine tune them using this very nice technique of Loha okay so law rank fine tuning and basically with my small GPUs actually I'm unable to fine tune you know a large language model like Lama2 so this is very great okay so basically what we would do we just need to change the proprietary tree LLM with the best one so you take the one that you like and then here instead of using the beta you can use Lama Lama 2 for example。

😊，Okay， so once you have your enriched node features。

 what you're going to do you are going to train your GNN okay with this new node features so you can use your favorite GNN okay。

 and then you can make the prediction。So we can compare the quality of Note feature now。

 so we are going to see shadow feature language model and large language model features okay so the shadow features so if you look at OGB data set so they have already designed some nice encrafted features for each data set。

 for example this skigram for OGB archive。And what happened is that you get 70% accuracy on the test set in only four minutes。

 okay， so this is very fast and I think this is a very good baseline for model performance。Now。

 as I said before， the state of the art is Glen and Gm was training simultaneously a language model okay and also a GNN and basically this model got the best accuracy of 76。

5% but of course because you need to train you know your language model。

 this is going to take more time so it's going to take 9。

2 hours okay so there is here at that time before the introduction of JGPT that was really a huge tradeoff between you if you want to increase your accuracy you need to pay the price of you know much more computational you know hardware but also training time。

So now this is what we suggested， basically we use this LLM we pump them。

 we translate into vectors and then we fine tune so accuracy was 75 Poson and only tris to do that so interestingly once we published the paper so we got at the top one of the leader ball and then there was other techniques that used the same approach of course a little better and then now the top even today actually I was surprised I look at that recently so even today the top three models for OGB archive are basically based on this type technique。

Okay， so of course， one review two say that oh we cannot trust your results because OGB archive is part of the LLM training data set so we cannot trust you so what we did basically is that we produce a new OGB archive not a GB but an archive data set we could it tape archive 23 so it's available to download we have 77000 papers same number of classes and basically we have the same conclusion there is nothing that changes when we do that and again this is also the reason that and then an LLM is even if has seen you know the test set is not able to reason very strongly with G structure so you still need a GNN you to use the topological relationship to make good prediction。

We did some applicationss study what we observe is that there is no specific feature which is better than the others。

 it's actually the combination which is important。😊。

So we term conclusion for this work so basically we can use the LLM knowledge and its reasoning abilities to enhance the node features of the tag and also the make it to fine tune to the specific to the specific task okay so what we did is something like was is not end to end so we don't run everything together it's actually we first generate good features。

And then we train the G。 So this is， I think， along the。

The trend now of LLM so basic LLM is not trend and to when they are like four steps sub supervised fine tu name reward reward model and finally。

A reinforceinment learning okay so so A each step is done independently because each step is very clear to do it to train and I think you know it's very stable and this is exactly the same conclusion that we have here。

😊，We can make it very stable and it's efficient everything which is stable we have better performance usually okay and also something interesting is that we can we can leverage both actually proprietary tree and open source LLMs so we have the best of the two worlds。

😊，It's also， of course， yeah， so some people will like it。

 it's interpretable because you can see the reasoning world of the NLM。😊。

Okay so now let me go to the Suig technique that I want to introduce。

 this is called G retrievetriver and the idea is as I told you before is that so LLM is very powerful knows everything but the poly is going to make errors because we never have a good pump in some sense。

 so we need to constrain to regularize you know the LLM response into a much smaller space。😊。

And and to do that actually we are going to use you know a tag a text activity graph like your knowledge graph and it will force the LLM basically to to answer related to this to this tag okay so the key question is of course how do we extract pertinent information from you know。

A graph and to force the LLM to be more focused。Okay so to do that we are going to use the tokens so I think now everybody understand that so because of the transformal architecture what is very nice is to walk everything as an input right so the input of your NM can be of course your query token but it can also be other information like visual visual tokens and also graph token so this is what we're going to do here we are going to use two kinds of tokens so the first one would be and I would explain that in the in the following slides a graph and code token which is going to be here and then a textbased token which is going to be here okay。

So the graph encode token it is something very natural for example if you do molecular science。

 you want to represent your graph as one vector and then you use this one vector to make a prediction for the property that you want so here this is the same idea so we can select any ferator GNN we apply multipleip graph learning layers to compute very deep node hidden feature you compute then the mean over on the node and then you apply an MP a small MP on that so this way you will have a graph encode token that summarize your topological graph and also the feature on your graph and with one vector okay so this is going to be this guy of d dimensions。

The other thing， of course， is that an LLM。😊，Wants to use word tokens right so the information the way it processes information is by using words so we need to you want to have access to tap you know to the knowledge of LLMs we basically need to transform the graph and its feature into a sequence of natural language tokens this is important to do that and so for example here you have a graph and you can represent this graph by some textual or representation for example a graph g is a set of directed edges defined by by G we are node i points to node G so here the graph g is defined with edges04 16 and so on okay so you have a one to one mapping between this mathematical representation of the graph and this text representation of the graph。

However， there are many ways to use language to represent graph so for example in this paper they have this nice example to show oh you have you know mini representation so the text based representation of a graph is not unique okay so is this can be an issue the other one is also what I say is not text equival okay it's not text equivalent in the sense that if you change you know this guy by this guy you will have probabilityffer results okay so we want to have some kind of equivals for the text representation but we don't have it。

The other thing is a scalability issue when you won't represent a graph with text okay so if you take an open source LLMs。

 the context window is limited right so for example if you use the one that we use in academia Lamma2 so basically this is 4000 tokens of limitation so if your graph is small no problem but if your graph is for example the Wikipedia graph then this is like a huge number you of nodes and huge number of edges so this is not something that you can do there is a scalability issue。

Of course LLMs are prone to eucation， so for example here we have an example that an LLM can produce nodes and edges that do not exist actually in the knowledge graph that we use so in the vocabulary of the graph you know we have some for example here the one G doesn't exist and is able still you know it's part of the vocabulary so the LLM can give you actually this entity here which doesn't exist。

So what we propose is basically， so first we are going to apply a graph hug that I'm going to explain how we do that to retrieve a subgraph from possibly a large text at which graphph。

 which is going to be relevant to the query of the user。Okay step two。

 we are going to concatenate the user query， but also the graph ending token and the text based graph tokens to create the input sequence of the LLM okay then step 3 the NLM will give us we generate an answer step four we will train everything so we will simultaneouslyly train the GNN parameters and also we fine tune the LLM parameters using Lo harm and the good thing is that you see if you use Lo harm you only using 0。

5% of 7 billion parameters so it's only 35 million and the GNN has something like 5 million so it's not that much actually to train so this is something we can do in academia。

So the graph hug that we propose so this is a graph retrieval augmented generation so H is today very popular here we want to extend to graph the main problem is of course the scalability so I'm going to tell you how how we solve this problem so。

😊，Yeah， let me maybe go here。 So the first thing is to do indexing。

 So what we do is that we're going to have a text attribute graph。 So we're going to take。

The node feature also this is a text node feature we have also the edge feature of the same。

 so we apply a pretrain and frozen large language model or small language models so at that time we just use a small language model but but we can use a large language model now so you do that and you get some D dimensional representation of all the nodes and of theH and you store them in in a database in a graph database so today we are lucky there are many open graph databases that that can be used so for example。

 pg DgL， but also Lama index also there is Microsoft and Nebula graph they are also some pop3 graph database like Neo4j。

Okay so we do that so we take this and we have Victor representation of the node and the edges okay the second thing is that we are going to do retrieval so given a query from the user we will represent to the query for example what is the name of justin BL border so we would use the same you know language model to represent the query as we did for the node and the edges okay and then what we will do is basically we just do a similarity metric evaluation so here we just use cosine metric and we can retrieve this way the top key notes and edges from the graph database okay so we would get something like this which is a noisy I would say subgraph。

😊，And then the next step we basically to extract just you know a smaller a smaller graph which has the mostly event formation and that's it okay so the way we do that we are going to solve the price collectingstein or tree okay so the price collectingsteinina tree is basically a tree so if you start from this original graph and then each node as a price so the higher other price。

 the more important you want to be in the tree in the final tree so you can solve you know you want to maximize the price。

 but also you don't want to take everything so you are going to have a penalty with the cost of the solution that you want so usually this is you don't want to or you should minus I sorry it should minus here so you don't want to take too much nodes so。

Basically this is the number of nodes that you have here okay so when you when you solve this comm optimization problem which is NP but you can have an approximate solution using semidefinite programming SDP you will get something like that okay so this is a directed tree and you see that a directed tree usually has some kind of Ho node any flowout for example and of course it wants to take the larger node okay so this is the original Steer。

inorine we can modify it because here there was only the node。

 but of course when we do graph learning there is also the edge feature that we want to use so we can incorporate we can easily incorporate edge information okay so we have a prize also for the edge so you see this is for example an age which is more important than this age here and we just you know modify a little bit the communatto optimization problem here and we can solve it you know using very fast technique so this is very the approximation is is a mostly linear al approximation。

Okay， so if we compare the standard hug with the graph hug。

 so the standard hug basically you will have your knowledge database and then you will extract you know the number of relevant document that you want。

😊，For the graph hug here， so we have also a graph database and we are going to extract a much smaller but very relevant graph related to the query of the user okay。

😊，So now I'm coming back to the tokens of the input LLM so the first one again this is the graph angle do tokens already talked about this。

 so this is you know an MP on the mean the the node hidden feature or the last layer of of the G and is's going to be here。

 so this is something of course if you have longable parameter here。

 everything can be changed you know by back propagation。😊，So for the text base。

 basically we will have two sequence of input worlds， so the first one is the query of the user。

 which is the name of Justin Biber on broader。And the second one will be the textualization of the class。

 okay， so this is。This is something again that is important for。😊，To use the to tap the LLM aity。

Okay， so we do that so we have the graphical presentation。

 it will go through the text andbe so any LLM as this first layer that is's doing to do the word embedding okay。

 so here I took an embedding and then it will go through you know the LLM。So the training。

 the training is very standard， so we give these input tokens。

 we have the transformer L transformformal layers and then the system we generate reccursively the response because we know the label using a training set we can basically fine tune the system to give us the right answer okay so we can again we are going to okay compute the crossenttropis with respect to the generated answer and the ground truth and then we are going to do the backward path to compute the gradient and update the parameters of the system so for GNN but also for the LLM。

Okay， so in summary georetrieval is composed of four steps so we have graphH which are this here。

 which is for subgraphph retrieval related to the query of the users then we have a computation of laptop tokens using a GN we have also。

Yes， response generations， once we go through the input token and also the transform layers and find any model training okay。

So here we try to combine the best of all world together and I think this is done in a very natural way yeah we have to change the existing data set and create a new benchmark to evaluate this task so doing the reasoning of text attributed graphs。

Yeah， so we have defined things， explanation graphs， scene graphs and web QSP。

So the menu result are basically that okay here there are many things but you should only focus on that。

 so if you do our technology retrieval， basically you will be able to beat if you only do LLM so you have your LLM your query and then you get the answer if you do only the GNN pump tuning so we still do better and if you only do the lowha fine tuning LLM so we still do better okay so this is really trying to combine the best of this world。

Okay， so the scalability is basically now we are able to， for example， only use。

 you know reduced by 83% and 99% the of you know the number of tokens number of node that we use so instead of taking you know the whole Wikipedia graph we will only take you know a very small graph subgraph of the Wikipedia of the Wikipedia network。

Yeah， of course the one that we were interesting is does it improve hallucination so we compare with the baseline which is just doing LLM with query and we manually。

 so what we did is that we ask we did one of the queries and then one of the responses and we look at manually if you know there is some missing nodes or adding you wrong nodes in the same as for the edges and we see that if you use Getry value really reduce the hallucination。

Add studies so what we observed basically is that and that was very interesting is so the token given by the GNN and the token the text tokens of the graph they are actually contribute equally okay so the information coming from the GNN and the information coming from the text and the LLM of the graph is they are basically both important okay so they are complementary they really improve everything so I think it makes sense of course because you have your GNN we know that they are pretty good for extracting graph information but also the LLM as a very strong capability with you know with text representation so the two are complementary。

😊，So in completion， so if we want to unlock the LLM Cap。

 we need to use you graph as represented as token as wells。

 but actually combining everything so the LLM， the GNN and the graphraph hack provides superior performance。

 so it's not only the LLM Cap that does the work is actually many authors。So GA is effective。

 efficient and mitigate and imaginationmun， so here are the paper the code and I really invite you to read the blog post by Shashing he so she has done a terrific work here。

 she's the main the main you know researcher in this project received for this actually the 2024 Google scholarship and I really yeah invite you to read a blog post if you want to have a highly running introduction of this。

So also yeah so the P to optmetric team you know they included the geori so this is very nice and I think also something which is very interesting is to look at again we are always go back to the question can we use GNN to improve products right and I think here there is an opportunity so if we look at the history of website engines so everything starting with you know Google data rank they didn't use any text processing。

Then there was walk to V they use walk to V then they use an language model they use also HaG and finally we have Gemini okay so Gemini is LLM plus high so LLM if they don't know the answer they will look at HaG when you pump Gi so the next step hopefully would be to use GNN and you know some textat graph knowledge graph that would that would be useful for web search engine so here what is attractive I think is that everything is integrated and learnable so this is you know diplomaron。

So the next step was of course that we are working with Xiao Xing is basically okay so we have the text the text feature。

 we have the graph feature so the last feature you want is basically doing the image introducing the image information so again today we as a transformer so what you can do is that you can take your image you can decompose your image into patches and then your patch can be represented by you just a vector so this is the visual token and then you go through transformal layers and you back propagate also to update the representation the parameters of your visual transformer。

And that's it， so thank you so much for。😊，Yeah， for being there and I'm happy to take any question。

难错的要送d。So if the participants have any questions， you can type those into chat or slacklack。And yeah。

 to perhaps get us started， I had a question。And yeah， I thought like how。

 how you propose to tokenize everything， essentially and fine tune with Laura。

 these language models is is really exciting， right， Like it really unlocks multimodality you。

 you did have a slide about graph tokenization， right， and that there is no canonical order。

 So how do you deal with that in this work， You know， like。

 how do you kind of decide how the graph is tokenized in the end。😊，Are you talking about this one？

Yeah， like you where you'd shown the talk like a graph slide as well， right？嗯。

I'm not sure what you mentioned this slide right so is this how you kind of textualize the graph？

So textualization， where is it？😊，This one， Yes， yeah， yeah。So textualization is arbitrary。

It's completely arbitrary， so I think Brian Pelozi has his nice paper。And this is an issue， right。

 so you want basically to have， as you said， a canononicical or presentation。And there is not。

Okay so so what is for the LLM that you have been trained you know the best representation to extract the most information so this is impossible to say so not only you don't have a unique representation but still need to have like a world of representation otherwise you cannot have access to the LLM knowledge but at the same time you have no idea you know if this representation is good or not so this is like prompt right so there is no no way to know the quality of your prompt before before you try so for example what we did I don't know if you notice that but for example there is a change of performance in the prompt。

If you put the title after the abstract。So so I think all these models they have this issue so the way you pump you're gonna to have very def so here there should be no def right if you put the title before before the abstract but there is a def so you need to play and this is you know I was half joking when when I talk about these LLMs that you know so they know they have seen everything they you know they have seen all the human knowledge and you can ask them anything but if you are not precise if you don't know how to ask them the question they will never give you a right answer so the only way I think to bypass this is is。

😊，Basically， to get along with the non uniqueness of the text representation， but then to find tune。

So before you know Loha was I was quite pessimistic about this technique and I say oh it's very hard you know to find actually some Google presentation that at the end will be some vectors and then you need to align the vectors together with the graph the graph vectors and everything else but you will never be able to get something good because they have not been trained this way but if you do this law you know fine tuning then you have a way to align this vector information so it's not perfect so far it also doesn't make sense if you do them。

😊，You know at the end yeah for example， yeah that's go to the end so it doesn't make sense in some sense in some sense。

 you know， to combine the visual tokens， the graph tokens with you know the world tokens because they are very different modities so what you do with the MLP that you put here you are trying to align you know the visual space the graph space with the text space and then what you do if here this is frozen basically you don't do anything you hope for the best you hope that in this space you know the alignment is good enough to give you good precision so so it's never the case I don't think so but there are so much parameters that eventually give you something good but now that we can fine tune with Lo then what you do is basically you force all these spaces to align。

And this is why it works better when you fine tune。Yeah， thanks， I mean， thanks for all the details。

 it's always exciting to discuss research this way。😊，嗯。そいうに。Do you have time for one more question。

 Yeah sure。Yeah， so I guess like another thing to discuss right would be So if we can convince industry of the potential of these approaches and I'm certainly excited。

 do you think it's it's possible in the future that we have LLMs which are let's say like more implicitly able to handle graph structure data here we had to fine tune with with Laura and so forth right。

 but do you think like there's a possibility to have graphs as one of the pretraining modalities Yeah。

 this is what the committee is trying to do today right so let me go to this slide。😊。

This is exactly what the community is trying to do right。

 it's basically can we do some graph foundation model？

So you would be able to portray your graph in different modalities。

And then you recombine then with auto modalities。 What we do today is that here we are using the LLM self attention layers。

😊，Right but what we would like to do ultimately is to use some so you train very powerful foundation model for graph foundation model for vision。

 foundation model for text， and then you combine them using this you know transformal layers for different task that would be you know I think for industry that would be the best way to go the problem is that today only LLMs as so much。

😊，Knowledge parameters you know so there is it's very inbal there is no way I think today that we can do a product we can be powerful with G only Gen today is just a top cherry on the top in some sense so so it's not like you know we are missing data training data so the way is full of you know graph structural data is full of that there is no issue with that the question is how do you make a product that would you know excite industry like you know meta Google and so on that would tomorrow say okay let's do like ChGPT but let's do this for graph so I think you know because it's not clear and also I have this you know that was already it at some point someone was saying you know why isn't GN in Hyman industry so that was a very interesting I think you know。

AD， so the only today the industry that likes G is biology。

Right so because you cannot do today LLM for biology。

 so the only one that can make money of that is basically biology and so we have Nobel Prize so there is a huge promise that this deep learning Raal network and so on can make money of course it's a promise it's not yet there but I think this is today where the money is for GNN but of course we would like to do it also you know for textex there is no reason we cannot do that but。

As long as we don't have like industry， I mean we need to be honest with our so industry drives you know the AI。

 the big AI you know improvement like like GT right so GPT could never be undeveloped in academia so of course academia is very important to get the ideas you know like for example division models that have been developed by a PhD student in Germany and so on so ideas we come from academia。

 but know scanning up and making a soetal change we come from industry so it's part of the pipeline if we are not able to convince people to use GNN for you know text so either GNN is not good enough or we are not doing a good work you know to find the right products。

いや。Nice， yeah。 I think， I think this work is is definitely going in that direction， right。

There's there's quick。Yeah。There's a question from the audience so the questions as follows for graph rag。

 could it be possible that retrieve nodes and edges are mutually exclusive and don't actually form connected subgraphs？

😊，If so， is there methodology to ensure that the subgraphra is connected？

Yeah so this is a very good question so I think this is completely arbitrary again so depending on the task you want to you know you want to solve so sometimes maybe you want directed you subgraph you want undirected there are many ways to retrieve you know subgraph so you know minimum span tree is the simplecht one but they don't have any you know in some sense the size of the the output size cannot be controlled this is the number of nodes so here we wanted something you know with less so I would say everything is possible as long as you know the task that you want to optimize so here we use this one but again you know that was for that was directed so that was for us and we also wanted to have like a small size so we can quickly you know do that so we wanted something better than the minimum span tree so this is why we used sh or tree which is done out but again。

I think you can use dependent you know subgraph extraction that is going to fit your objective there is no problem with that you just you know you need a process to extract a subgraph that's very important and this process has to be linear time because if if you do that on you know。

On large graph， I mean， at the end again， we are talking about product， you want to do it， you know。

 on the fly， so you want something very， very fast。That's I think the only condition。

 so of course a linear approximation a linear time approximation is never as precise。

 but if you take enough nodes and edges it should be good enough， I think for your application。

Yeah that makes sense thanks for all the detailed answers and yeah thanks for the wonderful talk I think it's really exciting and you know like how all these technologies come together and with graphphra with graphph vector databases I think we we're on the cusp of hopefully the kinds of breakthroughs you talked about。

😊，I'mThanks for。 Thanks for good talk。Sure， thank you very much again， everyone for the invitation。😊。

Yeah， and again， like to all our presenters for the next session， sorry for the delay。

 we had technical issues and I'm going to hand it over to Hey you and she'll be chairing the next session of oral presentations today。



![](img/c4b0d8eef1833c42d9a658dd61469cc2_3.png)

Can you make me the host？

![](img/c4b0d8eef1833c42d9a658dd61469cc2_5.png)

Yeah， thanks。So hi I sorry about the slide delay because we started about 15 minutes late so we will push back everything by 15 minutes。

 so we have three exciting papers to share during our oral presentation and the first one is decomposing force fields as follows on graphs reconstructed from stochastic trajectories and it will be presented by Raan。

😊。

![](img/c4b0d8eef1833c42d9a658dd61469cc2_7.png)

Yeah， can you share those screen please？上次 good。whenever you're ready， it's all yours， Okay。

 so you can hear me。😊，Yeah。Okay great so yeah thank you very much for the invite I'm going to be talking about this paper decomposing force fields as flows on graphs reconstructed from stochastic trajectories。

😊，Which is a paper in this year's learning on graphs。

So this paper focuses on stochastic processes and nonequ steady states。

 so in a simples form continuous space stochastic processes are described by SDEs or landjamin processes。

 which are stochastic differential equations given by some drift term F and some diffusive term D。

And whilst these are the dynamics of a single trajectory， they're density dynamics。

 So this is the evolution of this probability density。

 the probability of a trajectory being at position X at time T evolves according to the very famous Fcker plank equation。

And when the density is stationary， this means at this left hand side， Dp by Dt is0。

 and then the process is said to be in a steady state。

And a steady state can either be in equilibrium so this is thermal equilibrium or in non equilibrium and that depends on whether this probability flux J is0 or not。

 so if this probability flux J is0 at in the steady state。

 then you have an equilibrium steady state and if it is non-zero then you have a non equilibrium steady state。

So nonequilium processes are very important in a range of domains。

 but particularly biology because biological systems maintain themselves in a nonequilium steady state to avoid thermal equilibrium where thermal equilibrium is a situation where theyre no longer dissipating any more heat to their environment and it's synonymous with death so here are just three examples of biological systems that maintain themselves out of equilibrium you have red blood cells well the outer membranes of red blood cells flickering with non equilibrium dynamics。

 human brain dynamics here illustrated with just the first two principal components showing these strong fluxes in steady state and similarly in cell movements。

And these non equilibrium processes are also the basis of diffusion models in machine learning research。

 they've gotten a lot of interest recently。So I'm going to focus on what's called the helmhot hodge decomposition。

 so if you have some non equilibrium stochastic process。

 it can be decomposed into reversible and irreversible components。

So the reversible component is stochastic and it basically is the part of the drift that balances the diffusion。

 so you have some noisy fluctuations and then you have some drift that is going to maintain you on the stationary distribution。

And then you have the irreversible component which is driving this rotation around the stationary distribution you can see that here it's a circle so you put these two things together and what you get is a nonequili process on the left hand side that clearly is evolving over some kind of Mexican hat or donutt type distribution。

 but with a particular nonequili rotation around this circle。

And so if you're interested in non equilibrium dynamics。

 you're really interested in this irreversible component and how to extract it。😊。

So if you limit this to the case of isotropic diffusion。

 so this is where diffusion is happening in the x and y coordinates exactly or where your diffusion is a multiple of their identity。

 then the process is said to be reversible if and only if the drift is conservative so we can basically in the case of isosropic diffusion right F equals J over P plus some gradient flow if J over P is0。

 then F is just a gradient flow and so it's conservative and then your process has no irreversible component so this is quite a nice characterization of equilibrium versus nonequili stochastic processes。

Now if your process has non isotropic diffusion， you can just change coordinates so that you're in a basis or in a coordinate system that has isotropic diffusion。

Okay， so now I'm going to take a slight turn towards graphs and S complexes。

And show you the hodge decomposition on a splial complex or graph。

 So I'm sure you allll know what a graph is， but a sial complex can be defined automatically by basically taking higher order simplexes from the cliques。

 So a set of three nodes that are connected becomes a triangle etctera， etc cetera。

 you can define higher order complexes。Now an edge flow or flow along a sial complex is just a function defined on the edges that is alternating。

 so if you just give a real value for each edge and you have this convention that if you flip the edge you take minus that so XIj equals minus XJI。

 then you've defined a flow on the edges of your sial complex。

Now these flows have this decomposition into three components so you can basically decompose this edge flow X into a curling part gradient part and a harmonic remainder and this really closely mirrors the decomposition that we had for stochastic processes so the gradient of a flow defined on the vertices basically means you have some function F defined on each vertex and you take the difference of those to be between two vertices to be a flow along that edge then you have the curl of a flow defined on the triangles and then you have some harmonic remainder and I'm skipping over lots of the algebraic geometry here。

 but this is a wellknown decomposition in signal processing for graphs and s complexes。

So if your complex is specifically a triangulation then we know this harmonic remainder disappears。

 so here we have an example of an edge flow defined on the general S complex and it breaks down into these three components and if you sum the three components you get back the edge flow you have the gradient。

 the curl and the harmonic part。And on the right， you have specifically a triangulation。

 so you only have triangles and your edge flow breaks only into a gradient and a curl part。

And this decomposition has been used in many signal processing applications here are just two examples that I chose。

 you have this ocean drifter data or you have this RNA velocity field。

 it's just two examples of hodge decompositions in action。😊。

So how are we going to bridge this decomposition of a stochastic process with the decomposition of a flow on a S complex and the way we're going to bridge between the two is by considering the hodge decomposition of a discrete state Markov process。

So a discrete state markov process tells you the evolution of some probability where you have the probability of being in some discrete state I at time T。

😊，And this is defined by this matrix of rates， the Q matrix。

 which basically contains all the transition rates from node I to node J。

So this has a natural mapping as a graph because you can say that the vertices in your graph are the states of your discrete state process and transitions occur over edges。

So the real key here is how do we define an edgeflow that is analogous to the drift of the SDE so that if I decompose that edge flow with the discrete Hodge decomposition。

 it looks like the decomposition of the SDE with the helmholttzs Hodge decomposition。

And it turns out that the correct way to do this is by taking basically log of the square root of the ratio of these two rates。

 which seems like a bit of an arbitrary choice， but it's mathematically motivated because one it is naturally alternating so it satisfies that it's an edge flow and more importantly。

 this satisfies that the markov process is reversible and in equilibrium。

 if and only if x is conservative， so it is the gradient of a scalar potential。

And that marries up perfectly well with what we had in the continuous state。

 so our continuous state stochastic process is reversible if and only if F is a gradient flow and our discrete state process is reversible if and only if X is a gradient flow and so we're going to use this relationship to define a basically a mathematical pipeline。

😊，So here's the idea。😡，We're going to approximate a stochastic differential equation with a discrete state process that we're going to infer directly from stochastic trajectories。

We're then going to decompose the discrete state process。

 and this is going to recover the reversible and irreversible components of the SDE directly from these stochastic trajectories。

So here's our pipeline we're going to begin with a number of trajectories which we're just going to consider to be a point cloud in face space So here I've just got two trajectories but you might have up to 100 living in some face space here I've illustrated in two dimensions。

Now we basically want to get a grid that kind of represents where our points live and we want this to have a uniform density。

 so we perform furthest point subsampling and we get this uniform density subsle so you can see here I basically just picked out a uniform number of points that I can then grid in a way that will capture my manifold in my space。

So we triangulate these purple subsampled points and what we get is with the Delorney triangulation which is a very standard triangulation。

 I've put some references here if you're unfamiliar with these techniques。

 but the delorney triangulation is a natural way for basically filling in triangles in this space。😊。

And so what we get is a triangular mesh or a grid where you have these midpoints labeled in pink dots and you have the actual vertices labeled as purple dots。

So now we want to use these as our discrete states， these triangular bins。

 and so all we have to do is basically follow our trajectories through these bins and count the number of transitions from each state J to I and then the maximum likelihoodestator is very simple it's just the number of transitions from J to I divided by the holding time in state J。

And so this way you can see that I followed my trajectories around。

 and I've just inferred a discrete state stochastic process directly from some trajectories。😊。

Okay so what we have in this case actually is you have transitions going from the midpoint of each triangular bin to the adjacent triangular bin。

 but we actually want our discrete states to be vertices， not midpoints。

 so we take the dual tesseellation or the dual triangulation which is very natural s complex so're basically flips so that the midpoints are now vertices and the transitions are now occurring over edges。

So now we have what we wanted， which is we have a s complex or a triangulation where our discrete states are vertices and our transitions in our stochastic process are edges。

So all this left to do is to define our edge flow using our maximum likelihood estimates of the transition rate。

 taking log of the square root of the ratio， and we have a flow that represents the drift of our sacastic differential equation on this s complex。

So now we can decompose the edge flow so the discrete version of the edge flow is very simple to decompose it's basically just an LSQR problem and I'm skipping some of the mathematics again。

 but this is because the hodge decomposition decomposes into orthogonal spaces which basically means you can find some function G whose gradient is most similar to x and that is the scale of potential G that solves the helmhol hodge decomposition so here gradient basically is going to be a matrix a sparse matrix G is some vector in the space of the number of nodes and x is a vector in the space of the number of edges and so you just solve this problem with LSQR you get F and then you can just recover the curling part by taking the remainder x minus minus this gradient part。

And so we can decompose this edge flow into two parts， the car star and gradient of F。

 where car star phi represents all of the irreversible currents in our stochastic process and gradient of F represents the reversible component。

😊，Okay， so now we're going to try and see if this actually works with some numerical experiments。

So a first sanity check is your reversible components should be even under time reversal and your irreversible components should be odd under time reversal。

So we basically take one ensemble of stocaastic trajectories in this case from a linear onstein Unbe process。

 and we basically construct our edge flow going forward in time and decompose it。

And then we do it the same， but with the trajectories turned backwards in time。

And because you have the same meshes， you have the same edges， but the flow is what changes。

 So we can look at the correlation between the flow when it's going forward in time and when it's going backward in time。

 And what you get is that the gradient flow is strongly positively correlated。

 which suggests it is even under time reversal and that the curl star flow is strongly negatively correlated under time flipping。

 which suggests it is odd under time reversal。 So the sound to checks past and it suggests that we are actually capturing reversible and irreversible currents with these two parts。

😊，So as a general measure going forward of the level of non equilibriumilium or the level of irreversibility。

 we're going to take the proportion of the curl flow over the wholeuling。

 so basically how big is your curling component relative to the size of the curling component plus the size of the gradient component。

😊，And we're going to see if this metric PC lines up with what we expect。

So we begin with two solvable processes， the Einstein Ulenbeck， which is a linear process。

 which we know converges to a Gaussian distribution， and the plain limit cycle。

 which is a nonlinear process that we know converges to a Mexican hat style distribution。

And these have been parameterites so that there's some parameter theta which causes rotation around the station distribution。

 so it allows me to change how much irreversible current there is in each of these systems。

So I do this and I basically can compare PC in the reconstructed sub complex flow to PC in the exact solution。

 where the exact solution is just approximated on some square grid。

So I'm not exactly comparing a like we'd like here。

 but the idea is that I'm looking at how much of the exact solution is curlling and how much of it is reversible。

😊，And what you can see that we capture the correct qualitative dynamics。

 which is that as I increase theta， I'm increasing the level of curling irreversible flow in my exact solution。

 but also in our reconstruction， so our method is able to capture how nonequili your dynamics are relative to similar systems。

😊，So these are two solvable examples， but let's consider two unsolvable or nonlinear examples in the form of the stochastic van deol oscillator and the stochastic Rosler attractor so in these cases I haven't got an exact form for the stationary distribution and so I can't perform the Helm's Hodge decomposition exactly。

But I can use my parameter theta as a proxy measure for how much irreversible curl flow I have。

So I do this again and I increase theta and you can see that in the band a par oscillator。

 I am basically capturing quite well that as I increase theta。

 I'm increasing the level of curl or the level of irreversibility in my edgeflow。😊。

In the Roslow attractor I have the same behavior but it's slightly noisier because the Roslo tractor is in 3D。

 so I have to use slightly larger more coarse triangles in order to get good estimates and so you get more noise but the qualitative behavior is still there。

 you increase theta and you increase the proportion of coal in the reconstructive flow。

So with these numerical experiments it suggests that our method actually works。

 so we're able to capture the level of purling flow in the original data in our reconstruction。😊。

So we're going to apply it to some real world examples。

And so we're going to focus on two case studies where we know the kind of level of ground truth。

So the first one is a flickering red blood cells so you can have healthy red blood cells which have some high level of dissipation and you can have synthetic ATP depleted red blood cells。

 which we call passive which have much lower dissipation so that means they're more reversible than they're active or healthy counterparts and this is something that has been shown in real world data recently it came out in science。

 and then there's a result that is much much older but also well known。

 which is that aythic or aging human heartbeats have lower irreversibility or lower curling flow than healthy counterparts and in the first case the data is just a univariate time series is the outer membrane position of a red blood cell over time and in the human heartbeats。

 it's a unvariate time series in the form of an ECG an electrocardiogram。

So if I have these Uniivevariate time series in both the case of the red blood cells and the heartbeat dynamics。

 I want to basically reconstruct phasebased dynamics because that's where my method is actually working and so we just use a very simple time delay embedding into two dimensions where we picked tau with a very standard method which is the first zero crossing of the autocorlation。

And so this way I'm basically able to take my Uni time series and get some dynamics in phase space for both healthy and depleted or passive red blood cells。

 as well as healthy and ay rhythmic heartbeats。😊，So there's one caveat here。

 which is that in my framework and in my numerical examples I assumed isotropic diffusion。

 so I assumed that diffusion was happening exactly in the x and Y coordinates that D was a multiple of the identity in real world data you can't assume that to be true so we have to estimate the diffusion tensor and then do that coordinate change that I mentioned earlier。

So what we do is we use that this formula looks a bit ugly。

 but it's just the maximum likelihood estimator for the diffusion in each triangle so you follow the trajectories and then within each triangle you just compute this quantity where this product in the middle is a tensor product and it gives you the estimate of the diffusion matrix in that triangle I'm assuming that it's constant overall all triangle so I take the average and then I change coordinates so that my data is now evolving with isotropic diffusion。

And then I can apply my framework as before。And so what do we find we find that what we find what we expected to find。

 which is that healthy red blood cells have a significantly higher level of PC which is our irreversible curling flow compared to passive red blood cells and this is using ensemble bootstrapping so i'm able to generate a lot of ensembles to run this on and check the significance of the differences but you can see that we have significance at the highest level from healthy versus passive in this in this example and then we have similar results with with our human heartbeats so we see that healthy。

Heartbeats are significantly more irreversible， have significantly more curling flow than their ayic or unhealthy counterparts。

And this supports the previous results in the literature。

 but using our new kind of graph based framework。Okay， so to recap。

 we developed a method that leverages the discrete form of the helmholtdgedecomposition on a Sial complex to approximate the continuous form of the helm Hots HodgeD composition of an SDE。

We validated that our approach captured irreversible currents in both solvable and unsolvable stochastic processes。

And then we applied our approach to flickering red blood cells and human heartbeats to confirm that there are elevated levels of irreversible occurrence in healthy conditions compared to impaired conditions。

 either passive in the case of the red blood cells with ATP depletion or arrh rhythmic heartbeats with a myocardial infarction。

 which is the specific disease that we were looking at here。😊，So in conclusion。

 our approach breaks new ground， the interface of graph signal processing。

 particularly with applications to stochastic dynamics and biophysics。

 which is not something that is commonly looked at with graph signal processing techniques。

 and it prompts more mathematical work to basically look at the relationship between the discrete or graph-based form of this hodge decomposition which is getting very popular and the continuous form which is well known for both SDEs and for just functions can be decomposed similarly with the Helmts hodge decomposition。

So thank you for listening all that's left to do is to thank my co-authors so those are my supervisors Aang Gelli and Reult Lambyot at the University of Oxford。

 Morton Kingleback at the Center for youaimonia and my co-authors。

 poor expert David Bears and Alexander Strang If you're interested please read the paper and reach out。



![](img/c4b0d8eef1833c42d9a658dd61469cc2_9.png)

Thank you so much for the fantastic talk is there any question from the audience I think we can we have time to take one question。

😊。

![](img/c4b0d8eef1833c42d9a658dd61469cc2_11.png)

嗯。I have one question to start with， I find it very interesting that you have the application in red blood cells and heartbeat can elaborate on more practical usage of your model on other biophysics。

😊，So in terms of like。racicallyThere's a lot of interest in nonequ equilibrium dynamics in biology。

 these are just a couple of papers from recent years。Specifically。

 I don't know if it has any clinical applications at this level。

 but it's more about the energetics of being able to capture from data basically how much dissipation。

 energy dissipation entropy dissipation is happening in biological systems and this is quite difficult to do from dynamics and there's a number of different techniques and this is just one more in the quiver of techniques for analyzing non-eququiilibrium dynamics。

 we picked those two examples because there were situations where we kind of knew the ground truth or there were other methods that had been used to get a ground truth so we could see if our method actually worked it was partly kind of the there are other exploratory avenues for plot for looking at nonequilium dynamics specifically most of my work in neuroscience where we're looking at non-equilient dynamics in the brain as a signature of cognition and of consciousness。

😊，Thank you I think we have one quick question how could we use this framework to perhaps understanding the diffusion generatedative models do you have any insights Okay yeah i'm certainly not an expert on diffusion models I know that they that they they focus on non equilibrium stochastic processes as well。

😊，If you like I've seen recent work from this month。

 actually looking at the dynamical regimes that diffusion models go through。

In theory this could be applied to the output of any stochastic system as long as it evolves in a reasonably sized face space。

 a reasonable number of dimensions in face space， in terms of diffusion models you could apply to the trajectories that diffusion models take。

 however when you have access to the equations it's not necessary to use these kind of data- drivenriven techniques。

 so I think with a diffusion model from what I understand you might have access to the actual model forms in which case you can use basically the exact decomposition。

 the exact mathematics without having to go to a data drivenriven method。

Got it thank you so much again for the great talk Thank you thank you for your time and thank thanks for everyone for listening。

😊。

![](img/c4b0d8eef1833c42d9a658dd61469cc2_13.png)

Now we have the second presentation from Julia， the title is a cosmic scale benchmark for symmetrymtry preserving data processing。

Yeah， whenever you're ready。

![](img/c4b0d8eef1833c42d9a658dd61469cc2_15.png)

Oh thank you and thanks for the invite to speak so hi everyone I'm Julia and I'm a PhD student at MIT and today I'm excited to present our work on a cosmic scale benchmark for symmetryymmetry preserving data processing which was performed as part of the NSF Institute for AI and Fund Inter for Ifi for short。

So let's start with a bit of background on one of the flagship observations in cosmology。

 which is Gal clustering。And this is where the positions of galaxies and also their associated properties are measured by cosmological surveys。

 such as the one shown in the image here， and cosmologists are generally interested in studying the spatial distributions of galaxies because they can tell us a great deal about the underlying structure of the universe and also about the physical processes that shaped it。

And the data from these observations is usually very complex and high dimensional。

And so traditionally， cosmologists would resort to using summary statistics， like say。

 the two point correlation function。Which at a high level just quantifies the likelihood of finding pairs of galaxies separated by a given distance。

And so while these types of statistics have been incredibly useful。

 they also come with the drawback of potentially losing information about higher order correlations in the data。

And so this drawback motivates the development of machine learning tools that can reliably extract information from these types of surveys and process these point clouds。

And besides the downstream cosmological tasks of interest。

 Galaxy cluster data has several characteristics that make it a valuable benchmark for stress testing。

 graph neural networks and other machine learning methods。

So the first characteristic is the large point cloud cardinality right so most atomistic systems that you might encounter in other scientific domains where you use graph processing。

 like say in small molecules only have around 10 to 100 nodes per graph。

 whereas these data sets are sometimes on the order of over 10 to the six points per point cloud。

And so this presents unique challenges when it comes to scalability。

The second characteristic is the presence of information across spatial scales right so on one hand we have gravitational forces that cause matter to cluster and create these short range correlations。

And on the other hand， we also have the growth of structures that could have been initially in causal contact。

 but then might now be spatially separated。And this creates long g correlations。

And so this means that we need to use methods that can capture both local and global information。

And finally， the third characteristic is the symmetry structure in the data。

So since the universe is homogeneous and isotropic， its properties are spatially uniform。

 and so this means that the distribution of galaxies should exhibit Euclidean symmetry or in other words invariance to translations。

 rotations and reflections。And so this means that this data could also be useful for benchmarking E3 equivariant neural networks。

So taking all of these characteristics as motivation。

 our first contribution is the curation of a point cloud data set from existing cosmological and body simulations。

Along with an easy to use interface for accessing the data set。

And we also introduce a JaX based code repository， al EQ and A JaAX。

 which implements common equivariate neural network architectures all in one consistent framework。

And finally， we systematically evaluate the performance of various graph neural networks on downstream tasks using this data set with a particular focus on equi。

Varian models。Okay， so let's now talk about the data set in more detail。

So it's derived from an existing suite of antibody body cosmological simulations called the Quixote simulations。

And these simulations take as input a set of five cosmological parameters that describe the conditions of the universe。

 like say the matter density or the rate of expansion。

And then they follow the evolution of millions of dark matter particles in a periodic volume。

And it's important to note that since this volume is periodic。

 what this means is that a point that's say on the very right edge of the box is actually very close to a point that's on the left edge。

 and so it'll be very important for our models to also account for these periodic boundary conditions when computing quantities like distances。

And so by uniformly sampling these five parameters from some pre specified ranges。

 we can obtain different spatial distributions of dark matter corresponding to different possible universes。

And these simulations are very computationally expensive to run。

 and this emphasized the need for simulation efficient methods to extract information from these resulting data sets。

And in the subset of the data that we collected from these simulations， we have a total of 12。

384 in our data set。And so we' next post processed the simulations to identify dark matter halos。

 which are gravitationally bound structures that host galaxies。

 and then these halos are identified using an existing halo finding algorithm。

And we then select the 5，000 most massive halos in each simulation to construct our point clouds。

And each point in the point cloud comes with a set of node wise features， which are the position。

 velocity and angular momentum vectors， as well as the mass of each dark matter halo。

And also each point cloud is labeled with a set of global features corresponding to the inputted cosmological parameters。

And the two parameters that we focus on later in our benchmarking study are the matter density or mega M。

And the Sigma 8 parameter， which is the root mean squared a matter of fluctuation that's averaged over a sphere with the fixed radius。

And so for example， here we see two example point clouds in our data set with different parameter values。

 and they're especially different for omega M。And in particular。

 we see that the point cloud with low matter density on the left appears to be more clustered than the one with larger omega M on the right。

And for each point cloud in our data set， we also compute the two point correlation function or the two PC summary statistic to use as a baseline for benchmarking our models。

And I'll discuss this quantity a bit more in the next slide。

So the two point correlation function is essentially a measure of the excess probability of finding pairs of galaxies at a given distance R apart relative to a random distribution。

So on the right， we see the corresponding to PCf plots for the example point clouds。

So the one on the left with smaller omega M and hence more clustering has higher two PCF values at larger R than the one on the right。

So essentially the values of the two PCF for large are capture long range information while those for small are capture information on smalls。

Fs。And so how do we represent this function as an input into our models？Well。

 we can represent it as a vector by creating bins for increasing values of R and then computing the excess probability within each bin。

And so in practice， we actually use just 24 logarithmically spaced bins to construct the vector for each point cloud。

And we actually use these two PCF vectors as a baseline for our graph level prediction tasks where we train an MLP on these handcrafted summaries。

 and then we compare the performance against our GNN models and test their ability to extract more information than traditional summary statistics。

So let's now look at the benchmarking tasks that we consider so the first set of tasks is graph level parameter prediction。

Where given the positions of the galaxies as inputs。

 the task is then to recover the scalar cosmological parameters that served as input to the simulation。

And we focus on predicting mega M， which depends on long range correlations and on Sigma8。

 which depends on short range correlations。And we additionally consider a node level task where。

 again， given only the positions， the task is to predict the velocity vector at each point。

And this is a pretty challenging task since we're only considering the 5。

000 heaviest halos in our data set。So to benchmark GNNs on these tasks。

 we first convert the point cloud into a graph by adding edges to the K nearest neighbors for each node。

And all the models that we benchmark are essentially versions of the same core message passing framework。

 which can be represented as the classic edge level node level。

 and optionally the graph level update functions。And so we benchmark the following commonly used message passing architectures。

 three of which are E3 equivariant， and this means that they satisfy this condition that when you apply a transformation such as a translation。

 rotation or reflection to the input，The output is then transformed accordingly。

And each of these architectures achieves this constraint differently。

 which I won't go into details for for the sake of time。

 but I will say that one thing to note is that the SCGNN and NquiP both use stral feature representations that rely on spherical harmonics。

And they both use this like this parameter L maxax to control their maximum degree。And finally。

 we also compare our models to pointNe++， which operates directly on the point clouds and processes them in a hierarchical fashion。

And we also found that the use of radial basis functions as edge features was actually vital to the performance of our equivariant models。

And so what this does is it essentially transforms the scalar distances into。

A richer higher dimensional representation by projecting the relative distances on a basis of N radial vessel functions with some fixed radial cutoff C。

And we use vessel functions because they were also used inquip。Okay。

 so for our benchmarking experiments， we trained these models on the graph level and node level prediction tasks for which we use a randomly sampled subset of 2048 point clouds for training。

 as well as validation and test sets of size 512。And the lowest mean squared error that's achieved for each task is shown in bold in the table。

So。Looking across all three tasks variant models outperform the nonequvari， GNN and pointnet。However。

For the matter density prediction task that requires long range information。

 none of the models beat the baseline， which is simply training an MLP on the 2 PCF vector。

And this is perhaps surprising since these models should be able to capture higher order correlations compared to the two PCF。

 but it also is perhaps unsurprising。Sinceage。GNNs are known to struggle with propag long range information。

And so we can actually probe this further by seeing which information is present in the2 PCF that is not also captured by the learned graph embedding。

 and so we do this by concatenating the two PCF vector with the graph in final readout MLP。

And so the2 PCf becomes this additional global feature in this way。

And so we do this for our best performing model， which is the SGNN with LX equals2。

 and unsurprisingly， since there's strictly more information being fed into the model now。

 the performance improves over both the SGNN and the two PCF individually。

But we can take it a step further and only consider using parts of the two PCF。

 namely the components corresponding to small R and large R。And breaking it up this way。

 we see that using only the small scale components does not help the mega and prediction task。

 but when we use the large scale components， the performance is almost as good as when using the full vector。

And so this essentially verifies the fact that the missing information in our models is precisely the long range correlations。

And I think it's really nice that this data set and this type of analysis gives us an easy way to test whether a model captures the long range information or not。

And also， given the importance of simulation efficiency for these cosmological tasks。

 we also benchmark our models across different training set sizes。

 going from only four simulations to nearly the full 12，000。

And we see that the SEGNN models in particular show better performance across all training sample sizes。

And NQp also appears to do well in both the really data scarce and in the asymptotic regimes。

So just to summarize。Takeaways。We a set consisting of simulated galaxies whose spatial distribution is informative of the underlying cosmological model。

And we show that both the graph level and no level tasks can benefit from the use of equivariant models。

 which were also found to be more simulation efficient。And additionally。

 the two PCF summary statistic outperformed the GNNs in inferring a parameter that is sensitive to long range correlations。

And therefore we think this benchmark would be a great target for methods that aim to mitigate these issues associated with long range information preservation。

 that for example， it might make sense to test transformers on this as future work。And so with that。

 I'd like to thank my amazing collaborators and mentors。

And also to plug our repo on GitHub where you can find the link to the data set and the implementation for all of the previously described methods in JX。

So that's it for me and thanks for listening。Thank you so much for the enlightening talk and very interesting benchmark。

😊，There is some discussion among the audience。呃。So one of the question was whether there is an existence of dark matter particles。

 but I think one of your co-aus has answered it， so well I guess if there's no followout question and we are short in time。

 we have another question about the results， so the tables show that the EGNN model fail drastically and is there any intuition why？

😊，Yeah， so I think that it might be due to the lack of like model capacity。

 like it has the fewest parameters of all these models。

 but it also could just be not enough time spent hyperparameter tuning。So'm not sure， yeah。

So you mentioned there are two characteristics from the results of first one。

 equivariance matter and the second one La information also matters。

 do you have any issue how we can tackle both by combining them together because I think La depend is a well studied topic in Gna community as well。

Yeah， yeah， I definitely think it makes sense to try something like an equivariant transformer and I think we actually have an implementation of one in our repository but due to time constraints didn't include it in the benchmark。

 so I think that would be a good combination of the two。Co yeah， thank you so much for the talk。😊。

Okay， that's a reasonable question。 So point9 plus plus is long range， but now avariarian。😊。

Yeah that's right yeah so I point out is also nice because it processes the data in this like hierarchical way。

 so I think that's also something worth exploring because it seems unsatisfying to try to basically fool the node representations from like5000 nodes into just one vector at the end so yeah thank you so much for the talk Thank you。

Now let's welcome our third presentation from Saloian。Yeah。

 and the paper title is on the equivariance of graph convolution and mix up。嗯。O hello， everyone嗯。

Could you see my screen。Yes， okay。嗯。Yeah， thank you everyone for attending this talk at this special time。

 happy holiday at the beginning， I start my presentation on the equivalentvalence of graphra convolutional convolution and Miup。

 this paper was coed by case Me anytime AI UCLA and RIS。

Yeah at the beginning I would like to introduce the basic idea of this paper。

 we build up we build a connection between the graph con and a mixup they are two different ways of two different technologies of deep learning but they have some inherent relations based on our observation yeah let's get started in case some audience may not very familiar with the graph convolal neural network so I introduce a little bit about the original graph convolutional works at the beginning we have a graph we have a target node a here and when we do the graph convolution actually is do a neighbor aggregation it's aggregate the neighbors features into As feature and then we do a。

That the very original graph condition on neural network， we just averaged the。

Maybe do a weighted average and then we get a updated node feature for a。

 which highlight in the green color the the feature after aggregation is a for。3 here。

So it is acknowledged that the graph conal enhance or enrich the representation of the target node A because it's just aggregate the features from its neighbors in this way its neighbors feature will enhance or enrich the target node so this is why we think graphra conutional works previously so in this paper we found another explanation for this。

 suppose we focus on the task supervised node classification task at the left figure suppose we only have a one layer graph conal neural network and we consider the label of node a here the label is a010 this is a one hot label the node A belong to the second class。

So this is a one hot label， and we use this label to supervise the aggregated feature。4 three here。

But for the when we consider the idea of the neighbor regulationation or graph colsion。

 the four three or the aggregated feature is obtained from the original features of ADCB highlighted in the orange color。

 so this means because we use the label。01，0 to supervise the aggregated feature 43。

And the fourth three is obtained by the original four features。 So in some way in some way。

 this is a this also means that we use the label0，1，0 also to supervise the original node features。

The original note features of ADCB， orange color， So this is what this is the basic idea of our paper。

This means we use the nodes As label to train node BCDs feature。

 so in this way we think we use the target nodes label to train its neighbors neighbors feature。

 so this is why we think the graph Conium works so well。Yeah， based on this。

 we have a such equivalent training data at the left figure。

 this is the original graph conal neural network。 We use a label 010 to supervise the aggregated feature and we will get an equivalent training data actually this data have a four data data samples the first one is a node a with label 0。

10 and the node D1 the feature is a 16 with the label 010 as well and also this is the same to the node C and D so。

Based on this， we think。Why graph conal neural works。

 This is because the labelless neighbors of a target node are trained by the target nodes label。

So this means the Ace neighbor is trained by ace label。

 so this is why we think the graph conditional neural networks so based on this basic idea we proposed our own method like we build the connection between this two mix up technology we can say we when we do the mix up with the equivalent training data we have a full data sample here data samples here。

 we do the mix up on these four samples and we will get a updated one the new features as a new label we can say this is the same to the updated feature of a so。

After the mix up， the node A creature and the labels are the same to the label after graph col。

 so this is a kind of relation between the graph colvolal convolution and the mix up。Yeah。

 based on this basic idea， we propose our idea， the graph convolution is equivalent to mix up strategy。

 so this is a very， I think is' a very simple idea here。

This is the whole framework of our work at the beginning we do a homo rela， which means we assign。

Maybe I need to introduce the graph a little bit， we have a full node here and the target node is the blue node。

Only the target label， only the target node has a label y zero here at the first step we need to rela。

The neighbors， the red node and the green node， also to the label Y 0。

The same label to node to the target node and then based on this we do a mixup we can see finally the features of the features and the labels of the target node。

 the the blue node are the same after the graph con and the mixup so based on this simple strategy or assumptions homoly rela we called it we build the connection between the graph col and mix up。

So the main results of our paper are here， the graph con is a train as well as as well in the test time mix up and two mild and reasonable conditions how re and as well the test time mix up。

 I will briefly introduce this to later in the experimental parts。呃。

The previous slides we showed the basic idea of our paper intuitiveively actually mathematically the graph col and mix up are also very similar we can see the first two equations the first the first line is a graph convolal mathematical expression We update。

的 feature啊。Based based on the target nodes labels and do the average。

 but we use the target nodes label， we keep the target nodes label as the original one and then we get the updated feature。

 but for the mix up there is only one difference we also mix up with the labels。We also mix up why。

 So this is a kind of the difference。 So when we reformulate graph neural network as a mix up。

 we only need to do is to re the。Neighbors label to the target node label。

 which I highlight in red in the second equations。So once we do this， we do the homoly label。

 we label the target nodes neighbors label to be the target node label。

 we will totally we will totally reformulate the graph conal。As a mix up。Yeah。

 so in this slides we a we build the connection between the graph solution and mix up mathematically。

Does under one condition， home of rela。Yeah， in the next I show a experimental results it's a very simple one just to do a verification on what our proposal is correct or not this is the prediction accuracy for node classification task on three on three data is its a very popular graph data call here and PM。

we only focus on two accuracy here， the original accuracy on these three data sets。

 the average accuracy， the graph convolutional neural network is 83 and something。

But when we use the MLLP and also with the mixup， we get the similar results， 83 and 57。

 So the the average accuracy on these three devices are quite similar。

 So in this slide we we build the connection between the graph and the mix up experimentally。呃。

And in this experiment， we show when we use the homofle relabel with multi layerer pathogen and versus the results of a graph conal。

Graph cons。We only focus on the red line and the green line。

The red line is the results of the original graph conal neural network。

 and the green line is the MLP user using homophily rela。 We just said re of the。

Relaable the neighbors and use the new data and use mix up on this new data and then train a multi layer pass we。

We can see the accuracy are quite similar， the X axis。Is the ratio of training data。

We use a different ratio alpha tri nodes。And the Y axis is the node classification。Accuries。

 we can see on this four data set。The home of the rela achieves the similar performance to the graph conal。

帮我螺实。And also we propose a we also propose a test time mix up and also versus the graph conclusion the text the test time mix up means like that we train a multi layerer pass。

We train a multi layer position with the using the original data。

 only use the original labeled node in the graph。 And then we do a test time mix up the test。

It means we train a multi layerier poit and then we use the trend weight。

In a graph convolutional neural network and then do the inference we see the results also say the red line and the green line we can see the green line and the red line are quite similar so in this way in this experimental we show that the test time mix up achieves the similar performance to the graph convolutional as well。

Actually， this phenomena was also funded by some previous works。

As noted in the bottom of this slides。And also， I do some experiment to investigate more on the test time mix up。

 basically focus on the decision boundary。 We focus on on the。On the figures in the left hand。

This is the two classes for basically is the two classes and each and each point here represents one node。

啊。The left figure， this figure is the。Re of MLLP。 And this figure is the MLLP plus pass time mix up。

We can see they have some difference here。we plot the decision boundary for different classes。

 we focus on the second rules， and we can see after test time mix up， the nodes。

 the representation of nodes become more and more similar。And as well for the for the class zero。

 it has a similar phenomena and in this way， it's made the note。Far away from the decision boundary。

 it will in this way it will increase the accuracy and as well on the figures in the right hand we have the similar results。

个 this this figure this。This rule。This column is the class。Is it a class？Is a class one。

And then this this rule is the results of multi layerer procedure。

 and this rule is the this column is the test time mix up with MLLP。

So we can see is also get the similar results is make the different class far away more far away from the decision boundarying。

And also， in the last experiments， we combined the homoly re and test time mix and the test time mix up together。

 we can see。The difference。诶。We can see the results of the graph conal neuralnatal and the mixup strategy。

 also in the red line and in the red color and the green color。

 we can see combined these two strategy together， the results are quite similar。

Its is except one data set here。 This data set is a kind of a different result， but for others。It's。

 it's quite similar， totally even the same with different training data split ratio。Yeah。

In this presentation I present the basic idea of our paper。

 we build a connection between the graph col and mixup for more detail you can refer to our paper you can get it HQR code。

 yeah we do a lot of other experiments on our paper as well and we provide some mathematical and analysis as well。

I can take questions。FromThank you so much for the great talk it's a really interesting connection between graph convolution and mix up。

😊，We also want to add on that Salin's talk is our first log TmLR track oral so congratulations thank you there's one question does your definition of GCN in experiments use residual connections and batch or layer norm between layers oh we don't because for for the。

😊，In this paper we only investigated the original graph commercial on neural network。

 so we only use the normalized a matrix and multiply the node feature and then use a function this is a one layer so we don't use other strategies because we want to keep it simple this is also the original formulation of graph commercial。

Got it。And do you have any intuition on how the connection between Mi up can contribute to improving GCN architecture or posting new architectures to kind of tackle bottlenecks。

 for example， on hydrophilic settings？Okay， okay， this is a great question I think the benefit of this relation or connection is that the first benefits is that we can use a mix up to accelerate the graph conal。

Graph con because originally we need to use the whole adjacent matrix this called a computational bottleneck。

 but when we use a mix up we only need to train a multiar procedure So this is one benefit and for the whole for the heatfoly graph you mean right actually I didn't do the experiment on the heterofos graph。

 but you can see you can see from from the mathematical connection between the graph conal and graph con and mix up we do not have assumption the graph must be homoly So I think for the heatfoic graph we need to do more investigation experiments I think maybe it's extended to that kind of graphs as well。

Thank you very much again for the fantastic talk。😊。



![](img/c4b0d8eef1833c42d9a658dd61469cc2_17.png)

Thank you and I think this wraps up today's oral presentation thank you everyone for joining and sorry for the delay at the start。

😊，Thank you Sa T again for the talk Thank you so much happy Thanksgiving。😊，You too。 Thank you。哎。没有。

A you before we close。😔，I'm not sure what the best thing to do is。

 but I'm guessing it's better to like stop the YouTube live stream and stop the record。



![](img/c4b0d8eef1833c42d9a658dd61469cc2_19.png)