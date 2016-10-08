---
layout:     post
title:      Landauer's Principle into the Future
date:       Sat, 08 Oct 2016 16:20:21 -0400
---
Last month I talked about Landauer's principle in the context of brute-forcing
encryption keys.  As a quick review, Landauer's principle defines a minimum
necessary energy to flip a single bit as a function of the system's temperature.
This is tied to the laws of thermodynamics and sets a firm lower bound on the
amount of energy needed to brute-force an encryption key of a specified length.

The conclusion---after some miscalculation---was that current global energy
production _could_ fuel a 100%-efficient brute force computation, but that it
would certainly be large enough to be noticeable and might not even produce
proper results.  This was based on a very generous calculation:  that
enumerating a key cost exactly one bit-flip operation.  It also ignored the cost
of performing the decryption attempt with each key---a cost that would
necessarily be at least the same as generating the key itself.

But technology is moving faster than ever these days, and yesterday's
supercomputers are tomorrow's cell phones.  What sort of encryption keys are
needed to protect information well into the future, even against ever-rising
technological development?  In short, what does key-cracking look like with more
energy?

# The Kardashev Scale #
I'm going to summarize this briefly, but you can lose an afternoon on the
[Wikipedia page][Kardashev] if you are so inclined.  The Kardashev scale is a
three-point scale which can describe advanced civilizations based on their
energy production capabilities.  It was originally described about fifty years
ago and has since been re-described, extended, pre-pended, sub-divided, and
otherwise roundly discussed.  But here are the basics:

A Kardashev type I civilization can capture energy equivalent to the total
energy being input to Earth by the sun.  This shakes out to about $$10^{16}$$
watts.

A Kardashev II civilization can capture energy equivalent to the combined output
of our sun.  This is about $$3.846 \times 10^{26}$$ watts.

Finally, a Kardashev III civilization can capture energy on a galactic scale, or
approximately $$4 \times 10^{37}$$ watts.

You are here:  humans are currently a type 0, as we have a few centuries to go
before reaching type I assuming consistent future growth.  So anything too
difficult for a Kardashev I civilization to break can stay safe for a long time.

# Adjusting the Formula #
Previously, we adapted the Landauer formula to solve for the number of bits
needed in an encryption key.  However, since the Kardashev scale is described in
terms of wattage (which is energy per time), a better approach would be to see
how much time it would take to break a key of a given size.

Since each step of the Kardashev scale also depends on successful space travel
and infrastructure, it makes sense to place the super-efficient computer in cold
vacuum rather than room temperature.  So let's use 2.725K as the system
temperature, as measured from the Cosmic Microwave Background.  (Getting cooler
than this would require refrigeration efforts, which cost more energy that
could be used for key-cracking efforts.)

The new formula looks something like this:

$$
s = \frac{2^{n} \times 2.725 \times k \ln{2}}{W}
$$

Where _s_ is the time in seconds, _n_ is the number of bits in the key, _k_ is
the [Boltzmann constant][], and _W_ is the available wattage.

# 128-bit Key #
Typical key size for symmetric block ciphers is 128 bits.  This is the default
for AES which is widely used and even has built-in hardware support on many
chipsets.  Let's see how it holds up to advanced civilizations.

| Kardashev Level | Wattage                  | Time                          |
|-----------------|--------------------------|-------------------------------|
| I               | $$10^{17}$$              | 0.887 sec                     |
| II              | $$3.846 \times 10^{26}$$ | $$2.308 \times 10^{-11}$$ sec |
| III             | $$4 \times 10^{37}$$     | $$2.219 \times 10^{-22}$$ sec |

Hrm, that doesn't look very promising.  While still a colossal energy
expenditure, it would be almost trivial for an advanced civilization to
brute-for a 128-bit key.

# 192-bit Key #
This key size is a bit of an odd duck, as it isn't a power of two.  Still, AES
supports it, so let's give it a try:

| Kardashev Level | Wattage                  | Time                           |
|-----------------|--------------------------|--------------------------------|
| I               | $$10^{17}$$              | $$5.188 \times 10^{11}$$ years |
| II              | $$3.846 \times 10^{26}$$ | 13 years, 179 days, 5 hours    |
| III             | $$4 \times 10^{37}$$     | 0.004 sec                      |

That's quite a bit better.  It's still breakable by a super-advanced
civilization, but it's well out of reach of anything in humanity's foreseeable
future.

# 256-bit Key #
Just for completeness' sake, let's try a 256-bit key.  This is the largest key
size that AES supports:

| Kardashev Level | Wattage                  | Time                           |
|-----------------|--------------------------|--------------------------------|
| I               | $$10^{17}$$              | $$9.571 \times 10^{30}$$ years |
| II              | $$3.846 \times 10^{26}$$ | $$2.489 \times 10^{20}$$ years |
| III             | $$4 \times 10^{37}$$     | 2.392 million years            |

Ha!  With a 256-bit key, brute-forcing requires so much energy that even the
bug-eyed Andromedans can't manage it in time!  Although you probably have bigger
things to worry about if bug-eyed Andromedans are invading.

Really this all goes to show that brute-forcing an encryption key is the least
effective way to break security.

[Kardashev]: https://en.wikipedia.org/wiki/Kardashev_scale
[Boltzmann constant]: https://en.wikipedia.org/wiki/Boltzmann_constant
