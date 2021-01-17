# EME2000⇔Mean of Date(MOD)座標変換

## ー歳差(precession)行列の求め方ー

---

このノートは全編に渡って，下記のWebサイトとそこからダウンロードできるSOFAのソースコードを参考にした．
https://www.iausofa.org/

### 座標系の説明

- EME2000(Earth mean equator and equinox of J2000)
J2000.0の平均赤道面座標系
  - 平均春分点
  - J2000における平均赤道(歳差:未考慮，章動:未考慮)
- Mean of Date(MOD)
瞬時の平均赤道面座標系
  - 平均春分点
  - 瞬時の平均赤道(歳差:考慮，章動:未考慮)

EME2000とMean of Dateの違いは，地球の歳差(precession)を考慮するか否かである．
したがって，歳差行列$P(t)$を求めることで，以下のようにEME2000とMean of Dateの相互座標変換ができる．

$$
\begin{aligned}
\vec{r}_{MOD} &= \vec{r}_{2000}P(t) \\
\vec{v}_{MOD} &= \vec{v}_{2000}P(t) \\
\vec{r}_{2000} &= \vec{r}_{MOD}P(t)^{\mathrm{T}} \\
\vec{v}_{2000} &= \vec{v}_{MOD}P(t)^{\mathrm{T}}
\end{aligned}
$$

ここで、$\vec{r}$，$\vec{v}$は，位置・速度ベクトルであり，後にプログラミング言語で実装しやすいように行ベクトルとする．

$\vec{r} = (r_x \; r_y \; r_z)$
$\vec{v} = (v_x \; v_y \; v_z)$

したがって，座標変換行列(ここでは歳差行列$P(t)$)は，右からかける．(Green Book等は，列ベクトルで表記されていることに注意!)
なお下付き文字で座標系を示す．

以下では，この座標変換を行うための歳差行列$P(t)$を求める．

---

### 準備

- ３次元の回転行列(rotation matrix)

$R_x(\theta) = \left(
        \begin{array}{ccc}
            1 & 0 & 0 \\
            0 & \cos{\theta} & -\sin{\theta} \\
            0 & \sin{\theta} & \cos{\theta}
        \end{array}
    \right)$

$R_y(\theta) = \left(
        \begin{array}{ccc}
            \cos{\theta} & 0 & \sin{\theta} \\
            0 & 1 & 0 \\
            -\sin{\theta} & 0 & \cos{\theta}
        \end{array}
    \right)$

$R_z(\theta) = \left(
        \begin{array}{ccc}
            \cos{\theta} & -\sin{\theta} & 0 \\
            \sin{\theta} & \cos{\theta} & 0 \\
            0 & 0 & 1
        \end{array}
    \right)$

---

### 歳差行列の定式化

$$
P(t) = R_z(-z)R_y(\theta)R_z(-\zeta)
$$

ここで，$z$，$\theta$，$\zeta$は，Fukushima-Williams methodを用いて得られる歳差行列のオイラー角であり，フレームバイアス(?)を含む．

---

### $z$，$\theta$，$\zeta$の求め方

1. Fukushima-Williams angle($\bar{\gamma}, \bar{\phi}, \bar{\psi}, \epsilon_A$)を求める．

2. Fukushima-Williams angleからprecession-bias matrix($R$)を求める．

3. $R$から$z$，$\theta$，$\zeta$を求める．

---

#### 1.Fukushima-Williams angle($\bar{\gamma}, \bar{\phi}, \bar{\psi}, \epsilon_A$)を求める．

##### 準備

$\mathit{JD}_1$をエポックのJulian Dateとし，$\mathit{JD}_2$をJ2000すなわち2000/1/1 12:00:00のJulian Dateとする．なお両者ともタイムスケールはTT(Terrestrial Time)である．
このとき，$t$を以下のように定める．
$$
t = \frac{\mathit{JD}_1 - \mathit{JD}_2}{36525}
$$

なお，$\mathit{JD}_2 = 2451545$である．また$36525$は1ユリウス世紀の日数を表している．

##### Fukushima-Williams angle

Fukushima-Williams angle($\bar{\gamma}, \bar{\phi}, \bar{\psi}, \epsilon_A$)は以下のように定められる．単位は秒(arcsecond)．
$$
\begin{aligned}
\bar{\gamma} &= -0.052928 + 10.556378t + 0.4932044t^2 + -0.00031238t^3 + -0.000002788t^4 +
                0.0000000260t^5 \\
\bar{\phi} &= 84381.412819 + -46.811016t + 0.0511268t^2 + 0.00053289t^3 + -0.000000440t^4 +
                -0.0000000176t^5 \\
\bar{\psi} &=  -0.041775 + 5038.481484t + 1.5584175t^2 + -0.00018522t^3 +  -0.000026452t^4 +
                -0.0000000148t^5　\\
\epsilon_A &= 84381.406 + -46.836769t + -0.0001831t^2 + 0.00200340t^3 +  -0.000000576t^4 +
                -0.0000000434t^5
\end{aligned}
$$

