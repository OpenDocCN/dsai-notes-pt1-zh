# 图机器学习会议 ｜ Learning On Graphs Conference 2024 p01 P01_Day_1-Yusu_Wang_keynote__KG-LLM__heterophilic_graphs_tutorial__orals -BV1k9pAzpE8S_p1-

![](img/612e339adc11f63d4a03e36f622b3f2f_0.png)

Okay。All right， I will let everyone in。Right， now I can see a lot of people。😊，Alright。

 good morning good afternoon， good evening wherever you are welcome to the tutorial session for integrating knowledge graphs and large language models so you are most welcome to engage with your organizers and each other in the Zoom chat function the Zoom Q&A or on the tutorial discussions black channel this session will be one and a half hours long because there is another event following this session please do end on time thank you very much and please note that the tutorial will be recorded and uploaded to our YouTube channel for those who are unable to join or who would like to rewatch the session in the future so if you were prefer not to be recorded you're welcome to keep your cameras off and just participate privately in the chat。

😊，Now without further ado， I will hand it over to our tutorial organizers。Hello， so yeah。

Thanks for everyone who joined our tutorial for integrating large graph and large learning models for advancing scientific research。

 So， yeah， I should say good morning。 Good afternoon。 and good evening for for where you are。

 So I'm Joan。 So I will give an overview overview of this tutorial and then introduce the first part。

 And we have another two speakers。 Doctor Chang Zhang。

 who will give the second part and doctor Zi Chaomeeng， We give the。😊，The third part。

 So at the same time， let， let me start。So， yeah， this is some brief information for myself。

 I'm a lecturer at Department of Comp Science University of Manchester。

 So I mainly work on knowledge graph on knowledge plantation and the techniques or smart web techniques and the integration of all the symbolic technical with machine learning and large learning model。

😊，So you can find my email here。 So if you have a question， you can also email me。So okay， before I。

 before we give three parts。 So we want to first give you some ideas why we want to combine large graphs and large leg models。

So。Lning models， lightning models are very successful in many aspects。 For example， it has。

 they have general knowledge。 They they are very good at language processing。

 They have high generalizability， but we also know that light models still have some problems like implicity knowledge。

 hallucination。😊，Indecisiveness， black black box。 And they may also short off some kind of domain specific knowledge or short of new knowledge。

 so。But we find knowledge graphs actually can complement large language model in these aspects。

Nowge graphs have structured knowledge， which are usually quite a， decisive。

 and knowledge graphs can give interpretable decision making。

 and knowledge graphs can have domain specific knowledge。 And also。

 it's easy to deal with evolving knowledge。So this is the general perspective about why we try to combine large graphs and large leg models。

I always， we also want to give some perspective from the dimension of scientific knowledge and text understanding。

 So you see， this is a very typical pipeline of using large language model for task addressing。

 So we usually pering a large language model with a lot of copper and fine tune such a large language model with task specific data and then uses large language model for prediction or generation。

However， there are several problems in scientific knowledge and text understanding。

 The one is new discovered knowledge。 So we may find some new scientific phenomenon。

 And how can we last like model know these new phenomenon and new facts。And also， you know。

 knowledge will be updated。 We have new， new facts。 Then these new facts will be updated。

 will replace the old knowledge and update the the， the human knowledge。

 Then how can we deal with this updated knowledge。 So both newly discovered knowledge and updated knowledge。

 there should be no enough training samples in there there should be no enough training samples。

 Then how can we let large model capture this new knowledge。And also。

 the third issue is long tele knowledge。 So there are some， for example， rare disease。Oh， or some。

 some phenomenon that do not frequently appear。 How can we。

 How can we get enough training samples to to let language model capture these knowledge。

 So all these three issues are big problems in applying large language models for scientific text processing。

So we can utilize knowledge graph to deal with all these things。

 because knowledge graph may should be more easier to be updated。 And so we。

 if we have some mechanism to combine knowledge graph and large link models。

 so we can easily deal with all these three phenomenons， Three issues。So after。

 after giving the motivation， I want to give the presentation of the first part。

 knowledgegraphs for science。😊，So this part includes three components。

 The one is about knowledge graph definition and some core concepts。

 Then I will give a simple case of using knowledge graph for equalcotesological effect prediction。

 And finally， I will very brieflyly review knowledge graph for life science and the the the challenges of life science knowledge graphs。

The start from the definition of knowledge graph。 So in 2012， Google proposed a term knowledge graph。

 So it is some kind of knowledge base that is used by Google to support its searching search engines result。

 for example， if you search Manchester Baby on Google。

 it will returns not only one page of Manchester Baby。

 but also thestruct data on the right side such as the data introduced developer memory all these information are supported by the Google's knowledge graph。

 So in this definition of knowledge graph knowledge actually is current to instances and facts instances could be something of entitiess like Manchester Baby。

 Fox could be some kind of relational knowledge like the developer of Manchester Baby includes Tom Kburn。

😊，So， in such a knowledgegraph。Actually， it's。 it's， it's quite relevant。 or it's quite。

 The definition is quite equivalent to link the structure data。 So we link。😊，D the data。

 and they form a multi relational graph。So then I want to give some larger plantation perspective about the definition of large graph。

 So we need different knowledge plantation languages to build a large graph。

 So first one is RF resource discrete framework。 So it is to。Describe it is two model facts。

With three components subject， predicate and object。 So， for example。

 we can have Manchester Baby a subject has developed a predicate and Tom Kbin as object。

 Then we can we can describe the facts that Manchester baby has developed Tom Tom Kbin。

 So you can find that if we have a set of idea of triple。 they can form a graph。

 which is multi relational， as shown on the right side。 we can have the relation。

 we can have the relation of the three entitiess about Manchester baby and its developers。😊。

So database have schema to describe as met data。 RDF facts also has RDF schema to describe its met information。

 For example， we can define that Alan Turing is a mathematician。

 Home King is computer science scientist and mathematician and computer science。

 both belong to scientist。And we can also define the property。 And sorry。

 we can also define the domain range of each property for constraining their。

 for constraining what they are connected。Besides a schema。

 we can have another more advanced language called web ontology language。

 So it is widely used to define taxonomies and vocabies。 For example。

 the ontology called food and for defining all kinds of food terminologies。

 another example is noma city， which is widely used for defining all kinds of medical terms。

 And you may also know gene ont DI D disease ontology。 on the right side。

 I show a picture about a segment of the ont of food you can see the categorization of food products from plant food product to the leaf gluten soy bread。

😊，We Web oil ontology can not only describe such taxonomy。

 but also constrain and logical relationship， which which are underpin the discrete logic。

 That means it can define some logical relationship with logical operators like conjunction， disn。

 extension quantify universal quantifier and negation。For example。

 we can define that food and material is equivalent to equivalent to。

Environment material in conjunction with something has a role in food， right。

 And we can also define the personalityality of parent is too。

 which means every person should exactly have two parents， right。😊，So what is knowledge graph。

 So let's revisit this term。 So by Google's definition， it is。😊。

It is actually a set of other effects。 They form a relational graph。And。

Many people would think that we can add a schema to audio audio effects。

 So schema could also be a part of a large graph。And many researchers in the KR community or Sw community or traditional AI community。

 they would also think that。Onts or onts in oil could also be regarded as a large graph。

 That means this is a graph in conjunction with some reasoning agent。

 or we can say oil on target is some kind of logical equi knowledge graph。

So after we have a knowledge of what is knowledge graph。

 So let's take another look at why we try to use knowledge graph。

 So here are some good features about knowledge graph。

 But this is with regard to traditional database， traditional data management systems like relational database。

😊，The first it's more intuitive。 It doesn't foreign key， but using a graph view。

 and it can support a schema with logical equi ontology。

 And it used usually to connect things of the world instead of local data。 It's also more flexible。

 extendsable when you can easily merge to graphs by getting linking by。

 by linking the entitiess in the notes。😊，And it also supposed some rule language to do reason over the over the graph。

 like data rules， And it can support other features of utilizing the characteristics of the graph。

 For example， navigation over the graph。And calculating the similarity over the graph and utilize the location of the graph for kinds of。

 for solving kinds of problems。😊，so now嗯。Notge graph is good， but people often complain。 How can。

Construction large graph is very cost。So there are different ways for constructing knowledge graph。

 The one is directly using crossscing。 The example， one example is Wiiki data。

 which is continuously added and edited。By volunteers in the whole world。

 there are some other domain on target like like the J on target mentioned above C R O created by human expertss。

 Of course， we can also create。Nge graph from existing thermo or structure data。 For example。

 we can build the knowledge， For example， we build the knowledge graph D P utilizing the info box of Wikipedia。

So briefly， the first solution for knowledge of construction is crowdcing。

But the second solution is more automatic。So researchers in open information instruction or web mining。

 they try to automatically in an instance relations and facts from unstructed text or semistruct web data。

 So they utilize information like name anti recognition and linking relation instruction。

They may also utilize the features of the， of the data on the web。

 like the structure of the H TM L page to utilize to extract knowledge from resources on the web。

 So this is automatic knowledge graph construction。So， you know。

 many of our data of the enterprises or individuals are stored now stored in， in。

 in some kind of structure format or semi format。 So， for example， databases， web tables。

 Excel shade， CB files。 So how can we construct knowledge graph from all these existing resources。

So there are two solutions。 The one is we can define some rules or some。

 some agent based solutions to transform tables to large graph。So this is quite， quite absolute。

 And from the scratch。Another solution is knowledge graph population。

 So this is perhaps more practical。 So assume on the right on in this figure。

 assume we have an existing knowledge graph about formulaula 1。 So we have some drivers。

 racing drivers， as well as their country and the teams， right， So。

 and we have another table about more information of of formula1。

 So we can do two steps to populate the knowledge graph with the with the table。

The one is doing matching。 We can match the first column with the type of race。

 We can match the type of the first column with the concept of racing driver。

 We can match the relationship between the first column and the second column to the property of race race 4。

Then we can find that some cells in in this table have no corresponding knowledge in in the large graph。

 For example， Hamilton has no corresponding entity。

 Then we can extract this new entity from the table。

 and we can extract the facts that Hamilton lives in England into as a new facts into。

 and in this new factor into large graph。 So this is large graph construction or population utilizing。

Uil。Stionstructed or semiconductor data。Of course， we may have a lot of knowledge several knowledge graphs or onology。

 How can we integrate some knowledge graphs or onologies for a new knowledge graph。

 So there are several steps One is we can do， we need to do mapping。

 We need to find the equivalent entities between two knowledge graphs。

 and then we need to do modization。 We need to extract a subset of the knowledge from one knowledge graph as。

 as the knowledge you want to build the new knowledge graph， right。

 And you also need to do something like canonization。 So for example。

 you have two entities which are regarded as equivalent。

 and you need to merge the two entities to make to make the new knowledge graph more formally represented。

😊，So this is data integration for auto graph construction。

Heres a bit a small advertisement of our on work on deeper on。

 This is a light model based on target engineering library。

 So it provides some tools that you can use for on target alignment for on target completion and for also for on target population utilizing external resource like documents。

 So based to implement these tools we also provide basic APIs for on target processing。

 So it originally all on target processing APIs are based on Java。

 So we are reimment or package these APIs into Python， such as that。😊，啊。

Touch that they can be easily integrated with ILP or deep learning libraries for for kinds of functions。

So the second component of part one is a simple case study for utilizing knowledge graph for equal tasological effect assessment。

So this is Etological effect assessment in Norwegian Institute Water Research based on experiments。

 So assume we have some exposure about chemicals in the water。

 then we can get some risk quots and find some potential effects between species and chemicals。

 then we go back to the labs and test these potential effects between species and labs and we find some do have effects and some do not。

 And we can we can further come back to combined exposure and effects and analysis， other chemicals。

 other related chemicals and other species and do another round of experiments， right。

 but if if But if when there are more species， more chemicals to consider the combination space is big and you need a lot of experiments for for testing。

 right。😊，Its a huge waste of experimental resource， and it also has some aesthetic issues。

So we propose， propose to use knowledge graph embedding and link prediction to address this problem。

 So this is a Ph D project from。Yeah， this is a PhD project。 So。

 so the idea is to construct knowledge graph with a species knowledge， with the chemicals knowledge。

 together with some existing experimental results about chemical species effects。

 And we build this knowledge graph。 And we can predict further links。😊。

Further the links between species and chemical chemicals to find the potential effects in this way。

 we can make a filtering before we doing experiments。So in this project。

 we mainly focus on one effect， which is called mortality。

 This is some lethal concentration of the chemical to 50% of test population。

That lead to death or the lose of generation of next generation are measured at 48 hours。

So how can we construct this knowledge graph， So we need to select these sources。

 So for the biological set， we select the equalt logic database。

 which covers 1 million testings 12000 compounds and compound main chemicals and 13000 species and over the 0。

6 percentage of the chemical species， it has experimental results。 and it means 99。

4 percentage of the species。 they are missing they miss effects knowledge。

 and we need to using the embedding model to predict them。So for the biological side side。

 we use the N CBBI technology about species to get more features of the species。

 We also use the encyclopedia of life。 and for chemical side， we use three onts。😊，PPcam。

 Campbell and M， E， S， H。Then how can we integrate the different resources， So first。

 we use the resources on wki data。 So we did provide some mappings between species from encyclopedia。

Ecoological database and some species in N CBBR taxonies， For example。 And second。

 we use on target alignment tools。 We use log map and AM L。

 which are too very typical on target line system based on electrical matching。

 indexing and symbolic reasoning。Because this project was conducted five years ago。

 and now we can have some alternative， like bird map introduced above in In deep or introduced above。

对啊。So this is a picture of the knowledge graph we constructed。

 So this knowledge graph actually includes three parts。

The one part is about is a sub sub graph about taxonomy。

 The second part is a sub graph about chemicals， and the third third knowledge graph is about third sub graph is about the effects between chemicals and species。

And this knowledge graph is， is managed as audio of triples， and it can。

 it can be accessed by sparkle queries。 And we have open sourced this knowledge graph on。

On thato and also， as well as the codes for constructing this knowledge graph。So this。

 here are some triples about this knowledge graph。 I think I can omit them。

 And this is a picture about a segment of knowledge graph around a specific test。

 So this is an experimental test。 And you can find it is it is for specific this specific chemical。

 And this is it is for specific this。😊，Fcious， and you can find the。

 this testing it happens in the water。 And also the result， this is the result。

 whether it has a mortality relationship under specific concentration。With the large graph。

 we can do link prediction。 So we adapt two solutions。 One is very naive。

 which is just to feed the embeddings portrayed by some embedding algorithms like trans E into a fully connected neural network。

 So here we use two fully connected neural network。 One is quite simple。 It just con light。

 the embedding of the chemistry chemical and species and feed it into a fully connected layer。

 Another is adding。😊，Fully connected layer sub to the embedding of the chemical and the spec and contaminat the the output and further feed them into another fully connected layer。

So the second solution is end to end。 So it simultaneously train the knowledge graph embeddings and the material perception。

 So it has three losses。 The first loss is the embedding loss over the triples of the chemical sub knowledgege graph。

 The second loss is the is for the triples of the spec subnge graph。

 And third loss is the predicted is comparing the predicted output of the material perception with the effect in the effect subnge graph。

 And we can。 that means we train the material perception together with the three in with the chemical subnge graph and spec sub knowledgege graph embeddings。

😊，So we explored quite a few large graph embeddings ranging from chance A to rotate take E P， Ra E。

 and we also explore different sampling strategy。 And in the end。

 we can take a look at some segment of the result。 So we find under the best setting the sensitive could be larger than 0。

9。 and the specific could be larger than 0。75。Under the combination of this are these two embedding algorithms。

 right。即诶。Before I go to the cellular components， I would like to discuss discuss about using knowledgegraph for addressing such scientific discovery problems from a knowledge graph perspective。

The first is knowledge graph construction。 So it's always hard to construct a knowledge graph con old data sources。

Currently， we use some structure data， but and existing knowledge graphs。

 Can we consider literatures and reports， They may include new scientific knowledge。

 And how can we extract these new scientific knowledge facts and add them into the new knowledge graph。

 And there are a lot of。 there are quite a few specific experimental system。

 And they use some specific language for annotating the output results。

 How can we extract these results and insert them into knowledge graph。

And then how can we consider multimod data besides direct the data， Can we consider the text。

 images or even sequences like sequence of the genes。Plink prediction， of course， we try to。

Get better better result， which means not only acive， but also explanation。

 So can we use more advanced embedding technical， Can we consider larger angle model for auto graph embedding together with interpretation。

 And can we consider symbolic reasoning in conjunction with these embedding based technical。😊。

So in the end， I try to use two minutes to introduce some review and challenges about knowledge graph for life science。

 So briefly there's only one， briefly there is just one page to introduce。

 and this page give the sumization of a recent perspective paper called knowledge graph knowledge graphs for life sciences。

 recent development and challenges and opportunities。 So it includes three parts。

 we give some literature review about knowledge graph construction and management in life science about life science knowledge discovery about knowledge graph for explainable AI。

 And in this perspective paper， we we consider several kind。

 we reviewed several kinds of knowledge graphs like schema life knowledge graph。

 schema based knowledge graph simple ontology。Like taxonomies and expressive oil ontologists with logics。

And we also discussed quite a few challenges， like the scability in scientific discovery。

 together with notgraph construction and and also human interaction and explainability and multimod and multi domain and multimodality yeah。

 and also about representation learning， how to combine symbolic and sub symbolic representation。

 So finally， I need to mention that this prospective paper is published in a new journal called Trans or graph data and knowledge。

 So this is a fully open source open access journal。 this is to in the community of web。

 this new journal is to replace the original journal of web ses as the letter is non open access。😊。

Okay， so this is all about my part。 and perhaps I do not have time to answer questions。

 and you can answer questions by email me， but perhaps you can answer question in the select channel。

 I think， yeah， I'm on the select channel。 and we have created a channel for exactly for this tutorial。

 So you can ask question in that channel。 we， we will also upload the slide。So， yeah。

 that's all my part。 And if we have a time in the end， after all three parts。

We will also be in the in the zoom， and you can also ask questions at that time。

 So I would like to give my flow to Doctor。 Qang Zhang for second part。Okay， thank you。 Yes， so I is。

 so can you hear me。いや、。And also， you are able to see my screen， I believe。Can you see my screen？Yes。

 it's good to me。Okay， cool。 So let's get started。 Let's move on to the second part of this tutorial。

 So hi everyone。 My name is Qiang Zhang from Zjiang University。😊。

And for the second part of this tutorial， I would like to guide you through this part so in this part we will focus on scientific large language model。

And so to give you some background， I'm currently a tenure track assistant professor at Z University and before that I got my PhD degree from University College London in the UK and I also worked as a postal research fellow in UCL and even earlier and so I got my master degree from Chinese Academy of Sciences and my bachelor degree from Shan University。

 so my research interest is in data efficient machine learning。

 foundational models as well as some applications in natural learning process in knowledge graphs and AI for science。

Okay， so。In this， so here's the road map for these second part。

 I will start with a brief introductory presentation of scientific large language models followed by some technical details of how large language models can be leveraged and so。

Can be leveraged to deal with scientific data， such as proteins and moleculars。

 And in the in the in the third section， I will summarize the current technical challenges in this field and will also point out some promising direction research directions in the future。

So let's get started with the first section。 so as we know large language models have revolutionized artificial general intelligence。

 so it originates from the natural language processing area before the emergence of large language models。

 people use some upstream techniques such as sentence passing analysis and semantic analysis to try to make computers understand human languages。

And on top of that， researchers can conduct some downstream tasks such as question answering。

 knowledge extraction and complex reasoning， but after the emergence of large language models。

 a lot of things have changed okay so with between large language models such as GT GRM and Lama。

 people don't need to deal with the previous upstream techniques。

 they can simply use chatGPT or cloud to answer questions that extract knowledge and build knowledge graphs。

Okay， and after that， more advanced large language model systems have been developed and proposed。

 For example， hugging GT is one of the is one of the complicated models systems。

 So with this hug GT framework， GT could act as a controller in this system and。

It can conduct a task planning when when it receives a user request。

 and it can also try to select proper AI models according to their function descriptions and executes each subt。

And gather the response from these AI models and finally output the answers that the users expect and another model is any GT which is published earlier time in this year。

 so it could tokenize multimedia data into discrete tokens and in that way。

 it can achieve any280 transformation between multimedia data， such as image text video。

 audio and so on。And a lot of guys have must have heard so， which is developed by open AI。

 It is a very complicated large language models， multimedia， large language models。

 and it can generate coherent videos。According to human textile description， which is really amazing。

But large language models， I mean， general purpose， large language models are still kind of limited。

 although they are good at understanding and processing， generating human languages。

 they are not able to understand scientific data such as protein sequences genomes and chemical moleculars。

So suppose we have the information about we know the name of a protein。

 we also know the gene and we also know the sequence of a protein and we input all of this information into a general purpose large language model such as GDPGBT and we ask the model what is the soluability of this protein。

Well， in most cases these large language models are not able to give a proper answer。

 this is basically because these models were not properly trained with this scientific cor and with the words of Lu with Kingstan the limits of my language mean the limits of my world because these general purpose large language models were not trained with this scientific data it is reasonable that these models were not able to properly understand and deal with this scientific data okay。

And that's why people try to extend the limit of general purpose large language models and make them understand。

 make them grasp the ability to deal with scientific data。So so basically。

 there are some similarities between human languages and scientific languages。So in biology。

