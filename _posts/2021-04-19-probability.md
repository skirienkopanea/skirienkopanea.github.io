---
layout: post
title:  "Probability Theory and Statistics Summary"
date:   2021-04-19 00:51:00 +0200
categories: math
tags: CSE1210
---
{% include math.html %}
<!--more-->

# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Terminology](#terminology)
    - [Probability vs Statistics](#probability-vs-statistics)
  - [Numerical summaries](#numerical-summaries)
  - [Five-number summary](#five-number-summary)
  - [Graphical summaries](#graphical-summaries)
  - [Set theory](#set-theory)
    - [Tricks](#tricks)
    - [DeMorgan's Laws](#demorgans-laws)
  - [Probability](#probability)
    - [Sample spaces](#sample-spaces)
    - [Probability function for finite sample spaces](#probability-function-for-finite-sample-spaces)
      - [Probability function for 1 success after n trials](#probability-function-for-1-success-after-n-trials)
    - [Additivity property](#additivity-property)
    - [Product property](#product-property)
    - [Probability function for infinite sample spaces](#probability-function-for-infinite-sample-spaces)
  - [Conditional probability and independence](#conditional-probability-and-independence)
    - [Conditional probability](#conditional-probability)
    - [Multiplication rule](#multiplication-rule)
      - [Birthday example:](#birthday-example)
    - [Bayes rule (Librarian vs farmer)](#bayes-rule-librarian-vs-farmer)
      - [Law of total probability](#law-of-total-probability)
  - [Independence](#independence)
    - [Independence of two or more events](#independence-of-two-or-more-events)
  - [Discrete random variables](#discrete-random-variables)
    - [Probability distribution of a discrete random variable](#probability-distribution-of-a-discrete-random-variable)
      - [Discrete probability distribution of X aka mass function of X](#discrete-probability-distribution-of-x-aka-mass-function-of-x)
      - [(Cumulative) Distribution function of X](#cumulative-distribution-function-of-x)
    - [Expectation of discrete random variables](#expectation-of-discrete-random-variables)
    - [Variance of discrete random variables](#variance-of-discrete-random-variables)
    - [Combinatorics](#combinatorics)
    - [Binomial distribution](#binomial-distribution)
    - [Bernoulli distribution](#bernoulli-distribution)
    - [Geometric distribution](#geometric-distribution)
    - [Poisson distribution](#poisson-distribution)
      - [Relationship between binomial and possion distributions](#relationship-between-binomial-and-possion-distributions)
  - [Continuous random variables](#continuous-random-variables)
    - [Arbitrary example](#arbitrary-example)
    - [Expectation of continous random variables](#expectation-of-continous-random-variables)
    - [Variance of continuous random variables](#variance-of-continuous-random-variables)
    - [Quantiles (percentiles)](#quantiles-percentiles)
      - [Median](#median)
      - [kth percentile](#kth-percentile)
    - [Probability density functions](#probability-density-functions)
    - [Cumulative distribution function](#cumulative-distribution-function)
    - [Normal distribution \\(N(\mu,\sigma^2)\\)](#normal-distribution-nmusigma2)
    - [Standard normal distribution \\(N(0,1)\\)](#standard-normal-distribution-n01)
    - [Uniform distribution](#uniform-distribution)
    - [Exponential distribution](#exponential-distribution)
    - [Pareto distribution](#pareto-distribution)
    - [Change of variable](#change-of-variable)
      - [Standardizing Normally Distributed Random Variables](#standardizing-normally-distributed-random-variables)

## Terminology
* stochastic: having a random probability distribution or pattern that may be analysed statistically but may not be predicted precisely.
* An experiment is an activity or procedure that produces distinct (single) outcomes.
  * An event is a set that describes 1 or more outcomes. I.e. throw an odd dice number, throw number 3, etc. It is a subset of the sample space.
  * The set of all outcomes is the *sample space* \\(\Omega\\)
  * \\(Outcome \in Event \in \Omega\\)

### Probability vs Statistics
* **Probability**: Given a explicitly defined model, predict the outcome (accurate).
  * Define the probability of events and their distribution, frequency, relation.
* **Statistics**: Given the observed data, fit a model (estimation).
  * Describe the *observed* data (descriptive statistics)
  * Estimate parameters
  * Test hypotheses

## Numerical summaries
* Average (also known as mean) = \\(\mu\\)
  * We have to do the weighted average when the probabilities are different for each number
* Variance = (weighted) squared average of the difference between a number and the mean =\\(\sigma^2=(x-\mu)^2\\)
* Standard deviation = \\(\sigma\=\sqrt{\sigma^2}\\)
* Median (middle number/interval of the distinct set, not necessarily the same as the average)
* Minimum and maximum value (range)
* Lower and upper quartile
  * They are the 25th and 75th percentiles
  * That is, the value of x such that the area under the curve between \\(-\infty\\) and the percentile amounts to .25 and .75 respectievely

## Five-number summary
* Minimum
* Lower quartile (median of the set from the minimum to the median)
* Median (50th percentile)
* Upper quartile (median of the set from the median to the maximum)
* Maximum

## Graphical summaries
* Histogram
  * Displays data in intervals, which are determined by a "bin" width
  * Bin width is mostly arbitrary, but there are "sweet spots" that show more useful information than others

![histogram]({{ site.url }}/images/histogram_width.PNG)

* Two peaks indicate that there are likely two different types of eruption
* Scatter plots show tupple observations, often the x-axis is inteded to represent the independent variable (input, *cause*) and the y-axis the dependent variable (output).

![scatterplot]({{ site.url }}/images/scatterplot.PNG)

* However scatter plots can be deceiving. The two scatter plots look similar a priory. But if we use boxplots we can easily see the difference.

![boxplot]({{ site.url }}/images/boxplot.PNG)

* A boxplot is a visual simplified representation of the data distribution (rotated 90 degrees) 

![boxplot]({{ site.url }}/images/boxplot2.PNG)

* IQR stands for interquartile range, which is the distance between the upper and lower quartiles. This is the range of the middle half of the dataset.

## Set theory
* A set is a collection of objects
  * The members of the sets are called elements
  * \\(Tesla \in Car\ Manufacturers\\)
    * Tesla is an element of Car Manufacturers (Tesla is in the Car manufacturers set)
* Empty set: it has no elements in it \\(\emptyset\\)
* \\(\\{Tesla,SpaceX\\} \subset \\{Elon\ Musk\ companies\\}\\)
  * All subsets in this course are improper, so \\(A\subset A\\) is true for any set
  * When A and B are considered **events**, \\(A\subset B\\) is also read as A implies B
* Intersecttion: \\(\cap\\)
  * Two sets whose intersection is the empty set are called **disjoint (or mutually exclusive)**
* Union: \\(\cup\\)
* Universe set U: The set that contains evertyhing in the context of the problem
* Complement \\(^c\\) i.e if the set of all "computer" colors (universe set) is {R,G,B}, then \\(\\{R\\}^c=\\{G,B\\}\\)
* \\(U^c=\emptyset\\)
* Difference: \\(A\setminus B=A\cap B^c\\)

### Tricks
* \\(A=A\cap(B\cup B^c)=(A\cap B)\cup(A\cap B^c)\\)
  * \\(P(A)=P(A\cap B) + P(A\cap B^c)\\)
* \\((A\cup B)\cap B=B\\)
* \\((A\cup B)\cap B^c=A\cap B^c\\)
  * \\(((A\cup B)\cap B)\cup((A\cup B)\cap B^c)=B \cup A\cap B^c\\)
  * \\((A\cup B)\cap (B\cup B^c)=B \cup A\cap B^c\\)
  * \\((A\cup B)=B \cup A\cap B^c\\)
  * \\(P(A\cup B)=P(B) + P(A\cap B^c)\\)
* \\(P(A\cup B)-P(B)=P(A\cap B^c)\\)
* \\(P(A)-P(A\cap B)=P(A\cap B^c)\\)
  * \\(P(A\cup B)-P(B)=P(A)-P(A\cap B)\\)
  * \\(P(A\cup B)=P(A)+P(B)-P(A\cap B)\\)

* \\(P(A\cup A^c)=1\\)
* \\(P(A^c)=1-P(A)\\)

### DeMorgan's Laws
* The complement is the same as the negation, therefore
  * \\(\overline{A\cup B}=A^c \cap B^c\\)
  * \\(\overline{A\cap B}=A^c \cup B^c\\)

## Probability
* **Random** or unpredictable phenomena can be **modelled** (or better say, let a model be the platonic representation of a random  event with defined probabilities), such that this phenomena is regarded as **outcomes** of some **experiment** (not in the scientific method sence, but in the trial colloquial sense).
  * All the outcomes are elements of a **sample space** \\(\Omega\\)
  * Subsets of \\(\Omega\\) are called **events**
    * events have an assigned **probability** (between 0.0 and 1.0)

### Sample spaces
Coin flip: \\(\Omega = \\{Head,Tail\\}\\), has a size of 2 elements
* The size of a sample space that regards all the different possible sortings of a set of elements (i.e. row orders in an augmented matrix) is called a permutation (of n objects) and the total number of possible permutations is n! (factorial), because as we add the nth object, then that nth object can be placed in any of the n-1 permutations, in n different positions, that is, \\(n! = (n-1)\cdot n\\).
* We define \\(0!=1\\)
* **Events** are defined as elements of the **experiment outcome** set (sample space and Universe set in this context).
* Thes events can be combined according to set theory rules.
* **The empty set** is also regarded as "the impossible event".

### Probability function for finite sample spaces
* By defintion, each event must have a probability. An event whose probability is 0 means that it is not in the sample space (not a possible experiment outcome).
* A probability function P on a finite sample space \\(\Omega\\) assigns to each event A in \\(\Omega\\) a number P(A) in [0,1] such that \\(P(\Omega)=1\\) and P(A or B) = P(B) + P(A) iff A and B are disjoint.
  * Additivity of P implies that the probability of an event is obtained
by summing the probabilities of the outcomes belonging to the event.
* for the probability of a head **outcome** we cant write P(Head) as Head is technically just an element of a set, but not a set itself. Only events are assigned probabilities, therefore we use the event with a single outcome {Head} to denote that P({Head})=0.5
* But in practice it's okay to drop the brackets for single element sets
* For simplicity, we also let all months have equal probability 1/12 regardless of the fact that some are longer than others.

#### Probability function for 1 success after n trials
* It is the product of all the failures (1-p)^n and the single success p such that:

$$P(n)=(1-p)^np$$

### Additivity property
* \\(P(A\cap B)=P(A)+P(B)-P(A\cup B)\\)
* \\(P(A\cup B)=P(A)+P(B)-P(A\cap B)\\)
  * \\(P(A)=P(A\cap B) + P(A\cap B^c)\\)
  * \\(P(A\cup B)=P(B) + P(A\cap B^c)=P(A)+P(B \cap A^c)\\)
* \\(P(A\cup A^c)=1\\)
* \\(P(A^c)=1-P(A)\\)

### Product property
* The sample space of two successive coin flips is the cross product of \\(\Omega=\\{H,T\\}\times\\{H,T\\}=\\{(H,H),(H,T),(T,H),(T,T)\\}\\)
* if we consider two experiments with sample spaces \\(\Omega_1\\) and \\(\Omega_2\\) then the combined experiment has as its sample space the set \\(\Omega=\Omega_1 \times \Omega_2\\)
* The probability of (H,H) would be P(H) * P(H) for {H,T} as long as we agree that each successive throw is independent from the other and does not influence each other in any way.

### Probability function for infinite sample spaces
* It is the same only that \\(P(A_1 \cup A_2 \cup \dots) = P(A_1) + P(A_2) + \dots\\) with A being disjoint events.
* \\(P(\Omega)\\) is the sum of all the individual probabilities of 1 success after n trials, which should be 1, and it's proven with a calculus geometric series (like \\(\sum{\left(\frac{1}{2}\right)^n}=1\\), in binary 0.11111111...) such that:

$$P(\Omega)=\sum^\infty_{n=1}{\left((1-p)^{n-1}\cdot p\right)}=\frac{p}{1-p}\cdot\sum^\infty_{n=1}{\left((1-p)^{n}\right)}$$

* For \\(\sum^\infty_{n=1}{\left((1-p)^{n}\right)}\\) we have r = (1-p), therefore the sum equals \\(a_1\cdot\frac{1}{1-r}=\frac{1-p}{1-(1-p)}=\frac{1-p}{p}\\)

* $$P(\Omega)=\frac{p}{1-p}\cdot\frac{1-p}{p}=1$$

## Conditional probability and independence
The Monty Hall dilema shows how knowing an event has occured forces us to reasses the probability of another event. The new probability is known as the **conditional probability**. However if the new probability remains the same, then these events are **independent**.

For R = {januaRy, febRuaRy, maRch, apRil, septembeR, octobeR, novembeR, decembeR} and L ={january, march, may, july, august, october, december} with probabilities of P(R)=8/12 and P(L)=7/12. If we try to guess the birth month of a person at random (1/12 success) and we're told that it's a long month, then the **conditional probability of R given L**, written as P(R\|L) is not to be confused with \\(P(R\cap L)\\), which is 4/12. P(R\|L) is actually 4/7 as we no longer are we gonna guess from the entire pool, but from those months in L. Therefore, provided that P(B)>0:

### Conditional probability

$$P(A| B)=\frac{P(A\cap B)}{P(B)}$$

### Multiplication rule
* From the conditional probability formula, if we rewrite it we get:

$$P(A\cap B)=P(A| B)\cdot P(B) = P(B|A)\cdot P(A)$$

* Usually it is easier to compute P(B) and P(A\|B) separately than \\(P(A\cap B)\\)

#### Birthday example:

* Calculate the probability \\(P(B_n)\\) there are no coincident birthdays. We can split this event into two:
  * \\(P(B_{n-1})\\) is the event of no coincident birthdays among the n-1 persons
  * \\(P(A_n)\\) is the event of the birthday of the nth person, not coinciding with the birthday of any of the first n-1 perosns.
* Therefore:

$$P(B_n)=P(A_n\cap B_{n-1})=P(A_n| B_{n-1})\cdot P(B_{n-1})=\left(1-\frac{n-1}{365}\right)\cdot P(B_{n-1})$$

$$= \left(1-\frac{n-1}{365}\right)\cdot\left(1-\frac{n-2}{365}\right)\cdot\left(1-\frac{n-3}{365}\right)\dots\left(1-\frac{1}{365}\right)$$

* With \\(P(B_2)=1-\frac{1}{365}\\), which is the probability that the birthd day of the second person is different to the birthday of the first person. The birth day of the first person being different to the birth day of all **other** persons has a probability of 1, as such person is the only one in the pool, and also 1-0/365=0

### Bayes rule (Librarian vs farmer)
* Assume that 1% of the people are accountants, and thant 100% of accountants use a computer at work
* Also assume that from all the people that is not an accountant, 50% of those people use a computer at work
* The question is, if we know that our guy uses a computer at work, what's the probatility that our guy is an accountant?
* \\(P(Accountant \| Uses\ computer)=P(H\|E)\\)
  * Probability that the hypothesis is true, given an event
* \\(P(H)=P(Accountant)=1\%\\)
* \\(P(E\|H)\\) is the "likelihood": given that the person is an acountant, what is the probability that uses a computer at work. In this case 100%.
* \\(P(E\|H^c)\\) is the probability of the event being true (uses a computer) despite the hypothesis being false (not an accountant). In this case 50%.
* \\(P(H\|E)=\\) probability of being an accountant given our (here rather trivial) information that 
  * \\(P(Accountant\|UsesComputer)=\frac{Accountant \cap UsesComputer}{UsesComputer}=\\)
  * \\(\frac{P(Accountant)P(UsesComputer\|Accountant)}{P(Accountant)P(UsesComputer\|Accountant)+P(\lnot Accountant)P(UsesComputer\|\lnot Accountant)}\\)
* $$P(H|E)=\frac{P(H)P(E|H)}{P(H)P(E|H)+P(H^c)P(E|H^c)}=\frac{P(H\cap E)}{P(E)}$$
  * P(E) could be expressed instead of in just the 2 disjoint events of they hypotheses and the hypotheses complement, it could be expressed in terms of many other disjoint events as the law of total probability suggests.

#### Law of total probability
![Total Probability]({{ site.url }}/images/total_p.PNG)
* For \\(C_1, C_2...\\) disjoint events such that \\(C_1 \cup C_2 \cup ...=\Omega\\) :
* $$P(A)=P(A|C_1)P(C_1)+P(A|C_2)P(C_2)+\dots$$

## Independence
* Physical independence: When 2 events are implicitly independent due to their physical nature (i.e. tossing a coin after withdrawing a card)
* To show that A and B are (stochastically) independent it suffices to prove just one of the following:
  * \\(P(A\|B)=P(A)\\)
  * \\(P(B\|A)=P(B)\\)
  * \\(P(A\cap B)=P(A)P(B)\\)

### Independence of two or more events
* A and B being independent and B and C being independent and C and A being indepdendent does NOT imply \\(A \cap B \cap C\\)are indepdendent. It has to be strictly proven that Events \\(A_1, A_2,\dots\\) are independent iff:
  * \\(P(A_1 \cap A_2 \cap \dots ) = P(A_1)P(A_2)...\\)
    * It also holdes if you rpelace an event for its complement

## Discrete random variables
* Random variabels emerge from the process of mapping random processes (i.e. rolling two dice) to numbers (their sum)

$$S=sum\ of\ 2\ dice = \begin{cases}
               2 & if\ (1,1)\\
               3 & if\ (1,2)\\
               4 & if\ (1,3)\\
               \vdots & \vdots\\
               12 & if\ (6,6)
            \end{cases}
$$

* Random variables are denoted by capital leters
  * tossing a coin maps to X: \\({(H,T)} \mapsto X\\), where X is a random variable that is an integer (its no longer H or T)


$$X=\begin{cases}
               1 & if\ heads\\
               0 & if\ tails\\
            \end{cases}
$$

* The value choices for 1 and 0 are arbitrary, any values would still make X a random variable
* Random variables are not like the traditional variables in algebra such as y = 5x
  * You can assign values and/or solve for traditional variables (or unkowns)
  * A random variable does not have a fixed value, it can take many diferent values each time we re-run the experiment, with different probabilities.
    * Therefore random variables are more useful when we use them in terms of the probability of the random variable being equal to something. That probability is a fixed number.
    * \\(P(Y\le 6)\\)

* Discrete random variabels are those that can take "int" values, 1, 2, 3, 4, etc. Such as number of facebook friends. Whereas continuous random variables can take infinitue values between those "int" values, such as the 100m race time of an athlete (without rounding).

### Probability distribution of a discrete random variable
* Example, let X = "number of heads after 3 coin toss"
* $$\Omega=\left\{\begin{matrix}(H,H,H), \\ (H,H,T), \\ (H,T,H), \\ (H,T,T), \\ (T,H,H), \\ (T,H,T), \\ (T,T,H), \\ (T,T,T)\end{matrix}\right\}$$
* \\(P(X=0)=\\{(T,T,T)\\}.size()/\Omega.size()=1/8\\)
* \\(P(X=1)=3/8\\)
* \\(P(X=2)=3/8\\)
* \\(P(X=3)=1/8\\)

#### Discrete probability distribution of X aka mass function of X
* Also known as probability mass function p. p is the function that maps R to the interval [0,1]:
* \\(p(a)=P(X=a)\\)

![pf]({{ site.url }}/images/pf.png)

* The beauty is that we don't need to know much about X nor \\(\Omega\\) in order to use inputs for p(a).
* The allowed inputs are \\(-\infty < a < \infty\\)
* Before we were forced to describe all the events in set theory, such as \\(P(\\{(T,T,H)\\})\\), now we can just say p(1). We can also use P(X<2), which would be equal to P(X=0) + P(X=1) = 1/8 + 3/8 = 1/2. This is known as the distribution function.

#### (Cumulative) Distribution function of X 
* The distribution function F of a random variable X is similar to the mass function, but instead we map the cummulative probability of being smaller than the input:
* \\(F(a)=P(X\le a)=\sum_{a_i\le a}{p(a_i)}\\) for \\(-\infty < a < \infty\\)

![df]({{ site.url }}/images/df.png)

* The probablity when X = a does exist, the value taken dependes on wheter the inequality includes equal or not. The white line is just for the \\(aesthetics\\)
* As X approaches -\\(\infty\\) the value goes to 0 and as it approaches \\(\infty\\) the value goes to 1.

### Expectation of discrete random variables
* The expected value or expectation of a random variable is the theoretical mean of the random variable (it's not based on sampled data). It can be based on the individual probabilities of each event or on a function g(X) that returns the probabilities for each event of X.
* As we approach the law of large numbers, the probability that the average outcome is equal to the **expectation** is very high.
* \\(E(X)=\mu\\) = The weighted average of the probability distribution (probability mass function):
  * $$E(X)=\sum_{i=1}^{n}{(x_i\cdot p(x_i))}$$
* \\(E(g(X))=E[g(X)]=\mu\\) = also weighted average of the probability distribution of g(x): 
  * $$(E[g(X)])=\sum_{i=1}^{n}{(g(x_i)\cdot p(x_i))}$$
* \\(E[f(X)]\\) = The weighted average of f(X) with \\(p(x_i)\\) as weights and \\(f(x_i)\\) as values.

### Variance of discrete random variables
* This is also the theoritical variance and it's not drawn from sample data.
* As we approach teh law of large numbers, we will observe that the the observed variance approaches this theoretical one?
* $$\sigma^2=E\left[(X-\mu)^2\right]=\sum_{i=1}^{n}{((x_i-\mu)^2\cdot p(x_i))}$$
  * $$=\sum_{i=1}^{n}{((x_i^2-2x_i\mu+\mu^2)p(x_i))}$$
  * $$=\sum_{i=1}^{n}{(x_i^2\cdot p(x_i))}-\sum_{i=1}^{n}{((2x_i\mu-\mu^2)p(x_i))}$$
  * $$=E[X^2]-\sum_{i=1}^{n}{(2\mu\cdot x_ip(x_i)-\mu^2p(x_i))}$$
    * It's easier to prove with the E[X] operator from here, but without it it would be like:
  * $$=E[X^2]-\sum_{i=1}^{n}{(\frac{2\mu^2}{n}-\mu^2p(x_i))}$$
  * $$=E[X^2]-\mu^2\sum_{i=1}^{n}{(\frac{2}{n}-p(x_i))}$$
  * $$=E[X^2]-\mu^2\left(\sum_{i=1}^{n}{\frac{2}{n}}-\sum_{i=1}^{n}{p(x_i)}\right)$$
  * $$=E[X^2]-\mu^2(2-1)=E[X^2]-\mu^2$$
  * $$=E[X^2]-E[X]^2$$

### Combinatorics
* $$\begin{pmatrix}n \\ k \end{pmatrix}=nCk=\frac{n!}{k!(n-k)!}$$ is read as n choose k. n being the number of tosses and k the number of successes.

![btree]({{ site.url }}/images/btree.png)

* $$\begin{pmatrix}n \\k \end{pmatrix}$$ this binomial coeffecient calculates the count of scenarios with desired k success of a tree of n layers.

### Binomial distribution
* $$P(X=k)=\begin{pmatrix}n \\k \end{pmatrix}p^k(1-p)^{n-k}$$
* E[X]=np
* Var(X)=np(1-p)
* Since trials are independent, the individual probability for each counted scenarios to occur is \\(p^k\\), but we have nCk of them, therefore we multiply them with the binomial cofficient.
* In addition we have to complete the tree with the joint (independent) probability of the remaining failures, which are \\((1-p)^{n-k}\\)
* Formally the probability mass function is \\(p(x)=...\\)
* A binomial distribution is a discrete version of the normal distribution
  * The distribution becomes normal as the number of trials increases.
* This is a bernoulli distribution of n independent trials

![bd]({{ site.url }}/images/bd.png)
### Bernoulli distribution
* P(X=1) = p, P(X=0)-1-p
* E[X]=p
* Var(X)=p(1-p)
* A bernoulli distribution is a binomial distribution that only has X=0 and X=1 bars from 1 trial.

### Geometric distribution
* Binomial distribution whose probability mass function is given by:
* $$P(X=k)=(1-p)^{k-1}p$$
* E[X]=1/p
* Var(X)=\\((1-p)/p^2\\)
* We use this to compute the probability of a success at the kth trial

![gd]({{ site.url }}/images/gd.png)

* We can make a cummulative distribution of the geometric one to get the probability success by at least the nth trial.

![gcd]({{ site.url }}/images/gcd.png)

### Poisson distribution
* Counts the number of occurences (actually the probability of happening) of an event genearlly in a given unit of time (or distance, area, volume...)
* Events occur randomly and independently
  * (Homogeneity) The rate λ at which arrivals occur is constant over time: in a subinterval of length u the expectation of the number of telephone calls is λu.
  * (Independence) The numbers of arrivals in disjoint time intervals are independent
random variables.
* $$P(X=k)=\frac{\lambda^ke^{-\lambda}}{k!}$$
  * \\(\lambda=\mu=mean\\)
  * for the poisson distribution \\(\mu=\sigma^2=\lambda\\)

![pd]({{ site.url }}/images/pd.png)

* As lambda approaches 0 the right skewness of the poisson distribution becomes more noticeable

#### Relationship between binomial and possion distributions
* The binomial distribution tends toward the possion distribution as \\(n\to \infty, p \to 0\\) and \\(np\\) stays constant.
* The poisson distribution with \\(\lambda=np\\) closely approximates the binomial distribution if \\(n\\) is large and \\(p\\) is small

## Continuous random variables
* Their possible values constitute of an entire range (with infinite possible values in between), such as height, weight, time, etc.
* Probabilities and percentiles are found by integrating the probability density function
* Deriving the mean and variance also requires integration

### Arbitrary example
* Suppose for a random variable X that the probability density (mass) function follows \\(f(x)=cx^3\\) for \\(2\le x \le 4\\) and 0 otherwise.
* What value of c makes this a legit distribution?
  * \\(f(X)=cx^3\\) must be positive at all times, therefore c must be positive
  * \\(\int^{\infty}_{-\infty}f(x)dx=1\\)
 * $$\int^{4}_{2}cx^3dx=1$$
 * $$\frac{1}{4}c\left[x^4\right]^4_2=1$$
 * $$\frac{1}{4}c[4^4-4^2]=1$$
 * $$60c=1$$
 * $$c=\frac{1}{60}$$

### Expectation of continous random variables
* For discrete variables was \\(E(X)=\sum_{i=1}^{n}{(x_i\cdot p(x_i))}\\). If we do the Riemman summ of \\(\Delta x\\) and \\(n\to \infty \\) then we end up with the integral such that:
* $$E[X]=\mu=\int^{\infty}_{-\infty}{(xp(x))dx}$$
  * Only for the interval where the \\(p(X=x)\ge 0\\), where p(x) is the density function of X. This remains a weighted average.
  * E[X] is also called the expectation of X and the mass center of gravity of f(x)

### Variance of continuous random variables
* $$\sigma^2=E\left[(X-\mu)^2\right]=\sum_{i=1}^{n}{((x_i-\mu)^2\cdot p(x_i))}$$
* $$=\int_{-\infty}^{\infty}{((x-\mu)^2\cdot p(x))dx}$$
* Or better \\(\sigma^2=E[X^2]-E[X]^2\\)
 * $$=\sum_{i=1}^{n}{(x_i^2\cdot p(x_i))}-\mu^2$$
 * $$=\int_{-\infty}^{\infty}{(x^2\cdot p(x))dx}-\mu^2$$

### Quantiles (percentiles)
#### Median
* It's the X value that splits the area into two halves of equal size (0.5 each)
![median]({{ site.url }}/images/median.png)
#### kth percentile
* It's the value of x such that the area under the curve between \\(-\infty\\) and k amounts to k%

### Probability density functions
* $$P(a\le X \le b)=\int^{b}_{a}{f(x)}dx$$
* \\(P(a\le X \le b)=P(a\lt X \lt b)\\) because \\(P(X=a)=0\\)
* \\(f(x)\ge0\\)
* Area of entire curve is 1

### Cumulative distribution function
* $$P(X \le t)=\int^{t}_{-\infty}{f(x)}dx$$

### Normal distribution \\(N(\mu,\sigma^2)\\)
* E[X]=\\(\mu\\)
* Var(X)=\\(\sigma^2\\)
* $$f(x)=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{1}{2}(\frac{x-\mu}{\sigma})^2}$$

![nd]({{ site.url }}/images/nd.png)

### Standard normal distribution \\(N(0,1)\\)
* E[X]=0
* Var(X)=1
* How to convert

### Uniform distribution
* $$f(x)=\frac{1}{\beta-\alpha}$$ for \\(\alpha \le x \le \beta\\)
* E[X]=\\(\frac{1}{2}(\alpha+\beta)\\)
* Var(X)=\\(\frac{1}{12}(\beta-\alpha)^2)

![ud]({{ site.url }}/images/ud.png)

### Exponential distribution
* $$f(x)=\lambda e^{-\lambda x}$$
* $$F(X)=1-e^{-\lambda x}$$
* E[X]=\\(\frac{1}{\lambda}\\)
* Var(X)=\\(\frac{1}{\lambda^2}\\)

![ed]({{ site.url }}/images/ed.png)

### Pareto distribution
* \\(f(x)=0\\) for \\(x \lt 1\\) and \\(f(x)=\frac{\alpha}{x^{\alpha+1}}, F(X)=1-x^{-\alpha}\\) for \\(x \ge 1\\).
* E[X]=\\(\frac{\alpha}{\alpha-1}\\)
* Var(X)=\\(\frac{\alpha}{(\alpha-1)^2(\alpha-2)}\\)

![pareto]({{ site.url }}/images/pareto.png)

### Change of variable
* $$P(X^2\le a)=P(X\le \sqrt{a})$$
* $$(E[g(X)])=\sum_{i=1}^{n}{(g(x_i)\cdot p(x_i))}$$
* $$(E[g(X)])=\int_{-\infty}^{\infty}{(g(x)p(x))}$$
* \\(E[rX+s]=rE[X]+s\\)
* \\(Var(rX+s)=r^2Var(X)\\)

#### Standardizing Normally Distributed Random Variables
* Suppose X is a normally distributed random variable with mean \\(\mu\\) and standard deviation \\(\sigma\\).
  * \\(X\sim N(\mu,\sigma^2)\\)
  * Standard distribution has \\(\mu=0, \sigma^2=1\\)
* $$E(\frac{X-\mu}{\sigma})=\frac{E(X)-\mu}{\sigma}=\frac{0}{\sigma}=0$$
* $$Var(\frac{X-\mu}{\sigma})=\frac{1}{\sigma^2}Var(X-\mu)=\frac{\sigma^2}{\sigma^2}=1$$
* \\(Z=\frac{X-\mu}{\sigma}\\) is normally distributed such as \\(Z\sim N(0,1)\\)


![std]({{ site.url }}/images/std.png)

![rtpstd]({{ site.url }}/images/rtpstd.png)