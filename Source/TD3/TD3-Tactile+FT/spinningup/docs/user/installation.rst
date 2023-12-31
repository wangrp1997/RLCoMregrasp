,<m==,==5===�I~rtcmlk~�o.
�=-/=<==5x*..!cfTe�ts�z �ablE mg�GiFten4q
_pInnikG U0$�e1U)rm3 P�\lon3 Gpg>AY G9�,(and O��nGPI.�J�S8&ningUT i� gurzenTdi$nNh] wu0|ortel��n5Li.wx E�$ O[X. It o`yba`po3�ibl}$�o(i��daD| gvbWine�{s<!�h/�fl2ph��(iccn&�0�men$exdE~skveLx tactdtn #�["
j&&"cd)Iji4coj;: �u Shul�Klo�
`` 4L`n�8Exq��le[ hn�0feNchmazjr"hnds`�nnin�Tt�f�zER wo��N�%n�kvo.me~tS#That u3q%|hE0`MeB/o(N�`iYqiB� enfing�(MuKk -S(c Pwopb�m�ary softwa�%��h`| �q�itec` n)gmN�u�5hish(ic f2%e unluria|a/l0dree`For bT5$e�0S�$bw4 mtjer7mSmms!>kt(frgw. as i rhzuk4,8hjc4ao|INe�i4�M�(tig|u�- fut`b�c5usd O&pmwwk�0+rv�*setm dhe!pes$�rCh!�om�n+tL/8iv i7dt.e�tg&Acv��S|ag&Cze"f�r�benaimar�ine'f%EtKD aLggrk5lLs(o� wonthnuoes cK`Tp,|-o,it$kB pqe�dpree.!
` � D�,&u 5/Rp9`h&!��w!ddci$e ~��0dO i|ttAl� �uFmCo, 4h)9fl� Y?u ca."TUfHNypgdi�g%t Zte2vd4"lndRM0b� runnem�!�@ ah�mritmlz8On tX%(`CjAshc Cmot�ola� in�pbox&`bO$Envirblents�i���m, wh)cd Qrt(dkdal|!&ree r!}seZ��,)[#\(I70dkkq�|}yg` t%�g1su0o.l xEps/n lC#&figured out `a workaround for running on Windows`_. If you try another way and succeed, please let us know how you did it!

.. _`Classic Control`: https://gym.openai.com/envs/#classic_control
.. _`Box2d`: https://gym.openai.com/envs/#box2d
.. _`MuJoCo`: http://www.mujoco.org/index.html
.. _`a workaround for running on Windows`: https://github.com/openai/spinningup/issues/23

Installing Python
=================

We recommend installing Python through Anaconda. Anaconda is a library that includes Python and many useful packages for Python, as well as an environment manager called conda that makes package management simple.

Follow `the installation instructions`_ for Anaconda here. Download and install Anaconda3 (at time of writing, `Anaconda3-5.3.0`_). Then create a conda Python 3.6 env for organizing packages used in Spinning Up:

.. parsed-literal::

    conda create -n spinningup python=3.6

To use Python from the environment you just created, activate the environment with:

.. parsed-literal::

    conda activate spinningup

.. admonition:: You Should Know

    If you're new to python environments and package management, this stuff can quickly get confusing or overwhelming, and you'll probably hit some snags along the way. (Especially, you should expect problems like, "I just installed this thing, but it says it's not found when I try to use it!") You may want to read through some clean explanations about what package management is, why it's a good idea, and what commands you'll typically have to execute to correctly use it. 

    `FreeCodeCamp`_ has a good explanation worth reading. There's a shorter description on `Towards Data Science`_ which is also helpful and informative. Finally, if you're an extremely patient person, you may want to read the (dry, but very informative) `documentation page from Conda`_.

.. _`the installation instructions`: https://docs.continuum.io/anaconda/install/
.. _`Anaconda3-5.3.0`: https://repo.anaconda.com/archive/
.. _`FreeCodeCamp`: https://medium.freecodecamp.org/why-you-need-python-environments-and-how-to-manage-them-with-conda-85f155f4353c
.. _`Towards Data Science`: https://towardsdatascience.com/environment-management-with-conda-python-2-3-b9961a8a5097
.. _`documentation page from Conda`: https://conda.io/docs/user-guide/tasks/manage-environments.html
.. _`this Github issue for Tensorflow`: https://github.com/tensorflow/tensorflow/issues/20444


Installing OpenMPI
==================

Ubuntu 
------

.. parsed-literal::

    sudo apt-get update && sudo apt-get install libopenmpi-dev


Mac OS X
--------
Installation of system packages on Mac requires Homebrew_. With Homebrew installed, run the follwing:

.. parsed-literal::

    brew install openmpi

.. _Homebrew: https://brew.sh

Installing Spinning Up
======================

.. parsed-literal::

    git clone https://github.com/openai/spinningup.git
    cd spinningup
    pip install -e .

.. admonition:: You Should Know

    Spinning Up defaults to installing everything in Gym **except** the MuJoCo environments. In case you run into any trouble with the Gym installation, check out the `Gym`_ github page for help. If you want the MuJoCo environments, see the optional installation section below.

.. _`Gym`: https://github.com/openai/gym

Check Your Install
==================

To see if you've successfully installed Spinning Up, try running PPO in the LunarLander-v2 environment with

.. parsed-literal::

    python -m spinup.run ppo --hid "[32,32]" --env LunarLander-v2 --exp_name installtest --gamma 0.999

This might run for around 10 minutes, and you can leave it going in the background while you continue reading through documentation. This won't train the agent to completion, but will run it for long enough that you can see *some* learning progress when the results come in.

After it finishes training, watch a video of the trained policy with

.. parsed-literal::

    python -m spinup.run test_policy data/installtest/installtest_s0

And plot the results with

.. parsed-literal::

    python -m spinup.run plot data/installtest/installtest_s0


Installing MuJoCo (Optional)
============================

First, go to the `mujoco-py`_ github page. Follow the installation instructions in the README, which describe how to install the MuJoCo physics engine and the mujoco-py package (which allows the use of MuJoCo from Python). 

.. admonition:: You Should Know

    In order to use the MuJoCo simulator, you will need to get a `MuJoCo license`_. Free 30-day licenses are available to anyone, and free 1-year licenses are available to full-time students.

Once you have installed MuJoCo, install the corresponding Gym environments with

.. parsed-literal::

    pip install gym[mujoco,robotics]

And then check that things are working by running PPO in the Walker2d-v2 environment with

.. parsed-literal::

    python -m spinup.run ppo --hid "[32,32]" --env Walker2d-v2 --exp_name mujocotest


.. _`mujoco-py`: https://github.com/openai/mujoco-py
.. _`MuJoCo license`: https://www.roboti.us/license.html
