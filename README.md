# Genetic Engineering Attribution

## Introduction

Your goal is to create an algorithm that identifies the most likely lab-of-origin for genetically engineered DNA.

Applications for genetic engineering are rapidly diversifying. Researchers across the world are using powerful new techniques in synthetic biology to solve some of the world’s most pressing challenges in medicine, agriculture, manufacturing and more. At the same time, increasingly powerful genetically engineered systems could yield unintended consequences for people, food crops, livestock, and industry. These incredible advances in capability demand tools that support accountable innovation.

Genetic engineering attribution is the process of identifying the source of a genetically engineered piece of DNA. This ability ensures that scientists who have spent countless hours developing breakthrough technology get their due credit, intellectual property is protected, and responsible innovation is promoted. By connecting a genetically engineered system with its designers, society can examine the policies, processes, and decisions that led to its creation. As has been observed in other disciplines, reducing anonymity encourages more prudent behavior within scientific and entrepreneurial communities—without stifling innovation.

Development of attribution capabilities is critical for the maturation of genetic engineering as a field, protecting the significant benefits it promises society while promoting accountability, responsibility, and dialog. In this competition, we challenge you to advance the state-of-the-art in this exciting new domain!


```python
#Step:1 - Load Dataset
import nbconvert
import pandas as pd
train_df = pd.read_csv("G:/DataScienceProject/Drivendata-Genetic-Engineering-Attribution/train_values.csv")
train_label_df = pd.read_csv("G:/DataScienceProject/Drivendata-Genetic-Engineering-Attribution/train_labels.csv")
train_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sequence_id</th>
      <th>sequence</th>
      <th>bacterial_resistance_ampicillin</th>
      <th>bacterial_resistance_chloramphenicol</th>
      <th>bacterial_resistance_kanamycin</th>
      <th>bacterial_resistance_other</th>
      <th>bacterial_resistance_spectinomycin</th>
      <th>copy_number_high_copy</th>
      <th>copy_number_low_copy</th>
      <th>copy_number_unknown</th>
      <th>...</th>
      <th>species_budding_yeast</th>
      <th>species_fly</th>
      <th>species_human</th>
      <th>species_mouse</th>
      <th>species_mustard_weed</th>
      <th>species_nematode</th>
      <th>species_other</th>
      <th>species_rat</th>
      <th>species_synthetic</th>
      <th>species_zebrafish</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9ZIMC</td>
      <td>CATGCATTAGTTATTAATAGTAATCAATTACGGGGTCATTAGTTCA...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5SAQC</td>
      <td>GCTGGATGGTTTGGGACATGTGCAGCCCCGTCTCTGTATGGAGTGA...</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>E7QRO</td>
      <td>NNCCGGGCTGTAGCTACACAGGGCGGAGATGAGAGCCCTACGAAAG...</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CT5FP</td>
      <td>GCGGAGATGAAGAGCCCTACGAAAGCTGAGCCTGCGACTCCCGCAG...</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7PTD8</td>
      <td>CGCGCATTACTTCACATGGTCCTCAAGGGTAACATGAAAGTGATCC...</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 41 columns</p>
</div>




```python
train_label_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sequence_id</th>
      <th>00Q4V31T</th>
      <th>012VT4JK</th>
      <th>028IO5W2</th>
      <th>03GRNN7N</th>
      <th>03Y3W51H</th>
      <th>09MQV1TY</th>
      <th>0A4AHRCT</th>
      <th>0A9M05NC</th>
      <th>0B9GCUVV</th>
      <th>...</th>
      <th>ZQNGGY33</th>
      <th>ZSHS4VJZ</th>
      <th>ZT1IP3T6</th>
      <th>ZU6860XU</th>
      <th>ZU6TVFFU</th>
      <th>ZU75P59K</th>
      <th>ZUI6TDWV</th>
      <th>ZWFD8OHC</th>
      <th>ZX06ZDZN</th>
      <th>ZZJVE4HO</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9ZIMC</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5SAQC</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>E7QRO</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CT5FP</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>7PTD8</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1315 columns</p>
</div>




```python
#Step2 - Check for reduce unuseful columns
a = list(train_df)
for i, value in enumerate(a):
    if len(train_df[value].unique()) < 2:
        print(value)

a = list(train_label_df)
for i, value in enumerate(a):
    if len(train_label_df[value].unique()) < 2:
        print(value)
```

At this part, we runs 2 type of classifications:
- Use the train_df without 'sequence' column to classify our labels
- Use only 'sequence' as text sequense to classify our labels


```python
#Step3 - Build new dataframe for classifiy sequence to labels
df3 = train_label_df
df3 = df3.drop(['sequence_id'], axis=1)
df3['sequence'] = train_df['sequence']
df3.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>00Q4V31T</th>
      <th>012VT4JK</th>
      <th>028IO5W2</th>
      <th>03GRNN7N</th>
      <th>03Y3W51H</th>
      <th>09MQV1TY</th>
      <th>0A4AHRCT</th>
      <th>0A9M05NC</th>
      <th>0B9GCUVV</th>
      <th>0CL7QVG8</th>
      <th>...</th>
      <th>ZSHS4VJZ</th>
      <th>ZT1IP3T6</th>
      <th>ZU6860XU</th>
      <th>ZU6TVFFU</th>
      <th>ZU75P59K</th>
      <th>ZUI6TDWV</th>
      <th>ZWFD8OHC</th>
      <th>ZX06ZDZN</th>
      <th>ZZJVE4HO</th>
      <th>sequence</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>CATGCATTAGTTATTAATAGTAATCAATTACGGGGTCATTAGTTCA...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>GCTGGATGGTTTGGGACATGTGCAGCCCCGTCTCTGTATGGAGTGA...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>NNCCGGGCTGTAGCTACACAGGGCGGAGATGAGAGCCCTACGAAAG...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>GCGGAGATGAAGAGCCCTACGAAAGCTGAGCCTGCGACTCCCGCAG...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>CGCGCATTACTTCACATGGTCCTCAAGGGTAACATGAAAGTGATCC...</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1315 columns</p>
</div>




```python
#Step4 - Fastai
from fastai.text import *
```


```python
data = (TextList.from_df(df3, cols='sequence')
                .split_by_rand_pct(0.2)
                .label_for_lm()  
                .databunch(bs=48))
