### This repo points to data from the papers [Measuring Abstract Reasoning in Neural Networks](https://arxiv.org/abs/1807.04225); Barrett, Hill, Santoro et al. (2018) and [Learning to Make Analogies by Contrasting Abstract Relational Structures](https://openreview.net/pdf?id=SylLYsCcFm); Hill, Santoro et al. (2019). The data can be found [here](https://console.cloud.google.com/storage/browser/ravens-matrices) 

This data is made available for the purposes of non-commercial research only and is not to be used for any other purpose.

# Procedurally Generated Matrices (PGM) data
From the paper [Measuring Abstract Reasoning in Neural Networks](https://arxiv.org/abs/1807.04225), Barrett, Hill, Santoro et al. 2018.

### UPDATE -- fix re: array shape and readability of the data

Users have noted issues with readability of the data. This is caused by a mis-shaped numpy array upon loading. As noted below, images are of size 160x160x16. When loading the .npz, please reshape the array: 

~~~~
data = np.load("name.npz")
image = data["image"].reshape(16, 160, 160)
~~~~

The first dimension of image now indexes the array into the correct panels, as is intended. This array should now depict readable images when plotted.

## Directory and file organisation
The parent data folder contains 8 archived directories, corresponding to a particular generalisation regime:
- neutral.tar.gz
- interpolation.tar.gz
- extrapolation.tar.gz
- attr.rel.pairs.tar.gz
- attr.rels.tar.gz
- attrs.pairs.tar.gz
- attrs.shape.color.tar.gz
- attrs.line.type.tar.gz
  
Within each folder are 1.42M .npz files encoding the samples. The naming convention is: PGM_{split_type}_{train/test/val}_{id}.npz, where split_type is one of the 8 indicated above, train/test/val is the train, test, or validation set, and id is a numerical identifier for a particular matrix.

A saved array has the following structure:

**image**: a 160x160x16 integer array with values from 0 to 255. The last dimension denotes the panel number for the matrix, with the first 8 panels being the "context", and the last 8 being the "choices".

**meta_matrix**: A 4x12 binary array encoding the structure of the matrix (i.e., the triples $[r, o, a]$ contained in the matrix). The rows index a tuple, and the columns have the following syntax:

'shape' : 0
'line' : 1
'color' : 2
'number' : 3
'position' : 4
'size' : 5
'type' : 6
'progression' : 7
'XOR' : 8
'OR' : 9
'AND' : 10
'consistent_union' : 11

**meta_target**: an OR operation applied across all binary-encoded triples [r, o, a] (i.e., the rows of the meta_matrix).

**target**: integer value denoting the target for the particular matrix (i.e., the index of the correct answer among the "choice" panels).

## Notation 
$R$ denotes the set of relation types (progression, XOR, OR, AND, consistent union), $O$ denotes the object types (shape, line), and $A$ denotes the attribute types (size, colour, position, number). The structure of a matrix, $S$, is the set of triples $S=\{[r, o, a]\}$ that determine the challenge posed by a particular matrix. 

## Generalisation split details
The generalisation splits are as follows:

- **neutral**: The structures encoding the matrices in both the training and testing sets contain any triples $[r, o, a]$ for $r \in R$, $o \in O$, and $a \in A$. Training and testing sets are disjoint, with separation occurring at the level of the input variables (i.e. pixel manifestations).

- **interpolation**: As in the neutral split, $S$ consisted of any triples $[r, o, a]$. For interpolation, in the training set, when the attribute was "colour" or "size" (i.e., the ordered attributes), the values of the attributes were restricted to even-indexed members of a discrete set, whereas in the test set only odd-indexed values were permitted. Note that all $S$ contained some triple $[r, o, a]$ with the colour or size attribute . Thus, generalisation is required for every question in the test set.

- **extrapolation**: Same as in interpolation, but the values of the attributes were restricted to the lower half of the discrete set during training, whereas in the test set they took values in the upper half.

- **attr.rel.pairs**: All $S$ contained at least two triples, $([r_1,o_1,a_1],[r_2,o_2,a_2]) = (t_1, t_2)$, of which 400 are viable. We randomly allocated 360 to the training set and 40 to the test set. Members $(t_1, t_2)$ of the 40 held-out pairs did not occur together in structures $S$ in the training set, and all structures $S$ had at least one such pair $(t_1, t_2)$ as a subset.

- **attr.rels**: In our dataset, there are 29 possible unique triples $[r,o,a]$. We allocated seven of these for the test set, at random, but such that each of the attributes was represented exactly once in this set. These held-out triples never occurred in questions in the training set, and every $S$ in the test set contained at least one of them.

- **attrs.pairs**: $S$ contained at least two triples. There are 20 (unordered) viable pairs of attributes $(a_1, a_2)$ such that for some $r_i, o_i, ([r_1,o_1,a_1],[r_2,o_2,a_2])$ is a viable triple pair $([r_1,o_1,a_1],[r_2,o_2,a_2]) = (t_1, t_2)$. We allocated 16 of these pairs for training and four for testing. For a pair $(a_1, a_2)$ in the test set, $S$ in the training set contained triples with $a_1$ or $a_2$. In the test set, all $S$ contained triples with $a_1$ and $a_2$.

- **attrs.shape.color**, **attrs.line.type**: Held-out attribute shape-colour or line-type. $S$ in the training set contained no triples with $o$=shape and $a$=colour. All structures governing puzzles in the test set contained at least one triple with $o$=shape and $a$=colour. For comparison, we included a similar split in which triples were held-out if $o$=line and $a$=type.

# Visual Analogy data

From the paper [Learning to Make Analogies by Contrasting Abstract Relational Structures(https://openreview.net/pdf?id=SylLYsCcFm); Hill, Santoro et al (2019).

This data can be found in the {analogies} subdirectory, which contains archived directories corresponding to the visual analogy problems described in the paper.

novel.domain.transfer.tar.gz
novel.target.domain.line.type.tar.gz
novel.target.domain.shape.color.tar.gz
interpolation.tar.gz
extrapolation.tar.gz

Within each of these directories is a large number of .npz files. The filenames are of the form analogy_{split_type}_{train/test/val}_{lbc/normal}_{id}.npz, where split_type is the name of one of the corresponding directory, lbc/normal determines the nature of the incorrect answer candidates and id is a unique identifier for the file.




