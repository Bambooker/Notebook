# Markov Decision Processes

# 马尔科夫决策过程

<img src="../Images/image-20210608162547201.png" alt="image-20210608162547201" style="zoom:67%;" />

Markov Decision Process can model a lot of real-world problem. It formally describes the framework of reinforcement learning

Under MDP, the environment is fully observable

- Optimal control primarily deals with continuous MDPs
- Partially observable problems can be converted into MDPs

在强化学习中，agent和环境的交互如图所示。agent在得到环境的状态后，采取一个动作，并返还给环境；环境会进入下一个状态，把下个状态传回agent。这个交互过程可以通过马尔科夫决策过程表示。在马尔科夫决策过程中，环境是全部可观测的，部分观测问题也可以转换为MDP问题。

## Markov Property

## 马尔科夫性质

The history of states: $h_{t}=\left\{s_{1}, s_{2}, s_{3}, \ldots, s_{t}\right\}$











