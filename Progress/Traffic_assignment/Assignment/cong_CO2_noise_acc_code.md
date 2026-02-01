# 混雑・CO₂・騒音・事故外部性を考慮した System Optimum 配分  
*(Sioux Falls Network, Logit-SUE + MSA)*

## 概要
本コードは、Sioux Falls ネットワークを対象として、  
**混雑・CO₂排出・騒音・交通事故**の外部性を考慮した  
**System Optimum（SO）交通量配分**を実装したものである。

配分手法には、

- **Logit 型 Stochastic User Equilibrium（Logit-SUE）**
- **Method of Successive Averages（MSA）**

を用いている。



## 対象ネットワーク・需要
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

### BPR 関数（実旅行時間）
リンク $a$ の実旅行時間は BPR 関数で与える：

$$
t_a(x_a) = t_{0,a}
\left(
1 + \alpha \left( \frac{x_a}{c_a} \right)^{\beta}
\right)
$$

---

### 社会限界旅行時間（混雑外部性）
System Optimum 配分では、混雑外部性を内部化するため  
**社会限界旅行時間**を用いる：

$$
t_a^{\text{mc}}(x_a) =
t_{0,a}
\left(
1 + \alpha (1+\beta)
\left( \frac{x_a}{c_a} \right)^{\beta}
\right)
$$

---

## 2. CO₂ 排出コスト

### 排出係数モデル
CO₂ 排出係数は **Barth & Boriboonsomsin（Real-World 条件）**に基づく  
速度依存モデルを用いる。

$$
\ln EF(v) = A_0 + A_1 v + A_2 v^2 + A_3 v^3 + A_4 v^4
$$

$$
EF(v) = \exp(\ln EF(v))
$$

ここで $v$ はリンク平均速度 [km/h] である。

---

### 速度の算出
速度は実旅行時間から次式で求める：

$$
v_a = \frac{60 \cdot L_a}{t_a(x_a)}
$$

---

### CO₂ コスト（一般化費用）
リンク $a$ における 1 台あたりの CO₂ 排出量：

$$
E_a = \frac{EF(v_a) \cdot L_a}{1000}
\quad [\text{kg/veh}]
$$

これを時間単位に変換した CO₂ コストは：

$$
c_a^{\text{CO₂}} = \lambda_{\text{CO₂}} \cdot E_a
$$

ただし，

$$
\lambda_{\text{CO₂}} = \frac{p_{\text{CO₂}}}{\text{VOT}}
\quad [\text{min/kg-CO₂}]
$$

である。

---

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

ここで：
- $u_{\text{noise}}$：単位距離あたりの騒音被害額 [yen/km]
- $S_a$：リンク $a$ の騒音影響指標
- $\bar{S}$：ネットワーク全体の平均騒音影響指標

---

## 4. 事故コスト

事故コストは、**リンク別に固定された外部コスト**として扱う。

### 事故損失額（時間当たり）
$$
C_a^{\text{acc,yen}} = M_D D_a + M_I I_a
$$

- $D_a$：死亡事故件数  
- $I_a$：負傷事故件数  

---

### 1 台あたり事故コスト
UE 配分で得られたリンク交通量 $Q_a^{\text{UE}}$ を用いて：

$$
c_a^{\text{acc}} =
\frac{C_a^{\text{acc,yen}}}{Q_a^{\text{UE}} \cdot \text{VOT}}
\quad [\text{min/veh}]
$$

---

## 5. 一般化費用（System Optimum）

SO 配分において使用するリンク一般化費用は：

$$
c_a =
t_a^{\text{mc}}(x_a)
+
c_a^{\text{CO₂}}
+
c_a^{\text{noise}}
+
c_a^{\text{acc}}
$$

---

## 6. 経路選択モデル（Logit）

各 OD ペアについて $k$ 本の最短経路を生成し，  
経路 $p$ の効用を次式で定義する：

$$
U_p = -\beta \sum_{a \in p} c_a
$$

経路選択確率は Logit モデルにより：

$$
P_p =
\frac{\exp(\mu U_p)}
{\sum_{q} \exp(\mu U_q)}
$$

---

## 7. フロー更新（MSA）

Logit により得られた経路流量からリンク流量 $\hat{x}_a^{(n)}$ を計算し，  
MSA により次式で更新する：

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

## 出力結果
最終的に，各リンクについて以下を出力する：

- 配分交通量 $x_a$
- 実旅行時間 $t_a(x_a)$
- 社会限界旅行時間 $t_a^{\text{mc}}(x_a)$
- CO₂ コスト $c_a^{\text{CO₂}}$
- 騒音コスト $c_a^{\text{noise}}$
- 事故コスト $c_a^{\text{acc}}$

出力ファイル：

