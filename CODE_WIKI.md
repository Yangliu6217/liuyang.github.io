# NeuralVerse 代码文档

## 项目概述

**项目名称**: NeuralVerse - 人工智能前沿
**项目类型**: 单页Web应用 (Single Page Application)
**部署平台**: GitHub Pages
**源码仓库**: liuyanggithub.io

### 核心功能
- AI知识展示（机器学习、自然语言处理、计算机视觉）
- AI应用场景介绍（医疗、自动驾驶、创意生成等）
- DeepSeek API 驱动的AI对话功能
- Canvas实现的AI图像生成演示
- 3D粒子动态背景

---

## 技术栈

| 类别 | 技术 |
|------|------|
| 前端框架 | 原生 HTML5 + CSS3 + JavaScript (ES6) |
| 3D渲染 | Three.js r128 |
| 字体 | Google Fonts (Orbitron, Noto Sans SC) |
| AI后端 | DeepSeek API |
| 部署 | GitHub Pages |

---

## 项目结构

```
/workspace/
├── .github/
│   └── workflows/
│       └── static.yml          # GitHub Actions 部署配置
├── AI网站.html                  # 主页面（全部代码）
├── README.md                    # 项目说明
└── CODE_WIKI.md                # 本文档
```

---

## 主要模块

### 1. 加载动画 (Loader)
**CSS选择器**: `.loader`, `.loader-text`, `.loader-bar`, `.loader-progress`

**功能**: 页面初始加载时的品牌展示动画
- 渐变色文字动画 (gradientShift)
- 进度条填充动画 (loadProgress)
- 2.2秒后自动隐藏

### 2. 自定义光标系统
**CSS选择器**: `.cursor`, `.cursor-follower`, `.mouse-glow`

**JavaScript函数**:
- `animateCursor()` - 光标跟随动画主循环
- 光标悬停时添加 `.hover` 类放大
- 鼠标按下时缩放效果

### 3. Three.js 3D背景
**初始化函数**: `initThreeJS()`

**渲染管线**:
```
Scene → PerspectiveCamera → WebGLRenderer
           ↓
    BufferGeometry (6000粒子)
           ↓
    PointsMaterial (加法混合)
           ↓
    LineSegments (150条连接线)
```

**动画函数**: `animate()`
- 粒子群旋转 (X: 0.0004, Y: 0.0006 弧度/帧)
- 鼠标交互影响旋转方向

### 4. 导航栏
**CSS选择器**: `nav`, `.logo`, `.nav-links`

**JavaScript函数**: `handleScroll()`
- 滚动超过100px添加 `.scrolled` 类
- 导航栏内边距缩小、背景加深

### 5. 卡片系统
**CSS选择器**: `.card`, `.card-grid`, `.card-icon`, `.card-glow`

**交互**:
- 悬停: `translateY(-20px) rotateX(5deg)` 3D变换
- 点击: 调用 `openModal(type)` 打开详情

### 6. 模态框系统
**JavaScript数据结构**: `modalData` 对象

**函数**:
| 函数名 | 参数 | 功能 |
|--------|------|------|
| `openModal(type)` | type: string | 根据类型加载数据并显示模态框 |
| `closeModal(event)` | event: Event | 关闭模态框 |
| `ESC` 键 | - | 触发关闭 |

**支持的模态框类型**: `ml`, `nlp`, `cv`, `medical`, `auto`, `creative`, `manufacture`, `education`, `security`

### 7. 统计数据区
**CSS选择器**: `.stat-item`, `.stat-number`

**JavaScript函数**: `animateNumbers()`
- 目标数值从 `data-target` 属性读取
- 缓出动画 (easeOut Quart)
- 持续时间: 2500ms

### 8. API设置模块
**JavaScript变量**:
- `apiKey` - localStorage 存储的API密钥
- `chatHistory` - 对话历史记录
- `API_URL` - `https://api.deepseek.com/chat/completions`

**函数**:
| 函数名 | 功能 |
|--------|------|
| `initApiSettings()` | 初始化API设置状态 |
| `saveApiKey()` | 保存API Key到localStorage |
| `testApiConnection()` | 验证API连接 |
| `showApiStatus(type, msg)` | 显示状态消息 |

### 9. AI对话功能
**核心函数**: `sendMessage()`

**API调用参数**:
```javascript
{
  model: 'deepseek-chat',
  messages: [
    { role: 'system', content: '你是 NeuralVerse AI 助手...' },
    ...chatHistory.slice(-10)  // 保留最近10条对话
  ],
  temperature: 0.7,
  max_tokens: 1000,
  stream: false
}
```

