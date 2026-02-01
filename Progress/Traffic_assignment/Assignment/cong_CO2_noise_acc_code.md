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

---

# System Optimum Traffic Assignment with Congestion, CO₂, Noise, and Accident Externalities  
*(Logit-SUE + MSA, Sioux Falls Network)*

## Overview
This code implements a **System Optimum (SO) traffic assignment** on the Sioux Falls network,  
explicitly accounting for externalities arising from **congestion, CO₂ emissions, noise, and traffic accidents**.

The assignment is solved using:

- **Logit-based Stochastic User Equilibrium (Logit-SUE)**
- **Method of Successive Averages (MSA)**

A key feature of this implementation is that **travel time and congestion externality are explicitly separated in the formulation**.

---

## Data
- **Network**: Sioux Falls  
- **OD demand**: fixed OD table  
- **Link attributes**:
  - Free-flow travel time $t_{0,a}$ [min]
  - Capacity $c_a$ [veh]
  - Link length $L_a$ [km]
  - Noise exposure indicator $S_a$
  - Accident cost $c_a^{\text{acc}}$ [min/veh]



## 1. Travel Time and Congestion Externality

### Realized Travel Time (BPR Function)
The realized travel time on link $a$ is given by the BPR function:

$$
t_a(x_a) =
t_{0,a}
\left(
1 + \alpha \left( \frac{x_a}{c_a} \right)^{\beta}
\right)
$$



### Social Marginal Travel Time
The social marginal travel time is defined as:

$$
t_a^{\text{mc}}(x_a) =
t_{0,a}
\left(
1 + \alpha (1+\beta)
\left( \frac{x_a}{c_a} \right)^{\beta}
\right)
$$



### Congestion Externality
The congestion externality is defined as the difference between the social marginal travel time and the realized travel time:

$$
\text{Congestion Externality}_a =
t_a^{\text{mc}}(x_a) - t_a(x_a)=
x_a \frac{dt_a}{dx_a}
$$

In this code, **travel time and congestion externality are computed and treated separately**.



## 2. CO₂ Emission Cost

### Emission Factor Model
The CO₂ emission factor is based on the **speed-dependent real-world model by Barth & Boriboonsomsin**:

$$
\ln EF(v) = A_0 + A_1 v + A_2 v^2 + A_3 v^3 + A_4 v^4
$$

$$
EF(v) = \exp(\ln EF(v))
$$



### Speed Calculation
The average link speed is calculated from the **realized travel time** as:

$$
v_a = \frac{60 \cdot L_a}{t_a(x_a)}
$$



### CO₂ Cost
The CO₂ emissions per vehicle on link $a$ are given by:

$$
E_a = \frac{EF(v_a) \cdot L_a}{1000}
\quad [\text{kg/veh}]
$$

The CO₂ cost in time units is defined as:

$$
c_a^{\text{CO₂}} = \lambda_{\text{CO₂}} \cdot E_a,
\quad
\lambda_{\text{CO₂}} = \frac{p_{\text{CO₂}}}{\text{VOT}}
$$



## 3. Noise Cost
The noise cost is defined based on link length and relative noise exposure:

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

where $\bar{S}$ denotes the network-wide average noise exposure indicator.



## 4. Accident Cost
Accident cost is treated as a link-specific external cost.

### Accident Damage Cost
$$
C_a^{\text{acc,yen}} = M_D D_a + M_I I_a
$$



### Accident Cost per Vehicle
Using the link traffic volume obtained from the UE assignment $Q_a^{\text{UE}}$:

$$
c_a^{\text{acc}} =
\frac{C_a^{\text{acc,yen}}}
{Q_a^{\text{UE}} \cdot \text{VOT}}
\quad [\text{min/veh}]
$$



## 5. Generalized Cost (System Optimum)

The generalized link cost used in this code is defined as:

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

That is, the generalized cost internalizes:

- **Travel time**
- **Congestion externality**
- **CO₂ emissions**
- **Noise**
- **Accidents**

resulting in a System Optimum assignment.



## 6. Route Choice Model (Logit)
For each OD pair, $k$ shortest paths are generated.  
The utility of path $p$ is defined as:

$$
U_p = -\beta \sum_{a \in p} c_a
$$

The path choice probability is given by:

$$
P_p =
\frac{\exp(\mu U_p)}
{\sum_{q} \exp(\mu U_q)}
$$



## 7. Flow Update (MSA)
Link flows are updated using the MSA scheme:

$$
x_a^{(n+1)} =
x_a^{(n)} +
\frac{1}{n}
\left(
\hat{x}_a^{(n)} - x_a^{(n)}
\right)
$$

The convergence criterion is defined as:

$$
\frac{\sum_a |x_a^{(n+1)} - x_a^{(n)}|}
{\sum_a |x_a^{(n)}|}
< \varepsilon
$$

---

## Output
For each link, the following quantities are reported:

- Traffic flow $x_a$
- Realized travel time $t_a(x_a)$
- Social marginal travel time $t_a^{\text{mc}}(x_a)$
- Congestion externality $t_a^{\text{mc}}(x_a) - t_a(x_a)$
- CO₂ cost $c_a^{\text{CO₂}}$
- Noise cost $c_a^{\text{noise}}$
- Accident cost $c_a^{\text{acc}}$
- Generalized cost $c_a$

Output file:
