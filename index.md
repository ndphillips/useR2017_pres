---
title       : FFTrees
subtitle    : An R package to create, visualize, and evaluate fast-and-frugal decision trees
author      : Nathaniel Phillips, Economic Psychology, University of Basel, Switzerland
job         : useR! 2017, Brussels
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---




---  .nobackground


### Limited Time. Limited information. How can one make good decisions?

<br>


<img src="images/threeexamples.png" title="plot of chunk unnamed-chunk-1" alt="plot of chunk unnamed-chunk-1" width="100%" style="display: block; margin: auto;" />





---&twocol






## Cook County Hospital, 1996

***=left
<img src="images/crowdedemergency.jpg" title="plot of chunk unnamed-chunk-4" alt="plot of chunk unnamed-chunk-4" width="100%" style="display: block; margin: auto;" />

***=right


"As the city’s principal public hospital, Cook County was the place of last resort for the hundreds of thousands of Chicagoans without health insurance. Resources were stretched to the limit. The hospital’s cavernous wards were built for another century. There were no private rooms, and patients were separated by flimsy plywood dividers. [\...] Doctors once trained a homeless man to do routine lab tests because there was no one else available." Malcolm Gladwell, Blink.



---&twocol

***=left



<img src="images/paindecision.png" title="plot of chunk unnamed-chunk-5" alt="plot of chunk unnamed-chunk-5" width="70%" style="display: block; margin: auto;" />

***=right

### Heart Attack Diagnosis

- How do doctors make decisions? Experience. Intuition. Clinical judgment

<img src="images/doctordeciding.jpg" title="plot of chunk unnamed-chunk-6" alt="plot of chunk unnamed-chunk-6" width="90%" style="display: block; margin: auto;" />

- In a Michigan hospital, doctors sent 90% of patients to the ICU, although only 25%  were actually having a heart attack.

---&twocol

## Emergency Room Solution: a fast-and-frugal tree (FFT)

***=left

- A fast-and-frugal decision tree (FFT) developed by Green & Mehr (1997).
- Tree cut the false-alarm rate in half
- Tree is transparent, easy to modify, and accepted by physicians (unlike regression).

### What is a fast-and-frugal decision tree (FFT)?

***=right

<img src="images/GreenMehrFFT.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" width="65%" style="display: block; margin: auto;" />

Green & Mehr (1997) "What alters physicians' decisions to admit to the coronary care unit?"


--- &twocol

## Fast-and-Frugal Decision Tree (FFT)

- An FFT is a decision tree with exactly two branches from each node, where one, or both, of the branches are exit branches (Martignon et al., 2008)


<!-- #### Descriptive Uses -->

<!-- - Inference (Gigerenzer & Goldstein, 1996) -->
<!-- - Judge's bailing decisions (Dhami, 2003) -->
<!-- - Fish mating (Dugatkin & Godin, 1996) -->

<!-- #### Prescriptive Uses -->

<!-- - Terrorist attacks (Garcia, 2016) -->
<!-- - Bank failure (Neth et al., 2014) -->
<!-- - Depression diagnosis (Jenny et al., 2013) -->

<img src="images/complexvsimple.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" width="85%" style="display: block; margin: auto;" />


<!-- --- .quote .segue .nobackground -->


<!-- <q>No more entities should be presumed to exist than are absolutely necessary</q> Willam of Occam -->



<!-- --- .quote .segue .nobackground -->


<!-- <q>If a decision tree that measures up very well on the performance criterion is nevertheless totally incomprehensible to a human expert, can it be described as *knowledge?* Under the common-sense definition of this term [...] it is not." </q> (p. 498, Quinlan, 1999) -->


---
# Examples of FFTs


<img src="images/FFTexamples.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" width="100%" style="display: block; margin: auto;" />


---&twocol
## FFTrees

***=left


- `FFTrees` An easy-to-use R package to create, visualize, and evaluate fast-and-frugal decision trees.



```r
install.packages("FFTrees")
library("FFTrees")
    a      
   / \     
0   b  
     / \   
    0   1  
 FFTrees v1.3.2
```



***=right


<img src="images/FFTrees_Logo.jpg" title="plot of chunk unnamed-chunk-11" alt="plot of chunk unnamed-chunk-11" width="80%" style="display: block; margin: auto;" />


---

## Example: Heart Disease


