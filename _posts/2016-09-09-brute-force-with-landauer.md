---
layout:     post
title:      Brute-Force Decryption with Landauer
date:       Fri, 09 Sep 2016 21:43:04 -0400
---
Anyone who knows me well enough knows I'm a fan of science fiction.  A
well-written science fiction story will often expand on a known scientific field
in a self-consistent way.  Common trends of late tend to focus on information
and computational ability (especially in the cyberpunk sub-genre), but
theoretical physics remains a perennial favorite.  (Faster-than-light travel,
anyone?)  So I was pretty excited recently when I stumbled over Landauer's
principle.

For those who aren't aware, [Landauer's principle][] essentially provides a
formula for computing the minimum amount of energy necessary to flip a single
bit.  (It's a good deal more complicated and specific than that, but I don't
have the chops in theoretical physics to process it all.)  The energy
requirement is a function of temperature, and it's pretty miniscule even at room
temperature.  Given that it's a consequence of the laws of thermodynamics,
however, I'm confident in claiming that it's a hard limit.

(Of course, modern computer systems are nowhere near efficient enough to bump
into this.  Anybody who's picked up a hot laptop can tell you that there's room
for more energy efficiency.)

What can we do with this knowledge?  Well, if we know how many bit-flipping
operations it takes to compute something, we know at least how much energy is
needed.  Conversely, if we know how much energy we have available, we know the
maximum amount of computing we can do.  But Landauer's principle indicates
something like $$2.75 \times 10^{-21}$$ joules per bit at room temperature.  Won't we need
to flip a lot of bits to add up to anything substantial?

# Brute-forcing Encryption Keys #

That should do the trick!  Really the only way to beat a negative exponent is
with a positive exponent.  How many bit-flip operations would an attacker need
to perform to test an encryption key?  Difficult to tell:  it depends on the
encryption system and how well optimized the test execution is.  But such a
system will need to flip at least one bit in order to change one possible key
value to another.  So let's say one bit-flip per encryption key, to keep the
math simple.

Now, according to [Landauer's principle][], the energy E needed to flip one bit
is given by the formula:

$$
E = kT\ln{2}
$$

Where k is the [Boltzmann constant][] and T is the temperature of the system.
The energy needed to flip all the bits in an N-bit key is:

$$
E = 2^n kT\ln{2}
$$

Solving for N:

$$
\frac{
    E
}
{
    kT\ln{2}
}
= 2^n
$$

$$
\frac{
    \ln{
        \frac{E}
            {kT\ln{2}}
    }
}
{\ln{2}}
= n
$$

That doesn't look very elegant, but now we can see how many different keys an
attacker
can generate with a given amount of energy and a given ambient temperature.
(But remember they also have to power the decryption process itself.)

How much energy would our hypothetical attacker have, anyway?

# Global Energy Production #

Go big or go home, as they say.

The International Energy Agency [estimates][IEA estimate] that in 2013 the
total world energy consumption was $$3.89 \times 10^{20}$$ joules.  Let's say our attacker puts all
of this energy into the hypothetical key-cracker, running at a cool 20°C (293K):

$$
\frac{
    \ln{
        \frac{3.89 \times 10^{20}}
        {(1.381 \times 10^{-23}) \cdot 293 \cdot \ln{2}}
    }
}
{\ln{2}}
= 95.1
$$

Not bad!  So if we have an encryption key with at least 96 bits, it would be
physically impossible to brute-force the key without access to a year's worth of
global energy production.  This is also much smaller than modern key-size
recommendations, so there's even some margin for error in things like AES
or Twofish (128-bit key sizes).

Given the caveats and cost overhead, literally any other approach would be
cheaper and easier for our attacker.  Depending on power, patience, and audacity
our attacker would most likely focus on seizing hardware, interrogating users,
or even sabotaging public encryption infrastructure.

Despite it's name, "brute force" is really the _least_ frightening option.

[Landauer's principle]: https://en.wikipedia.org/wiki/Landauer's_principle
[Boltzmann constant]: https://en.wikipedia.org/wiki/Boltzmann_constant
[IEA estimate]: https://www.iea.org/publications/freepublications/publication/KeyWorld_Statistics_2015.pdf
