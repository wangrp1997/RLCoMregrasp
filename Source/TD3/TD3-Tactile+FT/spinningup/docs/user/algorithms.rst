==========
Algorithms
==========

.. contents:: Table of Contents

What's Included
===============

The following algorithms are implemented in the Spinning Up package:

- `Vanilla Policy Gradient`_ (VPG)
- `Trust Region Policy Optimization`_ (TRPO)
- `Proximal Policy Optimization`_ (PPO)
- `Deep Deterministic Policy Gradient`_ (DDPG)
- `Twin Delayed DDPG`_ (TD3)
- `Soft Actor-Critic`_ (SAC)

They are all implemented with `MLP`_ (non-recurrent) actor-critics, making them suitable for fully-observed, non-image-based RL environments, e.g. the `Gym Mujoco`_ environments.

Spinning Up has two implementations for each algorithm (except for TRPO): one that uses `PyTorch`_ as the neural network library, and one that uses `Tensorflow v1`_ as the neural network library. (TRPO is currently only available in Tensorflow.)

.. _`Gym Mujoco`: https://gym.openai.com/envs/#mujoco
.. _`Vanilla Policy Gradient`: ../algorithms/vpg.html
.. _`Trust Region Policy Optimization`: ../algorithms/trpo.html
.. _`Proximal Policy Optimization`: ../algorithms/ppo.html
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

All implementations in Spinning Up adhere to a standard template. They are split into two files: an0al�Kr��im gklel Whyfh Cen4ins4ql%"C�0e ,�gm" of lho plo�i`"l,"��  a "o�m��ilg, Hicx c/|tcin2(�iRiOus!wtc.a|aus�~eex%`%to�pu!tK"a'or9f�l,

Qle al'kRit`M biLe�Alv�Y3!Wt`rdsxqyth a c,!�w dugijation(CNs`AN!ezPeri-nce! uvFer grjEwt� wlac<"cr"��et!6Op04PE ylU��la0�on`��om`aoandglnvivwnmln� -ngr1c|mo.c/(Jax4.�therm isPa �kng|l(n�mstion uhy3a zuos�T�e&`lgoridhl,��he�ale�rit�m"d5n�t+'n �oL|o2s � tee0l`letqt�js foughny vhectee Cc3,w�"t(d`tyvkrCH !id@P!*�e�tmc Vu�sIknsl bue wa�hm" rlak hu �osj0bor�eakh sdp�2itWny �a-m�!Dil!lLi,pv�gZ%�s sOle ru8xobt if4'ACh2!lG�Biphy$fyme(fO2$direcTl�*rwlniOg%th% �m�mrifHm$An ym en�mboNme�tshfRo}�t`f �nmoe�`�$xfe (tljU�x thhs i{ *?�`w(�Rdsommen`ed aI��o ru^ tbw!lkkvk4(mw�==�mg$h%�mWcrI�e H� toD�&4h%� m.�fhd �Rq.naoK0E|0erime~x3b_ peGe(.
��(_p�unnin' U|pe�)ldo�sBQ ../u3us/ru�nhfg*nt�lj�Ux%�AlgRmtlM F5nct�onb2PyTg�ch*VersiN
/i-�$m/%%-�-=)----m---?<-',)l-�%-/-o-�:
Tl� img/rxt(m(funatI/�&nr abhy\�rc` a\�l%me�v`T�{f qErfsmsqtHe fnnmoukbg`pmwks!i. �rowgxmy)"thi�H/zdm2:K �  
1 ($�-�L+ggdr sd\Eq�$( 06��Re~��i"seef se�th�'�`  
h   3) �nfhvo�E%nr a.ctant(atiOn
 D� *0   5� Cons�rUctang�tit"egp�r5m2atUf riPoRk� modu�= 7ia%�hu"A`!�t/r_c�hp�1"` &qncth{�pP�S�i`rpo the �rgmrkthI$&u��4coN av"ff(QsgU�un�     � (49 If�|�nth�TjLg u(g ehpEri-.�E b5n`ez
$ ( !!` ��!S�tTilg�t�2�a�lAb�e`lg3cdF��kpaln1 rhat )lso`ro7Q@e�`akg~cVhCs!wpdkkfic 4o�phg e|corit*(0 $�0$2 5-!Making(�q]�pch opDi/izers."` �
  $ 9 Qetei�o Up�-negl cc6ijg`4*R�uc��4Ha liFfePZ�#   9)!YdTti~g qp An�updav�2�qnb4#Oj�4�a� vuns$jfe$epoqh$of4g0limizcTion`oP`n.e s4e %��2�ew}�/tJ%`$ 
 !0 q�)�Runing }h-h}qin"Lmmp(m�tXg"AlgkzktlmZ" !�
$  (40  i- Rmn$dk% Afe.t"ifdthe -n6h2/nma�} l,     `�(�bi Ra0�.ficellx u�dA�e0tle�0qsj/`tdrs /f thM aguzt"e�c.2diog"\+`vxe ieiobay�aTho^s(m�!thw(c,'n`itho
 *�  0 � �( c) �g"ke� X%RfopmanCe$mEttics2al`!{Ar` Q'en|�

��hg"Shgkrmthd ��#umod20fnso�Nhow e�{9on*-��-,--�m--%--)-%-�%55,---m--m-M�5%-%�--}`� al�orjthM$��oa$inm so� a RE�svfl�U!{mpla=EnvcvYo� �er6or�z`e�e fml�ngifw tiqo{ in (r5gl�y) tHKr0opdm~h �1! LO�g2#sMtupJZ$9""�- xAnDo�(��dt redtid�
$4$�
 ``!3	�o�iRomufw5i��va�Tiatio~H%   
b  "8("�ki�* ph!ceholtepq0eo2*$he #Nmrp|abkon Gbatx
  0"A $(5)���)ndk&g t�E(gctnr�cr)tyb�#ompq5�tiol(gz!Px!wi@"�`e�``ai4or^sv)tic b`fencdioL�p`s;ep }o�uleaL�mpat�}8fUncthn?8`S al`azg5m%hpB`1b 
 "  &� Ins��ndyav�O� %hT czsa6iddc� Bu`fer # �
    ) Builei�w�ja!co=puu�ti�j Grapx$for(�kcs�fwnctiols`and e�!c~ns�ic� shUcidIc`ug!ThE cl�cpI�hm
(   ��09	 ]iine+dvaiz�Ng jps
.� `
`�$ 4) Ekmg t(e \�8[essynn�and m�auqamI{9�g)tb letezw" �   8&90)pSet>in' up�mol5m {av�/g thrl5f "dh $gk-s   (
a�  11,`LEfhnKjg(��0T�knS�ne�fuegnҤr�o�h�g tBE!mq{o"loor mF 4e adgora��E �%.g/ =h%&co�� �p4ade oufCti/�$4g�|0`k�io�#Fe�cti�~%pan`!�abd �5�o< fqjgvi/n,pFepenfinc kn ux� al'rkt@)
8 " J (( =2�(Rwnn�jf tje$miIn`looZ"on�the&anemriumm��b �" h    a) Vuh phe c�en6 �n tG�oriB�nmmn}� �1 j!( "� 00b)!Pgri�DiB`ldy`5`dApe vhF0qarc/eT%:�2kf"�hm$abevu Ecggrda�w tG$thEmaif0Mquafi~�s wf`vhu@clgnrivjl ( )
  ,   $ c)�N�7 k%y rer�o2MQnjm oetri�s q.d(sare01gwj�**JVhe Cmre0Fife
'-%--h�%�---
 \hE0Co�e n�l%whdn7t �fhare��$ceO�a�= a3(ti� amgnv�tHe3 Nin���tm`cd4e_0�a�e-$b}|,DN2Luv�(Some$approxhmatg(tvUStt�e:�   1) ((T�nwn2fl+s�g*Ly:*0Fu/ctijNs r�FEtgl(to�oimi.' `nd hyoio�.b/Ph#'eho/LeRq��$0   � F5�w�)ons nnv bqilDang sect)�.s!v�Cn�qutiUho. �pCpy`�Elerant 5/$vxe�d@af`oR_yradMc1j mu�hod!for a$p+zViggl��"iLforitl�

 $("#) Anx O�xmrl�s��uh buNgTi�Zs

#7  4+ IedDeu.tatH/nw�For an0LH� eC�ob-cr)tac {�	�G|ibla })�L�the klGMrIu`m  wha�t coth�the pO�igQ(and!t`e(uAl7e�fu~a�iOo(v-��re0RdpP}seNted!b;07iM0k5`MLPs
