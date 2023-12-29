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
#### Unsupervised Environment Design, UED
To train generalizable RL agents, researchers recently proposed the Unsupervised Environment Design (UED), which formulates a teacher-student framework, where the teacher creates numerous environments to train the student so that the student will be robust to unseen scenarios. UED aims to find out what are the best training environments given the student's current policy. <br>
![image](figures/UED_overview.png#pic_center)

#### Regret
The leading algorithms in UED all rely on the *regret* notion, which is defined as the difference between the agent's optimal performance $V^\*(\pi)$ and current actual performance $\mathbb{E}[V(\pi)]$. The optimal performance and actual performance are captured by the maximum return and average return in PAIRED<sup>[1]</sup>. <br>
$$regret^{\theta}(\pi) \approx \max_{\tau \sim \pi} V^{\theta}(\tau) - \mathbb{E}_{\tau \sim \pi} V^{\theta}(\tau)$$



## Method
Some text <br>
<img src=figures/UED_overview.png width=60% />

- Instruction 1
- Instruction 2
- Instruction 3


## Experiment Results
Here goes all the budgets


## Acknowledgement
This research/project is supported by the National Research Foundation Singapore and DSO National Laboratories under the AI Singapore Programme (AISG Award No: AISG2-RP- 2020-017).


## Reference
<div id="paired"></div>
- [1] [Emergent Complexity and Zero-shot Transfer via Unsupervised Environment Design](https://arxiv.org/abs/2012.02096)
