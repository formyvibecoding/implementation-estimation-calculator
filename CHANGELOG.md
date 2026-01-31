# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.4.1] - 2026-01-31

### Added
- 产品参数完整数据：从Excel导入所有16个参数字段
  - 基本信息：型号名称、类型描述、额定负载、举升高度
  - 门架与货叉：门架最大提升高度、标准货叉尺寸、货叉最低位离地高度、货叉标准外间距
  - 性能参数：到位精度、空载/满载速度、爬坡能力、巷道空间、外形尺寸、自重
  - 电池与充电：标配容量、续航时间、充电时间、充电桩型号、充放比
- 编辑模态框扩展：分组显示所有参数，支持完整编辑

### Changed
- 编辑模态框布局优化：按"基本信息"、"门架与货叉"、"性能参数"、"电池与充电"分组显示
- 产品卡片显示保持简洁（4个关键参数），编辑时可修改全部参数

---

## [1.4.0] - 2026-01-31

### Added
- 产品库全面升级：从Excel导入26个完整产品型号
  - 平衡重式叉车：APe15, APx20, AP20, AP30, AP60
  - 料箱搬运车：AL05
  - 堆高车：AM15, AS15, AS20, AS30
  - 窄巷道堆高车：ASK15, ASK20, ASK30
  - 电动叉车：AE10, AE15, AE20, AE30
  - 前移式叉车：AR10, AR15, AR20, ARK15, ARV15
  - 托盘搬运车：TP30, TP60, TP100, TP150
- 产品选择模块横向滚动设计（苹果/Notion风格UX）
  - 单行展示所有产品卡片
  - 平滑横向滚动
  - 左右滚动指示器按钮
  - 边缘渐变效果
- 产品搜索功能
  - 支持按型号、类型搜索
  - 实时过滤显示
  - 一键清除搜索
- 产品参数修改持久化
  - 修改后的参数保存到localStorage
  - 重新打开页面保持最新修改值

### Changed
- 产品展示从系列改为直接显示型号
- 产品卡片简化：仅显示4个关键参数（定格负荷、速度、举升高度、充放比）
- 编辑模态框：移除"型号"和"举升速度"字段

### Removed
- 删除举升速度参数（使用固定值0.1 m/s）
- 移除产品系列概念，改为直接型号

---

## [1.3.5] - 2026-01-31

### Changed
- 更新计算公式逻辑：
  - 充电数量：工作时充电开启时 = 理论数量 × (1/充放比)，关闭时 = 0
  - 建议投入 = Roundup(理论数量 + 充电数量)
  - 利用率 = 理论数量 / (建议投入 - 充电数量) × 100%
- 充电数量现在基于理论数量计算，不再基于建议投入
- 保留"利用率 > 90% 时允许用户手动修改建议投入"功能

---

## [1.3.4] - 2026-01-31

### Changed
- 充电数量显示格式优化：
  - 不再向上取整，显示真实计算结果
  - 去掉多余的尾随零（如 1.5 显示为 1.5，不是 1.50）
  - 公式保持不变：工作时充电开启时为"汇总数量 / 充放比"，关闭时为 0

---

## [1.3.3] - 2026-01-31

### Changed
- 交管耗时改为用户手动输入：
  - 在搬运流程输入表中新增"交管耗时"列（默认值15秒）
  - 移除公式自动计算逻辑（原公式：理论数量 × 0.5 × 15秒）
  - 移除公式编辑模态框中的"交管系数"和"交管单位时间"参数
  - 移除测算说明中的交管耗时公式描述

---

## [1.3.2] - 2026-01-31

### Added
- 建议投入数量可编辑功能：
  - 当系统自动计算的利用率超过 90% 时，"建议投入机器人数量"输入框变为可编辑状态
  - 显示"（可编辑）"提示，输入框高亮显示
  - 用户可手动输入期望的建议投入数量
  - 输入后利用率和充电数量会根据用户输入值自动重新计算

