# 如何解决planning（和learning有区别）


## Bellman 方程
2种：期望方程、最优方程

取期望 / 取max
简单版：

期望：
$$v(s) = \sum_{a\in A}\pi(a|s)(R_s^a + \gamma \sum_{s'\in S}\mathcal{P}_{ss'}^a v(s'))$$
## Evaluation (给定policy求value function)
从任意的value function e.g. $\vec{v} = \vec{0}$ 开始，
用Bellman Expectation方程迭代$v$，最终$v$收敛到最优，并且可以求出最优 policy
$$v_{k+1}(s) = \sum_{a\in A}\pi(a|s)(R_s^a + \gamma \sum_{s'\in S}\mathcal{P}_{ss'}^a v_k(s'))$$

## Policy Iteration:
还是从任意$v$开始，交替进行**evaluation (↑)**和**improvement**。
improvement: Act greedily. 贪心，按照当前的value function，policy是总是走value function最大的地方。

$$\pi '(s) = \argmax_{a\in A} q_{\pi}(s,a)$$

循环进行，最终会收敛到最优value function及policy。（证明见下）

## Value Iteration

- 如果已经知道最优的$v_*(s')$
- 由Bellman方程求出 
  $$v_*(s)\leftarrow\max_{a \in A}\{\mathcal{R}_s^a + \gamma \sum_{s'\in S}\mathcal{P}_{ss'}^a v_*(s'))\}$$
- 反复迭代
- Intuition: 从最终的reward状态开始，一步步倒退前面的状态
实际操作：无论有没有最终状态，dp都可用。
类似Evaluation的操作，不同是使用Bellman Optimality Equation (取max)
$$v_{k+1}(s) = \max_{a\in A}(R_s^a + \gamma \sum_{s'\in S}\mathcal{P}_{ss'}^a v_k(s'))$$
过程：
- iteration k+1:
- for all $s \in S$
- update $v_{k+1}(s)$ from $v_k(s')$
  
*中间*的value function未必对应一个真的policy

*最终*的$v^*$对应一个确定的optimal policy $\pi^*$

和$k=1$的modified policy iteration是等价的。$k=1$的情况就是evaluation一步就马上贪心。
这里value iteration相当于把两步合并成一步，直接从$v_k$推$v_{k+1}$，不用求出中间的policy

# 总结：

### Prediction (Evaluation): 
- EQ: Bellman *Expectation* Eq. 
- Algorithm: iterative policy evaluation.

### Control: 

Policy Iteration:
- EQ: Bellman *Expectation* Eq. +  Greedy policy improvement
- Algorithm: Policy Iteration

Value Iteration:
- EQ: Bellman *Optimality* Eq.
- Algorithm: Value iteration


## 一些优化
Asynchronous DP:
1. In place DP. 不区分新旧$v(s)$, dp的时候直接在当前的值上更新(忽略$v$下标只存一份) 对更新的顺序敏感，一个顺序可能很快，另一个顺序可能啥都没干。
2. 再优化，用priority queue优先更新“改变最大”的状态$s$
3. Real Time DP. 优先更新一个Agent再环境中实际走到的状态 

## DP的问题
- 每个状态更新都得在全部的可能状态中取max，过于expensive。
- 必须知道环境（模型转移概率确定）