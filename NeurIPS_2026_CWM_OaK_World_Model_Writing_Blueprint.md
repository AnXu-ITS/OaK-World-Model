# NeurIPS 2026 Continual World Models Workshop 写作蓝图
## OaK-Inspired Continually Growing World Models

> **目标投稿**：NeurIPS 2026 Workshop on Continual World Models — **Idea Track**  
> **篇幅**：2–4 pages  
> **论文性质**：Position / Vision / New Problem Formulation / Early Research Direction  
> **核心目的**：提出一个清晰、可证伪、可后续实验化的研究命题——**用 Richard Sutton 的 OaK architecture 重新组织 world model，使其从“固定的预测器”转向“能够持续构造状态抽象、时间抽象与可规划知识的 growing world model”。**  
> **当前阶段要求**：不必证明完整 OaK 已经实现；重点是把问题定义、架构映射、研究假设与验证路线讲清楚。

---

# 0. 一句话定位

论文不要写成：

> We apply OaK to world models.

而应写成：

> **We propose an OaK-inspired view of continual world models in which the model does not merely update parameters from new experience, but continually constructs, evaluates, and reorganizes state and temporal abstractions that become reusable predictive knowledge for planning.**

核心变化：

```text
Conventional continual world model
new experience
      ↓
update θ
      ↓
WMθ'

OaK-inspired continual world model
new experience
      ↓
prediction error / utility / novelty
      ↓
Feature Construction
      ↓
SubTask
      ↓
Option
      ↓
Option Model
      ↓
Planning
      ↓
retain / revise / prune abstractions
```

真正要强调的不是“持续训练参数”，而是：

> **world model 的知识结构本身可以持续生长和重组。**

---

# 1. 推荐标题

## 首选标题

**Toward Continually Growing World Models: An OaK-Inspired Architecture for Learned Abstraction and Planning**

优点：

- 与 Workshop 的 continual world models 主题直接对齐；
- “Toward” 适合 Idea / Position Paper，不会过度宣称已实现；
- “Growing” 比单纯 “Continual” 更能体现 OaK；
- 把 abstraction + planning 两个 OaK 核心元素写出来。

## 备选 1

**From Frozen Predictors to Growing World Models: An OaK-Inspired Perspective**

更像 position paper。

## 备选 2

**Rethinking Continual World Models through the OaK Architecture**

更直接，但创新表达略弱。

## 备选 3

**Beyond Continual Parameter Updates: OaK-Inspired Structural Growth in World Models**

最适合强调“参数更新 ≠ 真正持续世界模型”。

---

# 2. 核心 Research Question

全文只围绕一个主问题：

> **Can a world model continually construct and reorganize its own predictive abstractions, rather than only updating the parameters of a fixed model?**

进一步拆成三个子问题：

### RQ1 — State abstraction
当模型持续遇到新经验时，能否产生新的、对预测和规划有用的 features，而不是固定使用预训练 latent representation？

### RQ2 — Temporal abstraction
模型能否从 primitive action transition：

\[
p(z_{t+1}\mid z_t,a_t)
\]

扩展到 temporally extended option model：

\[
p(z_{t+\tau}, R_{t:t+\tau}, \tau \mid z_t,o)
\]

从而学习“执行一个行为之后世界会怎样”？

### RQ3 — Structural continual learning
模型能否根据长期 utility：

- create；
- retain；
- revise；
- prune

自己的 abstraction / option / option model，而不是只更新固定神经网络中的权重？

---

# 3. 论文最核心的观点

建议在 Introduction 第一页就给出：

## Thesis

> **A continual world model should not be defined only by its ability to update parameters after deployment. A stronger notion of continual world modeling requires the predictive structure itself—its features, temporal abstractions, and reusable models—to grow and reorganize with experience. We argue that Sutton's OaK architecture provides a principled blueprint for this transition.**

可以把全文压缩成公式：

\[
\text{Continual WM}
\neq
\text{Fixed Architecture} + \text{Online Updates}
\]

而是：

\[
\boxed{
\text{Continual WM}
=
\text{Prediction}
+
\text{Continual Learning}
+
\text{Constructed Abstraction}
+
\text{Planning Utility}
}
\]

再用 OaK 的 FC-STOMP 对应：

\[
\boxed{
F \rightarrow S \rightarrow O \rightarrow M \rightarrow P
}
\]

其中：

- \(F\)：Feature Construction
- \(S\)：SubTask
- \(O\)：Option
- \(M\)：Option Model
- \(P\)：Planning