Researchers and scientists usually deal with the proteins and genes。 So for proteins。

 there are there there are 20 amino acids which could make the vocabulary while in terms of gene。

There are four different types of nucle basees， and these nu bies could make up the vocabulary for genome language。

And similarly in chemistry， chemical scientists have also formulated their own molecular language system。

 for example， smiles， deep smiles and cell files。 So they have their different grammars and vocabularies。

 but basically they use they use different symbols and characters to denote atoms。

 chemical bombs and substructures。And they also have their own structures to define the validity of a molecular。

 Okay， so when we try to formulate some when we want to develop some scientific。

 large language model， we usually make some we make comparison with human language。

 large language model。 Okay， so in natural in human language processing。There is a very。

 very fundamental hypothesis， which is called distributional semantic hypothesis。

 it means that the meaning of a world is determined by its context okay and with this hypothesis。

When researchers try to calculate the joint distribution of a sequence。

 they have developed different architectures where basically to two architect two type of architectures。

 one is B and the other HGPT So for the broader architecture。

 people try to mask some tokens and then train a model try to recover the mask tokens according to the provided context and with the GT architecture。

The model is trained to auto aggressivelygressively generate the next tokens by using the previously generated ones。

 okay。And similarly， we could also develop such architectures， but we should。

Which we need to consider。Whether such distributional hypothesises still hold。

When we deal with moleculars and proteins， Okay， so a lot of researchers say， maybe， okay。

 so maybe we we could guess this hypothesis poor， and we can continue with such hypothesis。

And then which motivates a lot of researchers to study on the scientific large language models and I will introduce you guys some technical advancements in this field okay。

 so the broad the scope of this scientific large language model could be very broad。So first of all。

 we definitely could include human language into this area。

 but such human languages could kind of different from the general domain we usually use some text and human languages from from research papers from patternss or from。

Textbook。And then in chemistry， we will see some chemical molecular languages and in biology。

 we usually need to deal with protein language and gene language。 and of course， we in some cases。

 we need to we can combine。 We can mix up these different types of scientific language。

 and then we could have a multimod scientific language system。So finally。

 we can also talk about scientific agent and embodied AI。

 which is kind of different from the general program。Yes。Yeah。哎，把强。好的，可以。We lost the voice right yes。

 we lost sound I was just double checking that it wasn't just me Qiang， can you hear us。Hello， Che。

I think he encountered some Internet problem。啊 sure嗯。yeah， I'm messaging him。 let's see。

I think we can also answer some questions if you have。 So some， yeah。

 some participants messaged me and I have replied one question。 So if you have more question。

 you can also ask。Okay， chance here。 Okay， and we can continue。Hi， can you hear me， yeah。Okay。

 sorry about the Internet connection。So where I should continue my talk。From this slice or even。

Earlier here， or here。We got to the part where you were talking about scientific agent and embodied AI Okay sure cool yeah。

嗯。Yeah， so because of the broad scope of this scientific large language model and the limited time I'm not able to cover every every part。

 so I'm just gonna focus on two aspects。 One is scientific text and the other is biological protein。

 Okay， so let's start with the scientific text。So basically， scientific test is still text。

 It is human language， and researchers have developed。那。A very common way so。For example。

 in biology and chemistry。So researchers use usually use birds GPT or GOM structures to。

Prettrain and further fine tune their models。So this table will summarize some good models in biology and chemistry and even some further comprehensive area。

And so it shows the name of these models， the publication time。

 the number of parameters of these models， the base model they used。

 the pre training data set and whether these models are published are open source or not。

And here we also list some general cors that are often used in biology in chemistry and so well okay。

Anda。So encoder model is a kind of。Commonly used architecture and and。

Usually I would like to introduce bioioB， which is pretrained on Pubcam and PMC and fine tune on some task specific data sets。

 so it is good at understanding text， it is good at named entityT recognition。

 relation instruction and question answering， so with the named entityT recognition and relation instruction。

 people can use it to build knowledge graphs。Okay， and。

Decoder only architecture is another commonly used architecture。For so for this architecture。

 I would like to introduce the div series model。 It， it is， it is based on the 7 B base model。

 and researchers have tried to。Build a fine tuning datas。And some question and answering data sets。

 some instruction data sets to fine too the base model， So at the end。

 the Darwin MDP model is very good at answering scientific questions。And go at summarization， okay。

And in terms of encoder and decoder architecture， a Sine G LM is a good model。

 which is published in the early time of this year。

 So the authors of this paper propose a self reflective。Instruction data set to make the model。

Reflect their output and try to correct the misleading parts of the output。 And they have also。

Examine examine whether a larger model could lead to a better performance。 And the answer is yes。

 So this figure shows the results。Okay。And when we try to evaluate such textual scientific large language model。

 researchers developed a lot of benchmarks。So this table summarized the typically used benchmarks。

 including MM M L U， C E， A G I E， S， Qe， Xie Zhi， Si evil， SQ， Si Ben， and S assess。So basically。

 this data sets contain questions and answers from different subjects from different levels。

 and it could be in different formats。And one benchmark that I want to highlight is text。

 it is called sign no evil， it draws ideas from the Chinese just traditional Chinese philosopher cons。

 and there are five different levels in this evaluation benchmark， which are knowledge coverage。

 knowledge exploration， reasoning， safety and application。And according to this benchmark。

 the GT series models are still among the best performing models， okay。

So that is the the texto scientific large language models and the other one is protein language models okay protein language models is kind of similar to human languages。

 it can be formulated with a sequence of symbols， but they have all their own characteristics。

So it has， so a protein has four different levels of architectures。 The primary structure is a。

Squence of amino acids， and it has also its secondary structure。

 the primary structure and qua structure。 Okay， so this table summarized the recently published a protein large language models。

It can still be organized in three different architectures。 The encoder only。

 the decor only and encoder decoder architecture and。We also list the publication time。

 the parameters of these models， the base model they use。

 the pretrain data set and what kind of capabilities they are they have and whether these models are open source or not and these models are usually have so their parameters are usually at the level of 100 million 1 B。

10 B or even 100 B， but most of them are at the level of 1 B and 10 B。Okay。

 so the training date could be from the public。Database such as unefF， P farm and Swissporting。

And with this trained data with these pre trained large language models。

 researchers can use them to do protein function prediction， protein family prediction， protein。

 protein contact sorry it's a amino acid contact prediction。

And people can also use it to design some normal protein sequences， for example。

 sequence optimization， protein deormal design， and inverse folding。Okay嗯。

And so for the encoder only model， I would like to highlight the ESM model from the metata AIT。Okay。

 so it is very similar to the bird architecture board。

 but it also incorporates the evolutional information from the protein domain。And。

So this ESM model basically tries to extract the sequence information into the pretraining objective function and another recent model is prompt protein which tries to incorporate more structural information into the pretraining objective function。

 So here there there there are three different pretraining objective functions。

 one is still very similar to B it is mask language modeling。

 but the other two are3D structure prediction and fourth level structure prediction and in that way more structural information could be incorporated into the during the pretraining stage。

Okay， the other， well， in terms of the decoder only architecture。Progene model is a very good one。

 It is published in nature biology in nature Bitechnology in last year。 it can accept。

 well it follows the GBT architecture and it can accept control tag as input and generates protein sequences。

As the output and the generated protein sequences are supposed to hold the well their functions of this these generated protein should be this control tag。

 there is a one too many corresponding relationship。Okay， and because of this。

 the strong capability and the generated protein sequences are usually normal。

It has been widely applied to enzyme design and therapeutic protein design， like entity， sorry。

 antibody body design。Okay， and for the encoder decor architecture。

 so extreme more P G L M is a very large scale protein large language model。

 It has the 100 B parameters， which I believe is the largest。The the largest protein language model。

 and it adopts the DLM architecture， which allows it to。To。

Do the protein understanding tasks and also generate normal protein sequences。Okay。

 and this table summarize the data sets for for protein protein training and benchmarking。

So for the pre training， usually researchers use unIRF dataset sets and also some structure related dataset sets like alpha4DB and in terms of pre training。

Sorry， in terms of a benchmarking， researchers can use discriminative tasks like classification and regression。

 but the generation ability of large language models。Well。

 it is more attractive and more interesting。And this leads to another problem。

 So how can we evaluate the generated protein sequences。

There are some computational based evaluation computational based evaluation metrics。

 such as the noveld diversity and protein sequence sorry。

 the protein distance of the affordability and recovery。 So with this metrics。

 computational scientists can。Computes， whether the generated protein sequences are novel or not。

 How similar with this generated sequences with the existing sequences and whether this this protein sequences could fold into a given 3D structure and。

And so on。 But such computation based evaluation metrics are not that reliable， and in most cases。

 we still need to do we level experiments。Okay， so that is the technical details about scientific。

 large language models。 and then I will move on to the third section。

 and I will summarize the challenges and some perspectives in this field。

So we have talked about scientific symbols and languages， for example， chemical moleculars。

 proteins and genomes， they have their own structures。

 they have their own vocabularies and grammars and accordingly researchers have developed some molecular languages。

 genome languages and protein languages and some multimod scientific languages。

There are still some interesting problems that we can explore in the future。 For example。

 compared to natural language， large language model。

 the skill and quality of the and fine data sets in scientific data it is not that gold and more importantly。

 theres there's a lack of cross cross model data sets。 So we must build such data by ourselves。

 If we want to build some if we want to develop some multimodal large language scientific large language model。

 Okay， and then we need to deal with the longer sequences。

Usually protein sequence and genome sequence contains so they are much longer。

 they have like hundreds or even thousands of tokens in a sequence。

 which is much longer than human language sentence Okay and then we we can somehow we must incorporate and properly utilize the threeD information and then the autoregressive learning problem could also be improved because well human human speak word by word from left to the right or from left or from right to the left。

 but protein sequences and genomes are not so theres no it is not unique directional okay and for model evaluation。

 we still。Most in most cases， we still rely on we lab experiments。

 but I think a good way is to try to reduce such dependency。And make the iteration faster Okay。

 and we also need to think about data privacy modelbi and give equal access to different organizations and and researchers。

 Okay， so to help you guys have a better understanding of this tutorial。

 I will also give you this some links。And some relevant materials on。

So here is the a company service that we have。We have uploaded to archive and also some Github repository and some other service。

Like a general introduction about AF science， the chemical molecules and biological proteins。 Okay。

 so I believe that is the end of this， this second part。

 Thank you very much for your attention and participation。 He， thank you。

 So next part will be shift to doctor Z Meng。 Okay， Thank you。Let me share my screen。Yeah。

 I think I will need to stop sharing on。Can you see my screen， okay。O。对。Okay， so hello， good morning。

 good afternoon， good evening everyone， so my name is Zi Xiao。

 I'm a lecturer at the University of Glasgow。So I'm doing some research in like AFO biomedicine。

 information retrieval， nurse graphs， law savings models， allan based agents。

 and AFO scientific discovery。So in my part， I will be covering some large incorporation frameworks for LLMs and the N drop integration for scientific MP tasks and also the scientific prediction tasks。

So in part one， Johnny has already discussed cons and pros between the Ls and knowledge graphs actually in general theyre like combining knowledge graphs and LMs have three paradigms the first time paradigm is the KG enhanced the Ls which is use KG to provide some fact energies or domestic technologies for Ls to improve their accuracy or symbolic reasoning abilities。

 the output of such a paradigm is actually a LM。And the second paradigm is actually the L augmented knowledge groupss。

 so we use LM to generate knowledge or use do some language processing to improve the generaliz abilities。

 and then the output will be some like Kg related tasks of the nu groupss。

The third paradigmM is actually the synergized LLMs plus KGs。

 where we use LLMs to learn the nu presentation for KGs and the KGs could provide some fact into LLMs。

 but in this studio area， I will only covering some like large enhanced LLMs which incorporate KG during different stages of LLMs and for the purpose of enhancing the understanding of the knowledge learned by LLMs。

These include some tasks including like N web pre traininging， N integration functionning。

 and N editing and N and learning， etc。So actually integrating ST technologies can occur at any stage of the development of Llums。

For example， it can be happened during the progen stage。

 we can indirectly use not2 as the progennic copper， some examples such as the pre KGE。

 earning and more XT。And CO also can happen during the post trainingning I mean post trainingning is like after pre trainingning。

 we can design a separate objective that specifically for some larges or some other informations some examples such as separatebi that we use like inject some syninonme information into the bird model to enhance the ability to do enterlinking and also like MOP。

 the mix of partitions on top protein etc。It also can occur during the feuning stage where we can find two the NLM suites directly with the N lab so that we can inject some nu information for some donxing tasks。

And some example include the fus DTI， Fu DT GA， and also separates have a lot of applications that can be fine tuned for some donstream tasks。

It also can happen during the inference stage where we don't need to do any training。

 but just put them into the LLMs as a prompt， especially in current stage we have the retrieval augmented generation we can actually retrieve some large graph or some large ships and then use rack to enhance the performance to inject some large into the language models。

 some example include M rack or biore。So in the second part of tutorial， Dr。

 Zgsang has already discussed the by encoder and actually。

 the by encoder offers like the efficiency encodings of individual entities can speed up retrieval and computation。

 but may sacrifice like final grant interactions， between different encoders。

 So it actually could achieve higher efficiency compared to cross encoder， So for cross encoder。

 we encode entities jointly and capturing some more detailed interaction。

 but at the cost of the greater computation resources and time。

 but it could achieve higher effectiveness needs compared to the byenr models。

 So these are basic like architecture that we can use to inject some large bulbs。

Here is an example of using the buying encoders for the drug target protections here we try to inject some interactions between the protein and drugg and then we use the like support protein encoders to encode oiddia acids into embeddings and for the drugs。

 which is actually in the reation of selfie， which is kind of replanation of molecules。

 we can encode them by using some molecule lens models such as cellform。

And then this that is pass over to the like Fus model to use a token level fusion model to effectively learn those fine grant information for the jack target interactions。

So more more XPT is another example that using a cross encoder for the more protein of predictions and actually is a unified language models of text and marks prechant on the smell strings and wrapped by the text so。

In the process， they construct some like text and the smells hetero tokenized separately。

 and this text and smell interaction was integrated during the protein chain stage and then further conude on some specific dostream tasks such as the molecularcur property predictions。

 mecur tax generations。So in terms of the fine tuning stage， apart from the full fe tuning。

 there are many integration techniques of LLMs which aims to enhance the efficiency。

 these techniques also code like parameter efficient tuning， some examples such as like adapters。

 rapid tunings roll flood big fits of prompt tuning。

 for these techniques they only vent tune a small set of parameters and keeping the original parameters fixed and try to reduce the computational cost。

 so as we can see that actually in different resources。

 they actually achieve quite different performance over different like data size。

So they're particularly useful when we fine tuned super large lecture models where in some like low resources settings or when the computational resources is limited。

呃So。Nararch editing is a particular settinging of the large integration for alums。

s the motivation of knowledge editing is that LLMs are neormously heucinated。

 perulated by and factually decay so we should be able to adjust specific behaviors of protein language models so these like large editing normally happens during the post training stage and it is like some techniques in the interaction of like prime tuning。

 continue learning machine unlening generations。Given it like knowledge。

 it will try to locate the information within light neuronss and then conduct some like operation such as insertion。

 modification eraions so that the information on launch can be updated for Lms。啊。

So this always happened during the post training stage。

So that's next look at the KG integration for the scientific onP tasks。So in general。

 there are many national links processing tasks related to scientific discovery。

 including like question answer， entity linking， document classification， summarization。

 note generations or hypothesis generation of large growth compil and reasoning。

 so I will describe some introduce some examples related to these tasks。

So here is an example of the KG integration for the clinical text state generation。

 so the colion is a largely important framework for the clinical date generation they use like topics from the KGs or topics from others and cells from as to formulate the prompts and they use the synthetic data and they use the synthetic data to be fine tuneed fors to further improve the general like domestic specific knowledge。

啊。So this like integration is actually done during the plauning stage。

Here is another example of the N integration during the feunum stage。

 this is like a KG based LLM agent with complex medical QA that leverage some no cod knowledge of LLMs and structured cod knowledge of medical concept using LLMs。

So in this model， upon seeing a query， this work generates some relevant triples by using the launch base of LLMs。

 and these triples are then verified against a grant a granted knowledge box to feed out some error information to ensure that only relevant information it can contribute to the final answers。

It also used like a Rola like prim tuning strategieses。

 so it was happened in the fine tuning stage and used Roa as the techniques。

and here is the example of using the N enhanced Ls for the question answering tasks。

 it actually contains like formal stages， the first like efficient construction labeling a limited set of examples and developing a based N graph extraction system to construct a the KG and and then they use a pre prelening techniques with like cableroll。

 which is can of post the chain stage try to integrate such like domestic specific nu graphs loan like Ro based fineunning and then they can they will do another stage of the cells super fine tuning。

 which involves retrieve some sub graphs from these domestic cageGs and modified input according accordingly。

And perform some supervised tu and then in the final stage， theses also will act as evaator。

 provide some like feedbacks on the large correctness and so that the model can better align the domain knowledge。

 So this work actually used both like post training and function tuning stage to integrate this large informations and the roll up is used to enhance the efficiency for tu。

In terms of the inference stage， the retrieve augmented generations was widely used to gain some large enhanced generation for the current airlines。

The bear right is an example that could adaptively select a nurse source and domestics tour to advanced biology question answering reason task。

So in this model， they use like LLs to do some kind of retrieval selection to select some suitable tools or retchirievalous method to retrieve the domestic efficacy documents and then use another prompt system to formulate the question for retrieval and then combine this retrieevval results for their context for the next step。

 and then they do kind of transfer of reasoning and before they generate the answer。

So this is actually happened in the inference stage。

 there's no any tuning and especially it's very useful for the current stage of LLMs。

Sometimes women need to deal with like super large scale knowledge graphs， for example， the UMSs。

 which is a one of the largest biomedical knowledge graphs containing formula concepts and more than 20 million relations。

And so how to deal with this large scale graph， the model mix of a mixture of partition gives idea of divt and conco where they actually use like a partition algorithm to partitioning。

Pratition the graph into some small graphs， for example using mattes。

 which is graph position algorithm and infuse these domestic specific knowledge of each subgraph into separate adapters so each subgraphs will injected into the separate adapters for the functiontuion stage they can add a mixed layer on top of these adapters。

And fine tune on some application， domain specific applications。 So using this scheme。

 we are able to deal with some large scale noises， and this is also using some primary tuning and this is happened during the vent tuning stage。

 a post change stage before the vent tuning。So also this。

Look at some KG generation example for the scientific prediction tasks。

 there are many scientific prediction tasks including the gene disease Association prediction。

 protein function prediction， drug repproposing drug target interactions。

 and protein to protein interactions， text to monitors。A many acid to contacts prediction tasks。啊。

I will briefly introduce some of the tasks and describe how we can use LLlums and knowledge to improve their effectiveness。

So here is an example of the gene disease Association prediction tasks。

 so the gene disease Association prediction tasks is a process of identify which gene are involved in a disease so for the gene we actually don't have like a very good but for some gene we can map them into a protein so we can go the protein sequence we can the wind acid sequence for the disease we actually don't have like apart from text。

 we don't have any other information， but we can actually use the setic distance between different disease so that we can use the text as the disease as the disease representationations so efficient DNA is a model that use fine encode encodes to do the disease gene disease association task and use a。

Bird model， apartment bird model to encode the disease description to encode the disease into the text embeddings。

 replanation beddings and encode gene into。Also using a protein language model to encode the gene represented by the protein sequence into the vector vector space and then further use a futures model to to a prediction hat。

So this is I by included models and also combined with the post training because they use both the general like gene D association。

 copper and further fineitude on some task specific benchmarks。And。

YeahOntop protein is a model that could deal with many different protein related tasks。

 such as protein function predictions， protein to protein interactions this model actually constructs a normal large scale knowledge graph that consists of the gene ontology and it is a related proteins and gene annotated text for protein sequence describe allos in a graph。

 So then this Kg then was integrated into a language model by using a normal contrasting learning and to jointly optimize the large graph and protein embedding space during like postal training stage。

So this is actually happened during the post training stage and then they will need to find two on some domain specific tasks。

 such as like protein structure protein function prediction or protein to protein interactions。

So it compares the post training and fine tuning。啊。

This is a task we call the omin acid contact prediction。

 which is a process of identifying which aminomin acid residues in a protein are closed together。

 so here this model called the keep is chain on large graph that consists of five million triples of the protein Kg25 and explore the large graph at more granular level by applying the cross attention again the cross attention。

to the secret of the protein embeddings and the language embeddings。

And it can be trained also using the mask language modeling heads。

 and it is also a binr architecture that encode protein and language model language inflammation are in parallel。

The drug repurposing is a task that widely used to seek to identify new drugs for new targets for drugs that has already approved for the treatment of the existing disease。

It was normally traditional it was normally addressed by some graft neural networks models such as like TxGLN。

 which is a graph foundational model for the zero shot drug repurposing。

 amified the therapeutic candidates even for the disease with limited treatment options or existing drugs。

It trained a based on medical knowledges and use graph neural networks and metric learning models to rate those drugs as a potential indications。

But now with the development of large legs modelss， particularly HIVBT。

 this tasks can be also addressed by using like TBT。

 so this work was like leveraging the HIVBT to recommend some drugs for the ashe disease for the ashe disease repurposing tasks。

And it actually further evaluate the potential efficacy of the 10 most frequent suggested drugs generated by JBT and using the electronic record data to validate the effectiveness and the conclusion suggests that JGBT can generate actually very high quality hypoides for drugg repproing。

