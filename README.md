
<!-- README.md is generated from README.Rmd. Please edit that file -->

# chandwich

[![AppVeyor Build
Status](https://ci.appveyor.com/api/projects/status/github/paulnorthrop/chandwich?branch=master&svg=true)](https://ci.appveyor.com/project/paulnorthrop/chandwich)
[![Coverage
Status](https://codecov.io/github/paulnorthrop/chandwich/coverage.svg?branch=master)](https://codecov.io/github/paulnorthrop/chandwich?branch=master)
[![CRAN_Status_Badge](https://www.r-pkg.org/badges/version/chandwich)](https://cran.r-project.org/package=chandwich)
[![Downloads
(monthly)](https://cranlogs.r-pkg.org/badges/chandwich?color=brightgreen)](https://cran.r-project.org/package=chandwich)
[![Downloads
(total)](https://cranlogs.r-pkg.org/badges/grand-total/chandwich?color=brightgreen)](https://cran.r-project.org/package=chandwich)

## Chandler-Bate Sandwich Loglikelihood Adjustment

### What does chandwich do?

The `chandwich` package performs adjustments of an independence
loglikelihood using a robust sandwich estimator of the parameter
covariance matrix, based on the methodology in [Chandler and Bate
(2007)](https://doi.org/10.1093/biomet/asm015). This can be used for
cluster correlated data when interest lies in the parameters of the
marginal distributions or for performing inferences that are robust to
certain types of model misspecification. Functions for profiling the
adjusted loglikelihoods are also provided, as are functions for
calculating and plotting confidence intervals, for single model
parameters, and confidence regions, for pairs of model parameters.
Nested models can be compared using an adjusted likelihood ratio test.

### A simple example

The main function in the chandwich package is `adjust_loglik`. It finds
the maximum likelihood estimate (MLE) of model parameters based on an
independence loglikelihood in which cluster dependence in the data is
ignored. The independence loglikelihood is adjusted in a way that
ensures that the Hessian of the adjusted loglikelihood coincides with a
robust sandwich estimate of the parameter covariance at the MLE. Three
adjustments are available: one in which the independence loglikelihood
itself is scaled (vertical scaling) and two others where the scaling is
in the parameter vector (horizontal scaling).

The `rats` data contain information about an experiment in which, for
each of 71 groups of rats, the total number of rats in the group and the
numbers of rats who develop a tumor is recorded. We model these data
using a binomial distribution, treating each group of rats as a separate
cluster. The argument `binom_loglik` to `adjust_loglik` is a function
that returns a vector of the loglikelihood contributions from each group
of rats. In one-dimensional examples like this the two adjustments using
horizontal scaling are identical, but this will not generally hold in
more than one dimension.

``` r
binom_loglik <- function(prob, data) {
  if (prob < 0 || prob > 1) {
    return(-Inf)
  }
  return(dbinom(data[, "y"], data[, "n"], prob, log = TRUE))
}
rat_res <- adjust_loglik(loglik = binom_loglik, data = rats)
plot(rat_res, type = 1:4, legend_pos = "bottom", lwd = 2, col = 1:4)
```

### Installation

To get the current released version from CRAN:

``` r
install.packages("chandwich")
```

### Vignette

See `vignette("chandwich-vignette", package = "chandwich")` for an
overview of the package.
