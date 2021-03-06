# Markov Decision Processes（马尔科夫决策过程）

<img src="../Images/image-20210608162547201.png" alt="image-20210608162547201" style="zoom:67%;" />

Markov Decision Process can model a lot of real-world problem. It formally describes the framework of reinforcement learning

Under MDP, the environment is fully observable

- Optimal control primarily deals with continuous MDPs
- Partially observable problems can be converted into MDPs

在强化学习中，agent和环境的交互如图所示。agent在得到环境的状态后，采取一个动作，并返还给环境；环境会进入下一个状态，把下个状态传回agent。这个交互过程可以通过马尔科夫决策过程表示。在马尔科夫决策过程中，环境是全部可观测的，部分观测问题也可以转换为MDP问题。

## Markov Property（马尔科夫性质）

The history of states: $ h_{t}=\{s_{1}, s_{2}, s_{3}, \ldots, s_{t}\} $

State $s_{t}$ is Markovian if and only if: 
$$
p\left(s_{t+1} \mid s_{t}\right)=p\left(s_{t+1} \mid h_{t}\right)
$$

$$
p\left(s_{t+1} \mid s_{t}, a_{t}\right)=p\left(s_{t+1} \mid h_{t}, a_{t}\right)
$$

如果状态的转移满足马尔科夫说明：下一个状态仅取决于当前状态，而与之前的状态无关。

## Markov Process/Markov Chain（马尔科夫链）

<img src="../Images/image-20210608170827350.png" alt="image-20210608170827350" style="zoom: 33%;" />

State transition matrix P specifies $p\left(s_{t+1}=s^{\prime} \mid s_{t}=s\right)$
$$
P=\left[\begin{array}{cccc}
P\left(s_{1} \mid s_{1}\right) & P\left(s_{2} \mid s_{1}\right) & \ldots & P\left(s_{N} \mid s_{1}\right) \\
P\left(s_{1} \mid s_{2}\right) & P\left(s_{2} \mid s_{2}\right) & \ldots & P\left(s_{N} \mid s_{2}\right) \\
\vdots & \vdots & \ddots & \vdots \\
P\left(s_{1} \mid s_{N}\right) & P\left(s_{2} \mid s_{N}\right) & \ldots & P\left(s_{N} \mid s_{N}\right)
\end{array}\right]
$$
状态转移矩阵P用于描述状态的转移，每个元素表示从一个状态转移到另一个状态的概率。

### Example of MP

<img src="../Images/image-20210608171445656.png" alt="image-20210608171445656" style="zoom: 25%;" />

Sample episodes starting from s3

s3; s4; s5; s6; s6

s3; s2; s3; s2; s1

s3; s4; s4; s5; s5

有了马尔科夫链，就可以取样，获得很多的轨迹。

## Markov Reward Process （马尔科夫奖励过程）

Markov Reward Process is a Markov Chain + reward

Definition of Markov Reward Process (MRP)

- S is a (finite) set of states
- P is dynamics/transition model that specifies P:  $P\left(s_{t+1}=s^{\prime} \mid s_{t}=s\right)$
- R is a reward function: $R\left(s_{t}=s\right)=\mathbb{E}\left[r_{t} \mid s_{t}=s\right]$
- Discount factor: $\gamma \in[0,1]$

If define number of states, R can be a vector

马尔科夫奖励过程的状态集合和状态转移和马尔科夫链相同。R是奖励函数，是期望，是到达某个状态后可以获得的收益。D是折扣率。马尔科夫奖励过程就像是随波逐流的小船，按照P流动，按照R获得收益。

### Return and Value function

#### Definition of Horizon

- Number of maximum time steps in each episode
- Can be infinite, otherwise called infinite Markov (reward) Process

#### Definition of Return

- Discounted sum of rewards from time step t to horizon

$$
G_{t}=R_{t+1}+\gamma R_{t+2}+\gamma^{2} R_{t+3}+\gamma^{3} R_{t+4}+\ldots+\gamma^{T-t-1} R_{T}
$$

- 从公式中可以发现，越往后的收益折扣的越多，因为我们更期待更快地获得收益。

#### Definition of  value function Vt (s) for a MRP

