# Comparison of Sequence Augmentation Techniques for Protein Classification Tasks

This project is a comparison of proposed protein sequence augmentation techniques for synthetic minority class oversampling for protein classification tasks. Here, I evaluate the performance of several previously published augmentation methods on a set of classification challenges and find that...

## Background

Data augmentation encompasses a variety of techniques which artificially expand a dataset by creating modified copies of its samples. These include synthetic minority oversampling (SMOTE) and adaptive synthetic oversampling (ADASYN) for tabular data, rotation, mirroring, and shearing for images, and synonym replacement and back translation for text, among others. However, relatively little attention has been paid to data augmentation for protein amino acid sequences, which are relevant to a variety of prediction tasks in molecular biology. In cases where few labeled examples exist, augmenting training data with synthetically generated samples has the potential to improve performance.

In "[Improving Generalizability of Protein Sequence Models via Data Augmentations](https://openreview.net/forum?id=Kkw3shxszSd)", Shen et al. (2020) compared the utility of a variety of sequence augmentation methods, including random substitution with alanine, random substitution with a similar amino acid, global and local random amino acid shuffling, and sequence reversal, at improving performance on several protein property prediction tasks. Considerable variability in results made it difficult to extract general conclusions from their experiments; however, the two most rational approaches--alanine substitution and substitution with similar amino acids--seemed to deliver the most consistent improvements.

Another method of protein augmentation known as nucleotide augmentation was proposed by Minot et al. in "[Nucleotide augmentation for machine learning-guided protein engineering](https://www.biorxiv.org/content/10.1101/2022.03.08.483422v1.full)". Each amino acid in the protein sequence was replaced at random by one of the 3-letter nucleotide codons which encodes it and the resulting nucleotide sequences were used as the input sequence for the models, either as unigrams or trigrams.

## Results

Binary classification of proteins based on whether or not they bind metal ions was chosen as a model classification task. Of the 20412 human proteins in UniProt, 2282 are labeled by the [Gene Ontology (GO) database](http://geneontology.org/) of protein annotations as having metal ion binding properties. I used these proteins as the positive set and the remaining 18130 as the negative set, resulting in a moderately imbalanced classification task.

The dataset was randomly undersampled by 75% to improve computational tractability and then divided into 5 folds. Each was successively held out as the test set for model training, with the remaining 4 serving as the training set (5-fold cross-validation). To determine the generalizability of each augmentation's effect across model types, both a convolutional neural network (CNN) and a long short-term memory (LSTM) network were trained on the data and evaluated. Training proceeded until test loss had not improved for 2 epochs. Details of the data processing, models, and training methods can be found in the included Jupyter notebook.

## Conclusion