---

# 4. 论文贡献写法

Idea Track 不需要伪装成已经完成的大型技术论文。

建议贡献只写 3 条。

## Contribution 1 — Problem formulation

提出：

> **Structural Continual World Modeling**

定义世界模型的 continual adaptation 不仅包括参数变化：

\[
\theta_t \rightarrow \theta_{t+1}
\]

还包括知识结构变化：

\[
\mathcal{K}_t
\rightarrow
\mathcal{K}_{t+1}
\]

其中：

\[
\mathcal{K}_t =
(\mathcal{F}_t,
\mathcal{G}_t,
\mathcal{O}_t,
\mathcal{M}_t)
\]

分别对应：

- learned features；
- subtasks；
- options；
- option models。

---

## Contribution 2 — OaK-to-World-Model mapping

把 OaK 的 FC-STOMP 映射到现代 World Model：

| OaK element | World-model interpretation |
|---|---|
| Feature Construction | learned predictive state abstraction |
| SubTask | locally useful predictive/control objective |
| Option | temporally extended behavior |
| Option Model | temporally abstract transition / return model |
| Planning | reasoning over primitive and abstract transitions |
| Utility feedback | create / retain / revise / prune knowledge |

这张表非常重要。

你的论文不是简单“引用 Sutton”，而是做出：

> **OaK → Continual World Model 的架构解释。**

---

## Contribution 3 — Testable research agenda

提出一组未来可以直接验证的 hypotheses：

### H1 — Temporal abstraction
Option-level models should enable longer effective planning horizons under a fixed planning budget.

### H2 — Continual adaptation
Constructing new abstractions should reduce adaptation cost under persistent distribution shift.

### H3 — Retention
Utility-based retention and pruning should mitigate uncontrolled growth while preserving reusable knowledge.

### H4 — Structural plasticity
A model capable of expanding its abstraction set should adapt to qualitatively new situations more effectively than a fixed-architecture continual baseline.

### H5 — Planning usefulness
Abstractions optimized for predictive accuracy alone may differ from abstractions that are useful for planning; OaK provides a mechanism for evaluating abstractions through downstream planning utility.

---

# 5. 推荐 2–4 页结构

建议目标：**3.5–4 页正文 + references**（以 Workshop 最终模板规则为准）。

---

# Section 1 — Introduction
## 建议长度：约 0.7–0.9 页

### 第一段：Workshop 对应的问题

从当前 World Model 的核心矛盾开始：

现代 world model 已经能够：

- learn latent dynamics；
- predict future observations/states；
- support imagination；
- support planning/control。

但是部署以后，大部分系统仍然建立在：

> **fixed representation + fixed temporal scale + fixed model structure**

之上。

即使加入 continual learning，很多方案的基本操作仍然是：

\[
\theta \leftarrow \theta - \alpha \nabla \mathcal{L}
\]

也就是：

> 新数据来了 → 更新原来的网络。

### 第二段：指出缺口

提出一个更强的问题：

> What if the world changes in a way that requires not merely new parameter values, but new abstractions?

举例：

```text
agent initially learns:
vehicle
lane
following

later encounters:
temporary construction coordination
multi-agent negotiation
novel recovery behavior
```

问题不一定只是：

> “原来的模型参数没调好。”

而可能是：

> **原来的模型根本没有适合描述和预测这个新结构的 abstraction。**

### 第三段：引入 OaK

介绍 Sutton 的 OaK：

- model-based RL architecture；
- all components continually learn；
- learned state/time abstractions；
- FC-STOMP：
  Feature Construction → SubTask → Option → Model → Planning。

然后立即提出：

> We argue that this progression naturally suggests a new architecture for continual world modeling.

### Introduction 最后一段：本文做什么

建议写成：

> In this position paper, we propose an OaK-inspired framework for continually growing world models. Rather than treating continual adaptation solely as parameter updating, the framework allows the agent to construct predictive features, turn useful features into subtasks, acquire temporally extended options, learn models of those options, and evaluate the resulting abstractions through their utility for planning. We formulate this as structural continual world modeling and outline testable hypotheses and an experimental agenda.

---

# Section 2 — What Is Missing from Continual World Models?
## 建议长度：约 0.5–0.7 页

这一节不要做大综述。

只需要制造一个非常清楚的 conceptual gap。

## 2.1 Fixed state abstraction

传统：

\[
o_t \xrightarrow{encoder} z_t
\]

