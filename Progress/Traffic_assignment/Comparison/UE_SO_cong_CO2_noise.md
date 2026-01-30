# UE・SO（congestion）・SO（congestion＋CO₂）・SO（congestion＋CO₂＋Noise）配分結果の比較  
*(Sioux Falls Network)*

## 1. 背景と目的

本節では，Sioux Falls ネットワークを対象として実施した以下の交通配分結果を比較する。

- **ユーザー均衡（User Equilibrium, UE）配分**
- **混雑外部性を考慮したシステム最適（SO (congestion)）配分**
- **混雑外部性・CO₂外部性・騒音外部性を同時に考慮した  
  システム最適（SO (congestion＋CO₂＋Noise)）配分**




## 3. 評価指標の定義

本比較では，以下のネットワーク集計指標を用いる。

### 3.1 Total Flow

ネットワーク全体のリンク交通量の総和：

$$
\text{Total Flow} = \sum_a x_a
$$



### 3.2 Total Travel Time (TTT)

リンク交通量と旅行時間の積の総和：

$$
\text{TTT} = \sum_a x_a \cdot t_a(x_a)
$$



### 3.3 Congestion Externality

混雑外部性による追加的社会的費用：

$$
\text{EXT}_{cong}
= \sum_a x_a^2 \frac{dt_a(x_a)}{dx_a}
$$



### 3.4 CO₂ Cost

リンク別 CO₂ 排出量を炭素価格で貨幣換算し，
旅行時間単位に変換したコストの総和。



### 3.5 Noise Cost

リンク交通に伴う騒音影響を，
リンク周辺建物数および距離減衰を考慮して算出した騒音コストの総和。



### 3.6 Total Social Cost (TSC)

全ての費用要素を合算した社会的総費用：

$$
\text{TSC}=\text{TTT}+ \text{EXT}_{cong}+ \text{CO}_2 \text{ Cost}+ \text{Noise Cost}
$$



## 4. 配分結果の比較

以下に，各配分ケースにおける評価指標の比較結果を示す。

## 配分結果の比較表（Sioux Falls）


| Indicator | UE | SO (congestion) | SO (cong + CO₂) | SO (cong + CO₂ + Noise) |
|---|---:|---:|---:|---:|
| **Total Flow** | 879824.0 | 914208.3 | 913680.7 | 914062.8 |
| **Total Travel Time (TTT)** [veh·min] | 4501546.0 | 4370625.0 | 4365611.0 | 4364273.0 |
| **Total Marginal Cost (TMC)** [veh·min] | 14281250.0 | 13236920.0 | 13220410.0 | 13220630.0 |
| **Congestion Externality (EXT)** [veh·min] | 9779705.0 | 8866298.0 | 8854795.0 | 8856354.0 |
| **Average Travel Time** [min/veh] | 5.116416 | 4.780777 | 4.778049 | 4.774588 |
| **EXT per Total Flow** [min/veh] | 11.11552 | 9.698335 | 9.691345 | 9.689000 |
| **Total CO₂ Cost** [veh·min] | 210233.6 | 209319.2 | 209030.2 | 208894.3 |
| **CO₂ Cost per Total Flow** [min/veh] | 0.2389496 | 0.2289623 | 0.2287782 | 0.2285339 |
| **Total Noise Cost** [veh·min] | 244624.7 | 246765.3 | 246344.2 | 245235.3 |
| **Noise Cost per Total Flow** [min/veh] | 0.2780383 | 0.2699225 | 0.2696174 | 0.2682915 |
| **Total Social Cost (TSC)** [veh·min] | 14736110.0 | 13693010.0 | 13675780.0 | 13674760.0 |







## 5. 考察


### 5.2 CO₂・騒音外部性の追加効果

SO（congestion＋CO₂＋Noise）配分では，

周辺建物への影響が大きいリンク

の一般化費用が相対的に増加するため，
交通流は **騒音コストの小さいリンクへ再配分** される。

その結果，

 Noise Cost の低減


が生じることが確認できる。


