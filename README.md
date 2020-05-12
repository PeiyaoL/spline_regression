# spline_regression

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

$$
    \begin{aligned}
    f(x) &= ax^{3} + bx^{2} + cx + d \\
    f^{'}(x) &= 3ax^{2} + 2bx + c \\
    f^{''}(x) &= 6ax + 2b
    \end{aligned}
$$


##Function i:
$$
    \begin{aligned}
    f_{i}(x) &= a_{i}x^{3} + b_{i}x^{2} + c_{i}x + d_{i} \\
    f^{'}_{i}(x) &= 3a_{i}x^{2} + 2b_{i}x + c_{i} \\
    f^{''}_{i}(x) &= 6a_{i}x + 2b_{i}
    \end{aligned}
$$


##Constrains:

$ \forall i \in \{2 \ldots n-1\} $:
$$
    \begin{aligned}
    f_{i}(x_{i}) - f_{i-1}(x_{i}) &= 0 \\
    f^{'}_{i}(x_{i}) - f^{'}_{i-1}(x_{i}) &= 0 \\
    f^{''}_{i}(x_{i}) - f^{''}_{i-1}(x_{i}) &= 0
    \end{aligned}
$$

$additonally:$
$$
    \begin{aligned}
    f^{''}_{1}(x_{1}) &= 0 \\
    f^{''}_{n-1}(x_{n}) &= 0
    \end{aligned}
$$


##Cost Function

$$
    \begin{aligned}
    F_{i} &= \begin{bmatrix}f_i & f_i^{'} & f_i^{''}\end{bmatrix}^{T} \\
    Y_{i} &= \begin{bmatrix}y_i & y_i^{'} & y_i^{''}\end{bmatrix}^{T} \\
    E_i^j &= F_i(x_j) - Y_j \\
    g_i^1 &= f_{i}(x_{i}) - f_{i-1}(x_{i}) \\
    g_i^2 &= f^{'}_{i}(x_{i}) - f^{'}_{i-1}(x_{i}) \\
    g_i^3 &= f^{''}_{i}(x_{i}) - f^{''}_{i-1}(x_{i}) \\
    g_i &= \begin{bmatrix}g_i^1 & g_i^2 & g_i^3\end{bmatrix}^{T} \\
    g_1 &=  f_1^{''}(x_1) \\
    g_n &=  f_{n-1}^{''}(x_n) \\
    \phi_{i} &= \begin{bmatrix}\phi_i^1 & \phi_i^2 & \phi_i^3\end{bmatrix}^{T}
    \end{aligned}
$$


$$ 
    \begin{aligned}
    O = &\sum_{i=1}^{n-1}({E_i^i}^TWE_i^i + {E_i^{i+1}}^TWE_i^{i+1}) +\sum_{i=1}^{n}\phi_{i}^Tg_i
    \end{aligned}
$$


$$ 
    \begin{aligned}
    \beta_i &= \begin{bmatrix}a_i & b_i & c_i & d_i\end{bmatrix}^{T} \\
    X &= \begin{bmatrix}\phi_1^T & \beta_1^T & \phi_2^T & \beta_2^T &\ldots & \phi_{n-1}^T & \beta_{n-1}^T & \phi_n^T\end{bmatrix}^{T}
    \end{aligned}
$$

$$ 
    \begin{aligned}
    \frac{\partial O}{\partial X} = \begin{bmatrix} \frac{\partial O}{\partial X_1} & \ldots & \frac{\partial O}{\partial X_m} \end{bmatrix}^{T}
    \end{aligned}
$$

$$
    \begin{aligned}
    H = \frac{\partial^2 O}{\partial X^2} = \frac{\partial }{\partial X}(\frac{\partial O}{\partial X}) =
        \begin{bmatrix}
            \frac{\partial^2 O}{\partial X_1^2} & \frac{\partial^2 O}{\partial X_1\partial X_2} & \ldots & \frac{\partial O}{\partial X_1\partial X_m} \\
            \frac{\partial^2 O}{\partial X_2\partial X_1} & \frac{\partial^2 O}{\partial X_2^2} & \ldots & \frac{\partial O}{\partial X_2\partial X_m} \\
            \vdots & \vdots & \ddots &\vdots \\
            \frac{\partial^2 O}{\partial X_m\partial X_1} & \frac{\partial^2 O}{\partial X_m\partial X_2} & \ldots & \frac{\partial^2 O}{\partial X_m^2}
        \end{bmatrix}
    \end{aligned}
$$

$$
    \begin{aligned}
    B &= -\left.\frac{\partial O}{\partial X}\right| _{X=0} \\
    B &= \begin{bmatrix}
        B_1^{\phi} & B_1^{\beta} & B_2^{\phi} & B_2^{\beta} & \ldots & B_{n-1}^{\phi} & B_{n-1}^{\beta} & B_n^{\phi}
        \end{bmatrix}^T \\
    B_i^{\phi} &= 0 \\
    B_i^{\beta} &= \begin{bmatrix}
        2w_1(y_ix_i^3 + y_{i+1}x_{i+1}^3) + 6w_2(y_i^{'}x_i^2 + y_{i+1}^{'}x_{i+1}^2) + 12w_3(y_i^{''}x_i + y_{i+1}^{''}x_{i+1}) \\
        2w_1(y_ix_i^2 + y_{i+1}x_{i+1}^2) + 4w_2(y_i^{'}x_i + y_{i+1}^{'}x_{i+1}) + 4w_3(y_i^{''} + y_{i+1}^{''}) \\
        2w_1(y_ix_i + y_{i+1}x_{i+1}) + 2w_2(y_i^{'} + y_{i+1}^{'}) \\
        2w_1(y_i + y_{i+1})
        \end{bmatrix}
    \end{aligned}
$$

$$
    \begin{aligned}
    HX = B \\
    X = H^{-1}B
    \end{aligned}
$$

##Form of H Matrix

$$
    \begin{aligned}
    H = \begin{bmatrix}
            0   & T_1^T                                                                                     \\
            T_1 & A_1       & -T_2                                                                          \\
                & -T_2^T    & 0     & T_2^T                                                                 \\
                &           & T_2   & A_2       & -T_3                                                      \\
                &           &       & -T_3^T    & 0         & T_3^T                                         \\
                &           &       &           & \vdots    & \vdots        & \vdots                        \\
                &           &       &           &           & -T_{n-1}^T    & 0         & T_{n-1}^T         \\
                &           &       &           &           &               & T_{n-1}   & A_{n-1}   & -T_n  \\
                &           &       &           &           &               &           & -T_n^T     & 0
        \end{bmatrix} \\
    \end{aligned}
$$

$$
    \begin{aligned}
    A_i^{11} &= 2w_1(x_i^6+x_{i+1}^6)+18w_2(x_i^4+x_{i+1}^4)+72w_3(x_i^2+x_{i+1}^2) \\
    A_i^{12} &= 2w_1(x_i^5+x_{i+1}^5)+12w_2(x_i^3+x_{i+1}^3)+24w_3(x_i+x_{i+1}) \\
    A_i^{13} &= 2w_1(x_i^4+x_{i+1}^4)+6w_2(x_i^2+x_{i+1}^2) \\
    A_i^{14} &= 2w_1(x_i^3+x_{i+1}^3) \\
    \\
    A_i^{21} &= 2w_1(x_i^5+x_{i+1}^5)+12w_2(x_i^3+x_{i+1}^3)+24w_3(x_i+x_{i+1}) \\
    A_i^{22} &= 2w_1(x_i^4+x_{i+1}^4)+8w_2(x_i^2+x_{i+1}^2)+16w_3 \\
    A_i^{23} &= 2w_1(x_i^3+x_{i+1}^3)+4w_2(x_i+x_{i+1}) \\
    A_i^{24} &= 2w_1(x_i^2+x_{i+1}^2) \\
    \\
    A_i^{31} &= 2w_1(x_i^4+x_{i+1}^4)+6w_2(x_i^2+x_{i+1}^2) \\
    A_i^{32} &= 2w_1(x_i^3+x_{i+1}^3)+4w_2(x_i+x_{i+1}) \\
    A_i^{33} &= 2w_1(x_i^2+x_{i+1}^2)+4w_2 \\
    A_i^{34} &= 2w_1(x_i+x_{i+1}) \\
    \\
    A_i^{41} &= 2w_1(x_i^3+x_{i+1}^3) \\
    A_i^{42} &= 2w_1(x_i^2+x_{i+1}^2) \\
    A_i^{43} &= 2w_1(x_i+x_{i+1}) \\
    A_i^{44} &= 4w_1 \\
    \\
    T_i &= \begin{bmatrix}
        x_i^3 & 3x_i^2 & 6x_i \\
        x_i^2 & 2x_i & 2 \\
        x_i & 1 & 0 \\
        1 & 0 & 0
        \end{bmatrix} \\
    T_1 &= \begin{bmatrix}
        6x_1 & 2 &0 & 0
        \end{bmatrix}^T \\
    T_n &= \begin{bmatrix}
        6x_n & 2 &0 & 0
        \end{bmatrix}^T
    \end{aligned}
$$
