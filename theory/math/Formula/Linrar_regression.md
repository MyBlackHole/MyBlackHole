线性回归
$$
\begin{align}
    假设拟合平面函数：& \quad y = w_0 + w_1x_1 + ...... + w_nx_n\\
    每个样本：& \quad y_i = W^Tx_i + \epsilon_i\\
    每个样本\epsilon_i是独立同分布满足高斯分布函数：&\quad p(\epsilon_i) = \frac{1}{\sqrt{2\pi\sigma}}exp(-\frac{(\epsilon_i)^2}{2\sigma^2})(均值为0、方差为w^2)\\
    (2)导入(3)：&\quad p(\epsilon_i) = \frac{1}{\sqrt{2\pi\sigma}}exp(-\frac{(y_i  -  W^Tx_i )^2}{2\sigma^2})\\
   满足似然函数(越大越好)：&\quad L(w) = \prod_{i=1}^np(y_i|x_i;w) =  \prod_{i=1}^n\frac{1}{\sqrt{2\pi\sigma}}exp(-\frac{(y_i  -  W^Tx_i )^2}{2\sigma^2})\\
   对数似然函数(越大越好)：&\quad logL(w) =log\prod_{i=1}^n\frac{1}{\sqrt{2\pi\sigma}}exp(-\frac{(y_i  -  W^Tx_i )^2}{2\sigma^2})\\
   展开似然函数化简：&\quad \sum_{i=i}^n\frac{1}{\sqrt{2\pi\sigma}}exp(-\frac{(y_i  -  W^Tx_i )^2}{2\sigma^2})\\
   &=nlog\frac{1}{\sqrt{2\pi\sigma}} - \frac{1}{\sigma^2}\cdot\frac{1}{2}\sum_{i=1}^n(y_i  -  W^Tx_i )^2\\
  求(8)最大就是求j(w)最小：&j(w) = \frac{1}{2}\sum_{i=1}^n(y_i-W^Tx_i)^2\quad(最小二乘)\\
  &\quad\quad =\frac{1}{2}(Y-XW)^T(Y-XW)\\
  &\quad\quad =\frac{1}{2}(Y^T-W^TX^T)(Y-XW)\\
  求偏导：\bigtriangledown_wj(w)&=\bigtriangledown_w(\frac{1}{2}(Y^T-W^TX^T)(Y-XW))\\
  &=\bigtriangledown_w(\frac{1}{2}(Y^TY-W^TX^TY-Y^TXW+W^TX^TXW))\\
  &=\bigtriangledown_w(\frac{1}{2}(Y^TY-W^TX^TY-(W^T(Y^TX)^T)^T+W^TX^TXW))\\
  &=\frac{1}{2}(-X^TY-(Y^TX)^T+2X^TXW)\\
  &=-X^TY+X^TXW=0\\
  w&=\frac{X^TY}{X^TX}
\end{align}
$$

## 逻辑回归

$$

$$

