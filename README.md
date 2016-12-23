# QNaNs.jl
Simplifying use of quiet NaNs to propagate information from within numerical computations
```ruby
                                                       Jeffrey Sarnoff © 2016-Mar-26 at New York
```

####Quick Look

```julia
> Pkg.clone("https://github.com/JeffreySarnoff/QNaN.jl") # expects Julia v0.5+
```
```julia
> using QNaN
> anan = qnan(36)
NaN
> typeof(anan)
Float64
> payload = qnan(anan)
36
> anan = qnan(Int32(-77))
NaN32
> payload = qnan(anan); payload, typeof(payload)
-77, Int32
> isqnan(anan), isqnan(NaN)
true, true
> isnan(anan), isnan(NaN)   # they propogate as NaNs
true, true
> isqnan(anan), isjnan(NaN) # Julia's NaNs are discernable
true, false

```
####About QNaN Propogation

A QNaN introduced into a numerical processing sequence usually will propogate along the computational path without loss of identity unless another QNaN is substituted or an second QNaN occurs in an arithmetic expression.

AFAIK Julia propogates the lhs of `-`. When two qnans have propogated to the same function, select qnan1 over qnan2 this way: ```return qnan1 - qnan2```. ## QNaN.jl
#####quiet NaNs were designed to propagate information from within numerical computations
```ruby
                                                       Jeffrey Sarnoff © 2016-Mar-26 at New York
```

####Quick Look

```julia
> Pkg.clone("https://github.com/J-Sarnoff/QNaN.jl") # expects Julia v0.4-any
```
```julia
> using QNaN
> anan = qnan(36)
NaN
> typeof(anan)
Float64
> payload = qnan(anan)
36
> anan = qnan(Int32(-77))
NaN32
> payload = qnan(anan); payload, typeof(payload)
-77, Int32
> isqnan(anan), isqnan(NaN)
true, true
> isnan(anan), isnan(NaN)   # they propogate as NaNs
true, true
> isqnan(anan), isjnan(NaN) # Julia's NaNs are discernable
true, false

```


#=
  A float64 quiet NaN is represented with these 2^52-2 UInt64 hexadecimal patterns:
  (positive) UnitRange(0x7ff8000000000000,0x7fffffffffffffff) has 2^51-1 realizations
  (negative) UnitRange(0xfff8000000000000,0xffffffffffffffff) has 2^51-1 realizations
  A float32 quiet NaN is represented with these 2^23-2 UInt32 hexadecimal patterns:
  (positive) UnitRange(0x7fc00000,0x7fffffff) has 2^22-1 realizations
  (negative) UnitRange(0xffc00000,0xffffffff) has 2^22-1 realizations
  A float16 quiet NaN is represented with these 2^10-2 UInt16 hexadecimal patterns:
  (positive) UnitRange(0x7e00,0x7fff) has 2^9-1 realizations
  (negative) UnitRange(0xfe00,0xffff) has 2^9-1 realizations
  Julia appears to assign as NaNs quiet NaNs with a payload of zero:
  0xfff8000000000000, 0xffc00000, 0xfe00, 0x7ff8000000000000, 0x7fc00000, 0x7e00.
=#

####About QNaN Propogation

A QNaN introduced into a numerical processing sequence usually will propogate along the computational path without loss of identity unless another QNaN is substituted or an second QNaN occurs in an arithmetic expression.

When two qnans are arguments to the same binary op, Julia propagates the qnan on the left hand side. 


When the information carried is costly to aquire, and one of two payload is usually the more importantd,  a pairing function may propogate more than one payload.  This is easiest whith smaller payloads. Pairing starts with zero and stops as zero is unpaired.  The initial pair: ``pair(pair(zero, first_payload),second_payload)``.


```julia
> using QNaN
> function test_rhs_of_subtract()
    lhs = qnan(-64)
    rhs = qnan(100)
    (qnan(lhs-rhs)==qnan(lhs), qnan(rhs-lhs)==qnan(rhs))
  end;
> test_rhs_of_subtract()
(true, true)
```


#####William Kahan on QNaNs

NaNs propagate through most computations. Consequently they do get used. ... they are needed only for computation, with temporal sequencing that can be hard to revise, harder to reverse. NaNs must conform to mathematically consistent rules that were deduced, not invented arbitrarily ...

NaNs [ give software the opportunity, especially when searching ] to follow an unexceptional path ( no need for exotic control structures ) to a point where an exceptional event can be appraised ... when additional evidence may have accrued ...  NaNs [have] a field of bits into which software can record, say, how and/or where the NaN came into existence. That [can be] extremely helpful [in] “Retrospective Diagnosis.”

-- IEEE754 Lecture Notes (highly redacted)

References:

[William Kahan's IEEE754 Lecture Notes](http://www.eecs.berkeley.edu/~wkahan/ieee754status/IEEE754.PDF)

["An Elegant Pairing Function" by Matthew Szudzik](http://szudzik.com/ElegantPairing.pdf)



When the information carried is costly to aquire, and one of two payload is usually the more importantd,  a pairing function may propogate more than one payload.  This is easiest whith smaller payloads. Pairing starts with zero and stops as zero is unpaired.  The initial pair: ``pair(pair(zero, first_payload),second_payload)``.


```julia
> using QNaN
> function test_rhs_of_subtract()
    lhs = qnan(-64)
    rhs = qnan(100)
    (qnan(lhs-rhs)==qnan(lhs), qnan(rhs-lhs)==qnan(rhs))
  end;
> test_rhs_of_subtract()
(true, true)
```


#####William Kahan on QNaNs

NaNs propagate through most computations. Consequently they do get used. ... they are needed only for computation, with temporal sequencing that can be hard to revise, harder to reverse. NaNs must conform to mathematically consistent rules that were deduced, not invented arbitrarily ...

NaNs [ give software the opportunity, especially when searching ] to follow an unexceptional path ( no need for exotic control structures ) to a point where an exceptional event can be appraised ... when additional evidence may have accrued ...  NaNs [have] a field of bits into which software can record, say, how and/or where the NaN came into existence. That [can be] extremely helpful [in] “Retrospective Diagnosis”

-- IEEE754 Lecture Notes (highly redacted)

References:

[William Kahan's IEEE754 Lecture Notes](http://www.eecs.berkeley.edu/~wkahan/ieee754status/IEEE754.PDF)

["An Elegant Pairing Function" by Matthew Szudzik](http://szudzik.com/ElegantPairing.pdf)


