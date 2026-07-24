# 协助 Agent 指导（AGENTS.md）

本文件写给参与本项目信息收集的协助 agent。开始任何调研或写入前，先读这里，再按下面的"权威文件地图"找到对应标准与格式。

## 0. 一句话背景

这是一个 public 的家庭搬家决策项目：在北汽蓝谷总部（北京经开区东环中路 5 号）通勤圈内，为一个计划长期租住、2026 年上小班的家庭找到合适的整租房源。你的工作是**按统一标准收集候选小区的结构化信息**，不做最终决策。

## 1. 权威文件地图（先认准，别找错地方）

| 你想找/改的东西 | 去这个文件 | 说明 |
|---|---|---|
| **选房标准**（硬门槛、一票否决、评分权重、通勤口径） | **[DECISION-MATRIX.md](DECISION-MATRIX.md)** | **唯一权威来源（SSOT）**。标准只在这里定义，别处只引用。改标准只改这里。 |
| 家庭需求与项目原则 | [README.md](README.md) | 需求段引用 SSOT，不复制标准 |
| 调研方法与扇区划分 | [research/expanded-candidates-plan.md](research/expanded-candidates-plan.md) | 怎么发现候选、扇区/锚点、工具口径 |
| 调研写作规范 | [research/README.md](research/README.md) | 事实/样本/推断三分、来源优先级 |
| 全扇区候选结论 | [research/expanded-candidates.md](research/expanded-candidates.md) | 已调研候选的详细档案 |
| 候选结构化数据 | [data/candidates.csv](data/candidates.csv) | 11 列，见下方字段契约 |
| 汇报用总览 | [docs/project-reference.html](docs/project-reference.html) | 自包含 HTML，改结论后需同步 |
| 任务进度 | [TASKS.md](TASKS.md) / GitHub issues | |

**关键规则：标准以 DECISION-MATRIX.md 为准。** 如果你发现某份报告里的标准与它冲突，以 DECISION-MATRIX.md 为准，并提醒修正那份报告。

## 2. 标准工作流（每个候选/扇区重复）

1. 先读 [DECISION-MATRIX.md](DECISION-MATRIX.md) 的硬门槛与一票否决项。
2. 按 [research/expanded-candidates-plan.md](research/expanded-candidates-plan.md) 的扇区/锚点发现候选小区。
3. 对每个候选，先过硬门槛（面积/租金/房龄≥2010/70年产权住宅/平峰≤30分钟）——不达标直接标排除并注明原因。
4. 达标的按"字段契约"采集数据，区分【事实】/【平台样本】/【推断】。
5. 结论写入 `research/expanded-candidates.md`，结构化行写入 `data/candidates.csv`。
6. 走分支 → PR → 合并流程提交（见第 5 节）。

## 3. CSV 字段契约（data/candidates.csv，共 11 列）

顺序固定，每行必须正好 11 列，**字段内不得出现裸逗号**（用"至""加"等替代，或去掉分隔）：

`candidate,district,education_admin,status,target_rent_cny,min_area_sqm,commute_baseline,kindergarten_status,parking_status,key_risk,last_verified`

- `status` 取值建议：`推荐` / `备选` / `观察` / `谨慎` / `淘汰` / `不推荐`（可加"待核实"后缀）
- `last_verified` 用 `YYYY-MM-DD`
- 写完用 `awk -F, '{print NF}'` 校验每行列数为 11。

## 4. 数据纪律（三分法 + 来源）

- **【事实】**：来自政府/学校/企业官网、平台"小区概况"页、百度百科，**必须附可点击链接**。
- **【平台样本】**：房源挂牌，**必须标抓取日期**，不把单套当市场均价。
- **【推断】**：基于坐标/路网/常识的分析，**必须显式标注**"估算/推断，非实测"。
- 来源优先级：政府/学校/企业官网 > 地图与正规房产平台 > 媒体与社区经验。
- 通勤：当前无高德 API，用路网几何/地图网页估算，**一律标注"非实时高峰"**；最终否决与定级须以工作日早高峰实测为准。

## 5. 提交流程（协作者身份）

- 本仓库通过协作者身份直接推分支：**新建分支 → 提交 → 开 PR → 合并**，不直接推 main。
- 分支名建议 `research/<扇区或主题>`。
- 提交信息用中文，说明改了什么。
- 增量写入：每批完成即提交，不等全部跑完。
- 改了结论后，记得同步 `docs/project-reference.html`（汇报材料）与 `data/candidates.csv`，避免三者打架。

## 6. 信息安全（public 仓库，硬红线）

**不得写入**：孩子姓名/生日/身份证号、户籍页、居住证、租赁合同、车牌、电话号码（园所公开咨询电话除外）、精确家庭住址、任何账号凭据。实际申请材料只在本地保存。

## 7. 本项目已知陷阱（前人踩过，别重复）

1. **预售/规划年 ≠ 竣工交房年**：平台常把两者并列成"2010-2021"区间，判房龄以竣工交房年为准。
2. **商住公寓陷阱**：40/50 年产权、4.5 米 LOFT、"可注册办公"的房子学位大概率不可用，触发一票否决——马驹桥/次渠多个盘有此问题，务必核对楼栋为 70 年住宅。
3. **行政区 ≠ 教育管理口径**：如南海家园行政属大兴瀛海镇，但可能走经开区学区——两套平台、两套材料，必须双向电话确认。
4. **"服务范围" ≠ "一定录取"**：公办园对市场租房家庭通常排末位批次，且常要求镇域社保满 X 年，别把"附近有园"当成能上。
5. **平台库存滞后**：挂牌可能已租/重复/下架，签约前须电话确认在租状态。
