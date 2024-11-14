# Computational Statistics Project: Metropolis-Hastings Algorithm

## Overview
This project explores the implementation of the Metropolis-Hastings (MH) algorithm, a Markov Chain Monte Carlo (MCMC) method used to sample from complex probability distributions. The algorithm is applied to multiple distributions to evaluate its performance and limitations, particularly in generating accurate samples for different target distributions.

## Introduction: MCMC and Metropolis-Hastings
The Metropolis-Hastings algorithm is a Markov Chain Monte Carlo (MCMC) method for sampling from a probability distribution. By constructing a Markov Chain with the desired distribution as its equilibrium distribution, MCMC methods are widely used in statistical computing, especially for approximating high-dimensional integrals.

## The Metropolis-Hastings Algorithm

The Metropolis-Hastings algorithm samples from a distribution \( P(x) \) by proposing candidate samples and accepting or rejecting them based on their probability relative to the current sample. This algorithm is particularly useful when we have a function \( f(x) \) proportional to the target density \( P(x) \) but can't directly sample from \( P(x) \).

### Pseudocode

#### Variable List
- **\( P(x) \)**: Target density (up to a constant).
- **\( x_t \)**: Current sample.
- **\( x' \)**: Candidate sample.
- **\( \alpha \)**: Acceptance ratio, determining the likelihood of accepting \( x' \).
- **\( g(x'|x_t) \)**: Proposal density (e.g., Gaussian).

#### Algorithm Steps

1. Set initial guess, \( x_0 \)
2. For each \( t \):
   a. Draw \( x' \) from \( g(x'|x_t) \)
   b. Compute acceptance ratio \( \alpha = \frac{f(x')}{f(x_t)} \cdot \frac{g(x_t | x')}{g(x' | x_t)} \)
   c. Draw \( u \sim U[0, 1] \)
   d. If \( u \leq \alpha \), accept \( x' \) as the next sample; otherwise, retain \( x_t \)


If the proposal density \( g \) is symmetric, the ratio \( \frac{g(x_t | x')}{g(x' | x_t)} \) cancels out.

### Drawbacks of the MH Algorithm
1. **Correlation**: Samples are correlated, as each sample is based on the previous one.
2. **Initial Point Dependence**: A poor initial point may delay reaching high-density regions of the target distribution.

## Experimental Results

The Metropolis-Hastings algorithm was implemented with two types of proposal densities:
1. **Gaussian**: \( N(x, 1) \)


3. **Uniform**: \( U[x-1, x+1] \)

### Experiment 1: 2D Gaussian Target Distribution
A 2D Gaussian distribution was used as the target density. Results show convergence toward the target distribution with as few as 1000 samples.

 ![image](https://github.com/user-attachments/assets/3513112f-55e1-4a93-90f9-84e07c9c873c)
![image](https://github.com/user-attachments/assets/62a48ad7-271e-4cb9-8d2a-b7bc0b26f682)


### Experiment 2: 1D Sinusoidal-Exponential Distribution
For a more complex distribution proportional to \( f(x) = \exp[\sin^2(x) + \sin^2(3x) - 0.5x^2] \), the algorithm was tested with both Gaussian and Uniform proposal densities. No significant differences were observed in convergence speed or sample quality.
![image](https://github.com/user-attachments/assets/e2c33c90-9f08-4853-a910-c019b5284673)
![image](https://github.com/user-attachments/assets/b704b212-d830-44b3-92c5-e6ea5b1b8a96)
![image](https://github.com/user-attachments/assets/af3a1ab2-dc0a-4929-8837-ee5ed55f0b15)



### Performance Insights
With Gaussian proposal density \( \sigma = 1 \) and uniform range \( [x-1, x+1] \), the MH algorithm performed similarly for both proposal densities.

## Observed Shortcomings
How to make the Metropolis-Hastings algorithm fail? Turns out that giving it a distribution with
a more complicated shape and a more limiting transition function can result in the algorithm not
returning an accurate sample. Below is an example of same sinusoidal distribution with transition
density being uniform over [x-0.2, x+0.2] - even with 100 000 draws, the algorithm completely misses
the right part of the graph!
![Uploading image.png…]()

## Conclusion
The Metropolis-Hastings algorithm was successfully implemented and tested on both simple and complex distributions. Although it converges well for a Gaussian target distribution, limitations in exploration were observed with overly restrictive proposal densities. Further tuning of proposal parameters could improve performance for more complex distributions.

## References
- **Class Notes**
- Bishop, C. "Pattern Recognition and Machine Learning", Section 11.2
