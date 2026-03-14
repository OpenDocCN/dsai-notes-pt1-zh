# Full Stack 深度学习 ｜ Full Stack Deep Learning   2022 p03 P03_Lab_01-PyTorch中的神经网络 -BV1k4YXznEjw_p3-

![](img/393eadabae439726e550de7337881748_0.png)

Hey， friends， welcomee to the first official lab of the Fullt Deep learning 2022 edition。

 I am Charles， and I helped revamp the labs for this year and I'm really excited to be sharing it with you。

 So let's dive in。 This is the repository for the labs this year on Gitthub。

 This is where all the information for the labs lives。 And so you can see there's folders for labs 1。

2 and 3 here already。 we're going to keep adding more lab folders as we go through the labs。

 we're gonna iteratively build a deep learning code base for training are text recognition system。

 If we take a look at what that looks like in lab1。 it's pretty simple。

 there's just this one library text recognizer and the text recognizer has basically two pieces for handling data and for handling models。

 If we look at the directory for lab 2。 We'll see now we've got an extra library for training。

 And we still have that text recognizer。😊，Library， but it's gotten more complicated。

 we've added more pieces。 We've added lightning models， for example。

 if you're following along with local development， then what you'll do is clone this repository and every week as we release new labs you can clone it again if you're watching this after the course is ended you'll just clone it once and you'll have all the labs and to do a lab you'll go into each lab folder look at the notebooks and open this notebook in Jupiter from the command line So for more details on how to get set up with local development you can check out the video on getting local development set up doing that requires that you have a Linux machine with a GPU available locally or set one up in the cloud the easiest way to get started is just use Google coab So jumping back to the main page to the readme and scrolling down we can find this table which has all the labs in it and then these badges that you can click that will open the notebooks up in coab So I'm gonna go ahead and click this one for that first lab。



![](img/393eadabae439726e550de7337881748_2.png)

Deep neural networks and pytorch。 So in these videos。

 I'm not gonna go line by line through these labs。 there's lots of text and content in there that explains everything that's going on in them。

 So what I'm going to do in these videos is give you an overview of how to use each lab and hit on some of the highlights to start off。

 we got to get our bearings here in these notebooks。

 So the first thing that I wanted to walk through was this setup cell here。

 this will be the thing you got to run at the beginning of each lab。

 So it's go ahead and run this on coabab it clones the repository it sets up the environment。

 if you're running things locally you don't need to clone。

 you don't need to set up the environment just gets us in the right directory so that we're ready to go once it's done it prints out the directory that we're in lab ones directory and the contents there So it's got that text recognizer library。

 So from there we can just keep going and go through the lab if you want But heads up that these notebooks are not static。

 you can edit them。😊，And change them as you need so for example。

 I might be curious what's going on inside of that folder so I can add an additional cell and in that cell。

 write my own python code， write my own shell commands。

 for example I can issue a shell command find and look at just what's inside that text recognizer directory so it shows me the contents of that directory the models folder the data folder and all those python files I can also run python code so I could import textrer data u as u and then one thing will do a lot in the labs is use this double question mark to examine an object let's run that and this will pull up inside the notebook the source code and the documentation for some python objects so you can also print objects if you want it's maybe more familiar or display objects but this is a nice way for taking a look at the actual code here we can see that this utilities library contains this base data set class。

😊，The rest of this lab， what we're going to do is we're going to work up to understanding what that base dataset class is doing and what the components of the models part of the text recognizer library are doing and we're going do that while learning about the ptorrch library as a whole So the way that this lab notebook works is that we first build the full training of neural network with just the very fundamental parts of ptorrch So torch do tensors and math operations on them and then we iteratively replace bits of that with higher level abstractions inside torch and then eventually from the text recognizer library and we eventually end up with a much cleaner neater way for fitting neural networks and for trying out different datas。

 different models and then in following labs will continue to elaborate on top of that So just two last things that are want to cover at a meta level about these labs and notebooks the first one is that you're going to want to run these notebooks linearly。

😊，Top to bottom。 if I'm scrolling along here and reading and not executing the cells and then I come across one that's interesting。

 and let's say I want to run this one。 and I run it。 I'll get an error says Xtrain is not defined。

 And the reason why is because it's expected that you've run the cells up to this point when you get to this cell。

 So Xtrain is defined up here when we're building our data So in this happens what you can do is run all the cells before the one that you're currently running again the way to do that in coab is runtime run before there should be a way to do that in other types of notebooks if we then execute this cell。

 we'll see that it runs all the variables in it been defined all the libraries have been imported and things have been set up if you get into a situation where that doesn't help then what you should do is start the notebook over so thats in coab run time restart run time So this will get you back to square one And then the last thing that I wanted to touch on was the exercises at the end of the lab。

😊，Notbooks these exercises are here in case you want to dive deeper on this particular component of the stack。

 They're not mandatory in any way。 They're there for your learning benefit。

 Maybe you don't feel like learning more about ptorch this time around。 That's fine。

 Maybe later you're gonna care to learn more about AWS lambda and the exercises are marked with these little stars。

 So the stars are meant to indicate how difficult really how much effort it's going take to complete an exercise。

 So the exercise just has one star， you're probably just changing around arguments for a function that's already implemented for you and seeing what they do or taking a component of a library that we've already talked about and using it in a slightly different way。

 Other exercises are marked with up to three stars3 star exercise is going require you to read the documentation of one of the libraries that we're using and extend what we did。

 So add new pieces I new functionality。 And then you'll also find some two star exercises that are somewhere in between That's all I have to say about this lab。

 So go ahead， dive right in。😊，Learning about Pytorrch and Torch。nn。

 and I will see you in the next lab on Pytorrch Lightning and convolutional networks。



![](img/393eadabae439726e550de7337881748_4.png)