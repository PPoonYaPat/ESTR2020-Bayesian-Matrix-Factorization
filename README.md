# ESTR2020 Project: Bayesian Matrix Factorization

## Overview

This project implements Bayesian Matrix Factorization (BMF) using a Metropolis-Hastings algorithm for recommendation systems. It uses the MovieLens dataset to predict user ratings.

## Code

The implementation is in the `project_estr2020.ipynb` Jupyter Notebook. For each function, you can refer to the description in that function's part. It provides the detailed explanation of that function, together with the input and the output format of the function.

## Dataset

* MovieLens Latest Small dataset (100,836 ratings from 610 users across 9,742 movies)
* The notebook expects the ratings data to be in `ml-latest-small/ratings.csv`
* Ratings range from 1-5 stars, normalized to [0,1] for processing
* Data was split 80/20 for training/testing

## Key Parameters

* `n_factors`: Latent dimension (K=8 in final implementation)
* `observation_noise`: Noise variance (σ²=0.01)
* `proposal_var_u`, `proposal_var_v`: Proposal variances for user and item factors (both set to 0.005)
* `num_iteration`: Number of MCMC iterations (2,000 total)
* `burn_in`: Burn-in period (first 400 samples discarded)
* `thinning`: Thinning interval (every 5th sample retained)

## Initialization

The model uses SVD on the observed rating matrix (with missing values filled with global mean rating) to provide a better starting state than random initialization.

## Results

The implementation showed consistent improvement in prediction quality as the number of MCMC iterations increased:

### Prediction Accuracy

<p align="center">
    <img src="output/Percentage%20of%20Correct%20Prediction.png" alt="Percentage of Correct Predictions" width=450px>
</p>

After the burn-in period when the Markov Chain reached stationary distribution, prediction correctness reached approximately 60% and continued to increase slightly over iterations.

### RMSE Performance

<p align="center">
    <img src="output/Root%20Mean%20Square%20Error.png" alt="Root Mean Square Error" width=450px>
</p>

The Root Mean Square Error decreased steadily over iterations, indicating improved prediction quality as more samples were collected.

### Convergence

<p align="center">
    <img src="output/Trace%20of%20Log%20Posterior.png" alt="Trace of Log Posterior" width=450px>
</p>

The log posterior trace shows rapid increase during the initial iterations (burn-in period) and then stabilizes after around 400-500 iterations, indicating successful convergence of the Markov chain. There is a slight decrease in the log posterior at around 200-400 iterations as a result of balancing the prior and likelihood of the posterior distribution.