| age| sex|cp | trestbps| chol| fbs|restecg     | thalach| exang| oldpeak|slope | ca|thal   | diagnosis|
|---:|---:|:--|--------:|----:|---:|:-----------|-------:|-----:|-------:|:-----|--:|:------|---------:|
|  63|   1|ta |      145|  233|   1|hypertrophy |     150|     0|     2.3|down  |  0|fd     |         0|
|  67|   1|a  |      160|  286|   0|hypertrophy |     108|     1|     1.5|flat  |  3|normal |         1|
|  67|   1|a  |      120|  229|   0|hypertrophy |     129|     1|     2.6|flat  |  2|rd     |         1|
|  37|   1|np |      130|  250|   0|normal      |     187|     0|     3.5|down  |  0|normal |         0|
|  41|   0|aa |      130|  204|   0|hypertrophy |     172|     0|     1.4|up    |  0|normal |         0|
|  56|   1|aa |      120|  236|   0|normal      |     178|     0|     0.8|up    |  0|normal |         0|

- Goal: Predict diagnosis as a function of cues.
- Regression: 6 significant cues (sex, cp, thalach, exang, oldpeak, ca)


---

## 3 Steps to creating FFTs with FFTrees


```r
# Step 0: Install FFTrees (v.1.3.2)
install.packages("FFTrees")

# Step 1: Load the package
library("FFTrees")

# Step 2: Create an fft decision model with FFTrees
heart.fft <- FFTrees(formula = diagnosis ~.,
                     data = heart.train,
                     data.test = heart.test,
                     main = "Heart Disease",
                     decision.labels = c("Low-Risk", "High-Risk"))
```


---

## Print an `FFTrees` object


```r
heart.fft
```



```r
Heart Disease
7 FFTs predicting diagnosis (Low-Risk v High-Risk)
FFT #1 uses 3 cues: {thal,cp,ca}

                   train   test
cases       :n    150.00 153.00
speed       :mcu    1.74   1.73
frugality   :pci    0.88   0.88
accuracy    :acc    0.80   0.82
weighted    :wacc   0.80   0.82
sensitivity :sens   0.82   0.88
specificity :spec   0.79   0.76

pars: algorithm = 'ifan', goal = 'wacc', goal.chase = 'bacc', sens.w = 0.5
```

---
## Print a tree "in words"


```r
inwords(heart.fft)
```


```r
[1] "If thal = {rd,fd}, predict High-Risk"            
[2] "If cp != {a}, predict Low-Risk"  
[3] "If ca <= 0, predict Low-Risk, otherwise, if ca > 0, predict High-Risk"
```


| cue| definition|Possible values |
|:------|----:|:-----|
|     thal: thallium scintigraphy| How well blood flows to the heart     |normal (n)fixed defect (fd), or reversible defect (rd)|
|     cp: chest pain type| Type of chest pain     | typical angina (ta), atypical angina (aa), non-anginal pain (np), or asymptomatic (a).|
|     ca: |number of major vessels colored by flourosopy     |0, 1, 2, 3    |





---


```r
plot(heart.fft, stats = FALSE)
```

<img src="figure/unnamed-chunk-18-1.png" title="plot of chunk unnamed-chunk-18" alt="plot of chunk unnamed-chunk-18" width="60%" style="display: block; margin: auto;" />


---


```r
plot(heart.fft)  # Training data
```

<img src="figure/unnamed-chunk-19-1.png" title="plot of chunk unnamed-chunk-19" alt="plot of chunk unnamed-chunk-19" width="55%" style="display: block; margin: auto;" />

---


```r
plot(heart.fft, data = "test")   # Testing data
```

<img src="figure/unnamed-chunk-20-1.png" title="plot of chunk unnamed-chunk-20" alt="plot of chunk unnamed-chunk-20" width="55%" style="display: block; margin: auto;" />


---

<img src="images/roc.png" title="plot of chunk unnamed-chunk-21" alt="plot of chunk unnamed-chunk-21" width="80%" style="display: block; margin: auto;" />

---


```r
plot(heart.fft, data = "test", tree = 6)   # Testing data, tree 6
```

<img src="figure/unnamed-chunk-22-1.png" title="plot of chunk unnamed-chunk-22" alt="plot of chunk unnamed-chunk-22" width="55%" style="display: block; margin: auto;" />


---


```r
plot(heart.fft, data = "test", tree = 7)   # Testing data, tree 7
```

<img src="figure/unnamed-chunk-23-1.png" title="plot of chunk unnamed-chunk-23" alt="plot of chunk unnamed-chunk-23" width="55%" style="display: block; margin: auto;" />




---&twocol

***=left

## Generalizing FFTrees

- The `FFTrees` package can be used with any dataset with a binary criterion.
- Simulation: 10 diverse datasets taken from the UCI Machine Learning Database.
- FFTrees vs. regression, Naive Bayes, Rnd For and more

### How well can a simple fast-and-frugal tree predict data?  

***=right

<img src="images/datacollage.png" title="plot of chunk unnamed-chunk-24" alt="plot of chunk unnamed-chunk-24" width="90%" style="display: block; margin: auto;" />






--- .class #id 
## Speed and frugality