data.show_batch()
```










<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>idx</th>
      <th>text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>c xxup atcggcgacgg xxrep 4 c xxup gtgctgctgcccgacaaccactacctgagcacccagtccgccctgagcaaaga xxrep 4 c xxup xxunk xxrep 4 c xxup gtggatgaagacggattgccctcatttgatgcactgacagatggagccgtgaccactgacaacgaggccagtccttcctccatgcctgacggca xxrep 4 c t xxrep 5 c xxup tcaggaggcagaagagccgtctctacttaagaagctcttactggcaccagccaacactcagctcagctacaatgaatgcagcggtcttagcactcagaaccatgcagcaaaccacacccacaggatcagaacaaaccctgccattgttaagaccgagaattcatggagcaataaagcgaagagcatttgtcaacagc xxrep 4 a xxup gccacaaagacgtccctgctcagagcttctcaagtatctgaccacaaacgatgaccctcctcacaccaaacccacag xxrep 4 a xxup caggaacagcagcagagacaaatgtgcttcc xxrep 5 a xxup gaagtcccatacacaaccgcagtcgcaacatgctcaagccaaaccaacaactttatctcttcctctga xxrep 4 c xxup agagtcaccaaatga xxrep 4 c xxup aagggtt xxrep 4 c xxup atttgagaacaagactattgagcgaaccttaagtgtggaactctctggaactgcaggcctaactcctcccacaactcctcctcataaagccaaccaagataaccctttcaaggcttcgccaaagctgaagccctcttgcaagaccgtggtgccaccgccaaccaagagggcccggtacagtgagtgttctggtacccaaggcagccactccaccaagaaagggcccgagcaatctgagttgtacgcacaactcagcaagtcctcagggctcagccgaggacacgaggaaaggaagactaaacggcccagtctccggctgtttggtgaccatgactactgtcagtcactcaattcc xxrep 4 a xxup cggatatactcattaacatatcacaggagctccaagactctagacaactagacttcaaagatgcctcctgtgactggca xxrep 4 g xxup cacatctgttcttccacagattcaggccagtgctacctgagagagactttggaggccagcaagcaggtctctccttgcagcaccag xxrep 4 a</td>
    </tr>
    <tr>
      <td>1</td>
      <td>xxunk xxrep 5 t xxup xxunk xxrep 4 t xxup xxunk xxrep 5 t xxup xxunk xxrep 5 t xxup xxunk xxrep 5 a xxup xxunk xxrep 5 a xxup xxunk xxrep 8 t xxup xxunk xxrep 4 a xxup xxunk xxrep 4 t xxup xxunk xxrep 5 t xxup xxunk xxrep 7 a xxup tgctttatttgtgaaatttgtgatgctattgctttatttgtaaccattataagctgcaataaacaagttaacaacaacaattgcattca xxrep 4 t xxup atgtttcaggttca xxrep 5 g xxup aggtgtgggagg xxrep 6 t xxup</td>
    </tr>
    <tr>
      <td>2</td>
      <td>4 c xxup tccgacgg xxrep 4 c xxup gtaatgcagaagaagactatgggctgggagccctccaccgagcgcctgta xxrep 5 c xxup xxunk xxrep 4 a xxup cgaaaggctcagtcgaaagactgggcctttcg xxrep 4 t xxup atctgttgtttgtcggtgaacgctctcctgagtaggacaaatccgccgggagcggatttgaacgttgcgaagcaacggcccggagggtggcgggcaggacgcccgccataaactgccagggaattccaactgagcgccggtcgctaccattaccaacttgtctggtgtc xxrep 5 a xxup taatagga xxrep 4 t xxup gtt xxrep 4 a xxup ttcgcgttaaa xxrep 5 t xxup gttaaatcagctca xxrep 6 t xxup aaccaataggccgactgcgatgagtggcagggc xxrep 4 g xxup cgtaa xxrep 7 t xxup aaggcagttattggtgcccttaaacgcctggtgctacgcctgaataagtgataataagcggatgaatggcagaaattcgaaagcaaattcgacccggtcgtcggttcagggcagggtcgttaaatagccgcttatgtctattgctggtttaccggtttattgactaccggaagcagtgtgaccgtgtgcttctcaaatgcctgaggccagtttgctcaggctct xxrep 4 c xxup gtggaggtaataattgacgatatgatcatttattctgcctcccagagcctgat xxrep 5 a xxup cggtgaatccgttagcgaggtgccgccggcttccattcaggtcgaggtggcccggctccatgcaccgcgacgcaacgc xxrep</td>
    </tr>
    <tr>
      <td>3</td>
      <td>xxrep 4 c xxup aaggacctgaaatgaccctgtgccttatttgaactaaccaatcagttcgcttctcgcttctgttcgcgcgcttctgct xxrep 4 c xxup gagctcaat xxrep 4 a xxup gagcccacaa xxrep 4 c xxup tcactc xxrep 4 g xxup cgccagtcctccgattgactgagtcgcccgggtacccgtgtatccaataaaccctcttgcagttgcatccgacttgtggtctcgctgttccttgggagggtctcctctgagtgattgactacccgtcagc xxrep 5 g xxup tctttcatttccgacttgtggtctcgctgccttgggagggtctcctctgagtgattgactacccgtcagc xxrep 5 g xxup tcttcacatgcagcatgtatc xxrep 4 a xxup ttaatttgg xxrep 8 t xxup cttaagtatttacattaaatggccatagttgcattaatgaatcggccaacgcgc xxrep 4 g xxup agaggcggtttgcgtattggcgctcttccgcttcctcgctcactgactcgctgcgctcggtcgttcggctgcggcgagcggtatcagctcactcaaaggcggtaatacggttatccacagaatca xxrep 4 g xxup ataacgcaggaaagaacatgtgagc xxrep 4 a xxup ggccagc xxrep 4 a xxup ggccaggaaccgt xxrep 5 a xxup ggccgcgttgctggcg</td>
    </tr>
    <tr>
      <td>4</td>
      <td>xxup ggatctaggtgaagatcc xxrep 5 t xxup gataatctcatgacc xxrep 4 a xxup tcccttaacgtgag xxrep 4 t xxup cgttccactgagcgtcaga xxrep 4 c xxup gtag xxrep 4 a xxup gatcaaaggatcttcttgagatcc xxrep 7 t xxup ctgcgcgtaatctgctgcttgcaaac xxrep 7 a xxup ccaccgctaccagcggtggtttgtttgccggatcaagagctaccaactc xxrep 5 t xxup ccgaaggtaactggcttcagcagagcgcagataccaaatactgtccttctagtgtagccgtagttaggccaccacttcaagaactctgtagcaccgcctacatacctcgctctgctaatcctgttaccagtggctgctgccagtggcgataagtcgtgtcttaccgggttggactcaagacgatagttaccggataaggcgcagcggtcgggctgaac xxrep 6 g xxup ttcgtgcacacagcccagcttggagcgaacgacctacaccgaactgagatacctacagcgtgagctatgagaaagcgccacgcttcccgaagggagaaaggcggacaggtatccggtaagcggcagggtcggaacaggagagcgcacgagggagcttcca xxrep 5 g xxup aaacgcctggtatctttatagtcctgtcgggtttcgccacctctgacttgagcgtcga xxrep 5 t xxup gtgatgctcgtca xxrep 6 g xxup cggagcctatgg xxrep 5 a xxup cgccagcaacgcggcc xxrep 5 t</td>
    </tr>
  </tbody>
</table>



```python
learn = language_model_learner(data,AWD_LSTM, drop_mult=0.3)
```

    Downloading https://s3.amazonaws.com/fast-ai-modelzoo/wt103-fwd
    






```python
learn.lr_find()
```



    <div>
        <style>
            /* Turns off some styling */
            progress {
                /* gets rid of default border in Firefox and Opera. */
                border: none;
                /* Needs to be in here for Safari polyfill so background images work as expected. */
                background-size: auto;
            }
            .progress-bar-interrupted, .progress-bar-interrupted::-webkit-progress-bar {
                background: #F44336;
            }
        </style>
      <progress value='0' class='' max='1' style='width:300px; height:20px; vertical-align: middle;'></progress>
      0.00% [0/1 00:00<00:00]
    </div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>epoch</th>
      <th>train_loss</th>
      <th>valid_loss</th>
      <th>accuracy</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table><p>

    <div>
        <style>
            /* Turns off some styling */
            progress {
                /* gets rid of default border in Firefox and Opera. */
                border: none;
                /* Needs to be in here for Safari polyfill so background images work as expected. */
                background-size: auto;
            }
            .progress-bar-interrupted, .progress-bar-interrupted::-webkit-progress-bar {
                background: #F44336;
            }
        </style>
      <progress value='99' class='' max='6216' style='width:300px; height:20px; vertical-align: middle;'></progress>
      1.59% [99/6216 00:27<28:03 4.4282]
    </div>



    LR Finder is complete, type {learner_name}.recorder.plot() to see the graph.
    


```python
learn.recorder.plot()
```


![png](output_11_0.png)



```python
learn.fit_one_cycle(1, 1e-2, moms=(0.8,0.7))
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>epoch</th>
      <th>train_loss</th>
      <th>valid_loss</th>
      <th>accuracy</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.330817</td>
      <td>0.262013</td>
      <td>0.947483</td>
      <td>31:19</td>
    </tr>
  </tbody>
</table>



```python
learn.save_encoder('fine_tuned_enc')
```


