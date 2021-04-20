Consider the following context free grammar to be used throughout the parser examples in this readme:
```cpp
Variables, V = {A, B, C, D} 
Terminals, T = {+, *, (, ), id}
Production rules, P: A -> A + B | B
                     B -> B * C | C
                     C -> (D) | id
Start symbol, S = A
```

### Predictive Parser 
First, we need to remove the left recursion (rules of the form `A -> Aα | B` need to be substituted with `A -> BA'` and `A' -> αA' | ε`) prevalent in the first two production rules, i.e., `A -> A + B | B` will be `A -> BA'` plus `A' -> +BA' | ε` and likewise, `B -> B * C | C` will be `B -> CB'` plus `B' -> *CB' | ε`.

