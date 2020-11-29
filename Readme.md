given a value S, collatz conjecture says that we end up with the values 4, 2, 1 just using thos directives:

if S is even, devide it by 2
if S odd, put 3*S + 1 in S
repeat.

As the result of the odd computation is always even, we can integrate the /2 directly.

Using binary represetation to imagine the manipulation on numbers ease to see it as shifting bits and summing values.
if give a naive algo (pseud C code)

while (S > 1) {
  if (S & 1) { // odd
    S = ((S << 1) + S + 1) >> 1;
  } else { //even
    S >> 1;
  }
}

(S << 1 + S + 1) >> 1; in binary ca be rewrote as
(S << 2 - S + 1) >> 1
and thus
(S << 1) - (S >> 1)
Which means that the result of the next step (for odd number) is the value of step n shifted to left minus the value of n shifted to right.

As a matter of fact, this implies that the next lower bit will always be the the bit that was formerly at position 1 (starting count from 0).

to unify odd en even notation in loop, we may rewrite using the value of the lower bit:

let s0 = s[0]
then the expression (odd or even) can be rewrote as:
s = (s << 1) * s0 + (1 - 2 * s0) * (s >> 1);
  = s0 * ((s << 1) - s) + (s >> 1)
or even:
s = s0 * (s + 1) + (s >> 1);

in case s is odd s + 1 will end with 0 and a carry will be propagated.
This explains the pattern on nuber 2^n -1 which have only 1:
127
127      aka 0b0000000000000000000000000000000000000000000000000000000001111111
191      aka 0b0000000000000000000000000000000000000000000000000000000010111111
287      aka 0b0000000000000000000000000000000000000000000000000000000100011111
431      aka 0b0000000000000000000000000000000000000000000000000000000110101111
647      aka 0b0000000000000000000000000000000000000000000000000000001010000111
971      aka 0b0000000000000000000000000000000000000000000000000000001111001011
1457     aka 0b0000000000000000000000000000000000000000000000000000010110110001

As the carry is propagated, on s, the number 2^n -1 +1 become 2^n, thus and extra 1 on the left followed by only zeros. while s >> 1 moves all preceding 1 on the right:
1111111 => (1111111 + 1) + (1111111 >> 1) = 10000000 + 11111 = 10111111;

One can see that this can be generalized to any number with trailing 1, that is why when a number appears with some kind of tail containing only 1, its tails seems to reduce at each step, until it reaches 0.

One may also notice that the tail srictly always reduce its size because a 0 is always injected between the 2^n result and the (2^n - 1)>> 1 which is alway < 2^(n-1)

At the same time, the value of s increases by half its size (obviously...), that's why those numbers see their value growing very fast when those 1-tails appears during cmputation (that is what happens to 63, for example, why has 4 1-tails appearing during computation making values "bounce" and eventually reach 4616)


