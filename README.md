## Mike's ML Blog
### Here is a blog for some cool projects / research ideas that I can do in my spare time

#### A little about me and this blogs motivation:
I graduated from UMass Amherst with a bachelors in Computer Engineering and Applied Mathematics where I also completed REUs in a span of fields from logic debugging and eigensolvers all the way to analysis on hyperspectral imageing. Currently, I work as a Machine Learning Researcher at Systems and Technology Research, a medium-sized defense contractor. In my free time I'm usually working on some project to help me get a hands on perspective of the pros/cons of whatever model I'm replicating/extending. Because of this, a friend recently mentioned I should start a Github blog and here I am :). I also sprinkle in some research focused ideas as well. The goal is to hopefully push a project at least once every one or two weeks. I started this on 10/07/2019, so if you see me slacking feel free to give me hell :P.

Note that I will be seperating repos/pages that are more random/fun-only and more research focused into the tags **(Rand)** and **(Research)** respectively.

<p align="center">
  <img src="./im_shaggy.png">
</p>  
Here is an image of "shaggy" old me :)

### Project 1 (Rand): [ANNagram](https://mshlis.github.io/ANNagram/)
**Keywords:** NLP, Transformer, Pointer-Networks, Anagrams

For my first project in this blog I write from scratch a pointer network with a transformer backbone. For smaller words its a silly idea, because exhaustive checking would be quick and accurate, but this grows in a combinatoric fashion. On the other hand a transformer-based encoder/decoder will grow quadratically with input length.  
`erif --> fire || sichpys --> physics || nfuctnio --> function`  

**What are pointer nets?:** Pointer Nets came out several years back, in attempts to setup a simplistic framework for varied length codomains. For a rearrangement problem like anagrams this is sensical approach given that different words have different sizes. These models extend encoder-decoder models, specifically the intial paper uses the seq2seq architecture if I remember correctly.

<p align="center">
  <img src="./pointer_net.png" width="600px" height="400px">
</p>  

**What is a transformer?:** Lately due to the rise in language models (Ill probably do a project down the line going through those too) people confuse the term transformer for just encoder portion of the original model which originlly was an encoder-decoder architecture. These models work almost solely on attention. The use forms of dot product attention, which just means each logit is an inner product of the key and query. This decision is due to its ability to be constructed as a matrix multiplication which is easily parallelizable and has efficient implemetnations. They use multiple attention heads to be able to learn various query spaces at a certain feature level. Pros: O(1) layers required for intra-layer communication (Note this only holds where the input fits within the receptive field -- this isnt always the case and there exists different tricks in extending this such as transformerXL). Cons: O(n^2) communications per attention head which can get pricey (Note there exist methodologies to use a tree like communication routine to push communication to O(n log n). This construction though comes at a cost of no temporal state. The original paper makes up for this by using a clever trick of adding frequency-varying sinsoids to the input that give the model the capabilities to learn a temporal representation. From an architectural perspective I feel this places alot of strain on the learning process, but empirically showed just as good results as learned embeddings. In most current implementations though you do see that return to learned temporal embeddings. 

<p align="center">
  <img src="./transformer.png" width="400px" height="600px">
</p>  

For this problem, were working on a set with no notion of time or order (on the encoder side) so we dont use any type of positional/temporal embedding. I chose this backbone because it thrives in informational communication along nodes. This aspect was particularly attractive from a design perspective because I can image how I would attempt looking for anagrams, and how this architecture can model that. Intuitively I would first check for combinations of letters that are commonly used together, group those and then use the vowels to glue those together until I hit a word that sounds familiar. On a single RTX2080Ti the model converges in nearly 20 minutes and produces somewhat decent results.  

**Possible extensions for later:** 
- we can design this to work with keys that can be used multiple times (example: `kntig --> knitting`
- we can add in spaces to actually learn to make phrases (example: `nrttgiarhee --> the tiger ran`)


### Project 2 (Research): [Intermediate Loss Sampling](https://mshlis.github.io/ILSampling)  
**Keywords:** Differentiable Sampling
For this project I introduce a new sampling technique that does not target its forward pass, but defines the problem as a multistep optimization one. Doing this allows for efficient calculation of gradients that utilizes directly the loss' gradient. This work solves a similar problem as Gumbel Softmax but doesnt have the same draw-integrity/gradient smoothness tradeoff. I show this with a toy example that on a problem that soft-sampling has an advantage, my sampling procedure actually performs just as well if not better. I hope to add more soon. Other positives of this method include that it can extend to non-categorical random variables as well, even ones without a reparametrization trick, assuming you can find a loss function that is closed-form differentiable with respect to the parameters you are learning.          

