---
title: "PRACTICAL: Benchmarking Molecular Dynamics Performance Using GROMACS"
teaching: 10
exercises: 20
questions:
- "To be determined"
objectives:
- "Gain a basic understanding of key aspects of running molecular dynamics
simulations in parallel."
keypoints:
- "To be determined"
---

## Aims

In this exercise, you will be running molecular dynamics simulations using
[GROMACS](https://manual.gromacs.org/). You will be benchmarking how
efficiently a small system will run as you increase the number of processors.

## Instructions

You  will be running a benchmark that has been used to explore the
performance/price behaviour  of  GROMACS on various  generations of CPUs and
GPUs. A number of publications have used this benchmark to report on, and
compare, the performance of GROMACS on different systems (*e.g* see:
[https://doi.org/10.1002/jcc.24030](https://doi.org/10.1002/jcc.24030)
).

The  benchmark system in question is “benchMEM”, which is available from the
list of Standard MD benchmarks at https://www.mpibpc.mpg.de/grubmueller/bench. This benchmark simulates a membrane channel protein embedded in a lipid bilayer surrounded by water and ions. With its size of ~80 000 atoms, it serves as a prototypicalexample for a large class of setups used to study all kinds of membrane-embedded proteins.For some more information see [here](https://www.mpibpc.mpg.de/16460085/bench.pdf).
