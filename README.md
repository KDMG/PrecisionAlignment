# PrecisionAlignment

In this repository we provide a faithful implementation for computing precision 1-alignment based of Petri nets with respect to an event log, following the approach proposed in the paper "[Measuring precision of modeled behavior](https://link.springer.com/article/10.1007/s10257-014-0234-7)".

A brief description of the approach is described below:

1) For each trace $\sigma_L$ in the log $L$, compute one optimal alignment $\gamma = \Lambda^{1}(\sigma_L)$, and let $\Lambda^{1}$ denote the multiset of these alignments over all traces in $L$.

2) Project each alignment $\gamma$ to model behavior only (model moves and synchronous moves), obtaining a fully compliant sequence $\bar{\lambda}(\gamma)$. Let $\bar{\lambda}(\Lambda^{1})$ be the multiset of projected sequences.

3) Build the alignment automaton $A_A=(Q,\Sigma,\delta,\epsilon,\omega)$, where $Q$ contains all prefixes of the sequences in $\bar{\lambda}(\Lambda^{1})$, $\epsilon$ is the empty prefix, and $\delta$ is the prefix transition function.

4) Assign a weight for each state $s \in Q$ of the automaton $A_A$ as $\omega(s) = \sum_{\forall \gamma \in \bar \Lambda} \omega(\gamma)$, where $\omega(\gamma)$ is the frequence of a sequence in $\bar \Lambda$;

5) For each state, compute the set of its available actions, i.e. possible direct successor activities according to the model ($a_v$) and then compare it with the set of executed actions, i.e. activities really executed in the traces ($e_x$);

6) Compute precision for automaton $A$ as follows:  $a_p = \frac{\sum_{s}\omega (s) \cdot |e_x(s)|}{\sum_{s}\omega (s) \cdot |a_v(s)|}$.

### Example
Consider the following process model, log and projected alignments.

| Process model| Projected alignments|
| :---: | :---: |
|<img src="https://github.com/KDMG/PrecisionAlignment/blob/main/example/model_example.png" width="400"> | <img src="https://github.com/KDMG/PrecisionAlignment/blob/main/example/log_example.png" width="400"> | 

The corresponding automaton is:

<p align="center">
<img src="https://github.com/KDMG/PrecisionAlignment/blob/main/example/automaton_example.png" width="500">
</p>

therefore, $a_p = \frac{5⋅1 + 5⋅3 + 2⋅2 + 1⋅1 +1⋅1 +1⋅1 +1⋅1 +1⋅1 +1⋅1 +2⋅1 +2⋅1}{5⋅1 + 5⋅3 + 2⋅2 + 1⋅3 +1⋅2 +1⋅3 +1⋅3 +1⋅1 +1⋅3 +2⋅1 +2⋅1} = \frac{34}{43} = 0.79$


## How to use

1) Install PM4Py with pip
```bash
pip install -U pm4py
```
2) Copy the script "[precision_alignment.py](https://github.com/KDMG/PrecisionAlignment/blob/main/precision_alignment.py)" ref inside the folder `pm4py/algo/evaluation/precision/variants/`.

3) Add the variant inside the `pm4py/algo/evaluation/precision/algorithm.py` file.
