# NeurIPS 2026 Continual World Models Workshop — Idea Track
## 第二轮审稿问题汇总与最终修改清单

> **论文标题**：*Toward Continually Growing World Models: An OaK-Inspired Architecture for Learned Abstraction and Planning*  
> **目标投稿**：NeurIPS 2026 — Continual World Models Workshop — Idea Track  
> **当前审稿判断**：**7/10 — Accept**  
> **Reviewer Confidence**：4/5  
> **当前状态**：论文主体框架已经成立，不建议再大改架构；提交前重点进行 **claim 收缩、formal precision、引用修正与排版清理**。

---

# 1. 当前版本整体评价

当前版本相比上一版有明显提升，上一轮的主要硬伤基本已经修复：

- [x] 将创新点从“把 OaK 映射到 World Model”重新定位为 **parameter adaptation + structural adaptation 的双轴 continual world modeling**；
- [x] 新增 feature 已真正进入 predictive state representation；
- [x] reward-respecting subtask 公式已改为更符合 Sutton 等人的原始定义；
- [x] H3 不再错误声称 threshold pruning 能保证 bounded capacity；
- [x] H4 从“prevent plasticity loss”改成了可验证的相对性假设；
- [x] 删除了过强的 “SCWM uniquely...” 等措辞；
- [x] appendix 已删除，正文控制为 4 页；
- [x] citation hyperlink 的彩色框问题已经处理。

目前最大的工作不再是“补内容”，而是：

> **收缩过强 claim + 修正引用 + 主动承认一个真正的 implementation open problem。**

---

# 2. 必须修改的问题

## 2.1 删除 Abstract 中的绝对化措辞

当前类似表述：

> parameter-only adaptation **inevitably suffers from** loss of plasticity, capacity saturation, and compounding rollout errors.

问题：

`inevitably` 表示所有 parameter-only 方法必然发生这些问题，这一结论并未被论文证明。

### 推荐修改

改为：

> parameter-only adaptation **can suffer from** loss of plasticity, capacity saturation, and compounding rollout errors.

或者：

> parameter-only adaptation **is vulnerable to** loss of plasticity, capacity saturation, and compounding rollout errors.

### 推荐程度

**必须修改。**

---

## 2.2 收缩 Introduction 中 “almost exclusively” 的表述

当前类似表述：

> current continual world modeling treats lifelong adaptation **almost exclusively** as online parameter optimization.

问题：

Continual World Model 领域也已经存在 replay、memory、predictive-state adaptation、online planning 等方向，因此 `almost exclusively` 容易被 reviewer 认为是在人为弱化已有研究。

### 推荐修改

改为：

> **Much existing continual world-model work primarily focuses on online parameter adaptation...**

或者：

> **A dominant formulation in continual world modeling focuses on parameter adaptation within a fixed predictive architecture...**

### 推荐程度

**必须修改。**

---

## 2.3 删除 Utility Pruning 中的 “guaranteeing compactness”

当前类似表述：

> pruning ..., **guaranteeing a compact, high-utility abstraction inventory.**

你的规则是：

\[
U(k) < \epsilon_{\mathrm{prune}}
\Rightarrow
\mathrm{prune}(k)
\]

但这一 threshold 规则并不能数学保证：

\[
|\mathcal K_t|
\]

有上界。

如果所有 component 都满足：

\[
U(k) > \epsilon_{\mathrm{prune}},
\]

那么系统仍可能持续增长。

### 推荐修改

将：

> guaranteeing a compact, high-utility abstraction inventory

改为：

> **encouraging a compact, high-utility abstraction inventory**

或：

> **favoring a compact, high-utility abstraction inventory**

### 推荐程度

**必须修改。**

---

# 3. 必须修正的参考文献问题

## 3.1 Sutton et al. 2023 作者列表错误

论文：

**Reward-Respecting Subtasks for Model-Based Reinforcement Learning**

当前参考文献中的作者列表存在错误。

### 正确作者

- Richard S. Sutton
- Marlos C. Machado
- G. Zacharias Holland
- David Szepesvari
- Finbarr Timbers
- Brian Tanner
- Adam White

期刊：

> *Artificial Intelligence*, 324:104001, 2023.

DOI：

> `10.1016/j.artint.2023.104001`