So and previously， Dr。 Zhang Shahan has already mentioned that for the language models。

 we actually need to deal with different mod of the nursecrafts。And for example。

 we have the proteins which can be represented by the oin acids and we have drugs。

 which is represented by the mergers how to combine these different modalities into language model to do different tasks is actually a challenging task。

 but it has been researched by many papers they are basically like architecture to merge these multimodality information of knowledgecos into language models。

 The first is we call the multimodal congestive learning。

 One example this is actually borrowed from the computation， the clip models。

 which learn the alignment between the text and image， but here we try to learn some representations。

 by by aligning the protein representation of the drug representation。

 so both the foundation models the multiple foundation models will be fine tuned for example by some constive。

何？The second architecture is。IIs is try to align to like a central mod。

 This is also borrowed from some current visual language models。

 where the image data or video data can map into the unified presentation of the text language models。

And this can be applied into the scientific discovery of domain as well， while we can， for example。

 map the molecule lens models， the representation of molecules and rep of proteins into a single text lens models。

 for example， lemma， right， then we can keep the lemma model being fixed and fine too。

 those protein lens model and molecule lens models。

The third architecture is kind of a transformation across modities。

 which they call the bridge models， which is try to do some transformation from two different modities。

 but they will update the parameters only with the transformational model。

So apart from thes scientific discovery， the LM agents is a very hot topic recently。

 and it could also apply to the scientific discovery domain。

 there are two main paradigms of these like scientific discovery agents。

 the specified specialized language model， which are chain on some like scientific data。

 which are typically tailored by some scientific tasks such as molecularcur related tasks。

 protein related tasks and gene tasks。And these models are used as tools to perform some specific tasks in which users actually provide information required for a task。

 and then the model output the prediction。Another paradigm is actually the general purpose language models where they are trained based on some diverse text information so from different materials。

 including like scientific papers， archives， some things。

 and they are fine tuned by some reason task or planning or information retrieval task。

 but they are serve as like assistant like usage， allow use user to use like plain language to interact digital the model。

In scientific discovery， another key challenge is actually the co because scientific is a co process。

 so this paper actually use the idea， try to build the connection from the text data。

 they try to overcome some unseen connection in the scientific data so that they can use different experts to do a pipeline of research and even use like multiagent systems for some reasoning collaborations。

Cal scientist is another example that using an lab agent to do some not only the generation task。

 but also manipulate the physical world experiments。

 so it actually build a connection between the language model and the hardware API documents so that it could conducts experiments in the physical world which build a connection between the generation world and physical world。

So yeah， to summarize this tutorial， so I think we had carried three parts of ourLs and large G for scientific discovery。

 we have introduced different knowledge Gs and motate why we need to integrate knowledge G into our limbs and we also introduced different knowledge G in life science and described some challenges。

 opportunities and。Dr。 Chang has also introduced some scientifics， including like biholder models。

 crossing code models， and in my part I also introduced some large integration frameworks and also the pipelines。

 pre training， post training， fine tuning parameter efficient tunings and also describe how to use them into real scientific NRP tasks and also the prediction tasks。

诶。I will conclude this tutorial children by leaving the foreign takeaways， for example。

 what does like KGs bring to as because actually KG could provide some enhanced na plantation could improve explainability。

 reasoning and inference and also integrating nastr into alums can increase accuracy and reduce the he nationations。

When we consider how to effectively incorporate knowledgeclub into LLMs。

 we need to consider the manufacturers， including the bankable models， whether this' protein。

 molecular text or encoder models， which can be like bankr models of cross encodecor models。

 and we need to consider what stage can be used to integrate knowledgecls。

 and also integrated techniques such as RoA and contact learning or LLM agents。So yeah。

 that's some reference and。This is the end of this tutorial。



![](img/612e339adc11f63d4a03e36f622b3f2f_2.png)

I'm not sure if we still have time to take any questions。Perhaps one quick question。All right， well。

 in case there are questions， there will be， please feel free to continue the discussion in the tutorial Disc channel on Slack or directly contacting the organizers of this tutorial。

All right we are now at time for transition first of all。

 thank you tutorial organizers for this amazing tutorial we really appreciate the time and care that you've clearly put into maintenance this tutorial really accessible and informative again please feel free to continue discussions in Slack or by directly contacting the organizers and if you haven't seen already as a reminder we also have a tutorial feedback form we greatly appreciate any feedback you have and we will of course share this with the tutorial organizers as well so with that I will now transition to Titania。

😊，Allright， I'm going to bring my screen sharing up。And is my audio good。Yes。



![](img/612e339adc11f63d4a03e36f622b3f2f_4.png)

Allright。嗯。So we've already started log， but officially welcome to the Learning on Graphs conference。

 its third edition now in 2024。😊，And we wanted to kick this off with just some housekeeping。

So I think all of you are aware of this， but just to repeat it and for those on YouTube。

 join our slacklack， all communication will be via Slack， all the links for Zoom， Ga town， etctera。

 are on Slack as well。😊，And the schedule for the log website is is always up to date。

 and you know we've tried to put it in in a Google sheet that people can access easily and it has all the time slots around the world。

😊，Everything is on Zoom， everything is recorded， it's also live streamed on YouTube and it will go up on YouTube afterwards as well so you can you know catch everything later as well and lastly poster sessions and socials will use Gaer town so most of your posters should already be imported into Gaer town and if they're not then we're actively in the process of doing so and if there are any doubts or if there's anything that's not clear about presenting at log or participating in log reach out via Slack or via the log conference Google Group email and will be shorter reply。

😊，So。Our mission with log has always been to advance graph and geometric machine learning， right。

 And this is a field that's growing rapidly and changing as well， growing in practical impact。

 as well as growing in terms of the size of the community。😊。

So the purpose of log was to have this really accessible to all and free to attend virtual event with global talks and top research in this field and to also bring local communities together to have these local meetups and to you know foster sense of community and togetherness around these research topics that we're all passionate about。

😊，And lastly， our third goal is review quality。 So as a new field， you know。

 we want to incentivize and have accountability in the review process， right。

 And we've been trying to take initiatives to do so primarily through trying to curate the reviewer pool the area chair pool and give out monetary rewards as well。

 and we'll give you more details about that very soon。😊。

I think the heart of log and something that's really amazing to see is 15 local meetups this year。😊。

And this is around the globe in Europe， the US， Asia as well and the Middle East。

 Thank you to all the organizers for putting these local meetups up。

 Thank you to local meetup chairs as well。 and I just came today from the new Delhi log meet up and it was amazing。

300 people showed up。 They were great posters。 they were great talks。 They were panel discussions。

 controversial thoughts。 it was amazing。 Like I really enjoyed it。

 And I think this is what makes log really special。😊，And。Next。

 I just wanted to say a word for our keynote speakers。

 so today we'll have Professor Yuuvan followed by Zachary Ulici。

 Zavier Bresson and Alden Heung we've prepared like really exciting talks covering theory methods and cutting edge applications as well。

 so I hope you're all excited for that I just wanted to also say note the different time slot for Xavier's keynote and the subsequent session which is to accommodate those of us in Asia as well。

😊，And finally， a big thank you to our programme chairs。

 Guy Wolff and Smha Krishnnaswami Without them， none of this would happen。

 And they've prepared a fantastic program， which they're going to tell us all about。

 So I'm going to stop sharing。 andmha will be taking over。😊，Okay， can you see my slides？And hear me。

Okay， cool， okay， thanks everybody thanks for joining you can see the slides。😊，I can see that yeah。

 Oh you can okay。嗯。Thanks everybody for joining today we're really excited about a log that's already going on and as you saw a lot of the meetups have happened so people are in this swing of things I just want to to start off and kind of reiterate that the reason that you know all of us are here the organizers this steering committee and the participants are because of course graph and learning based on graphs is increasing in importance with every passing year this is a chart that's actually showing a graph of the archive citation network in the archive citation network which itself is a big graph we see that the number of machine learning papers with abstracts containing the word graph have been steadily increasing and the only reason it looks like 2024 is lower because it's stopped in August but by December after N。

😊，This will zoom up so this is a topic that's of increasing importance and so we should sort of be proud of ourselves that werere in this in in the weeds。

 as you guys know graphs are very important to the world or around us。

 whether it be in social science and molecular and biomedical science in things that can help us discover drugs and targets but actually also many other fields including materials discovery market modeling。

 finance etc， and so this is what makes this a community that increases in importance with every every passing year it's because there's new and new data sets on many。

 many domains that continue to be added in the world and most of them can be modeled as graphs but not only that the notion of a graph of course。

Comes from mathematics so there's a very well developeded mathematical foundation that we can take advantage of so this combination of this mathematical foundation and a high degree of application I think will only increase the importance of learning on graphs and understanding what we're learning so the mathematical foundation you know as Tiittanya mentioned you know rests on can rest on geometry but maybe also many other things there's graph theory of many varieties there's of course geometry。

 Romanmania and algebraic discrete also topology topologies well developed on graphs especially in TDA。

😊，But increasingly we also see connections between Marovviian and stochastic processes。

 diffusion processes on graphs and of course graph signal processing which is at the core of a lot of the GN based work that we see so our conference also encompasses all of these topics so here's a fun log paper title word cloud so you see the biggest word here is knowledge so graphs are a way of encoding knowledge and reasoning about knowledge which is you know at the underpinning of you know machine learning and data science and we see of course GNNs so graphs in the context of neural networks gaining importance other terms that I want to show are representation there's a lot of things in graphs that can be represented nodes edges the entire graph itself to give us information about different layers of the data it's naturally multi。

😊，Skill course grain information and some of the applications that I mentioned you'll see in here so you'll see molecular you'll see large language strategies。

 social property cancer structural so this word cloud is sort of an embodiment of where you know LOG is going this year so I hope you're very excited to join in and with that i'm going to pass it over to Guy who can talk a little bit about the statistics of the papers we saw and what we were looking at。

😊，Thank you， Sammta， I'm going to actually try to share here if you let me share my screen we'll see if it works。



![](img/612e339adc11f63d4a03e36f622b3f2f_6.png)

嗯。

![](img/612e339adc11f63d4a03e36f622b3f2f_8.png)

Hopefully everyone can see。I see my screen now？Yes。So。So yeah， we。This year we have full papers。

 abstracts and TLR papers in order to bring here exposure for multiple avenues of research and when we look at the acceptance rate。

 it's at about 40% for full papers which is a slightly more rigorous process 56% for abstracts in order to allow a little bit more innovation and also submission that are not necessarily going to proceedings but can expose things that will later on be fully developed in other venues and TmLR papers or published papers that are given a spotlight here and that is why we have a high acceptance rate。

 we already rely on them being accepted and peerviewed there。

We also have here the division of ors from each of these categories balancing the different sources of papers coming in and posters。

 that still allow people to explore their work without necessarily having a slot in the program this is a very big operation so you can see you can see here how many area chairs and reviewers we have we thank all of them this is not easy to go through all of this process and select the best papers。

Have。And when we look at the scores that we expect out of 10 and。

You can see here we have the average score for a poster on the positive side around six。

 which is above exception and threshold basically and for or else we expect more confidence and full clear accept and the confidence score is roughly where we want it to be anyone who's reviewing knows that we hesitate to put five as full confident there's always a possibility of missing something but at the same time we don't want educated guesses so we're very happy where the quality of review ended up in terms of these statistics。

The average review length， you can see a distribution。

 we have a relatively good amount of text coming in from reviewers in order to justify their decisions。

 you know we can always do better but at the same time we have to remember the limited time and all the other duties especially with our review process being relatively short。

In parallel to IC and other conferences， people have a lot of workload。

 so we definitely appreciate those who took the time to have lengthy ours to help us make the decisions。

In order to improve， we also rate。The reviewers， the area chairs are helping us with this and again we would thank the area chair for this most of the reviewers were meeting expectations。

Although that exceed expectations we will be giving out awards and we will mention it in a few moments。

 those who are below expectations， we will work on improving our program committee for subsequent years。

 so we definitely make decisions based on these ratings for the future to ensure we improve our view quality。

Based on all of this fitting in， hopefully you will agree with us that we have a very exciting program with our 12 orals。

 we have four sessions， each of them with three orals， they are spread over time zones。

 of course based on our audience mostly concentrated on。Were on North America and Europe。

 but we also have a session dedicated to our audience in Asia and then on top of it we have of course the poster sessions and the tutorials to complete the program。

In the keynotes， of course， this is a team effort， we have a very big advisor board。

 including also SmeA that has a double head of program share and advisor they are ensuring the consistency of the program across years log is an annual event so we think all of them probably engaged in ensuring the longevity of this endeavor。

And we also have an organizing committee， just like Chatanya said without the program chairs and the reviewers and the area chairs。

 none of these would happen， also without the organizing committee， none of it would happen。

 they were extremely helpful throughout the process we relied on them Smith and I with a lot of the help behind the scenes。

So I definitely want to highlight them also for operating open review， you can see here。

 Lu and Han and Titania and many others they were there on a regular basis helping us operate everything and they deserve many。

 many thanks for making all this possible。And finally。

I can say that on the final day you have something to look forward to。

 we will be announcing a best paper award。That that we will select and we will also recognize area chairs and reviewers that exceeded expectations。

 as they said， so we have 30 plus best reviewer awards and also three top area chairs so you can look forward to that。

And yeah， and with that， I think our opening remarks are done。

 so I'll give the mic back to Jeania for the next parts。Thanks， guy。 Thanks， withta。



![](img/612e339adc11f63d4a03e36f622b3f2f_10.png)

So now we have a bit of a break。 like we have around 10 minutes still Professor Yuuvan's keynote and。

😊，I think if you like， you know， we're on slacklack and we can chat there。And yeah。

 we can just wait for。More people to join the zoom。

 go and tell your friends that the keynotes happening now and we we'll also wait for Professor Wang to join as well and help her get set up and so on。



![](img/612e339adc11f63d4a03e36f622b3f2f_12.png)

![](img/612e339adc11f63d4a03e36f622b3f2f_13.png)

Hello。😔，嗯He hello， how are you。😊，Good， how are you doing All right， thanks something good。Everything。

 All right。Yeah， I think it works well I can hear you loud and clear excellent all right be allotted time sure then I'll mute myself for now。

😊，As you like it， soundss good。Allright。I think we're ready to start soon。

So welcome back everyone and I'll hand it over to Yuquaang to introduce the speaker。哦，系。So hello。

 everyone。 welcomelcome to our L G conference this year。 And today。

 we are very happy to have Professor Su Yuwang from UC A UC C E SD to give us a talk about the science and lens generalization of neural models via algorithmmic alignment。

And Professor Wang has achieved many good works on graph learning and graph theory and and a spectral method and so on。

 and very welcome you to our session and looking forward to your talk。All right。

Yeah so thanks for the introduction and also thanks for having me it's really glad to be here well I know that this conference is learning on graphs I mean so we're going to see graphph neural networks and graphs very soon but in the later half of this talk so bear with me first。

😊，I'm actually going to start with one step back and talk about algorithms。

 which as a systematic procedure for solving problems， really has a very long history you know。

 since ancient time， like the Euccleus algorithm to computed the greatest common divisor， okay。

 so that's an algorithm procedure。😊，Now， in the last 60，70 decades。

 the modern computation power brought to us by computers really has led to a paradigm shift in algorithm。

 both in terms of how we view algorithm and how we design them， so for example。

 now when we think about algorithm， we usually view it as a computational algorithm。

 computer algorithms。😊，Now with the impressive power that we witnessed by modern AII。

 especially very recently those broader by the large language models。

 it's natural also to ask whether by fusing modern AI and the power of data。

 whether this could lead us yet another revolution to agz design。So in particular。

 so let's not forget that in computer science， the classical algorithm design really has been one of the corner stones in the last 40 years。

 50 years so that's a huge literature on that and this brought us。

 this gives us a many very beautiful deep insights about the mathematical structures behind the problems。

 elegant algorithm frameworks， many approximation algorithm with theoretical guarantees etc。😊。

But at the same time， not all theoretically sound algorithms readily transfer to practice， I mean。

 they may have nice asymptctic time complexity， but it' still not necessarily practical。

Furthermoremore， algorithm mean it's usually hard for us to really articulate what are the patterns。

 what are the structure in data and further to leverage that so through a way at the same time where they mentioned that we have witnessed amazing power of modern AI in learning discovering the hidden pattern from data as well as to leverage that for the task at hand but at the same time in general。

 it's not an easy question to know whether a learned model or generalize especially generalize out of distribution and in fact in many times we do not even know whether given a specific architecture。

 even have the capacity to implement the specific task specific algorithm at hand so one of the things that I've been really interested in in the last a couple of years is how can we combined。

The best of the both worlds to advance this neural algorithm design。

 So how can we combine algorithm ideas and insights with neural network so to develop more powerful frameworks that can learn from an adapt to data in this talk。

 what I'm going to focus on is the specific question of size or lens generalization of newer model。

 So basically the question is that can a neuraler model with only bounded complexity in terms of the number of parametersmeter that yet it can generalize to input of arbitrary sizes and syn sizes Okay so that's a question。

 So you want a neuraler model because once it's trained。

 you have to freeze it So the newer model has only bounded complexity which is independent of the input size。

 but you wanted to work for input of any lens。So this。

I'm using the size generalization instead of lens， I mean you will send that the recent years have a lot of work on。

 for example lens generalization for large language models I'm using the term size because I mean the input doesn't have to be a sequence so the size is the cardality。

😊，Now note that this size generalization really is one very fundamental property of computational algorithm if I give you a sorting algorithm。

 then it has to be able to sort any sequence of input of any lens whether I've seen it before or not okay so this is also very highly desirable for neuro algorithm models now there are multiple challenges involved and in different levels the simplest type of question is what I call the expressivity so basically given the specific architectural neural model does it have the capacity namely can I set up the parametermeter so that this model indeed can achieve size generalization or important in practice is that do I have a practical model that has this expressivity。

😊，The next layer of the question is that well， your model might have the capacity to achieve size generalization that can work for any input size okay but if you give me a trained model do I know whether it generalize or not so do I have efficient way to certify approval correctly that a given trained model indeed has this property can work with any input correctly okay so I call this certification problem because you are trying to certify a given model and in particular very important in practice that it's not just any certificate very often in order for this to be also useful for training。

 we actually want that the low loss somehow reflect the ability of size generalization so can of certain loss can low loss imply that。

ok。😊，The last type of question this is a most challenging one。

 this is an optimization question namely well can we train through， for example。

 the standard grade and decent or other optimization measures train a new model so that it produces a final model that can generalize to any and inputs okay so now obviously top down the question become more and more challenging okay and the optimization in particular challenge even in the classical statistical learning domain we don't have many tasks that we can provide privilege guaranteed optimization okay so。

U。In this talk， I'm going to tell you some of our recent work along this direction where for the size generalization of newer models。

 the main theme is that to use this alignment with certain algorithm structure to help us to achieve the size generalization so you can also view this as an algorithmm inductive bias and in particular I'm going to give you three small vignette。

 the first two focusing on this slightly the first expressivity type of question okay the last one is on the certification okay I'm going to spend so I I'm going to go a little faster on the first two examples just to give you a taste of how they work and hopefully spend most of the time on the last most challenging one which is to leverage alignment between。

😊，Gure network and the Belelman Ford procedure to certify that we can indeed learn the Bellelman Ford procedure。

😊，All right， and feel free to interrupt me with any questions， I think I can see the chat window。

All right。But before I start let me say that theoretically there have been a lot of very interesting recent work on understanding this excivity problem essentially to understand that the computational limitation of a neuro models。

 especially transformers okay so basically fundamentally whether they can compute and know whether they cannot compute okay now compared to those line of work I mean for example。

 I mean such as to show that a transformer can simulate finite anatma and so on so to compare with those work you can think that our approaches are more problem driven was the goal to have an effective and practical model to tackle the given problem at hand but also with certain theoretical guarantee the size generationization guarantee so you can view those line of work as you know computational complexity。

 computability。😊，Of study while our goal is to algorithm design。 we want to have this neuro a design。

 Okay， to solve your given problem， which can be a hard problem。

 say commatory optimization problem at hand。 And I would like to have a newer model to tackle that。

 So there are different challenges involved。 Okay， now this is also closely related to all this interesting work on neuro am reasoning。

As well as in the last few years， there's also a line of work on using machine learning for combinatorial optimization problems okay so this is closely related。

 but our focus again is on to have this size to pay attention。

 be careful about this size generalization issue and ultimately as I said earlier one would like to go beyond the expressivity and representation power one would like to be able to optimize a neural model to achieve that size generalization so the second part of my work on this neuro Belman Fort will be one step towards that goal。

😊，All right， so I'll start with the first example， so this is a joint to work with my colleague Andrew Khan。

 students Ra Nam and Qin Yi Yang。So the problem here is it's actually an MP hard problem。

 it's called a rectilinear shineino tree problem very briefly， it sounds very simple。

 you're given a bunch of points say in the plain euclan space here at the blue points and you just want to connect them using a tree structure and you want to minimize the length of the tree the total length of the tree。

 okay it's called a shineer tree because you allow to add additional nodes like this redin nodes that you see here。

😊，OkayAnd it's called recallinear because here we're going to measure the distance using L1 norm so but this really doesn't matter whether whichever norm you use。

 it doesn't change the hardness of the problem Okay so this rectallinear line tree will consider the specific version of rectallinear using L1 distance because it's coming from motivated by whsi chip design implications where the rectallinear line tree is usually used as a first step to give the global routing of the wires for a net when you put them on the chip。

