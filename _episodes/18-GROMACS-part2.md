---
title: "PRACTICAL: Benchmarking Molecular Dynamics Using GROMACS"
teaching: 10
exercises: 20
questions:
- "To be determined"
objectives:
- ""
- ""
keypoints:
- "To be determined"
---

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

module load epcc-job-env
module load xthi/1.0

export OMP_PLACES=cores
export OMP_NUM_THREADS=16

srun xthi
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

> ## Two hardware threads per core
>
> The `--hint=nomultithread` asks SLURM to ignore the possibility of running
> two threads per core. If we remove this option, this makes available  256
> "cpus" per node (2 threads per core in hardware). Can you write a script
> to run 8 MPI tasks with 1 task per NUMA region running 32 OpenMP threads?
> Note: physical cores appear as affinity 0-127, while the extra "logical"
> cores are numbered 128-255. Logical cores 0 and 128 occupy the same physical
> core etc.
>
>> ## Solution
>> ```
>>
>> #!/usr/bin/env bash
>>
>> #SBATCH --partition=standard
>> #SBATCH --time=00:20:00
>>
>> #SBATCH --nodes=2
>> #SBATCH --ntasks-per-node=8
>>
>> #SBATCH --hint=multithread
>> #SBATCH --distribution=block:cyclic
>>
>> #SBATCH --cpus-per-task=32
>>
>> module load epcc-job-env
>> module load xthi/1.0
>>
>> export OMP_PLACES=cores
>> export OMP_NUM_THREADS=32
>>
>> srun xthi
>> ```
> {: .solution}
{: .challenge}
