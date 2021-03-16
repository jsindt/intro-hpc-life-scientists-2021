---
title: "PRACTICAL: Benchmarking Molecular Dynamics Using GROMACS 3"
teaching: 10
exercises: 20
questions:
- "What is multithreading?"
- "How does load balancing affect GROMACS performance?"
objectives:
- "Understand what hyperthreading is, and how to use it on ARCHER2."
- "Learn how to disable GROMACS dynamic load-balancing."
keypoints:
- ""
---

## Two hardware threads per core

The `--hint=nomultithread` asks SLURM to ignore the possibility of running
two threads per core. If we remove this option, this makes available  256
"cpus" per node (2 threads per core in hardware). To run 8 MPI tasks with
1 task per NUMA region running 32 OpenMP threads, the script would look like:

```
#!/usr/bin/env bash

#SBATCH --partition=standard
#SBATCH --time=00:10:00

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8

#SBATCH --hint=multithread
#SBATCH --distribution=block:cyclic

#SBATCH --cpus-per-task=32

module load epcc-job-env
module load xthi/1.0

export OMP_PLACES=cores
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK

srun gmx_mpi mdrun -ntomp $SLURM_CPUS_PER_TASK -s benchMEM.tpr
```
Note: physical cores appear as affinity 0-127, while the extra "logical"
cores are numbered 128-255. Logical cores 0 and 128 occupy the same physical
core etc.

> ## Multithreading and GROMACS?
>
> Staring with the MPI-only case first, how does enabling multithreading
> affect GROMACS performance?
>
> What about the performance of hybrid MPI+OpenMP jobs?
{: .challenge}

## Load balancing

GROMACS performs dynamic load balancing when it deems necessary. Can you tell
from your md.log files so far whether it has been doing so, and what it
calculated the load imbalance was before deciding to do so?

To demonstrate the effect of the load imbalance counteracted by GROMACS’s
dynamic load balancing scheme, investigate what happens when this is turned
off by including the `-dlb no` option to `gmx_mpi mdrun`.

{% include links.md %}
