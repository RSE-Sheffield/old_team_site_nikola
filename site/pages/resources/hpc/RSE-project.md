.. title: Premium access to ShARC
.. slug: premium-hpc
.. date: 2017-05-05 06:15:18 UTC
.. tags:
.. category:
.. link:
.. description:
.. type: text

The Sheffield RSE group, along with a few other research groups around the University, have purchased some additional hardware and integrated it into ShARC. 
Only those who have contributed to this hardware can access it.
This is part of an experiment to try to make better use of computational resources at Sheffield and also an attempt to build more of a community around the HPC systems here.

Sheffield RSE is an academically-led group. We, and our collaborators, had computational needs that weren't addressed by Sheffield's HPC systems. Rather than build our own, separate system, we teamed up with the guys at CiCS to make Sheffield's HPC better for everyone.

# How does it work?

Research groups give us money to buy additional hardware and we purchase and integrate it via the Research Computing Group at CiCS.  
This will take time but we'll give you access to the current pool immediately.
We've seeded the process with ~£100,000 and already have nodes that provide capability not available elsewhere on ShARC or Iceberg.

# What's the advantage of doing it this way?

We find that the workloads of most research groups is 'spiky'. That is, much of the time you might not need any compute at all but when you *do* need it, you need a **lot** -- a lot more than you can afford to actually buy! By pooling resources in this way, it's more likely that you'll have the resource you need when you need it.

A similar model is in use at [The University of Manchester](http://ri.itservices.manchester.ac.uk/csf/) and has helped them build a system much bigger than the one at Sheffield. 
We believe that part of their success is due to the fact that academics and central IT are *co-investing* in a system rather than IT simply providing a service.
This is our attempt to replicate the same success here at Sheffield.

Traditonally, HPC at Sheffied has always been 'Free At Point of Use' and although this allows everyone to have *some* HPC access for 'free' (It really comes out of overheads), it usually leaves most people wanting more. You'll know what we mean if you've ever waited in the queues for longer than it takes your job to run! 

Free at Point of Use is great and we don't expect this to change but we want to offer our collaborators something more.

# Can you guarantee me access to *my* hardware exactly when I need it?

No (with caveats). The idea of this particular model is that you give up the convenience of guaranteed access at all times in exchange for access to much more hardware than you could otherwise afford. We can occasonally give our collaborators guaranteed access to certain nodes for short periods for mission critical work but we strongly discourage it. If you really must have guaranteed access to your nodes at all times, this may not be the model for you.

# What if I join the pool but find it doesn't work for me?

We'll generally recommend that people contribute to the pool such that they buy at least an entire node. 
That way, if it turns out that our model really doesn't work for you, we can remove you from the pool and put your hardware within its own project -- just as if you had purchased a node directly from CiCS.

# Once I join the pool. How do I access the hardware?

Add the line 

```
#$ -P rse
```

to your job submission script or start a `qrshx` session with `qrshx -P rse`

# How do you control access quotas

We currently use the standard fair-use policy operated by Sun Grid Engine.  In short, your priority goes down the more you use the system.
This is identical to the policy used on the rest of ShARC and is currently working fine for our users. 
We will, however, collaborate with the user community in modifying this policy if it proves to be necessary.

# What Hardware is currently available in the premium pool?

The pool consists of the following nodes. Of course, this is in addition to all of the publically accessible nodes available on ShARC.

## NVIDIA DGX-1

The NVIDIA DGX-1 is a specialised node that has been configured and optimised for deep learning workloads using GPUs.

* 8x Tesla GPU100 GPUs with 16GB RAM and 3840 CUDA Cores per GPU
* 2x 20 core Intel Xeon E5-2698 v4 2.2GHz
* 512GB RAM

Full details concerning its specification can be found at [https://images.nvidia.com/content/techn
ologies/deep-learning/pdf/61681-DB2-Launch-Datasheet-Deep-Learning-Letter-WEB.pdf](https://images.
nvidia.com/content/technologies/deep-learning/pdf/61681-DB2-Launch-Datasheet-Deep-Learning-Letter-
WEB.pdf)

## Big memory nodes

These nodes have the same number of CPUs as standard ShARC nodes but they have much more memory. This makes them ideal for big-data workloads. 

* 3 nodes
* Each node has 16 CPU cores 
* Each node has 768GB RAM

## Dense core nodes (coming soon)

These nodes have twice the number of CPUs as standard ShARC nodes with the same amount of RAM per core. 
This makes them ideal for shared-memory parallel workloads that can scale beyond what is currently available on ShARC

We are in the process of buying these right now so they are not available just yet. We currently propose to buy

* 3 nodes
* Each node has 32 CPU cores
* Each node has 128GB RAM

# What are the alternatives to this premium queue?

If you have money to spend on HPC equipment and don't want to be part of our experiment, here are some alternatives.

* **Buy your own hardware and shove it under your desk. Admin it yourself and have fun.**  This has a lot of advantages. You can see and touch your tin. You can do whatever you like with it and have access to it whenever you want. The disadvantages are that you'll not be integrated with the rest of our HPC system, you won't have access to all of our additional hardware and you won't have access to our support community.

* **Buy you own nodes on ShARC directly from CiCS**. The main advantage of this is that you'll have guarenteed access to your nodes whenever you want them. Possibly the best route for people who are going to have long, sustained workloads and who know exactly how much compute they need. The disadvantages are that you'll only have access to 'your nodes + general pool'. With us you'll have 'your nodes + general pool + our nodes'. Another disadvantage is that when you are not using your nodes, they'll be lying idle and will effectively be a waste of money

* **Buy a lease on nodes from CiCS**. CiCS let you lease nodes by the hour but you usually have to buy several months worth to make this worthwhile. You get guarenteed access to your leased nodes which is great but when your lease runs out you are left with nothing. The hourly rate is much cheaper than cloud computing but, unlike the cloud, you have to pay for your nodes even when you are not using them. We've found that some research groups who lease a node for a year only end up using it about ~10% of the time. This is such a huge waste of money that they may as well have used the cloud instead.

* **Buy time on the cloud**. The most flexible option available. It's currently more expensive than buying on-premise hardware and requires a different set of skills to use. We do a lot of cloud based work though and will be happy to talk to you about options.