encoder 训练后：

\[
z_t \in \mathcal{Z}
\]

representation 的基本形式固定。

问题：

> 新环境可能要求新的 predictive feature，而不是仅仅改变 feature value。

---

## 2.2 Fixed temporal granularity

常见 world model：

\[
(z_t,a_t)
\rightarrow
z_{t+1}
\]

多步预测依赖：

\[
z_t
\rightarrow
z_{t+1}
\rightarrow
z_{t+2}
\rightarrow
...
\rightarrow
z_{t+H}
\]

问题：

- error accumulation；
- planning depth；
- computational cost；
- long-horizon dependencies。

缺失的是：

\[
(z_t,o)
\rightarrow
z_{t+\tau}
\]

其中 \(o\) 是 temporally extended option。

---

## 2.3 Fixed knowledge structure

强调：

普通 continual learning：

\[
\theta_t \rightarrow \theta_{t+1}
\]

你提出：

\[
(\theta_t,\mathcal{K}_t)
\rightarrow
(\theta_{t+1},\mathcal{K}_{t+1})
\]

其中：

\[
\mathcal{K}_{t+1}
\neq
\mathcal{K}_{t}
\]

这就是文章真正的新 framing。

建议给它一个明确名字：

> **Structural Continual World Modeling (SCWM)**

注意：如果后续文献检索发现已有同名术语，应换名。

---

# Section 3 — OaK-Inspired Continually Growing World Model
## 建议长度：约 1.2–1.5 页
## 全文最重要的一节

建议这一节占最大篇幅。

---

## 3.1 Base World Model

先承认我们不要求推翻现代 WM backbone：

\[
z_t = E_\phi(o_{\le t})
\]

\[
\hat{z}_{t+1},\hat{r}_{t+1}
=
M_\theta(z_t,a_t)
\]

OaK-inspired architecture 建在已有 WM 之上。

这样可以避免被审稿人理解成：

> “作者要用几十年前 RL 结构替代现代 generative world model。”

你的立场是：

> **OaK supplies the continual abstraction and planning organization; the underlying representation/dynamics modules can remain modern neural world models.**

---

## 3.2 Feature Construction

定义：

\[
f_i = F_i(z_t)
\]

但重点不是 feature extraction 本身，而是：

\[
\mathcal{F}_t
=
\{f_1,\ldots,f_n\}
\]

可以变成：

\[
\mathcal{F}_{t+1}
=
\mathcal{F}_{t}
\cup
\{f_{n+1}\}
\]

新 feature 的产生可以由：

- persistent prediction error；
- novelty；
- planning failure；
- uncertainty；
- recurring interaction structure

触发。

当前 position paper 不需要指定唯一算法。

---

## 3.3 Feature → SubTask

对于有用 feature \(f_i\)，构造 subtask：

\[
g_i
\]

这里要强调 Sutton 的 reward-respecting 思路：

subtask 不是完全脱离主任务随便造一个 auxiliary objective，而应保持与原任务 reward / utility 的联系。

世界模型视角下可以解释为：

> **A feature becomes meaningful when the agent can formulate a behaviorally relevant question around it.**

---

## 3.4 SubTask → Option

学习 temporally extended option：

\[
o_i = (\mathcal{I}_i,\pi_i,\beta_i)
\]

其中：

- \(\mathcal{I}_i\)：initiation set；
- \(\pi_i\)：option policy；
- \(\beta_i\)：termination condition。

这一步让 world model 的 action representation 从：

\[
a_t
\]

扩展成：

\[
\{a_t,o_1,o_2,\ldots,o_n\}
\]

关键概念：

> **The world model no longer predicts only what one primitive action does; it can learn what a temporally extended behavior does to the world.**

---

## 3.5 Option → Option Model

核心公式：

\[
\mathcal{M}_{o_i}:
z_t
\rightarrow
(\hat{z}_{t+\tau},
\hat{R}_{t:t+\tau},
\hat{\tau})
\]

或者概率形式：

\[
p(
z_{t+\tau},
R_{t:t+\tau},
\tau
\mid
z_t,o_i
)
\]

这就是你和普通 hierarchical policy 最大的区别：

> 不是仅仅 learning options。

而是：

> **learning predictive models of options and inserting those models into the world model used for planning.**

这必须写清楚。

---

## 3.6 Option Models → Planning

Planner 可以同时考虑：

\[
\mathcal{A}^{+}
=
\mathcal{A}
\cup
\mathcal{O}
\]

