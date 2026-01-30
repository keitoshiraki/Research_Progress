# UE と SO（congestion）配分の比較結果

本節では、Sioux Falls ネットワークを対象として実施した  
**ユーザー均衡（User Equilibrium, UE）配分**と  
**混雑外部性を考慮したシステム最適（System Optimum, SO (congestion)）配分**  
の結果を比較する。



### 指標の定義

- **TotalFlow**  
  ネットワーク全体におけるリンク交通量の総和
  
$$
\sum_a x_a
$$

- **Total Travel Time (TTT)**  
  ネットワーク全体の総旅行時間  

$$
\sum_a x_a \cdot t_a(x_a)
$$

- **Total Marginal Cost (TMC)**  
  社会的限界費用の総和  

$$
\sum_a x_a \left( t_a(x_a) + x_a \frac{dt_a}{dx_a} \right)
$$

- **Congestion Externality (EXT)**  
  混雑外部性（限界費用と私的費用の差）

$$
\sum_a x_a^2 \frac{dt_a}{dx_a}
= \mathrm{TMC} - \mathrm{TTT}
$$

- **Average Travel Time**  
  平均旅行時間

$$
\frac{\mathrm{TTT}}{\sum_a x_a}
$$

- **EXT per TotalFlow**  
  交通量 1 単位あたりの混雑外部性  

$$
\frac{\mathrm{EXT}}{\sum_a x_a}
$$



### 配分結果の比較

| 指標 | 数式 | UE | SO (congestion) | SO − UE | SO / UE |
|:--|:--|--:|--:|--:|--:|
| TotalFlow | $\sum_a x_a$ | 879,824.00 | 914,208.26 | 34,384.26 | 1.04 |
| Total Travel Time (TTT) | $\sum_a x_a\,t_a(x_a)$ | 4.502e+06 | 4.371e+06 | -130,920.48 | 0.97 |
| Total Marginal Cost (TMC) | $\sum_a x_a\left(t_a(x_a)+x_a\frac{dt_a}{dx_a}\right)$ | 1.428e+07 | 1.324e+07 | -1.044e+06 | 0.93 |
| Congestion Externality (EXT) | $\sum_a x_a^2\frac{dt_a}{dx_a}=\mathrm{TMC}-\mathrm{TTT}$ | 9.780e+06 | 8.866e+06 | -913,406.61 | 0.91 |
| Average Travel Time | $\mathrm{TTT}/\sum_a x_a$ | 5.12 | 4.78 | -0.34 | 0.93 |
| EXT per TotalFlow | $\mathrm{EXT}/\sum_a x_a$ | 11.12 | 9.70 | -1.42 | 0.87 |



### 考察

SO（congestion）配分では、混雑外部性を内部化した社会的限界費用に基づいて
経路選択が行われるため、UE 配分と比較してネットワーク全体の効率性が向上している。

具体的には、

- **総旅行時間（TTT）**は約 **3% 減少**
- **混雑外部性（EXT）**は約 **9% 減少**
- **総限界費用（TMC）**は約 **7% 減少**

しており、混雑外部性の内部化による効果が明確に確認できる。

また、**平均旅行時間**も SO 配分の方が短くなっており、
社会最適化が必ずしも個人の移動効用を悪化させるものではないことが示されている。

なお、本研究における **TotalFlow** は OD 需要そのものではなく、
配分結果として得られるリンク交通量の総和である。
SO 配分では交通がより分散されるため、TotalFlow が UE 配分より
大きくなる場合があるが、これはネットワーク効率の改善と整合的である。

以上より、混雑外部性を考慮した SO 配分は、
UE 配分と比較してネットワーク全体の効率性を向上させることが確認された。

