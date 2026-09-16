# oh-my-rime（薄荷输入法）配置指南

> 基于 [oh-my-rime](https://github.com/Mintimate/oh-my-rime) 项目源码及官方文档 [配置覆写和定制](https://www.mintimate.cc/zh/guide/configurationOverride.html) 整理。

---

## 一、配置文件的两种类型

Rime 的配置总体分为两种：

| 类型 | 用途 | 典型文件 |
|------|------|----------|
| **输入法的应用配置**（客户端配置） | 设置客户端外观、皮肤、布局等 | `squirrel.yaml`（macOS）、`weasel.yaml`（Windows）、`ibus_rime.yaml`（Linux） |
| **输入法方案配置**（方案配置） | 设置输入方案内部行为，如翻页键、候选词个数、模糊拼音、词库等 | `default.yaml`（全局）、`rime_mint.schema.yaml`（全拼方案）、`double_pinyin_flypy.schema.yaml`（小鹤双拼方案）等 |

---

## 二、项目目录结构总览

```
Rime/
├── default.yaml                  # 全局输入方案配置
├── squirrel.yaml                 # macOS 鼠须管客户端配置
├── weasel.yaml                   # Windows 小狼毫客户端配置
├── ibus_rime.yaml                # Linux iBus 客户端配置
│
├── rime_mint.schema.yaml         # 薄荷拼音-全拼输入 方案配置
├── rime_mint.dict.yaml           # 薄荷拼音-全拼输入 词库索引
├── rime_mint_flypy.schema.yaml   # 薄荷拼音-小鹤混输 方案配置
├── double_pinyin_flypy.schema.yaml # 小鹤双拼-薄荷定制 方案配置
├── terra_pinyin.schema.yaml      # 地球拼音-薄荷定制 方案配置
├── wubi98_mint.schema.yaml       # 五笔98-五笔小筑 方案配置
├── wubi86_jidian.schema.yaml     # 五笔86-极点五笔 方案配置
├── t9.schema.yaml                # 仓九宫格-全拼输入 方案配置
│
├── melt_eng.schema.yaml          # 英文方案（被其他方案作为子翻译器调用）
├── melt_eng.dict.yaml            # 英文词库
│
├── symbols.yaml                  # 标点符号定义
├── terra_symbols.yaml            # 地球拼音标点符号
│
├── radical_pinyin.schema.yaml    # 反查：拆字拼音
├── radical_pinyin_flypy.schema.yaml # 反查：拆字拼音（小鹤双拼版）
├── stroke.schema.yaml            # 反查：笔画
│
├── rime.lua                      # Lua 脚本入口
│
├── dicts/                        # 词库目录
│   ├── rime_mint.base.dict.yaml       # 基础词库（万象）
│   ├── rime_mint.chars.dict.yaml      # 单字词库（万象）
│   ├── rime_mint.ext.dict.yaml        # 联想词库（万象）
│   ├── rime_mint.correlation.dict.yaml # 关联词库（万象）
│   ├── rime_mint.compatible.dict.yaml  # 兼容词库（万象）
│   ├── custom_simple.dict.yaml        # 自定义词库模板
│   ├── rime_ice.others.dict.yaml      # 雾凇拼音纠错词库
│   ├── rime_ice.en.dict.yaml          # 英文词库
│   ├── rime_ice.en_ext.dict.yaml      # 英文扩展词库
│   ├── rime_ice.cn_en.txt             # 中英混输词库
│   ├── other_emoji.dict.yaml          # Emoji 词库（已弃用）
│   ├── other_kaomoji.dict.yaml        # 颜文字词库
│   ├── wubi98_base.dict.yaml          # 五笔98词库
│   └── wubi86_core.dict.yaml          # 五笔86词库
│
├── lua/                           # Lua 脚本目录
│   ├── codeLengthLimit_processor.lua  # 限制拼音串最大长度
│   ├── super_preedit.lua              # 输入码显示全拼+音调
│   ├── corrector_filter.lua           # 错音错字提示
│   ├── autocap_filter.lua             # 英文自动大写
│   ├── reduce_english_filter.lua      # 降低英文单词优先级
│   ├── select_character.lua           # 以词定字
│   ├── shijian.lua                    # 时间/日期/农历/节日
│   ├── number_translator.lua          # 金额大小写
│   ├── mint_calculator_translator.lua # 计算器
│   ├── chineseLunarCalendar_translator.lua # 农历
│   ├── kp_number_processor.lua        # 小键盘数字处理
│   ├── auxCode_filter.lua             # 辅码滤镜
│   ├── tag_user_dict.lua              # 用户词典标记
│   ├── force_gc.lua                   # 强制GC
│   └── log.lua                        # 日志
│
├── opencc/                        # OpenCC 简繁转换配置
│   ├── emoji.json / emoji.txt
│   └── ...
│
└── plum/                          # 东风破（Rime 包管理器）配方
    └── full.recipe.yaml
```

---

## 三、各配置文件功能详解

### 3.1 客户端配置文件

#### `squirrel.yaml` — macOS 鼠须管前端配置

**生效范围**：仅 macOS 鼠须管客户端

**主要功能**：
- 皮肤/配色方案选择（`style/color_scheme`、`style/color_scheme_dark`）
- 候选词排列方向（`style/candidate_list_layout`：`stacked` 纵向 / `linear` 横向）
- 内嵌预编辑（`style/inline_preedit`）
- 字体、字号设置
- 窗口圆角、间距、阴影等外观
- 预设配色方案定义（`preset_color_schemes`）

**内置皮肤**：
- `mint_light_blue` / `mint_dark_blue` — 水鸭系列（默认）
- `mint_light_green` / `mint_dark_green` — 青涩系列
- 以及鼠须管自带的 `native`、`aqua`、`azure`、`luna`、`ink`、`lost_temple`、`dark_temple`、`psionics`

**自定义推荐**：创建 `squirrel.custom.yaml` 进行覆写

---

#### `weasel.yaml` — Windows 小狼毫前端配置

**生效范围**：仅 Windows 小狼毫客户端

**主要功能**：
- 皮肤/配色方案选择
- 字体设置（支持 fallback 字体链）
- 候选词排列方向（`style/horizontal`、`style/candidate_list_layout`）
- 内嵌预编辑、全屏模式、竖排文本等
- 布局微调（`style/layout`：边距、间距、圆角、阴影等）
- 针对特定应用的设置（`app_options`）
- 预设配色方案定义

**内置皮肤**：同鼠须管的水鸭系列和青涩系列，以及小狼毫自带的 `nord`、`aqua` 等

**自定义推荐**：创建 `weasel.custom.yaml` 进行覆写

---

#### `ibus_rime.yaml` — Linux iBus 前端配置

**生效范围**：仅 Linux iBus 客户端

**主要功能**：候选词方向、内嵌预编辑、光标类型、配色方案等

---

### 3.2 方案配置文件

#### `default.yaml` — 全局输入方案配置

**生效范围**：所有输入方案（全局）

**主要功能**：
- 方案列表（`schema_list`）：定义哪些输入方案被激活
- 方案切换快捷键（`switcher/hotkeys`）：默认 `Ctrl+~` 或 `Ctrl+Shift+~`
- 候选词个数（`menu/page_size`）：默认 6
- 快捷键绑定（`key_binder/bindings`）：翻页键、中英切换、简繁切换等
- 标点符号定义（`punctuator`）
- 西文模式切换键（`ascii_composer/switch_key`）
- 识别器规则（`recognizer/patterns`）：URL、Email 等

**重要**：薄荷输入法已实现 `default.yaml`，会自动覆盖 Rime 客户端自带的 `default.yaml`。

**自定义推荐**：创建 `default.custom.yaml` 进行覆写

---

#### `rime_mint.schema.yaml` — 薄荷拼音-全拼输入

**生效范围**：仅「薄荷拼音-全拼输入」方案

**主要功能**：
- 方案元信息（名称、版本、作者）
- 引擎组件（`engine`）：处理器、分词器、翻译器、过滤器
- 开关选项（`switches`）：中英切换、Emoji、全半角、声调显示、简繁切换
- 拼写规则（`speller/algebra`）：拼音匹配、简拼、自动纠错规则
- 词库引用（`translator/dictionary`）：指向 `rime_mint`
- 反查模块配置（五笔、笔画、拆字）
- 标点符号（`punctuator`）
- 快捷键绑定（`key_binder`）
- 候选词个数（`menu/page_size`）：**注意此处为 10，会覆盖 default.yaml 的 6**
- 拼音串最大长度（`codeLengthLimit_processor`）：默认 100
- Emoji 模块、简繁转换、语言模型等

**自定义推荐**：创建 `rime_mint.custom.yaml` 进行覆写

---

#### `double_pinyin_flypy.schema.yaml` — 小鹤双拼-薄荷定制

**生效范围**：仅「小鹤双拼-薄荷定制」方案

**主要功能**：与全拼方案类似，但拼写规则为小鹤双拼键位映射，额外包含：
- 鹤形拆字辅助滤镜（`chaifen_cc`）
- 小鹤双拼辅码/音形（`auxCode_filter`）

**自定义推荐**：创建 `double_pinyin_flypy.custom.yaml` 进行覆写

---

#### `rime_mint_flypy.schema.yaml` — 薄荷拼音-小鹤混输

**生效范围**：仅「薄荷拼音-小鹤混输」方案

**主要功能**：同时支持全拼和小鹤双拼输入

**自定义推荐**：创建 `rime_mint_flypy.custom.yaml` 进行覆写

---

#### `terra_pinyin.schema.yaml` — 地球拼音-薄荷定制

**生效范围**：仅「地球拼音-薄荷定制」方案

**主要功能**：支持声调输入（用 `- / < \` 输入四声），使用 `terra_pinyin.dict.yaml` 词库

**自定义推荐**：创建 `terra_pinyin.custom.yaml` 进行覆写

---

#### `wubi98_mint.schema.yaml` — 五笔98-五笔小筑

**生效范围**：仅「五笔98-五笔小筑」方案

**自定义推荐**：创建 `wubi98_mint.custom.yaml` 进行覆写

---

#### `wubi86_jidian.schema.yaml` — 五笔86-极点五笔

**生效范围**：仅「五笔86-极点五笔」方案

**自定义推荐**：创建 `wubi86_jidian.custom.yaml` 进行覆写

---

#### `t9.schema.yaml` — 仓九宫格-全拼输入

**生效范围**：仅「仓九宫格」方案（适用于仓输入法/元书输入法）

**特殊机制**：通过 `__include: rime_mint.schema.yaml:/` 继承全拼方案配置

**自定义推荐**：创建 `t9.custom.yaml` 进行覆写

---

### 3.3 辅助配置文件

| 文件 | 功能 |
|------|------|
| `rime_mint.dict.yaml` | 全拼方案的词库索引，引用 `dicts/` 下的各个词库文件 |
| `melt_eng.schema.yaml` | 英文输入方案，被其他方案作为子翻译器调用 |
| `symbols.yaml` | 标点符号和特殊符号定义（`/fh`、`/xq`、`/mj` 等） |
| `rime.lua` | Lua 脚本入口文件，加载 `lua/` 目录下的各脚本 |

---

## 四、配置优先级（覆写规则）

### 4.1 核心优先级链

对于**方案配置**，优先级从高到低为：

```
方案 .custom.yaml          ← 最高优先级（用户自定义覆写）
    ↓
方案 .schema.yaml          ← 方案自身配置
    ↓
default.custom.yaml        ← 全局自定义覆写
    ↓
default.yaml               ← 全局默认配置
    ↓
客户端自带的 default.yaml   ← 最低优先级（已被薄荷覆盖）
```

以「薄荷拼音-全拼输入」为例：

```
rime_mint.custom.yaml      ← 最高（用户创建）
    ↓
rime_mint.schema.yaml      ← 方案定义
    ↓
default.custom.yaml        ← 全局自定义
    ↓
default.yaml               ← 全局默认
```

### 4.2 客户端配置优先级

```
squirrel.custom.yaml       ← 最高（用户创建）
    ↓
squirrel.yaml              ← 薄荷提供的配置
    ↓
鼠须管内置默认配置          ← 最低
```

Windows 同理：`weasel.custom.yaml` > `weasel.yaml` > 小狼毫内置默认

### 4.3 关于 page_size 和 key_binder 的特殊说明

**这是最容易踩坑的地方！**

薄荷输入法为了兼容 [Rimetool](https://github.com/yanhuacuo/rimetool) 可视化配置工具，在**每个方案的 `.schema.yaml` 文件内冗余写入了 `menu`（含 `page_size`）和 `key_binder` 等配置**。

这意味着：
- 方案文件内的 `page_size` 和 `key_binder` **不会从 `default.yaml` 继承**
- 在 `default.custom.yaml` 中修改 `page_size` 或 `key_binder` **不会对方案生效**
- 必须在**对应方案的 `.custom.yaml`** 中覆写这些配置

例如：
- 修改全拼候选词个数 → 在 `rime_mint.custom.yaml` 中覆写
- 修改小鹤双拼候选词个数 → 在 `double_pinyin_flypy.custom.yaml` 中覆写
- 如果使用多个方案，需要为**每个方案分别创建 `.custom.yaml`**

---

## 五、自定义配置的标准语法

### 5.1 `.custom.yaml` 文件的基本格式

所有 `.custom.yaml` 文件必须使用 **`patch`** 关键字开头：

```yaml
patch:
  "路径/键名": 值
```

### 5.2 路径写法（关键！）

使用 `/` 分隔嵌套层级，并用**引号**包裹完整路径：

**正确写法**：
```yaml
patch:
  "menu/page_size": 9
  "style/color_scheme": mint_light_green
  "style/font_point": 18
```

**错误写法**（会清空整个父级）：
```yaml
patch:
  menu:
    page_size: 9       # 这会把 menu 内的其他配置全部清空！
  style:
    font_point: 18     # 这会把 style 内的其他配置全部清空！
```

### 5.3 覆写数组/列表

对于数组类型的配置，使用 `/@next` 追加元素：

```yaml
patch:
  "key_binder/bindings/@next":
    accept: "Control+Shift+E"
    toggle: emoji_suggestion
    when: always
```

### 5.4 一个文件只有一个 patch 节点

```yaml
# ✅ 正确：一个文件只有一个 patch
patch:
  "menu/page_size": 9
  "style/color_scheme": mint_light_green

# ❌ 错误：不能有多个 patch
```

---

## 六、常见自定义场景模板

### 6.1 修改候选词个数

**文件**：`rime_mint.custom.yaml`（以全拼为例）

```yaml
patch:
  "menu/page_size": 9
```

> 如果使用小鹤双拼，则创建 `double_pinyin_flypy.custom.yaml`，内容相同。

---

### 6.2 切换皮肤/配色方案

**macOS** — `squirrel.custom.yaml`：
```yaml
patch:
  "style/color_scheme": mint_light_green
  "style/color_scheme_dark": mint_dark_green
```

**Windows** — `weasel.custom.yaml`：
```yaml
patch:
  "style/color_scheme": mint_light_green
  "style/color_scheme_dark": mint_dark_green
```

---

### 6.3 修改候选词排列方向

**macOS** — `squirrel.custom.yaml`：
```yaml
patch:
  "style/candidate_list_layout": linear
```

**Windows** — `weasel.custom.yaml`：
```yaml
patch:
  "style/horizontal": true
```

> 注意：Windows 小狼毫上 `candidate_list_layout: linear` 可能无效，需要使用 `horizontal: true`。

---

### 6.4 开启模糊拼音

**文件**：`rime_mint.custom.yaml`

```yaml
patch:
  speller/algebra:
    - erase/^xx$/
    ## 模糊拼音
    - derive/^([zcs])h/$1/             # zh, ch, sh => z, c, s
    - derive/^([zcs])([^h])/$1h$2/     # z, c, s => zh, ch, sh
    - derive/([aei])n$/$1ng/           # en => eng, in => ing
    - derive/([aei])ng$/$1n/           # eng => en, ing => in
    - derive/([iu])an$/$lan/           # ian => iang, uan => uang
    - derive/([iu])ang$/$lan/          # iang => ian, uang => uan
    - derive/([aeiou])ng$/$1gn/        # dagn => dang
    - derive/([dtngkhrzcs])o(u|ng)$/$1o/  # zho => zhong|zhou
    - derive/ong$/on/                  # zhonguo => zhong guo
    - abbrev/^([a-z])[a-z]*$/$1/       # 简拼（首字母）
    - abbrev/^([zcs]h).+$/$1/          # 简拼（zh, ch, sh）
    ## 自动纠错规则（根据需要添加）
    # ...
```

> 注意：覆写 `speller/algebra` 时，需要把**完整的** algebra 列表写出来，因为这是整体替换。

---

### 6.5 修改拼音串最大长度

**文件**：`rime_mint.custom.yaml`

```yaml
patch:
  codeLengthLimit_processor: 100
```

默认值为 25，增大可输入更长拼音串，但可能增加卡顿风险。

---

### 6.6 自定义词库

**步骤**：

1. 在 `dicts/` 目录下创建自定义词库文件，如 `dicts/my_custom_dicts.dict.yaml`：

```yaml
# Rime dictionary
# encoding: utf-8
---
name: my_custom_dicts
version: "2025-10-01"
sort: by_weight
...
阿瓦隆	a wa long	915
自定义	zi ding yi	100
```

2. 创建 `rime_mint.custom.dict.yaml`（词库索引覆写）：

```yaml
---
name: rime_mint
version: "2025.07.06"
sort: by_weight
use_preset_vocabulary: false
import_tables:
  - dicts/custom_simple
  - dicts/rime_mint.chars
  - dicts/rime_mint.base
  - dicts/rime_mint.correlation
  - dicts/rime_mint.compatible
  - dicts/rime_mint.ext
  - dicts/other_kaomoji
  - dicts/rime_ice.others
  - dicts/my_custom_dicts       # 新增的自定义词库
...
```

3. 在 `rime_mint.custom.yaml` 中指向自定义词库索引：

```yaml
patch:
  translator/dictionary: rime_mint.custom
```

---

### 6.7 添加自定义快捷键

**文件**：`rime_mint.custom.yaml`

```yaml
patch:
  "key_binder/bindings/@next":
    accept: "Control+Shift+E"
    toggle: emoji_suggestion
    when: always
```

---

### 6.8 修改字体

**macOS** — `squirrel.custom.yaml`：
```yaml
patch:
  "style/font_face": "LXGW WenKai"
  "style/font_point": 18
```

**Windows** — `weasel.custom.yaml`：
```yaml
patch:
  "style/font_face": "Microsoft YaHei"
  "style/font_point": 14
```

---

### 6.9 激活未启用的输入方案

**文件**：`default.custom.yaml`

```yaml
patch:
  schema_list:
    - schema: rime_mint
    - schema: double_pinyin_flypy
    - schema: rime_mint_flypy
    - schema: terra_pinyin
    - schema: wubi98_mint
    - schema: wubi86_jidian
    - schema: t9
    - schema: double_pinyin_abc     # 新增：智能ABC双拼
    - schema: double_pinyin_mspy    # 新增：微软双拼
```

> 注意：`schema_list` 是数组，覆写时需要列出**完整的**方案列表。

---

## 七、重要注意事项

### 7.1 page_size 和 key_binder 必须在方案级覆写

由于薄荷在每个方案的 `.schema.yaml` 中都冗余写入了 `menu` 和 `key_binder`，在 `default.custom.yaml` 中修改这些配置**不会生效**。必须在对应方案的 `.custom.yaml` 中覆写。

### 7.2 覆写数组/列表时是整体替换

当覆写 `schema_list`、`speller/algebra` 等数组类型配置时，patch 会**整体替换**原数组，而非追加。因此需要把完整内容写出来。

### 7.3 皮肤内的局部配置优先于全局配置

如果在皮肤配色方案中设置了 `candidate_list_layout: stacked`，那么即使在 `style` 全局设置中改为 `linear`，也可能被皮肤内的局部配置覆盖。

### 7.4 鼠须管和小狼毫的可配置项不完全相同

不要以为两个客户端能修改的配置一样。具体可修改的外观配置，建议查看 `squirrel.yaml` 和 `weasel.yaml` 源文件。

### 7.5 推荐使用 custom 文件而非直接修改源文件

- 直接修改 `.schema.yaml` 或 `.yaml` 源文件，在更新薄荷方案时可能产生冲突
- 使用 `.custom.yaml` 文件，更新时可以直接覆盖同名文件，不影响个人配置
- 这也是 Rime 官方推荐的配置方式

### 7.6 修改后需要重新部署

任何配置修改后，都需要**重新部署** Rime 输入法才能生效。在托盘图标右键菜单中选择「重新部署」。

### 7.7 词库文件的格式要求

- 自定义词库文件必须以 `.dict.yaml` 结尾
- 词条格式：`文字\t编码\t权重`（Tab 分隔）
- 建议使用 VS Code 等编辑器打开，注意 Tab 和空格的区分
- 万象词库包含音调信息，自定义词库可以不包含音调

### 7.8 配置版本号

`squirrel.yaml` 和 `weasel.yaml` 中的 `config_version` 需要比共享目录同名文件的版本号大才能生效。

---

## 八、快速参考：我要改什么，应该编辑哪个文件？

| 我想修改... | 应编辑的文件 | 平台 |
|-------------|-------------|------|
| 候选词个数（全拼） | `rime_mint.custom.yaml` | 所有 |
| 候选词个数（小鹤双拼） | `double_pinyin_flypy.custom.yaml` | 所有 |
| 候选词个数（全局） | ❌ 无效，必须在方案级覆写 | — |
| 皮肤/配色 | `squirrel.custom.yaml` | macOS |
| 皮肤/配色 | `weasel.custom.yaml` | Windows |
| 候选词横向排列 | `squirrel.custom.yaml` | macOS |
| 候选词横向排列 | `weasel.custom.yaml` | Windows |
| 字体和字号 | `squirrel.custom.yaml` / `weasel.custom.yaml` | 对应平台 |
| 模糊拼音 | `rime_mint.custom.yaml` | 所有 |
| 拼音串最大长度 | `rime_mint.custom.yaml` | 所有 |
| 快捷键绑定 | `rime_mint.custom.yaml`（方案级） | 所有 |
| 翻页键 | `rime_mint.custom.yaml`（方案级） | 所有 |
| 自定义词库 | `rime_mint.custom.yaml` + 新建词库文件 | 所有 |
| 激活/禁用输入方案 | `default.custom.yaml` | 所有 |
| 标点符号 | `default.custom.yaml` 或方案 `.custom.yaml` | 所有 |
| 中英切换键 | `default.custom.yaml` | 所有 |
| 简繁切换 | `rime_mint.custom.yaml` | 所有 |
| 外观圆角/阴影/间距 | `squirrel.custom.yaml` / `weasel.custom.yaml` | 对应平台 |

---

## 九、配置文件存放路径

| 平台 | 路径 |
|------|------|
| Windows (小狼毫) | `%APPDATA%\Rime` |
| macOS (鼠须管) | `~/Library/Rime` |
| macOS (Fcitx5) | `~/.local/share/fcitx5/rime` |
| Linux (iBus) | `~/.config/ibus/rime` |
| Linux (Fcitx5) | `~/.local/share/fcitx5/rime` |
| Android (Fcitx5) | `/storage/emulated/0/Android/data/org.fcitx.fcitx5.android/files/data/rime/` |

---

> 文档生成日期：2026-05-05
> 参考来源：oh-my-rime 项目源码 + [官方配置覆写文档](https://www.mintimate.cc/zh/guide/configurationOverride.html)