I mentioned that the problem is NP hard but it's very easy to get actually comes with vector approximation algorithm。

 however in practice， given that this really directly tied to the cost of your the heating the cost of the chip so people really try to improve this make it as accurate as possible。

 and there are many different heuristic approach developed。

 the best theoretical approximation algorithm， it's a beautiful pits given by Sangiio Aurora which give us this onePlusus excellentps approximation for this problem okay but as I said their heuristic algorithm and recent years this also machine learning based approach rest is one reinforceforment learning-based approach for this problem。

😊，Okay。Now。The bias， the theoretical algorithm the pitus。

 even though it's a one plus epson approximation， but we'll see later that this is not practical so hasn't been implemented okay so in practice fluidte is what's been used most commonly。

All right， now this is a question that we want to solve and on the surface sounds like that well this is NP hard problem。

 how can we just have a newer model with only constant number parameters meters but applicable to input point sets of arbitrary size okay how can that be achieved。

Well the key idea is that maybe what we can do is that we don't do it just in the end to end neuromod。

 we actually mix this with some outer level algorithm framework okay so we let the algorithm also to carry a lot of weights to handle the complexity so in particular what you're going to see is that we are going to have this outer framework which is an existing algorithm framework。

 but inside we're going to call the neuro modules which those neuro modules are going to have only bounded the size independent of the input。

😊，Okay， and this is why this is using the algorithm alignment idea because we're now using some algorithm framework。

😊，In particular， what I'm going to next very briefly tell you and Shier is just a mixed neuroagrim version of this best theoretical algorithm。

 this PE proposed bio aurora。😊，I will not get into detail I just want to give you the high level ideas but the first I mean how does this the theoretical algorithm work it actually have a very simple and beautiful idea behind so let's say that we just looking at two dimensional case all the points are in the play okay and I want to find that the shine tree connecting fast shinetry connecting them。

😊，We just first we put a bounding box around it and then we just build a qua tree on top of that。

 which it just keeps sub divividing this bounding box till every cell contains only one point Okay。

 and for the technical reason to get a surgical bound。

 we actually need to randomly shift this qua tree Okay so the results is it is a randomized algorithm。

Then we're going to build thissteiner tree by choosing those Steer points in the bottom up manner so in particular you don't have to worry about details about the key idea in this pittas is that if I just look at a particular cell in your quad tree that's what you see here I really don't care how the tree look like inside in order for me to build the tree I only need to know this is a key idea by aurora I only need to know how the tree leave the cell。

 how they exit this side of the cell okay and in particular I don't care I don't have to check all the possible place where they live I can just focus on those orange points called Ports okay so you consume that the tree you're constructing it's going to leave the cell only through those portals I only need to know that the cost for those。

Ait pattern okay and to build that cost， you can do this in the bottom manner and when I consider a parent quad tree cell。

 I can just assemble this from those exit pattern from its four child cells okay so this is the dynamicim programming step where you just simply take those portal configurations from the child cells and use them to assemble the portal configuration for the parent cell and then you go all the way up to the rule root and at the root cell you pick the portal configuration that give you the lowest cost and then you can of propagate the back to retrieve the optimalstein tree and then that's the algorithm okay so once you build the quad tree you only need to the dynamicim programming only take a linear time times the time you have to inspect all the portal configuration at each dynamicim programming step。

So this is just a theoretical guarantee adapted from a Ro paper Now the algorithm is actually very simple conceptually why didn't we implement this this is because that this dynamicium programming step。

 you still have to inspect all possible portal configurations that has a very huge polynomial sorry even though it's constant abilities like a1 to the power of1。

 this is still a huge number so you cannot afford to enumerate all possible portal configuration exiting a specific cell okay this is just far too expensive to remember so the idea is where quite natural how about we just replace this little unit by neural network and the nice thing is that it doesn't matter which cell you're looking at the neural network will always take for child child cells portal configuration as input and spillit down to the parent cells portal configuration okay and you can instead of enumerating all possible portal configuration。

😊，You can try to learn this latent representation of the portal configuration Okay this is independent of size N the input size。

 but hopefully it's also much， much smaller than your explicitly enumerating all the configurations Okay so that's a main idea so in fact you need more than just one little this neural model you need four of them to handle the leaf case。

 the dynamic programming step， the top case and also this retrieve case okay but the key is that each of them only work locally okay each of them has is independent of the input has only bounded the size and your algorithm outside is your dynamic programming algorithm and that algorithm will call this neural modules multiple potential linear number of times and once this little components are learned they generalize to any input size because youre just repeatedly calling those。

好能。So that' and your training can be restricted to very small size input because you just want to train these components well。

And I will not get into the results here in detail， but roughly speaking。

 we only train this on point sets of size 200， but then we test it on size up to 5000 okay and as we can see that as the size become bigger the bigger so this red is the best so the bigger the numbers the better it is this is improvement then as the size become bigger we actually have a more improvement over the other existing algorithms。

😊，哎。And it's also much faster because it's a new approach。

So that's the first approach where the problem is hard and we kind of mix。

 we use existing algorithm to help us to design a neuralmod to tackle that。

 so that's where the a alignment come in to help us。Questions on this。All right。

 so now let me give you the second example， so here the ultimate goal so this is joined to work with my students E Samantha and Chen。

😊，So heres the high level question is to compare complex objects such as you know point cloud sampled from some geometric shapes that you want to compare them or maybe you want to find the mean or medium of a collection of complex objects so in particular here I'm just going to focus on the having a neuraler approximation of the was in distance between two Euclidean point sets okay so now the question is what is the right architecture that so that we can efficiently approximate this distance。

What you'll see later is that we're going to use this alignment with the sketching based approximation algorithm to help us to reduce the model complexity to be constant。

 and you also have to put some consideration to be what's right in your architecture so that you make sure that the result in architecture can really approximate such functions okay。

All right， I that I don't really need to introduce the w in distance so here the setup is that this is basically a distance you can use it to compare distributions supported on the same metric space it's been commonly used in machine learning I'm going to focus on the L1 largest in distance but it works for anyLP wide just in distance so basically this is also called Earths mover distance in the computer vision all of this is special cases of optimal transport so here you can think that I have a two distributions in say eucidean space and I want to move transport one to the other with the minimal cost where the cost is measured the total the amount of mass you have to move weighted by the distance that you're waiting you're moving them okay。

嗯。😊，Now in particular I'm going to focus on distributions induced from weight to the points。

 so you're really comparing to weight to the point sets now if the input at to weight to the point sets of size。

 let's say and then to compute them exactly takes N cube login time。

 but you can also use this as synhor， this antroic regular which takes quadratic time。

 but what we want is that we want just bounded the size newer model to approximate this even more efficiently and noted that once you have a newer model to approximate the wsin distance。

 then this can be this is a differentiable so you can now actually use this in much more easily in the machine learning pipeline where the ws distance is used say as part of the loss。

诶。All right， I should emphasize that here I'm only approximating the largest distance itself。

 not the optimal transport map that induced that distance。

 which is a harder problem okay so here I just want to know the distance okay。Okay。

 since if we care we want to approximate this distance。

 how do I model this distance so for simplicity let me just assume that my input points A and B。

 they're all in some hyper cubee in D dimensional space and the cardality is at most and okay so we don't need this。

 but this is just make it easier to describe。So now you can each of the points sets now is basically can be represented by this tensor where you have n rows。

 each row represents the coordinates of a point， and you have n number of points。Okay。

 so you can this way， you can view that as this largest in distance simply as that you can be two point sets like this and I spit out a single distance。

 okay， a real value， which is the largest in distance。😊。

But obviously this function has some constraints， The function has to be symmetric with respect to this two input because if I give you B and A or A and B。

 the function value is the same okay Furthermore， for each of these factors A and B。

 they themselves has to be permutation invariant well it has to be an invariant to the permutation of each of these factors so you have some permutation to permutations。

 if I permut this input points I should get the same distance， it doesn't change the distance value。

Okay， so we in other words， what we' are proximating is a function that satfi these conditions。

So this is a special case of what what we call this SFGI function here。

 just like the more general allow you to have just not just two objects， but K objects。

 say if you want to compute the min of K objects and allow you to have other group actions instead of just a permutation group okay but you don't have to worry about this more general concept。

 basically the key is that we're interested in approximating this fact from。😊。

This product space to R and this function should satisfy all the symmetries。All right。

Now we have seen a lot of really beautiful work in geometric deep learning in handling all kinds of symmetries and using those work it's very easy to see this first observation。

 which is that let's just focus on here basically because this function is symmetric with respect to this two input here A and B because of that there exists functions phi。

 which has to be G invariant to the group action in this case that means that the phi has to be permutation invariant。

 such that you can just apply the same operation phi to each of the input phi and of A and B。

 and then add them up， this is how you make this function to B permutation to be symmetric here and then followed by another function row outside so this is just following from existing work on handling the symmetries。

え？And what this suggests is that okay we have this simple architecture actually to approximate this v in distance。

 this is just a CMs network plus MOP at the end in particular for each of the input point sets you first go through this phi here and then you sum them up and then you follow by another MOP at the end applied to the sum of this image after Phi okay so it seems that to be very easy。

 we have the architecture。Well， you we just use this。

 the issue is that right now we don't know what's a composite of this。

 whether they're really of bounded size or not， maybe they need to grow as your input A and B become bigger okay in particular the note that this by itself has to be permutation invariant has to be invariant to the order of input points in each of the point sets okay and we know how to handle that I mean we have many models that give us permutation invariant achieve permutation invariant。

 deep set is one， but you can use some transformer and so on okay the issue is that if we just directly say we're going to use a deep set here。

 the dimension， the latent dimension of a deep set at least what we know right now the best dimension actually depends on input size N okay so we cannot argue this module is a bounded size。

です。Okay。And this is where this alignment to algorithm come into picture so here is actually a very simple sketching idea to approximate the virus in distance between two points set so we have this black points that's the input okay instead of using so that's the input point set instead of using them as using this points directly imagine that I have something called an epsilon cover of my domain in this case is a hyper cube and the epsilon cover is just a bunch of points in this case this blue ones such that the epsilon ball around them the union of this epsilon ball covers the entire space okay and the。

to know that you can assign these black points to this center of this epsilon cover。

 and so this eilon cover you get a bunch of weighted the points。 Okay。

 the weight of each point depends on how the assignment of the black points to them。

 Okay the key is that this weighted the point sets Now that dimension is only the socalled a covering number。

 which is independent of M， but only depends on the space where your points are coming from Okay so it's a property of the space。

 Okay， so you have euclidean space or something else called a doubling dimensional space。

 then you can bond to this covering number。 Okay so that's in other words。

 I have a this H that map every input points to。Bunded the representation now okay。

 this way to the point sets or bounded the size such that Now if you give me two point sets instead of computing the ws in distance between the original two point sets。

 I only need to compute a wides in distance between this two way to the point sets。 Okay。

 this in this output and that give you an approximation of the ws in distance with additivearrow Epsilon。

Okay， so。Furthermore， this map， that map this input points to this weighted point sets of bounded size actually can be expressed in this specific form。

 so this is a nice form because this can be written as you only need a function applied to each individual point instead of to the entire point sets and then sum it up。

Okay， now this is important in having the model with this。

 we now finally ready to have the final model its still a CMmes network。

 but you conceptually you can just think that every input point set first I'm going to go through this mapping。

To map to this weight to the point okay and that the mapping itself has this form， in other words。

 I only need to learn a pointwise function okay once I have this way to the point。

 I now use the earlier CMs network to send through another function by now this function by doesn't have any condition but it's operating on a bundleed size input now。

 then I sum the map and apply an MOP at the end so all of this neuro modules now only need to work on bounded the size only depends on the input space where your points are sampled from independent of the M okay so you get this expressivity of this final model that can it has a capacity of approximating the largest in distance of any two input point clouds。

Okay。And so these are some results and we compare with the Shorn so in fact I mean it very often it works this are some other neuro-based approaches and our approach works better sometimes is better than sinkhorn as well and all this neuro approaches is much much faster than the Shorn approximation。

Okay， this is the training time， not the inference time， the inference time is very small。So yeah。

 okay。All right， so that's a second example where we used this alignment with the sketching algorithm。

 try to reduce the size of the model to approximate wass in distance between point clouds。Questions。

嗯，人要是q and你。session。嗯 okay。Okay。Right， so indeed， so this previous approach was number one the。

The heuristic algorithm， this flute actually was highly optimized for small size。 In fact。

 for size smaller than9， it even have a lookup table to remember for different type of certain configuration。

s the how to solve it quickly okay so that's why as it become larger。

 this heuristic approach is actually works worse。 and the same thing so what's interesting is about this previous machine learning based approach using reinforcement learning okay so that model also works really well when the input size is small。

 but my my suspicion is that for the reinforcement learning since only trained on small instances when you increase the size。

 it it may not have really learned the right strategy that extends to the large large input size。

 That's why as the size increase the performance become worse。Yes， okay。 thank you yeah。All right。

 so now I want to go to the last part of the talk， which is going beyond just expressivity。

 rarely try to see whether your model has a given model can generalize two different sizes or not。

Okay， but first note that the model efficiency of a neural model is often affected by the interplay of your neural architecture and the task structure okay。

 so for example， the work by Sanford at all shows that transformer because of this self attention。

And also the parallelism， it actually is much more effective than at simulating this KHop induction hat。

 which is essentially used it in terms is kind of reasoning okay。

 then RM based approaches that's one reason that they believe that the transformer is more effective is better。

Now it has also impercolate， both in percalaly and also some theoretical justification observed that when you have this kind of alignment of neuraler architecture with certain ama structure or task structure that facilitates the am reasoning and out of distribution generalization okay in particular in the graph learning community。

 we have this GNNs are intuitively dynamicyn programmers。

 okay so a series of work observed that and they are naturally aligned with procedures such as Belman Ford procedure or Dtra and so on okay but in general。

 it's not very clear that how do we really articulate the precise benefit when we have this alignment of the let's say that the graph neural network with the Belman Ford okay can we articulate that。

😊，嗯。Advantage having that alignment。And that's what we hope to solve What can the alignment of graphraph neural networks with beman Ford proceed really give us in terms of this size generalization so that's what I'm going to talk about next well for this audience I assume that I don't need to introduce graph neural networks there are many different families of graph learning models that have been commonly used and one is the widely used is the message passing graphph neural network there are the graph transformers highor tensor- based models like IGN etc so I'm going to focus on this method passing graphph neural network which roughly speaking you basically maintain at every node some feature and you keep at each layer it just keep updating this feature okay and the way you update the feature is that every node will collect messages from its neighbors okay and then it's use the message from the feature。

From the neighbors to update and get a new feature。 Okay， so this is。

very common generic framework and you can in particular so here just saying that at the case level。

 every node will first collect features of its neighbors and then it aggregate them in a certain way and then it used this aggregated information with its own feature to update to give a new feature at this node V okay。

😊，You can set up this aggregation and update function in different ways this are going to be learned often they're represented by some let's say MOP okay the only condition is that this aggregation function should be permutation invariant so it should take them it shouldn't depends on the order of its neighbors okay now to achieve this permutation invaris where it is in this we could do something similar to the deep set namely I have this I take I sum up all the neighbors feature and then。

Followed by another function Okay， so this is one of the most common strategy people do in GNNs I mean just the sum or the neighbors or sometimes is the average instead of sum okay another way to achieve this permutation invaris is that I use the max of all the neighbors feature okay。

 so both the sum and the max permutation invariant okay and so this is also this has also been used in the GNN sometimes you also see some models they use all the strategies use the sum。

 the average and the max for example okay。I'm going to consider this max version。😊，Okay。

 as the aggregation functionion in my GNN model okay and instead of max I'm going to take the min but it symmetric it's the same thing this is because that using this min pooling local layer at each node this more better aligned with the Belelman for the procedure okay in particular the GNN I'm looking at this is pretty much the standard GNN。

 the only difference is that the pooling I'm using the min of the neighbors okay。

 so I'm going to look at this formulation as at each GNN layer okay。

 so you simply take the min of all the neighbors feature and then also take its own feature again and then and then update it okay。

 so that's the formulation of our graph neuralure network。Okay。😊，All right。

 and it's easy to see that well so first and let's define this Bllman fourth step。

 this is following the previous work by Darziik and Vekovic so this is in graph theory in algorithm we know this is how we one algorithm to compute short is pass where every node we have some original distance estimate the new distance estimate its just that it original distance estimate and also you look at all the neighbors and what's the best way to reach it from one of the neighbors okay so this I called we call this Belllman fourth step。

😊，Okay， and it's very easy to see that one can easily set up。😊。

MOP to represent this update MOP and the aggregate MOP so that it exactly simulate this Bel Belman Fort step。

 Okay， so that's alignment。 That's why that intuitively graph neural network has a natural alignment structure resemblance with the Belman Fort step procedure。

 Okay。Butum。That's not our goal。 The goal is that。It's easy to see I can simulate the Bman For。

 but can a GNN really learn either one step Belman force or the K step Belman For procedure correctly。

 you know over only affectffs the set of training graphs Okay。

 so you give me a training graph and we train our model that's the really learned the Bman Ford audit did it just fitted the input the training graph and memorized everything Okay。

 so that's the the question that way we hope to tackle Okay， so in particular。

 so the setup that we assume that our GNN。So we are given a graph where the initial node is just some initial shortest past distance estimate okay and the goal of a GNN is to predict what is the new distance estimate at each node after if you perform K step K Belman For steps okay and the training loss intuitively so you have a GNN and given a set of training graphs。

 the training loss is basically defined as the average distance prediction  error at each node across all the nodes in all of the training graph okay so that's a training loss okay。

All right。So。Is it hopeful to get this generalization site generalization Well let's actually first look at a very simple case okay。

 this is a warm up case let's assume that my GNN right now is what I call one layer。

 small GNN so we have only one single layer okay furthermore for the single layer inside this is how we set the GNN basically each of the update and aggregation is just the one MOP layer where the dimension。

 the latent dimension essentially is as small as possible the smallest model to to simulate the bowman forward basically okay and the activation is。

😊，So very interestingly， in this case， you can actually show that。😊，The low loss。

 So if you're train your neural network so that the loss is low， Say the loss。

 you train your neural network。 Okay， so that the loss is at most apsilom， okay， then。

Your perimeter has to satisfy these conditions。Okay， so what does this mean， Well， if this， if this。

No equal to exactly 0， then these are actually all solutions for the Belman Fort。 Okay。

 so in other words， if this equal to 0， they will exactly implement the Belman Fort procedure。 Okay。

 so there are infinite number of ways to implement the Belman For using this neural network。 Okay。

 if this is zero that they' are going to implement the Belman Fort。

 So here we're saying that were not deating too much from those one of those infinite number of solutions。

 basically。 Okay， and this further implies as a result that now once you train your model with this low loss。

 now you apply your model。To any input graph， with any positive edge weights。

 you will be able to guarantee to predict the Belmon Ford step correctly with the arrow Abpsilom。

 okay？What's most interesting is that you only need to train your model on this there exist only a very small training set of only eight graphs okay and your loss is evaluated on this eight graph and this is not just existence。

 you can actually easily this eight graph looks like this Four of them looks like this form and the four of them looks like this okay you just need to take this eight graph and if your neuraler network is trained to have low loss over this eight graph then it's guaranteed to generalize。

So intuitively， why is this happening intuitively what happens is that because of this alignment between the Belman Fort and this GNM。

 it's almost if you ignore the Re， it's almost like a linear system of equations okay so you're just trying to choose this test train graph to essentially enforced that the Re is not really playing a role and then you can kind of solve this linear system okay。

 so that's the intuition not exactly so but that's why this alignment really help us to achieve this。

😊，Okay， but this strongly relies on that right now my neuraler network is actually of this a very small size。

 Okay， so my latent dimension is very small so I don't have don't have lot of margin for error okay so in general。

 you want to have any neuraler network because I can control how the user is going to design their graph neural network so the question is that what if we over parametermetize our graph neural network Okay what if we I can have arbitrary number of layers in this GnN and inside your update and aggregate module can also this MOP can also have multiple layers and the widths of those each layer。

 the latent dimension D can also be large okay so it could have essentially arbitrary widths and depths。

😊，So。So no this is not empirical， this is a theoretical guarantee。

 namely for this eight graph where this graph is specifically chosen。😊。

If your loss on this eight graph is low， then you are guaranteed to generalize okay。

 so and the intuition why we choosing this those eight graph is as I explained earlier。

 is that intuitively here it's almost likely you have almost the linear system。

 but you have some ralu inside that to screw things up that you're choosing those graph to try to mitigate the effect of the Ralu。

😊，so this is not empirical results， but later can you see that this this does have an empirical implication in training of the graphphure network。

😊，Okay， so this is just to address the question in the。In the Q&A， okay， there's another one。Yes one。

 yeah。😔，From Rick about the right So no， no， that's an excellent question。 I mean。

 this is in general。 In fact， even for this one， for the next step。

 you will see that I have to change my loss a little bit。 I think this is a great question。

 and I do not think that in general， we have there actually， in fact。

 very few not many architecture where the low loss necessarily implies generalize it just good solution。

 Okay so a lot of the study， if you look at the optimization of neural network community。

 a lot of the work is that the opt solution can have low loss。

 but it' not the opposite direction is not obvious Okay so there has to be certain structure of your the problem at hand and also of your neuraler network that has to。

 in some sense， match each other in order to get such a clean results。

 but even for this case now I'm coming to this in the overpametize the case。

 I would have to change my loss a little bit。I think it's a good question and it's also a hard question。

 I don't know whether we have a general recipe that can easily translate this results to other problems。