### 修改建议

不要手工修改作者名。

建议从以下任一来源重新导入 BibTeX：

- Publisher；
- DOI；
- DBLP；
- Crossref。

### 推荐程度

**必须修改。**

因为该论文是全文最核心的 OaK / reward-respecting subtask 理论来源之一，引用错误会直接影响 reviewer 对论文严谨性的判断。

---

## 3.2 Schiewer et al. 2024 DOI 错误

论文：

**Exploring the Limits of Hierarchical World Models in Reinforcement Learning**

正确发表信息：

> *Scientific Reports*, 14, 26856 (2024)

正确 DOI：

> `10.1038/s41598-024-76719-w`

### 推荐程度

**必须修改。**

---

# 4. Feature Construction 仍需补充一个理论边界说明

当前定义：

\[
f_{\text{new}}: \mathcal Z \rightarrow \mathbb R
\]

并构造：

\[
\tilde z_t
=
[z_t,
f_1(z_t),
\ldots,
f_n(z_t)].
\]

这一写法已经解决了上一版“feature 没真正进入 World Model”的问题。

但是还存在一个重要理论事实：

如果

\[
f_i = f_i(z),
\]

只是已有 latent state \(z\) 的 deterministic transformation，那么新 feature **不会凭空增加原始 latent state 中已经丢失的信息**。

也就是说：

\[
I(\tilde z; o)
\]

不会因为增加 deterministic feature 就自动获得 encoder 已经丢弃的 observation information。

因此目前的 feature construction 更准确地说是在扩展：

> **computational / predictive basis**

而不一定是在增加：

> **new information content**。

---

## 推荐增加一句 Open Question

### 保守版本，推荐使用

> **In the present formulation, feature construction expands the computational basis of the predictive state. Extending structural construction to predictive information omitted by the base encoder remains an important open problem.**

这一版最稳。

---

### 更开放的版本

> **A full implementation may allow feature constructors to access observation history or agent-internal traces beyond the current embedding, enabling genuinely new predictive information to enter the structural state representation.**

这会扩大未来研究空间，但当前 paper 不需要实现。

### 推荐程度

**建议修改。**

不是拒稿级问题，但补上后理论表述会成熟很多。

---

# 5. 必须主动承认的 Implementation Open Problem

当前模型允许：

\[
\tilde z_t
\in
\mathbb R^{d_z + |\mathcal F_t|}.
\]

当新的 feature 被创建后：

\[
|\mathcal F_t|
\rightarrow
|\mathcal F_t| + 1,
\]

world model 的 input / output dimensionality 也会发生变化：

\[
p_{\theta,t}
(
\tilde z_{t+1}
\mid
\tilde z_t,a_t
).
\]

这意味着一个真正困难的问题：

> **当 state representation 动态扩维时，如何扩展 MLP / RSSM / Transformer dynamics，而不破坏已有 transition knowledge？**

这是 SCWM 最真实、最重要的工程挑战之一。

---

## 推荐在 Discussion 中增加

### 小标题

**Architecture-Compatible Expansion**

### 推荐文字

> **When the predictive state \(\tilde Z_t\) grows, the underlying dynamics model must allocate new capacity while preserving previously learned transition functions. Designing function-preserving structural expansion for neural world models therefore remains a central open problem for SCWM.**

这句话会让 reviewer 感觉作者明确知道：

> `K_t` 的增长不是只改一个集合符号，而是真正需要 neural architecture expansion。

### 推荐程度

**强烈建议修改。**

---

# 6. Reward-Respecting Subtask 部分还剩一个小问题

当前：

\[
g_i=(C_t=R_t,z_i)
\]

以及 terminal stopping bonus 的形式已经比上一版正确很多。

但是后续类似：

\[
\mathcal I_i
=
\{
\tilde z :
f_i(\tilde z) \geq \delta_{\mathrm{init}}
\}
\]

这种 initiation set 的定义属于本文提出的一种具体设计选择，并不是 Sutton 的 reward-respecting subtask 理论必然推出的结果。

### 推荐修改

不要写得像理论规定。

可以改成：

> **For example, one possible initiation set is**

然后再给：

\[
\mathcal I_i
=
\{
\tilde z :
f_i(\tilde z) \geq \delta_{\mathrm{init}}
\}.
\]

