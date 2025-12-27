## BERT-based sentiment classifier Analysis

The goal of this project (ML-teamproject/Embedding) is to see whether explicitly boosting emotionally-charged tokens inside BERT embeddings can improve sentiment classification.

The dataset is a 12K IMDb movie review subset, with binary labels: positive / negative.

The main idea is:
  - Before tokenizing with BERT, we run VADER over the sentence to detect words that carry positive or negative sentiment.
  
  - We map these sentiment words to BERT tokens.
	
  - In the embedding layer, if a token is in the “sentiment token” set, we scale its word embedding by a fixed ratio (e.g. 1.5×).

  - The rest of BERT (self-attention, encoder layers, classifier head) stays standard.

So this is basically a **BERT-based sentiment classifier** with a small modification at the embedding level that tries to give more weight to sentiment-bearing tokens.

I then compare different scaling ratios (1.0, 1.5, 2.0) across multiple random seeds.

### Result
<table>
  <tr>
    <th>Ratio</th>
    <th>Accuracy</th>
  </tr>
  <tr>
    <td>1.0</td>
    <td>0.9113</td>
  </tr>
  <tr>
    <td>1.5</td>
    <td>0.9092</td>
  </tr>
  <tr>
    <td>2.0</td>
    <td>0.9056</td>
  </tr>
</table>

Boosting sentiment tokens can help in some splits in some ways(check randomstate42, ratio 1.0),

but when it comes to average ability, ratio = 1.0 actually gives the best overall performance (0.9113).

So, simply amplifying sentiment-bearing tokens is not consistently beneficial.

It can sometimes help, but it can also amplify noise depending on the split.

This experiment is basically a first step to check: 

“If we tell BERT explicitly which tokens are emotional and push them harder in the embedding, does the model really like it?”

And the answer from this project is that the baseline BERT is still very strong even without explicit weighting.

