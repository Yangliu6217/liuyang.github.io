# Stitch 设计稿分析

> 基于 Stitch 项目 MoodCash 情绪生活志 的3个屏幕代码分析

---

## 设计语言总结

### 核心风格：**Glassmorphism + Mood-Responsive + Journal/Story**

Stitch 设计稿与当前 MoodCash 实现有**本质性的风格差异**：

| 维度 | 当前 MoodCash | Stitch 设计稿 |
|------|-------------|-------------|
| **风格** | 标准白色卡片 | 玻璃态(Glassmorphism) + 情绪光晕 |
| **背景** | 纯色 #F8F9FC | 奶油色 #FFFDF9 + 动态模糊光晕 |
| **卡片** | 白色实体 + 阴影 | 半透明毛玻璃 + 边框发光 |
| **字体** | 系统字体 | Literata(衬线) + Be Vietnam Pro(无衬线) |
| **情绪表达** | 小emoji + 细竖条 | 大光晕背景 + 星球隐喻 + 故事文字 |
| **导航** | 传统TabBar | 浮动胶囊导航栏 |
| **记账流程** | 表单式一步完成 | 3步沉浸式(情绪→故事→金额) |
| **统计页面** | 传统图表排行 | 星轨/星球隐喻 |

---

## 屏幕一：今日故事（首页）

### 设计要点

1. **标题用衬线字体**：`Literata` 28px/700，有文学感和日记感
2. **副标题是情绪摘要**："今日温和支出 ¥42.50 · 内心平静" — 不只是数字，有温度
3. **故事卡片**：每张卡片是一段"故事"，不只是账单
   - 左上角：情绪色圆形icon + emoji
   - 右上角：时间段标签（"黄金时刻"、"正午阳光"、"清晨时光"）
   - 正文：一句描述性文字（"雨后的午后，一杯温热的拿铁，看窗外雨滴赛跑"）
   - 底部：大号金额（Literata 40px）
4. **时间轴连接线**：卡片之间有一条居中的渐变竖线（opacity 30%）
5. **卡片旋转**：第2张卡片 rotate(1deg)，第3张 rotate(-1deg) — 手工摆放感
6. **卡片交互**：hover 时 scale(1.01) + translateY(-1px)
7. **背景**：顶部有一个巨大的 mood-halo（模糊渐变圆形，8秒呼吸动画）

### 与当前实现的差距

| 元素 | 当前 | Stitch |
|------|------|--------|
| 首页标题 | "MoodCash"（NavBar） | "今日故事"（衬线大字） |
| 副标题 | 无 | "今日温和支出 ¥42.50 · 内心平静" |
| 卡片内容 | 分类icon + 名称 + 金额 + 时间 | 时间段标签 + 故事文字 + 大金额 |
| 卡片风格 | 白色实体 + 阴影 | 毛玻璃 + 边框 + 旋转 |
| 时间轴 | 无 | 居中渐变竖线 |
| 背景 | 纯色 | 动态情绪光晕 |

---

## 屏幕二：记录瞬间（记账页）

### 设计要点

1. **3步沉浸式流程**（非表单）：
   - Step 1：选择情绪（轨道式布局，中心大emoji + 周围6个小emoji）
   - Step 2：写下故事（玻璃面板textarea）
   - Step 3：输入金额（数字键盘 + "上滑封存记忆"手势）

2. **情绪选择器**：
   - 中心：平静😌（最大，24×24，带呼吸动画和光晕）
   - 周围6个：兴奋🤩、幸福🥰、疲惫😴、难过🥺、生气😤、忧虑💭
   - 轨道式布局（绝对定位 + transform）
   - 每个emoji有 glass-panel 背景

3. **背景动态响应**：
   - 选择不同情绪 → mood-halo 颜色变化
   - calm: 蓝紫渐变 `#A1C4FD → #C2E9FB`
   - joy: 粉橙渐变 `#FF9A9E → #FAD0C4`
   - worry: 紫粉渐变 `#A18CD1 → #FBC2EB`

4. **步骤切换动画**：
   - 向上：`translateY(-100vh)` + opacity 0
   - 向下：`translateY(100vh)` + opacity 0
   - 时长 0.6s，cubic-bezier(0.4, 0, 0.2, 1)

5. **数字键盘**：
   - 3列网格，glass-panel 按钮
   - 金额显示用 Literata 40px
   - 底部"上滑封存记忆"手势提示

### 与当前实现的差距

