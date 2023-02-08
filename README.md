# Comparison of Sequence Augmentation Techniques for Protein Classification Tasks

This project is a comparison of proposed protein sequence augmentation techniques for synthetic minority class oversampling in protein classification using machine learning. Here, I evaluate the performance of several previously published augmentation methods on a set of classification challenges and find that substituting a small percentage of the amino acids in a sequence with a chemically similar amino acid (dictionary substitution) results in small but consistent gains in performance on several protein classification tasks. Several other proposed methods, in contrast, result in decreases in performance. These results indicate that dictionary substitution may be an underutilized method for enhancing protein classification in sequence-based machine learning.

## Background

Data augmentation encompasses a variety of techniques which artificially expand a dataset by creating modified copies of its samples. These include synthetic minority oversampling (SMOTE) and adaptive synthetic oversampling (ADASYN) for tabular data, rotation, mirroring, and shearing for images, and synonym replacement and back translation for text, among others. However, relatively little attention has been paid to data augmentation for protein amino acid sequences, which are relevant to a variety of prediction tasks in molecular biology. In cases where few labeled examples exist, augmenting training data with synthetically generated samples has the potential to improve performance.

In "[Improving Generalizability of Protein Sequence Models via Data Augmentations](https://openreview.net/forum?id=Kkw3shxszSd)", Shen et al. (2020) compared the utility of a variety of sequence augmentation methods, including random substitution with alanine, random substitution with a similar amino acid, global and local random amino acid shuffling, and sequence reversal, at improving performance on several protein property prediction tasks. Considerable variability in results made it difficult to extract general conclusions from their experiments; however, the two most rational approaches--alanine substitution and substitution with similar amino acids--seemed to deliver the most consistent improvements.

Another method of protein augmentation known as nucleotide augmentation was proposed by Minot et al. in "[Nucleotide augmentation for machine learning-guided protein engineering](https://www.biorxiv.org/content/10.1101/2022.03.08.483422v1.full)". Each amino acid in the protein sequence was replaced at random by one of the 3-letter nucleotide codons which encodes it and the resulting nucleotide sequences were used as the input sequence for the models, either as unigrams or trigrams.

## Results

Binary classification of proteins based on whether or not they bind metal ions was chosen as a model classification task. Of the 20412 human proteins in UniProt, 2282 are labeled by the [Gene Ontology (GO) database](http://geneontology.org/) of protein annotations as having metal ion binding properties. I used these proteins as the positive set and the remaining 18130 as the negative set, resulting in a moderately imbalanced classification task.

The dataset was randomly undersampled by 75% to improve computational tractability and then divided into 5 folds. Each was successively held out as the test set for model training, with the remaining 4 serving as the training set (5-fold cross-validation). In each fold, the training set was randomly oversampled to bring it into balance with the test set, with the newly-added copies having some of their amino acids replaced by chemically similar amino acids or alanine at several preset probabilties.

To determine the generalizability of each augmentation's effect across model types, both a convolutional neural network (CNN) and a long short-term memory (LSTM) network were trained on the data and evaluated after limited hyperparameter tuning. Training proceeded until test loss had not improved for 2 epochs. Details of the data processing, models, and training methods can be found in the included Jupyter notebook.

![](https://github.com/mcunningham1440/protein-augmentation/blob/main/assets/fig_1a.png)
![](https://github.com/mcunningham1440/protein-augmentation/blob/main/assets/fig_1b.png)

***Figure 1.** Test AUC of models trained on data with different probabilities of amino acid substitution with **A** a similar amino acid or **B** alanine. Average of 5 folds. Error bars reprsent standard error of the mean. Note the differing vertical scales*

Substituting similar amino acids during oversampling, known as dictionary substitution (Fig. 1A), resulted in small but consistent performance gains over simple oversampling at a 1%, 5%, and 15% probability of substitution, with the largest gain coming at 5%. Performance did not begin to fall substantially until the substitution probability reached 50%. Alanine substitution (Fig. 1B), by contrast, drastically reduced performance, particularly at substitution probabilities above 5%. At 15% and 50% substitution, AUC actually fell below 0.5, indicating that the model was performing worse than random chance. Given that only the positives in the training were oversampled, it is likely that this was due to the networks learning to recognize sequences with large numbers of alanines as positive, a feature which was not present in the test set. Performance curves were strikingly similar between the CNN and LSTM models, indicting that the performance changes induced by dictionary and alanine substitution are generally applicable across model architectures.

These results demonstrated that dictionary substitution can enhance classification performance, a conclusion not clearly displayed in Shen et al. However, as dictionary substitution was not evaluated side-by-side with nucleotide substitution in Minot et al., it has not been shown which is superior. Using the same CNN and LSTM architectures, I compared the effect of the two substitution methods on performance in the metal ion binding task. Trigram substitution, or replacement of each amino acid with a randomly chosen codon which encodes it followed by tokenization of the codons as trigrams, performed the best in Minot et al. and was selected for usage in this study. As there are 61 nucleotide codons which code for amino acids, the tokenizer library size for nucleotide substitution is 61, while that for amino acid substitution methods such as dictionary substitution methods is 20. A  probability of 5% was used for dictionary substitution as it performed best in Fig. 1.

![](https://github.com/mcunningham1440/protein-augmentation/blob/main/assets/fig_2.png)

***Figure 2.** Test AUC of models trained with different substitution methods. Average of 5 folds. Error bars reprsent standard error of the mean*

Dictionary substitution notably increased AUC for the LSTM model, consistent with the results shown in Fig. 1A, while not appreciably changing it for the CNN model. Nucleotide substitution, in contrast, resulted in a slight diminution of performance across both models. While this substitution method may improve model generalizability in other amino acid sequence classification or regression tasks, it appears to be counterproductive in this particular example of protein activity classification.

To determine whether the increase in performance over baseline for 5% dictionary substitution was a consistent phenomenon, I conducted similar experiments using several other protein classification tasks (Table 1).

| Task                 | Positives | Negatives | Database           | Description                                                          |
| -------------------- | --------- | --------- | ------------------ | -------------------------------------------------------------------- |
| Druggability         | 704       | 19708     | NIH Pharos         | Is the protein the target of an FDA-approved drug (labeled "Tclin")? |
| Pol II transcription | 984       | 19428     | Gene Ontology (GO) | Is the protein involved in RNA polymerase II transcription?          |
| Liver cancer         | 139       | 20273     | NIH Pharos         | Is the protein involved in liver cancer?                             |

***Table 1.** Classification tasks used to assess dictionary substitution performance*

For each task, the dataset was augmented with 5% dictionary substitution or simple oversampling of the minority class, and used to fit a CNN and LSTM model.

![](https://github.com/mcunningham1440/protein-augmentation/blob/main/assets/fig_3a.png)
![](https://github.com/mcunningham1440/protein-augmentation/blob/main/assets/fig_3b.png)

***Figure 3.** Test AUC of **A** convolutional network or **B** LSTM models trained with simple oversampling or dictionary substitution. Average of 5 folds. Error bars reprsent standard error of the mean*

## Conclusion
