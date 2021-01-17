# Mean of Date⇔True equator of Date座標変換

## ー章動(nutation)行列の求め方ー

---

このノートは全編に渡って，下記のWebサイトとそこからダウンロードできるSOFAのソースコードを参考にした．
https://www.iausofa.org/

### Mean of Date ⇔ True of Dateの相互座標変換

Mean of DateとTrue equator of Dateの違いは，地球の章動(nutation)を考慮するか否かである．
したがって，章動行列$N(t)$を求めることで，以下のようにMean of Date(MOD)とTrue equator of Date(TOD)の相互座標変換ができる．

$$
\begin{aligned}
\vec{r}_{TOD} &= \vec{r}_{MOD}N(t) \\
\vec{v}_{TOD} &= \vec{v}_{MOD}N(t) \\
\vec{r}_{MOD} &= \vec{r}_{TOD}N(t)^{\mathrm{T}} \\
\vec{v}_{MOD} &= \vec{v}_{TOD}N(t)^{\mathrm{T}}
\end{aligned}
$$

ここで、$\vec{r}$，$\vec{v}$は，位置・速度ベクトルであり，後にプログラミング言語で実装しやすいように行ベクトルとする．

$\vec{r} = (r_x \; r_y \; r_z)$
$\vec{v} = (v_x \; v_y \; v_z)$

したがって，座標変換行列(ここでは章動行列$N(t)$)は，右からかける．(Green Book等は，列ベクトルで表記されていることに注意!)
なお下付き文字で座標系を示している．

以下では，この座標変換を行うための章動行列$N(t)$を求める．

### 準備(回転座標変換行列)

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


### 章動行列の定式化

$$
N(t) = R_x(-\bar{\epsilon})
$$

章動の計算は，黄道面に対して行う．
そのため，一度，赤道面座標系から黄道面座標系に変換し，章動を計算した後，再度，黄道面座標系から赤道面座標系へ戻す，という手順となる．

**Mean** **of** **Date** (瞬時の平均赤道面座標系)
↓(Step.1 平均赤道面から平均黄道面へ)
**Mean** **ecliptic** **of** **Date** (瞬時の平均黄道面座標系)
↓(Step.2 章動の考慮)
**True** **ecliptic** **of** **Date** (瞬時の真黄道面座標系)
↓(Step.3 真黄道面から真赤道面へ)
**True** **equator** **of** **Date** (瞬時の真赤道面座標系)






### 章動(precession)行列の定式化

### Step.1 Mean of Date ⇔ Mean ecliptic Date


