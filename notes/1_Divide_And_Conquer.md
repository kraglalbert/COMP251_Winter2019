# Divide and Conquer

### Contents:

0. [The Master Theorem](#the-master-theorem)

1. [The Chip Problem](#the-chip-problem)
2. [Karatsuba Multiplication](#karatsuba-multiplication)
3. [Mergesort](#mergesort)
4. [Convex Hull Algorithm](#convex-hull-algorithm)
5. [Strassen Multiplication](#strassen-multiplication)
6. [Euclid's Algorithm for GCD](#euclid's-algorithm-for-gcd)
7. [Linear-Time Selection](#linear-time-selection)
8. [Hoare's Algorithm](#hoare's-algorithm)
9. [Binary Search](#binary-search)

### 0. The Master Theorem

I will cover this first since it is useful for solving many of the recurrence relations that will appear below. The theorem is used to solve recurrences of the form:

$T_{n} = aT{\frac{n}{b} + f(n)}​$

where $a \geq 1​$ and $b > 1​$, and $f(n)​$ is an asymptocially positive function. There are three cases for the theorem:

1. $n^{log_{b}(a)}$ is asymptotically greater than $f(n)$. Then $T_{n} = \Theta(n^{log_{b}(a)})$.
2. $f(n)​$ is asymptotically greater than $n^{log_{b}(a)}​$. Then $T_{n} = \Theta(f(n))​$.
3. $n^{log_{b}(a)}​$ is asymptotically equal to $f(n)​$, i.e. $f(n) = \Theta(n^{log_{b}(a)})​$. Then $T_{n} = \Theta(f(n) \cdotp log_{b}(n)) = \Theta(n^{log_{b}(a)} \cdotp log_{b}(n))​$.  

It is important to note that in the first two cases, one function must be *polynomially* greater than the other, i.e. $n^{log_{b}(a) - \epsilon} > f(n)​$ for $\epsilon > 0​$ and vice versa. There is also an additional condition for case 3, where it must also be true that $ af(\frac{n}{b}) \leq cf(n)​$ for some $c < 1​$. This is known as the *regularity* condition, and it is satisfied by most polynomially-bounded functions (but it should still be checked).

### 1. The Chip Problem

**Problem:** *A factory produces chips that are either good (G) or bad (B). To evaluate their state, the chips can be paired in a testing device (oracle) where the two chips evaluate each other, i.e. each chip gives its opinion on whether the other is good or bad. Good chips tell the truth, while bad chips are unreliable. Given $N​$ chips, determine their state (G/B), with the premise that the set of good chips $G​$ is larger than the set of bad chips $B​$: $|G| > |B|​$. Complexity is measured in terms of the number of uses of the oracle.*

**Solution:** 

In this problem, it is important to note that the algorithm only works if it is always strictly true for any set of chips that $|G| > |B|$. The algorithm to find one good chip then works as follows:

* Pair up all of the chips and get them to evaluate each other. There are 3 possible outcomes:
  * GG: This means that either both chips are good, or both chips are bad.
  * BG/GB and BB: This means that at least one of the chips is bad.
* Throw out all pairs that are not GG. Then, throw out one chip from each of the GG pairs.
  * We must verify that the $|G| > |B|$ property is preserved after this operation. By throwing out all pairs that are not GG, we are throwing out either one or two bad chips with each pair, and so the number of bad chips thrown out will be greater than the number of good chips thrown out. 
  * Now there are only GG pairs left, and we know that we still have $|G| > |B|$. This means there are more Good-Good than Bad-Bad pairs, so eliminating one of them from each pair will still guarantee that $|G| > |B|​$.
  * We know that there must be at least one GG pair at the start, since $|G| > |B|​$.
* Repeat these steps recursively until there is only one chip remaining, at which point we will know it is good since $|G| > |B|$ is preserved throughout.

These steps work for the case when the number of chips is even. When the number of chips is odd, we require a slight modification, explained below:

* Pick a chip at random and have the other $N-1$ chips evaluate it. Since there are more good chips than bad, we can take the majority vote to determine if the chip we have chosen is good or bad.
* If the chip is good, then we can stop. Otherwise, throw the chip out and proceed with the even case algorithm.

Once we have found a good chip, we can test all of the other chips with $n-1​$ evaluations to find the entire set of good chips. 

**Complexity:**

Even case:  $T_{n} \leq \frac{n}{2} + T_{\frac{n}{2}}$

$T_{\frac{n}{2}}​$ is the time required to compute the recursive call of size $\frac{n}{2}​$, and the $\frac{n}{2}​$ term is the time required to evaluate all pairs at each step of the algorithm.

Odd case:  $T_{n} \leq n - 1 + \frac{n-1}{2} + T_{\frac{n-1}{2}}$

Similar to above, except for the extra $n-1​$ term which denotes the time required to evaluate the first chip picked at random. 

We can solve the recurrence using substitution, done below:

$T_{n} \leq  \frac{n}{2} + T_{\frac{n}{2}}​$

$T_{n} \leq  \frac{n}{2} + \frac{n}{4} + T_{\frac{n}{4}}$

$T_{n} \leq  \frac{n}{2} + \frac{n}{4} + \frac{n}{8} + T_{\frac{n}{4}}​$

$T_{n} \leq  \frac{n}{2} + \frac{n}{4} + \frac{n}{8} ... + 1$

$T_{n} \leq  \frac{n}{2} + \frac{n}{4} + \frac{n}{8} ... + 1$

$T_{n} \leq  n(\frac{1}{2} + \frac{1}{4} + \frac{1}{8} ...) + 1 = n + 1$

Therefore $T_{n} = O(n)​$. The sum of fractions in the last step can be evaluated as an infinite geometric series, where $r = \frac{1}{2}​$ and the sum is then $\frac{a_0}{1 - r} = \frac{\frac{1}{2}}{1 - \frac{1}{2}} = 1​$.

### 3. Mergesort

This algorithm sorts a list/array in time $\Theta(nlogn)$. The algorithm works as follows:

* Split the array in half recursively until the partitions are of size 1 (base case).
* Merge the partitions such that they are sorted when merged together.
* Do this recursively until the entire list is merged back together.

**Pseudocode:** 

```
func mergesort (list) {
    if (list.length <= 1) return list
    else {
        leftList = mergesort(list[0..middle])
        rightList = mergesort(list[middle+1..length-1])
        
        merge(leftList, rightList)
    }
}
```

**Complexity:**

At each step of the recursion, the list (of size $n​$) is split into two halves of (roughly) size $\frac{n}{2}​$. Additionally, each merge step requires $n-1​$ comparisons, so the overall time complexity is:

$T_{n} \leq n-1 + T_{\frac{n}{2}} + T_{\frac{n}{2}}​$

$T_{n} \leq  n-1 + 2T_{\frac{n}{2}}$

We can solve this recurrence with the master theorem. Here we have $a = 2$, $b = 2$, and $f(n) = n-1$. Then we get:

$n^{log_{2}2}  = n​$

Which is asymptotically equal to $n-1​$. Therefore the overall complexity is $\Theta(f(n) \cdotp log_{b}n) = \Theta(nlog_{2}n)​$, as expected. 

### 9. Binary Search

**Problem:** *Given a sorted list and a target value $k$, return the index of $k$ in the sorted list or $-1$ if it is not in the list.*

This algorithm can be done fairly simply with a ternary oracle, but it can also be done with a binary oracle. I will assume the typical implementation where we have a ternary oracle that compares two elements $x$ and $y$ and tells us if $x < y$, $x = y$ or $x > y​$. 

**Solution:**

* Starting with the entire list, pick the middle element. If it is equal to $k$, then halt. If not, then go search the left half of the array if the middle element is greater than $k$ and the right half if it is less than $k$.
* Repeat this until $k$ is found or until a subarray of size 1 not equal to $k$ is reached, at which point $-1$ should be returned.

**Pseudocode:**

```
func binarySearch(list, k) {
	if (list.length < 1) {
		return -1
    } else if (list[middle] = k) {
    	return middle
    } else if (list[middle] > k) {
    	return binarySearch(list[0..middle-1], k)
    } else {
    	return binarySearch(list[middle+1..k], k)
	}
}
```

Note: although this pseudocode is recursive, the algorithm is usually implemented iteratively. 

**Complexity:**

At each step of the algorithm, we make a call to a problem of size $\frac{n}{2}​$ and also make use of the oracle, which takes constant time. Therefore:

$T_{n} \leq 1 + T_{\frac{n}{2}}​$

We can determine the complexity using the master theorem. We have $a = 1, b = 2$ and $f(n) = 1​$, which gives us:

$n^{log_{2}1} = n^0 = 1​$ 

Which is directly equal to $f(n)$. Hence the overall complexity is $\Theta(f(n) \cdotp log_{b}n) = \Theta(log_{2}n)$.