**辅助函数**:
| 函数名 | 功能 |
|--------|------|
| `addMessageToChat(role, content)` | 添加消息到聊天容器 |
| `addTypingIndicator()` | 显示打字指示器 |
| `removeTypingIndicator(id)` | 移除打字指示器 |
| `escapeHtml(text)` | HTML转义防止XSS |
| `handleKeyPress(event)` | Enter键发送消息 |

### 10. AI图像生成
**函数**: `generateImage()`

**实现方式**: Canvas 2D API (非真实AI)

**生成逻辑**:
1. 深色渐变背景
2. 250个随机位置、大小、颜色的粒子圆
3. 12个随机几何图形（圆形或多边形）
4. 8条渐变色连接线
5. 底部显示提示词文本

### 11. 滚动相关
**函数**:
- `scrollToSection(id)` - 平滑滚动到指定区块
- `scrollToTop()` - 返回顶部
- 滚动进度条自动更新

---

## 关键类与函数清单

### 全局变量
| 变量名 | 类型 | 说明 |
|--------|------|------|
| `cursor` | HTMLElement | 主光标DOM |
| `cursorFollower` | HTMLElement | 光标跟随圈DOM |
| `mouseGlow` | HTMLElement | 鼠标光效DOM |
| `mouseX`, `mouseY` | number | 当前鼠标位置 |
| `followerX`, `followerY` | number | 跟随圈位置 |
| `apiKey` | string | DeepSeek API密钥 |
| `chatHistory` | Array | 对话历史 |
| `scene`, `camera`, `renderer` | THREE.js对象 | 3D场景组件 |
| `particles` | THREE.Points | 粒子系统 |

### CSS变量
```css
:root {
  --primary: #00f5ff;      /* 主色调-青色 */
  --secondary: #8b5cf6;    /* 次色调-紫色 */
  --accent: #f72585;       /* 强调色-粉红 */
  --bg-dark: #0a0a0f;      /* 深色背景 */
  --bg-card: rgba(15, 15, 25, 0.8);  /* 卡片背景 */
}
```

---

## 依赖关系

```
AI网站.html
├── 外部CDN
│   ├── three.js r128
│   │   └── THREE.Scene, THREE.PerspectiveCamera, THREE.WebGLRenderer
│   │   └── THREE.BufferGeometry, THREE.Points, THREE.LineSegments
│   ├── Google Fonts
│   │   └── Orbitron (标题字体)
│   │   └── Noto Sans SC (正文字体)
│   └── DeepSeek API (外部服务)
│       └── chat/completions 端点
├── 内部模块
│   ├── CSS 动画系统
│   │   └── gradientShift, loadProgress, scrollWheel
│   │   └── rotate, modalFadeIn, rippleEffect
│   │   └── float, fadeInUp, textReveal
│   ├── JavaScript 模块
│   │   ├── 光标系统 (cursor, cursorFollower)
│   │   ├── 3D渲染系统 (scene, camera, renderer, particles)
│   │   ├── 模态框系统 (modalData, openModal, closeModal)
│   │   ├── API通信 (sendMessage, testApiConnection)
│   │   └── Canvas绘图 (generateImage)
│   └── localStorage
│       └── deepseek_api_key 存储
```

---

## 响应式设计

**断点**: 768px

**适配内容**:
- Hero标题: 4.5rem → 2.5rem
- 统计网格: 4列 → 2列
- 导航栏: 显示完整链接 → 隐藏链接
- 内边距调整

---

## 运行方式

### 本地运行
1. 直接在浏览器中打开 `AI网站.html`
2. 或使用本地服务器:
```bash
python -m http.server 8000
# 访问 http://localhost:8000/AI网站.html
```

### 部署
1. 推送到GitHub仓库
2. 启用GitHub Pages
3. 选择 `gh-pages` 分支或 `main` 分支

---

## 外部资源

| 资源 | URL |
|------|-----|
| Three.js | https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js |
| Orbitron字体 | https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900 |
| Noto Sans SC | https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@300;400;700 |
| DeepSeek API | https://platform.deepseek.com/ |

---

## 注意事项

1. **API Key安全**: API Key存储在localStorage中，仅适用于个人使用
2. **图像生成**: `generateImage()` 为Canvas模拟实现，非真实AI图像生成
3. **浏览器兼容**: 需要支持ES6+和WebGL的现代浏览器
4. **性能**: 3D背景和大量动画可能影响低端设备性能