```python
label_cols = list(df3)[:-1]
label_cols
```




    ['00Q4V31T',
     '012VT4JK',
     '028IO5W2',
     '03GRNN7N',
     '03Y3W51H',
     '09MQV1TY',
     '0A4AHRCT',
     '0A9M05NC',
     '0B9GCUVV',
     '0CL7QVG8',
     '0CML4B5I',
     '0DTHTJLJ',
     '0FFBBVE1',
     '0HWCWFNU',
     '0L3Y6ZB2',
     '0M44GDO8',
     '0MDYJM3H',
     '0N3V9P9M',
     '0NP55E93',
     '0PJ91ZT6',
     '0R296F9R',
     '0T2AZBD6',
     '0URA80CN',
     '0VRP2DI6',
     '0W6O08VX',
     '0WHP4PPK',
     '0XPTGGLP',
     '0XS4FHP3',
     '0Y24J5G2',
     '10TEBWK2',
     '11TTDKTM',
     '131RRHBV',
     '13LZE1F7',
     '14PBN8C2',
     '15D0Z97U',
     '15S88O4Q',
     '18C9J8EH',
     '19CAUKJB',
     '1AP294AT',
     '1B9BJ2IP',
     '1BE35FI1',
     '1CIHYCE4',
     '1DJ9L58E',
     '1DTDCRUO',
     '1EDZ6CA7',
     '1HCQTAYT',
     '1HK4VXP8',
     '1IXFZ3HO',
     '1K11RCST',
     '1KC6XYO6',
     '1KNFJ6KQ',
     '1KZHNVYR',
     '1LBGAU5Z',
     '1NXRMDN6',
     '1OQJ21E9',
     '1OWZDF82',
     '1PA232PA',
     '1PIGWQFY',
     '1Q1IUY3G',
     '1S515B69',
     '1TC200QC',
     '1TI4HS4X',
     '1UOA7CA1',
     '1UREJUSJ',
     '1UU0CHTK',
     '1VPOX8VI',
     '1VQS4WNS',
     '1X0VC0O1',
     '1XU60MET',
     '1ZC8RPN1',
     '20ABQYHS',
     '20CEB9KE',
     '216DWMG6',
     '21ZFBX5E',
     '24SL2992',
     '25UVYUID',
     '26KK8UM5',
     '27OS3BTP',
     '28D4D4QM',
     '298AMR5C',
     '29D6Q091',
     '2AQG6I31',
     '2BAFY4GP',
     '2CJHRNWD',
     '2FCX4O0X',
     '2GGU2QA2',
     '2GSZMU46',
     '2GTLIT33',
     '2H37WPKA',
     '2HNZZYDB',
     '2JPNC9X6',
     '2KDACBQT',
     '2L336TQL',
     '2M3CXS8N',
     '2MCB7LXW',
     '2MQ2NPMA',
     '2NEXWXMT',
     '2PY8K6GU',
     '2Q33W599',
     '2SSVM7H9',
     '2TVMHQTW',
     '2TXY439E',
     '2VP4JPB9',
     '2VTLZHDS',
     '2VX4F6RC',
     '2XC1478M',
     '2XX0N87I',
     '2Y9L13L4',
     '2YCH1PUI',
     '2YLQA8OZ',
     '303BN0Z0',
     '318RH8P0',
     '330L4OIV',
     '33AR5KVE',
     '343M819H',
     '34TE1Q0A',
     '35MKXPL0',
     '36W150XW',
     '36XLYYGZ',
     '37VO60SB',
     '384ASNLB',
     '38MDETY1',
     '38MEQ4SU',
     '39LLQ2PB',
     '39TEZ0C3',
     '39TPBOL7',
     '3BGLF8BC',
     '3C2VZQ2R',
     '3C952KY7',
     '3D9CMQ4V',
     '3EARN0Z7',
     '3EYBG174',
     '3EZXYI3U',
     '3FPH0N6R',
     '3FW33G68',
     '3GEXBRC0',
     '3KCEM7V4',
     '3L314D8W',
     '3LSNTL1N',
     '3MDRJUI2',
     '3MX1D3LD',
     '3N169DM2',
     '3NSJ6N02',
     '3O1GIAV7',
     '3QP4D23X',
     '3RK54JUW',
     '3TLD81QQ',
     '3TUFYWQN',
     '3TXFYNKG',
     '3X2GGDHW',
     '3XE0BJDW',
     '3YAQWNBK',
     '3YEGUN04',
     '3YYEC52Y',
     '40MD0YZ3',
     '40ZI3TDN',
     '443NZOSB',
     '448QVC4C',
     '44N2CYI9',
     '459BZKP3',
     '4648UZGD',
     '46AZ97U9',
     '48F0EUVN',
     '49571DXY',
     '49YZILWR',
     '4CKAV3LS',
     '4DGGCYVE',
     '4DGMNDIC',
     '4E7187A9',
     '4GF31RCS',
     '4GHCND6Z',
     '4IADYZ8R',
     '4IDTMY10',
     '4J7KEYE2',
     '4KSHU5M7',
     '4LCFACE1',
     '4LQ8L195',
     '4M3XG8RC',
     '4O39WLXM',
     '4O5RQHEF',
     '4PKCMX7O',
     '4QK5ZDHA',
     '4QU07FT7',
     '4RCA1UZG',
     '4RHLX089',
     '4S1LIWGV',
     '4TIT4L5F',
     '4U5LAAN5',
     '4VHMF1RI',
     '4WAQ4VFB',
     '4WRI77CU',
     '4X2RTV2D',
     '4Y4DT3SL',
     '4ZYW54M8',
     '50NBGIOB',
     '52Y9GFGK',
     '54C6PEBH',
     '54ZFOPSF',
     '558GIQ68',
     '55HTZ7T0',
     '579G0TJI',
     '57FHO8YC',
     '57NGF1YS',
     '58BSUZQB',
     '5ASQZ0OT',
     '5AUVXXDU',
     '5BNUT8AW',
     '5BTY65G6',
     '5CBNCRST',
     '5FUDT1QA',
     '5H71LUBY',
     '5K2PTY6L',
     '5KXWXV9G',
     '5LH9NUMK',
     '5OBD73W0',
     '5OF7OYEA',
     '5OFUVG9U',
     '5PC2F8NE',
     '5PR9OSRS',
     '5Q9ETXJL',
     '5QLBIUXN',
     '5QY2HU8J',
     '5SCOFTY2',
     '5SGMS705',
     '5V3Z108E',
     '5W2PCT95',
     '5X9VNAN3',
     '5Z4CMIY5',
     '5ZB8I3T0',
     '5ZW05824',
     '60HBQEP8',
     '62PKSARW',
     '638UYIQC',
     '64FFXH4M',
     '65CCBIXK',
     '669R7ER0',
     '66XSSS3Q',
     '685KTH3G',
     '68OY1RK5',
     '69M351P4',
     '6AT20D5S',
     '6DBY872A',
     '6E28DNQK',
     '6KT0EAKX',
     '6LQ0W02R',
     '6NCTAA30',
     '6NKNB308',
     '6NULQ6KP',
     '6PS2LHCV',
     '6PXRABDR',
     '6QBXXYN4',
     '6QUCW04X',
     '6SBB6IL2',
     '6T9SGGS1',
     '6TT5CXVI',
     '6TTWEXT3',
     '6UGWNYCX',
     '6UI9XACW',
     '6UXF7L28',
     '6WD2LIHN',
     '6WT1F4RJ',
     '6XVBD39G',
     '6YSX60MZ',
     '7039MMH2',
     '709K4VRB',
     '7185O9V8',
     '71R7TM8L',
     '738FBTIL',
     '73RKEO3U',
     '747XMBIJ',
     '74RXUGS4',
     '74TS5KG4',
     '78QGAL01',
     '78XDAJNS',
     '7ANCD9AK',
     '7DMNXU84',
     '7E63E5RD',
     '7F905YRZ',
     '7GWW4637',
     '7IHPTKFF',
     '7KG191H8',
     '7MUAYEHW',
     '7NGLQ1CA',
     '7O3PWIL0',
     '7OV5K86R',
     '7PWA4ZJN',
     '7QEORFJN',
     '7QF2VB5B',
     '7QWHL2C6',
     '7SW79VAJ',
     '7T28F53W',
     '7TYZHD5J',
     '7UU8O65I',
     '7WKS90AG',
     '7X3RSRT5',
     '7XPDUYJE',
     '7XU8ACPI',
     '7YSTNZME',
     '7ZV0Z1T9',
     '81QAZACE',
     '82NXGO4K',
     '862RYK1K',
     '86ET7WW4',
     '88E6O06E',
     '8ABA3MWO',
     '8BF8ANNO',
     '8C0T09C6',
     '8C9737JL',
     '8D4D6M5V',
     '8ECLELF1',
     '8EKC599S',
     '8F0XPAZX',
     '8FT6HD4D',
     '8FZMCIFG',
     '8G29TDOS',
     '8H6M75LF',
     '8HI3GY44',
     '8HW91I4K',
     '8HZXGARR',
     '8IPYO6SS',
     '8JKDTT0Y',
     '8K0HZBL0',
     '8MUKKVMF',
     '8MW998Z0',
     '8N5EPD5C',
     '8OBT3FSQ',
     '8ORZZFA7',
     '8RIKS696',
     '8SW7WFE6',
     '8T12OXHS',
     '8US76O46',
     '8VCFY56I',
     '8VI1RY3M',
     '8VLB2R3D',
     '8WAY3T1E',
     '8Z6SANMH',
     '8ZB94ICE',
     '8ZB99KHH',
     '904V6V2S',
     '909V5A2H',
     '91Y7NKBM',
     '91Z8RRSB',
     '92WF5WVN',
     '93R70J1L',
     '93WIIL7Y',
     '97FR69TQ',
     '97PR85CP',
     '99A19JAD',
     '9DBCRJYM',
     '9DKQF2I2',
     '9DRMDPIZ',
     '9G5XH4HI',
     '9GDHC3D0',
     '9HPM9NFY',
     '9HRDSOST',
     '9IVIPDX5',
     '9JRKFKVC',
     '9KHXMSMW',
     '9KV8R3HP',
     '9LSH625Y',
     '9MC0DPDJ',
     '9MC1YKKZ',
     '9MEFUZQN',
     '9MG50RM7',
     '9MZBKXJF',
     '9PWYZMNS',
     '9QQZ79I6',
     '9R765PJF',
     '9SJCUIKS',
     '9SSQ1FSY',
     '9U0DELRD',
     '9WEGTUIJ',
     '9WQQKFVK',
     '9XE0FL8P',
     '9Y5EWA8O',
     '9YM3QINZ',
     '9ZTEQPA4',
     'A0ADXLZU',
     'A0Z7XCDN',
     'A1738D1Z',
     'A18S09P2',
     'A1A8EROR',
     'A1J0YXZX',
     'A2A1R52R',
     'A2U1AIC1',
     'A332O9JW',
     'A3FZPLM1',
     'A3QUOXIX',
     'A44GW57T',
     'A4BM0B6A',
     'A6RCKKER',
     'A768XIWP',
     'A78F2YFJ',
     'A7CK3WNB',
     'A810BWR5',
     'A8FZHMOS',
     'A9G8OKRG',
     'AATDRXYQ',
     'AAURK3RG',
     'ABMAPCYN',
     'ABWCZWFU',
     'ACO8WWPF',
     'ADB7SAPN',
     'AG93GZYN',
     'AHMVJ2VP',
     'AL7N3DL2',
     'AM8AJH2H',
     'AMSPTQVJ',
     'AMV4U0A0',
     'AOCCEP3S',
     'AOFJN8HX',
     'AOFPYGHC',
     'AOKRU4AF',
     'AOQQU910',
     'AR433PVR',
     'AS30HPUK',
     'AUCMR8HU',
     'AUUSW2YZ',
     'AUZNSS79',
     'AV7ONIVD',
     'AWWC1KIV',
     'B131HDBV',
     'B17J3JSX',
     'B1I4L0XW',
     'B25KOPVH',
     'B2BULVFH',
     'B4L9R8JU',
     'B517ID6W',
     'B832TQ6U',
     'B8FC99WI',
     'B8YR9IIK',
     'B9H5SLHK',
     'BBTA1L43',
     'BBZJCYJ0',
     'BD9EXLDM',
     'BDQOSDFG',
     'BDSEVK9M',
     'BH7HW7XH',
     'BHKOO62U',
     'BHNI9DCI',
     'BHW9ILRC',
     'BJKTDFN4',
     'BL2TLVFC',
     'BLC9WIIM',
     'BLFM4YKK',
     'BLNELN02',
     'BN8BMXPM',
     'BNFZZTKX',
     'BP2X9ITX',
     'BPT27UPE',
     'BQJ79YS3',
     'BSEEWS00',
     'BSH6LB19',
     'BTQL3UFQ',
     'BV6PVSO5',
     'BV8D4RYV',
     'BWFN4ZI7',
     'BXMEKONO',
     'BY5IEG4O',
     'BZBNZDNS',
     'C1BIUBL5',
     'C35C2C2W',
     'C4W63WJ2',
     'CA0MBQ9S',
     'CAO2H0WE',
     'CAQEITX6',
     'CB714TAM',
     'CBCQST29',
     'CBFKYZ9S',
     'CBKRHK4I',
     'CDM3SRRP',
     'CDU1LWN3',
     'CEATO4LM',
     'CENOJ84D',
     'CFDEOSH4',
     'CFOET28L',
     'CFQ9PAJA',
     'CHTQ7QLX',
     'CJFLQNE1',
     'CK1M5UHL',
     'CKDZNQV2',
     'CLO7VQ12',
     'CNX48K3H',
     'COEMYLH1',
     'COVE5WRD',
     'CRP30ATM',
     'CTJGWLX0',
     'CTLP20Y9',
     'CWZP8AQK',
     'CY64689U',
     'CYCSYMQ3',
     'CZUGPH88',
     'D0EKC82X',
     'D0NFHXL2',
     'D0YWREJ5',
     'D10S0UDQ',
     'D1BZRMOB',
     'D2N5DOSQ',
     'D3KJQCYH',
     'D4PJE56U',
     'D4Q1QMRJ',
     'D63K976U',
     'D7L6VZNV',
     'D8MRQA91',
     'D8OQ3YNK',
     'DD0JBK3T',
     'DE6NAU7D',
     'DEFNZK0A',
     'DEWKAO5I',
     'DGE8LLAJ',
     'DGQ2L6KM',
     'DJW5U56I',
     'DKA65CRR',
     'DLSU0QRX',
     'DN01XVIU',
     'DQGG01WF',
     'DRFCUPZO',
     'DSE2G8LF',
     'DY0KIZZ9',
     'DZ2XFGQS',
     'E3CE5WE9',
     'E3CRPQL7',
     'E3FFACSU',
     'E4EF2K0A',
     'E4T4IQMG',
     'E59C5N01',
     'E5OB5QF1',
     'E6G69ESA',
     'E6TPDVWA',
     'E7CPRIYW',
     'E7EZD62E',
     'E8100WU0',
     'E8GMEHFW',
     'EA2DKNTD',
     'EBF1G8Z7',
     'ED0OS5OF',
     'EEC8D29F',
     'EFKGYR79',
     'EI8B4WEC',
     'EJ3T17DB',
     'EJXP2QAW',
     'EKHYS325',
     'EKXAPD70',
     'EL9FN1LB',
     'ELF2BN3S',
     'ELX1D1DS',
     'EMJXDINV',
     'EMNH5MYX',
     'EN78WKI4',
     'EOQAQ9X1',
     'EPDX32D3',
     'EQPB3YTZ',
     'ER1IJR80',
     'ETR2SP13',
     'EW4ZXWSN',
     'EXQZ5V7S',
     'EYOJGC9T',
     'EZ40BRHE',
     'EZL4HNHH',
     'EZMV5TKG',
     'F0ESSJYM',
     'F0MOWJYA',
     'F1X6DMDH',
     'F3D2JAYU',
     'F3S4VUQI',
     'F50DBVIK',
     'F8I0DT7Z',
     'F8LNIZ27',
     'FCI1HZ3G',
     'FEBWERSN',
     'FH8TEJI1',
     'FHR8UUYO',
     'FHZYKEUV',
     'FJTJ4KY0',
     'FLHGDG0P',
     'FLSWA4NU',
     'FLU9ZT18',
     'FMJ19E48',
     'FN1RKQ2M',
     'FN38BX60',
     'FNKCHGB7',
     'FNM1Z945',
     'FPH5H8JT',
     'FQ8V2QHL',
     'FRFT0H8N',
     'FRK40JVP',
     'FRX9XJYW',
     'FSR0IC6I',
     'FVYCRUFK',
     'FWOZ05UZ',
     'FXBIP7LS',
     'FXRWH0M9',
     'FZ37IFWH',
     'G2P73NZ0',
     'G4UJDFPK',
     'G57JANUL',
     'G6MP6EIN',
     'G7MXLRV8',
     'G81LO0AZ',
     'G8QWQL1C',
     'GB45D1XV',
     'GBX3MNVS',
     'GDV3S3ZG',
     'GHG5MDER',
     'GJKR73YA',
     'GJPI1WIV',
     'GKY6ZB15',
     'GKY7BZOQ',
     'GLOJFBA0',
     'GLUZC5HC',
     'GM3HKY2J',
     'GS8G1IFF',
     'GSK9JT39',
     'GSNU5TXL',
     'GT4RHNUE',
     'GTVTUGVY',
     'GUCIE6TT',
     'GUWYJRRS',
     'GWJ0A1IK',
     'GWP6E8FA',
     'GYCOAVYS',
     'GYCY8LCF',
     'GZMPRX5J',
     'H0WSDLJE',
     'H12S8X2Q',
     'H1G4FFR7',
     'H20JGHP0',
     'H3D82ATM',
     'H3RWZ7UR',
     'H48Y5BOY',
     'H5Y73UHQ',
     'H9RBDN30',
     'HB3OQUA5',
     'HCW1Y9QM',
     'HGN5HD65',
     'HGPS0FQN',
     'HHSIC4NY',
     'HI7ZNYCK',
     'HJNGSDJ5',
     'HK78MCH7',
     'HNGYSI62',
     'HODOBX62',
     'HQC2OFGM',
     'HRFD8R1G',
     'HRWBEBRE',
     'HT51BMN1',
     'HTXABMRS',
     'HV6GZXC3',
     'HVAG84XI',
     'HVBBJM37',
     'HVN93I56',
     'HVXSID0M',
     'HVZMFFNW',
     'HX2XDS73',
     'HX5NMCPJ',
     'HY9DN23J',
     'HZ5C2E4C',
     'I0J54PBT',
     'I16TS2B4',
     'I1RQMFZC',
     'I2ATV1DI',
     'I2N7C27Y',
     'I3UODLOR',
     'I5L6E1U2',
     'I5RNBXF3',
     'I6B3VKYD',
     'I7FXTVDP',
     'I8U0Q5FP',
     'I9MWC6I3',
     'IBBLXRDR',
     'ICDP084U',
     'ICRBJL24',
     'ID37U3DA',
     'IDXJ25FE',
     'IGHBC70Q',
     'IH12MVU4',
     'IIWFYXGG',
     'IJEA3NUI',
     'IL47R85Z',
     'ILKPIFSA',
     'IM2JLO1B',
     'IMFV7GM3',
     'IMVSI4VW',
     'INDCDVP0',
     'INELF20P',
     'INJ6L6NB',
     'IO2FYB6G',
     'IO56YRTG',
     'IOKPSO7K',
     'IOOQONCI',
     'IOPR6B78',
     'IP9XMFII',
     'IPV1W17S',
     'IPVYEI8G',
     'IQPZXRU2',
     'IS75OD95',
     'ISMP5LYF',
     'IUJPYIRX',
     'IYKXT23R',
     'IZD0O5Q0',
     'IZSQDCWP',
     'J0NVCXDJ',
     'J1UFMOCR',
     'J339EI56',
     'J3752QSY',
     'J3L1KD1J',
     'J3YKGOCX',
     'J5WRC3DJ',
     'J648LM1S',
     'J70NZZIW',
     'J7PWRE94',
     'J9M11KX1',
     'JAEI655A',
     'JB8JTFSG',
     'JC35D8WT',
     'JC6LUZLT',
     'JCHNPTSF',
     'JDENEZ6I',
     'JICWX3AS',
     'JJBJFUAT',
     'JK9C0VN8',
     'JKUCC6UK',
     'JL1OZP2G',
     'JMJD18BP',
     'JN497K3S',
     'JNB98WP1',
     'JNU5CAOV',
     'JO1WTZOB',
     'JPI7LZJ3',
     'JPO7CTQP',
     'JQ4YBT3Z',
     'JQ7Z5Q44',
     'JQJ499YN',
     'JRBK08H6',
     'JRDHZ51W',
     'JRRTJ3GV',
     'JS1KUAD6',
     'JS59HL6M',
     'JSEGAB8K',
     'JT4GYL2P',
     'JUC55NLK',
     'JUYW4QZ1',
     'JVWQ5HEJ',
     'JWVCJ3UR',
     'JWYYB1L5',
     'JXDP2C4M',
     'JYZ82A2B',
     'JZ1RSLKQ',
     'JZ2KQL0P',
     'JZS556ZA',
     'JZTRRSKQ',
     'K1DU5H0C',
     'K1K1AESM',
     'K212MH7P',
     'K25LXPOI',
     'K3QD4AHX',
     'K4AGNZ3R',
     'K57LN37R',
     'K83DA8K5',
     'KB0YFLBH',
     'KD7N9YDF',
     'KDW3ZVWJ',
     'KDZ388UF',
     'KF32BDPB',
     'KFWFMIUK',
     'KG943QKP',
     'KGMINGSB',
     'KH4VOX9Q',
     'KJJYCUJ7',
     'KKG07XA9',
     'KKIO1X0Z',
     'KM3OV97R',
     'KMPCXZUY',
     'KMSH5BSO',
     'KRS7ST1L',
     'KSFFKSV7',
     'KU0G64D0',
     'KUGU9MQC',
     'KUH39TQR',
     'KV5TCH8S',
     'KVLIE219',
     'KWH2Y6KA',
     'L0FS3EPM',
     'L27ULB0P',
     'L2HRYP1A',
     'L2UTYYJT',
     'L3OPGJO5',
     'L3RQSW75',
     'L3SSKU27',
     'L5AMS3QT',
     'L657W1BK',
     'L76WWQ74',
     'L78GOBQS',
     'L905DK46',
     'LDCSZOKC',
     'LF9AQIHZ',
     'LFQ6YRHV',
     'LGEAIIK8',
     'LGTP4O86',
     'LHMKC873',
     'LHNLO8Q8',
     'LKC4LOOM',
     'LKR5NGJZ',
     'LKVB0S84',
     'LL11R5T6',
     'LM6LV3JB',
     'LNTF6KP8',
     'LPBA27LH',
     'LPQY1SEL',
     'LQ6K46C8',
     'LU684LJ9',
     'LUHRMKEB',
     'LUI0TOT2',
     'LVXSGLT6',
     'LWQ8FULT',
     'LXBPBCS3',
     'LXOZJ3TV',
     'LXPTXE5K',
     'LYY8P69T',
     'M1CZ7MK8',
     'M2HPA1EK',
     'M2R84KMY',
     'M2W28OUV',
     'M3B15QGL',
     'M3MFQNC7',
     'M46L0EBU',
     'M4V0NJ97',
     'M59DNUXD',
     'M9265ASV',
     'M9PHW06O',
     'MB9HHEPN',
     'MBQUJESG',
     'MDCIP8E0',
     'MEKV5BRI',
     'MEVIH0XF',
     'MFZHQ165',
     'MGQBELNN',
     'MH0GC0GY',
     'MIUE47ZL',
     'MJR1CR7U',
     'ML1YCDCG',
     'ML5W6LDB',
     'MLGLKKI7',
     'MMU3QFIP',
     'MNV2YSWZ',
     'MOCIAZ0D',
     'MQKR83SM',
     'MQQTIYIC',
     'MQRIDTFZ',
     'MULMC195',
     'MUO5QBB6',
     'MV1CMX4O',
     'MXV7CSHI',
     'MZOM2K35',
     'N0CP1NI7',
     'N0FDUY5E',
     'N5LOOJSR',
     'N5X3YG2I',
     'N764BFJU',
     'N7BY4DKZ',
     'N8FNYI0A',
     'N8X63KYC',
     'N9I581ZL',
     'NBCZC85X',
     'ND7I48LA',
     'ND88CY09',
     'NDDT3NOB',
     'NDZT8PV3',
     'NHNLVWDR',
     'NIKHJTWP',
     'NIRCF0RK',
     'NK0S2WH6',
     'NKPC0Z4Q',
     'NKRRLD5O',
     'NMQKJMH3',
     'NNNIMDVI',
     'NPWC1BXV',
     'NQVW27OC',
     'NR26DCAB',
     'NRRH4BON',
     'NT9Y0D19',
     'NTLCS343',
     'NUOEY3LD',
     'NUSJ1NGL',
     'NUYVBFLU',
     'NWE84W10',
     'NWKWVAIA',
     'NX7I9PQG',
     'NYI75N90',
     'O1LMIA6M',
     'O3M287V6',
     'O4VJ2EV7',
     'O55K40VQ',
     'O5PJEO54',
     'O69KS0OS',
     'O7NEA7KO',
     'O8E18PJ4',
     'OAEZWMZR',
     'OAPTL0AF',
     'OB97CO94',
     'OCJ3W2EF',
     'OEGM98R5',
     'OERPDTWW',
     'OG01U0FT',
     'OJ9HCGTB',
     'OKI0Z2UO',
     'OKK933IV',
     'OKWROFEH',
     'OL1HWRRD',
     'OL59ZZX5',
     'OML0TEF3',
     'ON2CU60C',
     'ON9AXMKF',
     'ONPQ2I44',
     'OOKK1JHN',
     'OPPRIPN9',
     'OUA1CRWO',
     'OUJLF506',
     'OVPHRVOD',
     'OYRI4NVE',
     'P361G1OD',
     'P3Q11IAK',
     'P4H26KKX',
     'P8PW7Q1Q',
     'PEUBDA2B',
     'PFI6E05S',
     'PFNRAGJP',
     'PGWZZALU',
     'PHQEJTNO',
     'PIT16TZ9',
     'PJYVLL0Z',
     'PKC5LJ6W',
     'PMCWG8N5',
     'PNWFSSF0',
     'POKTJVRL',
     'PONI61NE',
     'POZMOX9T',
     'PQZ6Z3YJ',
     'PRU3JF6Y',
     'PRYT0A2P',
     'PS6MZN15',
     'PSY58O49',
     'PUECZ8ZI',
     'PV7QTHJV',
     'PW7GT7TE',
     'PXT3AJ7C',
     'PY8VPVM5',
     'PYX7I7X5',
     'Q1D88JO2',
     'Q1M9RXYR',
     'Q21CAL4Z',
     'Q2K8NHZY',
     'Q2LO2OGN',
     'Q35PXLRT',
     'Q3O4J4HB',
     'Q5V3EKJC',
     'QASMCASJ',
     'QC3VEU4P',
     'QEOKKUF1',
     'QJ5LYZHA',
     'QJJAG1IV',
     'QJMUUPFK',
     'QL3AU1NN',
     'QNE79S52',
     'QNKGHIRB',
     'QNQQVRNB',
     'QPA31HRW',
     'QQFF3LO5',
     'QQR3SE8Y',
     'QR91QBR2',
     'QSLQZQH2',
     'QT44Y8VV',
     'QTIRUM0G',
     'QUFMTUB3',
     'QUUKEGL5',
     'QV09SDY8',
     'QV71AJ91',
     'QVAHXT35',
     'QVAZPYQ8',
     'QYBCIW4J',
     'QYZ57QTQ',
     'QZ1V5GME',
     'QZ8BT14M',
     'QZD4I9UW',
     'R1BX2NZI',
     'R1OFLDKQ',
     'R2O5C424',
     'R3AAYF7V',
     'R3QOGZZF',
     'R5B3KVZI',
     'R67AMR4P',
     'R6QNKUC4',
     'R830GQGO',
     'RASRCD7I',
     'RBL3SN1I',
     'RBLPDV4R',
     'RBMLZBYW',
     'RD5YXSBA',
     'RD62G56Y',
     'RE7IER1C',
     ...]




