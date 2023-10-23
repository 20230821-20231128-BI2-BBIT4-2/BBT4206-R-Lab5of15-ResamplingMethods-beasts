Business Intelligence Project
================
<Specify your name here>
<Specify the date when you submitted the lab>

- [Student Details](#student-details)
- [Setup Chunk](#setup-chunk)
- [Dataset loader](#dataset-loader)
- [Splitting the dataset: Pima Indians
  Diabetes](#splitting-the-dataset-pima-indians-diabetes)
  - [Splitting the dataset](#splitting-the-dataset)
  - [Train a Naive Bayes classifier using the training
    dataset](#train-a-naive-bayes-classifier-using-the-training-dataset)
    - [naiveBayes() function in the e1071
      package](#naivebayes-function-in-the-e1071-package)
    - [naiveBayes() function in the caret
      package](#naivebayes-function-in-the-caret-package)
  - [Test the trained model using the testing
    dataset](#test-the-trained-model-using-the-testing-dataset)
    - [Test the trained e1071 Naive Bayes model using the testing
      dataset](#test-the-trained-e1071-naive-bayes-model-using-the-testing-dataset)
    - [Test the trained caret Naive Bayes model using the testing
      dataset](#test-the-trained-caret-naive-bayes-model-using-the-testing-dataset)
  - [View the Results](#view-the-results)
    - [e1071 Naive Bayes model and test results using a confusion
      matrix](#e1071-naive-bayes-model-and-test-results-using-a-confusion-matrix)
    - [caret Naive Bayes model and test results using a confusion
      matrix](#caret-naive-bayes-model-and-test-results-using-a-confusion-matrix)
- [Bootstrapping: Pima Indians
  Diabetes](#bootstrapping-pima-indians-diabetes)
  - [Split the dataset](#split-the-dataset)
  - [Train a linear regression model (for
    regression)](#train-a-linear-regression-model-for-regression)
  - [Test the trained linear regression model using the testing
    dataset](#test-the-trained-linear-regression-model-using-the-testing-dataset)
  - [View the RMSE and the predicted values for the 12
    observations](#view-the-rmse-and-the-predicted-values-for-the-12-observations)
  - [Use the model to make a prediction on unseen new
    data](#use-the-model-to-make-a-prediction-on-unseen-new-data)
- [CV, Repeated CV, and LOOCV: Pima Indians Diabetes
  Database](#cv-repeated-cv-and-loocv-pima-indians-diabetes-database)
  - [Regression: Linear Model](#regression-linear-model)
    - [10-fold cross validation](#10-fold-cross-validation)
    - [Test the trained linear model using the testing
      dataset](#test-the-trained-linear-model-using-the-testing-dataset)
    - [View the RMSE and the predicted
      values](#view-the-rmse-and-the-predicted-values)
  - [Classification: LDA with k-fold Cross
    Validation](#classification-lda-with-k-fold-cross-validation)
    - [LDA classifier based on a 5-fold cross
      validation](#lda-classifier-based-on-a-5-fold-cross-validation)
    - [Test the trained LDA model using the testing
      dataset](#test-the-trained-lda-model-using-the-testing-dataset)
    - [View the summary of the model and view the confusion
      matrix](#view-the-summary-of-the-model-and-view-the-confusion-matrix)
  - [Classification: Naive Bayes with Repeated k-fold Cross
    Validation](#classification-naive-bayes-with-repeated-k-fold-cross-validation)
    - [Train an e1071::naive Bayes classifier based on the diabetes
      variable](#train-an-e1071naive-bayes-classifier-based-on-the-diabetes-variable)
    - [Test the trained naive Bayes classifier using the testing
      dataset](#test-the-trained-naive-bayes-classifier-using-the-testing-dataset)
    - [View a summary of the naive Bayes model and the confusion
      matrix](#view-a-summary-of-the-naive-bayes-model-and-the-confusion-matrix)
  - [Classification: SVM with Repeated k-fold Cross
    Validation](#classification-svm-with-repeated-k-fold-cross-validation)
    - [SVM Classifier using 5-fold cross validation with 3
      reps](#svm-classifier-using-5-fold-cross-validation-with-3-reps)
    - [Test the trained SVM model using the testing
      dataset](#test-the-trained-svm-model-using-the-testing-dataset)
    - [View a summary of the model and view the confusion
      matrix](#view-a-summary-of-the-model-and-view-the-confusion-matrix)
  - [Classification: Naive Bayes with Leave One Out Cross
    Validation](#classification-naive-bayes-with-leave-one-out-cross-validation)
    - [Train a Naive Bayes classifier based on an
      LOOCV](#train-a-naive-bayes-classifier-based-on-an-loocv)
    - [Test the trained model using the testing
      dataset](#test-the-trained-model-using-the-testing-dataset-1)
    - [View the confusion matrix](#view-the-confusion-matrix)

# Student Details

|                                              |                             |
|----------------------------------------------|-----------------------------|
| **Student ID Number**                        | 119630,135844,131038,104135 |
| **Student Name**                             | beasts                      |
| **BBIT 4.2 Group**                           | A&B&C                       |
| **BI Project Group Name/ID (if applicable)** | beasts                      |

# Setup Chunk

**Note:** the following KnitR options have been set as the global
defaults: <BR>
`knitr::opts_chunk$set(echo = TRUE, warning = FALSE, eval = TRUE, collapse = FALSE, tidy = TRUE)`.

More KnitR options are documented here
<https://bookdown.org/yihui/rmarkdown-cookbook/chunk-options.html> and
here <https://yihui.org/knitr/options/>.

# Dataset loader

``` r
if (require("mlbench")) {
  require("mlbench")
} else {
  install.packages("mlbench", dependencies = TRUE,
                   repos = "https://cloud.r-project.org")
}
```

    ## Loading required package: mlbench

``` r
data(PimaIndiansDiabetes)
```

# Splitting the dataset: Pima Indians Diabetes

## Splitting the dataset

``` r
str(PimaIndiansDiabetes)
```

    ## 'data.frame':    768 obs. of  9 variables:
    ##  $ pregnant: num  6 1 8 1 0 5 3 10 2 8 ...
    ##  $ glucose : num  148 85 183 89 137 116 78 115 197 125 ...
    ##  $ pressure: num  72 66 64 66 40 74 50 0 70 96 ...
    ##  $ triceps : num  35 29 0 23 35 0 32 0 45 0 ...
    ##  $ insulin : num  0 0 0 94 168 0 88 0 543 0 ...
    ##  $ mass    : num  33.6 26.6 23.3 28.1 43.1 25.6 31 35.3 30.5 0 ...
    ##  $ pedigree: num  0.627 0.351 0.672 0.167 2.288 ...
    ##  $ age     : num  50 31 32 21 33 30 26 29 53 54 ...
    ##  $ diabetes: Factor w/ 2 levels "neg","pos": 2 1 2 1 2 1 2 1 2 2 ...

**Description:** 40% of the original data will be used to train the
model and 60% of the original data will be used to test the model.

``` r
library(caret)
```

    ## Loading required package: ggplot2

    ## Loading required package: lattice

``` r
train_index <- createDataPartition(PimaIndiansDiabetes$diabetes,
                                   p = 0.40,
                                   list = FALSE)
PimaIndiansDiabetes_train <- PimaIndiansDiabetes[train_index, ]
PimaIndiansDiabetes_test <- PimaIndiansDiabetes[-train_index, ]
```

## Train a Naive Bayes classifier using the training dataset

### naiveBayes() function in the e1071 package

``` r
PimaIndiansDiabetes_model_nb_e1071 <-
  e1071::naiveBayes(diabetes ~ .,
                    data = PimaIndiansDiabetes_train)
```

### naiveBayes() function in the caret package

``` r
PimaIndiansDiabetes_model_nb_caret <- # nolint
  caret::train(diabetes ~ ., data =
               PimaIndiansDiabetes_train[, c("pregnant", "glucose", "pressure",
                    "triceps", "insulin","mass", "pedigree","age","diabetes")],
               method = "naive_bayes")
```

## Test the trained model using the testing dataset

### Test the trained e1071 Naive Bayes model using the testing dataset

``` r
predictions_nb_e1071 <-
  predict(PimaIndiansDiabetes_model_nb_e1071,
          PimaIndiansDiabetes_test[, c("pregnant", "glucose", "pressure",
                        "triceps", "insulin","mass","pedigree","age","diabetes")])
```

### Test the trained caret Naive Bayes model using the testing dataset

``` r
predictions_nb_caret <-
  predict(PimaIndiansDiabetes_model_nb_caret,
          PimaIndiansDiabetes_test[, c("pregnant", "glucose", "pressure",
                        "triceps", "insulin","mass","pedigree","age","diabetes")])
```

## View the Results

### e1071 Naive Bayes model and test results using a confusion matrix

``` r
print(predictions_nb_e1071)
```

    ##   [1] neg neg neg neg pos pos neg neg neg neg neg pos neg pos pos pos neg neg
    ##  [19] neg neg neg neg pos neg pos pos pos neg neg neg neg pos pos neg pos pos
    ##  [37] neg neg pos neg neg neg neg neg neg neg neg neg pos neg pos neg neg neg
    ##  [55] neg neg pos neg neg neg neg neg pos neg neg neg pos neg neg neg neg neg
    ##  [73] neg pos pos pos neg neg neg neg neg neg neg neg pos neg pos pos neg neg
    ##  [91] neg neg neg neg neg neg neg neg neg pos pos neg neg neg pos pos pos neg
    ## [109] pos pos pos neg pos neg neg neg neg neg pos neg pos neg pos neg neg pos
    ## [127] neg pos neg neg neg neg pos neg neg pos neg neg pos pos neg neg neg pos
    ## [145] pos pos neg neg neg neg neg pos neg neg pos neg neg pos neg pos neg pos
    ## [163] pos pos neg neg neg neg neg pos neg pos pos neg neg neg neg pos neg neg
    ## [181] neg neg pos neg neg neg neg neg neg neg neg neg neg pos neg pos neg neg
    ## [199] pos pos neg neg neg pos pos neg neg neg neg neg pos pos pos pos pos pos
    ## [217] pos neg neg neg pos neg pos neg pos neg neg neg neg neg pos pos neg pos
    ## [235] pos neg neg pos neg neg pos pos neg pos neg neg pos pos neg neg neg neg
    ## [253] neg pos neg pos neg neg neg pos neg neg neg neg neg neg neg neg neg neg
    ## [271] neg pos neg neg neg neg neg neg neg neg neg neg neg neg neg neg pos neg
    ## [289] pos neg neg pos neg neg neg neg neg neg neg neg neg neg neg neg pos pos
    ## [307] neg pos neg neg neg neg neg neg neg neg neg neg pos pos neg pos neg neg
    ## [325] neg pos neg neg pos neg neg neg neg neg neg neg neg neg neg pos neg neg
    ## [343] pos pos neg pos pos pos neg neg neg neg pos neg neg pos neg neg pos neg
    ## [361] pos neg pos pos neg pos neg pos neg neg pos neg neg neg neg neg neg neg
    ## [379] neg neg pos neg neg neg pos pos neg pos neg neg neg pos pos neg pos pos
    ## [397] neg neg pos neg pos pos pos pos pos neg neg neg neg neg neg neg pos pos
    ## [415] neg pos neg pos neg neg neg neg neg neg neg neg neg pos neg pos neg neg
    ## [433] pos neg neg neg neg neg neg neg neg neg neg neg neg neg neg neg pos pos
    ## [451] neg pos neg neg neg pos pos neg neg neg
    ## Levels: neg pos

``` r
caret::confusionMatrix(predictions_nb_e1071,
                       PimaIndiansDiabetes_test[, c("pregnant", "glucose",                               "pressure","triceps", "insulin","mass","pedigree","age",
                                                    "diabetes")]$diabetes)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction neg pos
    ##        neg 252  67
    ##        pos  48  93
    ##                                           
    ##                Accuracy : 0.75            
    ##                  95% CI : (0.7078, 0.7889)
    ##     No Information Rate : 0.6522          
    ##     P-Value [Acc > NIR] : 3.954e-06       
    ##                                           
    ##                   Kappa : 0.4333          
    ##                                           
    ##  Mcnemar's Test P-Value : 0.09325         
    ##                                           
    ##             Sensitivity : 0.8400          
    ##             Specificity : 0.5813          
    ##          Pos Pred Value : 0.7900          
    ##          Neg Pred Value : 0.6596          
    ##              Prevalence : 0.6522          
    ##          Detection Rate : 0.5478          
    ##    Detection Prevalence : 0.6935          
    ##       Balanced Accuracy : 0.7106          
    ##                                           
    ##        'Positive' Class : neg             
    ## 

``` r
plot(table(predictions_nb_e1071,
           PimaIndiansDiabetes_test[, c("pregnant", "glucose", "pressure",
           "triceps", "insulin","mass","pedigree","age","diabetes")]$diabetes))
```

![](Lab-Submission-Markdown_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

### caret Naive Bayes model and test results using a confusion matrix

``` r
print(PimaIndiansDiabetes_model_nb_caret)
```

    ## Naive Bayes 
    ## 
    ## 308 samples
    ##   8 predictor
    ##   2 classes: 'neg', 'pos' 
    ## 
    ## No pre-processing
    ## Resampling: Bootstrapped (25 reps) 
    ## Summary of sample sizes: 308, 308, 308, 308, 308, 308, ... 
    ## Resampling results across tuning parameters:
    ## 
    ##   usekernel  Accuracy   Kappa    
    ##   FALSE      0.7310553  0.3966975
    ##    TRUE      0.7328922  0.3994130
    ## 
    ## Tuning parameter 'laplace' was held constant at a value of 0
    ## Tuning
    ##  parameter 'adjust' was held constant at a value of 1
    ## Accuracy was used to select the optimal model using the largest value.
    ## The final values used for the model were laplace = 0, usekernel = TRUE
    ##  and adjust = 1.

``` r
caret::confusionMatrix(predictions_nb_caret,
                       PimaIndiansDiabetes_test[, c("pregnant", "glucose",                               "pressure","triceps", "insulin","mass","pedigree","age",
                                                    "diabetes")]$diabetes)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction neg pos
    ##        neg 253  62
    ##        pos  47  98
    ##                                           
    ##                Accuracy : 0.763           
    ##                  95% CI : (0.7215, 0.8012)
    ##     No Information Rate : 0.6522          
    ##     P-Value [Acc > NIR] : 1.759e-07       
    ##                                           
    ##                   Kappa : 0.466           
    ##                                           
    ##  Mcnemar's Test P-Value : 0.1799          
    ##                                           
    ##             Sensitivity : 0.8433          
    ##             Specificity : 0.6125          
    ##          Pos Pred Value : 0.8032          
    ##          Neg Pred Value : 0.6759          
    ##              Prevalence : 0.6522          
    ##          Detection Rate : 0.5500          
    ##    Detection Prevalence : 0.6848          
    ##       Balanced Accuracy : 0.7279          
    ##                                           
    ##        'Positive' Class : neg             
    ## 

``` r
plot(table(predictions_nb_caret,
           PimaIndiansDiabetes_test[, c("pregnant", "glucose", "pressure",
           "triceps", "insulin","mass","pedigree","age","diabetes")]$diabetes))  
```

![](Lab-Submission-Markdown_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

# Bootstrapping: Pima Indians Diabetes

### Split the dataset

``` r
library(caret)

train_index <- createDataPartition(PimaIndiansDiabetes$diabetes,
                                   p = 0.40,
                                   list = FALSE)
PimaIndiansDiabetes_train <- PimaIndiansDiabetes[train_index, ]
PimaIndiansDiabetes_test <- PimaIndiansDiabetes[-train_index, ]
```

### Train a linear regression model (for regression)

``` r
train_control <- trainControl(method = "boot", number = 500)

PimaIndiansDiabetes_model_lm <- # nolint
  caret::train(`glucose` ~
                 `pedigree` + `mass` +
                   `insulin` + `triceps` +
                   `pressure`,
               data = PimaIndiansDiabetes_train,
               trControl = train_control,
               na.action = na.omit, method = "lm", metric = "RMSE")
```

### Test the trained linear regression model using the testing dataset

``` r
predictions_lm <- predict(PimaIndiansDiabetes_model_lm,
                          PimaIndiansDiabetes_test[, 1:9])
```

### View the RMSE and the predicted values for the 12 observations

``` r
print(PimaIndiansDiabetes_model_lm)
```

    ## Linear Regression 
    ## 
    ## 308 samples
    ##   5 predictor
    ## 
    ## No pre-processing
    ## Resampling: Bootstrapped (500 reps) 
    ## Summary of sample sizes: 308, 308, 308, 308, 308, 308, ... 
    ## Resampling results:
    ## 
    ##   RMSE      Rsquared   MAE     
    ##   29.25956  0.1532858  22.61575
    ## 
    ## Tuning parameter 'intercept' was held constant at a value of TRUE

``` r
print(predictions_lm)
```

    ##         4         5         6         7         9        10        13        15 
    ## 115.07996 144.20756 115.36588 111.41614 150.52317  98.94483 128.69100 127.28092 
    ##        17        20        21        25        26        27        30        31 
    ## 137.75183 121.28031 137.64644 127.29052 119.06744 126.90238 125.84280 115.88854 
    ##        32        33        35        39        41        43        46        48 
    ## 134.27075 113.49938 107.39566 109.50844 117.23038 108.53333 126.07207 108.88206 
    ##        49        50        51        52        55        56        58        60 
    ## 112.64398  85.59669 117.18320 110.85785 140.63711 106.09383 126.13321 122.27067 
    ##        61        62        63        65        67        68        69        70 
    ##  85.58759 121.38046 116.65973 120.31771 116.20529 137.15493 108.89097 117.57079 
    ##        71        72        73        76        77        78        79        80 
    ## 126.05880 116.71746 135.02366 101.96761 123.12381 112.24986 118.88357 105.51298 
    ##        81        82        84        86        88        89        91        92 
    ## 102.51643  83.74964 101.91450 124.83719 121.89113 120.28671 108.05488 133.72512 
    ##        93        94        96        98        99       101       102       103 
    ## 120.73060 114.36525 132.20992 108.27519 109.18571 134.78770 113.51119 116.72058 
    ##       104       105       109       111       113       114       115       116 
    ## 112.84921 131.57586 109.78103 119.97660 109.22257 121.87748 122.89255 125.42484 
    ##       121       123       125       128       130       131       133       134 
    ## 128.24167 120.32537 122.34826 113.38636 123.52802 129.39471 127.97153 114.63493 
    ##       136       139       140       142       143       148       149       150 
    ## 123.50089 125.16550 141.96439 115.59762 113.66342 125.21778 122.40541 107.95310 
    ##       151       152       157       158       159       161       162       164 
    ## 124.67564 113.27581 117.92952 122.11771 115.32284 105.87542 117.60256 109.01481 
    ##       165       166       167       169       170       173       174       177 
    ## 127.00830 130.61981 109.63098 121.55564 124.88647 102.73593 118.57338 121.95286 
    ##       178       179       180       181       186       188       190       191 
    ## 147.01123 130.94093 133.90483 113.31014 115.76486 122.38314 122.81390 110.74381 
    ##       192       193       196       198       199       200       201       204 
    ## 112.67123 119.58809 131.97601 114.94532 118.41992 135.63956 119.88731 108.50883 
    ##       205       207       208       209       210       211       213       218 
    ## 130.13039 142.83790 128.70241 117.53696 112.15353 106.58286 111.84479 119.07736 
    ##       219       220       221       222       225       226       228       229 
    ## 118.13587 124.23451 161.13032 127.86435 115.88261 113.78265 109.42049 196.52099 
    ##       230       232       234       235       236       238       240       241 
    ## 122.51791 151.98126 123.55845 111.06435 131.60565 125.23540 113.52356 106.60707 
    ##       242       244       245       246       247       248       249       251 
    ## 117.77054 130.57413 130.27292 123.34111 119.36499 190.75074 147.20192 118.13965 
    ##       253       254       258       261       264       265       266       267 
    ## 115.07750 109.40436 106.72690 125.47841 115.26119 118.82038 127.62406 119.54855 
    ##       270       273       274       275       277       279       282       283 
    ## 106.39756 114.40942 107.23743 121.92693 104.86916 119.76199 124.18354 131.58070 
    ##       286       287       289       291       292       293       295       296 
    ## 121.58794 163.74560 107.78505 119.97944 121.67243 140.71138 109.46680 124.13716 
    ##       297       298       299       302       304       307       308       309 
    ## 135.90606 131.82506 133.30833 118.63972 140.17847 118.37238 121.42526 138.13920 
    ##       310       313       314       316       318       323       324       327 
    ## 133.90738 120.19375 122.58923 121.80332 120.48783 108.31637 112.41202 127.08863 
    ##       328       330       331       332       333       334       336       337 
    ## 122.16300 111.15108 116.79944 116.45057 119.07055 114.10353 139.58336 117.59470 
    ##       338       339       341       342       343       345       347       348 
    ## 121.30608 130.83774 121.93687 117.99868 106.57003 126.37051 117.69294 102.80372 
    ##       349       350       351       352       353       354       355       356 
    ## 110.40174 116.18366 129.48238 121.64579 112.07389 117.34545 132.50921 122.06226 
    ##       358       359       360       363       364       366       367       369 
    ## 106.51485 112.90792 138.67729 116.41029 128.88717 117.81655 118.14926 119.08384 
    ##       370       374       375       376       377       379       381       383 
    ## 126.84257 112.63382 122.91778 142.72695 118.75859 133.50686 119.95546 133.74404 
    ##       385       387       388       389       390       393       394       395 
    ## 114.28754 112.64938 118.24888 139.76589 123.98631 147.35248 117.91424 127.10588 
    ##       398       400       402       407       408       410       411       414 
    ## 104.22418 109.44088 111.92438 119.23332 111.96444 166.15649 113.58296 112.89108 
    ##       415       416       417       422       425       426       428       429 
    ## 124.46126 156.03661 115.10354 117.71619 138.68050 135.18179 126.09232 125.15470 
    ##       430       433       436       437       438       439       440       442 
    ## 130.64239 122.81023 117.66983 112.76754 120.97684 102.27314 130.90781 117.13094 
    ##       445       447       448       450       451       454       455       456 
    ## 113.42354 120.57785 115.76974 118.35777 115.94519 105.63862 122.83398 107.41545 
    ##       458       461       462       463       465       467       469       473 
    ## 114.54628 112.26804 112.61455 114.82881 125.09444 113.69886 107.82368 113.72425 
    ##       474       476       477       478       479       480       485       486 
    ## 121.12814 107.15093 126.37554 119.92541 106.44152 108.02833 122.93704 134.97072 
    ##       489       490       492       495       497       498       500       501 
    ## 108.82424 119.81518 112.15250  84.40475 115.62926 122.61427 128.85616 117.16056 
    ##       503       506       508       509       510       511       515       516 
    ## 112.58630 123.08755 126.52267 120.12782 117.37554 106.19724 112.18209 122.42773 
    ##       517       518       519       520       522       523       525       526 
    ## 127.58881 127.38942 118.73223 144.25985 121.56040  84.54123 117.24296 105.06408 
    ##       527       529       530       532       535       536       537       538 
    ## 108.82811 125.65397 117.45067 135.39538 121.03033 111.16235 120.77649 115.14735 
    ##       539       541       542       545       546       549       550       551 
    ## 134.37169 133.82464 130.97659 117.46839 133.21098 111.76547 114.29516 104.52218 
    ##       552       553       554       556       558       559       560       563 
    ## 120.39332 119.53928 113.09342 120.80041 117.69675 113.13622 119.76722 118.69966 
    ##       564       566       567       568       570       571       576       577 
    ## 114.17837 120.25098 116.27839 116.03882 123.99076 120.77737 105.71814 119.19754 
    ##       579       581       582       583       587       592       594       595 
    ## 115.97952 112.88276 101.63120 110.08167 120.77756 118.75386 129.68080 129.00068 
    ##       596       597       598       599       600       604       606       607 
    ## 137.45608 130.91878 107.45866 123.05022 113.82122 127.63868 110.74793 146.81329 
    ##       610       611       613       614       616       617       618       622 
    ## 123.49895 123.50200 145.22235 116.08168 115.28413 120.58820 105.83084 119.84150 
    ##       623       624       626       627       628       630       631       634 
    ## 141.57370 129.58804 113.75093 113.83550 122.98643 103.68694 120.13794 127.35607 
    ##       635       637       638       640       643       644       645       647 
    ## 113.53835 117.41843 123.09969 109.46792 119.06620 110.15306 123.22617 122.34932 
    ##       648       649       650       651       652       653       654       655 
    ## 123.60180 117.08828 102.89086 111.41199 122.48813 113.14730 115.69122 121.95318 
    ##       657       658       659       661       662       664       665       666 
    ## 103.51697 134.11899 130.36046 118.28622 122.02189 122.81106 103.74492 117.18371 
    ##       667       674       677       678       679       681       683       684 
    ## 114.69715 157.97661 116.75897 121.43218 119.57158 105.38920 123.79082 124.50169 
    ##       686       687       689       690       691       692       693       694 
    ## 133.26728 112.98967 125.99194 131.43964 121.42346 134.70485 127.10021 118.00190 
    ##       696       697       698       699       700       702       703       705 
    ## 155.92800 123.15402 104.57109 137.94111 135.88082 107.87790 119.02239 118.14402 
    ##       706       707       709       711       713       717       718       720 
    ## 112.04286  85.19634 121.06839 150.11366 112.90668 130.45775 109.20082 113.77734 
    ##       721       722       723       724       726       728       729       731 
    ## 113.12040 128.51884 118.56267 125.43383 110.30698 113.37359 116.44637 117.07258 
    ##       733       735       737       738       740       741       745       747 
    ## 132.01031 116.98914 121.39839 114.08260 127.01580 133.21938 135.66293 121.03624 
    ##       749       750       753       755       757       758       760       761 
    ## 134.70670 112.39380 104.10792 110.08437 107.29496 123.91614 126.39503 111.00269 
    ##       762       763       764       767 
    ## 118.57763 110.66602 117.96856 118.16959

### Use the model to make a prediction on unseen new data

``` r
new_data <-
  data.frame(`pregnant` = c(9), # nolint
             `glucose` = c(200),
             `pressure` = c(30),
             `triceps` = c(55), 
             `insulin` = c(11),
             `mass` = c(80.5),
             `pedigree` = c(0.526), 
             `age` = c(40),
             `diabetes` = c('neg'), check.names = FALSE)

predictions_lm_new_data <-
  predict(PimaIndiansDiabetes_model_lm, new_data)

print(predictions_lm_new_data)
```

    ##        1 
    ## 132.6854

# CV, Repeated CV, and LOOCV: Pima Indians Diabetes Database

## Regression: Linear Model

### 10-fold cross validation

``` r
train_control <- trainControl(method = "cv", number = 10)

PimaIndiansDiabetes_model_lm <-
  caret::train(`mass` ~ .,
               data = PimaIndiansDiabetes_train,
               trControl = train_control, na.action = na.omit,
               method = "lm", metric = "RMSE")
```

### Test the trained linear model using the testing dataset

``` r
predictions_lm <- predict(PimaIndiansDiabetes_model_lm, PimaIndiansDiabetes_test[, -10])
```

### View the RMSE and the predicted values

``` r
print(PimaIndiansDiabetes_model_lm)
```

    ## Linear Regression 
    ## 
    ## 308 samples
    ##   8 predictor
    ## 
    ## No pre-processing
    ## Resampling: Cross-Validated (10 fold) 
    ## Summary of sample sizes: 277, 277, 277, 278, 278, 277, ... 
    ## Resampling results:
    ## 
    ##   RMSE     Rsquared   MAE     
    ##   7.00074  0.2366921  5.231729
    ## 
    ## Tuning parameter 'intercept' was held constant at a value of TRUE

``` r
print(predictions_lm)
```

    ##        4        5        6        7        9       10       13       15 
    ## 31.05121 35.45537 27.97594 35.47878 39.11235 31.92893 26.68749 34.67684 
    ##       17       20       21       25       26       27       30       31 
    ## 39.85304 36.39225 35.42102 38.05025 35.97992 32.27014 28.23440 29.76317 
    ##       32       33       35       39       41       43       46       48 
    ## 38.95596 28.69167 32.56427 38.20934 33.36847 30.35380 39.45413 31.39354 
    ##       49       50       51       52       55       56       58       60 
    ## 36.85741 24.57619 29.92723 28.78741 34.11026 27.75323 37.45912 34.16739 
    ##       61       62       63       65       67       68       69       70 
    ## 23.93756 32.07718 24.92927 31.00000 36.59427 26.42863 29.23550 33.85201 
    ##       71       72       73       76       77       78       79       80 
    ## 34.39751 33.98232 32.77468 27.40127 26.00537 33.00259 28.92653 31.36601 
    ##       81       82       84       86       88       89       91       92 
    ## 29.07962 23.67581 32.07970 32.37202 31.41955 37.46133 26.51169 30.30375 
    ##       93       94       96       98       99      101      102      103 
    ## 33.07769 30.23561 32.13111 28.74946 31.66676 32.51003 28.54125 29.63682 
    ##      104      105      109      111      113      114      115      116 
    ## 30.09762 26.50287 31.68069 39.25164 33.28017 26.59864 36.84267 31.36603 
    ##      121      123      125      128      130      131      133      134 
    ## 42.61541 32.89924 32.45582 33.38177 29.50272 35.32251 38.88338 31.73374 
    ##      136      139      140      142      143      148      149      150 
    ## 30.46174 28.17815 32.23872 32.47210 31.31557 31.99374 26.29476 30.39638 
    ##      151      152      157      158      159      161      162      164 
    ## 36.72571 26.79578 29.17915 30.30191 30.76700 35.44579 33.14985 31.33145 
    ##      165      166      167      169      170      173      174      177 
    ## 32.66086 33.81275 33.02999 27.33910 30.39489 27.37381 33.37637 26.45963 
    ##      178      179      180      181      186      188      190      191 
    ## 41.83246 27.57602 32.20652 27.48051 37.72822 39.62946 39.12583 27.82984 
    ##      192      193      196      198      199      200      201      204 
    ## 34.62289 32.57309 40.44139 33.77389 38.78921 36.56603 30.98130 29.97828 
    ##      205      207      208      209      210      211      213      218 
    ## 30.77902 36.93154 33.26461 31.76291 39.23629 30.19943 33.65911 32.59676 
    ##      219      220      221      222      225      226      228      229 
    ## 34.60169 30.89300 37.49146 30.96844 29.48276 32.27581 38.84646 34.64866 
    ##      230      232      234      235      236      238      240      241 
    ## 33.57791 37.60730 27.77506 31.58320 33.71278 39.20334 27.53162 31.26377 
    ##      242      244      245      246      247      248      249      251 
    ## 32.82700 34.05185 34.30284 35.49944 27.29975 34.91564 32.97111 25.90895 
    ##      253      254      258      261      264      265      266      267 
    ## 30.12080 32.27889 31.51731 31.49778 28.79274 31.38350 29.10906 28.94591 
    ##      270      273      274      275      277      279      282      283 
    ## 29.30189 27.41562 35.64296 26.34527 35.47426 25.66851 32.55385 31.01274 
    ##      286      287      289      291      292      293      295      296 
    ## 31.04400 35.75875 29.53771 32.75579 36.20099 38.08928 24.92800 33.35983 
    ##      297      298      299      302      304      307      308      309 
    ## 38.59948 33.34395 35.29591 37.64032 33.56760 35.78876 30.77217 34.86423 
    ##      310      313      314      316      318      323      324      327 
    ## 36.16718 35.93682 28.38144 31.20710 33.84896 34.79034 38.78225 36.63743 
    ##      328      330      331      332      333      334      336      337 
    ## 29.19404 32.31108 29.61212 29.24291 29.10516 27.42719 36.18234 22.67638 
    ##      338      339      341      342      343      345      347      348 
    ## 31.20756 38.64331 30.44238 29.96560 30.80273 25.36855 30.44329 24.71681 
    ##      349      350      351      352      353      354      355      356 
    ## 30.17487 34.30727 27.65291 28.92918 30.25298 28.72459 27.93238 32.98534 
    ##      358      359      360      363      364      366      367      369 
    ## 33.24198 32.88013 39.71526 32.78155 30.22081 31.01468 32.44293 30.70516 
    ##      370      374      375      376      377      379      381      383 
    ## 37.17409 33.63877 33.76893 38.28201 30.56518 33.10308 32.52872 28.39476 
    ##      385      387      388      389      390      393      394      395 
    ## 32.00170 36.53983 38.29471 35.16581 30.76430 30.10549 29.08777 33.17739 
    ##      398      400      402      407      408      410      411      414 
    ## 39.19456 39.44204 26.04729 30.77941 27.00116 40.64479 34.95507 32.67691 
    ##      415      416      417      422      425      426      428      429 
    ## 38.01774 39.65997 30.71715 30.39003 38.08234 40.23662 37.31808 36.91220 
    ##      430      433      436      437      438      439      440      442 
    ## 34.81304 29.04818 28.97847 34.24045 28.89372 30.23233 28.23861 30.81542 
    ##      445      447      448      450      451      454      455      456 
    ## 33.55492 29.11520 35.06210 30.95799 28.86290 20.75664 31.27883 38.15185 
    ##      458      461      462      463      465      467      469      473 
    ## 31.89308 30.28116 26.18838 32.63512 28.86724 27.68965 28.28092 32.41867 
    ##      474      476      477      478      479      480      485      486 
    ## 27.88492 30.95979 39.25517 30.69565 34.00635 31.48529 28.77669 39.24426 
    ##      489      490      492      495      497      498      500      501 
    ## 30.32526 27.52935 31.92685 23.87003 27.49133 29.45301 33.22648 32.33409 
    ##      503      506      508      509      510      511      515      516 
    ## 34.83561 27.00965 31.53964 29.92751 25.80299 35.69692 29.96272 36.14963 
    ##      517      518      519      520      522      523      525      526 
    ## 37.48177 27.29316 25.94174 27.55339 33.97367 24.63160 27.76011 30.00321 
    ##      527      529      530      532      535      536      537      538 
    ## 30.45760 32.74487 26.83347 27.80075 30.83690 29.43137 26.93848 22.41847 
    ##      539      541      542      545      546      549      550      551 
    ## 34.48454 37.31085 36.39617 31.95787 40.01960 34.86251 36.03554 32.86764 
    ##      552      553      554      556      558      559      560      563 
    ## 31.84826 25.91858 30.80431 32.85851 25.96583 33.51114 27.15202 32.63732 
    ##      564      566      567      568      570      571      576      577 
    ## 29.75749 28.89902 32.76780 30.84238 36.24839 25.95575 34.42073 28.93796 
    ##      579      581      582      583      587      592      594      595 
    ## 27.96965 41.91298 31.89298 29.12917 31.93529 36.52079 29.19884 34.87384 
    ##      596      597      598      599      600      604      606      607 
    ## 36.74881 25.26631 28.23785 32.89113 28.84968 36.14559 33.13365 40.77689 
    ##      610      611      613      614      616      617      618      622 
    ## 29.51548 30.34846 40.09351 32.78358 27.71526 29.17328 28.61211 30.20957 
    ##      623      624      626      627      628      630      631      634 
    ## 29.16296 31.89211 35.72539 28.26903 28.87814 31.26927 31.34197 31.69489 
    ##      635      637      638      640      643      644      645      647 
    ## 27.02528 26.30955 30.61325 29.42273 31.89686 23.34986 32.28358 35.66527 
    ##      648      649      650      651      652      653      654      655 
    ## 38.70601 38.40988 31.44753 30.65798 30.89719 34.86554 27.02973 32.40860 
    ##      657      658      659      661      662      664      665      666 
    ## 32.96106 34.71073 28.63831 28.15832 41.57901 40.16337 37.31853 35.71514 
    ##      667      674      677      678      679      681      683      684 
    ## 33.26544 35.25207 32.37463 26.72199 31.56988 30.52154 33.54767 32.82704 
    ##      686      687      689      690      691      692      693      694 
    ## 32.46562 28.28071 32.77885 39.30930 27.69758 34.75298 33.22790 39.37265 
    ##      696      697      698      699      700      702      703      705 
    ## 36.35696 36.41829 24.13531 30.56239 27.81756 36.33375 36.92674 31.33722 
    ##      706      707      709      711      713      717      718      720 
    ## 33.57052 28.86567 32.82445 30.60057 37.62261 39.73529 28.50963 34.61906 
    ##      721      722      723      724      726      728      729      731 
    ## 30.47373 33.70232 36.26988 32.54651 34.04720 33.64892 30.56035 36.07933 
    ##      733      735      737      738      740      741      745      747 
    ## 40.57943 25.66131 33.44426 29.53173 30.68253 37.46463 35.12314 40.78698 
    ##      749      750      753      755      757      758      760      761 
    ## 36.67014 31.39126 31.41699 37.72757 35.51716 30.30826 32.34242 30.98836 
    ##      762      763      764      767 
    ## 38.00743 26.73885 33.30800 30.20533

## Classification: LDA with k-fold Cross Validation

### LDA classifier based on a 5-fold cross validation

``` r
library(caret)
train_control <- trainControl(method = "cv", number = 5)

PimaIndiansDiabetes_model_lda <-
  caret::train(`diabetes` ~ ., data = PimaIndiansDiabetes_train,
               trControl = train_control, na.action = na.omit, method = "lda",
               metric = "Accuracy")
```

### Test the trained LDA model using the testing dataset

``` r
predictions_lda <- predict(PimaIndiansDiabetes_model_lda,
                           PimaIndiansDiabetes_test[, 1:9])
```

### View the summary of the model and view the confusion matrix

``` r
print(PimaIndiansDiabetes_model_lda)
```

    ## Linear Discriminant Analysis 
    ## 
    ## 308 samples
    ##   8 predictor
    ##   2 classes: 'neg', 'pos' 
    ## 
    ## No pre-processing
    ## Resampling: Cross-Validated (5 fold) 
    ## Summary of sample sizes: 246, 247, 247, 246, 246 
    ## Resampling results:
    ## 
    ##   Accuracy   Kappa    
    ##   0.7822316  0.4989278

``` r
caret::confusionMatrix(predictions_lda, PimaIndiansDiabetes_test$diabetes)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction neg pos
    ##        neg 262  75
    ##        pos  38  85
    ##                                          
    ##                Accuracy : 0.7543         
    ##                  95% CI : (0.7124, 0.793)
    ##     No Information Rate : 0.6522         
    ##     P-Value [Acc > NIR] : 1.466e-06      
    ##                                          
    ##                   Kappa : 0.4277         
    ##                                          
    ##  Mcnemar's Test P-Value : 0.0007077      
    ##                                          
    ##             Sensitivity : 0.8733         
    ##             Specificity : 0.5312         
    ##          Pos Pred Value : 0.7774         
    ##          Neg Pred Value : 0.6911         
    ##              Prevalence : 0.6522         
    ##          Detection Rate : 0.5696         
    ##    Detection Prevalence : 0.7326         
    ##       Balanced Accuracy : 0.7023         
    ##                                          
    ##        'Positive' Class : neg            
    ## 

## Classification: Naive Bayes with Repeated k-fold Cross Validation

### Train an e1071::naive Bayes classifier based on the diabetes variable

``` r
PimaIndiansDiabetes_model_nb <-
  e1071::naiveBayes(`diabetes` ~ ., data = PimaIndiansDiabetes_train)
```

### Test the trained naive Bayes classifier using the testing dataset

``` r
predictions_nb_e1071 <-
  predict(PimaIndiansDiabetes_model_nb, PimaIndiansDiabetes_test[, 1:9])
```

### View a summary of the naive Bayes model and the confusion matrix

``` r
print(PimaIndiansDiabetes_model_nb)
```

    ## 
    ## Naive Bayes Classifier for Discrete Predictors
    ## 
    ## Call:
    ## naiveBayes.default(x = X, y = Y, laplace = laplace)
    ## 
    ## A-priori probabilities:
    ## Y
    ##       neg       pos 
    ## 0.6493506 0.3506494 
    ## 
    ## Conditional probabilities:
    ##      pregnant
    ## Y         [,1]     [,2]
    ##   neg 2.935000 2.799996
    ##   pos 5.444444 3.692208
    ## 
    ##      glucose
    ## Y         [,1]     [,2]
    ##   neg 110.9600 24.70343
    ##   pos 141.9444 32.51853
    ## 
    ##      pressure
    ## Y         [,1]     [,2]
    ##   neg 67.42500 18.06589
    ##   pos 71.30556 21.35304
    ## 
    ##      triceps
    ## Y         [,1]     [,2]
    ##   neg 20.39500 15.06598
    ##   pos 22.37037 19.17679
    ## 
    ##      insulin
    ## Y         [,1]     [,2]
    ##   neg  73.8650 100.3892
    ##   pos 109.8426 158.5832
    ## 
    ##      mass
    ## Y         [,1]     [,2]
    ##   neg 30.57550 7.783616
    ##   pos 35.73796 6.906339
    ## 
    ##      pedigree
    ## Y          [,1]      [,2]
    ##   neg 0.4234250 0.2909826
    ##   pos 0.6053889 0.4036250
    ## 
    ##      age
    ## Y         [,1]     [,2]
    ##   neg 30.62500 11.24351
    ##   pos 37.11111 10.80268

``` r
caret::confusionMatrix(predictions_nb_e1071, PimaIndiansDiabetes_test$diabetes)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction neg pos
    ##        neg 246  67
    ##        pos  54  93
    ##                                           
    ##                Accuracy : 0.737           
    ##                  95% CI : (0.6942, 0.7766)
    ##     No Information Rate : 0.6522          
    ##     P-Value [Acc > NIR] : 5.947e-05       
    ##                                           
    ##                   Kappa : 0.409           
    ##                                           
    ##  Mcnemar's Test P-Value : 0.2753          
    ##                                           
    ##             Sensitivity : 0.8200          
    ##             Specificity : 0.5813          
    ##          Pos Pred Value : 0.7859          
    ##          Neg Pred Value : 0.6327          
    ##              Prevalence : 0.6522          
    ##          Detection Rate : 0.5348          
    ##    Detection Prevalence : 0.6804          
    ##       Balanced Accuracy : 0.7006          
    ##                                           
    ##        'Positive' Class : neg             
    ## 

## Classification: SVM with Repeated k-fold Cross Validation

### SVM Classifier using 5-fold cross validation with 3 reps

``` r
train_control <- trainControl(method = "repeatedcv", number = 5, repeats = 3)

PimaIndiansDiabetes_model_svm <-
  caret::train(`diabetes` ~ ., data = PimaIndiansDiabetes_train,
               trControl = train_control, na.action = na.omit,
               method = "svmLinearWeights2", metric = "Accuracy")
```

### Test the trained SVM model using the testing dataset

``` r
predictions_svm <- predict(PimaIndiansDiabetes_model_svm, PimaIndiansDiabetes_test[, 1:9])
```

### View a summary of the model and view the confusion matrix

``` r
print(PimaIndiansDiabetes_model_svm)
```

    ## L2 Regularized Linear Support Vector Machines with Class Weights 
    ## 
    ## 308 samples
    ##   8 predictor
    ##   2 classes: 'neg', 'pos' 
    ## 
    ## No pre-processing
    ## Resampling: Cross-Validated (5 fold, repeated 3 times) 
    ## Summary of sample sizes: 246, 247, 246, 246, 247, 247, ... 
    ## Resampling results across tuning parameters:
    ## 
    ##   cost  Loss  weight  Accuracy   Kappa     
    ##   0.25  L1    1       0.5844527  0.12108115
    ##   0.25  L1    2       0.5467125  0.06404017
    ##   0.25  L1    3       0.6039485  0.10790771
    ##   0.25  L2    1       0.7359422  0.37559541
    ##   0.25  L2    2       0.7066455  0.39942735
    ##   0.25  L2    3       0.5811916  0.24141960
    ##   0.50  L1    1       0.6070333  0.02949608
    ##   0.50  L1    2       0.5975674  0.06646850
    ##   0.50  L1    3       0.5564075  0.09856431
    ##   0.50  L2    1       0.7349022  0.38047889
    ##   0.50  L2    2       0.7102415  0.40750349
    ##   0.50  L2    3       0.5650626  0.21780260
    ##   1.00  L1    1       0.5885422  0.09551189
    ##   1.00  L1    2       0.6167460  0.12746211
    ##   1.00  L1    3       0.6242376  0.15060894
    ##   1.00  L2    1       0.7402785  0.39295528
    ##   1.00  L2    2       0.7057994  0.40232825
    ##   1.00  L2    3       0.5715494  0.22695140
    ## 
    ## Accuracy was used to select the optimal model using the largest value.
    ## The final values used for the model were cost = 1, Loss = L2 and weight = 1.

``` r
caret::confusionMatrix(predictions_svm, PimaIndiansDiabetes_test$diabetes)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction neg pos
    ##        neg 263  73
    ##        pos  37  87
    ##                                           
    ##                Accuracy : 0.7609          
    ##                  95% CI : (0.7192, 0.7992)
    ##     No Information Rate : 0.6522          
    ##     P-Value [Acc > NIR] : 3.04e-07        
    ##                                           
    ##                   Kappa : 0.4437          
    ##                                           
    ##  Mcnemar's Test P-Value : 0.0008465       
    ##                                           
    ##             Sensitivity : 0.8767          
    ##             Specificity : 0.5437          
    ##          Pos Pred Value : 0.7827          
    ##          Neg Pred Value : 0.7016          
    ##              Prevalence : 0.6522          
    ##          Detection Rate : 0.5717          
    ##    Detection Prevalence : 0.7304          
    ##       Balanced Accuracy : 0.7102          
    ##                                           
    ##        'Positive' Class : neg             
    ## 

## Classification: Naive Bayes with Leave One Out Cross Validation

### Train a Naive Bayes classifier based on an LOOCV

``` r
train_control <- trainControl(method = "LOOCV")

PimaIndiansDiabetes_model_nb_loocv <-
  caret::train(`diabetes` ~ ., data = PimaIndiansDiabetes_train,
               trControl = train_control, na.action = na.omit,
               method = "naive_bayes", metric = "Accuracy")
```

### Test the trained model using the testing dataset

``` r
predictions_nb_loocv <-
  predict(PimaIndiansDiabetes_model_nb_loocv, PimaIndiansDiabetes_test[, 1:9])
```

### View the confusion matrix

``` r
print(PimaIndiansDiabetes_model_nb_loocv)
```

    ## Naive Bayes 
    ## 
    ## 308 samples
    ##   8 predictor
    ##   2 classes: 'neg', 'pos' 
    ## 
    ## No pre-processing
    ## Resampling: Leave-One-Out Cross-Validation 
    ## Summary of sample sizes: 307, 307, 307, 307, 307, 307, ... 
    ## Resampling results across tuning parameters:
    ## 
    ##   usekernel  Accuracy   Kappa    
    ##   FALSE      0.7532468  0.4439377
    ##    TRUE      0.8019481  0.5507413
    ## 
    ## Tuning parameter 'laplace' was held constant at a value of 0
    ## Tuning
    ##  parameter 'adjust' was held constant at a value of 1
    ## Accuracy was used to select the optimal model using the largest value.
    ## The final values used for the model were laplace = 0, usekernel = TRUE
    ##  and adjust = 1.

``` r
caret::confusionMatrix(predictions_nb_loocv, PimaIndiansDiabetes_test$diabetes)
```

    ## Confusion Matrix and Statistics
    ## 
    ##           Reference
    ## Prediction neg pos
    ##        neg 248  69
    ##        pos  52  91
    ##                                           
    ##                Accuracy : 0.737           
    ##                  95% CI : (0.6942, 0.7766)
    ##     No Information Rate : 0.6522          
    ##     P-Value [Acc > NIR] : 5.947e-05       
    ##                                           
    ##                   Kappa : 0.4055          
    ##                                           
    ##  Mcnemar's Test P-Value : 0.1458          
    ##                                           
    ##             Sensitivity : 0.8267          
    ##             Specificity : 0.5687          
    ##          Pos Pred Value : 0.7823          
    ##          Neg Pred Value : 0.6364          
    ##              Prevalence : 0.6522          
    ##          Detection Rate : 0.5391          
    ##    Detection Prevalence : 0.6891          
    ##       Balanced Accuracy : 0.6977          
    ##                                           
    ##        'Positive' Class : neg             
    ## 
