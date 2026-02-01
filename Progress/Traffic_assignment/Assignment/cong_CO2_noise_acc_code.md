# System Optimum Traffic Assignment with Congestion, CO₂, Noise, and Accident Externalities  
*(Logit-SUE + MSA, Sioux Falls Network)*

## 概要
本コードは、Sioux Falls ネットワークを対象として、  
**混雑・CO₂排出・騒音・交通事故**の外部性をすべて考慮した  
**System Optimum（SO）交通量配分**を実装したものである。

配分には、

- **Logit 型 Stochastic User Equilibrium（Logit-SUE）**
- **Method of Successive Averages（MSA）**

を用いている。

本実装の特徴は、**旅行時間と混雑外部性を明示的に分離して定式化している点**にある。



## 使用データ
- **ネットワーク**：Sioux Falls  
- **OD 需要**：固定 OD 表  
- **リンク属性**：
  - 自由流旅行時間 $t_{0,a}$ [min]
  - 容量 $c_a$ [veh]
  - リンク長 $L_a$ [km]
  - 騒音影響指標 $S_a$
  - 事故コスト $c_a^{\text{acc}}$ [min/veh]

---

## 1. 旅行時間と混雑外部性

### 実旅行時間（BPR 関数）
リンク $a$ の実旅行時間は BPR 関数で与える：

$$
t_a(x_a) =
t_{0,a}
\left(
1 + \alpha \left( \frac{x_a}{c_a} \right)^{\beta}
\right)
$$



### 社会限界旅行時間
社会限界旅行時間は次式で与えられる：

$$
t_a^{\text{mc}}(x_a) =
t_{0,a}
\left(
1 + \alpha (1+\beta)
\left( \frac{x_a}{c_a} \right)^{\beta}
\right)
$$



### 混雑外部性
混雑外部性は、社会限界旅行時間と実旅行時間の差として定義する：

$$
\text{Congestion Externality}_a=
t_a^{\text{mc}}(x_a) - t_a(x_a)=
x_a \frac{dt_a}{dx_a}
$$

本コードでは、**旅行時間と混雑外部性を明確に分離**して計算している。



## 2. CO₂ 排出コスト

### 排出係数モデル
CO₂ 排出係数には、  
**Barth & Boriboonsomsin（Real-World 条件）**に基づく速度依存モデルを用いる。

$$
\ln EF(v) = A_0 + A_1 v + A_2 v^2 + A_3 v^3 + A_4 v^4
$$

$$
EF(v) = \exp(\ln EF(v))
$$



### 速度の算出
リンク平均速度は、**実旅行時間**から次式で求める：

$$
v_a = \frac{60 \cdot L_a}{t_a(x_a)}
$$



### CO₂ コスト
リンク $a$ における 1 台あたり CO₂ 排出量：

$$
E_a = \frac{EF(v_a) \cdot L_a}{1000}
\quad [\text{kg/veh}]
$$

CO₂ コスト（時間単位）は：

$$
c_a^{\text{CO₂}} = \lambda_{\text{CO₂}} \cdot E_a,
\quad
\lambda_{\text{CO₂}} = \frac{p_{\text{CO₂}}}{\text{VOT}}
$$



## 3. 騒音コスト
騒音コストは、リンク長と相対的な騒音影響指標に基づき定義する：

$$
c_a^{\text{noise}} =
\lambda_{\text{yen}\to\text{min}}
\cdot
u_{\text{noise}}
\cdot
L_a
\cdot
\frac{S_a}{\bar{S}}
$$

ここで $\bar{S}$ はネットワーク全体の平均騒音影響指標である。



## 4. 事故コスト
事故コストはリンク固有の外部コストとして与える。

### 事故損失額
$$
C_a^{\text{acc,yen}} = M_D D_a + M_I I_a
$$

### 1 台あたり事故コスト
UE 配分で得られた交通量 $Q_a^{\text{UE}}$ を用いて：

$$
c_a^{\text{acc}} =
\frac{C_a^{\text{acc,yen}}}
{Q_a^{\text{UE}} \cdot \text{VOT}}
\quad [\text{min/veh}]
$$



## 5. 一般化費用（System Optimum）

本コードで用いるリンク一般化費用は次式で与えられる：

$$
c_a =
t_a(x_a)
+
\left(
t_a^{\text{mc}}(x_a) - t_a(x_a)
\right)
+
c_a^{\text{CO₂}}
+
c_a^{\text{noise}}
+
c_a^{\text{acc}}
$$

すなわち、

- **旅行時間**
- **混雑外部性**
- **CO₂**
- **騒音**
- **事故**

をすべて内部化した System Optimum 配分である。



## 6. 経路選択モデル（Logit）
各 OD ペアについて $k$ 本の最短経路を生成し，  
経路 $p$ の効用を次式で定義する：

$$
U_p = -\beta \sum_{a \in p} c_a
$$

経路選択確率は：

$$
P_p =
\frac{\exp(\mu U_p)}
{\sum_{q} \exp(\mu U_q)}
$$



## 7. フロー更新（MSA）
リンク交通量は MSA により次式で更新する：

$$
x_a^{(n+1)} =
x_a^{(n)} +
\frac{1}{n}
\left(
\hat{x}_a^{(n)} - x_a^{(n)}
\right)
$$

収束判定条件は：

$$
\frac{\sum_a |x_a^{(n+1)} - x_a^{(n)}|}
{\sum_a |x_a^{(n)}|}
< \varepsilon
$$

---

## 出力
最終的に，各リンクについて以下を出力する：

- 交通量 $x_a$
- 実旅行時間 $t_a(x_a)$
- 社会限界旅行時間 $t_a^{\text{mc}}(x_a)$
- 混雑外部性 $t_a^{\text{mc}}(x_a) - t_a(x_a)$
- CO₂ コスト $c_a^{\text{CO₂}}$
- 騒音コスト $c_a^{\text{noise}}$
- 事故コスト $c_a^{\text{acc}}$
- 一般化費用 $c_a$

出力ファイル：