Allright， okay so now when we have this over parametermetized the case， Okay。

 as I said it now there are too many ways that things can go wrong。 Okay。

 I actually have to change the loss a little bit I have to use regular the loss okay。

 so basically because your your model is over parametermetized。

 I have to artificially to say that I want sparse solutions basically Okay。

 so that's what you see here。 I have to add a L0 as L zero sparsity。😊，To recognize this loss。 Okay。

 but with this， regular the loss， let's still just consider one step bowman For。 Okay， again。

 you get this serumorem。 This is a theoretical guarantee。 Okay。

 so this is the same eight graph that we saw earlier。 Okay。

 your training set can actually be bigger than this eight graph， you can have additional graph。

 but you need to contain this eight graph。 Okay， and let's say that that's the total comp of the training sets。

 so what the serumorem is saying that now okay， if your regular parametermeter here。

 this weight is chosen correctly， sufficiently small。 Okay， then as long as you' regular the loss。😊。

Of your neural model is within epsilon of the optimal like the smallest the possible you can achieve then again you get the same size generalization guarantee namely for any input positively weighted the graph applying this neural model to that graph will approximate the one step beman for with additive arrow okay so so once you in other words it's very similar to the previous one。

 it's just that now you need to use this check this I recognize the loss okay you also do have conditions on this this coefficient here and as you can see that the more graphs you are using or as your neural model become more and more complex then the condition on this is stronger you have a less and the less margin okay so you have to satisfy this condition but if this add。

Is sufficientness more than you get to the same generalization guarantee。

So both of these two so this is basic for one step Belman Ford okay and then the next question of course。

 is that can I use work with some multiple steps what if I just want to train a single module that can predict a K step Belman For directly you can already use this result saying that I'm just going to pile such module up I mean you can run this module K times okay so thearrow will also accumulate so you may want to directly go with a K Belman For step without any intermediate dis supervision okay so your training would just be the input graph with some initial distance estimate and your label is the distance estimate after K step Belman For steps so you want to see whether you can train this correctly okay so now this problem become much much more complicated and we cannot use the same eight graph that I was telling you those very simple graphs before anymore I have to choose。

My graph much more carefully in order to make this work。

 but still to know that you can explicitly construct a set of graph of order of k okay so the total number is order of k there's still a very simple form as you can see I listed some here okay and with this training set then can you get a similar guarantee as before。

 you can have over parametermetized GNM where the number of layer has to be at least the number of steps K okay and using the recognized the loss then you get size generalization as long as the loss is sufficiently small。

😊，Okay， so that's the。These are all the surgical results now as I said that this actually have interesting practical implication because so let's say that now I'm going to train my graph neural network really just to predict a Belman Ford procedure okay so in this experiments this is very preliminary experiments I'm just looking at one step one beman for step okay and but my neural network just to see what's happening let's use a single layer but the latent dimension is much bigger than needed okay so here the latent dimension is 128 okay and。

😊，And so we train what you see in this plot， okay？I train three cases This one is the training set is exactly the eight graph as our theory predicted that I should use。

 I should consider to test my loss Okay we also have two other setting where I just add additional graph to the training set so the training set is 3264。

 but they all include that eight graph needed Okay and the training laws I cannot really use L0 regular。

 so instead I'm using L1 regular as the training loss。So as you train。

 so here this very messy piece is basically you see how the X axis is an epochX during the training okay what you see is that all of this per me in your GNM okay different color means the different size so the green color is when I use just the8 training graph blue is when I use 32 training graph red is when I using in 64。

What is showing in this picture is that you see ultimately to implement the Belmon Fort。

 one of the spa solution is that basically one of them go to one and then the others all go to zero。

 essentially your parameter should if you get a spa solution okay and what this is indicating is that you can see that for the green roughly speaking。

 you can see all of this， they roughly become zero， most of them become zero around here。😊，Okay。

 and this one becomes one。But for the blue， it converges later， and for the red。

 it converges much later。Okay， in other words。When I train my neural network in this particular case more training graphs actually is not helping if I use the correct aid graph。

 it actually converges to the optimum solution， the spae solution much faster than when I add additional one and the reason is because that we know this aid graph contains the right signal and when you add additional graph essentially kind of blurred the signal intuitively so I find that this a very peculiar very interesting phenomenon because in general in the classical statistical learning because we don't have other assumptions and we assume that it data coming from this same distribution testing data coming from the same distribution in that case。

 the more training set we have the more beneficial it is while here interestingly from optimization perspective seems that if I just use this correct set。

 the smaller set it actually converges much faster。We know what our target task is。Okay。

 so this is all showing similar behavior。All right， I mean。

 there are many interesting questions immediately after this。

 I mean our theoretical guarantees for L0 regular， but in practice systems that L1 works pretty well empirically could that actually L1 is sufficient to get theoretical bond as well and can we also I hear I'm very carefully choose those those eight graph or order of K graph in order for the theory to work okay but we have some intuition that make us believe that I may not need to choose them so carefully。

 I could choose certain random graph with bound at the size it looks like that those are sufficient why is this interesting why do I want to just use random graph imagine that suppose in the future I'm going to train a neural model that is capable of implementing different procedures Belman Ford and other procedures okay and。

😊，You want your training set essentially can that's useful for all of this different procedure for all of them certain randomized the training set Sur。

 then I can just use the same training set for all of them okay of course that the optimization question is wide open namely even though we said your loss goes below the threshold then it's guarantee to generalize but how can I guarantee that using SGD I can really achieve low loss so that is not clear。

All right so I think that the three examples that I want to give you all of them are related to the size generalization。

 which is an important family of the out of distribution generalization so to me I think that in the modern application of AI and MO this out of distribution generalization is one of the perhaps most important questions because we constantly especially for a lot of so I'm involved in Tlos which is the intit for optimizationization very often you have this very harder problem。

 say in chip design robotics， the problem is coming to say NP hard commoid optimization problems and you don't have the luxury to train over a lot of examples large examples。

 you really hope that whatever we learned from smaller instances can generalize to large instances okay so this is a very crucial challenge that we're facing okay and I give you three examples where we use ideas insights resse。

😊，from algorithm to help us to tackle the size generalization problem but in general。

 I think this is all within this general research direction I would like to advocate which is using combining algorithm inside with machine learning to help us to design better more principled neuralags I'm hoping that neural algorithmm really give us a paradigm shift in how we think about algorithms okay and there are many interesting but also challenging open problems still remain how to use effective and efficient neural model to solve hard problems okay in particularly optimization is widely open there are also other questions in the example that I give you usually we have to put special consideration for the given problem at hand but can there。

I this a more universal recipe that works for a broad class of family。

 a broad class of problems instead we have to design one specific for input on the other hand。

 in the algorithm design usually we do design special algorithm for each for individual input task。😊。

And how about the composition of such suppose you have you know， you have many algorithm modules。

 a neural algorithm module that you know can approximate a certain algorithm tasks with generalization guarantee how about when we compose them because then you can think that the neural we can leave to machine learning to learn the normal ways to combine those neural modules but then we need a theory to understand that in what case can we argue that the composition of those modules still have generalization guarantee or other guarantees if each individual module has those property and what I in my example is the neural modules usually the model itself is just abundant module but you can use more powerful approaches。

 recurrent neural network or chain of sort which allow you to kind of have a longer memory and。

OnAnd what can we see in those cases Okay so there many interesting problems remain and I hope that this attract more people to work in this direction Thank you for attention。

Yeah。So thank you very much for Yu Su's very interesting and exciting talk and has expired some like thoughts and discussions during the talk。

 I think due to our time limit， because we are going to the next session。

 almost exceed several minutes。 So we will we would like to thank you again and hope like any audience interested in Yu Su's works and recent progress can like a content and maybe our conference as well。

 hope you can join our conference in the next few days。



![](img/612e339adc11f63d4a03e36f622b3f2f_15.png)

Thank you， Yu。 Thank you。 yeah。Okay， I will now hand our microphone to Haitao and Alex。

 Yeah for the next oral presentation session。OO。Thanks。So。Hi， everyone。

 I'm the session chair of this oral session。 I'm Hao from the Michigan State universities。

And let us like welcome。 Like the first speaker is Floridaiddao。 So Tori， are you here， Yeah。

 can you hear me。Yeah， yeah， I think you can check and the share the screen。

And so Frano Tori is a PhD student from YJ University Brussel。

 and today like his oral talk is about a very interesting paper。

 it's called the effectiveness of curvature based Rering and the role of hyper parametersmeters in the GN Revisit。



![](img/612e339adc11f63d4a03e36f622b3f2f_17.png)

![](img/612e339adc11f63d4a03e36f622b3f2f_18.png)

Yeah， I think I can see it great yeah。Yes， perfect， thank you very much， I was about to ask。Okay。

 so good afternoon everyone or morning， depending where you're listening from。

 so my name is Ferrianno and indeed I'm aPG students at the variety of State Brussel in Belgium and from today I will be presenting some of our work on reanalyzing curvature based in graph neural networks and the role of hyperpar in GNM revisitants。

Now， first。Let me acknowledge the performance that Graph neural networks have shown on graph structured data in recent years。

 which is really due to the ability of GNNs to use the implicit geometric prior underlying in the data so the graph structure of the data。

 which is really crucial in order to solve all kinds of graph problems。Now。

 one of the issues that does hinder graphph neural networks is the issue of over squashing。

So this is really because Gra neural network use message passing。

 and so that means that overcing appears when。A lot of messages have to pass through one single edge。

 or there are like there's a local region in the graph through which a lot of messages have to path。

And then we can call these edges so the message passing through these ashes is then compressed。

 so there's a sort of loss of information， and so we can speak of bottlenecks in these graphs so these edges through which these overcrotching occurs are then called bottlenecks and the graphal network will then lose performance because these messages don't contain the necessary information anymore in order to solve the task。

Now altering the graph structure has therefore become an important approach to alleviate this oversquaashing problem。

 and this graph modification can happen in different ways。

 but today I will focus on rewiring and by this I really mean the targeted addition or removing of edges in order to improve message passinging。

Specifically I will focus on discrete graph curvature rewiring。

 so this is a rewiring where we use discrete graph curvatures in order to detect local bottlenecks in graph so we can for each edge in a graph we can assign a curvature value and depending on the value of this curvature whether it's positive zero or negative it will it will be associated with a certain local topology of the graph and so depending on this topology message passinging will behave a bit differently around this edge and so we see if an edge has a negative curvature we can associate more of a local key like topology around it and indeed this corresponds this will correspond to more to edges more responsible for oversquaashing because we have this three structure which grows the numbers of neighbors and so theres a lot of messages that have to to pass through a couple of very specific edges which。

And caused overscorching to happen。Now， the question is， how can we compute these creature neurons？

So one of the nominal papers in in this field has to be top the paper from topping at all where they proposed balance form and curvature which is a notion that depends on three different aspects around an edge so suppose that we would want to compute the curvature around the orange edge between node1 and2 then in order to compute the form and curvature we would have to count the number of three cycles so these are the number of common neighbors neighbors between node one and two we would have to count the number of four cycles so these are neighbors from one and two that are also connected between each other and important here is we consider four cycles as when there's no edge between the neighbor of one and node2 for example or vices vera so they don't have a diagonal inside and then finally we also have a gamma max factor so this counts a sort of geneeracy so we can see here that node one as a neighbor which is responsible for two four cycles in around the orange edge and the geneeracy of the。

of four cycles for which to know is responsible will be captured by gamma max structure and so using these local properties around the edge。

 we can associate a curvature value to the orange edge which will be either negative positive or zero and so hormone curvature we go from has a minimal value of minus2 and can then increase according to the of these local topologies。

One of the main results from the paper from Topping Adel was what we call the oversquaashshing theorem so if we have a graph neural network creating embedding vectors。

 we can actually bound Jacobcoian of message passing based on the curvature notion here on the left hand side in the orange mouse we have the Jacobian of message passing so what does it say well its how sensitive node K is to messages arriving from node I which pass through a selected edge I and J to select the edge between IJ but so for this edge IJ we can compute the curvature value。

 which is a certain value， and it turns out that the delta which means like how far away the curvature value is from the minimal value of minus two of form curvature it turns out that this delta can be used to bound this Jacobbin of message passing and for the smaller the delta will be。

 the closer the edge is has a curvature of minus。But also the lower the bond will be of the Jacobian。

 which means that they're sort of oversquaashing happen because note K is losing sensitivity to the messages passing from I。

 passing through the HIJ。And so it's really important that this links the negatively curved edges in a data set to over questioning over these edges。

 and so we can use this theory in order to rewire graphs so how can we do this we can use stoatastic discrete Wihi flow so that means that if we have for example。

 the red edge right here which can be detected as the most negatively curved edge in the graph and we would want to rewire around this edge。

We can look at all possible other edges that can be added in order to add either three cycles or four cycles around this edge。

 and based on the improvement that these edges add to the curvature of the red edge。

 we can select one of them， the green one in this case。

 in order to improve message poing and improve the curvature value of the red edges。And in this way。

 improve we can rewire the graph in order to help message passing around these negatively curved edges。

 which should be responsible for over squing。Now there's one aspect of term that I didn't discuss。

 which is the fact that there's also a condition on the edge selected。

 so if we have the edge Ij which has a certain curvature value denoted by the value of delta，Well。

 actually this delta， which again is the difference between minus2。

 the most negative value of curvature and the value of curvature of the at Ij Well this delta has to satisfy two things。

 one it has to be smaller than one over the square root of the maximum of the degrees of node I and J so here D I and DJ node the degree of node I and J and this delta has to be smaller than one over gamma max。

So this is a condition and it's important because mathematically this condition is required in order to prove the final part of the theorem which is normally namely that the Jacobgan can then be bounded by this delta value so once we have an edge and we have a delta value it is necessary to treat。

This deelta conditions before we can actually make a statement about the oversshing nature of this edge namely is it a bottlene or not it turns out that when we perform rewiirring actually these conditions are never。

So means that when selecting edges to rewire around。

 these conditions are not explicitly checked in the process and if we look at the dataset sets Texas。

 which was used to evaluate rewiring， if we would rewire 89 edges。

 it turns out that none of them actually satisfy this condition we call condition2。

 the condition on the delta， but that means that none of the edges satisfy the condition that delta has to be smarter than one over gamma max or one over the square roots of the max of the degrees。

Now， it turns out actually that this condition2 is very stringent because none of the edge is satisfy。

 but actually mathematically there is。Mathematicically there's a sufficient softer condition that actually can be applied to obtain the same result and so here we call this condition 2B。

 so actually the condition on the degrees can be replaced by a condition on the number of three cycles or on the number of triangles and in this case the condition then becomes ata has to be smaller than one over the number of three cycles and smaller than one over gammams。

And indeed this is a softer condition because we see that out of the 89 edges rewired for the Texas data set。

 around 7% of these edges， which is still very low satisfied condition 2 B during rewiring。

But so here we still see that most of edges selected during wiring actually don't satisfy this condition。

 neither condition 2 or condition 2 B， which really limits their interpretation as overquaashing edges because again。

 this condition is needed in order to interpret the final part of the  theum as the edge being over an over squashing edge。

No，we asked is well， maybe this is a temporal type of effect namely that edges that do satisfy the condition are selected earlier and then edges are selected that actually don't satisfy the condition and this is something that we here on the figure on the x axis we on the figure we have all edges that do not satisfy condition2 be so these are the 83 edges from the data sets。

The is selected to be wirewid and the curvature value and on the x axis we have the number of three cycles around this edge and so the dotted line indicates one over the number of three cycles。

 so this is the condition that the edge has to satisfy and so any edge above the dotted line implicitly doesn't satisfy the condition of the number of triangles while anything below the dotted line also doesn't satisfy the condition but it doesn't satisfy the condition based on the gamma max parts of the condition。

And so we color coded all of these edges selected during rewire based on when they were selected in the rewiring process for the lighter the color。

 that means the earlier the edges were selected and the darker the later。

 and we see actually that all these edges that don't def the condition are selected a bit during the entire rewiring process。

 so this is definitely not a sort of saturation effect that these edges are selected at the end of the rewiring。

 but you see that this occurs during the entire process。So again。

 we have edges that don't division and which really if by rewiring these edges， which。

Strictly ading to theuterrum don't necessarily overst the information because。

We can't use the subsequent result of theuterum since this condition isn't satisfied。

Now we can ask where does this problem stem from， well actually if we look now at all curvature values of the edges in the excess data。

 these are not only rewired， but these are all edges in the dataset sets。

 we see that minus2 is the most minimal value for the curvature。

 but we see that there are very few edges actually close to this minimal value and recall call that delta which indicates how close we are to minus2 as a curvature value has to be small because it needs to be interpreted as a bound on the tracobin。

 but here we see that very few of these edges actually are close to this minus two bounds。

really limits their interpretation as a overctting edge。Now， of course。

 this is not a dataset specific artifact we detected from multiple dataset sets that are used to evaluate rewiing methods and we see that this problem occurs over all these data sets。

 there are some outliers， for example， core and equipment which are citation networks do have a higher rate of around 70% of edges selected that do satisfy the condition。

 but we see that in general， most datas only have around 30% of these edges that do satisfy this condition。

 and this is problematic because we might be rewiring edges that are not necessarily over squing information according to the tiem。

And also for all other data sets， we indeed also check that this is not a temporal effect。

 but this happens at any point during the rewire。So now we came a bit as a sort of at a contradiction because well these revivring algorithms are used and they are better accuracies are actually reported using these methods。

 but because from the theorem and the fact that these edges don't satisfy conditions。

 we can question well what is happening， why are these better aies reported in the sense that technically these edges aren't necessarily oversquaing。

So we set out to analyze the dependency of hyperparameter on all these rewiring techniques。

So here we decided to analyze not only balanced forman curvature DFC。

 but we also analyzed other types of curvature namely Justin Liu or augmented forin。

 and we also looked at variances of these curvature， so these are denoted by the subscript。

 for example， the sub3 denote the fact that we are looking at balance forin without computing four cycles which are computationally more。

Expense and so we performed a very large hyperparameter sweep over all these when rewiring this dataset set and we also compared it to the performances we obtained when we do no rewiing but instead of only focusing on the best setup that we obtained。

 we actually decided to look at the distribution of all the results obtained during the hyperparameter sweep so here the curvature or the distribution that we see the notes the amount of times a certain final test accuracy was obtained for each hyperparameter setup。

And we see actually that the reviving methods don't really offer from the performances that none offers。

 so really no reviving actually on a distribution level really follows the same the performance as rewiviving graph reviving the graph。

 and there are some outliers as well， but even if we look。

 for example at the top 10 percent so not all hyperparmeter configurationation， but only the top 10%。

 we see that there's very little variation in these performance。

And even if we look at the results are really an outlier in this distribution。

 well the top 10 performing result， we see that there's very little variation in in the result as well。

 and that rewiring seemingly doesn't offer that big of an advantage to no rewiring so it seems from these results and from the results that we get and of course we tested it on various data sets so this was not only for the Texas but we use we analyzed this for all datas presented。

嗯嗯。In the papers， it seems that rewiring and the results obtained from these remiing method are more linked to finding the optimal hyperparameter setup then really a structural shift in performance in the distribution due to rewiring。

 so it really doesn't seem that rewiring is bringing the necessary benefits that one would expect。

 which based on the fact that these edges selected during rewiring are not。

are not satisfying the condition it seems indeed that there's a kind of disconnect there So there are really take ways from our work one is this mismatch between theory and experimental verification so at no point is there a discussion over the validity of the theorem from topping adult so mathematically it's a very nice result linking the overcoting to negatively curved edges we just saw that on empirical data sets when applying these rewiring techniques it seems that these edges don't satisfy the condition and which really limits their interpretation as overcoing information so I think it would be really interesting to see could if these methods could be augmented with a sort of theorem check to see like okay we're actually rewiing edges that need that are actually oversing information or not。

And the second part is really the hyperparameter where we saw that the dependency of this result is。

 well there's a high dependency on hyperparameters。

 and so it's hard to distinguish the effect from rewiring purely from the effect of the hyper finding the ideal hyperparameter set up in this context。

And then finally I do want to address as well that data sets that we've used are under discussion in the literatures as well and we are aware of this for sure。

 but these data sets have been used to evaluate the reing rev varying methods initially。

 so we thought it was interesting to really just look at these dataset sets and look at their performance a different angle growth from the theoretical perspective as well as from the hyperparameter perspective。

 but it's definitely a point that can be worked on to see how this holdss on other dataset sets。

And then so I want to take the time to thank my collaborators for this work。

 and I also want to thank the reviewers from Lock and all the organization because this was a really nice review Ron and we got some really nice suggestions from the reviewers as well。

 if you have any questions feel free to ask him or you can come visit at the booth number two。

 or you can find our link for our code on GitHub and I want to thank you all for listening。Okay。

Hi thanks for the great talk so like I have like one general question about like the revivring part。

 so I think this is a very interesting job but I think what is like the revivring we only focus on like the data part we are trying to organize the data and the thing here is like we still have the graph neural network right？

😊，I mean， the performance is determined by both the Graph neural network and also from the data review。

 So do you think like there are like different kind of graphraph neural network architectures or like you even use a graph transformer can somehow like will influence like your experimental results。

