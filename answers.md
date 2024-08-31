# CMPS 6610  Lab 02
# Answers

**Name (Team Member 1):**__________Yongbo Chen__________  
**Name (Team Member 2):**__________Yan Zhu__________

You may work on this lab with a partner. We will investigate recurrences for work and span of algorithms.

## Tree method (2 pts)
Solve the following recurrences by analyzing the recursion tree as shown in Module 2.1. 

a) $W(n) = 3T(n/4) + n^2$  

- The level 0 (root) cost is $O(n^2)$  
- The level 1 has the cost of $3⋅(n/4)^2 = 3⋅(n^2/16)$  
- The level 2 has the cost of $3^2⋅(n/16)^2 = 9⋅(n^2/256)$  
- Therefore, to generalize the cost at level of $i$, the cost of level i is $3^i⋅(n/4^i)^2 = 3^i⋅(n^2/16^i) = (3/16)^i⋅n^2$  
- Since the tree has height of $log{4}{n}$, so the total cost is the sum of costs for every level, which is $n^2⋅\sum_{i=0}^{log{4}{n}} (3/16)^i$.  
- From the sum function, it is a geometric series with the ratio less than 1.  
- Therefore, the sum of cost is $n^2⋅\sum_{i=0}^{log{4}{n}} (3/16)^i \leq \frac{1}{1-\frac{3}{16}}$  
- Overall, the total cost is $n^2⋅\sum_{i=0}^{log{4}{n}} (3/16)^i \in O(n^2)$.  

  
b) $W(n) = 2T(n/2)+ n/ \log n$  

- The level 0 (root) cost is $\frac{n}{logn}$  
- The level 1 has the cost of $2⋅\frac{n/2}{log(n/2)} = \frac{n}{log(n/2)}$  
- The level 2 has the cost of $4⋅\frac{n/4}{log(n/4)} = \frac{n}{log(n/4)}$  
- Therefore, to generalize the cost at level of $i$, the cost of level i is $2^i⋅\frac{n/2^i}{log(n/2^i)} = \frac{n}{log(n/2^i)}$  
- Since the tree has height of $logn$, so the total cost is the sum of costs for every level, which is $\sum_{i=0}^{logn} \frac{n}{log(n/2^i)}$.  
- Therefore, the sum of cost is $\sum_{i=0}^{logn} \frac{n}{log(n/2^i)} = \sum_{i=0}^{logn} \frac{n}{logn-i} = n⋅\sum_{i=0}^{logn} \frac{1}{logn-i} \approx n⋅log(logn)$.  
- Overall, the total cost $\sum_{i=0}^{logn} \frac{n}{log(n/2^i)} \in O(nloglogn)$.  
  

## Brick method (2pts)
Solve the following recurrences using the brick method. First determine
whether they are root-dominated, leaf-dominated, or balanced and give adequate explanation. Then,
state the resulting asymptotic bound for $W(n)$.

a) $W(n) = 2 W(0.49 n) + 1.01 n$  

- At the root level, the cost is $1.01n$.  
- The level 1 has the cost of $2⋅0.49⋅1.01n = 0.9898n$. In this case the cost has decreased by a factor of 0.49 when going down the tree.  
- Therefore, this recurrence is **root-dominated**.  
- Since the recurrence is root-dominated, then the asymptotic bound for $W(n)$ is the root cost, which is $O(1.01n) \in O(n)$.  

b) $W(n) = W(n/2) + W(n/4) + 0.999n$  

- At the root level, the cost is $0.999n$  
- The level 1 has the cost of $0.999⋅(n/2+n/4) = 0.74925n$. In this case the cost has decreased by a factor of $\frac{3}{4}$ when going down the tree.  
- Therefore, this recurrence is **root-dominated**.  
- Since the recurrence is root-dominated, then the asymptotic bound for $W(n)$ is the root cost, which is $O(0.99n) \in O(n)$.  

c) $W(n) = \sqrt{n}W(\sqrt{n}) + \sqrt{n}$  

