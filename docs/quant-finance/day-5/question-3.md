# Day 5 · Burning Ropes

*Edited by Charles and Sophia*

**Brainteaser · Timing · Invariants**

[Original question on OpenQuant](https://openquant.co/questions/burning-ropes)

## Problem

You have two ropes and a lighter. Each rope takes exactly 60 minutes to burn
from one end, but its burning speed varies along its length. How can you
measure exactly 45 minutes?

Use the usual puzzle model: a segment has the same burn-time contribution
from either direction, and separate flames do not alter one another's burn rates
before they meet.

!!! tip "Core idea"
    Lighting both ends consumes the remaining single-ended burn time twice
    as fast, even when physical burning speed varies.

## Short solution

**1. Start three flames simultaneously.** Light both ends of rope A and one end
of rope B.

**2. Wait until rope A finishes.** Its two flames jointly consume 60 minutes
of single-ended burn time in

$$60/2=30\text{ minutes}.$$

At that moment, rope B has 30 minutes of single-ended burn time remaining.

**3. Light the other end of rope B.** Its remaining portion now burns from
both ends, finishing after

$$30/2=15\text{ additional minutes}.$$

!!! success "Answer"
    $$\boxed{30+15=45\text{ minutes}}$$

!!! warning "Watch out"
    Half the physical length need not represent half the burn time.
    The flames do not need to meet at the physical midpoint.

??? note "Why uneven burning does not change the timing"
    After $t$ minutes, each flame has consumed a portion requiring $t$ minutes
    to burn from its respective end. Until the flames meet, these portions do
    not overlap. Together they exhaust a 60-minute rope when $t+t=60$.
    The same reasoning applies to the 30-minute remainder of rope B.

---

[Previous question](question-2.md) · [Day 5 overview](index.md) · [Next question](question-4.md)
