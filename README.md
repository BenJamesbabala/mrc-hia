# Machine Reading Comprehension - (My Model)



Several Cloze-style reading comprehension datasets are readily available. Some general comparisons are given below.
Dataset 	Language 	Genre 	Blank
Type 	Document 	Query 	Content
CNN/Daily Mail
(Hermann et al., 2015) 	English 	news 	NE 	news article 	summary w/
a blank 	train+valid
+test
Children's Book Test (CBTest)
(Hill et al., 2015) 	English 	story 	NE, CN,
V, P 	20 consecutive
sentences 	21th sentence w/
a blank 	train+valid
+test
People Daily
(Cui et al., 2016a) 	Chinese 	news 	NE, CN 	news article w/
a blank 	the sentence that
blank belongs to 	train+valid
+test
Children's Fairy Tale (CFT)
(Cui et al., 2016a) 	Chinese 	story 	NE, CN 	story article w/
a blank 	the sentence that
blank belongs to 	test only1
Who did what
(Onishi et al., 2016) 	English 	news 	NE 	news article 	deleting an NE from the first sentence of the question article 	N/A2
BookTest
(Bajgar et al., 2016) 	English 	news 	NE,CN 	20 consecutive
sentences 	21th sentence w/
a blank 	train+valid
+test

    1 contains one human evaluated test set
    2 as this 'dataset' is made from LDC corpus (Gigaword), it is not available to the public unless licenced by LDC
    Abbreviation NE: Named Entity, CN: Common Noun, V: Verb, P: Preposition

Neural Architectures

In this section, the models will be compared with each other. As illustrated, the comment only represents my own views. Also note that, the evaluation DO NOT take the model performance into account, and it just shows the characteristics of the model itself.

    Rating will be in 1 ~ 5 stars, where 5★ means best, and 1★ means worst.
    Take Kadlec et al., 2016 as an example,
    in Model efficiency, I give a 5★, meaning the training speed is very fast;
    in Implementation complexity, I give a 4★, meaning it is relatively easy to implement such a neural network;
    in Hyper-parameter, I give a 4★, meaning there are fewer hyper-params that should be defined by human. Note that, here the hyper-parameters are only indicating the params in neural network itself, not including such as layer dimension or something else.

System 	Core
Architecture 	Prediction
Range1 	Model
Efficiency 	Implementation
Complexity 	Hyper-
parameter 	Other
tricks
Hermann et al., 2015 	Attention-
based RNN 	All words in
document 	★★★ 	★★★ 	★★★★ 	-
Hill et al., 2015 	Memory
Network 	All words in
document 	★★★ 	★★ 	★★★ 	self-
supervision
Kadlec et al., 2016 	Attention-
based RNN 	All words in
document 	★★★★★ 	★★★★ 	★★★★ 	-
Cui et al., 2016a 	Attention-
based RNN 	All words in
document 	★★★★ 	★★★★ 	★★★★ 	-
Chen et al., 2016 	Attention-
based RNN 	Only Entities in
document 	★★★ 	★★★★ 	★★★★ 	-
Dhingra et al., 2016 	Attention-
based RNN 	- 	★★★ 	★★★ 	★★★ 	-
Trischler et al., 2016 	Attention-
based RNN 	- 	★★ 	★★ 	★★ 	-
Sordoni et al., 2016 	Attention-
based RNN 	All words in
document 	★★★ 	★★ 	★★★ 	-
Cui et al., 2016b 	Attention-
based RNN 	All words in
document 	★★★★ 	★★★ 	★★★★ 	-
Weissenborn, 2016 	Attention-
based RNN 	Only Entities in
document 	★★★ 	★★ 	★★★ 	-
Li et al., 2016 	Attention-
based RNN
+CRF 	- 	★★★ 	★★ 	★★★ 	-
Shen et al., 2016 						

    1 only shows the CNN/Daily Mail condition. In CBTest condition, all models only considers 10 candidates given in the datasets.

Training Details

Here, I will have a brief comparison on the training details of each model, and hopefully you will have a better concept in parameter tuning.
System 	Embed
Init 	Hidden
Type 	Gradient
Clip 	Optimi-
zation 	Batch
Size 	Initial
LR 	Dropout 	Imple-
mentation
Hermann et al., 2015 	rand 	LSTM 	- 	RmsProp 	32 	0.00005 	yes 	-
Hill et al., 2015 	rand 	LSTM 	5 or 10 	SGD 	? 	RANGE 	yes 	-
Kadlec et al., 2016 	rand 	GRU 	10 	ADAM 	32 	0.001 	no 	TH+B
Cui et al., 2016a 	rand 	GRU 	10 	ADAM 	32 	0.0005 	yes 	TH+K
Chen et al., 2016 	GloVe 	LSTM 	10 	SGD 	32 	0.1 	yes 	-
Dhingra et al., 2016 	word2vec 	GRU 	10 	ADAM 	32 	0.0005 	yes 	TH+L
Trischler et al., 2016 	rand 	GRU 	- 	ADAM 	32 	0.001 	no 	TH+K
Sordoni et al., 2016 	rand 	GRU 	5 	ADAM 	32 	0.001 	yes 	TH+K
Cui et al., 2016b 	rand 	GRU 	5 	ADAM 	32 	0.001 	yes 	TH+K
Weissenborn, 2016 	GloVe 	GRU 	- 	ADAM 	32 or 128 	0.001 	yes 	TF
Shen et al., 2016 	GloVe 	GRU 	- 	ADAM 	64 	0.0005 	yes 	-
Li et al., 2016 	NNLM 	LSTM 	- 	RmsProp 	120 	- 	- 	-
Bajgar et al., 2016 	rand 	GRU 	10 	ADAM 	128 	0.0005 	no 	TH+B

    Abbreviation TH: Theano, B: Blocks, K: Keras, L: Lasagne, TF: TensorFlow

Training Tips

I show several tips in training these neural network models, FYI.

1) Use adaptive learning algorithm, if you are not expertise in optimizing vanilla SGD. Use learning rate decay, even if you use adaptive learning algorithm, such as ADAM, RmsProp etc.

2) Reshuffle training data in every epoch (general tips)

3) Any neural network models that are based on AS Reader (Kadlec et al., 2016) should pay attention to the overfitting problem. Add dropout or l2_regularization to solve this problem. Also, it should be noted that using regularization will slow down the convergence speed.

4) Do not use big batch size. Typically, 32 is a good start. Note that, though bigger batch size lead to faster training, the convergence speed may not be also fast (general tips)

5) GRU typically shares the comparable performance with LSTM, so feel free to use either of them.

6) Use gradient clipping 5~10, when using LSTM or GRU. Gradient Exploding can sometimes happen, which is more dangerous than gradient vanishing!
Overall Experimental Results

In this part, I only show the results on CNN/Daily Mail and Children's book story Named Entity and Common Noun (short for CBT-NE and CBT-CN) datasets. Only single model evaluation is showed here. For more results, such as ensemble performance, please directly refer to the related papers.