### 推荐程度

**建议修改。**

---

# 7. Related Work 当前已经基本正确，不建议继续扩大

当前文章已经承认：

- AgentOWL；
- SALT；
- Forecaster；

均已经涉及：

- state abstraction；
- temporal abstraction；
- skills/options；
- abstract world models；
- planning。

这是正确的。

当前真正需要守住的差异是：

\[
\boxed{
\text{create}
\rightarrow
\text{evaluate}
\rightarrow
\text{retain/revise}
\rightarrow
\text{prune}
}
\]

即：

> **the continual lifelong lifecycle of the abstraction structure itself**

而不是再争：

> 谁第一个把 option 放进 World Model。

---

## 当前建议保留的核心区别

> Existing approaches learn state or temporal abstractions for planning, while SCWM treats the **continual structural lifecycle of the abstraction inventory itself** as part of the world-model learning problem.

### 不要重新加入的表述

不要使用：

- `SCWM uniquely...`
- `Existing hierarchical RL stops at option discovery...`
- `No previous work...`
- `The first world model with options...`

除非未来进行了非常完整的 novelty search。

---

# 8. H3 当前修改正确

旧版错误：

> utility threshold pruning guarantees bounded capacity.

当前改成：

> **H3 — Growth–Utility Frontier**

并比较：

- unconstrained growth；
- prediction-only pruning；
- multi-objective pruning。

推荐继续保持：

\[
\text{performance}
\quad vs \quad
|\mathcal K|
\quad vs \quad
\text{compute}.
\]

这是一个合理且可证伪的 hypothesis。

### 不建议继续修改。

---

# 9. H4 当前修改正确

旧版：

> structural allocation prevents loss of plasticity.

当前：

> structurally allocating new feature-option units **preserves representation plasticity relative to fixed-capacity baselines**.

并考虑：

- effective rank；
- dormant units；
- adaptation ability。

这一版本已经从绝对性结论变成正常 hypothesis。

### 不建议继续修改。

---

# 10. 论文创新定位当前应保持不变

当前最合理的创新点不是：

> **We apply OaK to World Models.**

而是：

> **We use OaK to formulate a second, structural adaptation axis for modern continual neural world models.**

两条轴：

## Parameter adaptation

\[
\theta_t
\rightarrow
\theta_{t+1}
\]

## Structural adaptation

\[
\mathcal K_t
\rightarrow
\mathcal K_{t+1}
\]

其中：

\[
\mathcal K_t
=
(
\mathcal F_t,
\mathcal G_t,
\mathcal O_t,
\mathcal M_t
).
\]

因此完整 continual adaptation：

\[
(
\theta_t,
\mathcal K_t
)
\rightarrow
(
\theta_{t+1},
\mathcal K_{t+1}
).
\]

这是当前文章最重要的 conceptual contribution。

### 不建议再改回 “OaK-to-WM Mapping” 作为主要 novelty。

---

# 11. Figure 1 的排版建议

Figure 1 当前逻辑已经完整：

```text
Fast Online Interaction Loop
        +
Slow Structural Continual Loop

Feature
  ↓
SubTask
  ↓
Option
  ↓
Option Model
  ↓
Planning
  ↓
Utility Evaluation
  ↓
Retain / Prune
```

并且已经加入：

> Extended Predictive State

\[
\tilde z_t.
\]

内容层面没有明显问题。

---

## 当前主要问题

图中文字偏小。

Figure 1 是全文最重要的 visual contribution，不应该让 reviewer 放大 PDF 才能看清。

### 推荐

- Figure 放大约 **20%–25%**；
- 减少框内解释文字；
- 优先保留关键词；
- 不要再加入更多模块。

### 推荐程度

**建议修改。**

---

# 12. Table 1 的排版建议

Table 1 内容是有价值的，但部分信息与 Figure 1 重复。

如果 4 页排版空间紧张：

> **优先保证 Figure 1 清晰，Table 1 可以进一步压缩。**

例如 Table 只保留：

| OaK | SCWM |
|---|---|
| Feature | predictive state expansion |
| SubTask | reward-respecting objective |
| Option | temporal abstraction |
| Model | option-level predictive dynamics |
| Planning | hybrid multi-scale imagination |
| Utility | retain / revise / prune |

