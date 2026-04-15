# Estimation of Emission Factor Function Using CAN Data

## 1. Objective

The objective of this analysis is to estimate a function that describes the relationship between vehicle speed and CO₂ emission factors using CAN data.

To incorporate CO₂ emissions into traffic assignment, emission factors must be assigned to each link based on its travel speed. Therefore, this study first constructs a speed-based emission factor table and then formulates it as a continuous function.



## 2. Data and Preprocessing

Vehicle speed and fuel consumption data were obtained from CAN data and aggregated into speed bins to construct a **speed-based emission factor table**.

The table includes:

- Number of observations $n$  
- Emission factor $EF$ $[g/km]$  
- Log-transformed emission factor $\ln(EF)$  

However, unrealistic values such as 200 km/h and 999 km/h were observed, likely due to GPS errors or data anomalies.

The following preprocessing steps were applied:

1. Remove observations with missing $\ln(EF)$  
2. Restrict speed range to **1–130 km/h**  

A total of 130 bins were used for estimation.



## 3. Model Specification

A fourth-order polynomial model was adopted:

$$
\ln(EF(v)) = A_0 + A_1 v + A_2 v^2 + A_3 v^3 + A_4 v^4
$$


## 4. Estimation Method

Since each bin represents an average, its reliability depends on the number of observations.

To account for this, **Weighted Least Squares (WLS)** was applied:

$$
\sum_i w_i (y_i - \hat{y}_i)^2
$$

where:

- $y_i = \ln(EF_i)$  
- $\hat{y}_i$ = predicted value  
- $w_i = n_i$  


## 5. Results

Estimated function:

$$
\ln(EF(v)) =5.0978- 0.0863 v+ 0.001658 v^2- 1.493 \times 10^{-5} v^3+ 5.353 \times 10^{-8} v^4
$$

### Parameters

| Parameter | Value |
|----------|------|
| $A_0$ | 5.0978 |
| $A_1$ | -0.0863 |
| $A_2$ | 0.001658 |
| $A_3$ | $-1.493 \times 10^{-5}$ |
| $A_4$ | $5.353 \times 10^{-8}$ |

### Model Performance

- Observations: 130  
- $R^2$: 0.900  
- Adjusted $R^2$: 0.897  
- F-statistic: 281.7  
- p-value: $1.65 \times 10^{-61}$  

All coefficients are statistically significant ($p < 0.05$).


## 6. Interpretation

### Goodness of fit
The model explains approximately 90% of the variation in emission factors.

### Functional form
The estimated function exhibits a **U-shaped pattern**:

- Low speeds → inefficient driving → higher emissions  
- High speeds → higher engine load → higher emissions  

This is consistent with previous studies.

### Error metrics

- RMSE (ln): 0.153  
- MAE (ln): 0.090  
- RMSE (EF): 26.41 g/km  
- MAE (EF): 6.92 g/km  

→ The model fits well overall, with some larger deviations at certain speeds.



## 7. Application to Traffic Assignment

The estimated function enables emission calculation from link speeds.

Based on this, the following were constructed:

- Speed-based emission factor table  
- Link-based emission factor table  

This allows traffic assignment incorporating CO₂ emissions.



## 8. Conclusion

The emission factor function was successfully estimated using CAN data.

- Outliers removed  
- WLS applied  
- High explanatory power ($R^2 = 0.90$)  
- Realistic U-shaped relationship  

The model is suitable for use in CO₂-aware traffic assignment.

<img width="3000" height="1800" alt="2DF3AE6A-176E-4705-99E7-B3168A76DFE1" src="https://github.com/user-attachments/assets/c6b19fd7-9d8a-4d0b-8ee1-7802b9323b2b" />


---

# 排出係数関数の推定（CANデータ）

## 1. 目的

本分析の目的は、CANデータから得られる車両の速度情報および燃料消費量情報を用いて、**速度とCO₂排出係数の関係を表す関数**を推定することである。

