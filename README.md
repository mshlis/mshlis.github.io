## Mike's ML Blog
### Here is a blog for some cool projects / research ideas that I can do in my spare time

#### A little about me and this blogs motivation:
I graduated from UMass Amherst with a bachelors in Computer Engineering and Applied Mathematics where I also completed REUs in a span of fields from logic debugging and eigensolvers all the way to analysis on hyperspectral imageing. Currently, I work as a Machine Learning Researcher at Systems and Technology Research, a medium-sized defense contractor. In my free time I'm usually working on some project to help me get a hands on perspective of the pros/cons of whatever model I'm replicating/extending. Because of this, a friend recently mentioned I should start a Github blog and here I am :). I also sprinkle in some research focused ideas as well. The goal is to hopefully push a project at least once every one or two weeks. I started this on 10/07/2019, so if you see me slacking feel free to give me hell :P.

Note that I will be seperating repos/pages that are more random/fun-only and more research focused into the tags **(Rand)** and **(Research)** respectively.

<p align="center">
  <img src="./images/im_shaggy.png">
</p>  
Here is an image of "shaggy" old me :)

[Project 1]({% post_url 2019-10-06-ANNagram %})
  
### Project 2 (Research): [Intermediate Loss Sampling](https://mshlis.github.io/ILSampling)  
**Keywords:** Differentiable Sampling  
For this project I introduce a new sampling technique that does not impact the forward pass but defines the problem as a multistep optimization one. Doing this allows for efficient calculation of gradients that utilize directly the loss' gradient. This work aims to solve a similar problem as Gumbel Softmax. Different from that approach this does not have the same draw-integrity/gradient smoothness tradeoff. I show this with a toy example on a problem where soft-sampling would actually be advantagous. Intermediate Loss Sampling performs just as well if not better. The algorithms construction is as so:  

<p align="center">
  <img src="./images/ILS.png" width="650px" height="250px">
</p> 
  
I hope to add more test cases soon. Other positives of this method include that it can extend to non-categorical random variables assuming you can find a loss function that is closed-form differentiable with respect to the parameters you are learning.     
  