- At the root level, the cost is $\sqrt{n}$.    
- The level 1 has the cost of $\sqrt{n}⋅\sqrt{\sqrt{n}} = n^{\frac{3}{4}} \geq \sqrt{n} = n^{\frac{1}{2}}$. 
- The level 2 has the cost of $\sqrt{n}⋅\sqrt{\sqrt{\sqrt{n}}} = n^{\frac{7}{8}} \geq \sqrt{n} = n^{\frac{3}{4}}$.  
- In this case, this recurrence is **leaf-dominated** as it has increased by a factor of $\frac{1}{2^{2^i}}$ when going down the tree.  
- Since in each recursive call, the problem size decreases from $n$ to $\sqrt{n}$. Suppose at $K$-level the $n$ will become 1, which means $\sqrt[n]{k} = 1$. To calculate, we got $k=\log \log n$. Therefore, there should have $log \log n$ levels in this recurrence.  
- Overall, the sum of work for this recurrence should be $n⋅\sum_{i=0}^{loglogn} \frac{1}{2^{2^i}}$. Since $\sum_{i=0}^{\infty} \frac{1}{2^{2^i}}$ approach to 1. Therefore, the total work $n⋅\sum_{i=0}^{loglogn} \frac{1}{2^{2^i}} \lt 1*n$.  
- Therefore, the asymptotic bound for $W(n)$ should $\in O(n)$.  


## Analyzing a recursive, parallel algorithm


You were recently hired by Netflix to work on their movie recommendation
algorithm. A key part of the algorithm works by comparing two users'
movie ratings to determine how similar the users are. For example, to
find users similar to Mary, we start by having Mary rank all her movies.
Then, for another user Joe, we look at Joe's rankings and count how
often his pairwise rankings disagree with Mary's:

|      | Beetlejuice | Batman | Jackie Brown | Mr. Mom | Multiplicity |
| ---- | ----------- | ------ | ------------ | ------- | ------------ |
| Mary | 1           | 2      | 3            | 4       | 5            |
| Joe  | 1           | **3**  | **4**        | **2**   | 5            |

Here, Joe (somehow) liked *Mr. Mom* more than *Batman* and *Jackie
Brown*, so the number of disagreements is 2:
(3 <->  2, 4 <-> 2). More formally, a
disagreement occurs for indices (i,j) when (j > i) and
(value[j] < value[i]).

When you arrived at Netflix, you were astounded to see that
they were using the following algorithm to solve the problem:



``` python
def num_disagreements_slow(ranks):
    """
    Params:
      ranks...list of ints for a user's move rankings (e.g., Joe in the example above)
    Returns:
      number of pairwise disagreements
    """
    count = 0
    for i, vi in enumerate(ranks):
        for j, vj in enumerate(ranks[i:]):
            if vj < vi:
                count += 1
    return count
```

``` python 
>>> num_disagreements_slow([1,3,4,2,5])
2
```

a) What is the work/span of this method?  
- **Work**:  
  - For the outer loop, it iterates from 0 to the index $len(ranks) - 1$, which is $len(ranks)$ in total.  
  - For the inner loop, it iterates from i to the index $len(ranks) - 1$, which is $len(ranks) - i$ in total.  
  - Since the rest of the operations take constant time, which is $O(1)$ and assume the length of ranks is $n$.  
  - Therefore, in the worst case, the total work should be **Total iteration of outer loop⋅Total iteration of inner loop**, which is $\sum_{i=0}^{n-1} (n - i) = (n-1) + (n-2) + ... + 1 = \frac{n⋅(n-1)}{2} \in O(n^2)$.  

- **Span**:  
  - Since the algorithm runs sequentially within a nested loop, and according to the definition, the span of the algorithm refers the longest chain of dependent operations.  
  - Therefore, the span of this algorithm is same as the work because the works covers the longest chain of operations, which is $O(n^2)$.  
  
***
You really don't really like what you're seeing, especially after completing Module 2. Armed with your recently acquired knowledge, you quickly threw together this
recursive algorithm that you claim is both more efficient and easier to
run on the giant parallel processing cluster Netflix has.

``` python
def num_disagreements_fast(ranks):
    # base cases
    if len(ranks) <= 1:
        return (0, ranks)
    elif len(ranks) == 2:
        if ranks[1] < ranks[0]:
            return (1, [ranks[1], ranks[0]])  # found a disagreement
        else:
            return (0, ranks)
    # recursion
    else:
        left_disagreements, left_ranks = num_disagreements_fast(ranks[:len(ranks)//2])
        right_disagreements, right_ranks = num_disagreements_fast(ranks[len(ranks)//2:])
        
        combined_disagreements, combined_ranks = combine(left_ranks, right_ranks)

        return (left_disagreements + right_disagreements + combined_disagreements,
                combined_ranks)

def combine(left_ranks, right_ranks):
    i = j = 0
    result = []
    n_disagreements = 0
    while i < len(left_ranks) and j < len(right_ranks):
        if right_ranks[j] < left_ranks[i]: 
            n_disagreements += len(left_ranks[i:])   # found some disagreements
            result.append(right_ranks[j])
            j += 1
        else:
            result.append(left_ranks[i])
            i += 1
    
    result.extend(left_ranks[i:])
    result.extend(right_ranks[j:])
    print('combine: input=(%s, %s) returns=(%s, %s)' % 
          (left_ranks, right_ranks, n_disagreements, result))
    return n_disagreements, result

```

