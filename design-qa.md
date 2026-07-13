# 上游编辑弹窗 Design QA

## 对照目标

- 视觉系统基准：`images/add-upstream.png`
- 实现截图：`.planning/upstream-dialog-default.png`
- 补充状态：`.planning/upstream-dialog-selected.png`、`.planning/upstream-dialog-advanced.png`
- 完整对照：`.planning/upstream-dialog-comparison.png`
- 局部/状态对照：`.planning/upstream-dialog-states-comparison.png`
- Viewport：768 px 宽桌面弹窗；默认态高 626 px，滚动态高 842 px
- 状态：新增上游；全部模型、仅指定模型、高级设置展开

旧图用于现有 Token Proxy 弹窗的字体、控件、边框、密度和视觉语气基准。用户批准的结构变化是“渐进分区”：连接和模型访问默认可见，高级设置默认折叠。旧图为英文深色主题，实现截图为中文浅色主题；主题和 locale 差异不作为缺陷，组件需继续使用项目语义 token。

## 完整视图对照

- 信息架构：连接、模型限制和低频高级项已形成清晰层级；默认态不再平铺 ID、代理、优先级、映射、兼容开关和 Header 复写。
- 布局与间距：标题区、字段网格、分隔线、分区间距和底部操作区节奏一致；默认态无需滚动，滚动态保留固定操作区。
- 字体：沿用项目 sans 字体栈；标题、分区标题、字段标签和辅助文案权重层级清楚；letter spacing 未额外压缩，无异常换行或截断。
- 颜色：表单、边框、弱化文案、主按钮、禁用状态和选中状态均使用现有语义 token；浅色截图对比度正常，未引入独立色板或渐变。
- 图标：搜索、同步、添加、移除、展开、帮助和密码可见性图标来自现有图标库，尺寸与线宽一致，无手绘 SVG 或文本符号替代。
- 图片资产：此工具弹窗不包含产品图片、插画或 Logo，不需要生成或替换位图资产。
- 文案：明确区分“全部模型”“仅指定模型”“模型访问”和“模型映射”；空白名单含义可直接理解。

## 局部与状态对照

- 仅指定模型：已选计数、标签、搜索、同步、候选勾选、手工输入和添加按钮完整呈现；单模型状态不改变工具栏尺寸。
- 高级设置：ID、代理、优先级、可转格式、模型映射、Header 复写和兼容开关保留；展开后字段对齐，无嵌套卡片或无意义装饰。
- 滚动与裁切：768 x 842 截图中内容区独立滚动，底部取消/保存持续可用；无控件重叠、文字溢出或横向裁切。
- 交互：全部/指定分段、模型勾选、同步、手工添加、移除、高级展开和状态开关均有可操作控件及语义标签。
- 可访问性：字段具有关联标签或 `aria-label`；原生 `details/summary` 支持键盘展开；焦点样式沿用项目组件。桌面 Tauri 弹窗不以移动端为目标，本次未做手机 viewport 验收。

## Findings

无可执行的 P0、P1 或 P2 问题。

P3：深色主题未在本轮浏览器截图中单独捕获。实现完全使用现有语义 token，未发现硬编码浅色色值；可在后续桌面发行验收中补一张真实 Tauri 深色截图。

## 本轮修复记录

- 将平铺长表单改为连接、模型访问、高级设置三个渐进分区。
- 将状态移到标题区，将低频项收入默认折叠的高级设置。
- 新增白名单模式切换、模型搜索/勾选、上游同步、手工添加和移除。
- 移除“拉取模型后创建 `model -> model` 映射”的语义混用。
- 保留原有高级能力和固定底部操作区。

## 实现检查清单

- [x] 默认态信息密度降低
- [x] 白名单关键交互完整
- [x] 高级能力可发现且默认收起
- [x] 字体、间距、颜色、图标和文案通过
- [x] 768 px 桌面 viewport 无重叠或裁切
- [x] 无剩余 P0/P1/P2

## 全选补充验收

- Source visual truth：`.planning/upstream-dialog-selected.png`
- Implementation screenshot：`.planning/upstream-dialog-select-all-768.png`
- Checked state：`.planning/upstream-dialog-select-all-checked.png`
- Full-view comparison：`.planning/upstream-dialog-select-all-comparison.png`
- Viewport：768 x 842，新增上游、仅指定模型、两个候选中选中一个
- Focused evidence：`.planning/upstream-dialog-select-all.png`，用于检查半选减号、列表行对齐和文案

对照确认新增表头复选框保持原列表宽度、边框、字体和 16 px 控件规格；半选态使用现有 primary token 与 Lucide `Minus`，全选态使用 `Check`。表头占用固定滚动区内一行，不改变搜索、手工添加或底部操作区尺寸。768 px 下无重叠、横向裁切、异常换行或文本溢出。

交互验证：半选时 DOM 为 `aria-checked="mixed"` / `data-state="indeterminate"`；点击后两个候选均为 checked，批量文案切换为“取消全选”。自动化测试另覆盖搜索后只清除当前结果。无新增图片资产，不涉及图片质量变化。

本轮无 P0/P1/P2/P3 发现。

final result: passed