🤢，Well， I think there's on the graph architectures。

 I think there's definitely different performances based on which architecture we use。

 but underlyingly the rewiring really aims to find the optimal data structure to apply these architectures on so we had some experiments on other architecture types as well and we saw that there there was also the same ID that there's no distributional shift from rewiring so it's not very architecture dependence。

 but I think it's interesting to look because it's something that we were considering as well is these hyperparameters there's both rewiring hyperparameters like how many times do wewire the graph which edges do we select as well as there's learning hyperparameters for the architecture。

 how many layers what's the learning rate and I think it would be interesting to see what is the dependency on the different types of hyperparameters splitting between the learning and the data modification but it's indeed it's an interesting point to split the dataset and the learning part for sure。

です。Oh， thanks for it。 And they are like。Yeah。So I think there's no。 let me。

 I think there's no further questions。Yeah。And yeah， okay。 Thank you。

 And I think like we can move to the next oral talk。 iss Nicholas。 Yeah， Can you share your screen。



![](img/612e339adc11f63d4a03e36f622b3f2f_20.png)

![](img/612e339adc11f63d4a03e36f622b3f2f_21.png)

Okay， so I will make so let's welcome like Nicholas Canra from。



![](img/612e339adc11f63d4a03e36f622b3f2f_23.png)

sorry it's recorded oh my god。好。嗯。Yeah， so let's welcome like Nicholas Ker。

 which is PhD student from TOM， and today he will introduce his job。

 Expressiveity and generalization， fragment bias for molecular genes。Yeah。Nicholas。

 hi can you hear me， can hear you and can see so hi， I'm Nicholas。

 I'm going to present our paper expresslusivity and generalization。

 fragment biases for molecular gen。😊，hich I developed together with my amazing colleagues from technicalnical University of Munich。



![](img/612e339adc11f63d4a03e36f622b3f2f_25.png)

嗯嗯。Okay， so let's first look at the following two graphs which could represent molecules。

And they are obviously not the same。 And you don't have to be a chemist to know that they will probably have different chemical properties。

But。AndFor traditional G N N， as you might know， they are undistinguishable。

 This problem is known as the limited expressivity of traditional G N Ns。

And it can be seen in two different facets。First， the ability to distinguish non isomorphic graphs as in the example is bounded by the famous vicephlaimmon test。

 and secondly， which is connected to the first point。

 traditional gene ends applied to almost all substructures， a substructure could be， for example。

 a six ring， like in the example above for five ring。

And this is a big problem as substructures are very important in many different domains。

 especially so in chemistry and molecules which we will focus on。

So how can we deal with this problem and how can we increase expressivity The first approach is well。

 we can just adopt the model architecture such that the model is able to learn more complicated more complicated functions by its elephant can learn to recognize substructures。

And this approach is followed by so called higher order GNs。

And they depart from just having one representation for each individual note and instead have learned representation for higher order constructs like every K couple of notes。

This comes of course， with an increase in complexity。

 which can be quite large and can make them unsuitable for tasks with larger graphs。😊。

But we have very well established theory with which we can accurately describe and relate the expressivity to expressivity measures like the KVL test。

😊，And in terms of expsivity， quite they are better than the traditional G andNs。

 but especially in terms of substructure recognition。

 they are still limited and often can only recognize smaller substructures like smaller rings。

 but not a six ring， but this is like very important in chemistry。

And it has been shown that they perform quite poorly in generalization tasks。

 so generalizing to out of distribution data and they are also susceptible to adversarial attacks。

So instead of trying to adopt the model architecture。

 we can also just provide more information to the model and this is done by so called or what we call fragment bias and ends。

They hear we first fragment the。The graph into substructures and then this substructure information is given as an explicit inductive bias to the models。

And they often retain the linear complexity of traditional gene ends。

 but we don't really have a lot of fury that could relate or compare existing fragrant biasg ends to each other。

So we can't really say much about the explusivity and it's also there's also not a lot of work on generalization for those fragment by gene ends so in this work we want to focus on those fragment by gene ends with a particular focus on explusivity and generalization。



![](img/612e339adc11f63d4a03e36f622b3f2f_27.png)

So if we want to build a fragment bias gene N， we have to answer too many questions。 First。

 how should a graph be fragmented， So what are the substructures that we want to give to our model as inductive bias and then secondly。

 how are we going to use these substructure informations in our model。



![](img/612e339adc11f63d4a03e36f622b3f2f_29.png)

Okay， let's first focus on the fragmentation。And in all generality a fragmentation scheme looks something like this。

 we have a set which is predefined of substructures and we call this our vocabulary and then the fragmentation scheme just identifies some of the substructures in our vocabulary in the given input graph。

And so which substructure should we consider？On the one hand。

 we course want to have all important abstractstructures。

 because while this gives scheme model valuable information。

Important obviously depends on the domain and the task attempt for the chemical domain of five。

 so rings could be important， maybe a junction too and you could think of many other important substructures。

But on the other hand， we don't want to be too fine grained because the fragmentation should still facilitate generalization across diverse graph structures。

 So even though this particular substructure might be very， very important for a particular task。

We probably shouldn't included it because it might only appear once's advice twice in a complete data set and might make generalization to other data from other distributions difficult or finding similarities between different graphs。

So we somehow have to find a balance between a fragmentation that's too fine rate and to coarse grained。

And we propose rings past fermentation that tries to be somewhere in the middle。

And so it works like this。 we get a molecular graph。😊，Then we first extract a minimal cycle basis。

And the remaining edges are then connected to form maximally long uninterrupted paths。 And with this。

 we are able to fragment the complete graph using only two types of substructures。

 basically rings and paths。

![](img/612e339adc11f63d4a03e36f622b3f2f_31.png)

Okay， now that we have fragmented our molecular graph。

 how are we going to use this information in our model and we first have to encode it。



![](img/612e339adc11f63d4a03e36f622b3f2f_33.png)

What's traditionally used in Fr biased gene ends is how simple when hot encoding。

Which also means that similar fragments get completely different encodings as all fragment types gets completely different incodings。

And this might be a problem， as we might suspect that the5 ring into sixth string are actually quite similar and share similar properties and should get similar encodings。

Additionally， this only supports a fixed number of fragments。Our ordinal encoding， on the other hand。

 is designed with the idea in mind that similar fragments should also get similar encodings。

And we achieve this by splitting the encoding into two parts and the first part only encodes the class of the fragment。

 so if it's a path or a ring and the system shared across all paths and shared across all rings。

And the second part encodes the size of the fragment， and this is just scaling alert embedding。😊。

And with this， where the similar fragments actually get similar encodings and the model is able to transfer knowledge between similar fragments。

 as we might later see。😊，Additionally， we now can support infinitely many different rings and paths。



![](img/612e339adc11f63d4a03e36f622b3f2f_35.png)

Okay， now that weve encoded our fragment information。

 how are we actually going to use it in our model？😊。



![](img/612e339adc11f63d4a03e36f622b3f2f_37.png)

And there are three natural ways in which this can be done and in which this is already done in literature too。

The first and simplest is， okay， we can just concatenate the fragment information as a node feature to the existing node features of the model and then apply a normal GNN to it。

Secondly， we could have learned representation for each fragment individually that exchanges messages with the underlying atom and vice versa。

And lastly， we could build or or we could connect neighboring fragment representations to form a higher level graph in which additional messages are sent。

So which one of those approaches ensures the maximal expresssivity。

 that's not immediately clear if this higher level abstraction actually comes with increase in explusivity and if the higher level graph is actually more expressive than just having note features。

So。We need some measure of excsivity with which we can compare it because it's often difficult to compare the models directly with each other。

AndT we take inspiration but inspiration by the classical vicefaer Lement test。

 which is used for gene ends without any fragmentation。

 and we develop corresponding variants for the take into account as additional fragment information。

😊，And with this， we are actually able to show that the excivity strictly increases。😊。

And as a nice side effect， we are also able to compare existing fragment bias geneNs because they or most of them can be bounded by one of those free tests and then in turn we get some sort of hierarchy。



![](img/612e339adc11f63d4a03e36f622b3f2f_39.png)

Among those existing frequent bias gens。Okay， now that we have answered all questions。

 let's put everything together and build our own model。



![](img/612e339adc11f63d4a03e36f622b3f2f_41.png)

Which we call fragmentnet。so given a molecular graph， we first apply our rings past fmentation。And。

Then we use our ordinal encoding to get like representations for each fragment。

And for message passing， we use a higher level graph to ensure a to ensure maximum expressivity。

So we have in in total four kinds of messages， messages on the。Original graph。

Messages from the higher level to the lower level graph。

The other way round and messages on the higher level graph。



![](img/612e339adc11f63d4a03e36f622b3f2f_43.png)

Okay。So let's come to the empirical evolution in terms of expluivity。

 some common benchmarks and lastly generalization。

![](img/612e339adc11f63d4a03e36f622b3f2f_45.png)

We empirically test the exclusivity by its ability to count chemically important substructures。

And as you can see here in this table， our model is able to count perfectly simple substructures like this five ring and this four path。

😊，And this should not be a big surprise as those fragments are actually part of our vocabulary。

 so we provide this information directly to the model so。

It's no surprise that it can learn to count dose。But we are actually able to count also more complicated substructures that were not part of the original vocabulary。

 and this not only holds true for those shown here but for a wide range of substructures。

And this maybe shows a solution to the before mentioned conflict。

 We don't have to include all important substructures。

 We just have to include enough such that the model can then learn to combine the existing substructures and to learn more complicated ones。

It might be maybe a bit like in the German language where you can like combine simple words to build really complex fonts like Duno D Schd's capitanes Caelshaft。

😊，Okay， let's come to the empirical performance and here we focus on the popular penalized Lo P regression on zinc and the long range peptides benchmark。

And fragment is best among all fragment byst ends and at least achieves comparable performance to the state of the art model。

 which is the transformform a gridit。Okay， lastly， let's come to the generalization abilities。

And we test the generalralization abilities by removing all。😊。

But we want to test the ability to generalize to molecules containing completely unseen fragments。

And for this， be remove all molecules containing seven rings from the training set。

 but a test that still contains molecules with seven rings。

 So we have to generalize two molecules containing completely unseen fragments。😊。

And here our ordinal encoding actually really helps and we are able to greatly outperform the state of the art model。

 and this shows that we actually can transfer knowledge from similar fragments。😊。

With our or encoding。And in our paper， we not only showed that we improve generalization for unseen fragments。

 but we can also show that we are better in the setting where molecules contain very rare fragments。

 so at the tail of the distribution。And also molecules from a completely different distribution。

So in summary we present fragment， a robust and highly expressive fragment biasGN。

 it retains the linear complexity of traditional GNNs。

 we develop theory that with which we are able to compare the expressivity of existing frant bias GNs in this theory we achieve the higher level graph。

 the highest degree of expressivity。We show that our ordinal encoding actually helps for generalization。

 and we achieve state of the art performance at least for fragment bias gene ends。😊，And yeah。

 you can find our code and our full paper under this who QR code and thanks for your attention and thanks to the organizers and now I' am happy to answer questions if you might have any。

😊，Yeah， nias， like sex for a great talk。 So I have a small question。 It's like， yeah， recently。

 like more and more people are talking about like the foundation model。

 like in the molecular domains and also like， I want to see a small question。

 maybe I'm not so familiar with that， but I want to ask like。

 do you think such kind of like fragmentnet is a more suitable way like to observe like better scaling behavior because I see some kind of paper。

 They are working on this direction。 Yeah， thanks。I am not actually sure。

 I think this so fragment provides like really strong inductive bias by providing this additional fragmentation。

 And I think it's especially important， if you have like。If your data set is like smaller。

 then the model actually needs this inductive bias and I think for like bigger models。

It might not be needed。 And we already saw that like the transformer。

 which doesn't have this strong inductive bias， was actually able to outperform us on those bigger data sets。

O， gotcha。都是他们。Let me see if theres any other further questions。I think there's no further questions。

 And thanks Nicholas， for a great talk。 Yeah， thank you。



![](img/612e339adc11f63d4a03e36f622b3f2f_47.png)

And next， let's welcome the tomorrow。 So， yeah， can you open yourmic or like， share your screen。

 yeah。

![](img/612e339adc11f63d4a03e36f622b3f2f_49.png)

Yeah， so Tamara is a PhT student from the University of Edperor。

 and today like she's going to introduce her very interesting job is a new symbolic framework for answering graph pattern queries in knowledge graph。

Tmer， can you hear me， yeah。I think still yeah yeah。

 yeah okay I can hear thank thank you very much for the introduction yes will be presenting our method for answering graph pattern duties or knowledge graphs。

😊，So this is joint work。With Michael Coez and Daniel Rasa from the Ra University of Amsterdam。

 Palo Vercelo Ho Ro andil Romemerro from the Pontific University Se Chile。

 and Florsge and me from the University of Antwar。So I will briefly start introducing the object of study of our work which are knowledge graphs in knowledge graphs。

 the data is represented as entities and relationships between these entities。😊。

One key task we might want to perform over at knowledge graphs is query answering， as for instance。

 in this example， we want to know we might want to retrieve the people who have friends who lives in Santiago。

To do this， there is。Many efficient or not so efficient algorithms。

 but we just explore the graph and get the get the。😊，The answer is we need。

 so we start from Santiago， we look who lives in Saniago after we look who are friends with these people。

But one problem that we have with this approach is that in practice。

 most knowledge graphs tend to be incomplete， so the answers we get by applying these techniques might not be all the answers。

 all the relevant answers that we know that we want from the graph。😊。

So this problem has been tackled by the my children and community and the models that they use。

 they combine reasoning over knowledge graphs in particular link prediction with in between the evaluation of the query itself。

 so actually the train models that can evaluate queries while predicting some some of the possible missing links I will outline very general how these models work。

😊，In general， I have a query like the one I showed you before。

 they start from the constants of the queries in this case， the constant Ianttiago， they predict。😊。

The people that lives in San Diego， of course these include the links that are indeed in the graph and the links that might be missing from it after in this case we will do another prediction from this set。

 we do an extra prediction to see who are friends with these people。😊。

And then we can answer the query。This method can be summarized or can be described as a bottom up method of evaluating the queries where again we start from the constants and we go up up until the root or the target node of the  query。

 sadly not all queries can be evaluated in this fashion。

 actually just three like queries can be evaluated like this three like queries in particular three like queries that have constants in the leaves and the the only three variable or the target variable sorry in the。

😊，So what do we propose， we propose a strategy or a framework to leverage attach these methods by keeping the essence but allowing them to evaluate a broader classes ofqueries。

😊，So how do we do this and this is specifically what I want to talk with you today。

 our premier andra I will present it after I will talk about a bit about the implementation we use the particular implementation and the results and some final remarks。

😊，So in a nutshell the framework andravel given a query。

 the first question you ask is you look at the shape of the query you want to tree like if it is tree like then you feed it into a model like the one I described before and you get the answers if it's not tree like then it goes inside of our little unraveling process where from this nonT like query we approximated using tree like queries or tree like approximations and after we fit these approximations into the model and we get the answers。

😊，So what kind of approximations are， of course they are three like and I will now illustrate how to get this。

😊，This approximation， so let's take the triangle query in this case。

 our target variable is x and we have relation R S and T。😊，So let's go and travel the triangle query。

We start by the target variable in this case x and we will go nagating the query let me start going from x to set via the t relation so I will go from x to set after from set to y here in this case I'm using the inverse of the s relation and then I go from y to x okay and can I can keep going and this make this query as long as I want after I can do the other direction I go from x to y from y to set and from set to x and this way I have3 like approximation of the triangle query I can do several approximations depending on how how deep I want this approximation to be so for instance here I have the unraveling of depth1 to3 and you can。

You can go on and make it make them as deeper as you want。

So the the nice thing about this and traveling about this approximation is that they enjoy some strong theoretical warranties as the first two theoretical warranties I want to discuss are safety and conservativeness safety means that。

😊，The approximation we are getting our over approximation， which means we don't have。

 we don't get false negative every every。😊，So， yeah， we don't get fast negative every。

Every answer to the original query is also an answer to the approximation and the other property is conservativeness is that if we apply this unraveing process to a query that is like we get a query that is equivalent to the original one。

😊，Second and maybe more more important is the optimality for any quality we have that thetra this is the the。

😊，The query that we get by unraveling this query is the best possible over approximation from if you consider any other tree like approximation。

 then untraveling is the best one。😊，And finally， one other interesting property is that as I said before。

 these are over approximations which have no false negative but might have fast positive but you can go as you go further as you go deeper in these approximations they start refining refining refining and potentially when you go deep enough the approximation might tend to be better again this is。

😊，These are theoretical warranties or theoretical properties of the untraraveling map。😊。

So let's dive into the experiment here we have two main question that we want to address。

 the first one is how do these theoretical properties of the unraveling techniques translate into practice and the second one is in general how is the performance of unra in the benchmark in the citizen benchmark query benchmark and knowledge graphs benchmark for complex query answering。

😊，So in a particular implementation we use and we presented it in the paper。

 we use as the underlying。complex query answering model we use GNN QE。

 GNNQE is neuro symbolmbolical model that uses as the link predictor it uses GNN N VFNes。😊。

And for the other logical operators， it uses face set operators， basically all them。

 the operators are done using face sets and then faceologic operations over these face sets。😊。

For the experimental setup following previous work。

 we focus to address the performance of the model we focus on ranking metrics this is。😊，The model。

Will give us a score for all possible answers after the answers or the possible answers are ranked according to this scores。

 and we calculate the ranking metrics such as the neurociprocal rank or the hitet K by comparing this with the ground through。

 let's say with the real answers of the quedries。😊，And for the query sets， we also。

Include the traditional tree like queries used for the benchmark for complex query answering。

 but also and as we are more focused on the approximation schema。

 we include some cyclic queries in particular we include nine cyclic patterns that would introduced in a paper from last year from the real first or the logic  query and we also introduce fresh new cyclic queries which in this case have one special property that they don't have constant in between the  query。

 let's say everything is existentially quant either existentially quantified or the target variable。

😊，So we evaluate our model on these three sets ofqui。What the experimental result says。

 well the first thing we want to know is how the things go from theory to practice in particular。

 this property I talk about you。😊，I told Europe about some minute ago about the depth of the unrivalings。

 theoretically the deeper we go the better the approximation should be。

 but we see in practice and especially when focusing on this ranking metrics that this is not the case we did some further exploration and we see two possible explanations for this。

 the first one is that in the benchmark knowledge graphs。

 we don't tend to see the cases where we actually need longer。😊。

Longer approximations or longer queries to get the real answers this is one and the second come from one inherentrant weakness of several complex query answering models and in particular the one we use in our implementation DNA and Qe is that the longer the query the performance tends to get a bit worse this is because you start concatednating successive projections this is success in predictions and the errors start proagating。

😊，Along the query， which can。Counterbalence， the benefit one might get by using longer approximation so we series for the in particular here I'm showing the result for the triangle query and the square query on the data set on a free waste data set。

😊，Okay， so now let's move into the the real numbers the real performance of。

Of unravel for cycling queries， we see that in in the。😊。

To the rest of our knowledge first paper that addresses thecyclic query problem which was published last year and they also introduced these data sets。

 we evaluate our strategy in these data sets and we see that for two out of the three benchmark data sets in this case for the three waste ones。

 our model outer forms，😊，The other model of the model before was the state of the art forcycl queries and remains very competitive also for the nail data set。

😊，Moreover more， sorry for the cyclic queries without constant。

 we see that our model in general outperforms two baseline that we that we consider in this case。

 it allperformance on pretty much all of the queries except for one  query maybe or so on the nail data sets so we see that。

😊，That actually our framework is a viable strategy for answering pschic quis。

 which is the main purpose of the work。😊，As final remarks， I want to。

To to state that and driver provides a framework that allows existing model which have this limitation of all being able to evaluate tree like queries。

 they allow them to evaluate a broader class of queries and that it is actually a viable strategy instead of。

Of trying to handerra or going into more expensive models to evaluate cycles。

 one can use these approximations with a real nice performance。

So thank you very much for your time and your attention。

I will be happy to answer any of your questions。Yeah。

 it's a great talk like open up like a new directions or new sub directions in the knowledge graph answering So I have like very quick questions。

 So the thing is like yeah for long questions like what is like the time efficiency problem like across like those tasks because it seems like very like expensive especially the most those higher order problem I think especially like MBF nets they miss this problem quite often so I'm very curious about that part yeah。



![](img/612e339adc11f63d4a03e36f622b3f2f_51.png)

Yes， well in general， I think that the efficiency part comes from two ends。

 One is the projection itself， let's say the link prediction and the other one is actually evaluating the query。

 This is one advantage that have the models like the one I。😊。

I introduce is that they operate over sets so they don't have to actually。

Instiate each of the single entities and go doing the link prediction they all operates over sets。

 which is why this might by be an interesting like framework to take this complex query。

 make it into three legs so you can exploit the efficiency of working with sets instead of really addressing every single entity and doing the predictions and the projections。

😊，O ok， thank you。Yeah， let me check like if there are other questions here。W。Oh。

 I think there's no further questions。 Yeah， thanks all the great Okay we can look in the post that after anyway。

 thank you。 Yeah， okay， thank you。😊，Okay， so thanks everyone， for the attendance。

 So this will be the end of this part oral talk。 and then the next part will be a tutorial about hit roughly graph。

 which is start half an hour later。 Okay， thanks for the attendance。



![](img/612e339adc11f63d4a03e36f622b3f2f_53.png)

hello。嗯，停点。Yeah。So hellello everyone， welcome to the lock conference and tutorial on Heophilicgraphra learning I'm Xiaox Xing and we willll be the host for this section so today we are excited to have a fantastic lineup of speakers who will present the latest progress in heteroophilgraphra learning now let's welcome our first speakers to time to and tutorial on。

