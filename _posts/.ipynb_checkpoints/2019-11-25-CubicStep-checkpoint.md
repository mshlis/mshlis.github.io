---
author: Michael Shliselberg
date: 2019-11-25
tags: [optimization, research]
comments: true
excerpt: "Current gradient based optimization ignores 0th order information. In this post I want to motivate others to try utilizing it."
mathjax: "true"
title: "(Res) Cubic Step: We Have More Info, Lets Use It!"
---   

Hello everyone! In this blog post I introduce an optimizer called Cubic Step where I fit a cubic poynomial to the current and previous step's losses and gradients to solve for the next update. My goal with this post is not to introduce some revolutionary optimizer but rather make aware the lack of the loss function's utility in modern gradient based optimization.   

zeroth order calculations add just as much information about the loss manifold as any other sample, **so why is it ignored?** This is because most differntiable optimization techniques use gradient based climbing schemes that only care about the local curvature. 

$$
\begin{aligned}
x_{t+1} \leftarrow x_t \pm \eta \frac{\partial f}{\partial x_t} \\
\end{aligned}
$$

Other common techniques like momentum, adagrad, adam, etc. all work on either trying to approximate higher order terms or generate an adaptive learning rate ($$\eta$$ in the above) to perform smarter updates. Considering this, loss is ignored because it has no bearing on the locality of the parameters in the loss manifold. This though is only true with a single draw; accross multiple updates these observed losses can even help such an update.  

Here are some images to examplify the importance of zeroth order info (excuse my terrible drawing):
<p align="center">
  <img src="/images/CubicStep/example_curves.png">
  <br><b>different curves with the same gradients at two samples</b>
</p>  


Excusing my terrible drawings in the figure, the idea is that even with maintaining first order information, the actual losses change the desired minima. I want to provide a motivating use-case. I devise the Cubic Step optimizer in this post to help deal with oscillations over a critical point in parameter space. The idea is simple, rather than taking a gradient step, I find a cubic that fits the four points $$l(w_0), l(w_1), l'(w_0)$$ and $$l'(w_1)$$ where $$w_0,w_1$$ is the current and previous parameter while $$l(w)$$ is the loss function. I use this though only in the setting of an oscillating gradient, otherwise I take a normal ascent/descent step (for my experiments I use SGD but it could be any optimizer step). Denoting the fitted polynomial as $$p(x) = \alpha_0 + \alpha_1 x + \alpha_2 x^2 + \alpha_3 x^3$$, solving for the coefficients leads us to 4 equations and 4 unknowns. The matrix form is as so:


$$\begin{pmatrix} 1 & w_0 & w_0^2 & w_0^3 \\ 
                1 & w_1 & w_1^2 & w_1^3 \\ 
                0 & 1 & 2w_0 & 3w_0^2 \\ 
                0 & 1 & 2w_1 & 3w_1^2 \end{pmatrix}
\begin{pmatrix} \alpha_0 \\ 
                \alpha_1 \\ 
                \alpha_2 \\ 
                \alpha_3 \end{pmatrix}=
\begin{pmatrix} l(w_0) \\ 
                l(w_1) \\ 
                l'(w_0) \\ 
                l'(w_1) \end{pmatrix}$$  
                
or equivalently:

$$\begin{pmatrix} \alpha_0 \\ 
                  \alpha_1 \\ 
                  \alpha_2 \\ 
                  \alpha_3 \end{pmatrix}=
\begin{pmatrix} 1 & w_0 & w_0^2 & w_0^3 \\ 
                1 & w_1 & w_1^2 & w_1^3 \\ 
                0 & 1 & 2w_0 & 3w_0^2 \\ 
                0 & 1 & 2w_1 & 3w_1^2 \end{pmatrix}^{-1}
\begin{pmatrix} l(w_0) \\ 
                l(w_1) \\ 
                l'(w_0) \\ 
                l'(w_1) \end{pmatrix}$$  
                

Its pretty easy to see the inverse exists as long as $w_0 \neq w_1$. In my initial implementation I got lazy and used tensorflow's `tf.linalg.inv` but that lead to a stream of issues (It worked fine on CPU but on GPU It always caused the program to hang even though I checked that the eigenvalues were numerically stable with respect to floating point precision) so I did what we all had to freshman year of college $$\rightarrow$$ I did it by hand. The tedious excersize took around 5ish minutes. You get the inverse $Z$ as so...

$$
\begin{aligned}
Z[:,:] &\leftarrow I_{(4x4)} \\
Z[3,:] &\leftarrow Z[3,:] - \frac{2}{(w_0-w_1)}(Z[0,:]-Z[1,:]) + Z[2,:] \\
Z[3,:] &\leftarrow \frac{Z[3,:]}{(w_1-w_0)^2} \\
Z[2,:] &\leftarrow Z[2,:] - \frac{1}{(w_0-w_1)}(Z[0,:]-Z[1,:]) - (2w_0^2 - w_0w_1 - w_1^2)Z[3,:] \\
Z[2,:] &\leftarrow \frac{Z[2,:]}{(w_0-w_1)} \\
Z[1,:] &\leftarrow Z[1,:] - Z[0,:] - (w_1^2-w_0^2)Z[2,:] - (w_1^3-w_0^3)Z[3,:] \\
Z[1,:] &\leftarrow \frac{Z[1,:]}{(w_1-w_0)} \\
Z[0,:] &\leftarrow Z[0,:] - w_0Z[1] - w_0^2Z[2,:] - w_0^3Z[3,:] \\
\end{aligned}
$$

Now that we did our little excersize we can actually run the optimizer. Unfortunately this entire scheme makes the difficult assumption that $$l_k(w)$$ and $$l_{k+1}(w)$$ are the same functions where $$k$$ is the iteration or batch number. This can be gauranteed if the batch size covers the whole dataset but as the batch gets smaller the variance increases. Lets test the effects of this empirically. I use a 4096 image subset of the MNIST dataset rather than the whole 60K so I can have batches run up to 100% of the data. For the experimental setup I reinitialize the weights for each batch size but keep initializations the same accross optimizer types. Note that I use batchsizes ranging form 128 to 4096 or 3-100% of the dataset. Also Cubic Step is only utilized in the regions where the weights oscillate, so it should be heavily impacted by the batch gradient's variance-- to find safer regions I use 25 epochs of warmup of only SGD. 

<p align="center">
  <img src="/images/CubicStep/mnist_results.png">
  <br><b>results on a 5 layer CNN on MNIST 4096</b>
</p>  

These results were interesting and better than I expected. It seems the two methods perfomed nearly the same and for the most part batch percentage had very little effect on the efficacy of Cubic Step and it was also slightly smoother in some cases. The latter was the intended goal, but the former doesnt make much sense given how much this method should be dependant on intra-batch variance. I will atest that probably due to the smoothness of the shallow net's manifold but will require further testing to be sure.  

In conclusion the methods worked similarly but I think this result is enough to atleast give all who are reading this enough motivation to atleast keep the idea that zeroth order information is essentially free to access and could be applied in possible future work. Cubic Step only used 4 points, but in reality we can access all previous temporal samples if needed. I chose 4, because cubic was convenient to solve for critical points analytically, while post-5th order is provably impossible as a generality. Actually in a previous post, I go over Focal Gradient Loss as an intuitive competitor of Foal Loss, and that could be seen as an adaptive weighting dependant on zeroth order information too.

I hope you guys enjoyed the post and [here](https://github.com/mshlis/CubicStep) is the repo for reproduction and playing yourself with Cubic Step