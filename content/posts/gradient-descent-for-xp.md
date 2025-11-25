+++
date = '2023-05-09T20:05:10+05:30'
title = 'Gradient Descent for XP Practitioners'
+++
## What is Gradient Descent?

Gradient descent is an optimisation algorithm used in machine learning to deduce the optimal weights / parameters that incurs minimum cost for the model being trained. 

A machine learning model needs to be trained on a dataset before it can start predicting. We can call training as done, if we have figured out the optimal values for its parameters. We can say the parameter values are optimal when the cost (difference between prediction and actual) is minimum. Gradient descent helps us find the optimal parameters for a model, given the training data and cost function.

## How does it work?

It is suggested to start with a random value for parameters and gradually update them till minimum cost is achieved. 

The rate at which the parameters are updated is called **learning rate**. It is important that the learning rate shouldn't be too large or small. If it is too large, the model takes bigger steps and may not converge towards the global minimum. On the other hand, if the learning rate is too small, it may take a long time to arrive at the global minimum. 

The cost function acts as the **feedback mechanism** to guide the gradient descent towards global minimum. It is not expected to start at a perfect value for the parameters. We rely on the feedback mechanism and the iterative updation of parameters to guide us towards the optimal value for the parameters, in baby steps. Each **iteration** in the gradient descent is just the most confident step we could take in the right direction.

{{< figure src="/images/gradient-descent-xp.jpeg" title="Gradient Descent" >}}

## So, how does it relate to XP?

We can draw parallels to the Gradient descent from XP practices and principles. XP states that striving for perfection is the enemy of progress. You don't need a perfect place to start. Rather, it is important to start somewhere that is logical to the team and equip ourselves with the right feedback mechanisms so that we can confidently move towards the expected state. 

When we say move, it implies taking baby steps, rather than huge leaps. Taking baby steps can help us the safety net to fail and learn, course correct ourselves when things go wrong.

In summary, Gradient descent and XP are similar at a fundamental level, even though they have completely different applications.

- Continuous Improvement
- Relying on Feedback mechanism
- Importance of Iterations
