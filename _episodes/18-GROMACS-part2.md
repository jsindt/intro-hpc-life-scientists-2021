---
title: "PRACTICAL: Benchmarking Molecular Dynamics Using GROMACS"
teaching: 10
exercises: 20
questions:
- "How do we run hybrid MPI and OpenMP jobs on ARCHER2?"
- "Does adding OpenMP to MPI GROMACS affect performance?"
objectives:
- "Run a hybrid MPI with OpenMP simuation on ARCHER2."
- "See how GROMACS performance is changed by including OpenMP."
keypoints:
- "To be determined"
---

## Aims

GROMACS can  run  in  parallel using  MPI  and  simultaneously  also  using
OpenMP  threads (see [here](http://manual.gromacs.org/current/user-guide/mdrun-performance.html#multi-level-parallelization-mpi-and-openmp)).
In this tutorial, you will learn how to run hybrid MPI and OpenMP jobs on
ARCHER2, and you will benchmark the performance of the `bechMEM` system to see
whether performance improves when using OpenMP threads.

## Hybrid MPI and OpenMP jobs on ARCHER2

When running hybrid MPI (with the individual tasks also known as ranks or
processes) and OpenMP (with multiple threads) jobs you need to leave free
cores between the parallel tasks launched using `srun` for the multiple
OpenMP threads that will be associated with each MPI task.

As we saw above, you can use the options to `sbatch` to control how many
parallel tasks are placed on each compute node and can use the
`--cpus-per-task` option to set the stride between parallel tasks
to the right value to accommodate the OpenMP threads. The value
of `--cpus-per-task` should usually be the same as that for
`OMP_NUM_THREADS`.

As an example, consider the job script below that runs across 2 nodes with
8 MPI tasks per node and 16 OpenMP threads per MPI task (so all 256 cores
are used).
Here we use the standard OpenMP control setting `OMP_PLACES=cores`
to specify that placement should be on the basis of cores.

```
#!/bin/bash

#SBATCH --partition=standard
#SBATCH --qos=standard
#SBATCH --time=00:10:00

#SBATCH --nodes=2
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=16

#SBATCH --hint=nomultithread
#SBATCH --distribution=block:cyclic

module restore /etc/cray-pe.d/PrgEnv-gnu
module load gromacs

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

srun gmx_mpi mdrun -ntomp $SLURM_CPUS_PER_TASK -s benchMEM.tpr
```
{: .language-bash}

Each ARCHER2 compute node is made up of 8 NUMA (*Non Uniform Memory Access*) regions (4 per socket)
with 16 cores in each region. Programs where the threads span multiple NUMA regions
are likely to be *less* efficient so we recommend using thread counts that fit well into the
ARCHER2 compute node layout. Effectively, this means one of the following options for nodes
where all cores are used:

* 8 MPI tasks per node and 16 OpenMP threads per task: equivalent to 1 MPI task per NUMA region
* 16 MPI tasks per node and 8 OpenMP threads per task: equivalent to 2 MPI tasks per NUMA region
* 32 MPI tasks per node and 4 OpenMP threads per task: equivalent to 4 MPI tasks per NUMA region
* 64 MPI tasks per node and 2 OpenMP threads per task: equivalent to 8 MPI tasks per NUMA region

## Instructions

For this tutorial, you will start by comparing the performance of a simulation
that uses all of the cores on a node. Using the code above as a template, try
running simulations that use varying levels of MPI and OpenMP (making sure
that the number of MPI ranks and OpenMP threads always multiply to 128 or
less). How do the simulation times change as you increase the numbers change?
How do these times change if you do not spread the threads over the NUMA
regions as suggested?

You may find it helpful to fill out this table

| MPI Ranks | OpenMP threads | Walltime (s) | performance (ns/day) |
|-----------|----------------|--------------|----------------------|
|       128 |              1 | | |
|        64 |              2 | | |
|        42 |              3 | | |
|        32 |              4 | | |
|        25 |              5 | | |
|        16 |              8 | | |
|         8 |             16 | | |



{% include links.md %}
