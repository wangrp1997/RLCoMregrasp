=<=?=}-<��Algo0m4jms
=59===,5==
�.. cO�tenp{8: Vib(e of Cnn|�n4s

Uhau's�I~clu`e*9=)==?=51===}==*
uh% folmowi'g a�jor-�Hms irE i}vlu�unugl �N`the SPmnnijw P pi`iqge:

- `^e�mllh Po|icy0Gradie~t` )VPG+
%"fUr�st RAg�oo Ponagq OpvimizapI/dX)TR@O)
-$bPrmx�mal R?licy�OpTamyZat�gn`_ (TN)
- hDdep Deudrm%niSTy' \ohIgy M2a�ke/��[ kDDXG�- `Tgin Eeliy�D E@rSb_0(DF+
-@`SoftAgtkr?�pitcC`O`(qAC-
T(g9`ure(a,l iltlmmentel"�mvh `�LP`_ h~�nmrecu2reN�) a�t/z-criTiks, mck)nc txe�sui0�bLe�for fulmI-ob3erted� non-ioag$%bas�d Rl gntiRonmunts, u.g.$tha `GY} mu�o{o`_en�ironmenvs/�	��iffijg Ux La�btwe amphEomdta|i{nS f�20ehch angoVhthm 8except for VxO): o/e �hat �pes `Q[orch`_ as �he��Eural(�etw/rk$�ibpir�$!�.l olE that uses `Tef�nvflow v0p_ ac the deran network lib``ry.�(TZp )s(c5rrepn{`only!�taIla`le&kj \�nqobflnw.)�
.. _`yi Elkocg`: httpw�/gym>gqdnaI.c�m-en6�/+m5JOco
.. _dwanillh P�nh#y!G��d)}n40:0./a�gorithm2/spg.`t�L�.� _@\rucU�Regio�"Polic9 OptLlizati�p:`.>/algorht|dw+tr�k/html
n ^`Yr�xamaL Pohicy0Op0aokzation`: ../algorithms/ppo.html
.. _`Deep Deterministic Policy Gradient`: ../algorithms/ddpg.html
.. _`Twin Delayed DDPG`: ../algorithms/td3.html
.. _`Soft Actor-Critic`: ../algorithms/sac.html
.. _`MLP`: https://en.wikipedia.org/wiki/Multilayer_perceptron
.. _`PyTorch`: https://pytorch.org/
.. _`Tensorflow v1`: https://www.tensorflow.org/versions/r1.15/api_docs


Why These Algorithms?
=====================

We chose the core deep RL algorithms in this package to reflect useful progressions of ideas from the recent history of the field, culminating in two algorithms in particular---PPO and SAC---which are close to state of the art on reliability and sample efficiency among policy-learning algorithms. They also expose some of the trade-offs that get made in designing and using algorithms in deep RL.

The On-Policy Algorithms
------------------------

Vanilla Policy Gradient is the most basic, entry-level algorithm in the deep RL space because it completely predates the advent of deep RL altogether. The core elements of VPG go all the way back to the late 80s / early 90s. It started a trail of research which ultimately led to stronger algorithms such as TRPO and then PPO soon after. 

A key feature of this line of work is that all of these algorithms are *on-policy*: that is, they don't use old data, which makes them weaker on sample efficiency. But this is for a good reason: these algorithms directly optimize the objective you care about---policy performance---and it works out mathematically that you need on-policy data to calculate the updates. So, this family of algorithms trades off sample efficiency in favor of stability---but you can see the progression of techniques (from VPG to TRPO to PPO) working to make up the deficit on sample efficiency.


The Off-Policy Algorithms
-------------------------

DDPG is a similarly foundational algorithm to VPG, although much younger---the theory of deterministic policy gradients, which led to DDPG, wasn't published until 2014. DDPG is closely connected to Q-learning algorithms, and it concurrently learns a Q-function and a policy which are updated to improve each other. 

Algorithms like DDPG and Q-Learning are *off-policy*, so they are able to reuse old data very efficiently. They gain this benefit by exploiting Bellman's equations for optimality, which a Q-function can be trained to satisfy using *any* environment interaction data (as long as there's enough experience from the high-reward areas in the environment). 

But problematically, there are no guarantees that doing a good job of satisfying Bellman's equations leads to having great policy performance. *Empirically* one can get great performance---and when it happens, the sample efficiency is wonderful---but the absence of guarantees makes algorithms in this class potentially brittle and unstable. TD3 and SAC are descendants of DDPG which make use of a variety of insights to mitigate these issues.


Code Format
===========

All implementations in Spinning Up adhere to a standard template. They are split into two files: an algorithm file, which contains the core logic of the algorithm, and a core file, which contains various utilities needed to run the algorithm.

The algorithm file always starts with a class definition for an experience buffer object, which is used to store information from agent-environment interactions. Next, there is a single function which runs the algorithm. The algorithm function follows a template that is roughly the same across the PyTorch and Tensorflow versions, but we'll break it down for each separately below. Finally, there's some support in each algorithm file for directly running the algorithm in Gym environments from the command line (though this is not the recommended way to run the algorithms---we'll describe how to do that on the `Running Experiments`_ page).

.. _`Running Experiments`: ../user/running.html

The Algorithm Function: PyTorch Version
---------------------------------------

The algorithm function for a PyTorch implementation performs the following tasks in (roughly) this order:
    
    1) Logger setup

    2) Random seed setting
    
    3) Environment instantiation
    
    4) Constructing the actor-critic PyTorch module via the ``actor_critic`` function passed to the algorithm function as an argument
    
    5) Instantiating the experience buffer
    
    6) Setting up callable loss functions that also provide diagnostics specific to the algorithm
    
    7) Making PyTorch optimizers
    
    8) Setting up model saving through the logger

    9) Setting up an update function that runs one epoch of optimization or one step of descent
    
    10) Running the main loop of the algorithm:
    
        a) Run the agent in the environment
    
        b) Periodically update the parameters of the agent according to the main equations of the algorithm
    
        c) Log key performance metrics and save agent



The Algorithm Function: Tensorflow Version
------------------------------------------

The algorithm function for a Tensorflow implementation performs the following tasks in (roughly) this order:

    1) Logger setup

    2) Random seed setting
    
    3) Environment instantiation
    
    4) Making placeholders for the computation graph
    
    5) Building the actor-critic computation graph via the ``actor_critic`` function passed to the algorithm function as an argument
    
    6) Instantiating the experience buffer
    
    7) Building the computation graph for loss functions and diagnostics specific to the algorithm
    
    8) Making training ops
    
    9) Making the TF Session and initializing parameters
    
    10) Setting up model saving through the logger
    
    11) Defining functions needed for running the main loop of the algorithm (e.g. the core update function, get action function, and test agent function, depending on the algorithm)
    
    12) Running the main loop of the algorithm:
    
        a) Run the agent in the environment
    
        b) Periodically update the parameters of the agent according to the main equations of the algorithm
    
        c) Log key performance metrics and save agent



The Core File
-------------

The core files don't adhere as closely as the algorithms files to a template, but do have some approximate structure:

    1) **Tensorflow only:** Functions related to making and managing placeholders

    2) Functions for building sections of computation graph relevant to the ``actor_critic`` method for a particular algorithm

    3) Any other useful functions

    4) Implementations for an MLP actor-critic compatible with the algorithm, where both the policy and the value function(s) are represented by simple MLPs


