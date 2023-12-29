# Generalization through Diversity: Improving Unsupervised Environment Design, IJCAI-2023

<p align="center">
  <h3 align="center">WenjunLi, PradeepVarakantham, DexunLi</h3>
  <p align="center">
    Singapore Management University
    <br>
    <a href="https://www.ijcai.org/proceedings/2023/0601.pdf">[Paper]</a>
    ·
    <a href="https://github.com/wenjunli-0/diplr_webpage/">[Codes]</a>
  </p>
</p>


## Introduction
Agent decision-making using Reinforcement Learning (RL) heavily relies on either a model or simulator of the environment (e.g., moving in an 8x8 maze with three rooms, or playing Chess on an 8x8 board). Due to this dependence, small changes in the environment (e.g., positions of obstacles in the maze, size of the board) can severely affect the effectiveness of the policy learned by the agent. To that end, existing work has proposed the Unsupervised Environment Design (UED) framework to train RL agents on an adaptive curriculum of environments (generated automatically) to improve performance on out-of-distribution (OOD) test scenarios. Specifically, existing research in UED has employed the potential for the agent to learn in an environment (captured using *regret*) as the key factor in selecting the next environment(s) to train the agent. However, such a mechanism can select similar environments (with a high potential to learn) thereby making agent training redundant in all but one of those environments. To that end, we provide a principled approach to adaptively identify diverse environments based on a novel distance measure relevant to environment design. We empirically demonstrate the versatility and effectiveness of our method in comparison to multiple leading approaches for unsupervised environment design on three distinct benchmark problems used in literature.


## Background
### Unsupervised Environment Design, UED
To train generalizable RL agents, researchers recently proposed the Unsupervised Environment Design (UED), which formulates a teacher-student framework, where the teacher creates numerous environments to train the student so that the student will be robust to unseen scenarios. UED aims to find out what are the best training environments given the student's current policy. <br>
![image](figures/UED_overview.png#pic_center)

### Regret
The leading algorithms in UED all rely on the *regret* notion, which is defined as the difference between the agent's optimal performance and current actual performance. The pioneering paper in UED, PAIRED<sup>[1]</sup>, rollouts the agent policy in the environment (denoted by $\theta$) and collect multiple trajectories. The regret is approximated as the difference between the maximum return and the average return. <br>
$$regret^{\theta}(\pi) \approx \max_{\tau \sim \pi} V^{\theta}(\tau) - \mathbb{E}_{\tau \sim \pi} V^{\theta}(\tau)$$
![image](figures/regret_demo.png)

The teacher in UED will generate<sup>[1]</sup> or replay<sup>[2]</sup> high-regret environments, and the student will learn in the environments provided by the teacher. By iteratively repeating such interactions between teacher and student, the student gradually accumulates experiences and generalizes to unseen scenarios. 

## Method
Although the algorithms in UED have improved the student's generalization performance significantly by utilizing regret. However, to generalize better, it is not sufficient to train the student only in high-regret environments. We can have multiple high-regret environments, that are very “similar” to each other and the student does not learn a lot from being trained in similar environments. Thus, environments also have to be sufficiently “different”, so that the student can gain more perspective on different challenges that it can face.

To that end, in this paper, we introduce a diversity metric in UED, which is defined based on the distance between occupancy distributions associated with student trajectories from different environments. We then provide a principled method to select levels with the diversity metric to provide better generalization performance than the state-of-the-art.

### Distance between Environments
Assume two environments, denoted by $\theta^1$ and $\theta^2$. We want to calculate the distance between them. An illustration can be found in the below figure. One potential option is to encode each level using one of a variety of encoding methods (e.g., Variational Autoencoder) or using the parameters associated with the level and then taking the distance between encodings of environments. However, such an approach has multiple major issues. <br>
![image](figures/distance_demo_small.png)

- Encoding a level does not account for the sequential moves the student agent will make through the level, which is of critical importance as the similarity is with respect to student policy;
- There can be stochasticity in the level that is not captured by the parameters of the level. For instance, the Bipedal-Walker domain has multiple free parameters controlling the terrain. Because of the existence of stochasticity in the level generation process, two levels could be very different while we have near-zero distance measurement given their environment parameter vectors;
- Distance between environment parameters requires normalization in each parameter dimension and is domain-specific.

Since we collect several trajectories within current levels when approximating the regret value, we can naturally get the state-action distributions induced by the current policy. Therefore, we propose to evaluate similarity on the different levels based on the distance between occupancy distributions of the current student policy. The hypothesis is that if the levels are similar, then the trajectories traversed by the student agent within the level will have similar state-action distribution. 
![image](figures/trajectory_distance_small.png)


### Diversity Induced Prioritied Level Replay, DIPLR
Our algorirthm maintains a buffer of high-potential environments for training. At each iteration, we either: (a) generate a new level to be added to the buffer; or (b) sample a mini-batch of levels for the student agent to train on. To increase diversity of the environment buffer, we add new environments to the buffer that have the highest value of this distance (amongst all the randomly generated levels). We can also combine this distance measure with regret when taking a decision to include a level in the buffer so that different and challenging levels are added. Instead of solely considering the diversity aspect of the environment buffer, we can also take learning potential into account by using regrets so that we will have an environment buffer that is not only filled up with diverse training environments but also challenging environments that continuously push the student. An overview of our proposed algorithm is shown below.
![image](figures/algo_pipeline_small.png)


## Experiment Results
Results on three highly distinct benchmark domains. 

- Minigrid
- Bipedal-Walker
- Car-Racing


## Acknowledgement
This research/project is supported by the National Research Foundation Singapore and DSO National Laboratories under the AI Singapore Programme (AISG Award No: AISG2-RP- 2020-017).


## Reference
<div id="paired"></div>
- [1] [Emergent Complexity and Zero-shot Transfer via Unsupervised Environment Design](https://arxiv.org/abs/2012.02096)
- [2] [Prioritized Level Replay](https://arxiv.org/abs/2010.03934)
