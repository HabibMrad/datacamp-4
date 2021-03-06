# Factor Analysis in R
## Jennifer Brussow

# Evaluating Your Measure with Factor Analysis
- **Psychometrics** is the study of unobservable variables.
- **Classical Test Theory**: Scores are the unweighted sum of item scores.
- **Factor Analysis**: an empirically weighted sum of item scores, where weights are determined by the items' correlations to each other.
- **Structural Equation Modeling**: Extends factor analysis to allow the relationships between latent variables to be modeled.
- There are two types of *Factor Analysis*:
  1. Exploratory.
  2. Confirmatory.
- Primarily will use the package *psych* developed by William Revelle.
- To run the *factor analysis*, we will use the function `fa()`.
```
# View the factor loadings
EFA_model$loadings
```
```
# Create a path diagram of the items' factor loadings
fa.diagram(EFA_model)
```
- You can see the scores of the model by checking the `object$scores` variable.
- When you are creating a measurement of an unobservable variable:
  1. Develop items for your measure.
  2. Collect pilot data from a representative sample.
  3. Check out what that dataset looks like.
  4. Consider whether you want to use EFA, CFA, or both.
  5. If both, split your sample into random halves.
  6. Compare the two samples to make sure they are similar.
- You can see visual representations of the error bars for a column using the function `error.gots()`.
```
# Graphical representation of error
error.bars(gcbs)
```
```
# Compare stats across groups
describeBy(gcbs_grouped, group = gcbs_grouped$group_var)
statsBy(gcbs_grouped, group = 'group_var')
```
- You can use the function `lowerCor(<data>)` to get a cleaner correlation matrix.
- You can use the function `corr.test(<data>, use = "pairwise.complete.obs")$p` to get the p-values for the correlations.
- You can use the function `corr.test(<data>, use = "pairwise.complete.obs")$ci` to get the confidence intervals for the correlations.
- **Coefficient - or Cronbach's - Alpha** is used to measure psychometric test's reliability.
- Publications prefer Alphas that are above .8.
- You can use the function `alpha( <data> )` to get *Cronbach's Alpha* along with some other statistics.
- You can use the function `splitHalf(<data>)` to get the Split Half statistics from the data.


# Multidimensional EFA
- You can use Exploratory Factor Analysis for Dimensional Reduction.
- In the absence of a theory, you can use an Empirical approach,
- You can use thee **Eigenvalues** of the correlation matrix for unique factors.
- Once you have the matrix, you can use the function `eigen(<matrix>)` to calculate the values.
- You can find the values in the object's `values` column.
- A good rule of thumb is that *eigen values* greater than one represent meaningful factors.
```
# Then use that correlation matrix to create the scree plot
scree(bfi_EFA_cor, factors = FALSE)
```
```
# Then use that correlation matrix to create the scree plot
scree(bfi_EFA_cor, factors = FALSE)
```
- A **Construct** is a attribute of interest that cannot be directly observed.
- You can view the loadings of the Factor analysis inside the `<object>$loadings` column.
- Do not interpret factor loadings until you have a theory to test!
- You can tell the function `fa()` how many factors to use using the parameter `nfactors = <n>`.
- There are two kinds of model fits to compare with:
  1. Absolute: `have intrinsic meaning and suggested cutoff values.`
  2. Relative: `have meaning when comparing models.`
- To use relative models, we need to models to compare.
```
# Run each theorized EFA on your dataset
bfi_theory <- fa(bfi_EFA, nfactors = 5)
bfi_eigen <- fa(bfi_EFA, nfactors = 6)
```
- You can access the **Bayes Information Criterion** using the column name `BIC`.


# Confirmatory Factor Analysis
- You use a Confirmatory Analysis when you want to confirm a theory that you've already developed.
- You also want to publish the CFA when presenting a new measurement.
- The function `tructure.sem( <model> )` will convert EFA into CFA for you.
- You can setup the theory for the functions using a format similar to:
```
# Set up syntax specifying which items load onto each factor

theory_syn_eq <- "
AGE: A1, A2, A3, A4, A5     #Agreeableness
CON: C1, C2, C3, C4, C5     #Conscientiousness
EXT: E1, E2, E3, E4, E5     #Extraversion
NEU: N1, N2, N3, N4, N5     #Neuroticism
OPE: O1, O2, O3, O4, O5     #Openness
"
```
- You will need to add in the variances and covariances but that can be done for you.
- You can use `theory_syn <- sem::cfa(text = theory_syn_eq, reference.indicators = FALSE)` to run this for you.
- You can use the function `sem()` to run the CFA by passing the theoretical model as well as the data.
```
#Use the sem() function to run a CFA
theory_CFA <- sem(theory_syn, data = bfi_CFA)
```
- You will still need to investigate and report the fit of a CFA as well.
- You can add other fit statistics to the results using `options(fit.indices = c("CFI", "GFI", "RMSEA", "BIC"))`


# Refining Your Measurement or Model
- Don't forget the differene between EFA and CFA.
- Make sure to pay attention to what the factor is vs the labelled name.
```
# Calculate factor scores for the EFA dataset
CFA_scores <- fscores(EFA_CFA, data = bfi_EFA)
```
- To improve model fitting, you can add more loadings.
- Be careful about doing this as it might violate the Theoretical Model.
- You can add those new loadings to the theory syntax.
- You can also remove factors from the equation by simply removing them from the syntax.


# Research:
- Tucker Lewis Index?


# Reference:
- [Model Fit Discussion](http://davidakenny.net/cm/fit.htm)
