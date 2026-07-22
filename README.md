# lovstudio-bp

![Version](https://img.shields.io/badge/version-0.1.0-CC785C)
![License](https://img.shields.io/badge/license-MIT-173D2A)
![Free](https://img.shields.io/badge/Free-open--source-5DC98F)

把零散项目资料变成投资人看得懂、愿意继续聊的 BP：有证据的叙事、专业图表、PPTX/PDF，以及一份不会自我吹捧的审稿报告。

它不是“套模板写十个章节”。它把 BP 当成一条可验证的生产链：

```text
项目资料 → 证据账本 → 投资叙事 → 12–15 页大纲
        → 专业图表与真实素材 → PPTX / PDF → 逐页质检报告
```

由 [LovStudio](https://lovstudio.ai) 免费开源，属于
[lovstudio general skills](https://github.com/lovstudio/general-skills)。

## 安装

```bash
npx lovstudio skills add bp -g -y
```

或直接克隆：

```bash
git clone https://github.com/lovstudio/bp-skill \
  "${LOVSTUDIO_SKILLS_INSTALL_DIR:?请先设置 LOVSTUDIO_SKILLS_INSTALL_DIR}/lovstudio-bp"
```

最终 PPTX/PDF 生成依赖免费的
[`lovstudio-any2deck`](https://github.com/lovstudio/any2deck-skill)。基础工作区和审稿脚本仅需 Python 3.8+，无第三方依赖。

## 使用

```text
$lovstudio-bp 根据当前项目资料做一份种子轮 BP
$lovstudio-bp 重写这份融资 PPT，让非技术投资人 10 秒理解产品
$lovstudio-bp 只审稿，给我逐页问题和修改优先级
$lovstudio-bp 做 12–15 页中文投资人 BP，不要问我问题，按推荐方案
```

也可先创建标准工作区：

```bash
python3 scripts/init_bp.py --name "Project Name" --stage seed --output ./business-plan
python3 scripts/audit_bp.py --input ./business-plan/outline.md \
  --output ./business-plan/reports/bp-review.md --strict
```

## 为什么它更像真正的 BP 工作流

- **先找证据，再写结论**：事实、推断、假设和缺口分开记录。
- **先解决 What，再谈 Vision**：一句话必须是产品定位，不是空泛口号。
- **每页只讲一个观点**：标题直接写结论，图表负责证明。
- **真实素材优先**：产品截图、用户现场、访谈、订单和业务流程优先于图库照片。
- **市场规模有口径**：TAM/SAM/SOM 必须能从买家、价格和渗透路径推导。
- **交付不是大纲**：默认给 PPTX、PDF、全稿预览、证据账本和审稿报告。
- **视觉必须验收**：逐页检查排版、图表、二维码、Logo、来源和文件名。

## 默认页面节奏

1. 一句话说清楚你是谁
2. 用户遇到的具体问题
3. 产品如何解决
4. 产品演示或核心体验
5. 为什么现在值得做
6. 已获得的真实验证
7. 商业模式
8. 市场规模与切入路径
9. 竞争与差异化
10. 增长计划
11. 团队为什么适合做
12. 融资金额、用途和下一阶段目标

允许扩展到 15 页，但每一页都必须争取一次投资人的注意力。

## 默认视觉标准

- 16:9 横版，白色或暖灰底，一个品牌主色。
- 深色标题、大字号结论、正文不小于 20 pt。
- 每页一个主观点，至少约三分之一留白。
- 咨询报告式可信度、产品发布会式清晰度、创业团队真实感。
- 图表有轴、单位、图例、日期和来源；概念图明确标“示意”。
- 避免装饰性卡片墙、无意义图标、渐变背景和伪精确数字。

## 公开案例：Yoda 种子轮 BP

Yoda 的初稿曾把产品写成“多智能体编程工作台”，投资人容易把它理解成又一个 Agent 团队工具。迭代后，材料从用户价值和新的人机关系出发，把单 Agent、多 Agent、团队模式降为实现方式，并用真实产品、开源迭代和商业验证缺口组织投资叙事。

![Yoda BP 封面](cases/yoda-bp-cover.png)

![Yoda BP 全稿预览](cases/yoda-bp-overview.png)

- [案例复盘](references/case-yoda.md)
- [案例质量报告](reports/yoda-bp-case-report.md)
- [通用审稿报告模板](assets/templates/bp-review.md)

## 输出结构

```text
business-plan/
├── brief.md                 # 融资任务与关键信息
├── evidence-ledger.md       # 事实 / 推断 / 假设 / 缺口
├── outline.md               # 逐页结论、证据和图表说明
├── assets/                  # Logo、截图、照片、二维码、数据
├── reports/bp-review.md     # 内容与视觉审稿报告
├── project-bp.pptx
├── project-bp.pdf
└── project-bp-preview.png
```

## 配置

Skill 不依赖任何作者本机路径。品牌和输出位置可以通过参数、环境变量或共享配置传入：

```bash
${LOVSTUDIO_SKILLS_PROFILE:-$HOME/.lovstudio/skills/profile.json}
```

支持：

| 变量 | 作用 |
|---|---|
| `LOVSTUDIO_BP_OUTPUT_DIR` | 默认 BP 输出目录 |
| `LOVSTUDIO_BP_BRAND_PROFILE` | 品牌资料文件 |
| `LOVSTUDIO_BP_DESIGN_GUIDE` | 设计规范文件 |
| `LOVSTUDIO_SKILLS_PROFILE` | 通用 LovStudio Skill 配置 |

详见 [references/user-config.md](references/user-config.md)。

## 报告分数

`audit_bp.py` 以 100 分评估：

| 维度 | 分值 |
|---|---:|
| 故事结构 | 25 |
| 投资人可读性 | 20 |
| 证据与数据卫生 | 25 |
| 图表与视觉规格 | 20 |
| 融资交付完整性 | 10 |

建议在生成 PPT 前达到 85 分以上，并清除所有阻断项。

## License

MIT
