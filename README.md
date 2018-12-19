## Parts of Speech Tagger:

### Simple Model:

For Simple model we have just considered the dependency of each word on its corresponding parts of speech.
For this we just have to maximize the the the posterior probability P(S|W) i.e Prior * Likelihood (P(S) * P(W|S)), at each word in the sentence.
We acheived a word accuracy of 93.92% and sentence accuracy of 47.45% by doing this.

### Hidden Markov Model:
For this model, there is an additional dependency on consecutive words rather than just the word to sentence dependency.
Used the Viterbi technique to acheive this, where we keep track of the maximum probabilities till each of the word of the sentence, starting from the first word, and iteratively calculating the max probability till a word, by making use of the maximum probability till the previous word, which are pre-calculated.

We have acheived a word accuracy of 95.4% and sentence accuracy of 53.3% with this model.


### Monte Carlo Markov Chains - Gibbs Sampling:
Transition Probabilities: In this case transition probabilities for two precedent parts of speech tags
must be considered from thrid word in a sentence.

P(S(i) | S(i-1), S(i-2)) where Si is the Parts of Speech tag for i'th word in a sentence.
Earlier for HMM we have built transition probabilities dictionary of dictionaries. In this case since we
have to store another variable(parts of speech tag), we need to add one more layer.
This will be helpful to obtain transition probability by just accessing corresponding keys in the dictionary.

For example, P(noun | verb, adj) can be obtained by looking at dict[noun][verb][adj]
If in case there are no probabilities for a particular sequence then a min probability is assigned
for computation feasibility.

Sampling:
For a sentence we need to generate large number of samples with parts of speech tags for each word.
Here we are generating 100 samples with 50 samples left out as warm-up.
Tried for different number of samples ranging from 100 to 2000, but the accuracies did not significantly change.

So keeping in mind the running time too, 100 samples were considered in the end.
Initially we are starting with the word predictions using HMM method.Instead of starting with random tags
this is a better choice and chances are high for the convergence to happen quicker.

For a sample, sentence is looped over for words
  For each word
      Tags for all other words are kept constant.
      Probability distribution of current word's parts of speech is calculated.
      This is done by calculating probabilities for current word being each of 12 speech tags.
      For each of 12 parts of speech tags
          Probability of current word being current parts of speech tag is calculated.
      Using the distribution a tag is picked for current word and is updated to be used for next sample.
  This updated one will be iterated over for generating next sample.

In this way we generate 1000 samples to let the distributions become stationary, not storing any of them
Then the samples are stored for next 1000 iterations and these are stored.
From these stored values we choose the speech tag with maximum number of occurrences for particular word and
assign the same.
