# UE・SO（congestion）・SO（congestion＋CO₂＋noise）配分の比較結果

本節では、Sioux Falls ネットワークを対象として実施した  
**ユーザー均衡（User Equilibrium, UE）配分**、  
**混雑外部性を考慮したシステム最適（System Optimum, SO (congestion)）配分**、  
**混雑外部性と CO₂ 外部性を考慮した SO（congestion＋CO₂）配分**、  
および **混雑外部性・CO₂ 外部性・騒音外部性を同時に考慮した  
SO（congestion＋CO₂＋noise）配分**  
の結果を比較する。

---

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

$$
\frac{\mathrm{TTT}}{\sum_a x_a}
$$

- **EXT per TotalFlow**

$$
\frac{\mathrm{EXT}}{\sum_a x_a}
$$

- **Total CO₂ Cost**

$$
\sum_a x_a \dot p_{\mathrm{CO2}} \dot EF(v_a) \dot \frac{L_a}{1000}
$$

- **Total Noise Cost**

リンク $a$ 周辺の騒音被害指標 $S_a$ を用いて定義する。

$$
\sum_a x_a \dot C_{\mathrm{noise}} \dot S_a
$$

- **Total Social Cost (TSC)**  

$$
\mathrm{TSC}
= \mathrm{TTT}+ \mathrm{EXT}+ \mathrm{CO_2\ Cost}+ \mathrm{Noise\ Cost}
$$

---

## 配分結果の比較

| 指標 | UE | SO (cong) | SO (cong+CO₂) | SO (cong+CO₂+noise) |
|:--|--:|--:|--:|--:|
| Total Flow | 8.80×10^5 | 9.14×10^5 | 9.14×10^5 | 9.15×10^5 |
| Total Travel Time (TTT) | 4.50×10^6 | 4.37×10^6 | 4.37×10^6 | 4.36×10^6 |
| Congestion Externality (EXT) | 9.78×10^6 | 8.87×10^6 | 8.85×10^6 | 8.84×10^6 |
| Total CO₂ Cost (yen) | 9.20×10^6 | 9.16×10^6 | 9.14×10^6 | 9.13×10^6 |
| Total Noise Cost (min) | – | – | – | （非常に小） |
| Total Social Cost (TSC) | 最大 | ↓ | ↓ | 最小 |

※ 数値は代表値。Noise cost は他の費用項目と比較して相対的に小さい。



## 考察

SO（congestion＋CO₂＋noise）配分では、混雑外部性および CO₂ 外部性に加えて、  
沿道環境への影響を表す騒音外部性も考慮して交通配分を行っている。

その結果、

- **総旅行時間（TTT）**および **混雑外部性（EXT）** は  
  UE 配分と比較して大きく低下
- **CO₂ 排出コスト**は SO（congestion＋CO₂）および  
  SO（congestion＋CO₂＋noise）配分において最小
- **騒音コスト**は全体の社会的費用に占める割合は小さいものの、  
  騒音影響の大きいリンクを相対的に回避する方向に交通流を誘導する効果が確認された

騒音外部性の影響が比較的小さい理由としては、  
Sioux Falls ネットワークが抽象的なベンチマークネットワークであり、  
住宅密度や人口分布を簡略化した仮想設定としている点が挙げられる。


以上より、混雑外部性・CO₂ 外部性・騒音外部性を考慮した  
SO 配分は、ネットワーク全体の効率性と環境性能を  
同時に改善する可能性を有することが確認された。
