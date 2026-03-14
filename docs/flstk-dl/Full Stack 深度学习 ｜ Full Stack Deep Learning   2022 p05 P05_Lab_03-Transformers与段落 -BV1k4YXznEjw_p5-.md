# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p05 P05_Lab_03-Transformers与段落 -BV1k4YXznEjw_p5-

![](img/7d012e2354038f7b6e84b68eccecdb97_0.png)

Hey folks， welcome back to the FullSt deep Learning 2022 edition Labs， we're on Lab 3。

 and in this lab we're going to introduce the transformformer architecture and the ResNe transformformer model that we'll be using in our text recognition system and we're going to apply it to paragraphs of handwritten text。

So as always， we're starting from the Github repo for the labs here。

 If you're running the labs on coabab， just scroll down to the bottom here and click this opening coabab link。

 but I also wanted to show what this looks like if you're running the labs on your own machine I've got a shell here on my Linux machine that's got a GPU and I've activated the Conda environment for the course FSDL text Rer 2022 and make sure that you have gone through the local setup instructions so that you're ready to go and then let's run our Jupiter server if you have a machine with an attached screen。

 it should open up automatically otherwise again see the setting up local development instructions for how to get Jupyter running So from the root directory of the repository navigate to lab3 notebooks and the transformers notebook。

 the lab 3 notebook that's new this time and make sure to run that first setup cell and to in general。

 run the cells from top to bottom as you're reading through the notebook。



![](img/7d012e2354038f7b6e84b68eccecdb97_2.png)

![](img/7d012e2354038f7b6e84b68eccecdb97_3.png)

Lb covers some of the core ideas of the transformer architecture for sequence modeling。

 including why transformers have largely replaced recurrent neural networks for sequence modeling over the past couple of years and as with the other overview videos。

 I'm going to focus on the components of this lab that touch on the FSDL text Rer codebase and especially the components that we'll be using in future labs。

So one of the most important pieces that's introduced in this lab is a new model。

 the Resnet transformer model He uses a small Resnet to encode our input image and then a transformer to decode that image into a sequence So let's take a look at the forward method for that resnet transformer So this is what gets called if you just call the model directly So if we look in the forward method it takes images as input and returns a sequence of class labels that we can interpret as a sequence of characters in the input image focusing in this code on the most important bits。

 we encode the inputs here that's done with the resnet so you can check out the encode method if you want to see the details of how that works and then we start a loop we loop over the length of the sequence and we take the current outputs and feed them in alongside those encoded inputs into our transformer decoder here in this self do decocode call and the rest of this is just setting。

upp so that we append the outputs as we're going and then we stop once all of the sequences in the batch have reached the end。

 This is the way that our transformer model is going run during inference。

 But one of the most important things about transformers is that they run differently during inference and during training。

 we can see that difference， if we look at the transformer lit model。

 So this is the lightning module that wraps around our basic pytorch module and allows us to train it。

 So if we look at the training step method And remember this training step method is what getss called in pytorch lightning during each step of training。

 we scroll down to the actual source code for the training step。

 we can see that takes that batch of both input images and ground truth labels and passes both of those things to this method teacher forward So that's not the normal forward method。

 And if we look at that teacher forward method and see what's different about it。

 the most important difference is this teacher forward method has both inputs and。😊。

Ground truth labels as part of its signature。 So this is obviously something we can't do during inference when we don't have ground truth labels。

 but when we do have them， it's a much simpler process to run our transformer。

 we just encode the inputs and then decode them using the ground truth labels instead of the model's outputs in that decoding step。

 This is the sort of secret sauce that makes the transformer architecture a lot more scalable。

 or at least one of the most important components of what makes the transformer architecture really scalable。

 this decocode call here because it's all happening at once and not rolled out into a for loop is really amenable to parallelization。

 So that's parallelization across the length of a sequence。

 not just parallelization across a batch or parallelization of model computations。

 So there's lots more information about the transformer architecture and intuitions about how it works and its relation to re currentrent architectures in this lab。

 I want to focus on how we use this architecture to。😊，To train our text recognition system。

 The main reason we're using this architecture here is because we want to have variable length outputs for fixed size input。

 So we want to decouple how big input is and how big output is。

 And the multilayer perceptron s fully connected architectures we've looked at and the convolutional architectures we've looked at aren't able to do that。

 So you see why we need that， let's take a look at some of our data。

 So this cell here draws random elements from a batch and shows it to us。

 So looking at the text there。 Some thought the result would be all sorts of horrible illnesses brought on by the confined atmosphere。

 the shareholders who travel by it will be so heartily sick， etc ce。

 We have this paragraph here in this image。 And if we run this cell again and get a new input。

 We can see it's the same size image。 But the length of the paragraph is different。

 Let's run it again。The lengths of these paragraphs is changing each time that we execute this cell。

 even though the size of the input image is the same。

 And so our transformer decoder can produce variable length sequences from that encoded input。

 We've now got all the pieces that we need to train a model to take in images of multiple lines of text and output the content of that image。

 This model is quite a bit bigger than the previous models that we've been working with。

 we're using a resnet a lot bigger than the previous convolutional networks we've been using and now we've tacked on this transformer。

 We're definitely going need GPU acceleration from this point on。

 and we're going to need some tricks to make sure that we can do our training and our experimentation quickly enough。

 So this is the cell that runs training here and has got some flags past to our run experiment script to try and make that training quicker。

 So I'm going to start it running and then start describing some of these flags here。've got。😊。