| 元素 | 当前 | Stitch |
|------|------|--------|
| 流程 | 一步表单（金额→分类→心情→备注） | 3步沉浸（情绪→故事→金额） |
| 情绪选择 | 5个emoji横排 | 轨道式布局，中心+6卫星 |
| 情绪反馈 | 无 | 背景光晕随情绪变色 |
| 文字输入 | 备注框（可选） | 故事textarea（核心步骤） |
| 金额输入 | 底部NumberPad | 独立步骤 + 上滑手势 |
| 视觉风格 | 白色卡片 | 全屏毛玻璃 + 动态背景 |

---

## 屏幕三：情绪星轨（统计页）

### 设计要点

1. **星轨隐喻**：统计不是图表，是"星球"
   - 每个消费分类是一颗"星球"（glass-panel 圆形）
   - 星球之间有 SVG 星座连线（虚线，渐入动画）
   - 点击星球展开"记忆云"（glass-panel 弹出卡片）

2. **星球设计**：
   - 大星球：频繁消费的分类（如餐饮，w-32 h-32）
   - 小星球：低频消费的分类
   - 每个星球有情绪色渐变背景（blur 效果）
   - 内部：Material Icon + 分类名

3. **记忆云**：
   - 点击星球后从下方弹出
   - 显示最近3笔该分类的消费
   - 每笔：emoji + 描述 + 金额 + 心情标签

4. **背景**：3个不同颜色的 mood-halo 浮动（20秒动画循环）

5. **顶部**：glass-panel 说明卡片 "你的情绪星轨 · 触摸星球，唤起每一笔消费背后的故事"

### 与当前实现的差距

| 元素 | 当前 | Stitch |
|------|------|--------|
| 统计呈现 | 传统排行榜 + 进度条 | 星球隐喻 + 星座连线 |
| 分类展示 | 列表行 | 浮动星球节点 |
| 详情展示 | 无（点击跳转） | 展开记忆云 |
| 背景 | 纯色 | 3个浮动光晕 |
| 情绪关联 | 分析卡片（文字） | 星球颜色反映情绪 |

---

## 共性设计模式

### 1. Glassmorphism（毛玻璃）

```css
.glass-panel {
    background: rgba(255, 255, 255, 0.4-0.6);
    backdrop-filter: blur(12-20px);
    border: 1px solid rgba(255, 255, 255, 0.4);
    box-shadow: 0px 10px 30px rgba(0,0,0,0.04);
}
```

### 2. Mood Halo（情绪光晕）

```css
.mood-halo {
    filter: blur(60-100px);
    opacity: 0.5-0.6;
    animation: breathe 8s ease-in-out infinite alternate;
}
/* 每种情绪对应不同渐变色 */
```

### 3. 字体系统

- **Literata**（衬线）：标题、金额、日记感
- **Be Vietnam Pro**（无衬线）：正文、标签、现代感

### 4. 导航栏

```css
/* 浮动胶囊导航 */
nav {
    position: fixed;
    bottom: 40px;
    left: 50%;
    transform: translateX(-50%);
    width: 320px;
    background: rgba(255,255,255,0.2);
    backdrop-filter: blur(20px);
    border-radius: 9999px; /* 完全圆形 */
    border: 1px solid rgba(255,255,255,0.4);
}
/* 中间FAB更大 + 脉冲动画 */
```

### 5. 色彩体系

| Token | Hex | 用途 |
|-------|-----|------|
| primary | #4d41df | 品牌紫（比当前更深沉） |
| primary-container | #675df9 | 按钮/容器 |
| surface-cream | #FFFDF9 | 页面背景（暖奶油色） |
| text-ink | #2D2D3A | 正文文字 |
| mood-calm | #A1C4FD → #C2E9FB | 平静情绪光晕 |
| mood-joy | #FF9A9E → #FAD0C4 | 快乐情绪光晕 |
| mood-sad | #A18CD1 → #FBC2EB | 悲伤情绪光晕 |

---

## 实施建议（从易到难）

### 可以快速实现的

1. **页面背景色**：`#F8F9FC` → `#FFFDF9`（暖奶油色）
2. **卡片毛玻璃**：白色实体 → 半透明 + backdrop-filter
3. **首页副标题**：增加"今日温和支出 ¥XX · 内心XX"
4. **导航栏样式**：改为浮动胶囊 + 中间FAB突出
5. **时间轴连接线**：卡片之间加居中渐变竖线

### 需要较大改动的

6. **情绪光晕背景**：页面顶部加动态模糊渐变
7. **记账流程重构**：从表单式改为3步沉浸式
8. **字体引入**：加载 Literata + Be Vietnam Pro
9. **统计页星球化**：从排行榜改为浮动节点

