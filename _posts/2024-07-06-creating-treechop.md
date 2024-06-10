---
title: "Treechop"
date: 2024-06-07
---

<p align="center">
  <img src="/assets/img/tree.png">
  <img src="/assets/img/tree.png">
  <img src="/assets/img/tree.png">
</p>

### Reproducing the tree gridworld for investigating goal misgeneralisation 

_View the code for TreeChop [here.](https://github.com/myndrws/procgenAISC/tree/master) View the code for goal misgeneralisation in procgen [here.](https://github.com/myndrws/goal_mis_ppo_procgen)_

_This work was done for my mini-project as part of [BlueDot Impact’s technical AI alignment course](https://course.aisafetyfundamentals.com/home/alignment), between May and June 2024._

## Aims

I aimed to create a realisation of the description of a tree gridworld given in [Shah et al (2022)](https://arxiv.org/abs/2210.01790), ready to be used in a project to replicate their goal misgeneralisation results by also being able to run an reinforecement learning agent through it. Through this project, I aimed to build up reinforcement-learning environment software engineering experience and eventually to get a ‘gearsy’ inside-view/ understanding of this specific instance of inner alignment and goal misgeneralisation.

## Background

The paper [‘Goal Misgeneralisation: Why Correct Specifications Aren’t Enough For Correct Goals’ (Shah et al, 2022)](https://arxiv.org/abs/2210.01790) investigates goal misgeneralisation in deep reinforcement learning agents, resulting from distributional shift, across multiple settings. 

One very simple environment described by Shah et al (2022) in their paper is that of the tree gridworld. As for the other settings in the paper, they use the environment to demonstrate a how an optimal-seeming policy has actually been learned from a capable agent misgeneralising to a proxy goal. This setting is a 10*10 grid in which trees spawn at a given rate, proportional to how many trees are left in the environment, whilst the agent can only move to and chop trees. However, unlike the other settings studied, this environment is an online, reset-free, ‘infinite’ setting. That is:


> … the agent acts and learns in the environment, without any train / test distinction and without an ability to reset the environment (preventing episodic learning). To cast this within our framework, we say that at a given timestep, [the training distribution] consists of all past experience the agent has accumulated. The optimal policy in this setting is to chop trees sustainably: the agent should chop fewer trees when they are scarce, to keep the respawn rate high. _(Shah et al, 2020)_

In this setting, the distributional shift that gives rise to the exhibition of goal misgeneralisation is caused by the _agent’s own interaction with the environment_. The agent is rewarded +1 per tree chopped down. The authors find that the agent initially fails to learn the intended goal of ‘chop trees sustainably’, instead learning to chop trees quickly until there are very few trees and an extended period of very low reward whilst the agent learns that chopping with fewer trees in the environment is non-optimal. 

There is no accompanying open-sourced code or released images for the tree gridworld (confirmed with the authors), but it should be possible to replicate the environment from the description in the paper:

> 10 × 10 gridworld with entities (initially): agent, 10 trees. The agent is provided the full gridworld as a 12 × 12 × 2 one-hot encoded image in its observations. Chopping a tree provides +1 reward. If there are fewer than 10 trees, the probability of a new tree spawning at a random empty location is given by: 
>
> _r = max(rmin, rmax × log(1 + ncurrent)/ log(1 + nmax))_
> 
>where rmin and rmax are the minimum and maximum respawn rates, and ncurrent and nmax are the current and maximum number of trees. In the experiment shown in Figure 3 we set rmin = 10−6 , rmax = 0.3, and nmax = 10.


I decided that I would make the gridworld in the style of other OpenAI procgen games, rather than use the binary on-off grid of squares described in the paper. This of course makes the environment more visually complex, but would also allow me to easily plug code into another, open-sourced, paper code base that helped me understand how to train an agent in this environment.

The paper [‘Goal Misgeneralization in Deep Reinforcement Learning’ (Langosco et al, 2021)](https://arxiv.org/abs/2105.14111v7) released open-source codebases forked from both [OpenAI’s Procgen benchmark repo](https://github.com/openai/procgen) and a [PyTorch implementation of a PPO agent for OpenAI’s procgen](https://github.com/joonleesky/train-procgen-pytorch). These three resources formed the bases for my being able to create the extended tree gridworld and be ready to plug in a Proximal Policy Optimization agent[^1]. 

<p align="center">
  <img src="/assets/img/tree_chopper.png"> 
</p>

## What I have achieved 

I have taken the conceptual description of an environment in a paper and embellished it to produce a full, open-sourced procgen implementation of this environment based on OpenAI’s procgen. Specifically, I have taken the description of a one-hot-encoded binary tree gridworld in Shah et al. (2022) and created an open-source version of this environment which is uses C++ and bespoke artefacts to create a more complex visual environment, whilst preserving the principles of the originally described gridworld, such as the 10*10 grid layout and the spawning of trees.

I have subsequently tested out and got working a PPO reinforcement learning agent training process for this gridworld, and trained this agent with simple-to-run-in-CPU-environment parameters. I have set up and improved upon the codebase released by Langosco et al (2021) to do this, and I am now essentially ready to transfer the set up to a HPC environment to make use of GPUs with which to carry out training and attempt to replicate the goal misgeneralisation results found on the tree gridworld in Shah et al (2022). 

A training run for a PPO agent learning a policy in Treechop is shown below. Though this isn’t robust, it’s an example of an agent working with an easily adaptable set up in an environment I have built. 

<p align="center">
  <img src="/assets/img/treechop_gif.gif"> 
</p>

## What I did

* I researched the tree gridworld thoroughly to understand what resources were available - this involved reaching out to the authors, reading the original and closely related papers, and investigating codebases. This led to the discovery that code for goal misgeneralisation studied elsewhere by Langosco et al (2021) was open source, and subsequently to the procgen and PPO agent repos.
* I [forked and built the modified procgen environment](https://github.com/myndrws/procgenAISC/tree/master) from source. Following the installation steps was not simple - I had to contend with conda and cmake, for which I subsequently reported [as the answer to an issue here](https://github.com/openai/procgen/issues/97#issuecomment-2110768974), and deal with a lack of documentation between the original OpenAI procgen repo and the reimplementation of it in the repo I actually built from.
* I tested the installed procgen environments without agents, e.g. running `python -m procgen.interactive --env-name coinrun_aisc --level-seed 6`
* I then set about making my own procgen game! I used bigfish.cpp as an initial template to start messing around in C++, having never used this language before. I designated the name of the tree gridworld to be ‘Treechop’. After messing around a bit, I realised that the miner.cpp game mechanics were much closer to what I wanted in the end product so I started again.
* I made my own pixel art for the environment and figured out how to load these in. 
* With a lot of trial and error, I achieved a basic grid world game! I also gave it the additional optional features of a easy (10*10) or hard (20*20) mode, and when a tree is cleared by the treechopper for the tree to either disappear or for a stump to randomly remain in place for a few steps before that disappears too. The game is runnable in the terminal with the command `python -m procgen.interactive --env-name treechop --distribution-mode easy`

<p margin="5%" align="center">
  <img margin="5%" width="40%" src="/assets/img/treechop_forest.png">
  <img margin="5%" width="40%" src="/assets/img/treechop_stumps.png">
</p>


* I then set about [forking to my own repo](https://github.com/myndrws/goal_mis_ppo_procgen) and running the main codebase used by Langosco et al (2021) to replicate their basic setup for goal misgeneralisation with procgen environments (e.g. Coinrun), so that once this was running I could just swap in my new treechop game. Again, there were a host of laborious dependency issues, but I was eventually able to create an almost-clean `.yml` for future installs. 
* I first got a random agent running in Treechop with logging output, though not visual output, using [example scripts provided from the procgen benchmark repo](https://github.com/openai/procgen/tree/master/procgen/examples) and the [gym3 repo](https://github.com/openai/gym3/blob/master/README.md).
* I then plugged my Treechop game into the training procedure and associated commands, and through minimal trial and error got this running with `python train.py --exp_name treechopper --env_name treechop --distribution_mode easy --param_name debug --num_timesteps 50000 --num_checkpoints 2 --seed 1080 --gpu_device 0 --device cpu`
* I figured out how to add the video-record wrapper whilst training the agent. This was not simple because the method seems to have [had well-documented bugs anyway](https://github.com/openai/procgen/issues/90), but the specific error I was encountering wasn’t recorded anywhere and could only have been definitively solved by updating the imageio-ffmpeg package, something which procgen didn’t allow for. In the end, I had to solve it with a temporary patch in my local installation[^2].
* Combining the video recording wrapper with a simple-hyperparameter training run, I was able to record an instance of a PPO agent learning! In the above sample, where the agent is approaching 100,000 timesteps of continuous gameplay (approx. 1 hour).

## Things I have learned

I have learned to create a simple, small game using C++, and how to use procgen, gym and PyTorch to get a PPO agent working in this environment. Having never touched C++ previously, and not having implemented my own RL agent before, these are steps that make going further to replicate the study seem much more manageable.

It’s really good to keep on track, but it’s also really good to know when to pivot. I started this project with the aim of replicating the full tree gridworld experiment and then opening up the agent to look inside. This was too big a goal and it really helped to monitor my progress and de-scope after examining how much work it would take for me to feel comfortable setting up in a HPC environment. To descope, I researched the possible paths carefully - for example, I looked into the major cloud providers, thought about how I’d get something remote plugged into my local IDE of choice, picked resources that most closely aligned with those used in the original paper - so I have all the next steps for this lined up, I just removed them from the initial MVP. 

I knew this (albeit less intimately) already, but often, untangling other people’s conda dependencies is a bit of a  nightmare, especially if they haven’t produced an automated .yml from their environment to comprehensively cover dependencies and versioning information. 

## Next steps

My next aim is to replicate the goal misgeneralisation behaviour observed in the original paper. The stretch aim is to investigate the agent’s learned behaviour using approaches in mechanistic interpretability to understand _why _the goal misgeneralisation is happening, plus make interesting modifications to the set up (e.g., use tree stumps as visual reminders of trees, have a seedling appear first like a ‘warning’ behaviour to see if that influences the agent.)

The next steps would be to tidy the repo and refactor the code that I am using to make use of modern dependencies. This would solve dependency issues.  This is a less glamorous but probably necessary step. 

I also need to figure out a way to count the number of trees at any given point and calculate the policy ‘affinity’ as described in the original paper. I’ll also need to figure out a way to get screenshots rather than a full video of the agent whilst it’s training/ deactivate the recorder for most of training. I think it will also be easiest to package up my modified version of the procgen repo so this can be installed as a dependency for others.

Then, I aim to run my code on an NVIDIA V100 for 30 training hours. AWS’s P3 series gives access to these, and for 30 hours this would cost about £45. To enable this step I’ll need to be comfortable setting cost limits in the AWS platform so that I don’t accidentally spend loads of money. The plan would be to set up a free-tier EC2 instance first, to make sure I understand all the ssh and environment set-up steps using conda, and then to replicate that on the GPU instance, [following this tutorial.](https://aws.amazon.com/tutorials/train-deep-learning-model-aws-ec2-containers/)

<p align="center">
  <img src="/assets/img/tree_stump.png">
  <img src="/assets/img/tree_stump.png">
  <img src="/assets/img/tree_stump.png">
</p>

## Reference papers and posts

Goal Misgeneralization: Why Correct Specifications Aren’t Enough For Correct Goals - [https://arxiv.org/abs/2210.01790](https://arxiv.org/abs/2210.01790) and [https://sites.google.com/view/goal-misgeneralization](https://sites.google.com/view/goal-misgeneralization) 

Goal Misgeneralization in Deep Reinforcement Learning - [https://arxiv.org/abs/2210.01790](https://arxiv.org/abs/2210.01790) 

Examples of goal misgeneralisation in the wild - [https://docs.google.com/spreadsheets/d/e/2PACX-1vTo3RkXUAigb25nP7gjpcHriR6XdzA_L5loOcVFj_u7cRAZghWrYKH2L2nU4TA_Vr9KzBX5Bjpz9G_l/pubhtml](https://docs.google.com/spreadsheets/d/e/2PACX-1vTo3RkXUAigb25nP7gjpcHriR6XdzA_L5loOcVFj_u7cRAZghWrYKH2L2nU4TA_Vr9KzBX5Bjpz9G_l/pubhtml) 

CoinRun: Solving Goal Misgeneralisation - [https://arxiv.org/pdf/2309.16166.pdf](https://arxiv.org/pdf/2309.16166.pdf) 

[https://www.lesswrong.com/posts/8hqwzYfCKLN9x35Jd/reinforcement-learning-goal-misgeneralization-can-we-guess](https://www.lesswrong.com/posts/8hqwzYfCKLN9x35Jd/reinforcement-learning-goal-misgeneralization-can-we-guess) 

[https://www.lesswrong.com/posts/Cfe2LMmQC4hHTDZ8r/more-examples-of-goal-misgeneralization](https://www.lesswrong.com/posts/Cfe2LMmQC4hHTDZ8r/more-examples-of-goal-misgeneralization) 

Unsolved Problems in ML Safety - [https://arxiv.org/abs/2109.13916](https://arxiv.org/abs/2109.13916) 

V-MPO: ON-POLICY MAXIMUM A POSTERIORI POLICY OPTIMIZATION FOR DISCRETE AND CONTINUOUS CONTROL - [https://arxiv.org/abs/1909.12238](https://arxiv.org/abs/1909.12238) 

AI Safety Gridworlds - [https://arxiv.org/abs/1711.09883](https://arxiv.org/abs/1711.09883) 

Quantifying generalization in reinforcement learning -[https://openai.com/research/quantifying-generalization-in-reinforcement-learning](https://openai.com/research/quantifying-generalization-in-reinforcement-learning) 

[https://deepmind.google/discover/blog/specifying-ai-safety-problems-in-simple-environments/](https://deepmind.google/discover/blog/specifying-ai-safety-problems-in-simple-environments/) 

[https://openai.com/research/procgen-benchmark](https://openai.com/research/procgen-benchmark) 

## Reference repositories and notebooks

[https://github.com/google-deepmind/ai-safety-gridworlds/tree/master?tab=readme-ov-file](https://github.com/google-deepmind/ai-safety-gridworlds/tree/master?tab=readme-ov-file)

[https://github.com/google-deepmind/pycolab/tree/master](https://github.com/google-deepmind/pycolab/tree/master)

[https://github.com/openai/coinrun?tab=readme-ov-file](https://github.com/openai/coinrun?tab=readme-ov-file)<span style="text-decoration:underline;"> </span>

[https://colab.research.google.com/drive/11yqoQgckV3lpe7adN0gkaqUcsKEutiCS?usp=sharing](https://colab.research.google.com/drive/11yqoQgckV3lpe7adN0gkaqUcsKEutiCS?usp=sharing) 

[https://github.com/qqiang00/Reinforce](https://github.com/qqiang00/Reinforce) 

[https://github.com/JacobPfau/procgenAISC/tree/master/procgen](https://github.com/JacobPfau/procgenAISC/tree/master/procgen) 

[https://github.com/joonleesky/train-procgen-pytorch/tree/master](https://github.com/joonleesky/train-procgen-pytorch/tree/master) 

[https://github.com/openai/procgen?tab=readme-ov-file](https://github.com/openai/procgen?tab=readme-ov-file) 

[https://github.com/google-deepmind/lab/tree/master/docs](https://github.com/google-deepmind/lab/tree/master/docs)

[https://huggingface.co/learn/deep-rl-course/en/unit6/hands-on](https://huggingface.co/learn/deep-rl-course/en/unit6/hands-on) 

[https://github.com/google-deepmind/lab](https://github.com/google-deepmind/lab) 


<!-- Footnotes themselves at the bottom. -->
## Footnotes

[^1]: Shah et al (2022) actually use a V-MPO-based agent, rather than PPO. 

[^2]: I had to apply a patch to _io.py in ffmpeg because of a consistent keyword error for the argument 'audio_path', which wasn’t being set by me, but was throwing an exception. I just added '**kwargs' to def write_frames args to solve this. I suspect this is due to package dependency issues, but not clear if it's affecting anyone else; I can’t find references to this in the Issues on the OpenAI procgen repo.

_View the code for TreeChop [here.](https://github.com/myndrws/procgenAISC/tree/master) View the code for goal misgeneralisation in procgen [here.](https://github.com/myndrws/goal_mis_ppo_procgen)_