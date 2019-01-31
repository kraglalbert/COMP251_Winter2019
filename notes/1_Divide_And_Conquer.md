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

## The Master Theorem

I will cover this first since it is useful for solving many of the recurrence relations that will appear below. The theorem is used to solve recurrences of the form:

<img src="/notes/tex/3583ac5e5ceb0b16c20879b3ab20ac24.svg?invert_in_darkmode&sanitize=true" align=middle width=125.68234964999999pt height=24.65753399999998pt/>

where <img src="/notes/tex/b48fb206d64410c96f41820292147f35.svg?invert_in_darkmode&sanitize=true" align=middle width=38.82599489999999pt height=21.18721440000001pt/> and <img src="/notes/tex/c5f281686751bb80330f78aff1bd5cfc.svg?invert_in_darkmode&sanitize=true" align=middle width=37.19163689999999pt height=22.831056599999986pt/>, and <img src="/notes/tex/f1ff21e8d1167175c22db7db4aa70df9.svg?invert_in_darkmode&sanitize=true" align=middle width=32.46972299999999pt height=24.65753399999998pt/> is an asymptocially positive function. There are three cases for the theorem:

1. <img src="/notes/tex/36e484f92549c1cb812bec684bacf8ee.svg?invert_in_darkmode&sanitize=true" align=middle width=50.42418974999999pt height=29.190975000000005pt/> is asymptotically greater than <img src="/notes/tex/3d425a215e8eeb2a056f553633aaae4a.svg?invert_in_darkmode&sanitize=true" align=middle width=32.46972299999999pt height=24.65753399999998pt/>. Then <img src="/notes/tex/00de17c0b2e8641301681e16d1b82f11.svg?invert_in_darkmode&sanitize=true" align=middle width=117.28864949999999pt height=29.190975000000005pt/>.
2. <img src="/notes/tex/f1ff21e8d1167175c22db7db4aa70df9.svg?invert_in_darkmode&sanitize=true" align=middle width=32.46972299999999pt height=24.65753399999998pt/> is asymptotically greater than <img src="/notes/tex/0d3f3fa140f2aca87931d9f9d8b1c433.svg?invert_in_darkmode&sanitize=true" align=middle width=50.42418974999999pt height=29.190975000000005pt/>. Then <img src="/notes/tex/f2d8d66a5227e4a1fbf4c211a8298d0e.svg?invert_in_darkmode&sanitize=true" align=middle width=98.51234414999998pt height=24.65753399999998pt/>.
3. <img src="/notes/tex/0d3f3fa140f2aca87931d9f9d8b1c433.svg?invert_in_darkmode&sanitize=true" align=middle width=50.42418974999999pt height=29.190975000000005pt/> is asymptotically equal to <img src="/notes/tex/f1ff21e8d1167175c22db7db4aa70df9.svg?invert_in_darkmode&sanitize=true" align=middle width=32.46972299999999pt height=24.65753399999998pt/>, i.e. <img src="/notes/tex/4d88c9b7dd75e17b3092c8808cacb0c6.svg?invert_in_darkmode&sanitize=true" align=middle width=131.20424955pt height=29.190975000000005pt/>. Then <img src="/notes/tex/35e53fe72236c3755281fbde229dc497.svg?invert_in_darkmode&sanitize=true" align=middle width=312.44267835pt height=29.190975000000005pt/>.  

It is important to note that in the first two cases, one function must be *polynomially* greater than the other, i.e. <img src="/notes/tex/0dd3f34fcd714853547ecce5650cdb4f.svg?invert_in_darkmode&sanitize=true" align=middle width=121.38627104999999pt height=29.190975000000005pt/> for <img src="/notes/tex/7d9ef5253ca6a34a812adb9aca44a57e.svg?invert_in_darkmode&sanitize=true" align=middle width=36.80923124999999pt height=21.18721440000001pt/> and vice versa. There is also an additional condition for case 3, where it must also be true that <img src="/notes/tex/d7253d630b0826809b55bb30bf2f3917.svg?invert_in_darkmode&sanitize=true" align=middle width=104.8643739pt height=24.65753399999998pt/> for some <img src="/notes/tex/6aaeafa087b9a553d22af0cb75db0831.svg?invert_in_darkmode&sanitize=true" align=middle width=37.25064419999999pt height=21.18721440000001pt/>. This is known as the *regularity* condition, and it is satisfied by most polynomially-bounded functions (but it should still be checked).

