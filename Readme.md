# IDENTIFYING DUPLICATE QUORA QUESTIONS 
This project was created to work with in the Quora Question Pairs data set (see [1])

Basically Quora released a training dataset with more than 400K pairs of questions in which it was known which questions were similar.  The test data set consisted of more than 2M pairs of questions.

Example: consider the following questions
Q1: Is Obama giving a speech to the press today?
Q2: Is the president talking with the journalists today?
Q3: What time is it?

The Q1 and Q2 have similar meaning but Q2 and Q3 do not have a similar meaning. And hence the training data set has the entries

Question1	Question2 	is_repeated

Q1        Q2          1

Q2        Q3          0		


# My Approach
I constructed around 125 features that can be divided in the following groups

* Very basic features: features involving the size of the questions, the number of words, the number of words not including stop words, number of question marks, number of commas, number of capital letters, flags for the most common first words (are, is, what, would, why, ..) 

* Naive Bayes/TFIDF - based features: some words are expected to be decisive when they are in one question but not in the other. Think about proper nouns like Obama, Trump, Armstrong, ....; it is intuitively less likely that two questions match provided that one of them has the word Trump and the other one does not. We initially rated the importance of each word in the training data set  using a TFDIF (see wikipedia) and then extended this rating to words in the test dataset set using a pre trained word2vect model. We used this rating in order to create a set of features for the words that were in both questions of the pair and another set of features for the words that were in exactly one of the questions. 

* Parts of Speech-based features: We used NLTK (natural language toolkit) in python in order to extract the parts of speech of each question (the verbs, adverbs, nouns, pronouns, ... ) and then measured the distance of the part of speech of each question using different distances: Jaccard, WordMover, Cosine.

* Abhishel Thakur Features: he shared these features during the competition and many of the top teams were also using them, many of these features overlap with the ones explained before. See: https://github.com/abhishekkrthakur/is_that_a_duplicate_quora_question

In order to make the classification I used Random Forests, and tuned them using a GridSearch with cross validation 


# Potential improvements
* The document https://medium.com/@InDataLabs/how-to-win-kaggle-competition-based-on-an-nlp-task-not-being-an-nlp-expert-58944df5644c   explains how to create some features that were used by many groups during the competition (and shared between them) that might be interesting to include. A family of this features referred in the document as “magic features” seams to be very relevant to the competition even though it is very dependent on the way the data set was provided and the way in which they organized the document.

* Creating an ensemble of trees using Gradient Boosting seams to be a very popular technique amongst Kagglers that very often outperforms Random Forests. It would be interesting to run XGBoost with the features explained before.

# References
[1]  https://www.kaggle.com/c/quora-question-pairs/data