交通配分にCO₂排出を組み込むためには、各リンクの走行速度に応じて排出係数を与える必要がある。そのため、本研究ではまず速度別の排出係数を整理し、それを連続的な関数として表現することを目指した。



## 2. 使用データと前処理

CANデータから、各観測について速度および燃料消費量の情報を取得し、それらを速度帯ごとに集計することで**速度ビン別排出係数テーブル**を作成した。

このテーブルには、各速度ビンについて観測数 $n$、排出係数 $EF$ $[g/km]$、およびその対数値 $\ln(EF)$ が含まれている。

ただし、元データには 200 km/h や 999 km/h など、実際の道路交通では考えにくい速度が含まれていた。これらはGPS誤差やデータ異常によるものと考えられ、推定結果を不安定にする可能性がある。

そこで、以下の前処理を行った。

### 前処理の内容

1. $\ln(EF)$ が欠損している行を除外  
2. 速度範囲を **1〜130 km/h** に制限  


## 3. 排出係数関数の定式化

速度 $v$ に対する排出係数 $EF(v)$ の関係を表現するため、以下の4次多項式モデルを仮定した。

$$
\ln(EF(v)) = A_0 + A_1 v + A_2 v^2 + A_3 v^3 + A_4 v^4
$$


## 4. 推定方法

速度ビン別排出係数は観測値の平均であるため、その信頼性は観測数 $n$ に依存する。

観測数が多いほど信頼性が高く、少ない場合はばらつきが大きくなるため、本研究では通常の最小二乗法ではなく、**加重最小二乗法（WLS）** を用いた。

$$
\sum_i w_i (y_i - \hat{y}_i)^2
$$

ここで、

- $y_i = \ln(EF_i)$  
- $\hat{y}_i$：モデルによる予測値  
- $w_i = n_i$  

とした。


## 5. 推定結果

推定された排出係数関数は以下の通りである。

$$
\ln(EF(v)) =5.0978- 0.0863 v+ 0.001658 v^2- 1.493 \times 10^{-5} v^3+ 5.353 \times 10^{-8} v^4
$$

### パラメータ

| 係数 | 推定値 |
|------|--------|
| $A_0$ | 5.0978 |
| $A_1$ | -0.0863 |
| $A_2$ | 0.001658 |
| $A_3$ | $-1.493 \times 10^{-5}$ |
| $A_4$ | $5.353 \times 10^{-8}$ |

### モデル評価

- 観測点数：130  
- $R^2$：0.900  
- Adjusted $R^2$：0.897  
- F統計量：281.7  
- p値： $1.65 \times 10^{-61}$  

すべての係数は統計的に有意（ $p < 0.05$ ）。


## 6. 結果の解釈

### 決定係数
$R^2 = 0.90$ より、モデルは速度と排出係数の関係を良好に説明している。

### 関数形
係数の符号構造（−, +, −, +）により、排出係数は**U字型**を示す。

- 低速：燃費悪化 → 排出増加  
- 高速：負荷増加 → 排出増加  

これは既存研究と整合的である。

### 誤差指標

- RMSE（ln）：0.153  
- MAE（ln）：0.090  
- RMSE（EF）：26.41 g/km  
- MAE（EF）：6.92 g/km  

→ 平均的な誤差は小さく、一部にやや大きな誤差が存在する。


## 7. 交通配分への利用

本関数により、リンク速度から排出係数を算出可能となる。

本研究では以下を作成：

- 速度別排出係数テーブル  
- リンク別排出係数テーブル  

これにより、CO₂排出を考慮した交通配分分析が可能となった。



## 8. まとめ

CANデータを用いて速度と排出係数の関係を推定した。

- 異常値除去  
- 加重最小二乗法の適用  
- 高い説明力（ $R^2=0.90$ ）  
- U字型の合理的な関数形  

以上より、本モデルは交通配分への適用に有効である。