## The Chip Problem

**Problem:** *A factory produces chips that are either good (G) or bad (B). To evaluate their state, the chips can be paired in a testing device (oracle) where the two chips evaluate each other, i.e. each chip gives its opinion on whether the other is good or bad. Good chips tell the truth, while bad chips are unreliable. Given <img src="/notes/tex/f9c4988898e7f532b9f826a75014ed3c.svg?invert_in_darkmode&sanitize=true" align=middle width=14.99998994999999pt height=22.465723500000017pt/> chips, determine their state (G/B), with the premise that the set of good chips <img src="/notes/tex/5201385589993766eea584cd3aa6fa13.svg?invert_in_darkmode&sanitize=true" align=middle width=12.92464304999999pt height=22.465723500000017pt/> is larger than the set of bad chips <img src="/notes/tex/61e84f854bc6258d4108d08d4c4a0852.svg?invert_in_darkmode&sanitize=true" align=middle width=13.29340979999999pt height=22.465723500000017pt/>: <img src="/notes/tex/cba681754b455e98851e83d375fb4c5b.svg?invert_in_darkmode&sanitize=true" align=middle width=66.40057379999999pt height=24.65753399999998pt/>. Complexity is measured in terms of the number of uses of the oracle.*

**Solution:** 

In this problem, it is important to note that the algorithm only works if it is always strictly true for any set of chips that <img src="/notes/tex/ddf6c8671d1b8fba0efeb052b6b77312.svg?invert_in_darkmode&sanitize=true" align=middle width=66.40057379999999pt height=24.65753399999998pt/>. The algorithm to find one good chip then works as follows:

* Pair up all of the chips and get them to evaluate each other. There are 3 possible outcomes:
  * GG: This means that either both chips are good, or both chips are bad.
  * BG/GB and BB: This means that at least one of the chips is bad.
* Throw out all pairs that are not GG. Then, throw out one chip from each of the GG pairs.
  * We must verify that the <img src="/notes/tex/ddf6c8671d1b8fba0efeb052b6b77312.svg?invert_in_darkmode&sanitize=true" align=middle width=66.40057379999999pt height=24.65753399999998pt/> property is preserved after this operation. By throwing out all pairs that are not GG, we are throwing out either one or two bad chips with each pair, and so the number of bad chips thrown out will be greater than the number of good chips thrown out. 
  * Now there are only GG pairs left, and we know that we still have <img src="/notes/tex/ddf6c8671d1b8fba0efeb052b6b77312.svg?invert_in_darkmode&sanitize=true" align=middle width=66.40057379999999pt height=24.65753399999998pt/>. This means there are more Good-Good than Bad-Bad pairs, so eliminating one of them from each pair will still guarantee that <img src="/notes/tex/cba681754b455e98851e83d375fb4c5b.svg?invert_in_darkmode&sanitize=true" align=middle width=66.40057379999999pt height=24.65753399999998pt/>.
  * We know that there must be at least one GG pair at the start, since <img src="/notes/tex/cba681754b455e98851e83d375fb4c5b.svg?invert_in_darkmode&sanitize=true" align=middle width=66.40057379999999pt height=24.65753399999998pt/>.
* Repeat these steps recursively until there is only one chip remaining, at which point we will know it is good since <img src="/notes/tex/ddf6c8671d1b8fba0efeb052b6b77312.svg?invert_in_darkmode&sanitize=true" align=middle width=66.40057379999999pt height=24.65753399999998pt/> is preserved throughout.

These steps work for the case when the number of chips is even. When the number of chips is odd, we require a slight modification, explained below:

* Pick a chip at random and have the other <img src="/notes/tex/e35caf405a5e9b4afd75a0d338c4dc12.svg?invert_in_darkmode&sanitize=true" align=middle width=43.31036984999999pt height=22.465723500000017pt/> chips evaluate it. Since there are more good chips than bad, we can take the majority vote to determine if the chip we have chosen is good or bad.
* If the chip is good, then we can stop. Otherwise, throw the chip out and proceed with the even case algorithm.

Once we have found a good chip, we can test all of the other chips with <img src="/notes/tex/7dc4ba0209d9b0e2068750b1b3fa9419.svg?invert_in_darkmode&sanitize=true" align=middle width=38.17727759999999pt height=21.18721440000001pt/> evaluations to find the entire set of good chips. 

**Complexity:**

Even case:  <img src="/notes/tex/f68695770ffc6844a142f91ac0a3708f.svg?invert_in_darkmode&sanitize=true" align=middle width=91.45240664999999pt height=22.853275500000024pt/>

<img src="/notes/tex/3f4014e387572fb38112f1103a8ece15.svg?invert_in_darkmode&sanitize=true" align=middle width=18.818246699999992pt height=22.465723500000017pt/> is the time required to compute the recursive call of size <img src="/notes/tex/ac036c6070f1558718f3bc3352c2a143.svg?invert_in_darkmode&sanitize=true" align=middle width=8.126022299999999pt height=22.853275500000024pt/>, and the <img src="/notes/tex/ac036c6070f1558718f3bc3352c2a143.svg?invert_in_darkmode&sanitize=true" align=middle width=8.126022299999999pt height=22.853275500000024pt/> term is the time required to evaluate all pairs at each step of the algorithm.

Odd case:  <img src="/notes/tex/306656a5bb3f6677891526599089fccf.svg?invert_in_darkmode&sanitize=true" align=middle width=181.0453557pt height=27.77565449999998pt/>

Similar to above, except for the extra <img src="/notes/tex/7dc4ba0209d9b0e2068750b1b3fa9419.svg?invert_in_darkmode&sanitize=true" align=middle width=38.17727759999999pt height=21.18721440000001pt/> term which denotes the time required to evaluate the first chip picked at random. 

We can solve the recurrence using substitution, done below:

<img src="/notes/tex/bfc619205ddd68856988c72231821ead.svg?invert_in_darkmode&sanitize=true" align=middle width=91.45240664999999pt height=22.853275500000024pt/>

<img src="/notes/tex/474fac389e7dc828ca55a8ea0f820451.svg?invert_in_darkmode&sanitize=true" align=middle width=123.61481384999998pt height=22.853275500000024pt/>

<img src="/notes/tex/3389225b8dc18b9ea6b99d4b15c28671.svg?invert_in_darkmode&sanitize=true" align=middle width=155.77722104999998pt height=22.853275500000024pt/>

<img src="/notes/tex/dfa7170e562800ba8e960f484f1b9906.svg?invert_in_darkmode&sanitize=true" align=middle width=158.87685659999997pt height=22.853275500000024pt/>

<img src="/notes/tex/dfa7170e562800ba8e960f484f1b9906.svg?invert_in_darkmode&sanitize=true" align=middle width=158.87685659999997pt height=22.853275500000024pt/>

<img src="/notes/tex/3b8837c91bd6802064c6ca1f573eefd2.svg?invert_in_darkmode&sanitize=true" align=middle width=236.90363895000002pt height=27.77565449999998pt/>

Therefore <img src="/notes/tex/622fa20bb0dccb4932f3bbdeb9911fdb.svg?invert_in_darkmode&sanitize=true" align=middle width=76.11948959999998pt height=24.65753399999998pt/>. The sum of fractions in the last step can be evaluated as an infinite geometric series, where <img src="/notes/tex/42b5521eed2cde8063b032c047e75d0c.svg?invert_in_darkmode&sanitize=true" align=middle width=38.315730749999986pt height=27.77565449999998pt/> and the sum is then <img src="/notes/tex/c631e3404998f43c2e9dd82c968b704e.svg?invert_in_darkmode&sanitize=true" align=middle width=107.62163774999999pt height=37.54198139999998pt/>.

