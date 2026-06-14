# 公式速查卡

> 预测一场比赛时的快速计算流程。详细说明见 SKILL.md 对应章节。

---

## 一页纸流程

```
Step 1: True_Elo = Raw_Elo + Confed_Adj
Step 2: Club_Quality = (Big5_Minutes% × 0.5) + (TM_Value_Pct × 0.5)
Step 3: Recent_Form = PPG10 × (Opponent_Avg_Elo / Global_Avg_Elo)
Step 4: Strength = True_Elo×0.55 + Club_Quality(normalized)×0.25 + Recent_Form(normalized)×0.20
Step 5: λ = 1.50 × Home_Attack × Away_Defense × Home_Adv × (1 + ΣEnv_Home)
        μ = 1.10 × Away_Attack × Home_Defense × (1 + ΣEnv_Away)
Step 6: P(x,y) = τ(λ,μ,x,y) × Poisson(x|λ) × Poisson(y|μ)
Step 7: 1X2 = Σ P(x,y) 归类汇总
Step 8: 应用行为金融修正 → P_final（第五章）
Step 9: Edge = (P_final − P_market) × Odds
Step 10: ≥18% → 🟢 | 10-18% → 🟡 | <10% → 🔴（竞彩基准）
```

---

## 联合会修正

```
UEFA=0  CONMEBOL=-15  CONCACAF=-40  AFC=-25  CAF=-20  OFC=-55
```

## 攻防系数

```
Home_Attack  = GF_home_last20 / GF_all_avg
Home_Defense = GA_home_last20 / GA_all_avg
Away_Attack  = GF_away_last20 / GF_all_avg
Away_Defense = GA_away_last20 / GA_all_avg
```

## 泊松公式

```
P(k|λ) = (λᵏ × e⁻λ) / k!
λ = 主队预期进球, μ = 客队预期进球
```

## τ 修正（国际赛事 ρ = −0.13）

```
(0,0): 1 − λμρ    (1,0): 1 + μρ
(0,1): 1 + λρ     (1,1): 1 − ρ
其他: 1
```

## 环境修正速查

| 因素 | 条件 | 主队 | 客队 |
|------|------|------|------|
| 海拔 | 500-1500m | −2% | −3% |
| | 1500-2500m | −5% | −8.5% |
| | >2500m | −8% | −13% |
| 气温 | 30-35°C | −3% | −3% |
| | >35°C | −5% | −5% |
| | <5°C | −3% | −3% |
| 湿度 | 70-85% | −1% | −1% |
| | >85% | −2% | −2% |
| 旅途 | 3000-5000km/4天 | −4% | −4% |
| | >5000km/4天 | −7% | −7% |
| 时差 | 3-5h/3天 | −2% | −2% |
| | >5h/3天 | −4% | −4% |
| 休息 | <3天 | −5% | −5% |
| 伤病 | 1核心缺阵 | −3% | −3% |
| | 2核心缺阵 | −6% | −6% |
| | 3+核心缺阵 | −8% | −8% |
| 主场观众 | 60% | +1% | — |
| | 70% | +2% | — |
| | 80%+ | +3% | — |

## 行为金融修正速查

| 因素 | 量化方法 | 典型修正 |
|------|---------|---------|
| ① 情绪溢出 | 抽水膨胀值（>7% 部分） | ±3-8pp |
| ② 标志胜利 | (对手Elo差/400) × 权重 × 30 | ±1-3pp |
| ③ 伤病影响 | 身价占比 × xG参与率 × 0.7 | −1-4pp/人 |
| ④ 叙事压力 | 历史东道主首秀偏差表 | ±1-2pp |

> 四项叠加，封顶 ±12pp。详细公式见 SKILL.md 第五章。

## Edge Score 速查（竞彩基准）

```
Edge = (P_model − P_market) × Odds

≥18% → 🟢 出击（竞彩11%抽水+模型误差打穿）
10-18% → 🟡 观察
<10% → 🔴 放弃
```

> 国际庄家阈值：15% / 8%。竞彩因为抽水更高（11% vs 5-7%），门槛上浮 3pp。

---

## 交叉校验

当 λ/μ 与 Elo 经验值偏差 >20% 时：
```
λ = λ_攻防 × 0.6 + λ_Elo经验 × 0.4
μ = μ_攻防 × 0.6 + μ_Elo经验 × 0.4
```

Elo → λ/μ 经验参考：
```
ΔElo=0:   λ≈1.35 μ≈1.15
ΔElo=50:  λ≈1.55 μ≈1.05
ΔElo=100: λ≈1.75 μ≈0.95
ΔElo=200: λ≈2.15 μ≈0.75
```
