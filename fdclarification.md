---
layout: page
---

# Clarification about Functional Dependency Decomposition

This document clarifies decomposition for functional dependencies.  What I demonstrated in class was misleading.

## BCNF

In BCNF, you take the FDs as-is, and perform decomposition.  The order of functional dependencies that you use to perform decomposition will affect the tables that you end up with.  

        DecomposedRelations = { R }

        while DecomposedRelations is not empty
          R = pop a relation from DecomposedRelations
          FR = subset of FDs in R's projection
          for fd X->Y in FR
            if fd violates BCNF with respect to R
              NewRs = decompose R using fd 
              add NewRs to DecomposedRelations
              continue while loop


The key is how `violates` is checked.  You start with the **fds in R's projection**, and then check

1. is fd trivial?  otherwise
2. does `X` determine R when using the fds in R's projection?


What this means is that your decomposition may have been lossy and thus lose potential keys!  Take the example from the lecture notes:

        R:   ABCDE
        FDS: A->BCDE, BC->A, D->B, C->D

        # suppose we first check D->B:

        D doesn't determine R, thus we decompose into: BD, ACDE

        Let's check BD:
        
          Only D->B is in its projection, thus its in BCNF.  
          
        Let's check ACDE:
        
          Only C->D is in its projection, so ACDE is redundant.
          Decompose into: CD, ACE

        ACE has empty projection, thus it's in BCNF


## 3NF

In 3NF, you first compute the minimal cover of the FDs.  Note that the minimal cover may not always be unique.  Once you have the minimal cover, the procuder is similar to BCNF, with two key differences

1. checking violation of 3NF also allows Y to be part of a key (minimal key).
2. once you have performed BCNF decomposition, any lost dependencies are added as new relations.
