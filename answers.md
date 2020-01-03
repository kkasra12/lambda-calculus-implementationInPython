# Question1
- ``λαβγ.((αβF)T(αγF))T(βγF) ≡  λαβγ.((αβ(λxy.y))T(αγ(λxy.y)))T(βγ(λxy.y))``
- Initially `xor` function should be defined.\
as we know:
```
xor(a,b) = if a (not b) else b
```
So using lambda calculus:
```
xor α β 𑁔 λαβ.α(¬β)β  𑁔 λαβ.α((λx.xFT)β)β 𑁔  λαβ.α((λx.x(λuv.v)(λab.a))β)β
```
therefor the answer is:
```
xor (or α β) (and not α γ) 𑁔
(or α β)(¬(and (¬α) γ))(and (¬α) γ) 𑁔
(αTβ)(¬((¬α)γF))((¬α)γF) 𑁔
(αTβ)(((αFT)γF)FT)((αFT)γF) 𑁔
(α(λxy.x)β)(((α(λxy.y)(λxy.x))γ(λxy.y))(λxy.y)(λxy.x))((α(λxy.y)(λxy.x))γ(λxy.y))
```

# Question2
As we know *The predecessor function* is defined as below:
```
Φ ≡ (λpz.z(S(pT))(pT))
P ≡ (λn.nΦ(λz.z00)F
```
So `≥` can be defined as:
```
Z ≡ λx.xF¬F
G ≡ (λxy.Z(xPy))
```
according to `a>b 𑁔 not(b⩾a)` the definition for `>`:
```
B 𑁔 (λxy.¬Gyx)) 𑁔 (λxy.¬Z(yPx))) 𑁔 (λxy.¬((yPx)F¬F)))
```
In the same way, Initialy the `≤`(less than or equal) should be defined:
```
L ≡ (λxy.Z(yPx))
  ```
according to `a<b 𑁔 not(b⩽a)`,the definition for `<` is:
```
S = (λxy.¬Lyx)) 𑁔 (λxy.Z(xPy))) 𑁔 (λxy.¬((xPy)F¬F)))
```
# Question3
In order to define signed numbers, lets consider each number as pair of numbers, which one them is zero always. As we know `pair (a,b)` can be represented in λ-calculus using the function `(λz.zab)`
```
pair 𑁔 λa,b.(λz.zab) 𑁔 λa,b,z.zab
```
A natural number is converted to a signed number by,
```
convert 𑁔 λx.pair x 0 𑁔 λx.λz.z(x 0) 𑁔 λx,z.z(x 0)
```
Negation is performed by swapping the values.
```
neg 𑁔 λx.pair (xF) (xT)
```

For instance:
```
+1 = convert 1 = pair 1 0 = λz.z(1 0)
-1 = neg +1
   = pair (+1F) (+1T)
   = pair (λz.z(1 0)F) (λz.z(1 0)T)
   = pair (λz.z(1 0)(λxy.y)) (λz.z(1 0)(λxy.x))
   = pair ((λxy.y)(1 0)) ((λxy.x)(1 0))
   = pair 0 1
   = λz.z(1 0)
```

all signed numbers are natural if and only if one of the pair is zero.The `OneZero` function achieves this condition. to understand how this function works here is the psuedocode:
```python
def OneZero(x:pair):
  if (not Z(xT)) and (not Z(xF)):
    return OneZero(pair(P(xT))(P(xF)))
  else:
    return x
```
it is as same as:
```python
def OneZero(x:pair):
  if not (Z(xT) or Z(xF)): # ¬∨(xT)(xF)
    return OneZero(pair(P(xT))(P(xF)))
  else:
    return x
```
`IF` condition in λ-calculus can be defined as:
```
IF 𑁔 λxab.xab
```
> it means `if x then a else b`

So:
```
OneZero 𑁔 λx.IF(¬∨(ZxT)(ZxF)) (OneZero pair (P(xT))(P(xF))) (x)
```

for instance:
```
OneZero pair 5 3 = OneZero pair 4 2 = OneZero pair 3 1 = OneZero pair 2 0 = pair 2 0
```

Now addition and subtraction can be defined like this:
```
add = λx,y.OneZero pair ((xT)S(yT)) ((xF)S(yF))
sub = λx,y.OneZero pair ((xT)P(yT)) ((xF)P(yF))
```

# Question4

As we know:
```
n/m = if n⩾m then 1+(n-m)/m else 0
```
using λ-calculus notations divide function is:
```
div = λn,m.IF(G n m)(S(div (nPm)m))(0)
```

# Question5

As we know multiplication of two numbers x and y can be computed using the following
function:
```
M 𑁔 (λxyz.x(yz))
```
recursive form of the **factorial function** is:
```
def fact(n):
  if n==0:
    return 1
  else:
    return n*fact(n-1)
```
using the λ-calculus notations the factorial function is:
```
fact = λn.IF(Zn)(1)(M n fact(Pn))
```
