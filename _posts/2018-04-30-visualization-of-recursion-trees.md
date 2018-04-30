---
layout: post
title:  "Visualization of recursion trees"
date:   2018-04-30 07:36:56 -0700
categories: archive
---

> This is an old post from 2012 which I wrote when I was a graduate student. My old blog was hosted on my university's CS department's server which seem to no longer exist. I had backups of some posts from my old blog which I am republishing now as it might be useful. 

I was trying to understand recursion and took up the problems of generating permutations and combinations of string as an example to work on.

Here is the Python implementation of Java code to generate combinations by Robert Sedgewick and Kevin Wayne:

```python
class Combinations:
    ''' Time complexity is O(n * 2^n) '''
    def __init__(self):
        ''' Initialize a list to hold the combinations '''
        self.st= []

    def combi(self, prefix, s):
        ''' Recursively form combinations
        >>> combi("",'abc')
        ['a', 'ab', 'abc', 'ac', 'b', 'bc', 'c']
        '''
        if len(s)==0: return
        else:
            self.st.append(prefix+s[0])
            self.combi(prefix+s[0],s[1:])
            self.combi(prefix,s[1:])
            return self.st

if __name__=='__main__':
    c = Combinations()
    print c.combi("",'abc')
```

The end result is `['a', 'ab', 'abc', 'ac', 'b', 'bc', 'c']`. I drew the whole recursion tree to understand what was happening. The Depth First Traversal of this tree yields the result.

| ![Combinations Recursion Tree]({{ "/assets/images/combi_graph.png" | absolute_url }})
|:--:| 
| *Combinations Recursion Tree* |


And here is the Python implementation of the Java code to generate permutations by Robert Sedgewick and Kevin Wayne with minor changes:
```python
class Permute:
    ''' Time complexity is O(n!) '''
    def __init__(self,st):
        ''' Converting the string into a list, initialing output list'''
        self.s = list(st)
        self.op=[]

    def swap(self,a,b):
        ''' Swap elements at position a & b'''
        self.s[a],self.s[b] = self.s[b],self.s[a]        

    def perm(self,i,n):
        ''' Recursively permute '''
        if i==n:
            self.op.append(''.join(self.s))
            return
        for j in range(i,n):
            self.swap(i,j)
            self.perm (i+1,n)
            self.swap(i,j)

        return self.op

if __name__=='__main__':
    string = 'abc'
    n = len(string)
    p = Permute(string)
    print p.perm(0,n)
```

The end result is `['abc', 'acb', 'bac', 'bca', 'cba', 'cab']`. As before, the Depth First Traversal of the recursion tree yields the result. Here, when a leaf node(base case)  is reached, the string is appended to the output list. The leaf nodes are shown in red.

| ![Permutations Recursion Tree]({{ "/assets/images/perm_graph.png" | absolute_url }})
|:--:| 
| *Permutations Recursion Tree* |