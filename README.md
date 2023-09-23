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
Interestingly, addition and muiltipication work "as expected" - for example, you can multiple two numbers within the range and take the remainder: `5 * 6 (mod 12) = 30 (mod 12) = 6 (mod 12)`. Another example is `6*4 (mod 12) = 24 (mod 12) = 0 (mod 12)`. That's strange - we multiplied two non-zero numbers and got a zero!
That property is important enough to give it a name - in math, we say that `6` and `4` are `zero divisors`. It's quite straightforward to prove mathematically that if work work `mod n`, then there must be zero divisors if and only if `n` is not a prime. Therefore, prime numbers hold special significance in modular arithmetics!

### Substruction
In math, substraction is generally *addition of negative numbers*. What are negative numbers?
Well, the negative of a number is called *the additive inverse* of it, and that's number that adding it to the positive number results in a 0.
Under modular arithmetics, the negative of a number is simply the modular base minus the positive number. For example, under `mod 12` we get `-3 = 12-3 (mod 12) = 9 (mod 12)`. Because addition works "as expected" and because substraction is practically and addition operation, substruction is also straightforward.

### Division
