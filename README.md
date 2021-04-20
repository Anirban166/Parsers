Consider the following context free grammar to be used throughout the parser examples in this readme:
```cpp
Variables, V = {A, B, C} 
Terminals, T = {+, *, (, ), id}
Production rules, P: A -> A + B | B
                     B -> B * C | C
                     C -> (A) | id
Start symbol, S = A
```

### LL(1) Parser 
First, we need to remove the left recursion (rules of the form `A -> Aα | B` need to be substituted with `A -> BA'` and `A' -> αA' | ε`) prevalent in the first two production rules, i.e., `A -> A + B | B` will be `A -> BA'` plus `A' -> +BA' | ε` and likewise, `B -> B * C | C` will be `B -> CB'` plus `B' -> *CB' | ε`.

Thus, the production rules of our considered grammar after elimination of left recursion are:
```
A  -> BA'         ...rule 1
A' -> +BA' | ε    ...rules 2 and 3
B  -> CB'         ...rule 4
B' -> *CB' | ε    ...rules 5 and 6
C  -> (A)  | id   ...rules 7 and 8
```
Note the inclusion of new variables `A'` and `B'`, making `V = {A, A', B, B', C}`.

Next, we find the first and follow for these variables:
```
First(A)  = First(B) = First(C) = {(, id}
First(A') = {+, ε}
First(B') = {*, ε}
```
```
Follow(A)  = {$, )}
Follow(A') = {Follow(A)} = {$, )}
Follow(B)  = {(First(A') - ε), Follow(A)} = {+, $, )} 
Follow(B') = {Follow(B)} = {+, $, )} 
Follow(C)  = {(First(B') - ε), Follow(B)} = {*, +, $, )} 
```
With the aforementioned knowledge, we can now proceed to make the parsing table for our predictive parser:
```
rule 1 => First(A) => {(, id}
rule 2 => First(A' -> +BA') => {+}
rule 3 => Follow(A') => {$, )}
rule 4 => First(B) => {(, id}
rule 5 => First(B' -> *CB') => {*}
rule 6 => Follow(B') => {+, $, )} 
rule 7 => First(C -> (A)) => {(}
rule 8 => First(C -> id) => {id}
```

|    | id       | (        | )        | +          | *           | $         |
|----|----------|----------|----------|------------|-------------|-----------|
| A  | A -> BA' | A -> BA' |          |            |             |           |
| A' |          |          | A' -> ε  | A' -> +BA' |             |  A' -> ε  |
| B  | B -> CB' | B -> CB' |          |            |             |           |
| B' |          |          | B' -> ε  |  B' -> ε   | B' -> \*CB' |  B' -> ε  |
| C  | C -> id  | C -> (A) |          |            |             |           |

### LR(0) Parser

> coming soon