Expected return from t in state s
$$
\begin{aligned}
V_{t}(s) &=\mathbb{E}\left[G_{t} \mid s_{t}=s\right] \\
&=\mathbb{E}\left[R_{t+1}+\gamma\left|R_{t+2}+\gamma^{2} R_{t+3}+\ldots+\gamma^{T-t-1} R_{T}\right| s_{t}=s\right]
\end{aligned}
$$

- Present value of future rewards
- 价值函数就是Return的期望，也就是进入某个状态后，有可能在未来获得多少价值。

#### Why Discount Factor 

- Avoids infinite returns in cyclic Markov processes

- Uncertainty about the future may not be fully represented

- If the reward is financial, immediate rewards may earn more interest than delayed rewards Animal/human behavior shows preference for immediate reward

- It is sometimes possible to use undiscounted Markov reward processes

    $\gamma$ = 0: Only care about the immediate reward

    $\gamma$ = 1: Future reward is equal to the immediate reward

马尔科夫链是带环的，需要避免无限的return。为了尽可能快地得到收益，而不是在未来某个时间获得收益。

### Example of MRP

<img src="../Images/image-20210608173110889.png" alt="image-20210608173110889" style="zoom:25%;" />

Reward: +5 in s1, +10 in s7, 0 in all other states.

So that we can represent R = [5; 0; 0; 0; 0; 0; 10]

定义好每个状态的收益后，R就可以用向量表示。

Sample returns G for a 4-step episodes with $\gamma$ = 1/2

- return for s4; s5; s6; s7 : $0+\frac{1}{2} \times 0+\frac{1}{4} \times 0+\frac{1}{8} \times 10=1.25$
- return for s4; s3; s2; s1 : $0+\frac{1}{2} \times 0+\frac{1}{4} \times 0+\frac{1}{8} \times 5=0.625$
- return for s4; s5; s6; s6 : = 0

### Bellman Equation

$$
V(s)=\underbrace{R(s)}_{\text {Immediate reward }}+\underbrace{\gamma \sum_{s^{\prime} \in S} P\left(s^{\prime} \mid s\right) V\left(s^{\prime}\right)}_{\text {Discounted sum of future reward }}
$$

Bellman equation describes the iterative relations of states

贝尔曼方程表示了状态之间的递推关系。
$$
\left[\begin{array}{c}
V\left(s_{1}\right) \\
V\left(s_{2}\right) \\
\vdots \\
V\left(s_{N}\right)
\end{array}\right]=\left[\begin{array}{c}
R\left(s_{1}\right) \\
R\left(s_{2}\right) \\
\vdots \\
R\left(s_{N}\right)
\end{array}\right]+\gamma\left[\begin{array}{cccc}
P\left(s_{1} \mid s_{1}\right) & P\left(s_{2} \mid s_{1}\right) & \ldots & P\left(s_{N} \mid s_{1}\right) \\
P\left(s_{1} \mid s_{2}\right) & P\left(s_{2} \mid s_{2}\right) & \ldots & P\left(s_{N} \mid s_{2}\right) \\
\vdots & \vdots & \ddots & \vdots \\
P\left(s_{1} \mid s_{N}\right) & P\left(s_{2} \mid s_{N}\right) & \ldots & P\left(s_{N} \mid s_{N}\right)
\end{array}\right]\left[\begin{array}{c}
V\left(s_{1}\right) \\
V\left(s_{2}\right) \\
\vdots \\
V\left(s_{N}\right)
\end{array}\right]
$$

$$
V=R+\gamma P V
$$

#### Analytic solution（解析法）for value of MRP

$$
V=(I-\gamma P)^{-1} R
$$

- Matrix inverse takes the complexity O(N3) for N states
- Only possible for a small MRPs
- 当状态数量很多时，矩阵求逆的复杂度是非常高的。所以解析法只适用于小型MRP。

#### Iterative Algorithm（迭代算法）for value of MRP

- Dynamic Programming
- Monte-Carlo evaluation
- Temporal-Difference learning

##### Monte Carlo Algorithm

<img src="../Images/image-20210609121423686.png" alt="image-20210609121423686" style="zoom:50%;" />