不一定需要在 Table 中再次完整放所有公式。

### 推荐程度

**可选优化。**

---

# 13. 当前页数安排合理

当前：

- Page 1–4：正文；
- Page 5–6：References；
- 无 Appendix。

这是比上一版更合理的 Idea Track 投稿形式。

### 不建议

- 恢复 Algorithm Appendix；
- 新加完整伪代码；
- 新增大段 Related Work；
- 新增完整 autonomous-driving case study。

Idea Track 当前更需要：

> **clear idea + precise formulation + falsifiable agenda**

而不是更多内容。

---

# 14. 当前不应该再增加的内容

不要继续增加：

- VLA；
- autonomous driving full architecture；
- diffusion world model；
- transformer implementation；
- LLM；
- vehicle-road-cloud；
- 额外 benchmark；
- theorem；
- extensive pseudo-code；
- 企业应用案例。

当前论文已经足够完整。

继续加内容反而会使中心命题变弱。

---

# 15. 建议最终提交前修改清单

## P0 — 必须改

- [ ] `inevitably suffers` → `can suffer from` / `is vulnerable to`
- [ ] `almost exclusively` → `much existing work primarily focuses on`
- [ ] `guaranteeing a compact...` → `encouraging/favoring a compact...`
- [ ] 修正 Sutton et al. 2023 作者列表
- [ ] 修正 Schiewer et al. 2024 DOI
- [ ] 对所有 2025–2026 文献重新检查 BibTeX 元数据

---

## P1 — 强烈建议改

- [ ] Discussion 增加 **Architecture-Compatible Expansion**
- [ ] 明确 dynamic latent dimensionality expansion 是 open problem
- [ ] 说明当前 feature construction 扩展的是 predictive/computational basis
- [ ] 承认恢复 base encoder 已丢失的信息仍是 open problem
- [ ] initiation set 前加 `e.g.` / `one possible choice is`

---

## P2 — 排版优化

- [ ] Figure 1 放大 20%–25%
- [ ] 缩短 Figure 框内文字
- [ ] Table 1 可适当压缩
- [ ] Figure 优先级高于 Table
- [ ] 最后检查所有 DOI / title / author / year

---

# 16. 修改后预期审稿结果

当前版本：

> **7/10 — Accept**

如果上述 P0 + P1 问题全部修正，预计文章不会再有明显的“硬伤型” reviewer objection。

届时最大的争议将只剩：

> **Is the dual-axis structural continual world-model formulation sufficiently novel and useful as a position / idea paper?**

对于 Continual World Models Workshop — Idea Track 的定位，这一问题目前已经可以给出积极答案。

---

# 17. 最终应守住的论文主线

全文不要偏离：

\[
\boxed{
\text{Continual World Modeling}
\neq
\text{Only Parameter Adaptation}
}
\]

而应该强调：

\[
\boxed{
\text{Continual World Modeling}
=
\text{Parameter Adaptation}
+
\text{Structural Adaptation}
}
\]

即：

\[
\theta_t
\rightarrow
\theta_{t+1}
\]

同时：

\[
\mathcal K_t
\rightarrow
\mathcal K_{t+1}.
\]

OaK / FC-STOMP 的作用是为第二条轴提供一个具体的 constructive mechanism：

\[
F
\rightarrow
S
\rightarrow
O
\rightarrow
M
\rightarrow
P
\]

再加：

\[
\text{utility}
\rightarrow
\text{retain / revise / prune}.
\]

---

# 18. 推荐保留的核心句

## 论文中心句

> **Continual world modeling should mean more than continually updating the parameters of a fixed predictor.**

## SCWM 中心句

> **We introduce a structural adaptation axis alongside parameter adaptation, allowing predictive state abstractions, temporal abstractions, and option models to evolve with experience.**

## 最终结论句

> **The central question is not only how a world model continues updating its weights, but how it continually discovers, organizes, and retains predictive knowledge worth having.**

---

# 最终判断

当前论文：

> **已经具备 NeurIPS 2026 Continual World Models Workshop — Idea Track 的投稿质量。**

下一步不建议继续扩展理论框架。

最合理的工作是：

> **完成 P0 必改项 → 补两个 open problem → 做最后一轮引用和排版检查 → 投稿。**