```python
learn.export('G:/DataScienceProject/Drivendata-Genetic-Engineering-Attribution/fastai.pkl')
```


```python
test_df = pd.read_csv("G:/DataScienceProject/Drivendata-Genetic-Engineering-Attribution/test_values.csv")
```


```python
test_datalist = TextList.from_df(test_df, cols='sequence', vocab=data.vocab)

data_clas = (TextList.from_df(df3, cols='sequence', vocab=data.vocab)
             .split_by_rand_pct(0.2)
             .label_from_df(cols= label_cols , classes=label_cols)
             .add_test(test_datalist)
             .databunch(bs=32))

data_clas.show_batch()
```














<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>text</th>
      <th>target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>xxbos xxup aaattgtaaacgttaata xxrep 4 t xxup gtt xxrep 4 a xxup ttcgcgttaaa xxrep 5 t xxup gttaaatcagctca xxrep 6 t xxup aaccaataggccgaaatcggc xxrep 4 a xxup tcccttataaatc xxrep 4 a xxup gaatagaccgagatagggttgagtgttgttccagtttggaacaagagtccactattaaagaacgtggactccaacgtcaaagggcg xxrep 5 a xxup ccgtctatcagggcgatggcccactacgtgaaccatcaccctaatcaag xxrep 6 t xxrep 4 g xxup tcgaggtgccgtaaagcactaaatcggaaccctaaagggag xxrep 5 c xxup gatttagagcttgac xxrep 4 g xxup aaagccggcgaacgtggcgagaaaggaagggaagaaagcgaaaggagcgggcgctagggcgctggcaagtgtagcggtcacgctgcgcgtaaccaccacacccgccgcgcttaatgcgccgctacagggcgcgtcccattcgccattcaggctgcgcaactgttgggaagggcgatcggtgcgggcctcttcgctattacgccagctggcgaaa xxrep 5 g xxup atgtgctgcaaggcgattaagttgggtaacgccaggg xxrep 4 t xxup cccagtcacgacgttgt xxrep 4 a xxup</td>
      <td>BLFM4YKK</td>
    </tr>
    <tr>
      <td>xxbos xxup xxunk xxrep 4 t xxup xxunk xxrep 4 t xxup xxunk xxrep 4 a xxup xxunk xxrep 5 t xxup xxunk xxrep 5 a xxup xxunk xxrep 4 a xxup xxunk xxrep 4 t xxup xxunk xxrep 4 c xxup xxunk xxrep 4 t xxup xxunk xxrep 5 a xxup tta xxrep 4 t xxup xxunk xxrep 4 t xxup xxunk xxrep 4 t a xxrep 4 c</td>
      <td>HVAG84XI</td>
    </tr>
    <tr>
      <td>xxbos xxup cggccgcacgtctaagaaaccattattatcatgacattaacctat xxrep 5 a xxup xxunk xxrep 4 c xxup at xxrep 4 c xxup xxunk xxrep 4 g xxup xxunk xxrep 4 a g xxrep 4 t xxup cttt xxrep 4 c xxup xxunk xxrep 4 a xxup xxunk xxrep 4 g xxup xxunk xxrep 4 g xxup xxunk xxrep 4 a xxup xxunk xxrep 5 g xxup xxunk xxrep 4 c xxup xxunk xxrep 4 c</td>
      <td>QVAZPYQ8</td>
    </tr>
    <tr>
      <td>xxbos xxup xxunk xxrep 4 c xxup gagaagtt xxrep 6 g a xxrep 4 g xxup tcggcaattgaaccggtgcctagagaaggtggcgc xxrep 4 g xxup taaactgggaaagtgatgtcgtgtactggctccgcc xxrep 5 t xxup cccgagggt xxrep 5 g xxup agaaccgtatataagtgcagtagtcgccgtgaacgttc xxrep 5 t xxup cgcaacgggtttgccgccagaacacaggtaagtgccgtgtgtggttcccgcgggcctggcctctttacgggttatggcccttgcgtgccttgaattacttccacctggctccagtacgtgattcttgatcccgagctggagcca xxrep 4 g xxup cgggccttgcgctttaggag xxrep 4 c xxup ttcgcctcgtgcttgagttgaggcctggcctgggcgct xxrep 4 g xxup ccgccgcgtgcgaatctggtggcaccttcgcgcctgtctcgctgctttcgataagtctctagccattt xxrep 4 a xxrep 5 t xxup gatgacctgctgcgacgc xxrep 7 t xxup ctggcaagatagtcttgtaaatgcgggccaggatctgcacactggtatttcgg xxrep 5 t xxup gggcccgcggccggcgac</td>
      <td>5PR9OSRS</td>
    </tr>
    <tr>
      <td>xxbos xxup cggccgcacgtctaagaaaccattattatcatgacattaacctat xxrep 5 a xxup xxunk xxrep 5 a c xxrep 5 a xxup xxunk xxrep 5 c xxup xxunk xxrep 4 g xxup xxunk xxrep 4 c xxup xxunk xxrep 4 g xxup xxunk xxrep 5 c xxup xxunk xxrep 6 g xxup xxunk xxrep 4 g xxup xxunk xxrep 4 a xxup xxunk xxrep 4 a xxup xxunk xxrep 5 c xxup xxunk xxrep 4 g</td>
      <td>QVAZPYQ8</td>
    </tr>
  </tbody>
