# 歳差行列の定式化のバリエーション

## 目次

<!-- TOC depthFrom:2 -->

- [目次](#目次)
- [定義](#定義)
- [IAU1976](#iau1976)
- [IAU2000/2006](#iau20002006)
- [Fukushima(2003)](#fukushima2003)
- [Precession angles(arcsecond)](#precession-anglesarcsecond)

<!-- /TOC -->

## 定義

- 位置・速度ベクトル
$$
\begin{aligned}
    \vec{r} = (r_x, r_y, r_z) \\
    \vec{v} = (v_x, v_y, v_z)
\end{aligned}
$$

- 座標変換回転行列
$$
\begin{aligned}
R_x = \left(
    \begin{array}{ccc}
        1 & 0 & 0 \\
        0 & \cos{\theta} & \sin{\theta} \\
        0 & -\sin{\theta} & \cos{\theta}
    \end{array}
\right) \\
R_y = \left(
    \begin{array}{ccc}
        \cos{\theta} & 0 & -\sin{\theta} \\
        0 & 1 & 0 \\
        \sin{\theta} & 0 & \cos{\theta}
    \end{array}
\right) \\
R_z = \left(
    \begin{array}{ccc}
        \cos{\theta} & \sin{\theta} & 0 \\
        -\sin{\theta} & \cos{\theta} & 0 \\
        0 & 0 & 1
    \end{array}
\right)
\end{aligned}
$$

- EME2000 ⇔ Mean of Dateの座標変換
歳差行列$P(t)$に対して，
$$
\begin{aligned}
\vec{r}_{MOD} &= \vec{r}_{2000}P(t) \\
\vec{v}_{MOD} &= \vec{v}_{2000}P(t) \\
\vec{r}_{2000} &= \vec{r}_{MOD}P(t)^{\mathrm{T}} \\
\vec{v}_{2000} &= \vec{v}_{MOD}P(t)^{\mathrm{T}}
\end{aligned}
$$

- 1ユリウス世紀におけるJ2000エポックからの経過時間
$$
t = \frac{\mathit{JD}_1 - \mathit{JD}_2}{36525}
$$
ここで，$\mathit{JD}_1$は座標変換のエポック(Julian Date)．
$\mathit{JD}_2 = 2451545$は，J2000のエポック(Julian Date)．
タイムスケールはTT．
$36525$は，1ユリウス世紀の日数．

## IAU1976

- 一次文献
Lieske, J. H., Lederle, T., Fricke, W., and Morando, B., 1977, “Expressions for the precession quantities based upon the IAU (1976) System of Astronomical Constants,” Astron. Astrophys., 58(1-2), pp. 1–16.

- 定式化

$$
P(t) = R_z(-z_A)R_y(\theta_A)R_z(-\zeta_A)
$$

## IAU2000/2006

- 一次文献
Capitaine, N.,Wallace,P.T.,andChapront,J.,2003c,“ExpressionsforIAU2000 precession quantities,” Astron. Astrophys., 412(2), pp. 567–586, doi:10.1051/0004-6361:20031539

- 定式化

$$
P(t) = R_z(\chi_A)R_x(-\omega_A)R_z(\psi_A)R_x(\epsilon_0)
$$

## Fukushima(2003)

- 一次文献
Fukushima, T., 2003, “A new precession formula,” Astron. J., 126, pp. 494–534.

- 定式化

$$
P(t) = R_x(-\bar{\epsilon})R_z(-\bar{\psi})R_x(\bar{\phi})R_z(\bar{\gamma})
$$

## Precession angles(arcsecond)

以下のPrecession anglesは，

Hilton, J. & Capitaine, N & Chapront, J & Ferrándiz, José & Fienga, Agnès & Fukushima, Toshio & Getino, Juan & Mathews, P & Simon, J.-L & Soffel, Michael & Vondrak, Jan & Wallace, Patrick & Williams, James. (2006). Report of the International Astronomical Union Division I Working Group on Precession and the Ecliptic. Celestial Mechanics and Dynamical Astronomy. 94. 351-367. 10.1007/s10569-006-0001-2. 

TABLE 1より抜粋した．


| Angle                  | $1$            | $t$           | $t^2$        | $t^3$         | $t^4$                  | $t^5$                 |
| ---------------------- | -------------- | ------------- | ------------ | ------------- | ---------------------- | --------------------- |
| $z_A$                  | $−2.650545$    | $2306.077181$ | $1.0927348$  | $0.01826837$  | $−0.000028596$         | $−2.904\times10^{−7}$ |
| $\theta_A$             | -              | $2004.191903$ | $−0.4294934$ | $−0.04182264$ | $−7.089\times10^{−6} $ | $−1.274\times10^{−7}$ |
| $\zeta_A$              | $2.650545$     | $2306.083227$ | $0.2988499$  | $0.01801828$  | $−5.971\times10^{−6}$  | $−3.173\times10−^{7}$ |
| $\chi_A$               | -              | $10.556403$   | $−2.3814292$ | $−0.00121197$ | $0.000170663$          | $−5.60\times10^{−8}$  |
| $\omega_A$             | $\epsilon_0$   | $−0.025754$   | $0.0512623$  | $−0.00772503$ | $−4.67\times10^{−7}$   | $3.337\times10^{−7}$  |
| $\psi_A$               | -              | $5038.481507$ | $−1.0790069$ | $−0.00114045$ | $0.000132851$          | $−9.51\times10^{−8}$  |
| $\epsilon_A$           | $84381.406000$ | $−46.836769$  | $−0.0001831$ | $0.00200340$  | $−5.76\times10^{−7}$   | $−4.34\times10^{−8}$  |
| $\bar{\psi}_{J2000}$   | -              | $5038.481507$ | $1.5584176$  | $−0.00018522$ | $−0.000026452$         | $−1.48\times10^{-8}$  |
| $\bar{\phi}_{J2000}$   | $\epsilon_0$   | $−46.811015$  | $0.0511269$  | $0.00053289$  | $−4.40\times10^{−7}$   | $−1.76\times10^{−8}$  |
| $\bar{\gamma}_{J2000}$ | -              | $10.556403$   | $0.4932044$  | $−0.00031238$ | $−2.788\times10^{−6}$  | $2.60\times10^{−8}$   |
| $\bar{\psi}_{GCRS}$    | $−0.041775$    | $5038.481484$ | $1.5584175$  | $−0.00018522$ | $−0.000026452$         | $−1.48\times10^{−8}$  |
| $\bar{\phi}_{GCRS}$    | $84381.412819$ | $−46.811016$  | $0.0511268$  | $0.00053289$  | $−4.40\times10^{−7}$   | $−1.76\times10^{−8}$  |
| $\bar{\gamma}_{GCRS}$  | $0.052928$     | $10.556378$   | $0.4932044$  | $−0.00031238$ | $−2.788\times10^{−6}$  | $2.60\times10^{−8}$   |

ここで，$\epsilon_0 \equiv \epsilon_A(t = 0)$である．