## Mergesort

This algorithm sorts a list/array in time <img src="/notes/tex/cfde6d26888cfdf1fdb409e3c2aded3b.svg?invert_in_darkmode&sanitize=true" align=middle width=66.9313722pt height=24.65753399999998pt/>. The algorithm works as follows:

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

At each step of the recursion, the list (of size <img src="/notes/tex/2b4b9ed020dae80ebe8d4d680e46ae89.svg?invert_in_darkmode&sanitize=true" align=middle width=9.86687624999999pt height=14.15524440000002pt/>) is split into two halves of (roughly) size <img src="/notes/tex/ac036c6070f1558718f3bc3352c2a143.svg?invert_in_darkmode&sanitize=true" align=middle width=8.126022299999999pt height=22.853275500000024pt/>. Additionally, each merge step requires <img src="/notes/tex/7dc4ba0209d9b0e2068750b1b3fa9419.svg?invert_in_darkmode&sanitize=true" align=middle width=38.17727759999999pt height=21.18721440000001pt/> comparisons, so the overall time complexity is:

<img src="/notes/tex/3fe06f285c0109f2424c5fad8d148cbf.svg?invert_in_darkmode&sanitize=true" align=middle width=159.26239724999996pt height=22.465723500000017pt/>

<img src="/notes/tex/d8e1a5658ec84e11cfa47f95a35ce93d.svg?invert_in_darkmode&sanitize=true" align=middle width=125.77767839999999pt height=22.465723500000017pt/>

We can solve this recurrence with the master theorem. Here we have <img src="/notes/tex/cf34371d316f136e859347be44d26b44.svg?invert_in_darkmode&sanitize=true" align=middle width=38.82599489999999pt height=21.18721440000001pt/>, <img src="/notes/tex/d9437a17ba5a0df9089e53386bb19c86.svg?invert_in_darkmode&sanitize=true" align=middle width=37.19163689999999pt height=22.831056599999986pt/>, and <img src="/notes/tex/c3762a2d88fb8f48e610b1eeb16ae99b.svg?invert_in_darkmode&sanitize=true" align=middle width=92.56463039999998pt height=24.65753399999998pt/>. Then we get:

<img src="/notes/tex/fbb538e1a20882f505d503057992ca0f.svg?invert_in_darkmode&sanitize=true" align=middle width=72.56688614999999pt height=27.91243950000002pt/>

Which is asymptotically equal to <img src="/notes/tex/efcf8d472ecdd2ea56d727b5746100e3.svg?invert_in_darkmode&sanitize=true" align=middle width=38.17727759999999pt height=21.18721440000001pt/>. Therefore the overall complexity is <img src="/notes/tex/eb149ea5ebdddd486b96e52d373bb5b7.svg?invert_in_darkmode&sanitize=true" align=middle width=198.48674834999997pt height=24.65753399999998pt/>, as expected. 

## Binary Search

**Problem:** *Given a sorted list and a target value <img src="/notes/tex/0a8d5abd0504bcc0f25b96fe4945362a.svg?invert_in_darkmode&sanitize=true" align=middle width=9.075367949999992pt height=22.831056599999986pt/>, return the index of <img src="/notes/tex/0a8d5abd0504bcc0f25b96fe4945362a.svg?invert_in_darkmode&sanitize=true" align=middle width=9.075367949999992pt height=22.831056599999986pt/> in the sorted list or <img src="/notes/tex/6d0c6128e95ddd529082377484030522.svg?invert_in_darkmode&sanitize=true" align=middle width=21.00464354999999pt height=21.18721440000001pt/> if it is not in the list.*