なお，以降の計算で使用する際には，単位はradianにしたいので，それぞれの角度に，$4.848136811095359935899141 \times 10^{-6}$をかけて，単位をarcsecondからradianに変換しておく．

$\epsilon_A$は，黄道の平均傾斜角．他はちょっとまだわからない．．

#### 2. Fukushima-Williams angleからprecession-bias matrix($R$)を求める．

precession-bias matrix($R$)は，Fukushima-Williams angle($\bar{\gamma}, \bar{\phi}, \bar{\psi}, \epsilon_A$)を用いて，以下のように求めることができる．

$$
R = R_x(-\epsilon_A)^{\mathrm{T}}R_z(-\bar{\psi})^{\mathrm{T}}R_x(\bar{\phi})^{\mathrm{T}}R_z(\bar{\gamma})^{\mathrm{T}}
    \left(
        \begin{array}{ccc}
            1 & 0 & 0 \\
            0 & 1 & 0 \\
            0 & 0 & 1
        \end{array}
    \right)
$$

#### 3. precession-bias matrix($R$)から$z$，$\theta$，$\zeta$を求める．

まず，$x_1$，$y_1$を以下とする．

$$
y_1 = \begin{cases}
        R_{23} & (-R_{13} >= 0) \\
        -R_{23} & (otherwise)
    \end{cases}
$$
$$
x_1 = \begin{cases}
        -R_{13} & (-R_{13} >= 0) \\
        R_{13} & (otherwise)
    \end{cases}
$$

このとき，

$$
z = \begin{cases}
        -atan2(y_1, x_1) & (x_1 \neq 0 \; or \; y_1 \neq 0) \\
        0 & (otherwise)
    \end{cases}
$$

である．

次に，求めた角度$z$で$R$を逆回転する．

$$
R' = R_z(z)^{\mathrm{T}}R
$$

求めた$R'$から，

$$
\begin{aligned}
    y_2 &= R'_{13} \\
    x_2 &= R'_{33}
\end{aligned}
$$

とおくと，

$$
\theta = \begin{cases}
        -atan2(y_2, x_2) & (x_2 \neq 0 \; or \; y_2 \neq 0) \\
        0 & (otherwise)
    \end{cases}
$$

となる．

また，
$$
\begin{aligned}
    y_3 &= -R'_{21} \\
    x_3 &= R'_{22}
\end{aligned}
$$

とおくと，

$$
\zeta = \begin{cases}
        -atan2(y_3, x_3) & (x_3 \neq 0 \; or \; y_3 \neq 0) \\
        0 & (otherwise)
    \end{cases}
$$

となる．

以上で，歳差行列のオイラー角$z$，$\theta$，$\zeta$が求まった．

---

### 付録:前提知識なしの人(自社の社員)に向けた今回の座標変換の説明

■今週の座標変換
EME2000⇔Mean of Date

人工衛星の軌道計算では，様々な座標系を用います．
慣性座標系は，地球の外側から見た衛星の位置を記述するもので，地球の外側をまわる人工衛星の位置を記述するには，慣性座標系が便利です．
一方で，地球上から空を見上げたときに，衛星がどの位置にいるかを記述するためには，つまり望遠鏡を空のどこに向ければその衛星が見えるのかを知るためには，地球の自転と共に座標軸も回転する地球固定座標系が必要です．

例えば，静止衛星の位置は，慣性座標系で記述すると，地球の周りをぐるぐると回っている(24時間で地球を一周する)ように見えますが，地球固定座標系では，地球の自転とともに座標軸も回転しているため，静止衛星はいつも同じ位置にあって動かないように見えます．つまり，いつも同じところに望遠鏡を向ければ，同じ場所に静止衛星は見えます．

慣性座標系から地球固定座標系への変換は，次のように行います．

【慣性座標系】→(歳差の考慮)→(章動の考慮)→(地球の自転の考慮)→(極運動の考慮)→【地球固定座標系】

今週の座標変換では，まず1番最初の(歳差の考慮)の部分を考えます．

スタート地点の慣性座標系は，EME2000(Earth mean equator and equinox of J2000)といい，
地球の中心を原点とした赤道面座標系で，エポックがJ2000，すなわち2000/1/1 12:00:00の時点での赤道面をxy平面とし，それに対して90度北向きをz軸とした右手系の座標系です．
「2000/1/1 12:00:00の時点での赤道面」という言い方をしたのはなぜかと言うと，地球の赤道面は時間とともに変化しているので，いつの時点での赤道面かを指定してあげないといけないからです．

地球の赤道面が時間とともに変化するのは，地球の自転軸が，傾いたコマのように首振り運動をしているためです．この首振り運動は，歳差と章動に分けることができます．そして，歳差と章動を考慮すると，指定した日時(エポック)の赤道面を計算することができます．

そこで，今週の座標変換では，まず「歳差の考慮」を行います．
EME2000に対して，歳差を考慮した座標系が「Mean of Date(瞬時の平均赤道面座標系)」です．
つまり，
【EME2000】→(歳差の考慮)→【Mean of Date】
が，今週の座標変換です．
