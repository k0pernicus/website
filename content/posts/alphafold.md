---
author: "Antonin"
date: "2020-12-03T23:22:00+02:00"
tags: ["alphafold", "artificial intelligence", "machine learning"]
title: "AlphaFold 2: a new AI achievement"
description: "The end of a 50 years old grand challenge"
toc: true
readTime: true
---

The last 30th of November, [DeepMind](https://deepmind.com/) published on their website 
[an article about _AlphaFold_ 2](https://deepmind.com/blog/article/alphafold-a-solution-to-a-50-year-old-grand-challenge-in-biology), 
an Artificial Intelligence that solves with 92.4 _GDT_, on simple protein structure, a 50 years old grand challenge: the **protein folding problem**.

DeepMind released the first version of _AlphaFold_ in 2018.  
This first version achieved a score of 58 in the Global Distance Test (_GDT_) at the Critical Assessment of Structure Prediction (_CASP_) competitions, which was the highest score at that time.  
For this occasion, an article [has been published on Nature](https://www.nature.com/articles/s41586-019-1923-7.epdf?author_access_token=Z_KaZKDqtKzbE7Wd5HtwI9RgN0jAjWel9jnR3ZoTv0MCcgAwHMgRx9mvLjNQdB2TlQQaa7l420UCtGo8vYQ39gg8lFWR9mAZtvsN_1PrccXfIbc6e-tGSgazNL_XdtQzn1PHfy21qdcxV7Pw-k3htw%3D%3D), and the source code publicly available on [Github](https://github.com/deepmind/deepmind-research/tree/master/alphafold_casp13).
With version 2, DeepMind's dedicated team performed a median score of **92.4** _GDT_ overall across all targets on the same competition dataset, and a median score of **87.0** _GDT_ for the very hardest protein targets.  
Almost 30 more _GDT_ in two years, a ***huge*** improvement between those two years.

In this blog post, I will decompose this breakthrough into several parts: talk about the protein folding problem, the competition and the achievements before _AlphaFold_, and very quickly about both _AlphaFold_ 1 and 2.

## Protein? Folding? What?

A protein is a macromolecule (= **big** molecule) that performs a vast array of functions within organisms, 
from transporting molecules to DNA replication[1].  
In short: **proteins are essential to life**.  
Like every other chain of molecules - a protein is composed by amino acids - two proteins are different by their content, or composition.  
However, something is specific and unique to a protein: **its 3D structure**!

Indeed, a protein with the same amino acids than a second one have two different functions if their 3D structure (or their physical shape in space) vary, as
a shape can deliver, or hide, an activation section for every other molecule around in order to interact specifically with.  
Even [if people are still fighting again on asking ourselves if we can simplify the sequence similarity measure between two proteins as a structural similarity](https://www.researchgate.net/post/Does_sequence_similarity_between_two_proteins_mean_structural_similarity), we will 
assume that the 3D structure of a protein **determines its own function** within an organism.

The structure of a protein is, almost all the time, **very** complex to understand, as it depends of four different levels of
complexity: hydrophobic interactions, hydrogen bonds, the Van der Waals forces between nearby amino acids, 
and di-sulphide connections between cysteine molecules.

To test all the possible folds of a protein using brute force methods is not possible then, as, based on the Levinthal's paradox,
it could be more than **10^143 ways** to fold a single protein, which is way more than playing chess (**10^120**) but less than all
the possible moves in the game of Go (**10^360**).  
Obviously, misfolding a protein has way more implications than making a wrong move in a Go game...

![](/images/3D_protein_structure.jpg#small "Ribbon diagram of the 3D structure of hemoglobin - credits to Azom.com[2]")

Unfortunately, a protein can be modified, or **misfold**, and lead to strong issues in the organism...  
Indeed, an accumulation of misfolded proteins - proteins that contain at least one structure modifications - 
can lead one or multiple disease(s), [sometimes neurodegenerative like Alzheimer's disease, or Huntington's disease](https://www.nature.com/scitable/topicpage/protein-misfolding-and-degenerative-diseases-14434929/).

So, reducing the problem of identifying a protein by its amino-acids sequence can help the scientists to design a specific protein
for a drug, or to understand how a modification, or mutation, in a protein can cause a specific disease.

## Competitions, researches and results

Every two years a community-wide worldwide experiment for protein structure prediction, called [_CASP_](https://en.wikipedia.org/wiki/CASP),
is launching global competitions to review the recent scientific researches and results on protein folding.
This review is done by computing the [_GDT_, or *G*lobal *D*istance *T*est](https://en.wikipedia.org/wiki/Global_distance_test) using a 
complex dataset of 200 million of proteins.  
A _GDT_ is a measure of similarity **between two protein structures with identical amino acid sequences and different shapes** - in short, 
a prediction accuracy for proteins shape.  
Claimed by the Science community, the higher total score of _GDTs_, the better a given model is in comparison to reference structure.

The _CASP_ competitions for protein folding are performed since 1994, and new algorithms and results are evaluated
 by this community every two years.

![](/images/CASP_2020.png#small "CASP results, 30th November of 2020 - credits to DeepMind[3]")

## The AlphaFold breakthrough

_CASP_ 13, in 2018, has been won by the first version of _AlphaFold_, with 58 _GDT_, which was the highest score at this time.  
This year, _CASP_ 14 has been won by the second version of _AlphaFold_ with nearly 90 _GDT_.

The secret of the second version of _AlphaFold_ is not known yet, but, as stated by Mirko Torrisi, Gianluca Pollastri, and Quan Le,
 the secret of the first version is known as a distance map predictor, implemented as a very deep convolutional neural networks,
trained on more than 170_000 proteins by 200 GPUs.[4][5][6]  

_AlphaFold_ 1 has two steps in the process:
1. a Convolutional Neural Network ([CNN](https://en.wikipedia.org/wiki/Convolutional_neural_network)), that takes 
- at least - as input the amino acids residue sequences, and output the distance metrics between the amino-acids in space,  
2. a [Gradient descent optimisation](https://en.wikipedia.org/wiki/Gradient_descent) of folding the 3D structure to 
match the distances between amino-acids in the protein.

![](/images/alphafold_1_struct.png#small "AlphaFold 1 architecture - credits to DeepMind[6]")

As stated by the team of _AlphaFold_, [the second version of the model may be **significantly** different from the first one](https://fortune.com/2020/12/01/lessons-from-deepminds-a-i-breakthrough-eye-on-ai/), as the first version has a (light) tendency of overfit the data.

People tend to think that the second version replaced the CNNs with transformers, an [attention-based neural network](https://towardsdatascience.com/intuitive-understanding-of-attention-mechanism-in-deep-learning-6c9482aecf4f), and speculate on more accurate feature engineering 
of amino acids residue sequences.

## What's next ?

> "In some sense the problem is solved."
[John Moult](https://www.nature.com/articles/d41586-020-03348-4)

_AlphaFold_ **will** have implications for areas like drug design or treatments, biomaterials, environmental sustainability - like 
designing enzymes to eat the plastic in our oceans - or agriculture.  
Indeed, _AlphaFold_ could be used to understand how protein structure can lead to specific diseases, how to identify proteins
 that have malfunctioned, but also learn unknown functions of genes encoded in DNA.

Both _AlphaFold_ versions are the work of incredible talented engineers and researchers at DeepMind, but also
of all the researchers who participated since the last fifty decades at least.  
So, congratulations to John Jumper, Richard Evans, Alexander Pritzel, Tim Green, Michael Figurnov, 
Kathryn Tunyasuvunakool, Olaf Ronneberger, Russ Bates, Augustin Žídek, Alex Bridgland, Clemens Meyer, Simon A. A. Kohl, 
Anna Potapenko, Andrew J. Ballard, Andrew Cowie, Bernardino Romera-Paredes, Stanislav Nikolov, Rishub Jain, Jonas Adler, 
Trevor Back, Stig Petersen, David Reiman, Martin Steinegger, Michalina Pacholska, David Silver, Oriol Vinyals,
Andrew W Senior, Koray Kavukcuoglu, Pushmeet Kohli, Demis Hassabis, and all the other engineers and researchers
who have been involved in any way in this subject.

As stated in their blog post, DeepMind is pretty optimistic about the impact _AlphaFold_ can have on the wider world... I am too!  
To me, _AlphaFold_ is another proof that AI can be beneficial for humanity.

Five years after [Alphago](https://deepmind.com/research/case-studies/alphago-the-story-so-far), DeepMind just solved another 
big challenge... Who knows which big challenge DeepMind will solve in 2025!

## Online resources

* [1] [Wikipedia - Protein definition](https://en.wikipedia.org/wiki/Protein)
* [2] [An introduction to proteins](https://www.azom.com/article.aspx?ArticleID=11177)
* [3] [DeepMind - AlphaFold: a solution to a 50-year-old grand challenge in biology](https://deepmind.com/blog/article/alphafold-a-solution-to-a-50-year-old-grand-challenge-in-biology)
* [4] [Wikipedia - AlphaFold](https://en.wikipedia.org/wiki/AlphaFold)
* [5] [Deep learning methods in protein structure prediction](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7305407/)
* [6] [DeepMind - AlphaFold: Using AI for scientific discovery](https://deepmind.com/blog/article/AlphaFold-Using-AI-for-scientific-discovery)
* [YouTube - Protein folding explained](https://www.youtube.com/watch?v=KpedmJdrTpY&t=7s)
* [YouTube - Alex Fridman about DeepMind AlphaFold](https://www.youtube.com/watch?v=W7wJDJ56c88)
* [YouTube - The making of a scientific breakthrough](https://www.youtube.com/watch?v=gg7WjuFs8F4)
