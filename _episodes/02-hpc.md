---
title: "LECTURE: High-Performance Computing (HPC)"
teaching: 15
exercises: 5
questions:
- "What is high-performance computing?"
- "What is a high-performance computer?"
- "Why are computer clusters used?"
objectives:
- "Learning about computer clusters and how they are made"
- "Understanding what differentiates a HPC system from a cluster"
keypoints:
- "Computer clusters enable users to run larger, more complex jobs in a reasonable time"
- "HPC systems are state-of-the-art clusters composed of a large number of powerful computers linked by very fast interconnects."
---

## Defining high-performance computing

The simplest way of defining hugh-performance computing is by saying that it
is the using of high-performance computers. However, this leads to our next
question.

> ## What is high-performance computer?
> On the Course Etherpad, write down what you think defines a high-performance
> computer.
> > ## Solution
> >
> > A high-performance computer is a network of computers in a cluster that
> > typically share a common purpose and are used to accomplish tasks that
> > might otherwise be too big for any one computer.
> >
> {: .solution}
{: .challenge}

## Generic parallel machines and computer clusters

Consider the computer you are using to see this -- it will have:

* a (probably multi-core) processor
* a hard drive
* memory
* an operating system (hopefully)
* ...

While modern computers can do a lot (and a lot more than their equivalents
10-20 years ago), there are limits to what they can do and the speed at which
they are able to do this. One way to overcome these limits is to pool computers
together to create a cluster of computers. These pooled resources can then be
used to run software that requires more total memory, or need more processors
to complete in a reasonable time.

One way to do this is to take a group of computers and link them together
*via* a network switch. Consider a case where you have five 4-core computers.
By connecting them together, you could run jobs on 20 cores, which could
result in your software running faster.

{% include figure.html url="" max-width="80%"
file="/fig/02-hpc/Camp_cluster.jpg" alt="Beowulf-style cluster"
caption="Beowulf-style cluster" %}

However, while this can increase the speed at which your codes run, or allow
you to run larger systems, this is not high-performance yet...

## HPC architectures

Most HPC systems follow the ideas described above of taking many computers and
linking them *via* network switches. What distinguishes a high-performance
computer from the computer clusters described above is:

* The number of computers/nodes -- HPC systems have 100s to 10,000s of nodes. At
present ARCHER2 has 1024 nodes, and will grow to 5,585 nodes.
* The strength of each individual computer/nodes -- HPC systems are usually
build state-of-the-art computers. Each ARCHER2 node has two EPYC Zen2 (Rome)
7742, 2.25 GHz, 64-core processors, for a total of 128 cores each. Further,
each node had 256 GB of RAM. One downside to this criterion is that, as
computing technology evolves very quickly, HPC facilities do not remain
"high-performance" for long, and need to be updated or changed every 5-10
years.
* The network interconnect -- this dictates the communication speed between
nodes. The faster this speed is, the more a group of individual nodes will act
like a unit. For ARCHER2, this inter-node communication speed is 25 GB/s!

### ARCHER2 architecture

The ARCHER2 Cray Shasta system consists of a number of different node types.
The ones visible to users are:

* Login nodes
* Compute nodes
* Data analysis (pre-/post- processing) nodes

{% include figure.html url="" max-width="80%"
file="/fig/02-hpc/archer2_architecture.png" alt="ARCHER2 architecture diagram" caption="ARCHER2 architecture" %}

## Who uses HPC?

HPC is used more and more frequently by people from a growing number of
fields. Traditional uses of HPC include:

* mateerials science/solid state physics
* computational chemistry
* biomolecular simulations
* particle physics
* environmental modelling
  * weather and climate
  * geosciences
  * oceanography
* many engineering applications

## HPC and me

HPC platforms try to cater to a broad range of computation needs, and have
co-evolved with their user communities. HPC systems are traditionally built to
achieve strong floating-point performance (number crunching), and are there to
solve very large and complex scientific problems quickly (though they can also
be used to run a large number of smaller, simpler systems in batches as well).

HPC platforms have evolved to fit the problem sets they are most used for,
and have become more optimised towards certain problems. As HPC platforms have
evolved, so has scientific software evolved to get the most out of modern HPC
facilities. One example is molecular dynamics software that is nowadays heavily
optimised for use on HPC systems.

Recently, some HPC facilities are specifically optimised for certain problem
categories, and can help users to solve very specific problems very quickly.
For example, NextGenIO is built for fast read-write speed, has large amounts
of shared memory, and as a result is very useful for DNA assembly in
bioinformatics.

{% include links.md %}
