---
author: Michael Shliselberg
date: 2019-10-14
excerpt_separator: <!--more-->
title: "Intermediate Loss Sampling"
---

I introduce a new sampling technique that does not impact the forward pass but defines the problem as a multistep optimization one. Doing this allows for efficient calculation of gradients that utilize directly the loss' gradient. This work aims to solve a similar problem as Gumbel Softmax. Different from that approach this does not have the same draw-integrity/gradient smoothness tradeoff. 

<!--more-->
  
<p align="center">
  <img src="/images/ILS/ILS.png" width="650px" height="250px">
  <br><b>algorithm</b>
</p> 
  
I show the efficacy of the approach with a toy example where soft-sampling would actually be advantagous. Intermediate Loss Sampling actuaally performs just as well if not better. I hope to add more test cases soon. Other positives of this method include that it can extend to non-categorical random variables assuming you can find a loss function that is closed-form differentiable with respect to the parameters you are learning.  

Click [here](https://github.com/mshlis/ILSampling) to see the Repo