也就是：

```text
primitive action
primitive action
option
primitive action
option
...
```

这样规划可以在不同 temporal scales 间切换。

例如：

```text
primitive WM:
a → a → a → a → a → a → a → a

OaK-WM:
        option A
z ─────────────────→ z'
                      │
                      option B
                      └──────────→ z''
```

核心假设：

> 在固定 planning compute 下，temporal abstraction 可以让 planning 看得更远。

---

## 3.7 Utility Feedback: Growing and Pruning

这一部分决定它是否真正是 “growing world model”。

定义 abstraction utility：

\[
U(k)
=
\lambda_1 U_{\text{prediction}}
+
\lambda_2 U_{\text{planning}}
+
\lambda_3 U_{\text{reuse}}
-
\lambda_4 C_{\text{complexity}}
\]

这里暂时作为 conceptual form，不宣称这是最终算法。

根据 utility：

```text
new experience
      ↓
candidate abstraction
      ↓
learn
      ↓
evaluate
      ↓
 ┌────┴────┐
useful    useless
 ↓          ↓
retain    prune
```

因此：

\[
\mathcal{K}_{t+1}
=
\operatorname{GrowPrune}
(
\mathcal{K}_{t},
e_{t:t+\Delta}
)
\]

这是整篇论文最值得形成“新概念”的一句话：

> **Continual world modeling becomes a problem of continual knowledge-structure adaptation, not merely continual parameter adaptation.**

---

# 6. 必须有的一张主图

2–4 页 paper 最多做 **1 张主架构图 + 1 个小表格**。

主图建议占 0.4–0.6 页。

## Figure 1 — From Fixed World Models to OaK-Inspired Growing World Models

建议布局：

```text
┌─────────────────────────────────────────────┐
│             Environment / Experience        │
└───────────────────┬─────────────────────────┘
                    ↓
             Observation / State
                    ↓
        ┌─────────────────────┐
        │ Neural World Model  │
        │   representation z  │
        └──────────┬──────────┘
                   ↓
           prediction error
        novelty / uncertainty
                   ↓
        ┌─────────────────────┐
        │ Feature Construction│
        └──────────┬──────────┘
                   ↓
                SubTask
                   ↓
                 Option
                   ↓
             Option Model
                   ↓
               Planning
                   │
                   ↓
              Environment
                   │
                   └───────────────┐
                                   ↓
                          utility evaluation
                         create / retain / prune
                                   │
                                   └────→ Feature Construction
```

图中需要明确标注：

> **FC-STOMP**

以及一条粗箭头：

> **continual structural adaptation**

---

# 7. Figure Caption 推荐

> **Figure 1: OaK-inspired continually growing world model.** A conventional neural world model provides predictive state representations and short-horizon dynamics. Persistent prediction errors, novelty, or planning failures can motivate feature construction. Following the OaK FC-STOMP progression, useful features induce subtasks, temporally extended options, and predictive models of those options. Planning utility provides feedback for retaining, revising, or pruning abstractions, allowing the predictive knowledge structure itself to evolve with experience.

---

# 8. Section 4 — Research Hypotheses and Experimental Agenda
## 建议长度：0.6–0.8 页

不要做完整实验设计。

只需要证明：

> 这个 idea 是可证伪的，不是哲学口号。

---

## Experiment A — Temporal abstraction

比较：

### Baseline

\[
p(z_{t+1}|z_t,a_t)
\]

### OaK-inspired

\[
p(z_{t+\tau}|z_t,o_t)
\]

控制相同 planning budget。

测：

- success rate；
- effective planning horizon；
- planning compute；
- model error；
- return。

验证 H1。

---

## Experiment B — Persistent distribution shift

环境顺序：

```text
Environment A
    ↓
Environment B
    ↓
Environment C
```

Baseline：

- frozen WM；
- online fine-tuning WM；
- replay-based continual WM。

OaK-inspired：

- existing abstractions；
- construction of new features/options/models；
- utility retention/pruning。

测：

- adaptation speed；
- forward transfer；
- backward transfer；
- forgetting；
- number of retained abstractions。

验证 H2/H3/H4。

---

## Experiment C — Planning utility vs prediction accuracy

构造两个 abstraction：

```text
Abstraction A:
high predictive accuracy
low planning utility

Abstraction B:
moderate predictive accuracy
high planning utility
```

问题：

> World models 是否应该仅按照 reconstruction / prediction objective 选择 abstraction？

OaK-inspired hypothesis：

