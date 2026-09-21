# 箭了又箭（arrow-out-wechat）

Cocos Creator **3.8.8** 微信小游戏。点箭头沿朝向飞出；被挡住则弹回并扣心。3 颗心 + 限时，清空全部箭头过关。

## 立刻试玩（无需 Cocos）

打开本地文件即可：

```bash
open play/index.html
```

或用 Chrome 直接打开 `play/index.html`（`index.html` + `game.js`）。玩法与 Creator 版一致：逆向可解关卡、飞出/弹回、心与计时、中文 UI、拖尾/爆花动画。

## 玩法

- 棋盘上有若干长度 1–N 的方向箭头
- **点击**箭头 → 沿朝向飞出；路径畅通则飞出棋盘并消除
- 若前方有其他箭头挡住 → **弹回**并失去 1 心
- 心数为 0 或时间耗尽 → 失败；全部清空 → 过关进入下一关
- 第 1 关有中文操作提示
- 关卡由 **逆向剥离（reverse / peel）** 算法生成，可解且可用种子复现

## 环境要求

- macOS + [Cocos Creator 3.8.8](https://www.cocos.com/creator-download)
- 预览可用浏览器；正式包需 [微信开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

## 用 Creator 打开

1. 启动 **Cocos Creator 3.8.8**
2. **打开项目** → 选择本仓库根目录（含 `package.json`、`assets/`、`settings/` 的目录）
3. 等待导入完成（首次会生成 `library/`、`temp/`）
4. 打开场景 `assets/scenes/Main.scene`
5. 确认 `Canvas/GameRoot` 上挂有 `GameController` 组件（场景已预挂）

### 预览空白屏排查

若预览只有深色背景、没有菜单：

1. 选中 `Canvas/GameRoot`，看属性检查器是否显示 **Missing Script**
2. 若是：把 `assets/scripts/game/GameController.ts` **重新拖到** GameRoot 上，保存场景后再预览
3. 确认 GameRoot `_active` 为勾选，Canvas 下有 Camera
4. Label 未指定字体时部分环境中文可能不显示，但 Graphics 按钮底色仍应可见；若连按钮底都没有，几乎一定是脚本未挂上

根因说明：早期手写场景使用了伪 UUID；现已换成合法 UUID，但 Creator 首次导入仍可能重写 `.meta`，导致场景里组件类型 UUID 失配。失配时重挂脚本即可。

## 浏览器预览（Creator）

1. 菜单 **项目 → 项目设置**：设计分辨率建议 **720 × 1280**，适配宽度（仓库 `settings` 已配置）
2. 点击编辑器上方 **预览**（浏览器图标）
3. 在预览页点击 **开始游戏**，点箭头试玩飞出 / 弹回 / 过关

## 构建微信小游戏

1. 菜单 **项目 → 构建发布**
2. 发布平台选择 **微信小游戏**
3. 填写小游戏 AppID（测试可用测试号）
4. 点击 **构建**，输出目录一般为 `build/wechatgame`
5. 打开 **微信开发者工具** → 导入 `build/wechatgame`

> 本仓库不含 `build/` 产物（已 gitignore）。请在本机 Creator 中构建。

## 工程结构

```
play/index.html + play/game.js   # 免 Cocos 的 HTML5 试玩版（推荐）
assets/
  scenes/Main.scene
  scripts/
    core/Types.ts
    game/LevelGenerator.ts | Board.ts | ArrowView.ts | GameController.ts
    vfx/VFXHelper.ts
    ui/UIManager.ts
tools/test-level-generator.mjs
```

## 本地可解性自检

```bash
node tools/test-level-generator.mjs
```

期望输出 `ALL SOLVABLE`。

## 许可

仅供学习与个人项目使用。