</table>



```python
learn_classifier = text_classifier_learner(data_clas, AWD_LSTM, drop_mult=0.5)
learn_classifier.load_encoder('fine_tuned_enc')
learn_classifier.freeze()
```


```python
learn_classifier.lr_find()
```



    <div>
        <style>
            /* Turns off some styling */
            progress {
                /* gets rid of default border in Firefox and Opera. */
                border: none;
                /* Needs to be in here for Safari polyfill so background images work as expected. */
                background-size: auto;
            }
            .progress-bar-interrupted, .progress-bar-interrupted::-webkit-progress-bar {
                background: #F44336;
            }
        </style>
      <progress value='0' class='' max='1' style='width:300px; height:20px; vertical-align: middle;'></progress>
      0.00% [0/1 00:00<00:00]
    </div>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>epoch</th>
      <th>train_loss</th>
      <th>valid_loss</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table><p>

    <div>
        <style>
            /* Turns off some styling */
            progress {
                /* gets rid of default border in Firefox and Opera. */
                border: none;
                /* Needs to be in here for Safari polyfill so background images work as expected. */
                background-size: auto;
            }
            .progress-bar-interrupted, .progress-bar-interrupted::-webkit-progress-bar {
                background: #F44336;
            }
        </style>
      <progress value='99' class='' max='1575' style='width:300px; height:20px; vertical-align: middle;'></progress>
      6.29% [99/1575 04:50<1:12:12 0.4770]
    </div>



    LR Finder is complete, type {learner_name}.recorder.plot() to see the graph.
    