### 需要重新设计的

10. **故事化账单**：每笔账单从"分类+金额"变为"时间段+故事+金额"
11. **情绪轨道选择器**：从横排改为轨道式布局
12. **星轨统计**：从图表改为星球隐喻

---

---

## 屏幕四：个人中心（Profile）

### 设计要点

1. **Hero卡片**：毛玻璃面板，居中布局
   - 大头像（96px）+ 发光背景（blur-xl, primary/20）
   - 用户名用衬线字体（Literata 34px）
   - 副标题："记录生活里的每一个静谧时刻。"
   - 双数据展示：142 记忆瞬间 | 89 快乐时光（Literata 40px 数字）

2. **预算进度卡片**：
   - 毛玻璃背景
   - 标题"预算进度"+ 右侧"编辑预算"链接
   - 大号已用金额（Literata 40px）+ 总额
   - 进度条：h-3, rounded-full, 内部有白色模糊光晕

3. **菜单列表**：
   - 每个菜单项是独立的毛玻璃卡片
   - **关键细节**：每个卡片有微小旋转（0.2deg ~ 0.8deg），左右交替
   - hover 时 scale(1.01) + 300ms 过渡
   - 左侧：圆形容器 + Material Icon
   - 右侧：chevron_right 箭头
   - **交互彩蛋**：鼠标移动时，卡片背景跟随鼠标位置产生径向渐变高光

4. **菜单项**：
   - 个人资料 → 编辑信息与头像
   - 消费日历 → 基于情绪的日历视图
   - 主题与氛围 → 切换明/暗/自定义风格
   - 数据与隐私 → 导出/清理数据
   - 关于 MoodCash → 版本 2.4.1

### 与当前实现的差距

| 元素 | 当前 | Stitch |
|------|------|--------|
| Hero卡片 | 紫色渐变实体 | 毛玻璃 + 发光头像 |
| 数据展示 | 普通数字 | Literata大字 + 情绪标签 |
| 菜单项 | 白色卡片 + 阴影 | 毛玻璃 + 微旋转 + 鼠标跟随光效 |
| 预算展示 | 独立页面 | 内嵌卡片 + 大字金额 |
| 副标题 | "记录生活，感知情绪" | "记录生活里的每一个静谧时刻。" |

---

## 屏幕五：生活档案（Monthly Archive）

### 设计要点

1. **书架隐喻**：每个月是一本"书"
   - 3列网格布局（响应式）
   - 每本书是 3:4 比例的卡片
   - 书脊效果：左侧 2px 黑色半透明条 + 内阴影

2. **书籍卡片**：
   - 背景：情绪色渐变（mood-calm/mood-joy/mood-sad）+ 纹理叠加
   - 底部信息区：白色毛玻璃小面板
     - 月份标题（Literata 22px）
     - 描述文字（"一个沉静反思、稳步成长的季节"）
     - 左侧：记忆碎片数量（金色渐变数字，gilded-stamp 效果）
     - 右侧：主导情绪 icon

3. **悬停动画**：
   - `translateY(-10px) scale(1.02)` — 书本浮起
   - `box-shadow: 0px 20px 40px rgba(0,0,0,0.08)` — 阴影加深
   - 时长 0.4s，弹性 cubic-bezier(0.175, 0.885, 0.32, 1.275)

4. **金色印章效果**（gilded-stamp）：
   ```css
   background: linear-gradient(135deg, #CFB53B 0%, #E8D37B 50%, #CFB53B 100%);
   -webkit-background-clip: text;
   -webkit-text-fill-color: transparent;
   ```

5. **背景**：两个不同颜色的 mood-halo 浮动 + 径向渐变纹理

### 与当前实现的差距

| 元素 | 当前 | Stitch |
|------|------|--------|
| 月报展示 | 单张深色卡片 | 书架网格（多个月份） |
| 视觉隐喻 | 数据报告 | 书籍/档案 |
| 卡片交互 | 无 | 悬停浮起 + 弹性动画 |
| 数字样式 | 普通白色 | 金色印章渐变 |
| 情绪表达 | 文字标签 | 情绪色背景 + icon |
| 背景纹理 | 无 | 透明纹理叠加 |

---

## 完整设计系统提取

### 字体

```css
/* 衬线 — 标题、金额、日记感 */
font-family: 'Literata', serif;
/* 用于：页面标题(34px/700)、金额显示(40px/700)、日记标题(22px/600) */

/* 无衬线 — 正文、标签 */
font-family: 'Be Vietnam Pro', sans-serif;
/* 用于：正文(16px/400)、副标题(14px/400)、标签(12px/600) */
```

