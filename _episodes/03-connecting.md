---
title: "PRACTICAL: Connecting to ARCHER2 and transferring data"
teaching: 10
exercises: 10
questions:
- "How can I access ARCHER2 interactively and transfer data?"
objectives:
- "Understand how to connect to ARCHER2."
- "Know how to transfer data onto and off of ARCHER2 efficiently."
keypoints:
- "ARCHER2's login address is `login.archer2.ac.uk`."
- "The password policy for ARCHER2 is well documented."
- "There are a number of ways to transfer data to/from ARCHER2."
---

## Connecting using SSH

The ARCHER2 login address is

```
login.archer2.ac.uk
```
{: .language-bash}

Access to ARCHER2 is via SSH using **both** a password and a passphrase-protected SSH key pair.

## Passwords and password policy

When you first get an ARCHER2 account, you will get a single-use password from the
SAFE which you will be asked to change to a password of your choice. Your chosen
password must have the required complexity as specified in the
[ARCHER2 Password Policy](https://www.archer2.ac.uk/about/policies/passwords_usernames.html).

The password policy has been chosen to allow users to use both complex, shorter passwords and
long, but comparatively simple passwords. For example, passwords in the style of both
`LA10!£lsty` and `horsebatterystaple` would be supported.

> ## Picking a good password
> Which of these passwords would be a good, valid choice according to the ARCHER2 Password
> Policy?
>
> 1. `mypassword`
> 2. `rainbowllamajumping`
> 3. `A!94ufskl$?`
> 4. `horsebatterystaple`
>
> > ## Solution
> >
> > 1. **No** This would not be accepted or a good choice as it is too short and is made up of obvious words
> > 2. **Yes** This would be a good choice as it is long enough and easy to remember
> > 3. **Yes** This would be accepted but may be difficult to remember and type (though you could use a password manager to store it)
> > 4. **No** While this meets the criteria, it is a well known example from a popular web comic and so would not be accepted
> >
> {: .solution}
{: .challenge}

## SSH keys

As well as password access, users are required to add the public part of an SSH key pair to access ARCHER2.
The public part of the key pair is associated with your account using the SAFE web interface.
See the ARCHER2 User and Best Practice Guide for information on how to create SSH key pairs
and associate them with your account:

* [Connecting to ARCHER2](https://docs.archer2.ac.uk/user-guide/connecting.html)

## Data transfer services: scp, rsync, Globus Online

ARCHER2 supports several data transfer mechanisms. The one you choose depends
on the amount and structure of the data you want to transfer and where you want to transfer
the data to. The three main options are:

* `scp`: The standard way to transfer small to medium amounts of data (in the order of MBs or GBs) off ARCHER2 to any other location
* `rsync`: Used if you need to keep small to medium datasets (in the order of MBs or GBs) synchronised between two different locations
* *Globus Online*: Used to transfer large amounts of data (in the order of TBs) to other sites which are Globus Online enabled

More information on data transfer mechanisms can be found in the ARCHER2 User and Best Practice Guide:

* [Data management and transfer](https://docs.archer2.ac.uk/user-guide/data.html)

{% include links.md %}

