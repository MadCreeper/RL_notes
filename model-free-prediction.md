# 前提: 不知道model（在dp中是知道model的）
# 如何Evaluate 一个已知的policy？

## Monte-Carlo Learning
不知道模型（转移矩阵）和rewards，模拟n次，求均值。

value = mean return

Return: *total* discounted reward
$$G_t = R_{t+1} + \gamma R_{t+2} + \dots +\gamma^{T-1}R_T$$

Evaluate state s:
### One time Monte-Carlo
对于每个episode（单次模拟），
第一次到s时计数器N(s)++，然后计算总的return $G_t$，最后按照计数器N(s)取平均

大数定律，均值趋向于期望

### Every-time Monte-Carlo
区别是每次访问到s都会统计

### Incremental Mean
$$\mu_k = \frac{1}{k}\sum_{i=1}^kx_i =\frac{1}{k}( x_k + \sum_{i=1}^{k-1}x_i ) = \mu_{k-1} + \frac{1}{k}(x_k-\mu_{k-1})$$
用这种方式直接更新每个episode的均值，就不用记录总和了

e.g. Every-Visit Monte-Carlo update
$$V(S_t) \leftarrow V(S_t) + \frac{1}{N(S_t)}(G_t - V(S_t)) $$
下面的算法会把$\frac{1}{N(S_t)}$，$G_t$替换掉

exponential forgetting rate: use $\alpha$
$$V(S_t) \leftarrow V(s_t) + \alpha(G_t - V(S_t)) $$

## Temporal Difference Learning (TD)
好处：可以从不完整的episode中学习, i.e. bootstrapping

Estimated Reward = Immedaite Reward + (discounted) *Estimated* Future Reward

TD Target:
$$R_{t+1} + \gamma V(S_{t+1})$$
TD Error:
$$R_{t+1} + \gamma V(S_{t+1}) - V(S_t)$$

Intuition: 先看当前一步的reward，再猜后一步会得到什么reward ($V(S_{t+1})$ )


完整式：
$$V(S_t) \leftarrow V(S_t) + \alpha( R_{t+1} + \gamma V(S_{t+1}) - V(S_t))$$

Bootstrapping: 用旧的估计值估计新的的估计值（式子里有一个$V(S_{t+1})$）, 而MC中只有一个观察值的求和

比Monte-Carlo更短视（？）（但是效果很可能更好）

MC v.s. TD:
- MC: 无偏估计，简单，慢，方差高
- 必定收敛
- 对初值不敏感
- TD: 有偏估计，方差低，效率高
- TD(0) 在function approximation条件下不一定收敛
- 对初值敏感

MC - 收敛到 mean-squared error 最小的解

TD(0) - 收敛到最大似然Markov模型（以最大概率解释看到的数据的Markov模型）的解 (best fits the data)

TD 更好地利用了Markov Property; MC 忽略Markov Property
相应地在Markov/非Markov环境中各有优劣

MC 和 TD 的折衷？

N-step learning

n-step return:
$$G_t^{(n)}(S_t) = R_{t+1} + \gamma R_{t+2} + \dots + \gamma ^{n} V(S_{t+n})$$

n-step TD:
$$V(S_t) \leftarrow V(S_t) + \alpha(G_t^{(n)} - V(S_t ))$$

***如何找到最佳的$n$?***

A: 找不到的（不同的问题最佳n不同）

TD($\lambda$): 取n-step return的几何平均
$$G_t^{\lambda} = (1 - \lambda)\sum_{n=1}^{\infty} \lambda ^{n-1} G_t^{(n)}$$

$$V(S_t) \leftarrow V(S_t) + \alpha(G_t^{\lambda} - V(S_t ))$$