This algorithm can be done fairly simply with a ternary oracle, but it can also be done with a binary oracle. I will assume the typical implementation where we have a ternary oracle that compares two elements <img src="/notes/tex/332cc365a4987aacce0ead01b8bdcc0b.svg?invert_in_darkmode&sanitize=true" align=middle width=9.39498779999999pt height=14.15524440000002pt/> and <img src="/notes/tex/deceeaf6940a8c7a5a02373728002b0f.svg?invert_in_darkmode&sanitize=true" align=middle width=8.649225749999989pt height=14.15524440000002pt/> and tells us if <img src="/notes/tex/aa99d366a0125c60e3800f3bed358ad4.svg?invert_in_darkmode&sanitize=true" align=middle width=39.96184334999999pt height=17.723762100000005pt/>, <img src="/notes/tex/7defd50098be0a3d3e6d4bf5ca764b65.svg?invert_in_darkmode&sanitize=true" align=middle width=39.96184334999999pt height=14.15524440000002pt/> or <img src="/notes/tex/78bdc28b55ffcf72965ced5360da934b.svg?invert_in_darkmode&sanitize=true" align=middle width=39.96184334999999pt height=17.723762100000005pt/>. 

**Solution:**

* Starting with the entire list, pick the middle element. If it is equal to <img src="/notes/tex/63bb9849783d01d91403bc9a5fea12a2.svg?invert_in_darkmode&sanitize=true" align=middle width=9.075367949999992pt height=22.831056599999986pt/>, then halt. If not, then go search the left half of the array if the middle element is greater than <img src="/notes/tex/63bb9849783d01d91403bc9a5fea12a2.svg?invert_in_darkmode&sanitize=true" align=middle width=9.075367949999992pt height=22.831056599999986pt/> and the right half if it is less than <img src="/notes/tex/63bb9849783d01d91403bc9a5fea12a2.svg?invert_in_darkmode&sanitize=true" align=middle width=9.075367949999992pt height=22.831056599999986pt/>.
* Repeat this until <img src="/notes/tex/63bb9849783d01d91403bc9a5fea12a2.svg?invert_in_darkmode&sanitize=true" align=middle width=9.075367949999992pt height=22.831056599999986pt/> is found or until a subarray of size 1 not equal to <img src="/notes/tex/63bb9849783d01d91403bc9a5fea12a2.svg?invert_in_darkmode&sanitize=true" align=middle width=9.075367949999992pt height=22.831056599999986pt/> is reached, at which point <img src="/notes/tex/e11a8cfcf953c683196d7a48677b2277.svg?invert_in_darkmode&sanitize=true" align=middle width=21.00464354999999pt height=21.18721440000001pt/> should be returned.

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

At each step of the algorithm, we make a call to a problem of size <img src="/notes/tex/ac036c6070f1558718f3bc3352c2a143.svg?invert_in_darkmode&sanitize=true" align=middle width=8.126022299999999pt height=22.853275500000024pt/> and also make use of the oracle, which takes constant time. Therefore:

<img src="/notes/tex/beaaad2b8a9ea6e9690ff36660c0cae0.svg?invert_in_darkmode&sanitize=true" align=middle width=87.60040079999999pt height=22.465723500000017pt/>

We can determine the complexity using the master theorem. We have <img src="/notes/tex/2873890d143e27fd897f843aee734521.svg?invert_in_darkmode&sanitize=true" align=middle width=83.32351334999998pt height=22.831056599999986pt/> and <img src="/notes/tex/e91aab9557531ccd77a1d77ad2a2c603.svg?invert_in_darkmode&sanitize=true" align=middle width=62.60656214999998pt height=24.65753399999998pt/>, which gives us:

<img src="/notes/tex/4c45e43f378a5c72179c2ea4a21d9f88.svg?invert_in_darkmode&sanitize=true" align=middle width=110.07818579999999pt height=27.91243950000002pt/> 

Which is directly equal to <img src="/notes/tex/3d425a215e8eeb2a056f553633aaae4a.svg?invert_in_darkmode&sanitize=true" align=middle width=32.46972299999999pt height=24.65753399999998pt/>. Hence the overall complexity is <img src="/notes/tex/99a6b43eebdfc2b732ceb86e1f72fdb5.svg?invert_in_darkmode&sanitize=true" align=middle width=188.61987209999998pt height=24.65753399999998pt/>.