Okay， so I can hear some echoQ Okay， so now let's welcome our first speaker sito to present the introduction and background knowledge Sio is a PhD student at McGill he is working on graph representation learning and AI for science。

So。Cit， you can stop with your presentation Okay thank you for the introduction X Ho everyone my name is Sian。

I'm a postdoc at MillerA。Today， I'm very happy to give the tutorial of Catophilicgraph Learning on conference together with the two brilliant speakers。

 Jian Jinhua and Qin Chenglu。啊。I would also give some special thanks to our code assistant。

 Jia Qiizhu and advisors Guo and Xiao Wencho。So。What is hyperphily and why should we care about it in graph learning so before we introduce hyperphily。

 we need to know the concept of homophily。So hopefully in ancient Greek it means same common or friendship love。

 it is a concept in sociology and evolutionary biology。

 and it states the tendency that individuals with similar characteristic are easier to communicate and bond with each other。

There are a lot of example in our world that has the homophily phenomena， for example。

 the birds of the feather blocks together or assortive beating， and in our real life。

 there are also some homophily phenomena， for example， in social media， people with similar ideology。

 religion， interests， education background， race and ethnicity。

 are more likely to connect or follow each other。So what is homophiing in graph learning or more specifically homophiing in message passing？

啊。It isInvest passing homely is a principle or assumption that is implicitly imposed in our v passing process。

 as shown in this figure。For the indiscernable boundary node， if we have a homophiic graph structure。

 this kind of structure will provide extra useful information into the aggregated node features over the original node features。

So that the indistinguishable will become distinguishishable after the measured passing。

 so such kind of relational inductive bias is thought to be a major contributor to the superiority of GNs over traditional neural networks。

 or various tasks， especially on node level tasks。So hyperphi means lack a homophily or low homophily。

More specifically， notes with different labels from different causes are more likely to be connected。

If we do message passing on such kind of graph structure。

 those from different classes are more likely to be connected and their future are more likely to be mixed。

So that notes from different classes will become indistinguishable and this will make our GN harder to classify them。

There are also some empirical observations。We found that some graph aware models will underperform their corresponding graph agnostic models on some data tests。

 for example， Cornell， Wisconsin， texts， and field。

A simple MOP can outperform some baseline G Es like GCN， GATT and graphs。嗯。

And heterophily is considered as one of the main reasons for this kind of performance degradation。

And they found that kind of these data sets marked by red have some low homophiiame。So this means。

There do exist some kind of bad benchmark graphs that we need to be very be careful about that。So。

Can we recognize those kind of bad graphs with a scalar metric and which set of graphs are the really bad graphs and how can we categor them？

So in the next two sections， Qin Cheng will introduce these two topics。Let's welcome， teacher。

I will stop sharing here。Thank you，tao。 in this section。We will introduce homophily metrics。First。

 we have metrics based on graph label consistency。They are edge homophily， not homophily。

 class homophily and adjusted homophily。These definition are all based on linear feature。

 independent graph label consistency。So if the matrix value is small。

 it indicates the inconsistency between graph and label。

And graph structure would have an negative effect on the performance of June N。For example。

 here we give the definition of atphily and not homophily。

The actualphi measures the proportion of edges that connect two nodes in the same class。

And node homophily evaluate the average proportion of edge label consistency of all nodes。

Then we also have similarity based matrices。These metrics are based on some linear measurements of low similar。

With no features。For example， the journalist at home Phil use cosine similarity between those features。

So currently， the matches we have introduced are all based on pairwise comparison。

Then the third type is the neighborhood identifiability or informativeness。

These metrics are based on a neighborhood distribution， instead of pairwise comparison。

They are nonlinear and are independent of not features。For example， the label informativeness。

It finds different connectivity patterns by measuring the informativeness of the labels of neighbor。

The last type is the hypothesis testing based performance metrics。For this type of metrics。

 please find the reference here for details。Basically。

 it use the P value of hypothesis testing as metrics to measure the no distinguishability of the aggregate feature compared with the original features。

This matrix is the first one that can capture nonlinear， feature dependent information。

So we have no various kind of homophi metrics。 homophimetrics are basically proposed to identify tough hydrophilly data sets。

Next， we would like to do a comparison for them。The purpose is to see whether this matrix can identify good or bad graph。

The approach is that we use the performance of baseline J N models on synthetic graphs。

As synthetic grafts can be generated for various kind of homophie levels。So for each homophi level。

 we can check whether the homophi metricss agree with g performance。

So if the matrix value agree with the during performance， we can see that this is a good metric。😊。

Specifically， the steps are summarized here。First， we generate the synthetic graph with various homo levels。

 then for each generated graph。Notes are randomly split into train validation and test sets。Next。

 we train the baseline model on this graph and calculate matrix values for each graph。Finally。

 we apply the metrics values and the G performance curve with respect to homophi levels to see whether there is an agreement between metrics and G performance。

In this comparison， we generate synthetic graph using use the following frameworks。The regular graph。

 professional attachment and gin cat。The first one。

 the regular graph is generated according to the attromorph level and the base data sets。

Where for each node， it uniformly generate some random inter class edges and some inter class edges。

And note features are sampled from the corresponding class of the basic data assets。

The professional attachment method uses coefficient to control the probability of creating intra class edges and sample node features from overlap to de Gausion。

The last one， the Jinkai model generated graph based on the edge connection proportion coefficients and base dataset。

Where a large coefficients means fewer into class edges。

And they propose an algorithm to generate node feature and edges based on the coefficients and based data asset。

We conduct experiments， and these are the results。He each column corresponds to a generation method。

And the first role is for G performance。Where the green and blue lines are for G C N and SG C performance。

And the second row shows the matrix values。And for each graph。

 the X axis represents the homophily levels。Then for each generation method in a column。

 we compare the June performance curve with in the upper figure with a metric curve in the lower figure。

Then from this curve， we have several observations。

 and we conclude that current homophimetrics are not good enough。

Because we compare when we compare the June performance curve and matches curve。

 we can see that the correlation between different metrics and June performance are different。

For example， we can see that on R G in the first column。

 the G performance curve in the figure A is u shaped。

Then a good homophiometric should also have this kind of view shape curve。

Then we check the mattress curve in figure D。For R G。

 we can see that probably the aggregation homophily is the best one。However。

 we can see that on other two types of synthetic graphs。

The agreement between aation homophily and the gym performance is not so good。Therefore。

 we still need to find a good homophilimetric。 and an idea homophilimetric should show same correlation with gene performance on graph generated by all these three methods。

So in the future research， a newly proposed metric should be tested on synthetic graphs with all three generation methods to get a comprehensive evaluation and comparison。

So here we provide the repository for synthetic experiments。

The repository can be assessed here by scanning the QR code。

We also build the collab and we here show how it works。For each generation method。

 the workflow is the same。 And here we use the R G， for example。First， we choose the coefficients。

 which control the homophiic patterns of the graph。

 and we use the query data sets at the base data sets。

It will generate 10 graphs use home of user coefficients。

Then we would evaluate all these metrics on the graph。And finally。

 we can get a plot showing all the matches values here the position of each circle stands for the mean matches value and the size of circle stand for the variance。

So in the next section， we would discuss benchmark data sets。So in the pre section。

 we find that current homophilimetrics are not good enough。

So for distinguish good graph and a better graph， the only standard we have now is the actual performance on data sets。

Which means that when to compare the performance of so called graph aware model and graph agnostic model to know whether the graph structure is beneficial or not。

SoIn this section， we would like to summarize and categorize the popular benchmark data sets for heteroophilic graph learning。

So here， for example， the GCN is a graph aware model。

 and the corresponding graph agnostic model is MP 2。

Since the only difference between G CN and MP 2 is whether the graph is used。 and similarly。

 the corresponding graph a model for S G C1 is MP1。Here we exam 27 popular data sets。

We compute the edge and node homely on on these data sets。

 and we provide the performance of baseline， graphware and graph and models。

So we say if the graph aware model performs better than the graph agnotic model。

 then the graph structure is beneficial for G N。Then based on this result。

 we can observe three kinds of patterns for hydrophil data sets。For the first category。

For these data sets， the edge homo fully and node homo fully are very low。

And the graph aware model performs worse than the corresponding graph agnostic model。

So these data sets are the most difficult data sets for G N。 So we call them malignantphily。

In this case， the graph structure provides harmful information in the future education step。

For the second category， we can see that the graph aware model perform better than graph agnostic model。

😊，But the node and edge homophily values are very small。So。

We still agree regard them as hydroophily data sets。And call them benignophilly。That in this case。

 heteroophilate aggregation is actually beneficial for Gens。Now， we look at the third type。Here。

 the graph aware model and the graph agnotic models have inconsistent comparison results。For example。

 for the secure filtered these data sets。We can see that ML LP2 perform better than G N。

 but S G C1 performs better than ML LP1。So we'll call this kind of data sets as ambiguousfully data sets。

In this case， the underlying synergy between graph structure and model linear nonlinearity can influence g performance altogether。

So here we summarize all these results。Our total repository also provide codes to train these hydrophilly specific GNs on 27 data sets。

So let's go through the workflow together。First， we can choose a journal model from the menu Here。

 for example， we choose the AM GC N。Then here on the left panel on this page。

It shows the step where we choose the data sets。And on the right panel。

 it shows that we are training the model on， on these data sets。Next。

 will introduce hydrophilly specific models。

![](img/612e339adc11f63d4a03e36f622b3f2f_55.png)

![](img/612e339adc11f63d4a03e36f622b3f2f_56.png)

Sorry。I think。

![](img/612e339adc11f63d4a03e36f622b3f2f_58.png)

Any that's that to。

![](img/612e339adc11f63d4a03e36f622b3f2f_60.png)

![](img/612e339adc11f63d4a03e36f622b3f2f_61.png)

O。Okay， thanks， Ting。In the next session， I will。Introduce some popular method to address the hydrophilic problem。

We mainly summarize 10 most popular methods。The first one is Eagle neighborhood separation。That is。

 we encode the Eode embedding and its aggregated neighborhood embedding separately。

Because they are likely to be dissimilar in healthability setting。

Zhu and others proved that concatednation is a better combination function than averaging function on the generalization of heterophigraphs。

And we proposed the H2 GTN。With the Eagle neighbor separation。

The second method is signed message passing。So before we introduce smetric packing。

 we need to introduce two concepts that is the frequency and low cost scter frequency is the eigenvalue of bra Lapaian and small eigenvalue corresponds to the smooth eigenvector and large eigenvalue corresponds to the nons eigenvectors。

嗯。😊，A future is called low pass， if it does not significantly affect the content of the low frequency person signal or the smooth signal。

 but ats the magnitude of the high frequencyency components or the non pass component。

It is found that the neighborhood aggregation step or the multiplication with the normalized a matrix can be seen as a special form of low pass filter。

 which captures the low frequency or the smooth information in your input low features。However。

Bo and others found that high frequency information is also important to capture the differences between those。

 and especially in hydrophilic setting， we need to add low pass。

 low frequency and high frequency signal together。So we propose the FAGCN。

 which is pretty similar as the G At network， but they allow the attention score to be negative。

So that the negative edge weights can propose the high frequency information。Also。

 Xie and others use the generalized age rank。Which associate a learnable weight to each step of validation for different hops and the weight is learnable and can be both positive or negative。

And the signs of the weights can adopt the heterophily and homophily structures。

Li and others proposed the Galogian， which estimate a coefficient matrix to model the relative importance of those。

The coefficient matrix Z is learnable and it can be solved by solving an optimization problem on local feature similarity and multi of graph structure similarity。

So the Z matrix is different from the simple attention mechanism or the self attention mechanism。

 it allows the negative force and is more efficient to compute so that it can solve the heteroophil problem。

The third method is directly using the Hypascutor and use the nodewise channel mixing mechanism。

 so it is proved that Hypacutor is effective for some kind of hyperability situation。

And Luan and others also found that different nodes may have different kinds of local hyperophillic situations。

As shown in this figure， so for different nodes， some nodes has a pretty low local homophie situation and for other nodes they might have very high local homophily situations and this curve this distribution are different from between different data sets。

 therefore different nodes may need information from different channels。

 some need information from low pass channel， some need information from high pass channel therefore。

They developed a adaptive channel mixing mechanism， which include low passs。

 high paths and identity channel together in eachGN layer。

We also develop a nodewise channel mixing mechanism。

 which is learnable to combine the channel information adaptively and not wisely。

So here is a visual of the output。So as we can see the input feature is very， it's pretty noisy。

 we can see no patterns in the input feature if we fit it to GCN。

The output of GCN still has no clear boundaries between different classes， however。

 if we feed it into ACM GCN。We can see that in low pass channel， still no clear patterns。

 but in high pass channel and identity didn channel， we can see that。

Some some patterns already start to emerge through the nodewise channel mixing by the upper values in the final output。

 we can see there are very clear boundaries between different nodes。

 especially when you compare it with the output the original GN。And through Appplach study。

ACM is demonstrate that it can。Boost the baseline the performance of data。

 and it can surpass the so models on almost all data sets。The fourth method。

It's the selective message passing that is the impose a selective mechanism in the message passing framework so that the model can learn to return the use or relevant information and discard the detrimental parts in the node embedding。

 for example， in GBBTGN。The author use。Two kernels NGCN。

One kernel to deal with the homoophilic node pairs and the other part deal with the heophilic node pairs。

 and they also use a gate which is included by an MLLP。And it's learnable。

It can understand which of the two kernels should be applied for each note pair。And in FSGN。

 the authorss use soft Macax as a regularizer and a soft selector for the features aggregated from neighborhoods at different hop distance。

And this year， Finnkkoen and others proposed the cooperative GN。And in this network。

 each node has the select to choose different actions from an action set。

Where there it includes listen， broadcast， listen and broadcast and isolate actions for different notess。

 so different notess has the freedom to choose what we want to do in the massive passing。

 so there are more flexibility and more expected power in the massive passing for work。啊。

The fifth method is the spectralGNs， spectralGN is a type of network that is built on graph signal filtering technique in the spectral domainamine。

 people who try to develop more expressive spectral filters to enhance the aggregation function for better performance on a heterophiligraph。

For example， he and others proposed a burn， which use burst polynomial to learn arbitrary graph specbuts。

 Wang and others propose the Jacob con， they use the Jacobcopi basis due to its orthogonality and flexibility to adapt to a wide range of weight functions。

He and others proposed the Chat2， which fix Chat with Chasha interpolation。

 it enhances the original chair shelf for polynomial approximation and can reduce the wrong beom。

So another method is beyond local information。So the logic behind it is that since we know that the local information are not informative or they are bad。

 so why not try the information from distant notes。So。

There are mainly two different methods to encode the dis information the first one is through the multihg information we can do it by DFGNs。

 for example Jnet APPN or others there is a special multihg method called link X it is pretty simple it encodes the ag matrix by an MLP。

And the author show that it can encode the。Monoly information in social science into such network。

 and it can be considered as a kind of toolh method。

Another kind of method to encode global information is to connect distant node in a latent space。

 that is you rebuildbu the neighborhood with a latent known embeddings， for example， in GMGCA。

Pay and others precompfuse the unsupervised node emdding in the late space and redefine the geometric relation and reconnect notes so that some distant note might be directly be reconnected。

However， in this year， Xin and others found that we cannot increase the receptive scope blindly because the distant notes are not always beneficial in heteroophilgraph。

For example， in this figure， the curve being the proportion of Khawk neighbors that have the same label with equal notess。

So。The actor that is the red curve is the curve for the heophilgraph and as we can see。

For one hop neighbor， they are around only 20% of the local neighbors that will share the similar labels with the ego notes。

 however， when you increase your receptive field，To the Khawk neighbors。

 the proportion almost remain the same。 That is。The multiho neighbor does not necessarily means they are good。

 so we cannot just blindly increase our receptive scope。

 we need some adaptively some adaptive mechanism。ForFor the reive field。

 therefore Luu and others proposed the flexible diffluion scopes by PDGNN。

We propose a new class of parameterized laplastine matrix。

 which probably offers more flexibility in controlling the diffusion distance between node than the conventional graph laplastene。

 and it allows the long range information be captured adaptively through the diffusional graph。

The seventh method is class structure learning。So since GN are highly sensitive to the quality of the given graph。

So why not we just optimize the graph structure so that GN can learn on a good graph instead of。

Instead of a better graph， so there are two main ways for graph structure learning。

 the precomp graphing rery and end to end structure learning。So for the pre completed one。

 the graph structure is optimized。The graph structure of optimization and model training are to separate process and they cannot be integrated into a single differentiable pipeline and for the end to end learning。

 the structure optimization is inserted into the model training pipeline and the whole pipeline is differentiable。

For the precomputed graphic rewiring， people usually define a metric for the guidance of the rewiring。

 for example， Gong and others propose the homophily oriented rewiring。

 that is they define the neighborhood homophi value。

 which measures the label complexity in the neighborhoods and members are grouped and aggregated based on this metric。

And similar like the homophily oriented one， Lee and others proposed the distance oriented environment method。

Surs propose the similarity oriented reviv method and Troy and others proposed an information theory based method。

To do the graph very。So for the end to end structure learning。

 Zhao and others used a neural edge predictor which can learn to promote intraclass edges and demo intraclass edges during the model training process。

And Wu and others propose a probabilistic based method。

 he and others proposed the causal inference based training and Ye and others proposed an edge requirement for the structure learning。

So although it is found effective in some scenarios。

 there is also some controversies around graph structure learning， for example。

 Joehou and others inherently find that there' is no significant correlation between the homophily of the learn graph and the performance of the tasks which challenge the common belief that higher homophily can lead to better performance。

And Zg and others provide a theoretical analysis showing that the similarity based re method provides no information being on those specific tasks and questions the effectiveness and necessity of infrastructure learning。

So the eighth method is。The physics informed method。

So physics informed method incorporates physical constraints or physical laws into breast learning。

 for example， the repulsive or attractive policies。

 these kind of physical laws can help to model the complicated interactions between those including the haophilic relations。

For example， in the All key message passing， the author use All key force。

 which is associated to some particle system。That is influenced by both attractive or repulsive forces。

There's some other method， for example， Choy and others proposed grid， which include a。

Reaction diffusion layer with the combination of three standard reaction equations from natural sciences and for additional reaction terms。

 Zhao and others proposed the convection diffusion equations。

 which use a term for homoophilic neighbors and convection term for the heteroophilic max propagation。

Park and owners propose the reverse heat diffusion process。

They use the reverse of the heat diffusion process to learn the known states in the past time steps。

 which are far from the equilibrium states of the diffusion process。

The ninth one is enhanced information diffusion that is they boost the expressivity of GNs by modifying the information diffusion process in the aggregation function with more advanced and sophisticated mechanism。

Yeah。For example， in auto g M。The authors aligned the hierarchy of the rooted tree of a central no with ordered neurons in the representation。

Making specific specific plot of neurons targeted for mass passing within specific crops。啊。

In half hop， the author use an edge upsely method that adds slow nodes at each edge to mediate the communication between a source and target node。

There are some other method， for example， Boner and others propose the shift convolution that use a hierarchyical shift diffusion process。

And Michelly proposed the graph Estate network。Where they use the。The echo computing。

 the Rer computing paradigm， that is the recursivelyfuse input and latent neighborhood features with random initialized and untrained input to rer and reservr recurrent weights。

The tenth method is graph transformer， the self attention in graph transformer essentially reconstruct a learnable。

 fully connected graph structure， and they are shown to have the ability to capture long range dependency between those and they are more effective than a graph network hydrophilic problem。

However， it is imperatively found that there is still a huge gap between the performance of blood transformers and the SoGs hyperophilic problem。

There are some efforts that try to make graph transformer work。On heteroophilic graphs， for example。

 Bo and others proposed the specform， they encode a range of eigenvalue via positional encoding to capture both magnitudes of frequency and relative frequency information。

 and they design theder with a bank of learnable basis to capture high frequency graph signals。

Lee and others proposed the MP former。We they aggregate the center node and the different hub of neighborhood information。

 and they treat the node features as token and sterilize the token as sequencees。

And we can capture the distinction between the egoos and the neighborhood nose。

There are also some other method。For example， the compatibility matrix based method。嗯。

Compatibility matrix are just the model connection。

 probability of nodes between each pair of classes， it is a fine brain classwise relation。

And it is pretty powerful when those features are incomplete。Zhu and others propose the CPGN。

 which model the label correlation through a compatibility matrix and propagates a prior belief estimation into the GN with the compatibility matrix。

Zong and others propose clip we use a constraint enhanced compatibility matrix to construct。

 desired neighborhood messages。Another interesting one is to use the edge directionality。

Rosie and others shows that treating the edges as direct increase the effective homophie of the graphs。

 suggesting that a potential performance gain from the correct use of directionality information。

The logic behind it is that if we have。An edge from A to B。

It is possible that we don't have an edge from B to A。

So we should allow this kind of flexibility in our mess passing instead of that we only assume a connectivity between A and B and we assume this kind of symmetric relation。

So they design and return that。Which account for the edge directionality information by separate a of the incoming and upcoming edges？

🤧嗯。Luan and others conduct fair and comprehensive evaluation over almost all the above popular methods and from their experimental results we found that only the high pass filtering with adaptive channel mixing and the selective message passing are verify to be really effective for heteroophilly and for other So GNNs a lot of them just sacrifice their ability on homophi graph to achieve relative better performance on heteroophil graph。

 for example， is to GCN， GPRGN， Burn， link X and G GN。