The best result in each category is marked with bold face.
System 	CNN News
Valid 	CNN News
Test 	Daily Mail
Valid 	Daily Mail
Test 	CBT-NE
Valid 	CBT-NE
Test 	CBT-CN
Valid 	CBT-CN
Test
Deep LSTM Reader1 	55.0 	57.0 	63.3 	62.2 	- 	- 	- 	-
Attentive Reader1 	61.6 	63.0 	70.5 	69.0 	- 	- 	- 	-
Impatient Reader1 	61.8 	63.8 	69.0 	68.0 	- 	- 	- 	-
Human (ctx+query)2 	- 	- 	- 	- 	- 	81.6 	- 	81.6
MemNN(+self-sup)2 	63.4 	66.8 	- 	- 	70.4 	66.6 	64.2 	63.0
AS Reader3 	68.6 	69.5 	75.0 	73.9 	73.8 	68.6 	68.8 	63.4
CAS Reader4 	68.2 	70.0 	- 	- 	74.2 	69.2 	68.2 	65.7
Stanford AR5 	72.4 	72.4 	76.9 	75.8 	- 	- 	- 	-
GA Reader6 	73.0 	73.8 	76.7 	75.7 	74.9 	69.0 	69.0 	63.9
EpiReader7 	73.4 	74.0 	- 	- 	75.3 	69.7 	71.5 	67.4
Iterative Attention8 	72.6 	73.3 	- 	- 	75.2 	68.6 	72.1 	69.2
AoA Reader9 	73.1 	74.4 	- 	- 	77.8 	72.0 	72.2 	69.4
QANN10 	- 	73.7 	- 	77.2 	- 	70.6 	- 	-
ReasoNet11 	72.9 	74.7 	77.6 	76.6 	- 	- 	- 	-
Li et al., 2016 	77.7 	77.1 	78.9 	78.0 	- 	- 	- 	-
AS Reader12
(trained on BookTest) 	- 	- 	- 	- 	80.5 	76.2 	83.2 	80.8

    Reference Those marked by 1 are taken from Hermann et al., 2015;
    2 are taken from Hill et al., 2015;
    3 are taken from Kadlec et al., 2016;
    4 are taken from Cui et al., 2016a;
    5 are taken from Chen et al., 2016;
    6 are taken from Dhingra et al., 2016;
    7 are taken from Trischler et al., 2016;
    8 are taken from Sordoni et al., 2016;
    9 are taken from Cui et al., 2016b;
    10 are taken from Weissenborn, 2016.
    11 are taken from Shen et al., 2016.
    12 are taken from Bajgar et al., 2016.

Related Papers

You can either download the related papers from this repository or in the following links.

    (Hermann et al., 2015) Teaching Machines to Read and Comprehend
    http://arxiv.org/abs/1506.03340

    (Hill et al., 2015) The Goldilocks Principle: Reading Children's Books with Explicit Memory Representations
    http://arxiv.org/abs/1511.02301

    (Kadlec et al., 2016) Text Understanding with the Attention Sum Reader Network
    http://arxiv.org/abs/1603.01547

    (Chen et al., 2016) A Thorough Examination of the CNN/Daily Mail Reading Comprehension Task
    https://arxiv.org/abs/1606.02858

    (Dhingra et al., 2016) Gated-Attention Readers for Text Comprehension
    https://arxiv.org/abs/1606.01549

    (Sordoni et al., 2016) Iterative Alternating Neural Attention for Machine Reading
    https://arxiv.org/abs/1606.02245

    (Trischler et al., 2016) Natural Language Comprehension with the EpiReader
    https://arxiv.org/abs/1606.02270

    (Cui et al., 2016a) Consensus Attention-based Neural Networks for Chinese Reading Comprehension
    https://arxiv.org/abs/1607.02250

    (Cui et al., 2016b) Attention-over-Attention Neural Networks for Reading Comprehension
    https://arxiv.org/abs/1607.04423

    (Dirk Weissenborn, 2016) Separating Answers from Queries for Neural Reading Comprehension
    http://arxiv.org/abs/1607.03316

    (Li et al., 2016) Dataset and Neural Recurrent Sequence Labeling Model for Open-Domain Factoid Question Answering
    https://arxiv.org/abs/1607.06275

    (Onishi et al., 2016) Who did What: A Large-Scale Person-Centered Cloze Dataset
    http://arxiv.org/abs/1608.05457

    (Shen et al., 2016) ReasoNet: Learning to Stop Reading in Machine Comprehension
    https://arxiv.org/abs/1609.05284

    (Bajgar et al., 2016) Embracing data abundance: BookTest Dataset for Reading Comprehension
    https://arxiv.org/abs/1610.00956

Contact

For any problems, please leave a message in the Github Issues.


