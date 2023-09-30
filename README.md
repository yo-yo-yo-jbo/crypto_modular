# Introduction to cryptography - modular arithmetics
In the last blogpost we introduced cryptography and dicsussed `Monoalphabetic Substitution Ciphers`. We could already see operations that are *cyclic*.  
For example, in Caesar Cipher we *rotated* the letters by 3, which we could imagine as a "circle" with the 26 letters being rotated.  
That kind of rotation and arithmetic operations on it is called `Modular arithmetics`, and while the name sounds scary - some of it is quite straightforward.

## Modular Arithmetics - basic operations
The usual example for modular arithmetics is a clock - we know that 5 hours after 11:00 o'clock is precisely 4:00 (assuming a 12-hour clock).  
That works because the *remainder* of `11+5=16` *modulu 12* is `4`. In mathematical notion, we write `16 = 4 (mod 12)`.  
Normally we work with a finite set of numbers (between `0` and `11` in our case).  
Besides the obvious clock example, if you've ever worked with languages like C or Assembly you already kind of got exposed to the idea anyway:

```c
#include <stdio.h>

int main()
{
    volatile unsigned char x = 255;
    printf("%d\n", x+1);
    return 0;
}
```

This prints out zero since bytes (`unsigned char`) work `mod 256`.

### Addition and multipication
Interestingly, addition and muiltipication work "as expected" - for example, you can multiple two numbers within the range and take the remainder: `5 * 6 (mod 12) = 30 (mod 12) = 6 (mod 12)`.  
Another example is `6*4 (mod 12) = 24 (mod 12) = 0 (mod 12)`. That's strange - we multiplied two non-zero numbers and got a zero!  
That property is important enough to give it a name - in math, we say that `6` and `4` are `zero divisors`. It's quite straightforward to prove mathematically that if work work `mod n`, then there must be zero divisors if and only if `n` is not a prime.  
Therefore, prime numbers hold special significance in modular arithmetics!

### Substruction
In math, substraction is generally *addition of negative numbers*. What are negative numbers?  
Well, the negative of a number is called `the additive inverse` of it, and that's number that adding it to the positive number results in a `0`.  
Under modular arithmetics, the negative of a number is simply the modular base minus the positive number. For example, under `mod 12` we get `-3 = 12-3 (mod 12) = 9 (mod 12)`.  
Because addition works "as expected" and because substraction is practically and addition operation, substruction is also straightforward.

### Division
Division is probably the most complicated "basic" arithmetic operation, since it's not defined for every two numbers.  
Just like we said substruction is addition of the additive inverse of a number, so does division is multipication with the `multipicative inverse` of the number. That inverse is the number that results in a multipication result of `1`.  
The problem is that we can't find such an inverse for every number - `0` cannot have such an inverse (just like in real numbers) since `0` times everything is still `0` so we never get to `1`.  
Besides `0` we have the `zero divisors` I mentioned earlier - it's trivial to prove that all numbers that are not `zero divisors` have a multipicative inverse.  
Finding that inverse is a different story. Naively, we could always check every nunmber, but if the modulus is too big then finding it will take a very long time (remember that a number `n` takes `lg n` bits to represent it, so brute-forcing all numbers is an *exponential operation* and therefore - expansive).  
To see an example: `5*6 (mod 29) = 30 (mod 29) = 1 (mod 29)`, so we say `6` is the multipicative inverse of `5`, and also, for example, `2/5 (mod 30) = 2*6 (mod 30) = 12 (mod 30)`.
Well, it turns out we could use the [Euclidean Algorithm](https://en.wikipedia.org/wiki/Euclidean_algorithm) to find that number, very efficiently!

## The Euclidean Algorithm
The Euclidean Algorithm generally finds the `Greatest Common Divisor (GCD)` of two given numbers, and does so very efficiently.  
Naively we can calculate the GCD by taking the lowest prime powers of each number, but prime factorization is a computationally difficult problem (in terms of complexity). Therefore, we need a different trick.
The basic idea is to see how many times the smallest number "fits" in the larger number but take the remainder only, and then swap roles.
Let's take the numbers `5` and `29` for example:
1. To calculate `gcd(29, 5)` we see that `29 = 5*5 + 4`, so `4` is the remainder. Therefore, `gcd(29, 5)` = `gcd(5, 4)`.
2. To calculate `gcd(5, 4)` we see that `5 = 4*1 + 1`, so `1` is the remainder. Therefore, `gcd(5, 4)` = `gcd(4, 1)`.
3. To calculate `gcd(4, 1)` we see that `4 = 1*4 + 0`, so `0` is the remainder. Since we got to a remainder of `0`, the final result is `1`.
Therefore, `gcd(29, 5) = 1`.

The reason why I showed the entire computation is that we can clearly see that the GCD can be expressed as a linear combination of the inputs!
For example, `4*29-23*5 = 1` (this can be derived from the 3 steps I showed earlier). 



