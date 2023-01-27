# Comparison of Sequence Augmentation Techniques for Protein Classification Tasks

This project is a comparison of several previously published protein sequence augmentation techniques for synthetic minority class oversampling for protein classification. Synthetic data generation methods such as SMOTE, ADASYN, image augmentation, and others are commonly used in machine learning to combat overfitting by artificially creating new samples, but have seen relatively little use for protein sequences. Here, I compare several published protein sequence augmentation techniques and find...

## Background

Data augmentation encompasses a variety of techniques which artificially expand a dataset by creating modified copies of its samples. These include synthetic minority oversampling (SMOTE) and adaptive synthetic oversampling (ADASYN) for tabular data, rotation, mirroring, and shearing for images, and synonym replacement and back translation for text, among others. However, relatively little attention has been paid to data augmentation for protein amino acid sequences, which are relevant to a variety of prediction tasks in molecular biology. In cases where few labeled examples exist, augmenting training data with synthetically generated samples has the potential to improve performance.

In "[Improving Generalizability of Protein Sequence Models via Data Augmentations](https://openreview.net/forum?id=Kkw3shxszSd)", Shen et al. (2020) compared the utility of a variety of sequence augmentation methods, including random substitution with alanine, random substitution with a similar amino acid, global and local random amino acid shuffling, and sequence reversal, at improving performance on several protein property prediction tasks.

## Results

## Conclusion