蒙特卡洛方法就是对全部return取平均的方法 。

##### Iterative Algorithm for Computing Value of a MRP

<img src="../Images/image-20210609122724987.png" alt="image-20210609122724987" style="zoom:50%;" />

将贝尔曼方程作为update，不断地迭代更新，直到状态变化不大时返回价值。

## Markov Decision Process (MDP)

Markov Decision Process is Markov Reward Process with decisions.

Definition of MDP

- S is a finite set of states
- A is a finite set of actions
- $P^{a}$ is dynamics/transition model for each action: $P\left(s_{t+1}=s^{\prime} \mid s_{t}=s, a_{t}=a\right)$
- R is a reward function: $R\left(s_{t}=s, a_{t}=a\right)=\mathbb{E}\left[r_{t} \mid s_{t}=s, a_{t}=a\right]$
- Discount factor: $\gamma \in[0,1]$

MDP is a tuple: $(S, A, P, R, \gamma)$

马尔科夫决策过程比马尔科夫奖励过程多了决策，也就是当前的状态S和采取的动作A共同影响P和R。

### Policy in MDP

Give a state, specify a distribution over actions

Policy : $\pi(a \mid s)=P\left(a_{t}=a \mid s_{t}=s\right)$

Policies are stationary (time-independent), $A_{t} \sim \pi(a \mid s)$ for any $t>0$

Given an MDP and a policy $\pi$
$$
\begin{aligned}
P^{\pi}\left(s^{\prime} \mid s\right) &=\sum_{a \in A} \pi(a \mid s) P\left(s^{\prime} \mid s, a\right) \\
R^{\pi}(s) &=\sum_{a \in A} \pi(a \mid s) R(s, a)
\end{aligned}
$$
当给出MDP和策略函数后，可以将MDP转换为MRP。

### Comparison of MP/MRP and MDP

<img src="../Images/image-20210609162610022.png" alt="image-20210609162610022" style="zoom: 50%;" />

### Value function for MDP

The state-value function $V^{\pi}(s)$ of an MDP is the expected return starting from state s, and following policy $\pi$
$$
v^{\pi}(s)=\mathbb{E}_{\pi}\left[G_{t} \mid s_{t}=s\right]
$$
The action-value function $q^{\pi}(s, a)$ is the expected return starting from state s, taking action a, and then following policy $\pi$
$$
q^{\pi}(s, a)=\mathbb{E}_{\pi}\left[G_{t} \mid s_{t}=s, A_{t}=a\right]
$$
We have the relation between $v^{\pi}(s)$ and $q^{\pi}(s, a)$
$$
v^{\pi}(s)=\sum_{a \in A} \pi(a \mid s) q^{\pi}(s, a)
$$

### Bellman Expectation Equation

The state-value function can be into immediate reward plus discounted value of the successor state
$$
v^{\pi}(s)=E_{\pi}\left[R_{t+1}+\gamma v^{\pi}\left(s_{t+1}\right) \mid s_{t}=s\right]
$$
The action-value function can similarly be decomposed
$$
q^{\pi}(s, a)=E_{\pi}\left[R_{t+1}+\gamma q^{\pi}\left(s_{t+1}, A_{t+1}\right) \mid s_{t}=s, A_{t}=a\right]
$$
上面两个等式表达了当前状态和未来状态的关系。
$$
\begin{aligned}
v^{\pi}(s) &=\sum_{a \in A} \pi(a \mid s)\left(R(s, a)+\gamma \sum_{s^{\prime} \in S} P\left(s^{\prime} \mid s, a\right) v^{\pi}\left(s^{\prime}\right)\right) \\
q^{\pi}(s, a) &=R(s, a)+\gamma \sum_{s^{\prime} \in S} P\left(s^{\prime} \mid s, a\right) \sum_{a^{\prime} \in A} \pi\left(a^{\prime} \mid s^{\prime}\right) q^{\pi}\left(s^{\prime}, a^{\prime}\right)
\end{aligned}
$$
上面两个等式表达了当前状态的价值和未来价值的关系。

#### Backup Diagram for $V^{\pi}$

<img src="../Images/image-20210609165823638.png" alt="image-20210609165823638" style="zoom: 50%;" />

