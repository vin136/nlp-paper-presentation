What ?

This paper is trying to study the effect of inductive biases imposed by various design choices of a Transformer model to solve compositional tasks.

Goal Of the presentation:

Understand what do we mean by 

a. Compositional tasks

b.Inductive bias

c. Transformer and the design choices we have

d. Wrap it by appreciating the Empirical results.


## What is Composition ?


Composition (Intuitive discussion)

The models we build consist of simple primitives and the rules for combining them. We would want our machine learning models to generalize via similar means. Below I offer few different ways to look at it

- A great deal of complex phenomenon in the world can be explained via simple rules.
- (slightly different possibly controversial view)We want to build intelligent systems that can understand the artifacts we have built or discovered - Natural language, Mathematics, Natural Signals(Sound,Images etc) - Our brains can only see compositionally(energy and physical constraints) - thus we only discover the aspects of the world that are amenable to simple explanations - Underlying reality might remain opaque.

eg: 
a. Consider the Software we build.

Interoperability and compositionality(more clearly visible in Functional languages like Haskell)
eg: Compositionality is at the heart of UNIX
cat \var\log\nginx\access.log | awk '{print $7}' | sort | uniq -c | sort -r -n | head -n 5
We have managed to bulid complex software as we can seamlessly use other peoples code to make our own.

b.Consider the Math we discovered.

Linear Algebra -> Start with defining the axioms of a vector space -> Rest all (the notion of spannig sets,independence, linear mappings,change of basis etc) can be seen as implications. (Thus under right inductive bias the code length of Linear Algebra will be much smaller than a 300 page-college textbook).
Note: This is true for all of mathematics, people lately discovered this and is studying under the area called CATEGORY THEORY.

Composition (Practically Speaking)

**Task** We want to measure and build models that generalize via composition ?

Remember in the end we need concrete and repeatable tests to benchmark our progress.

Wait but how would you quantify this ?

Here are 5 proxies to encode and measure compositionality .

**Warning**: 

1.we do not yet have a consensus and these are just heuristics that the community seems to have agreed to live with, for now.

2. Some of these do not correctly represent how we parse natural language,neverthless they are still useful.


 ## Systematicity:
 
 Definition:
 
 “[t]he  ability  to  produce/understand  some  sentences  is  intrinsically  connected  to  the ability to produce/understand certain others” (Fodor and Pylyshyn, 1988, p.  25)" - How it's defined by original authors who introduced it.
 
In Action:

 someone who understand ‘brown dog’ and ‘black cat’ also understands ‘brown cat’.

Critique:

According to definition if I use a dictionary to store the meanings of all sentences, I would be able to understand any variation of the original sentence -> NO Systematicity
But by the same logic any out-of-distribution generalization can be attributed to Systematicity. -> So it's vacuous.
 
 How to test/quantify:
 
 So we should only test if the rules the model learned are in line with the rules we have in mind.
 
 
  ## Productivity:
  
 Definition:
 
 'language makes infinite use of finite means’ - Naom Chomsky
 Both Systematicity and Productivity seems like same phenomenon but here we emphasize on **unboundedness.**

 How to test/quantify:
 
separate sequences in the data based on lengthand evaluate models on their ability to cope with longer sequences after having been familiarisedwith the shorter ones

 ## Substitutivity:
 
  If an  expression  is  altered  by replacing  one  of  its  constituents  with  another  constituent  with  the  same  meaning  (a  synonym),this does not affect the meaning of the expression (Pagin, 2003)
  
  Critique:
  a.Plato was bald
  
  b.baldness was an attribute of Plato
  
  Above sentences have similar meaning but one can't be replaced with the other in the below sentence:
  
  The  philosopher whose most eminent pupil was Plato was bald
  
  How to test/quantify:
  
artificially introduce synonyms and consider how the prediction of amodel changes when an atomic unit in an expression is replaced by its synonym.
**Synonymous words should occur equally often in comparable contexts**

## Localism:

The meaning of a complex expression depends only on its localstructure and the meanings of its immediate parts.

compare a model’s output when it is given a composed sequence X, built up from two parts A and B with the output the same model gives when it is forced to first separately process A and B in a local fashion.

Now a complete example:

Sample dataset:
probabilistic  context  free  grammar  (PCFG)

  <img width="873" alt="Screen Shot 2022-02-04 at 5 32 05 PM" src="https://user-images.githubusercontent.com/21222766/152612278-f3bbf897-a6f8-4023-9a93-f8ac1cf33c49.png">

<img width="680" alt="Screen Shot 2022-02-04 at 5 35 12 PM" src="https://user-images.githubusercontent.com/21222766/152612394-612adf08-5724-4a83-8a5a-3a062cc2f179.png">


How to seperately test out various properties of the model using this dataset:

Measure:
sequence  accuracy,  whereonly instances for which the entire output sequence equals the target are considered correct.
Now we can measure overall 

task accuracy.

How to test for systematicity:

consider some-pairs

swap repeat,append removesecond,repeat removesecond append swap

Ensure training doesn't have any of these pairs together and all sequences in test data contain atleast one of them.

reverse repeat removesecond A B , C D  cannot appear in the training set, but repeat reverse removesecond A B , C D can.

How to test for Productivity:
we redistribute the training and testing data such that there is no evidenceat all for longer sequences in the training set. 


 Testing for Localism:
 due to recursive nature of the PCFGSET expressions and interpretation functions, this is a relatively straightforward
 
 echo append C, prepend B , A
 
 prepend B , Aand then replace the constituent by this predicted meaningo1.  In the next step, we use the modelto compute the meaning of the then smallest constituentechoo1, and replace the constituent inthe  sequence  with  the  model’s  prediction  for  this  constituent.
 
 Final prediction is compared with models total output.
 
 
 In the paper, we will look at task-accuracy or any other single metric. Some datasets can only measure few of the attributes of Compositionality that we discussed - but we ignore all that use a single summary statistic.
 
 There will be brief slide showing the datasets and we will put the faith on the authors that everything is taken care of(splitting traning and test sets appropriately so that we only measure the phenomenon we are thinking we are - like the gotcha above - taking care of frequency of synonymous words.)


## Everything You need to know abount Inductive biases and Transformers.

Objective: Look at this picture and understand most of what's happening

<img width="1049" alt="Screen Shot 2022-02-04 at 7 29 29 PM" src="https://user-images.githubusercontent.com/21222766/152620555-426165b7-7570-4bed-bd1d-5f6f7e6e51e6.png">

As a cosequence maybe -> Convince (try to) you all that transformers have less inductive biases to begin with than a typical CNN(or others) -> Thus they are a better starting point as we can learn those from data or we can add-in what's necessary at our will.




Inductive Bias - Make some models more likely than others before you see that data. In other words force the model to know what you know before starting out.

Comments: Bayesian modeling does this naturally but here i meant them in a more general sense.

Let's excercise our ability to stare at images and figure out inductive biases.


Fully connected vs CNN


Fully connected 

<img width="678" alt="Screen Shot 2022-02-04 at 7 08 36 PM" src="https://user-images.githubusercontent.com/21222766/152619659-34b8409b-2016-4bab-ab33-f36f4d857aff.png">

CNN

<img width="680" alt="Screen Shot 2022-02-04 at 7 06 53 PM" src="https://user-images.githubusercontent.com/21222766/152619211-f52659b0-b03d-4ea4-a402-ed4076084ba8.png">


What I know:

a. In Natural signals(eg: images,sound etc) there are strong local correlations.(In otherwords predicting future is easy given recent past compared to earlier versions). In images it's over spatial dimensions whereas in say Sound or Natural language it's over temporal dimension. - `Locality`


b.Same Patterns repeat again and again. - `Stationarity`

c. There are hierarchies. Letters make up words,words make up sentences and so on - `Compositionality`.


In the above figure(CNN) If I stack the 2-d input into a 1-d vector by unrolling left to right and top to bottom, the convolution can be represented as a matrix multiplication.





<img width="679" alt="Screen Shot 2022-02-04 at 7 25 38 PM" src="https://user-images.githubusercontent.com/21222766/152620354-7044a746-c909-448e-936d-a5ec6a7044c9.png">


The `zeros` along the columns encode `locality` while the replication of same weights along the rows account for `stationarity`. If the above properties doesn't make sense for your input , then CNN's aren't the right choice.

Verdict

CNN's are a linear layer with lots of weight sharing and sparsity. RNN's just use weight sharing but BPTT takes the locality into account.

Above we assumed the implicit notion of *Order* in our signals - let's remove this bias.

General Seq-Seq mapping:

Let’s call the input vectors 𝐱1,𝐱2,…,𝐱t and the corresponding output vectors 𝐲1,𝐲2,…,𝐲t. The vectors all have dimension k.


<img width="772" alt="Screen Shot 2022-02-04 at 8 46 55 PM" src="https://user-images.githubusercontent.com/21222766/152624170-4de3d388-4ac2-4e74-b66c-fd7e1594f2ac.png">


As we can see there is nothing fancy going on, no assumption of locality or implicit ordering.

a. If we want our model to have some sensitivity to word ordering - we create a second vector of equal length, that represents the position of the word in the current sentence, and add this to the word embedding. We can make different choices here.

b. And some historical baggage -> Encoder decoder style architecture.

(1) the type of position en-codings
(2) the use of copy decoders
(3) modelsize, 
(4) weight sharing
(5) the use of interme-diate representations for prediction