And this kind of heteroophilia fever result implies that their proposed methods are not universally effective。

They also observe some serious scalability issue， for example， GGN。

 GKGN and FSGN suffer from severe out of memory problem。

 which means that we should consider the computational complexity when you design a hydrophillic specific model。

Yeah。Any question on this section？Will we have a question session after the tutorial？哦。

Hi Se yeah do you mind turn back to page 46，46。Yes。

 yeah so I have a question about the compatibility metrics， so how do define the compibility metrics。

 is it the learnable matrix or heuristic metrics。I think。It is learned。

It is estimated through the learning process。Okay yes。

 and we we since we have the label and we have a ground truth one。

And I think I remember in the original paper， the author just compared the ground truthatibility metrics with the learned compatibility matrix they found that it can be learned from the data。

Okay， got it， thank you。Are there any other questions from the audience？Yeah， if no of this go ahead。

 Okay， that's mobile。So。In the following section， I will introduce the。

Theoretical analysis around homophiia and hephie。They mainly investigate how those homophily and hyperphily impact the behavior of GNs。

So let's start from introducing the shortcomings of the homophie metrics。

So the old homophimetrics mill captures the graph label consistency， that is。

 we measure the proportion of edges that connect nodes from the same class。So however。

 based on such kind of principle， they fail to explain some low homomobil cases， for example。

 in this figure， the message passing on by party graph has some kind of different behavior。

 so as we can see all the nodes from class red connect to class blue and all the nodes from class blue connect to class red。

 and there is zero intratra class edges。So based on the old definition of chromphilimetric。

This graph has zero homomoophil value which means this graph is a very bad graph。However。

 after F aggregation。The note they just switch the note colors。

 but they are still distinguishable for the classifier。

 so which means such kind of graph label consistency is not enough to understand the G behavior and we need to find some different perspective to consider graph structure and homo buildinging。

Luan and others propose to study the homophily from the post aggregation known similarity productive。

 and they define the post aggregation known similarity matrix， the S matrix。

 they define a new aggregation similarity score。Yeah。

This score mainly calculate the proportion of node that are more similar to node from the same class than node from different classes。

And they define a new a new metric based on this score。

They conduct synthetic experiments to see the agreement between the metrics and G performance。

So for a good metric， we are expected to see a monotononic increasing curve for the G performance that is a low metric value we will indicate the bad。

GN performance and a high metric value can indicate the good G performance therefore the curve a good curve should be monotonically increasing however。

 under some old homophiia metrics， for example， the actualphi not homophiia or classical homophiing we observe a u shape curve。

 which means this old metric cannot explain the performance of G in the low homophiing area。

In the new proposed metric， although it has some variance。

 but it looks like a monotonically increasing metric， which means it is。

A better metric than the previous metric to explain G performance。

So another perspective by Ma and other others is that。

They propose as long as node within the same class share similar neighborhood patterns。

 their embeddings will become similar after aggregation， for example， node one and2。

Note1 and two are from class blue， but they all connect to class orange， yellow and green。

 so all their connections are hydrophilic， but the hydrophilic patterns are the same so after aggregvation their node embeddings will still be the same so it will still be classified into the same class。

Therefore， they claim that homomobile is not necessity for bracket to perform good。However。

 such kind of analysis has a deficiency that is the only consider the intraclass node the distinguishability。

 but ignore the intraclass node the distinguishability， for example， suppose we have。

The node3 which is from Class green and node3 has the similar neighborhood pattern with node1 and2 so after aggregation node3 node3s embedding will become similar as node13 or node1 and2 and will be classified into class blue。

 which is bad。Therefore， Lu one and others propose that we need to consider both intraclass and intraclass node distinguishability。

 and an ideal case is that we have a smaller intra class node distinguishability than intraclass node distinguishability。

 for example， the node1，2 and4。So has this kind of ideal condition。So based on such clay。

 the authors propose to quantify the no distinguishability on a new proposed toy model CSBMH。

In this CMBMH， we have a parameter that can control the homophie level of the geneative graph。

And we compute two metrics to quantify the node distinguishability that is the probabilistic bias error and negative generalized Jeffy divergence。

And they plot the relation between the homophie levels and the node distinguishability on this toy example。

 they found that a medium level of homoe has a more detrimental effect on the node disability than extremely low levels of homophie。

And they call it the mid homomophi pitfall。So Zheng and others find that the current understanding of homophie is still not comprehensive because all the above analysis only consider the label aspect of graph data。

And since we have three basic elements for B structured data， that is the structure information。

 the node features， and the node labels。So there are two missing aspects we need to study。

So Zheng and others disentangle the effect of homophily into three different aspects。

the and label structure and feature homomophiia and they claim that the synergy between these three components can provide a more complete view of the impact of homomophiia on G performance they define the label homomophiia as the consistency of node labels across topology。

They define the structure homophie as the consistency of node structure information within the same class and the feature homophie as the dependencies of structure agnostic node in node features on the topology。

So based on this three definition of homomophie。They conduct analysis on a new proposed graph generation model called CSBM 3H。

And we derive a new metric which is called tri ho， and it considers all three the predefined aspects。

Through the tests of 31 real world graph data sets and the comparison with 17 existing homophie metrics。

 Trahooma is verified to have significantly higher correlation with gene performance than the existing metrics。

All right。Okay， Mao and others proposed a fine grade analysis of homomo。

They found that both homophily and hyperphily patterns exist within a single graph。

And GN can outperform MLOP on one pattern， but fails on otherwise， for example。

On the figures on archive data sets。So the y axis of the figures is the MLP performance minus GCN performance。

The column below the0 line means MLOP will underperform GCA。

And the column over the zero line means NOP will outperform GCA and more and others study such kind of difference on。

a more f level of homophiing intervals， and they found that on archives。

MOP will underperform GCN in the high homophiing intervals， but on squir root。MlP will。

Outtoperform GCN in the whole home high home building intervals。So they show that。

GN form admirably on nose with the majority neighborhood patternss。

 that is the homoophilic node within homoophilic graphs and hyperophilic nodes within hyperophilic graphs while struggling on noses with the minority patternss。

So。They conduct analysis on such a disparity of the performance。

 and they propose a rigorous non ID pack based generalization bound。

 revealing that there exists some kind of distribution shift between the training and test node。

And this kind of distribution shift leads to the performance disparity。

 and we identified that the homophiia racial difference between training and test node。

 that is a graph structure shift as a new graph out of distribution scenario。

There are some other interesting study to explore the relation between distribution shift and homomophily。

 for example， Loveland and others and Zhao and others study the shifts between local and global homomophiity。

Zhu and others study the shift between input features and output labels。

Hatophie is also related to other problems that G will suffer from， for example。

 the oversmthing problem。So oversmotthing problem is that as the depth of G gets deeper。

 the node representationations are gradually smoothed out and the information loss will occur。

Yan and others take a unified view to explain the oversmthy and hyperophilic problem simultaneously by profiling those with two metrics。

The relative degree of node and the node level has ph。Based on the two metrics。

 we managed to operate dons for oversing and heteroophilly and predict the GSN performance。

Here we would like to emphasize the difference between more smoothie and hetero。

That is oversthing will only happen deep GCS， but not in shallow Gs。

 but heteroophilly will cause performance degradation to all kinds of Gs no matter it is deep or shallow。

Heerophil is also found to be related to the over question。

Over question means as the number of layers increases。

 the information from the exponentially growing re field is compressed into fixed length node vectors which will cause information loss for messages from a distant node。

And the Jacobbin of those representations is a popular tool to study the oversption problem。

Rubbin and others use the Jacob matrix to develop a unified。

 theoretically theoretical framework to understand the combined effect of hydrphily and overs sququaing。

They name it the homoophilic bottlenecking， which means the bottlenecking between those of the same type。

There are also some other interesting theoretical findings， for example。

 Choy and others point out two drawbacks of the signed message propagation that is。

Under some conditions that。Those from different classes share a high similarity。

 the sign method passing will decrease the separability between those and also the sign method passing will increase the uncertainty and instability of your model。

啊。Shi and others study the double decent phenomenon in graph learning。

Double descent means increasing model complexity will first reduce the tester rates and then leads to higher risk due to overfitting。

However， when you continue to scale up the complexity of the model into over parameterized regime。

 it will result in a decreasing risk again， so this is called double disent。

Shi and others studied the relation between double descent and homomobile。

We predict the existence of double descent in GNs and expose that a negative sub will result in better performance on health playground。

Lee and others study the future shortly。graph network。

 that is we explore how random feature shling among those in the same class affect GM performance。

They found that the feature shortling will reduce the dependency between graph technology and features。

 it can boost the GM performance on a homophiic graph， but not in aheadophil graph。ok。Any question？

commentsments on。this section。Hi theres a question in the Q andA box could you please check yes so there's a question was the intuition behind the better performance of German and US MP on the Heophil。

ok， so。The question is， what is the intuition behind the better performance of GNs versus MLLPs on heterophy？

Benign and a graph。 Yeah， that is since what we are studying is to find out the bad graph structure for method passing。

 right， and it is。Found that not all hyperphiicgraphs are bad for message passing， right？So。

We would like to。Through the experiments we would like to find out， so which hetero ofophilic？

Which hetero of telegraph will lead to the worst performance？For the network。 and which will not。

So on the benign heteroophil graph， those are the graphs that will not lead to bad performance of the network。

Abiguous graph is it depends so in some kind of cases it will lead to bad performance and in other in other other cases it will not and the malignant one is that it always lead to bad performance so the malignant catophilic data sets。

Are the real embedded。Graphs that we would like to pick out to test your model。

So that is why we would like to categorize those kind of。He ofophil grass。So。Yeah， that's it。Yeah。

 thank you for addressing this question。All right。So。你播歌。Yeah。啊。In this section。

 I will introduce some hyperophil related applications。诶。

The most popular one is the detection task that is the fraud anomaly bo detection。

The detection tests aim to identify and isolate the abnormal and malicious nodes of subgraphs in the network。

 which can have significantly impact on the security privacy robustness for real applications on network data。

For the graph based fraud detection。The f nos are usually surrounded by dorm nodes that have been cheating。

So we inherently have the hyperophilic structures and it plays an important role。

As the figure shown on the right side， the red notes are fraud users。

 the blue notes are legitimate users and we can see that the fraud users and legitimate users they are densely connected each of each other。

 therefore we have very highly hyperophilligraph。For the fraud detection graph。诶。

For other detection tests for the graph based anomaly detection。

It mainly identify the real observations that deviate significantly from the majority of the objects in relational and structured data。

The abnormal users are sparse and connect to the majority of normal nodes which lead to the hydrophilic structure。

For the bo detection， the bos are automated programs that mimic human behaviors and they usually have some malicious purpose。

 for example， this spread， some misinformation， some hates。

 violence or something bad and those bos having shown that will intentionally interact more with normal people。

 therefore it also has some catophilic relations。Hetroophilly is also closely related to some traditional graph learning tasks。

 for example， graph coloring。Graph coloring is to assign different colors to the nose of a graph such that no adjacent node will have the same color。

Wang and others recognize the graph coloring task as cattroph problem。

 and they introduce negative method passing into physics inspired graph network。

 which allows node to exchange information with their neighbors in a negative way such that the note features can become more dissimilar after each met passing layer。

There are also some other graph learning tasks。That is related to atrophily， for example。

 link prediction， graph clustering， graph classification， and recommend system。啊。

There are also some computer vision tasks that have scenario of hyperphiia， for example。

 the point curve segmentation。Point cloud is widely used of 3D data。For the 3D computer mission。

And print of segmentation。It aims to divide the unclassified target point cloud into separate regions with different attributes or functions。

However， a most popular method for green cow segmentation implicitly assume homophily and ignore the hyperophilly nature of some edges。

 especially at the boundary regions。As shown in the figure on the right side。

In the boundary re notes， there exists some local heteroophillic structure and the ignorance the ignorance of the heteroophilic structure will undermine the distinguishability of node representations。

诶。Urban networks are often hyper as nodes with different attributes or functions may have strong connections。

 for instance， roads with different traffic conditions。

 land uses or geographic locations can influence each other。

And urban computing involves designing and optimizing the structures and functions of urban networks and can be better learned by GNS with Ha。

Brain networks also have a half of the leak structure in it。嗯。

The brain the brain regions of interest that is ROI respond to nodes and the connect features represent the edges and from this kind of graph sample we can see that there exists both homoe and heavily structure and。

Because this seminari can physically attach to each other。

Such interplay between homomophily and catphily will poses challenges to the analysis of brain networks。

Yeah， that's it。ForFor the application， those applications share some similar properties。

 that is first you use message passing all these applications and in these applications you have。

The graph that knows with different attributes were densely connected， so in such scenario。

 Cattrophi will play an important role。So。Any questions on the applications？ok。So in a next step。

Yeah， I'm talking about like okay， challenges yeah yeah， okay， in the next section。

 we will'll introduce the challenges and future directions of catophilic graph learning。

Is welcome to you。Yeah， heteroophilly could have impact in heterogeneous graph learning。

 hypergraph learning， also temporal graph learning， as well as a molecule generation。

Hedlogeneous graphs refer to the graphs with multiple types of nulls or edges and like but the current studies hily only study。

Only focus on graphs with single types of nodes or edges。So what we need to do is we， we need to。

Establish。A complete benchmark for heteroophilly heterogeneous graphs。

 as far as we need to bring more like a awfulophilly based metrics for heterogeneous graphs。

And second is the。Is the howophil usage on temporal graphs。

Heerophigens are mostly investigated on classification tasks or static graphs while under。

While underexd node level regressions over dynamic graphs， in this case。

 we just have time bring graph homophily values as shown figure from graph A to B to C to B this graph structures where like homophily values should just change over time。

 so we need to find a better way to define the homophily value temporal graphs as time goes by also we need a complete benchmarking for or had hopefully on temporal graphs。

In the survey is heterophily application hypergraphs。

 hypergraphs refers to those graphs where edges can connect to multiple nodes。

 like more than just two nodes and heterophily has been proven as a more common phenomena in hypergraphs than being simple graphs。

In the previous study。But they haven't addressed the challenge。

 like how to define the homophily metrics on hypergra。

As well as also we need to establish a better benchmark on this hydroabbs and related hetero learning。

Also had roughly could have an impact of fairness in graph representation learning Loveland review the link。

Between group fairness and local homoography and discovered that not all known neighborhoods are equal and those dominated by a single category of assistive attributes often for changes in achieving fair treatment。

This particular， these are really particular when local class and sensitive attributes are homofully diverged。

Also， Lo introduced a node injection based furnace attack called MIFA designed with a homophily increase principle。

 it can significantly undermine the furnace of mainstreamstream genome is。

 including the furnace of wear genes is by only injecting one% of nodes。Also。

 Cao studied the evolution of a recommendation of furnace over time and showed that extreme values homophily or ahead ofly can be。

Determinal to recommendation fairace in the long run， even when group sizes are balanced in the data。

They also demonstrated that promoting Mophily in Hadophilic networks and Hadophilly homoic networks can improve the furnace of recommendations。

Also， homophie， the like the idea of homoophilly can be used in market design。

The laser figure shows the node level homophily of molecules in the kill un niole cells。

And similar to what's been commonly used like partition coefficients。

 synthetic accessibility score or drug likeness。The the homophily were。

No number mostly can be used as a measure of diversity to compare between the generated distribution。

Where is the real trend distribution just to give a measure on。On the generated molecule diversity。

Okay， I'll headed it back to Seattle。

![](img/612e339adc11f63d4a03e36f622b3f2f_63.png)

Thank you， Will。嗯。There is a question from Vicky。So。诶。He or she asked that。诶。

Are the metrics measuring how well a G N may work or the。Ha to fill of the graph。

A GM may not work as well for a number of other reasons of our has。In the old days。

 those metrics are only a measure of home free anropphyological graph。

 but since we introduce this kind of concept in graphra network and we try to identify the good and bad graphs with this metric。

So this metric gradually become the metric to recognize the performance of G on those graphs。

Or more specifically， we would like to use this metric。To measure how well。

The aggregation step will provide to the neural network。So nowadays。

 although people still use the word homophy or heterophily。

 I would like you to use the new word it called performance metric。

So since homophiia and heterophily is not good enough it's already proved that it's not good enough to identify the good and bad bad graphs right so nowadays there are just some baselines to measure the performance of GNs on some kind of graph。

Yeah。There do have some other reasons apart from heteroophilia that can influence the G performance。

 for example， over squing worse bluein and others。So that is why when we try to find out those bads。

 we need to compare the graph aware model and its corresponding graph as model。For example。

 when you compare a 10 layer GCN with a two layer MLLP and you found that the 10 layer GCN perform bad and you claim that the graph is bad。

 that is unfair because the 10 layer GCN might suffer from oversmthing instead of the bad graph structure。

Therefore， you should compare the two layer GN with two layer MlP。And to see if。

The graph is good bad。So at least you have two。呃。Just a。

Try to remove other other effect as much as you can so that you can identify。

Whether the massive passing step is good or not for the graph number。Yeah。



![](img/612e339adc11f63d4a03e36f622b3f2f_65.png)

Thank you for the answer and are there any questions from the audience？Okay。

 so there is a question from Slack。Okay， let me check you can also check the chat box in Zoom。

Totorial discussion， right。Com。Patrogene啊。A heterogeneous metric， right？AndIntuition， so。Oh。

Let me share model。

![](img/612e339adc11f63d4a03e36f622b3f2f_67.png)

Yeah， here is the slide question。啊。Wonderful session， thank you。

 any thoughts on next step for defining metrics？Hetgeneous heterophiligraph。

 also any intuition for when we should consider these techniques。Always supply homophilimetric。

 investative G and under performanceformance。诶。So for the heterogeneous heteroophilic matrix。

 I think this metric are mainly useful for the metapath based heterogeneous heteroophilic matrix and since different metapaths play different roles in the whole heterogeneous pipeline。

Because you have， you have several different like sub channels for the heterogeneous graph。

 so we need to compound the。The metric value from a different metapath based subgraph to consider the whole effect on a single heterogeneous G。

 So how can we combine it and how does this different subgraph impact each other。

 I think that is the the means step to define the metric on heterogeneous G， there are some。啊。

Some true， some。Experiments or some initial definitions of them， for example。

 you use the maximize homophimetric over all these subgraphs or you use the average value of all the metrics of the subgraphs。

 but I would not consider them as conclusive。So they are still a long way to go at least you need to find out that there do exist a relation between your defined metric and the performance heterogeneous G。

So if you can find such kind of relation， I will say that okay， you'll find a good metric。嗯。And。

When we should consider these techniques。🤧呃。Actually。So。Back to like two years ago。

 I think the metrics should be considered before you train any graph model。

Especially in the large scale model， for example， if you need like one week of training of the model。

And you don't know whether we should use the graph structure or not。

 you need to first test if the graph structure is good or not before you training because you don't want to waste any time。

So I first study this because my supervisor told me。

A student of her used Graph network in reinforcement learning and they found that it performed worse than a single MLP。

 so what happens to the network？嗯。And back to like the early 2020。

I don't know I really don't know the reason， so in the old days。

 I thought thatGN is universally effective for any tasks。But。After the。

The juices work in 2020 New paper， I just recognize that there do exist some kind of graphs that provide bad graft structures。

So therefore we should use this metric to first test your graph before you really train your graph model。

 especially if it is pretty small， for example if you train like three 30 seconds maybe you don't care about the training costs right but if it is pretty large。

 you need to spend like one week to train on it you don't want to waste your computational resource so it's better to like。

First computer momentum。啊。Are graph transformer less the？The skepticical to issues。That arise from。

Do you means。Okay， do you mean graph transformer？Might。Be better to deal with。

The problem fromic grass。So what do you mean the less susceptible？To the issues。

Do means potentially better on。How complete for us。IfYeah， if this is your question。啊。I would say no。

Because from。As I show in the slides， Millereller and others。

 they conduct very comprehensively exact experiments they show in that had to believe the graph transformer still has long。

It still has a long way to go for solving hyper problem。



![](img/612e339adc11f63d4a03e36f622b3f2f_69.png)

Because they connect。They connect each pair of edges on the graph， however。

 this kind of fully connected graph is not always good。And。

The graph embedding and edge embedding is not good enough to provide the information。

To the self attention， to distinguish the hydrophilic edge and homoophilic edge。So。Therefore。

Graph transformer can learn a good enough graph。For massive presence， I think。So nowadays。

I think the specformer by Bo and others is quite impressive。

 that is the first transformer like model that can form pretty good on hydro graph and I remember from their ablation study they're defined new defined graph node encoding。

Is the most effective component to improve the performance。Therefore。

 to boost the web transformer on head of the photographgraph we can maybe study。

From the domain I perspective。Yeah， any。questions from the audience？Okay， so。So。

I think that's all for our section for our section on Tra。

Philip Gaf learning and thank you to all our speakers for sharing a comprehensive review on Heath Philip G learning and also thank thanks all the audience for your engagement so we hope you have enjoyed this section and enjoyed the rest of the love conference。

Yeah。Thank you thank you so much and as a reminder we have a tutorial feedback form which I will put in the zoom chat right now we absolutely appreciate your feedback so please share as much as you are willing to share and with that we will this brings us to the end of today's law conference we'll see you tomorrow for another day of v we'll begin with another exciting tutorial this one on geometric geometric generative models so please join us on zoom or YouTube live stream for another lively discussion have a great rest of your day。

😊。

![](img/612e339adc11f63d4a03e36f622b3f2f_71.png)