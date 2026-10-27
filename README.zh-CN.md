# some_skills

由 [zhulei227](https://github.com/zhulei227) 维护的 AI 助手技能（skill）与项目笔记合集。

[English](README.md)

## 目录内容

### `kaggle_solutions/` —— Kaggle 方案库技能

一个 [CodeBuddy](https://www.codebuddy.ai) 技能，用于检索、离线阅读并借鉴 **721 个 Kaggle 竞赛 / 4700+ 条按名次整理的方案链接**（镜像自 [kaggle.farid.one](https://kaggle.farid.one)，数据源仓库 [faridrashidi/kaggle-solutions](https://github.com/faridrashidi/kaggle-solutions)）。

**核心特性**

- **离线优先**：完整元数据（721 个竞赛）与约 1280 页金/银/铜牌方案正文保存在 `kaggle_solutions/data/`，所有查询本地完成，无需联网
- **业务匹配**：给定业务描述或项目目录，自动匹配历史相似竞赛，从优胜方案中挖掘高频技术并生成可执行的优化建议报告（`match_task.py`）
- **语义搜索**：基于 numpy 的本地 TF-IDF 向量索引（`embedding_index.py`），覆盖竞赛元数据与方案正文，与关键词匹配混合排序
- **中文兼容**：内置中文→英文概念映射，中文描述、中文文档、中文搜索开箱即用
- **自动抓取**：基于真实浏览器的方案正文下载器，支持断点续传（`download_solutions.py`，需 playwright-cli + Edge/Chrome）

**快速开始**

```powershell
cd kaggle_solutions

# 匹配当前业务并生成优化建议报告（中英文均可）
python scripts\match_task.py "信用卡反欺诈检测" --metric auc --out report.md
python scripts\match_task.py --dir path\to\your\project --out report.md

# 离线阅读优胜方案 / 全文检索 / 元数据查询
python scripts\kaggle_solutions.py read 721 1
python scripts\kaggle_solutions.py grep "pseudo label"
python scripts\kaggle_solutions.py search image segmentation --kind Research --solutions

# 语义搜索
python scripts\embedding_index.py "零售销量预测 时间序列" --corpus solutions --top 10
```

完整工作流与数据 schema 见 `kaggle_solutions/SKILL.md`。

**作为技能启用**：将 `kaggle_solutions/` 复制到 `~/.codebuddy/skills/`（用户级）或 `.codebuddy/skills/`（项目级）。

### `easymlops/` —— EasyMLOps 技能

[EasyMLOps](https://github.com/zhulei227/EasyMLOps)（MLOps 工具库）的笔记、参考文档与示例 wheel，详见 `easymlops/SKILL.md`。

## 仓库结构

```
some_skills/
├── kaggle_solutions/          # Kaggle 方案库技能
│   ├── SKILL.md               # 技能清单：工作流、数据 schema
│   ├── scripts/               # 查询 / 下载 / 匹配 / 语义索引 CLI
│   └── data/                  # 离线快照（元数据、方案正文、向量索引）
└── easymlops/                 # EasyMLOps 笔记与参考文档
```

## 离线方案库更新

```powershell
cd kaggle_solutions
python scripts\download_solutions.py --status      # 查看抓取进度（rank≤3）
python scripts\download_solutions.py --max-rank 3  # 断点续传/继续抓取
python scripts\kaggle_solutions.py refresh         # 刷新元数据
python scripts\embedding_index.py build            # 重建向量索引
```

## 说明

- `logs/` 与 `.codebuddy/` 为工作数据，未纳入版本跟踪
- Kaggle 方案正文抓取自公开可访问页面，仅供个人学习，版权归原作者所有