> **Predictive knowledge should ultimately be evaluated by how it supports useful planning.**

这是非常有讨论价值的一点。

---

# 9. 是否加入自动驾驶案例

建议：

## 主论文保持 general

不要让标题直接变成：

> OaK for Autonomous Driving World Models

因为 Workshop 是 general world models。

但是可以在 Introduction 或 Figure 旁边加一个 3–4 句 motivating example：

```text
Autonomous agent initially learns:
lane following
yielding
merging

Deployment introduces:
temporary roadwork
unusual interaction protocol
novel recovery situation

Fixed WM:
update parameters inside an existing representation.

OaK-inspired WM:
detect persistent model failure
→ construct a new feature
→ formulate a subtask
→ learn a new option
→ learn its option model
→ reuse it in future planning.
```

这样可以：

- 保留未来自动驾驶研究线；
- 又不会缩小 workshop 读者范围。

---

# 10. Discussion / Open Questions
## 建议长度：0.3–0.5 页

Idea Track 非常适合主动承认难点。

建议列 4 个开放问题。

### Q1 — What should trigger feature construction?

prediction error 并不等于一定需要新 abstraction。

需要区分：

- noise；
- temporary shift；
- persistent structure；
- true novel concept。

---

### Q2 — How to prevent unbounded growth?

Growing model 最大风险：

\[
|\mathcal{K}_t|
\rightarrow
\infty
\]

必须有：

- utility；
- redundancy；
- reuse；
- complexity cost；
- pruning。

---

### Q3 — How should planning utility be measured?

一个 feature：

- prediction 很准；
- 但规划根本不用。

是否值得保留？

这是 OaK 对 world-model representation learning 可以提出的新问题。

---

### Q4 — How much of full OaK is necessary?

当前 paper 不需要声称：

> “我们实现完整 OaK。”

反而应该明确：

> OaK is a broad architectural vision. We use FC-STOMP and continual structural adaptation as a conceptual blueprint for designing future continual world models.

这句话可以主动降低审稿风险。

---

# 11. Conclusion
## 建议长度：100–150 words

只收束一个观点：

```text
World Model 1.0
predict what happens next

        ↓

Continual World Model
keep updating predictions

        ↓

OaK-inspired Growing World Model
continually construct
what is worth representing,
what is worth doing,
what is worth predicting,
and what is worth planning with.
```

推荐最后一句：

> **The central question is therefore not only how a world model should continue learning, but how it should continue deciding what knowledge is worth having.**

这句话适合作为全文 ending。

---

# 12. Abstract 蓝图

Abstract 控制在约 150–200 words。

按照 5 句结构写。

## Sentence 1 — Background

World models increasingly support prediction, imagination, and planning, yet most retain a fixed representational and temporal structure after training.

## Sentence 2 — Gap

Existing continual adaptation commonly focuses on updating model parameters, leaving open how the model itself could acquire new reusable abstractions from experience.

## Sentence 3 — Proposal

We propose an OaK-inspired framework for continually growing world models, drawing on Sutton's FC-STOMP progression of Feature Construction, SubTask formation, Option learning, Option-Model learning, and Planning.

## Sentence 4 — Key idea

The framework augments primitive dynamics with continually constructed state features and temporally extended option models, whose retention is governed by predictive and planning utility.

## Sentence 5 — Research agenda

We formulate structural continual world modeling as a research problem and outline testable hypotheses concerning planning horizon, adaptation efficiency, forgetting, abstraction growth, and utility-based model organization.

---

# 13. Introduction 的逻辑必须是这个顺序

不要：

```text
Sutton很厉害
↓
OaK很厉害
↓
World Model很火
↓
我们把它们结合
```

这种写法非常像“概念拼接”。

必须：

```text
World Model 的 continual-learning 缺口
↓
参数更新不足以解决 structural adaptation
↓
需要 state + temporal abstraction 的持续构造
↓
OaK 正好提供 principled mechanism
↓
因此提出 OaK-inspired growing WM
```

也就是说：

> **问题先于 OaK。**

OaK 是解决问题的理论依据，而不是论文存在的理由。

---

# 14. 需要强调的创新边界

## 可以说

- We propose an OaK-inspired formulation of continually growing world models.
- We distinguish parameter-level continual adaptation from structural continual adaptation.
- We map FC-STOMP onto world-model representation, temporal abstraction, predictive modeling, and planning.
- We propose testable hypotheses.
- We outline a research agenda.

## 现在不要说