<img src="figure/unnamed-chunk-25-1.png" title="plot of chunk unnamed-chunk-25" alt="plot of chunk unnamed-chunk-25" width="60%" style="display: block; margin: auto;" />


--- .class #id 
## Speed and frugality


<img src="figure/unnamed-chunk-26-1.png" title="plot of chunk unnamed-chunk-26" alt="plot of chunk unnamed-chunk-26" width="60%" style="display: block; margin: auto;" />





--- .class #id 
## Prediction accuracy across 10 datasets

<img src="images/avgsim.jpg" title="plot of chunk unnamed-chunk-27" alt="plot of chunk unnamed-chunk-27" width="90%" style="display: block; margin: auto;" />


--- .class #id 

<img src="images/MLR_Simulation_Trees.jpg" title="plot of chunk unnamed-chunk-28" alt="plot of chunk unnamed-chunk-28" width="72%" style="display: block; margin: auto;" />



---
### Mushrooms
<img src="figure/unnamed-chunk-29-1.png" title="plot of chunk unnamed-chunk-29" alt="plot of chunk unnamed-chunk-29" width="55%" style="display: block; margin: auto;" />



---
### Breast Cancer
<img src="figure/unnamed-chunk-30-1.png" title="plot of chunk unnamed-chunk-30" alt="plot of chunk unnamed-chunk-30" width="55%" style="display: block; margin: auto;" />


---
### Titanic
<img src="figure/unnamed-chunk-31-1.png" title="plot of chunk unnamed-chunk-31" alt="plot of chunk unnamed-chunk-31" width="55%" style="display: block; margin: auto;" />




---

### Additional functions and arguments

<img src="images/FFTrees_Tutorial_SS.png" title="plot of chunk unnamed-chunk-32" alt="plot of chunk unnamed-chunk-32" width="90%" style="display: block; margin: auto;" />




---&twocol

***=left
### Define an FFT manually


```r
# Create an FFT manually
FFTrees(formula = diagnosis ~.,
data = heart.train,
my.tree = "If chol > 350, predict True. 
           If cp != {a}, predict False. 
           If age <= 35, predict False.
           Otherwise, predict True")
```




***=right

<img src="figure/unnamed-chunk-35-1.png" title="plot of chunk unnamed-chunk-35" alt="plot of chunk unnamed-chunk-35" width="100%" style="display: block; margin: auto;" />


---&twocol
### Include cue costs

***=left
`heart.cost`

|   |cue      |  cost|
|:--|:--------|-----:|
|2  |sex      |   1.0|
|13 |thal     | 102.9|
|3  |cp       |   1.0|
|12 |ca       | 100.9|
|4  |trestbps |   1.0|


```r
# Original FFT (without costs)
FFTrees(formula = diagnosis ~.,
 data = heart.train)
```


***=right

### Original FFT (without costs)

<img src="figure/unnamed-chunk-38-1.png" title="plot of chunk unnamed-chunk-38" alt="plot of chunk unnamed-chunk-38" width="100%" style="display: block; margin: auto;" />


---&twocol
### Include cue costs

***=left
`heart.cost`

|   |cue      |  cost|
|:--|:--------|-----:|
|2  |sex      |   1.0|
|13 |thal     | 102.9|
|3  |cp       |   1.0|
|12 |ca       | 100.9|
|4  |trestbps |   1.0|


```r
# Create an FFT that is cheap
FFTrees(formula = diagnosis ~.,
 data = heart.train,
 cost.cues = heart.cost,
 cost.outcomes = c(0, 200, 100, 0))
```


***=right



### Cheap FFT


<img src="figure/unnamed-chunk-42-1.png" title="plot of chunk unnamed-chunk-42" alt="plot of chunk unnamed-chunk-42" width="100%" style="display: block; margin: auto;" />


---
# Create a forest of FFTs


```r
heart.fff <- FFForest(formula = diagnosis ~., data = heartdisease)
```


<img src="figure/unnamed-chunk-44-1.png" title="plot of chunk unnamed-chunk-44" alt="plot of chunk unnamed-chunk-44" width="90%" style="display: block; margin: auto;" />


---&twocol

## Conclusion

***=left

### Why use FFTrees?

- See how, and how well, a crazy simple, transparent fast-and-frugal tree can make sense of your data.
- You might be surprised by how well it works, and generate new insights.


```r
library("FFTrees")
    a      
   / \     
0   b  
     / \   
    0   1  
 FFTrees v1.3.2
```

***=right


<img src="figure/unnamed-chunk-46-1.png" title="plot of chunk unnamed-chunk-46" alt="plot of chunk unnamed-chunk-46" width="80%" style="display: block; margin: auto;" />



---&twocol
## Publication and Collaborators

***=left

### Publication