A data class in a model class， theyre new data classes and model classes， the I am paragraphs。

 dataset set of paragraphs of handwritten text and this resnet transformer model class。

 we also use the loss flag to say which lightning model we want to use which training wrapper we want to use we want to use the transformer one we're using GPU acceleration as we have in the past but now we're doing a couple of different things to make this loop run faster first we're reducing the batch size if you run into out of memory errors you might need to reduce the batch size further to run on your machine。

 if you hit a batch size of one and you still can't run it on your machine。

 then your GPU is not big enough to run this model。

 One trick that we use to try and be able to fit the model on smaller GPUus is this precision flag which sets the size of the floating point numbers。

 the precision of the floating point numbers that we're using typical in Python is 64 bit floating point numbers common in deep learning for years to use single。

Precision or 32 B floating point numbers that's built into Pytorch from the very beginning with Pytorch lighting。

 it's really easy to reduce the precision and size of these floating point numbers even further down to 16 bit。

 but you'll notice that first line doesn't say 16 bit precision but 16 bit automatic mixed precision。

 So we do sometimes need higher precision single precision floating points rather than half precision floating points and luckily there's nice tools built into pytorch lightingning and pytorch now for changing that just with a little flag rather than having to write code And then the other thing that we do is we limit the number of training and test and validation batches that we draw。

 So instead of drawing all batches from the training set we only draw 10 we only test on one batch we only validate on two batches。

 So this is to allow us to kind of play around with this model play around with some of these flags and get used to it before we launch these full long training jobs in future labs。

 So as you can see we've hit the validation step that actually takes a pretty long time even though it's just two。

😊，Examples and you can see the testing data load has started running during validation and testing。

 we run the model the same way we would during inference in production。

 We don't use the teacher forward method。 We use the regular forward method。

 So validation in testing run more slowly。 Once that's done。

 we get out some metrics that tell us how our model is doing a loss and a character error rate。

 the character error rate is a kind of edit distance。

 how many characters as a fraction of the number of characters in the ground true sequence。

 how many characters we have to remove and add back in to get the correct value we want to character error rate down in the low 5 to 15% and we're at almost 200%。

 So a lot more training needed。😊，The exercise in section don't direct you to actually try and train this model that's going to take some pretty serious time。

 Instead， we're gonna start learning things about training models with pytorch lightning and using some of these pieces here。

 Try out some of these training tricks built into the pytorch lightning trainer via command line flags and then also trying out this really important trick or workflow in Ml engineering overfitting a single batch So getting the model to predict perfectly the outputs for just one batch。

 So we'll talk a lot more about why we do overfitting of a single batch and how it fits into your overall Ml engineering workflows and your model development practice。

 But to start， let's just try and get it working。 There's some suggestions here on how to go about bat。

 how to get the model to start fitting a single batch within just a few minutes of training and then tune it so that you can end up getting a model that fits very well has a very low character error rate and low loss for an individual batch at the end。

😊，OfAbout 1000 forward passive。 So in future labs， we'll start training models on this more realistic handwriting data sets and adding more instrumentation around our training so that it's easier to iterate。

 We can keep track of what's happening。 We can visualize what's happening。

 and then we can hand off models that we've built through this training process to our production deployment process。

 So we'll see all of that in future labs。😊。

![](img/7d012e2354038f7b6e84b68eccecdb97_5.png)