- We solve continual world modeling.
- We implement the full OaK architecture.
- We are the first to combine hierarchical planning and world models.
- OaK eliminates catastrophic forgetting.
- Option models necessarily outperform primitive dynamics.
- Our architecture achieves open-ended intelligence.
- This architecture is guaranteed to scale.

---

# 15. 和已有领域区分

审稿人可能会问：

> 这不就是 hierarchical RL / options / continual learning 吗？

必须提前回答。

---

## vs Hierarchical RL

Hierarchical RL 通常关注：

> 学习 high-level / low-level policies。

你的核心是：

> **learn the model of temporally extended options and make those models part of an evolving world model used for planning.**

---

## vs Option-Critic

Option-Critic：

> learn option policy + termination。

你的关注点：

\[
Option
\rightarrow
Option Model
\rightarrow
World Model
\rightarrow
Planning
\]

并进一步：

\[
Feature
\rightarrow
SubTask
\rightarrow
Option
\rightarrow
Model
\rightarrow
Planning Utility
\]

---

## vs Continual Learning

普通 continual learning 重点：

\[
\theta_t
\rightarrow
\theta_{t+1}
\]

OaK-inspired WM：

\[
(\theta_t,\mathcal{K}_t)
\rightarrow
(\theta_{t+1},\mathcal{K}_{t+1})
\]

重点是：

> **knowledge structure changes.**

---

## vs Hierarchical World Models

不要宣称 hierarchy 本身是创新。

你的论点应该是：

> hierarchy is **not fixed**; it is continually constructed and evaluated from experience.

关键词：

> **continually constructed abstraction hierarchy**

而不是：

> hierarchical world model。

---

# 16. 建议提出的新定义

可以在正文中用一个 Definition Box。

## Definition — Continually Growing World Model

> A **continually growing world model** is a predictive model whose representational and temporal abstractions are not fixed at deployment. In addition to updating predictive parameters, the agent can construct, evaluate, retain, revise, and remove reusable predictive abstractions from ongoing experience, with their utility assessed partly by downstream planning.

数学上：

\[
W_t =
(\theta_t,\mathcal{F}_t,\mathcal{O}_t,\mathcal{M}_t)
\]

持续学习：

\[
W_{t+1}
=
\mathcal{U}
(
W_t,
e_{t:t+\Delta}
)
\]

且允许：

\[
|\mathcal{F}_{t+1}|
\neq
|\mathcal{F}_{t}|
\]

\[
|\mathcal{O}_{t+1}|
\neq
|\mathcal{O}_{t}|
\]

这就把：

> continual parameter learning

提升成：

> continual structural learning。

---

# 17. 最小 formalism

2–4 页不要堆数学。

只保留 3 组公式。

## Formula 1 — Primitive world model

\[
p_\theta(z_{t+1},r_{t+1}\mid z_t,a_t)
\]

---

## Formula 2 — Option model

\[
p_{\psi_i}
(
z_{t+\tau},
R_{t:t+\tau},
\tau
\mid
z_t,o_i
)
\]

---

## Formula 3 — Growing knowledge structure

\[
\mathcal{K}_{t+1}
=
\operatorname{UpdateStructure}
(
\mathcal{K}_t,
\mathcal{E}_t,
U
)
\]

其中：

- \(\mathcal{E}_t\)：new experience；
- \(U\)：predictive / planning utility；
- \(\mathcal{K}\)：feature-option-model knowledge set。

足够了。

---

# 18. 关键词

建议：

- Continual World Models
- World Models
- OaK Architecture
- Continual Reinforcement Learning
- Temporal Abstraction
- Options
- Model-Based Reinforcement Learning
- Continual Learning
- Planning
- Open-Ended Learning

如果只能选 5 个：

> Continual World Models; OaK Architecture; Temporal Abstraction; Model-Based Reinforcement Learning; Planning

---

# 19. 参考文献骨架

第一版至少覆盖以下文献群。

## OaK / Alberta Plan

1. Sutton — **The OaK Architecture: A Vision of SuperIntelligence from Experience**, NeurIPS 2025 / RLC 2025 invited talk.
2. Sutton et al. — **The Alberta Plan for AI Research**.
3. Sutton et al. — **Reward-Respecting Subtasks for Model-Based Reinforcement Learning**.

## Options / temporal abstraction

4. Sutton, Precup, Singh — options framework.
5. Bacon, Harb, Precup — **The Option-Critic Architecture**.

## World models

补：

