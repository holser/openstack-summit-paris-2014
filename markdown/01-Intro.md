# Definitions and Scope of this talk

+ Failure of one of the components
  at one moment of the time
+ No force majeure events
+ Uncommon physical destruction (not deterioration) 
  of components
+ Zombie Apocalypse

Note: First of all, before we start talking about OpenStack and its underlying components themselves, let's define High Availability and part of High Availability we are going to cover in this talk.

High Availability is a characteristic of a system that retains certain level of availability even if system is exposed to failures.

Scope of this talk is about retaining availability of OpenStack cluster in case of failure of one of the underlying components:

We do not consider:
1. Simultaneous failure of several components
2. Force majeures and natural disasters
3. Non-common failure events (poweroff, kernel panic, network partitioning)
4. Physical destruction of the hardware (hammers, bombs, zombies)