---

## [1.3.1] - 2026-01-31

### Fixed
- 充电数量逻辑修正：
  - 当"工作时充电"未选中时，充电数量固定为 0
  - 当"工作时充电"选中时，充电数量按公式计算并允许显示小数（不再向上取整）

---

## [1.3.0] - 2026-01-31

### Added
- "工作时充电" (Work-time Charging) toggle switch in efficiency calculation section
  - When enabled: 建议投入 = ceil(理论数量 × (1 + 1/充放比))
  - When disabled: 建议投入 ensures 利用率 ≤ 90%
- Utilization rate (利用率) display card - now a calculated value instead of user input
  - Formula: 利用率 = 理论数量 / (建议投入 - 充电数量) for work-time charging mode
  - Formula: 利用率 = 理论数量 / 建议投入 for standard mode
  - Automatically ensures utilization rate does not exceed 90%

### Changed
- Charging count calculation now uses robot's charge ratio from configuration
- Removed manual utilization rate input - now auto-calculated
- Improved calculation logic based on Excel spreadsheet formulas

---

## [1.2.0] - 2026-01-31

### Added
- Formula parameter editing feature in efficiency calculation results section
  - Edit button next to "第三步：效率测算结果" title (same UX as robot parameter editing)
  - Modal dialog for editing formula parameters:
    - 取放货耗时 (Pick/Put time per action) - default 20s
    - 工步通讯 (Communication time per action) - default 5s
    - 栈板识别 (Pallet recognition time) - default 15s
    - 转弯耗时 (Turn time per turn) - default 5s
    - 设备对接 (Docking time per dock) - default 10s
    - 交管系数 (Traffic coefficient) - default 0.5
    - 交管单位时间 (Traffic time per unit) - default 15s
  - Formula parameters are persisted in localStorage
  - Real-time recalculation after parameter changes

---

## [1.1.0] - 2026-01-31

### Changed
- Switched to pure Chinese interface (removed Japanese text)
- Changed step order: Flow input is now Step 1, Robot selection is Step 2
- Updated robot types:
  - Removed MP Series
  - Added AR Series (AR10/AR15/AR20) - Reach truck (1000kg, 1.5/1.0 m/s, 1600mm lift)
  - Added APe Series (APe15) - Counterbalance forklift (1500kg, 1.5/1.5 m/s, 205mm lift)

### Added
- Robot parameter editing feature - click edit button on any robot card to modify:
  - Series name and models
  - Type description
  - Load capacity
  - Speed (empty/loaded)
  - Lift height
  - Charge ratio
  - Lift speed
- Modal dialog for parameter editing
- Real-time card update after parameter changes

### Fixed
- Improved user workflow by placing flow input before robot selection

---

## [1.0.0] - 2026-01-31

### Added
- Initial release of Robot Calculator System (机器人数量测算系统)
- Robot type selection module with 3 series:
  - TP Series (TP30/TP60/TP100/TP150) - Pallet transport vehicles
  - MP Series (MP10) - Stacker
  - APx Series (APx20/APx30) - Counterbalance forklift
- Multi-flow input support with dynamic row management
- Real-time efficiency calculation including:
  - Pick/put time (取放耗时)
  - Lift time (举升耗时)
  - Communication time (工步通讯)
  - Pallet recognition time (栈板识别)
  - Running time (运行耗时)
  - Turning time (转弯耗时)
  - Docking time (对接耗时)
  - Traffic control time (交管耗时)
- Result display with summary cards showing:
  - Recommended robot count
  - Total transport capacity
  - Charging station count
- Detailed result table with all calculated metrics
- Utilization rate adjustment (default 85%)
- Bilingual interface (Chinese/Japanese)
- Responsive design for different screen sizes
- Design style based on Robot List HTML templates

### Technical Details
- Pure HTML + CSS + JavaScript implementation
- No external dependencies required
- Single file deployment
- Mobile-friendly responsive layout
