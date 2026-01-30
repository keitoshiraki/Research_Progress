# UE・SO（congestion）・SO（congestion＋CO₂）配分の比較結果

本節では、Sioux Falls ネットワークを対象として実施した  
**ユーザー均衡（User Equilibrium, UE）配分**、  
**混雑外部性を考慮したシステム最適（System Optimum, SO (congestion)）配分**、  
および **混雑外部性と CO₂ コストを同時に考慮したシステム最適（SO (congestion＋CO₂)）配分**  
の結果を比較する。



## 指標の定義

- **Total Flow**  
  ネットワーク全体におけるリンク交通量の総和  

$$
\sum_a x_a
$$

- **Total Travel Time (TTT)**  
  ネットワーク全体の総旅行時間  

$$
\sum_a x_a \, t_a(x_a)
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

- **Total CO₂ Cost**  
  ネットワーク全体の CO₂ 排出コスト  

$$
\sum_a x_a \; p_{\mathrm{CO2}} \; EF(v_a) \; \frac{L_a}{1000}
$$

- **CO₂ Cost per TotalFlow**  
  交通量 1 単位あたりの CO₂ コスト  

$$
\frac{\sum_a x_a \; p_{\mathrm{CO2}} \; EF(v_a) \; L_a / 1000}{\sum_a x_a}
$$



## 配分結果の比較

| 指標 | 数式 | UE | SO (congestion) | SO (cong+CO₂) |
|:--|:--|--:|--:|--:|
| Total Flow | $\sum_a x_a$ | 8.80×10^5 | 9.14×10^5 | 9.14×10^5 |
| Total Travel Time (TTT) | $\sum_a x_a\, t_a(x_a)$ | 4.50×10^6 | 4.37×10^6 | 4.37×10^6 |
| Total Marginal Cost (TMC) | $\sum_a x_a\left(t_a(x_a)+x_a\frac{dt_a}{dx_a}\right)$ | 1.43×10^7 | 1.32×10^7 | 1.32×10^7 |
| Congestion Externality (EXT) | $\sum_a x_a^2\frac{dt_a}{dx_a}$ | 9.78×10^6 | 8.87×10^6 | 8.85×10^6 |
| Average Travel Time | $\mathrm{TTT}/\sum_a x_a$ | 5.12 | 4.78 | 4.78 |
| EXT per TotalFlow | $\mathrm{EXT}/\sum_a x_a$ | 11.12 | 9.70 | 9.69 |
| Total CO₂ Cost (yen) | $\sum_a x_a \; p_{\mathrm{CO2}} \; EF(v_a)\; L_a/1000$ | 9.20×10^6 | 9.16×10^6 | 9.14×10^6 |
| CO₂ Cost per TotalFlow (yen) | $\mathrm{CO2}/\sum_a x_a$ | 10.45 | 10.01 | 10.01 |



## 考察

SO（congestion＋CO₂）配分では、混雑外部性に加えて
走行速度に依存する CO₂ 排出コストも考慮されるため、
交通流はより環境負荷の小さい状態へと誘導される。

具体的には、

- **総旅行時間（TTT）**は UE と比較して減少
- **混雑外部性（EXT）**は UE → SO（congestion）で大きく減少し，
  SO（congestion＋CO₂）ではさらにわずかに低下
- **CO₂総コスト**も SO（congestion＋CO₂）配分において最小

となっており、混雑外部性と環境外部性を同時に内部化することで、
ネットワーク全体の社会的費用が最小化されることが確認できる。

また、**平均旅行時間**も SO 配分の方が短く、
社会最適化が必ずしも利用者の移動利便性を損なうものではないことが示されている。

なお、本研究における **TotalFlow** は OD 需要そのものではなく、
配分結果として得られるリンク交通量の総和である。
SO 配分では交通が分散されるため、UE 配分より TotalFlow が
大きくなる場合があるが、これはネットワーク効率の改善と整合的である。

以上より、混雑外部性および CO₂ 外部性を考慮した SO 配分は、
UE 配分と比較してネットワーク全体の効率性と環境性能を同時に向上させることが確認された。

---

## SO(congestion+CO2)配分のコストの総量内訳

| Cost component               | Unit    | Value        |
|:-----------------------------|:--------|-------------:|
| Total Travel Time (TTT)      | veh·min | 4,370,000    |
| Congestion Externality (EXT) | veh·min | 8,850,000   |
| CO₂ Cost (converted)         | veh·min | 209,000    |
| Total Social Cost (TSC)      | veh·min | 13,400,000    |