```python
learn_classifier.recorder.plot()
```


![png](output_20_0.png)



```python
learn_classifier.fit_one_cycle(1, 2e-2, moms=(0.8,0.7))
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>epoch</th>
      <th>train_loss</th>
      <th>valid_loss</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.004540</td>
      <td>0.004090</td>
      <td>22:14</td>
    </tr>
  </tbody>
</table>



```python
learn_classifier.freeze_to(-2)
learn_classifier.fit_one_cycle(1, slice(1e-2/(2.6**4),1e-2), moms=(0.8,0.7))
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>epoch</th>
      <th>train_loss</th>
      <th>valid_loss</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.004006</td>
      <td>0.004002</td>
      <td>22:43</td>
    </tr>
  </tbody>
</table>



```python
learn_classifier.freeze_to(-3)
learn_classifier.fit_one_cycle(1, slice(5e-3/(2.6**4),5e-3), moms=(0.8,0.7))
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: left;">
      <th>epoch</th>
      <th>train_loss</th>
      <th>valid_loss</th>
      <th>time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0.003620</td>
      <td>0.003387</td>
      <td>23:24</td>
    </tr>
  </tbody>
</table>



```python
learn_classifier.show_results()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>text</th>
      <th>target</th>
      <th>prediction</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>xxbos xxup xxunk xxrep 4 a xxup ctttgaccttgtgaagtgtcaaccttgactgtcgaaccaccatagtttggcgcgaattgagcgtcataattgtttactctcagtgcagtcaacatgtcgagtttcgtgccgaataaagagcaaacgcggacagtattaa xxrep 4 t xxup ctg xxrep 4 t xxup catttgaag xxrep 4 a xxup xxunk xxrep 4 a xxup gttgaaa xxrep 5 t xxup xxunk xxrep 4 t xxup xxunk xxrep 4 t xxup gac xxrep 4 a xxup xxunk xxrep 4 t xxup xxunk xxrep 4 t xxup xxunk xxrep 5 t xxup cgaa xxrep 4 t xxup caataa xxrep 4</td>
      <td>FRX9XJYW</td>
      <td></td>
    </tr>
    <tr>
      <td>xxbos xxup catcatcaataatatacctta xxrep 4 t xxup ggattgaagccaatatgataatga xxrep 5 g xxup tggagtttgtgacgtggcgc xxrep 4 g xxup cgtgggaac xxrep 4 g xxup cgggtgacgtagtagtgtggcggaagtgtgatgttgcaagtgtggcggaacacatgtaagcgacggatgtggc xxrep 4 a xxup gtgacg xxrep 5 t xxup ggtgtgcgccggtgtacacaggaagtgacaa xxrep 4 t xxup cgcgcgg xxrep 4 t xxup aggcggatgttgtagtaaatttgggcgtaaccgagtaagatttggcca xxrep 4 t xxup cgcggg xxrep 4 a xxup ctgaataagaggaagtgaaatctgaataa xxrep 4 t xxup gtgttactcatagcgcgtaatatttgtctagggccgc xxrep 4 g xxup actttgaccgtttacgtggagactcgcccaggtg xxrep 5 t xxup ctcaggtg xxrep 4</td>
      <td>U8FRHWSV</td>
      <td></td>
    </tr>
    <tr>
      <td>xxbos xxup aaattgtaaacgttaata xxrep 4 t xxup gtt xxrep 4 a xxup ttcgcgttaaa xxrep 5 t xxup gttaaatcagctca xxrep 6 t xxup aaccaataggccgaaatcggc xxrep 4 a xxup tcccttataaatc xxrep 4 a xxup gaatagaccgagatagggttgagtgttgttccagtttggaacaagagtccactattaaagaacgtggactccaacgtcaaagggcg xxrep 5 a xxup ccgtctatcagggcgatggcccactacgtgaaccatcaccctaatcaag xxrep 6 t xxrep 4 g xxup tcgaggtgccgtaaagcactaaatcggaaccctaaagggag xxrep 5 c xxup gatttagagcttgac xxrep 4 g xxup aaagccggcgaacgtggcgagaaaggaagggaagaaagcgaaaggagcgggcgctagggcgctggcaagtgtagcggtcacgctgcgcgtaaccaccacacccgccgcgcttaatgcgccgctacagggcgcgtcccattcgccattcaggctgcgcaactgttgggaagggcgatcggtgcgggcctcttcgctattacgccagctggcgaaa xxrep 5 g xxup atgtgctgcaaggcgattaagttgggtaacgccaggg xxrep 4 t xxup cccagtcacgacgttgt xxrep 4 a xxup</td>
      <td>VGWO9SBA</td>
      <td></td>
    </tr>
    <tr>
      <td>xxbos xxup aaattgtaaacgttaata xxrep 4 t xxup gtt xxrep 4 a xxup ttcgcgttaaa xxrep 5 t xxup gttaaatcagctca xxrep 6 t xxup aaccaataggccgaaatcggc xxrep 4 a xxup tcccttataaatc xxrep 4 a xxup gaatagaccgagatagggttgagtgttgttccagtttggaacaagagtccactattaaagaacgtggactccaacgtcaaagggcg xxrep 5 a xxup ccgtctatcagggcgatggcccactacgtgaaccatcaccctaatcaag xxrep 6 t xxrep 4 g xxup tcgaggtgccgtaaagcactaaatcggaaccctaaagggag xxrep 5 c xxup gatttagagcttgac xxrep 4 g xxup aaagccggcgaacgtggcgagaaaggaagggaagaaagcgaaaggagcgggcgctagggcgctggcaagtgtagcggtcacgctgcgcgtaaccaccacacccgccgcgcttaatgcgccgctacagggcgcgtcccattcgccattcaggctgcgcaactgttgggaagggcgatcggtgcgggcctcttcgctattacgccagctggcgaaa xxrep 5 g xxup atgtgctgcaaggcgattaagttgggtaacgccaggg xxrep 4 t xxup cccagtcacgacgttgt xxrep 4 a xxup</td>
      <td>BLFM4YKK</td>
      <td></td>
    </tr>
    <tr>
      <td>xxbos xxup gacct xxrep 6 g xxup cacagtgacaatacccgcggccagccttcgagcggcccgcatggccgcccgtcggccggtgcgacgtgcgcggttaagcagggccgccgccgcgcgttgggcggcagtgccgggtcggcggcggtggcgacgtgctacgcgcctccgccgtctcttca xxrep 4 t xxup agcatagcgccgggctccgcgcaccacggtctgaatggccgcgtccactgtggacactggtggcggcgtgggcgtgtagttgcgcgcctcctccaccaccgcgtcgatggcgtcatcgacggtggtgcgcccagtgcggccgcgtttgtgcgcg xxrep 4 c xxup agggcgcgcggtagtgcccgcgcacgcgcactgggtgttggtcggagcgcttcttgg xxrep 4 c xxup gccaaacatcttgcttgggaagcgcagg xxrep 4 c xxup agcctgtgttattgctgggcgatataaggatggacatgcttgctc xxrep 5 a xxup gtgcggctcgataggacgcgcggcgagactatgcccagggccttgtaaacgta xxrep 4 g xxup caggtgcggcgtctggcgtcagtaatggtcactcgctggactcctccgatgctgttgcgcagcggtagcgtcccgtgatctgtgagagcaggaacg xxrep 4 t xxup cactgacggtggtgatggt xxrep 5 g xxup ctggcgggcgcgcc xxrep 4 a xxup tctggttctcgggaaagcgattgaacacgtgggtcagagaggtaaactggcggatgagttgggagtagacggcctggtcgttgtagaagctcttggagtgcacgggcaacagctcggcgcccaccaccggaaagttgctgatctggcgcgtggagcggaaggtcac xxrep 4 g xxup tcttgcatcatgtctggcaacgaccagtagacctgctccgagccgcaggttacgtcaggagtgcaaagcagggtccatgagcggattccggtctgagggtcgccgtagttgtatgcaaggtaccagctgcggtactgggtgaaggtgctgtcattgcttattaggttgtaactgcgtttcttgctgtcctctgtca xxrep 4 g xxup tttgatcaccggtttcttctgaggcttctcgacctcgggttgcgcagc xxrep 5 g xxup cggcagcttcggccgctgcttcggcctcagcgcgcttctcctcagcccgtgtggcaaaggtgtcgccgcgaatggcatgatcgttcatgtcctccaccggctgcattgccgcggctgccgcgttggagttctcttccgcgccgctgccactgctgttgctgccgcctgcgcca xxrep 5</td>
      <td>69M351P4</td>
      <td></td>
    </tr>
  </tbody>
