# Intro
## What is RL?
Agent:
Executes Action $A_t$, gets reward $R_t$, observation of environment $O_t$

Environment: gets action $A_t$, emits reawrd $R_t$ and environment $O_t$

The **history**: $$H_t =A_1, O_1, R_1, \dots, A_t, O_t, R_t$$
the history decides what happens next.

History is too enormous. Use **states**:
$$S_t = f(H_t)$$

Environment state: $S_e^t$
环境内部的状态。Agent往往是无法看到的。
**是Markov的**

Agent state: $S_a^t$: Agent直接观察到的信息.
i.e. agent用来决策的信息
也是histor$H_t$的函数。

Information State (Markov State) 马尔可夫状态
一个包含了所有历史的状态
$$P[S_{t+1}|S_t] = P[S_{t+1}|S_1,\dots,S_t]$$
>未来只取决于现在的状态，与过去无关。
环境内部状态是Markov的。

不同的(Markov) State选择决定了Agent的预测结果和决策
 
## Full Observable Environments:
$O_t = S_t^a = S_t^e$ 
Agent完全看到环境内部的全部细节


## Partially Observable Environments:
打扑克，机器人摄像头，etc.
$S_t^a\neq S^e_t$
几种可选状态：
- $S_a^t = H_t$ 选全部历史（naive）
- Beliefs of environment state: $S_a^t = (P[S_t^e=s^1],\dots, P[S_t^e=s^n])$ 由环境状态处在不同概率组成的向量
- Recurrent Neural Network: 
 $S_a^t = \sigma(S^a_{t-1}W_s + O_t W_o)$
上一步的状态和当前观察线性组合一下，再加个sigmoid

## RL Agent:
- Policy: agent的行为（函数？）
- Value function: 每个state或者action的好坏评价
- Model: agent对于环境的认识

### policy
就是agent的行为, 知道policy就是知道面对一个状态agent该干什么
- 决定性policy
- 随机性(stochastic) policy

### Value function
**对未来reward的预测**
用来评价一个state的好坏

$$v_{\pi}(s)=E_{\pi}[R_t + \gamma R_{t+1} + \gamma^2R_{t+2} + \dots |S_t = s $$
$\gamma$: discount factor. 未来reward对当下的value要打个折扣 e.g. $\gamma=0.99$

### Model
用来预测环境将来会怎么样
 
- Transitions: *P* 预测下个状态
- Rewards: *R* 预测下个reward
RL常常是可以Model-free的

## RL 的分类

### Value Based 
- No policy
- 只看一个Value function

### Policy Based
- 只看policy （只有迷宫里的箭头）
- 没有Value function

### Actor Critic
- policy & value function


### Model Free
- 看Policy and/or Value function
- No model， 不认识环境

### Model Based
- 看Policy and/or Value function
- 有Model