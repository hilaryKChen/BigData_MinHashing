# BigData_MinHashing
Exploring implementation of MinHashing for string matching problem in big data analysis. Compare results of MinHashing with normal Jaccard Similarity.

## About MinHashing
Minhashing is a method to convert large sets(documents) to short signatures, while preserving similarity. 

Suppose we need to find near-duplicate documents among N=1 million documents, naively, we would have to compute pairwise jaccard similarities for every pair of documents.
- $N(N-1)/2 \approx 5*10^{11}$ comparisons,
- At $10^5$ secs/day and $10^6$ comparisons/sec, it would take 5 days.

By hashing each column of document $S$ into a small signature $sig(S)$, such that:

- $sig(S)$ is small enough that the signature fits in RAM
- $sim(S_1, S_2)$ is almost the same as the "similarity" of signatures $sig(S_1)$ and $sig(S_2)$.

we can handle the scalability problem mentioned above.

A suitable hash function $h(\times)$ for the Jaccard Similarity is called Min-Hashing.

## One-pass Implementation
Approximating row permutation in Min-Hashing with K random hash functions $h_i$.

- Pick k hash functions $h_1, h_2, ..., h_k$
- Let $Sig(i,S_j)$ be the element of the signature matrix for the i-th hash function and document $S_j$.
- Initialize the signature matrix by setting all $Sig(i,S_j) = \infty$
- ```
  For each row r
    Compute h_1(r), h_2(r), ..., h_k(r)
      For each column S_j
        If it has 1 in row r
          For each hash function h_i
            If h_i(r) is smaller than Sig(i, S_j) then
              Sig(i,S_j) = h_i(r)
  ```

We can choose a random hash function by the format:
$$h_{a,b}(x) = ((a*x+b)mod p)mod N$$
  
where $N$ is the number of rows(hash functions), $a$ and $b$ are random integers, and $p$ is a prime number ($p>N$).

In this project we are going to test N=50 and N=100 for comparison.

After generating signature matrix, we compute the simialrity matrix $S(i,j)$ where each element is the Jaccard similarity between article i and j.

## Dataset

BBCSport Dataset from http://mlg.ucd.ie/datasets/bbc.html

- Consists of 737 documents from the BBC Sport website corresponding to sports news articles in five topical areas from 2004-2005.
- Class Labels: 5 (athletics, cricket, football, rugby, tennis
 
  