Phillips, Nathaniel D., Neth, Hansjoerg, Woike, Jan & Gaissmeier, Wolfgang. (2017). FFTrees: A toolbox to create, visualize and evaluate fast-and-frugal decision trees. *Judgment and Decision Making*.

### Collaborators

- Wolfgang Gaissmaier (University of Konstanz)
- Hansjoerg Neth  (University of Konstanz)
- Jan Woike  (MPI for Human Development)

***=right

<img src="images/collaborators.png" title="plot of chunk unnamed-chunk-47" alt="plot of chunk unnamed-chunk-47" width="90%" style="display: block; margin: auto;" />




--- .class #id 

### FFTrees

- FFTrees: `install.packages("FFTrees")`, http://www.github.com/ndphillips/FFTrees

#### My Links

- This presentation: [https://ndphillips.github.io/useRJuly2017](https://ndphillips.github.io/useRJuly2017)
- Website: https://ndphillips.github.io
- Email: Nathaniel.D.Phillips.is@gmail.com







---&twocol

***=left

### FFTrees Unfriendly Data

- Many cues, weak validity, ind errors

<img src="figure/unnamed-chunk-48-1.png" title="plot of chunk unnamed-chunk-48" alt="plot of chunk unnamed-chunk-48" width="100%" style="display: block; margin: auto;" />


***=right

### FFTrees Friendly Data

- Few cues with high validity, dep errors.

<img src="figure/unnamed-chunk-49-1.png" title="plot of chunk unnamed-chunk-49" alt="plot of chunk unnamed-chunk-49" width="100%" style="display: block; margin: auto;" />






---&twocol

***=left
# Tree Building Algorithm


1. For each cue (aka, feature), calculate a threshold that maximizes `goal.chase` (default: balanced accuracy)
2. Rank order cues by `goal.chase`
3. Select the top `max.levels` (default: 4)
4. Create a "fan" of all possible trees with all possible exit directions.
5. Select the tree that maximizes `goal` (default: balanced accuracy)

***=right

<img src="figure/unnamed-chunk-50-1.png" title="plot of chunk unnamed-chunk-50" alt="plot of chunk unnamed-chunk-50" width="80%" style="display: block; margin: auto;" />


<img src="images/roc.jpg" title="plot of chunk unnamed-chunk-51" alt="plot of chunk unnamed-chunk-51" width="70%" style="display: block; margin: auto;" />



---


```r
plot(heart.fft, what = "cues")
```

<img src="figure/unnamed-chunk-52-1.png" title="plot of chunk unnamed-chunk-52" alt="plot of chunk unnamed-chunk-52" width="60%" style="display: block; margin: auto;" />



---

### When should I consider an FFT?

<img src="images/considerFFT.jpg" title="plot of chunk unnamed-chunk-53" alt="plot of chunk unnamed-chunk-53" width="70%" style="display: block; margin: auto;" />





---
### Conclusion



<img src="figure/unnamed-chunk-54-1.png" title="plot of chunk unnamed-chunk-54" alt="plot of chunk unnamed-chunk-54" width="90%" style="display: block; margin: auto;" />


--- .class #id 

### Conclusion



<img src="figure/unnamed-chunk-55-1.png" title="plot of chunk unnamed-chunk-55" alt="plot of chunk unnamed-chunk-55" width="90%" style="display: block; margin: auto;" />






--- .class #id 

### Conclusion



<img src="figure/unnamed-chunk-56-1.png" title="plot of chunk unnamed-chunk-56" alt="plot of chunk unnamed-chunk-56" width="90%" style="display: block; margin: auto;" />



---
### How accurate can FFTs be?



<img src="figure/unnamed-chunk-57-1.png" title="plot of chunk unnamed-chunk-57" alt="plot of chunk unnamed-chunk-57" width="90%" style="display: block; margin: auto;" />



---
### How accurate can FFTs be?




<img src="figure/unnamed-chunk-58-1.png" title="plot of chunk unnamed-chunk-58" alt="plot of chunk unnamed-chunk-58" width="90%" style="display: block; margin: auto;" />



---
### Forest Fires (Training)

<img src="figure/unnamed-chunk-59-1.png" title="plot of chunk unnamed-chunk-59" alt="plot of chunk unnamed-chunk-59" width="55%" style="display: block; margin: auto;" />

---
### Forest Fires (Testing)

<img src="figure/unnamed-chunk-60-1.png" title="plot of chunk unnamed-chunk-60" alt="plot of chunk unnamed-chunk-60" width="55%" style="display: block; margin: auto;" />


---
### Forest Fires (Testing)

<img src="figure/unnamed-chunk-61-1.png" title="plot of chunk unnamed-chunk-61" alt="plot of chunk unnamed-chunk-61" width="55%" style="display: block; margin: auto;" />