- Dreamer 系列；
- model-based RL / latent dynamics；
- generative world models；
- planning with learned world models。

## Continual learning / continual world models

必须重点检索：

- continual adaptation of latent dynamics；
- online / streaming world models；
- memory and replay；
- catastrophic forgetting in world models；
- world models under distribution shift。

注意：

> 最终投稿前一定要专门做一轮 “OaK + World Model / Options + Continual World Model / Continually Growing Model / Structural Continual Learning” 撞题检索。

---

# 20. 与 NeurIPS 2026 CWM Workshop Scope 的对齐

Workshop 官方核心命题可以概括为：

> world models should change when the world changes.

你的 paper 更进一步提出：

> **When the world changes, the model may need to change not only its parameters, but also the abstractions through which it represents, predicts, and plans.**

你的内容与 Workshop scope 对齐点：

| Workshop concern | 本文对应 |
|---|---|
| updating latent dynamics | continual base WM update |
| learning from new experience | continual FC-STOMP |
| persistent model error | trigger feature construction |
| distribution shift | abstraction growth |
| retaining useful knowledge | utility-based retention |
| catastrophic forgetting | stable reusable abstractions |
| adaptation efficiency | option reuse |
| active interaction | subtask / option acquisition |
| continual planning | option-model planning |

---

# 21. 推荐最终文章结构

```text
Title

Abstract

1. Introduction
   - frozen structure problem
   - parameter adaptation is not enough
   - OaK as a blueprint
   - contributions

2. Beyond Parameter-Level Continual World Modeling
   - fixed state abstraction
   - fixed temporal abstraction
   - structural continual learning definition

3. An OaK-Inspired Continually Growing World Model
   3.1 Feature Construction
   3.2 SubTasks
   3.3 Options
   3.4 Option Models
   3.5 Planning
   3.6 Utility-Driven Growth and Pruning

   Figure 1: architecture

4. Testable Hypotheses and Research Agenda
   - temporal abstraction
   - distribution shift
   - retention / forgetting
   - planning utility

5. Discussion and Open Questions
   - growth trigger
   - bounded complexity
   - utility
   - relation to full OaK

6. Conclusion
```

---

# 22. 四页篇幅预算

建议：

| 内容 | 预算 |
|---|---:|
| Abstract | 0.15 page |
| Introduction | 0.75 page |
| Problem formulation | 0.55 page |
| OaK-WM framework | 1.30 page |
| Figure | 0.45 page |
| Hypotheses / agenda | 0.55 page |
| Discussion + conclusion | 0.25 page |
| References | 按模板处理 |

如果最终只有 2 页：

优先保留：

1. Introduction；
2. Structural continual WM definition；
3. FC-STOMP mapping；
4. 主图；
5. 3 个 hypotheses。

删掉长 Related Work。

---

# 23. 写作风格

## 应该像

> 一个明确指出领域缺口，并提出新研究路线的 NeurIPS workshop position paper。

## 不应该像

- 中文博士开题报告；
- OaK 教程；
- Sutton 思想介绍；
- 自动驾驶项目申请书；
- 大而全的 World Model survey；
- 没有实验却伪装成 full technical paper。

语气：

> assertive but bounded.

例如：

✅ **We argue that...**  
✅ **We propose...**  
✅ **We hypothesize...**  
✅ **We outline...**  
✅ **We view OaK as...**

少用：

❌ **We demonstrate...**  
❌ **We prove...**  
❌ **Our method outperforms...**

除非后面真的有实验。

---

# 24. 最关键的 Reviewer Question

投稿前必须保证论文能回答以下问题：

### Q1
**Why does a continual world model need structural growth instead of only online fine-tuning?**

### Q2
**Why OaK rather than generic hierarchical RL?**

### Q3
**What exactly is being added to the world model?**

答案必须是：

> continually constructed features + options + predictive option models.

### Q4
**What makes an abstraction useful?**

答案：

> not prediction alone; downstream planning utility is part of the criterion.

### Q5
**How can the proposal be falsified?**

答案：

> through explicit hypotheses on planning horizon, adaptation speed, forgetting, reuse, and structural growth.

---

# 25. 最值得成为论文“记忆点”的三句话

## 句子 1

> **Continual world modeling should mean more than continually updating the parameters of a fixed predictor.**

## 句子 2

> **An OaK-inspired world model can grow not only what it predicts, but also the abstractions over which prediction and planning are performed.**

## 句子 3

