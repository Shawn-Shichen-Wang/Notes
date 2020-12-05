[TOC]

# $\LaTeX$

## 排版方式

**行级元素(inline)**，行级元素使用`$...$`，两个$表示公式的首尾。

**块级元素(displayed)**，块级元素使用`$$...$$`。块级元素默认是居中显示的。

## 常用西文符号

- `\alpha`, `\beta`, …, `\omega`代表$\alpha$,$\beta$,…$\omega$. 

- 大写字母,使用`\Gamma`, `\Delta`, …, `\Omega`代表$\Gamma$,$\Delta$…,$\Omega$.

## 上标与下标

使用 ^和 _ 表示上标和下标.

- `x_i^2`: $x_i^2$ 
- `\log_2 x`: $\log_2^x$
- 使用{}来消除二义性——优先级问题
  - 例如10^10 
    - $10^10$，显然是错误的，正确的语法应该是`10^{10}`。
  - 同样的，
    -  `x_i^2`: $x_i^2$  与
    -  `x_{i^2}`: $x_{i^2}$的区别。

## 括号

- 小括号和中括号直接使用
- 大括号由于用来分组，所以需要**转义**。
  - `\{1+2\}`: $\{1+2\}$

## 运算

- 分数：`\frac{}{}`。例如，`\frac{1+1}{2}+1`: $\frac{1+1}{2}+1$
- 求和：`\sum_1^n`: $\sum_1^n$
- 积分：`\int_1^n`: $\int_1^n$
- 极限：`lim_{x \to \infty`: $lim_{x \to \infty}$
- 矩阵：`$$\begin{matrix}…\end{matrix}$$`，使用&分隔同行元素，\\换行。例如：

```latex
$$
        \begin{matrix}
        1 & x & x^2 \\
        1 & y & y^2 \\
        1 & z & z^2 \\
        \end{matrix}
$$
```

得到的公式为：
$$
\begin{matrix}
        1 & x & x^2 \\
        1 & y & y^2 \\
        1 & z & z^2 \\
        \end{matrix}
$$

## 杂例

- `\bar x`: $\bar x$
  
- `\overline {xyz}`: $\overline {xyz}$
  
- `$$h(\theta)=\sum_{j=0}^n \theta_jx_j$$`  
  $$
  h(\theta)=\sum_{j=0}^n \theta_jx_j
  $$

- `$$J(\theta)=\frac1{2m}\sum_{i=0}(y^i-h_\theta(x^i))^2$$`  
  $$
  J(\theta)=\frac1{2m}\sum_{i=0}(y^i-h_\theta(x^i))^2（均方误差\ 𝑜𝑟 \ 𝑐𝑜𝑠𝑡 \ 𝑓𝑢𝑛𝑐𝑡𝑖𝑜𝑛）
  $$

- `$$\frac{\partial J(\theta)}{\partial\theta_j}=-\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i))x^i_j $$`
  $$
  \frac{\partial J(\theta)}{\partial\theta_j}=-\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i))x^i_j（批量梯度下降的梯度算法）
  $$



- 

```latex
$$
f(n) =
    \begin{cases}
    n/2,  & \text{if $n$ is even} \\
    3n+1, & \text{if $n$ is odd}
    \end{cases}
$$
```

$$
f(n) =
    \begin{cases}
    n/2,  & \text{if $n$ is even} \\
    3n+1, & \text{if $n$ is odd}
    \end{cases}
$$



- 

```latex
$$
\left\{ 
    \begin{array}{c}
        a_1x+b_1y+c_1z=d_1 \\ 
        a_2x+b_2y+c_2z=d_2 \\ 
        a_3x+b_3y+c_3z=d_3
    \end{array}
\right. 
$$
```

$$
\left\{ 
    \begin{array}{c}
        a_1x+b_1y+c_1z=d_1 \\ 
        a_2x+b_2y+c_2z=d_2 \\ 
        a_3x+b_3y+c_3z=d_3
    \end{array}
\right. 
$$



- 

```latex
$$
X=\left(
        \begin{matrix}
            x_{11} & x_{12} & \cdots & x_{1d}\\
            x_{21} & x_{22} & \cdots & x_{2d}\\
            \vdots & \vdots & \ddots & \vdots\\
            x_{m1} & x_{m2} & \cdots & x_{md}\\
        \end{matrix}
    \right)
    =\left(
         \begin{matrix}
                x_1^T \\
                x_2^T \\
                \vdots\\
                x_m^T \\
            \end{matrix}
    \right)
$$
```

$$
X=\left(
        \begin{matrix}
            x_{11} & x_{12} & \cdots & x_{1d}\\
            x_{21} & x_{22} & \cdots & x_{2d}\\
            \vdots & \vdots & \ddots & \vdots\\
            x_{m1} & x_{m2} & \cdots & x_{md}\\
        \end{matrix}
    \right)
    =\left(
         \begin{matrix}
                x_1^T \\
                x_2^T \\
                \vdots\\
                x_m^T \\
            \end{matrix}
    \right)
$$





- ```LaTeX
  $$
  \begin{align}
  \frac{\partial J(\theta)}{\partial\theta_j}
  & = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i)) \frac{\partial}{\partial\theta_j}(y^i-h_\theta(x^i)) \\
  & = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i)) \frac{\partial}{\partial\theta_j}(\sum_{j=0}^n\theta_jx_j^i-y^i) \\
  & = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i))x^i_j
  \end{align}
  $$
  ```

  

$$
\begin{align} 
\frac{\partial J(\theta)}{\partial\theta_j} 
& = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i)) \frac{\partial} {\partial\theta_j}(y^i-h_\theta(x^i)) \\
& = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i)) \frac{\partial} {\partial\theta_j}(\sum_{j=0}^n\theta_jx_j^i-y^i) \\
& = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i))x^i_j 
\end{align}
$$

## 参考资料

- [在博客中使用LaTeX插入数学公式](https://www.cnblogs.com/Sinte-Beuve/p/6160905.html)

- [一份其实很短的LaTeX入门文档](https://liam.page/2014/09/08/latex-introduction/)

- [以 Markdown 撰写文稿，以 LaTeX 排版](https://liam.page/2020/03/30/writing-manuscript-in-Markdown-and-typesetting-with-LaTeX/)

- [LaTeX2e for authors](https://www.latex-project.org/help/documentation/usrguide.pdf)
- [LaTeX各种写法](https://my.oschina.net/davelet/blog/1831306)