</table>



```python
preds, target = learn_classifier.get_preds(DatasetType.Test, ordered=True)
labels = preds.numpy()
```






```python
labels
```




    array([[3.748678e-06, 3.451483e-06, 6.919496e-06, 1.534465e-07, ..., 3.832419e-07, 1.143104e-06, 2.633506e-07,
            1.066121e-05],
           [2.207659e-06, 6.514254e-08, 7.488711e-07, 5.801184e-07, ..., 1.063493e-05, 3.631477e-07, 1.782875e-06,
            1.178191e-04],
           [2.846830e-05, 2.629395e-05, 4.144958e-06, 2.324558e-06, ..., 3.497438e-06, 3.117921e-03, 2.758713e-05,
            7.757686e-05],
           [3.330139e-04, 1.016890e-03, 4.978001e-05, 1.965206e-05, ..., 5.273554e-05, 1.716481e-04, 2.916880e-04,
            2.784313e-04],
           ...,
           [4.717038e-04, 7.015335e-03, 1.412177e-04, 1.463277e-05, ..., 1.526126e-04, 9.404898e-05, 5.651919e-04,
            2.701334e-05],
           [2.968377e-04, 7.048445e-04, 2.311201e-04, 1.644337e-05, ..., 7.194062e-05, 6.729727e-04, 1.607442e-04,
            4.259677e-06],
           [1.671917e-04, 1.125913e-04, 4.333867e-05, 1.417296e-05, ..., 7.188282e-05, 2.338895e-04, 1.049925e-04,
            1.210500e-04],
           [3.465746e-04, 1.890754e-03, 5.130416e-05, 1.416654e-05, ..., 5.479210e-05, 1.523943e-04, 4.751486e-04,
            6.288937e-04]], dtype=float32)




```python
submission_part1 = test_df['sequence_id']
submission_part1 = pd.concat([submission_part1, pd.DataFrame(preds.numpy(), columns = label_cols)], axis=1)

submission_part1.to_csv('G:/DataScienceProject/Drivendata-Genetic-Engineering-Attribution/submission_part1.csv', index=False)
submission_part1.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>sequence_id</th>
      <th>00Q4V31T</th>
      <th>012VT4JK</th>
      <th>028IO5W2</th>
      <th>03GRNN7N</th>
      <th>03Y3W51H</th>
      <th>09MQV1TY</th>
      <th>0A4AHRCT</th>
      <th>0A9M05NC</th>
      <th>0B9GCUVV</th>
      <th>...</th>
      <th>ZQNGGY33</th>
      <th>ZSHS4VJZ</th>
      <th>ZT1IP3T6</th>
      <th>ZU6860XU</th>
      <th>ZU6TVFFU</th>
      <th>ZU75P59K</th>
      <th>ZUI6TDWV</th>
      <th>ZWFD8OHC</th>
      <th>ZX06ZDZN</th>
      <th>ZZJVE4HO</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>E0VFT</td>
      <td>0.000004</td>
      <td>3.451483e-06</td>
      <td>6.919496e-06</td>
      <td>1.534465e-07</td>
      <td>3.622415e-06</td>
      <td>5.709481e-07</td>
      <td>0.000005</td>
      <td>0.000007</td>
      <td>0.000113</td>
      <td>...</td>
      <td>0.000003</td>
      <td>4.770832e-07</td>
      <td>0.000002</td>
      <td>8.364696e-07</td>
      <td>0.000253</td>
      <td>0.000040</td>
      <td>3.832419e-07</td>
      <td>1.143104e-06</td>
      <td>2.633506e-07</td>
      <td>0.000011</td>
    </tr>
    <tr>
      <th>1</th>
      <td>TTRK5</td>
      <td>0.000002</td>
      <td>6.514254e-08</td>
      <td>7.488711e-07</td>
      <td>5.801184e-07</td>
      <td>5.589681e-07</td>
      <td>2.380848e-05</td>
      <td>0.000002</td>
      <td>0.000010</td>
      <td>0.000279</td>
      <td>...</td>
      <td>0.000002</td>
      <td>5.032452e-07</td>
      <td>0.000011</td>
      <td>9.114181e-07</td>
      <td>0.000086</td>
      <td>0.000013</td>
      <td>1.063493e-05</td>
      <td>3.631477e-07</td>
      <td>1.782875e-06</td>
      <td>0.000118</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2Z7FZ</td>
      <td>0.000028</td>
      <td>2.629395e-05</td>
      <td>4.144958e-06</td>
      <td>2.324558e-06</td>
      <td>2.056403e-03</td>
      <td>8.042430e-04</td>
      <td>0.000166</td>
      <td>0.000029</td>
      <td>0.002467</td>
      <td>...</td>
      <td>0.000279</td>
      <td>1.242659e-05</td>
      <td>0.000064</td>
      <td>3.350295e-05</td>
      <td>0.000001</td>
      <td>0.000003</td>
      <td>3.497438e-06</td>
      <td>3.117921e-03</td>
      <td>2.758713e-05</td>
      <td>0.000078</td>
    </tr>
    <tr>
      <th>3</th>
      <td>VJI6E</td>
      <td>0.000333</td>
      <td>1.016890e-03</td>
      <td>4.978001e-05</td>
      <td>1.965206e-05</td>
      <td>8.176383e-04</td>
      <td>5.590113e-05</td>
      <td>0.000175</td>
      <td>0.000224</td>
      <td>0.014802</td>
      <td>...</td>
      <td>0.000123</td>
      <td>3.783884e-05</td>
      <td>0.000277</td>
      <td>3.524519e-04</td>
      <td>0.000297</td>
      <td>0.000044</td>
      <td>5.273554e-05</td>
      <td>1.716481e-04</td>
      <td>2.916880e-04</td>
      <td>0.000278</td>
    </tr>
    <tr>
      <th>4</th>
      <td>721FI</td>
      <td>0.000054</td>
      <td>3.143457e-04</td>
      <td>2.665746e-06</td>
      <td>8.624604e-06</td>
      <td>3.526671e-03</td>
      <td>2.415285e-04</td>
      <td>0.000185</td>
      <td>0.000380</td>
      <td>0.003872</td>
      <td>...</td>
      <td>0.000046</td>
      <td>5.552264e-05</td>
      <td>0.000087</td>
      <td>2.934498e-04</td>
      <td>0.000134</td>
      <td>0.000008</td>
      <td>2.778973e-05</td>
      <td>1.547617e-04</td>
      <td>2.292135e-04</td>
      <td>0.001108</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 1315 columns</p>
</div>


