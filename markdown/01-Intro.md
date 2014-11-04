# Definitions and Scope of this talk
\+ Failure of a single component
  at a single point in time

\- Force majeure events

\- Uncommon physical destruction (not deterioration)
  of components

Note: Speaker - Vladimir Kuklin:

First of all, before we start talking about OpenStack and its underlying components individually, let's define High Availability and what kind of High Availability we are going to cover in this talk.

High Availability is a characteristic of a system that retains a certain level of availability, even if the system is exposed to failures.

Scope of this talk is about retaining availability of OpenStack cluster in case of failure of one of its underlying components.

We do not consider:

1. Simultaneous failure of several components
2. Force majeure and natural disasters
3. Physical destruction of the hardware