> **The question is not only how a world model keeps learning, but how it keeps deciding what knowledge is worth learning and retaining.**

这三句话基本就是整篇文章的思想骨架。

---

# 26. 不建议第一稿加入的内容

为了保证 2–4 页聚焦，第一稿先不要展开：

- autonomous driving 完整 architecture；
- diffusion / video generation 细节；
- VLA；
- LLM；
- vehicle-road-cloud；
- multi-agent communication；
- complete OaK per-weight meta-learning 实现；
- 大规模 benchmark；
- 企业应用；
- 专利；
- 过长的 OaK 历史。

这些可以留给后续 full paper。

---

# 27. 后续完整技术论文可以怎么长出来

这篇 Workshop paper 是母命题：

\[
\boxed{
Fixed\ World\ Model
\rightarrow
Continually\ Growing\ World\ Model
}
\]

之后可以拆成：

## Paper A
**Option-Conditioned World Models for Temporally Abstract Planning**

验证：

\[
a_t
\rightarrow
o_t
\]

是否提高长时域 planning efficiency。

---

## Paper B
**Continual Option Discovery for Growing World Models**

验证：

\[
\mathcal{O}_t
\rightarrow
\mathcal{O}_{t+1}
\]

能否应对新任务 / 新分布。

---

## Paper C
**Utility-Driven Structural Plasticity in World Models**

研究：

\[
create
\leftrightarrow
retain
\leftrightarrow
prune
\]

解决无界增长。

---

## Paper D
**OaK-Drive**

最后再把完整框架落到 autonomous driving world model。

这样 Workshop Idea Paper 是：

> **理论起点 / public intellectual timestamp / research agenda**

而不是终点。

---

# 28. 当前投稿版的“最小完成标准”

在真正开始写 full text 前，只要先完成下面 8 项：

- [ ] 最终标题
- [ ] 一句话 thesis
- [ ] Structural Continual World Modeling 定义
- [ ] OaK → WM 映射表
- [ ] primitive model vs option model 两个公式
- [ ] Figure 1
- [ ] 3–5 个 hypotheses
- [ ] 15–25 篇核心参考文献

做到这些，就已经可以展开一篇完整的 2–4 页 Idea Track paper。

---

# 29. 推荐写作顺序

不要从 Introduction 开始。

按以下顺序：

```text
1. Figure 1
      ↓
2. 一句话 thesis
      ↓
3. OaK → WM mapping
      ↓
4. 3 个核心公式
      ↓
5. Hypotheses
      ↓
6. Section 3 Architecture
      ↓
7. Section 2 Gap
      ↓
8. Introduction
      ↓
9. Abstract
      ↓
10. Conclusion
```

这样最不容易写散。

---

# 30. 当前版本最终定位

这篇 Workshop paper 最好被理解成：

> **一篇提出新 continual world-model research formulation 的 OaK-inspired position / idea paper。**

它不需要现在证明：

> “OaK World Model 已经成功。”

它需要做到的是让读者接受：

1. 只持续更新参数，对 continual world modeling 的定义可能太弱；
2. world model 应该能够持续形成新的 state / temporal abstractions；
3. Sutton 的 FC-STOMP 为这个过程提供了一个非常自然的架构蓝图；
4. option models 可以成为连接 temporal abstraction 与 world-model planning 的关键对象；
5. abstraction 应该由 prediction + planning utility 共同评价；
6. 这个命题可以通过明确实验被证伪。

最终希望审稿人读完后记住的不是：

> “有人把 Sutton 的 OaK 用到了 World Model。”

而是：

> **“Continual world models may need to grow their knowledge structure, not just update their weights—and OaK gives us a concrete way to think about how.”**

---

# 资料与投稿信息核对

- NeurIPS 2026 **Continual World Models Workshop**：Idea Track 接受 2–4 页 focused research ideas、position/vision papers、new problem formulations 和 early directions；non-archival；通过 OpenReview 投稿；当前官方截止日期为 **29 Aug 2026, Anywhere on Earth**。
- Richard Sutton 在 RLC 2025 / NeurIPS 2025 对 OaK 的公开描述：OaK 是 model-based RL architecture，强调所有组件 continual learning、per-weight meta-learned step sizes，以及 **Feature Construction → SubTask → Option → Model → Planning (FC-STOMP)** 的状态与时间抽象构造过程。
- Sutton 等关于 reward-respecting subtasks 的工作为 SubTask → Option → Option Model → Planning 这条逻辑提供了直接理论背景。
