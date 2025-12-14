# GW arXiv Daily（引力波每日 arXiv 追踪）

## 中文说明

### 1. 项目简介
本项目用于每天抓取 arXiv 的 `gr-qc/new` 与 `astro-ph/new`（可自行修改），在“New submissions + Cross-lists”中检索与引力波相关的条目，并生成：
- 当天批次的结构化 JSON 归档（永久保存）
- 一个可公开访问的静态网页，展示当天匹配结果、作者信息、以及累计统计

本项目不依赖本地电脑常驻运行：由 GitHub Actions 定时触发并在云端执行。

---

### 2. 功能特性
- **自动抓取**：抓取 arXiv 列表页（/new）并解析条目
- **关键词匹配**：匹配 GW(s) / gravitational-wave(s) / gravitational wave(s) 等关键词（可配置）
- **过滤 replaced**：不统计 “replaced” 条目
- **按批次日期归档**：以 arXiv 页面显示的 “Showing new listings for …” 日期为批次日期
  - 如果当天 arXiv 不更新，则不产生新批次、不写新数据
- **静态网页展示**：
  - 当日匹配列表（默认隐藏作者，按钮一键显示/隐藏）
  - 归档链接（默认展示最近 30 次更新批次的 JSON）
  - Top 作者（累计匹配次数）
  - 支持作者筛选（如你启用了对应按钮/交互逻辑）

---

### 3. 目录结构（示例）
├── scripts/
│ └── gw_watch.py # 主脚本：抓取、匹配、生成 docs
├── docs/
│ ├── index.html # GitHub Pages 首页（自动生成）
│ ├── .nojekyll # 禁用 Jekyll（自动生成）
│ └── data/
│ ├── 2025-12-12.json # 每个批次一个 JSON（自动生成、永久保存）
│ └── ...
├── requirements.txt
└── .github/
└── workflows/
└── daily.yml # GitHub Actions 工作流

### 4. 核心配置（你可能会改的地方）

#### 4.1 监控分类（LIST_PAGES）
在 `scripts/gw_watch.py` 中配置：
```python
LIST_PAGES = {
  "astro-ph": "https://arxiv.org/list/astro-ph/new",
  "gr-qc": "https://arxiv.org/list/gr-qc/new",
}