### 色彩（完整）

| Token | Hex | 用途 |
|-------|-----|------|
| primary | #4d41df | 品牌主色 |
| primary-container | #675df9 | 按钮/活跃态容器 |
| surface-cream | #FFFDF9 | 页面背景 |
| text-ink | #2D2D3A | 正文文字 |
| on-surface-variant | #464555 | 次要文字 |
| outline-variant | #c7c4d8 | 边框/分割线 |
| glass-border | rgba(255,255,255,0.4) | 毛玻璃边框 |
| mood-calm | #A1C4FD → #C2E9FB | 平静 |
| mood-joy | #FF9A9E → #FAD0C4 | 快乐 |
| mood-sad | #A18CD1 → #FBC2EB | 悲伤 |
| mood-neutral | #E2E2E2 → #F5F5F5 | 中性 |
| tertiary-container | #b65c00 | 暖色强调 |
| secondary | #2e6385 | 冷色辅助 |
| gilded | #CFB53B → #E8D37B | 金色印章 |

### 毛玻璃面板（统一）

```css
.glass-panel {
    background: rgba(255, 255, 255, 0.3-0.6);
    backdrop-filter: blur(12-20px);
    -webkit-backdrop-filter: blur(12-20px);
    border: 1px solid rgba(255, 255, 255, 0.4);
    box-shadow: 0px 10px 30px rgba(0,0,0,0.04);
}
```

### 情绪光晕（统一）

```css
.mood-halo {
    position: fixed;
    border-radius: 50%;
    filter: blur(60-100px);
    opacity: 0.3-0.6;
    z-index: -1;
    pointer-events: none;
}
/* 呼吸动画 */
@keyframes breathe {
    0% { transform: scale(1); opacity: 0.5; }
    100% { transform: scale(1.1); opacity: 0.7; }
}
```

### 浮动胶囊导航栏

```css
nav {
    position: fixed;
    bottom: 32-40px;
    left: 50%;
    transform: translateX(-50%);
    width: 280-320px;
    background: rgba(255,255,255,0.2);
    backdrop-filter: blur(20px);
    border: 1px solid rgba(255,255,255,0.4);
    border-radius: 9999px;
    padding: 8px;
}
/* 中间FAB：w-14 h-14, bg-primary, shadow紫色光晕, animate-pulse */
/* 当前Tab：w-12 h-12, bg-primary-container, scale-110 */
/* 其他Tab：w-10 h-10, 无背景, hover时显示 */
```

### 交互模式

| 交互 | 实现 |
|------|------|
| 卡片悬停 | scale(1.01) + translateY(-1px~10px) |
| 按钮按下 | active:scale(95%) |
| 鼠标跟随光效 | radial-gradient 跟随 mousemove |
| 涟漪点击 | liquid-ripple ::after 扩散 |
| 菜单微旋转 | rotate(0.2~0.8deg)，左右交替 |
| 浮动动画 | translateY 4s ease-in-out infinite |

---

## 完整实施路线图

### Phase 1：视觉基础（低风险，高回报）

- [ ] 页面背景色 `#F8F9FC` → `#FFFDF9`
- [ ] 卡片改为毛玻璃样式（rgba白色 + backdrop-filter）
- [ ] 引入 Literata + Be Vietnam Pro 字体
- [ ] 统一 section 标题颜色为品牌紫

### Phase 2：导航与布局（中等风险）

- [ ] TabBar 改为浮动胶囊样式
- [ ] 中间 FAB 放大 + 紫色光晕 + pulse 动画
- [ ] 首页增加情绪光晕背景
- [ ] 首页标题改为"今日故事" + 副标题情绪摘要

### Phase 3：组件升级（中等风险）

- [ ] TimelineItem 改为故事卡片风格（时间段标签 + 描述文字）
- [ ] 卡片之间增加居中渐变时间轴连接线
- [ ] Profile 菜单项增加微旋转 + 鼠标跟随光效
- [ ] Profile Hero 增加发光头像效果

### Phase 4：流程重构（高风险，高价值）

- [ ] 记账流程改为3步沉浸式（情绪→故事→金额）
- [ ] 情绪选择器改为轨道式布局
- [ ] 背景光晕随情绪选择动态变色

### Phase 5：高级视觉（高风险）

- [ ] 统计页改为星球/星轨隐喻
- [ ] 月报改为书架/档案隐喻
- [ ] 金色印章数字效果
- [ ] 书本悬停浮起动画