``` python
>>> num_disagreements_fast([1,3,4,2,5])
combine: input=([4], [2, 5]) returns=(1, [2, 4, 5])
combine: input=([1, 3], [2, 4, 5]) returns=(1, [1, 2, 3, 4, 5])
(2, [1, 2, 3, 4, 5])
```

As so often happens, your boss demands theoretical proof that this will
be faster than their existing algorithm. To do so, complete the
following:

b) Describe, in your own words, what the `combine` method is doing and
what it returns.  
  
- The `combine` function takes two sub-lists as input, and merge them into a single sorted list while counting how many pairs are out of order when comparing elements from the two sub-lists, which means counting the total number of pairwise disagreements found during the merging process.  
- The function return two values, the first is the total number of pairwise disagreements found during the merging process. The second returned value is the merged and sorted list from two sub-lists.  
  
c) Write the work recurrence for `num_disagreements_fast`.  

- From the algorithm of `combine`, since it performs a linear pass through the two input lists, `left_ranks` and `right_ranks`, which together should have a total length of $n$. Therefore, the `combine` algorithm should takes $O(n)$ in time.  
- Back to the recurrence for `num_disagreements_fast`, since the statement `left_disagreements, left_ranks = num_disagreements_fast(ranks[:len(ranks)//2])` and `right_disagreements, right_ranks = num_disagreements_fast(ranks[len(ranks)//2:])` takes half of the input, so the work of two recurrences should be $W(n/2)$ for each of the recurrence.  
- Overall, the work recurrence for `num_disagreements_fast` should be $W(n) = 2⋅W(n/2) + O(n)$.  
  
d) Solve this recurrence using any method you like.  

- From c), we know the work recurrence for `num_disagreements_fast` is $W(n) = 2⋅W(n/2) + O(n)$.
- To solve $W(n) = 2⋅W(n/2) + O(n)$, we use the tree method, then the root level is $O(n)$. The first level is $2⋅O(n/2) = O(n)$. The second level is $4⋅O(n/4) = O(n)$
- To generalize the work at each level, the work at each level should be $2^i⋅O(n/2^i) = O(n)$.
- Since the work recurrence is a binary tree, then there are $logn$ levels, and the sum of work is $O(n)+O(n)+...+O(n)$ ($logn$ times) $= logn⋅O(n) = O(nlogn)$.
  
e) Assuming that your recursive calls to `num_disagreements_fast` are
done in parallel, write the span recurrence for your algorithm.  

- If the recursive calls are done in parallel, then it means the statement `left_disagreements, left_ranks = num_disagreements_fast(ranks[:len(ranks)//2])` and `right_disagreements, right_ranks = num_disagreements_fast(ranks[len(ranks)//2:])` are done in parallel.  
- According to c), assume the span for each recurrence is S(n/2), and the span of function `combine` is $O(n)$ since `combine` runs sequentially based on its algorithm.
- Then the span of the algorithm `num_disagreements_fast` is $S(n) = S(n/2) + O(n)$, since each recurrence is running in parallel.  

f) Solve this recurrence using any method you like.  

- According to e), the span recurrence for `num_disagreements_fast` is $S(n) = S(n/2) + O(n)$.  
- To solve $S(n) = S(n/2) + O(n)$, we use the tree method, then the root level is $O(n)$. The first level is $O(n/2) = O(n/2)$. The second level is $O(n/4) = O(n/4)$.  
- To generalize the span at each level, the span at each level should be $O(n/2^i)$.  
- Therefore, the sum of total span is $O(n/2^0) + O(n/2^1) + O(n/2^2) + ... + O(\frac{n}{2^{\log n}})$ ($logn$ times) $= O(n)*(1+1/2+1/4+...+\frac{1}{2^{\log n}}) \in O(n)$.  
  
g) Now argue how your approach relates to the method used by Netflix.  

- Firstly, both our approach are perform the similar tasks with the method used by Netflix, which both methods tried to count the number of pairwise disagreements between two users' movie rankings.  
- However, from the perspective of running efficiency, our method perform better since Netflix's old algorithm run $O(n^2)$ in total of work time, while our approach runs $O(nlogn)$ in total of work time, which is more efficient especially for large datasets.  
- Besides, if we can make sure our approach's recurrence runs in parallel, our approach performs much greater since according to f), our approach costs $O(n)$ in span cost, while since Netflix's old algorithm runs sequentially, it will cost $O(n^2)$ in terms of span cost, which is pretty lower efficient. 
