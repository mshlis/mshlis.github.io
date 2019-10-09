## MSHLIS : Michael Shliselberg
### Here is a webpage for some cool projects / research ideas that I can do in my spare time

#### A little about me and this pages motivation:
I graduated from Umass Amherst with a bachelors in Computer Engineering and Applied Mathematics where I also completed REUs in a span of fields from logic debugging and eigensolvers all the way to analysis on hyperspectral imageing. Currently I work as a Machine Learning Researcher at Systems and Technology Research, a medium-sized defense contractor located in Woburn MA. In my free time, im usually working on some project to help me get a hands on perspective of the pros/cons of whatever model im replicating/extending. Because of this, a friend recently mentioned I should start a github blog and here I am :). I also sprinkle in some research focused ideas as well. The goal is to hopefully push a project atleast once every one or two weeks. I started this on 10/07/2019, so if you see me slacking feel free to give me hell :P. 

Note that I will be seperating repos/pages that are more random/fun-only and more research focused into the tags **(Rand)** and **(Research)** respectively.

<p align="center">
  <img src="./im_shaggy.png">
</p>  
Here is an image of "shaggy" old me :)

### (Rand) Project 1: [ANNagram](https://mshlis.github.io/ANNagram/)
For my first project in this blog I write from scratch a pointer network with a transformer backbone. For smaller words its a silly idea, because exhaustive checking would be quick and perfectly accurate, but this grows in a combinatoric fashion. On the other hand a transformer-based encoder/decoder will grow quadratically with input length.  
`erif --> fire || sichpys --> physics || nfuctnio --> function`
**Keywords:** NLP, Transformer, Pointer-Networks, Anagrams

### (Research) Project 2: [Intermediate Loss Sampling](https://mshlis.github.io/ILSampling)  
For this project I introduce a new sampling technique that does not target its forward pass, but defines the problem as a multistep optimization one. Doing this allows for efficient calculation of gradients that utilizes directly the loss' gradient. This work solves a similar problem as Gumbel Softmax but doesnt have the same draw-integrity/gradient smoothness tradeoff. I show this with a toy example that on a problem that soft-sampling has an advantage, my sampling procedure actually performs just as well if not better. I hope to add more soon. Other positives of this method include that it can extend to non-categorical random variables as well, even ones without a reparametrization trick, assuming you can find a loss function that is closed-form differentiable with respect to the parameters you are learning.
**Keywords:** Differentiable Sampling

