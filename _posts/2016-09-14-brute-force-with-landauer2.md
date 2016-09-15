---
layout:     post
title:      Corrections on Brute Force
date:       Wed, 14 Sep 2016 19:27:12 -0400
---
Last week I discussed Landauer's principle and attempted to determine how long
an encryption key would need to be before the necessary energy exceeded the
global energy consumption rate.

I made a mistake.

My algebra was correct, but I tripped at the finish line.  I made a mistake
entering the formula into [WolframAlpha][] and came up with a lower answer than
I should have.  Essentially I botched my parenthesis, one of the perennial
annoyances of software development.

Here's the formula I mistakenly wound up entering:

$$
\require{cancel}
\cancel{
\ln{\left(
    \frac{
        \frac{3.89 \times 10^{20}}
        {(1.381 \times 10^{-23}) \cdot 293 \cdot \ln{2}}
    }
    {\ln{2}}
    \right)
}
= 95.1
}
$$

Note how the natural log (with its parenthesis) encloses the entire left-hand
side.  The formula I _intended_ to enter has the natural log of the overall
numerator:

$$
\frac{
    \ln{\left(
        \frac{3.89 \times 10^{20}}
        {(1.381 \times 10^{-23}) \cdot 293 \cdot \ln{2}}
    \right)}
}
{\ln{2}}
= 136.67
$$

And what a difference that makes:  from 96 bits to 137.  This actually somewhat
exceeds the default key length for AES (128 bits) although AES can also support
longer key sizes.  Enumerating all possible 128-bit keys would require a little
less than 0.2% of the global energy consumption in 2013.

This comes out to 211 billion kilowatt-hours.  [WolframAlpha][] helpfully
informs me that this is 23 days' worth of combined energy production from all
the nuclear power plants on the globe.

While this is a generous upper limit of search capabilities, it suggests that a
sufficiently powerful and determined organization _could_ brute-force a 128-bit
key.  It would require a great deal of energy, a near-optimum computing
facility, and weeks or months to do, but it could be done.  And with growing
energy production and computing efficiency, it may be possible in the
not-too-distant future.

Luckily, each additional bit of key material doubles the search space.  All it
takes to avoid brute force is to make encryption keys that are just that much
longer.

[WolframAlpha]: http://www.wolframalpha.com/
